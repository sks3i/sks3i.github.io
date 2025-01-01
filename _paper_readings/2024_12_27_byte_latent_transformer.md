---
layout: post
title: Byte Latent Transformer
date: 2024-12-27
thumbnail: /paper_readings/blt-figure.png
description: Byte Latent Transformer processes bytes directly and achieve similar performance as tokenization models.
---

# Byte Latent Transformer


<img src="{{ site.baseurl }}/assets/paper_readings/blt_svg.svg" alt="Byte Latent Transformer Architecture" class="zoomable" />

<script>
  mediumZoom('.zoomable', {
    margin: 24,
    background: '#000000d1'
  });
</script>

Byte Latent Transformer (BLT) is a tokenizer free model which matches the performance of 
tokenized model for rhe 1st time. The input bytes are fed to a byte encoder, which learns to patch the bytes and sends to latent transformer. Latent transformer outputs 
patches which is decoded to bytes by byte decoder.


