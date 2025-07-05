---
layout: post
title: Designing My Home Automation System: Part 1 - Choosing the initial setup
---

# Designing My Home Automation System: Part 1 - Choosing the initial setup

Anyone who's ever been a fan of Iron Man has probably dreamed of having a smart, AI-powered home that just knows everything — and today, that's more possible than ever. That dream stuck with me for years. Back in undergrad — nearly 15 years ago — I even built a basic system that let me control fans and lights using a TV remote, and later, an Android app. It was simple, but I loved doing it.

Of course, back then there was no Alexa, no ChatGPT, and certainly no smooth voice interfaces or local AI agents. The idea of having a natural, conversational interface with your home was still science fiction. But the seed was planted.

In recent years, I've been using Google Home for basic automation, but there’s always been some friction — things never felt seamless. Recently, a friend introduced me to the [Home Assistant](https://www.home-assistant.io/) project. Now that I own a home, the spark has returned, and I’m finally setting out to build the smart system I always imagined. I’m not sure how much of the "physical-to-digital" friction I’ll be able to eliminate, but I’m excited to find out.


## Choosing a Platform for Home Assistant

[Home Assistant](https://www.home-assistant.io/) is an excellent starting point for building a smart home system. It offers a robust automation platform along with a well-designed dashboard interface — perfect for creating a centralized control panel.

The first step is installing **Home Assistant OS (HAOS)**. It can run on several platforms:
- Dedicated hardware like [Home Assistant Green](https://www.home-assistant.io/green)
- A **Raspberry Pi 4 or 5**
- A **Windows or Linux machine** via a virtual machine

### My Available Hardware

I already had a few devices lying around:
- A **laptop with an RTX 3080** (ideal for GPU-heavy AI tasks)
- An **iPad Air 4** (great for display)
- A **Raspberry Pi 2 and 3**

### Two Architecture Options

Based on this, I considered two possible setups:

#### Option 1: Raspberry Pi + Display
- Run **HAOS on a Raspberry Pi** (ideally Pi 5 for performance and compatibility)
- Use a 10-inch display as a wall-mounted dashboard
- Offload heavy AI workloads to the **laptop via local network**

#### Option 2: Laptop + iPad
- Run **HAOS in a virtual machine on my laptop**
- Use the **iPad Air** as a display interface (via [Fully Kiosk](https://www.fully-kiosk.com/) or Safari in kiosk mode)


Option 1 would require me to buy some additional hardware before I can get started. Option 2, on the other hand, lets me start right away — and I can refine the setup later as I learn more and better understand my needs.

