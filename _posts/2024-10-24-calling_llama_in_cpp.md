---
layout: post
title: Running Llama Inference in C++
subtitle: 
tags: [cpp, ml, llm, llama]
---

C++ is one of the go to languages where speed, efficiency and reliability are important. Calling Llama models directly in C++ can help in reducing additional overheads.

### Downloading model weights and basic setup

To call Llama, you need access to the model weights and it can be requested in HuggingFace (this is preferred for now.) Once the access has been granted, you can download it via huggingface-cli. For example, to download the 1B model,

```
huggingface-cli download meta-llama/Llama-3.2-1B
```

It will print the download path in the end. Probably, cli tool also provides a way to specify download path.


Next step is to build `llama.cpp` repo. We need to build shared libs so we can integrate it with other C++ applications. 
```
> git clone https://github.com/ggerganov/llama.cpp.git
> cd llama.cpp
> mkdir build && cd build
> cmake ..
> make
```

`llama.cpp` expects the model in gguf format. To convert the llama model you just downloaded to gguf format, you can run `convert_hf_to_gguf.py`. It only works for models downloaded via hugging face, as the name suggests. Otherwise, you need to first convert the model into hugging face format.
```
python3 convert_hf_to_gguf.py /path/to/model --outfile llama_1b.gguf
```

You can test it using `llama-cli` (this is found in llama.cpp dir)

```
./llama-cli -m /path/to/llama_1b.gguf -n 100 -p "explain transformers in machine learninig"
```

In the cli tool, you can use `-m` to specify the model path, `-n` to specify number of output tokens and `-p` to specify the prompt.


### Integrating llama.cpp with C++ programs

Simplest way is to add `llama.cpp` as a git submodule to your C++ application. You can create a new CMakeLists.txt as follows

```Makefile
cmake_minimum_required(VERSION 3.12)
project(MyProject)

add_subdirectory(llama.cpp)

# Add main.cpp as an executable and link it with the llama library
add_executable(main main.cpp)
target_link_libraries(main PRIVATE llama)
```

Here are some simplistic overview of some of the important functions (I'll go over in more deeply in another post.)

* To load a `.gguf` model, use `llama_load_model_from_file`. 
* To tokenize a text, use `llama_tokenize` function. It's args are (llama_model* model, const char* text, int text_len, llama_tokens* tokens, int max_tokens, bool add_spl_token, bool parse_spl). You need to initialize an array of `llama_tokens` and pass it to this function.
* You can update the model's context using `llama_new_context_with_model`.
* While decoding, you need a sampler to sample the token from the logits. You can use `llama_sampler_chain_init` to initialize the sampler.
* To decode, you need the model context and `llama_batch`. It basically holds the batch of the token sequence. 

Some basic code to tokenize the prompt and generate a text is as follows.

```cpp
/*
Sample C++ program to call Llama 1B model.
*/

#include <iostream>
#include <string>
#include <vector>
#include <cassert>
#include "llama.h"

const std::string MODEL_PATH = "/home/saba_rish91/llama_converted.gguf";
const int MAX_OUTPUT_TOKENS = 100;

llama_model* initialize_model() 
{
    llama_model_params params = llama_model_default_params();
    return llama_load_model_from_file(MODEL_PATH.c_str(), params);
    
}

void tokenize(std::string str_to_tokenize, llama_model* model, std::vector<llama_token>& tokens) 
{
    const int n_tokens = -llama_tokenize(model, str_to_tokenize.c_str(), str_to_tokenize.size(), NULL, 0, true, true);
    std::cout << "Number of tokens: " << n_tokens << std::endl;
    tokens.resize(n_tokens);
    llama_tokenize(model, str_to_tokenize.c_str(), str_to_tokenize.size(), tokens.data(), n_tokens, true, true);
}

llama_context_params setup_llama_ctx()
{
    llama_context_params ctx = llama_context_default_params();
    ctx.n_ctx = MAX_OUTPUT_TOKENS;  // Number of output tokens

    return ctx;
}

llama_sampler* get_sampler()
{
    llama_sampler_chain_params params = llama_sampler_chain_default_params();
    llama_sampler *smpl = llama_sampler_chain_init(params);
    llama_sampler_chain_add(smpl, llama_sampler_init_greedy());
    return smpl;
}


int main(int argc, char* argv[]) 
{
    assert((void*)"Provide prompt as input" && argc > 1);
    std::string prompt = argv[1];

	llama_model *model = initialize_model();

    if (model == NULL) 
    {
        fprintf(stderr , "%s: error: unable to load model\n" , __func__);
        return 1;
    }

    std::vector<llama_token> tokens;
    tokenize(prompt, model, tokens);
    assert((void*)"No tokens found" && tokens.size() > 0);

    llama_context_params ctx_params = setup_llama_ctx();
    llama_context *ctx = llama_new_context_with_model(model, ctx_params);

    if (ctx == NULL) {
        fprintf(stderr , "%s: error: failed to create the llama_context\n" , __func__);
        return 1;
    }

    llama_sampler *smpl = get_sampler();
    llama_batch batch = llama_batch_get_one(tokens.data(), tokens.size());
    std::string generated_text = prompt + ":: ";
    
    llama_token new_token_id;
    for(int i = 0; i < MAX_OUTPUT_TOKENS; ++i)
    {
        llama_decode(ctx, batch);
        new_token_id = llama_sampler_sample(smpl, ctx, -1);  //Sample from logits of last output.
        if(llama_token_is_eog(model, new_token_id))
        {
            break;
        }
        char buffer[128];
        int n = llama_token_to_piece(model, new_token_id, buffer, sizeof(buffer), 0, true);
        if (n < 0)
        {
            std::cerr << "Error: Failed to convert token to piece" << std::endl;
            return 1;
        }
        std::string new_token(buffer, n);
        generated_text += new_token + " ";
        batch = llama_batch_get_one(&new_token_id, 1);
    }
    std::cout << "Generated text: " << generated_text << std::endl;
    return 0;
}
```