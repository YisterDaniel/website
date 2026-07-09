---
title: "Setting Up a Local AI Workspace with Ollama and Docker"
date: 2026-07-08
tags:
- ai
- docker
- ollama
- local-llm
- development
---

## What I Worked On

I started experimenting with running local Large Language Models (LLMs) by setting up a private AI workspace using Docker, Ollama, and the Odysseus AI Workspace platform.

The main goal was to explore how local AI could fit into my developer workflow. I wanted to have a private AI assistant that I could use for brainstorming ideas, debugging issues, and experimenting with different models without relying completely on cloud-based AI services.

Before this project, I had very little experience with Docker and no experience running local AI models. This was my first time setting up an environment where I could manage and test multiple AI models locally.

My setup:

- GPU: NVIDIA RTX 3060 Ti (8GB VRAM)
- RAM: 32GB
- CPU: Intel Core i7-10700KF
- Environment: Windows + Docker Desktop

## Setup Process

The setup involved running Odysseus AI Workspace through Docker and connecting it with Ollama to manage local models.

One of the reasons I chose this approach was the ability to experiment with multiple models through a single interface instead of manually managing each model separately.

The workspace allowed me to:

- Run AI models locally
- Switch between different models
- Test different model strengths and weaknesses
- Interact with models through a chat interface

The setup process was surprisingly straightforward. The biggest learning curve was not the installation itself, but understanding how local AI models differ from cloud-based AI services.

## Models Tested

For my first tests, I focused on models that could potentially help with programming tasks, debugging, and general development questions.

The models I tested were:

- Llama 3.2
- Devstral 24B
- Qwen 3.5 9B

### Llama 3.2

Llama was the fastest model I tested, but I found that the quality of responses was inconsistent.

For general questions, it performed reasonably well, but when asking programming-related questions, it sometimes struggled to fully understand the context or intent behind my questions.

### Devstral 24B

Devstral was interesting because it is a larger model designed with coding tasks in mind.

However, with my current hardware, the model was difficult to evaluate properly. Since it was much larger, response generation was significantly slower, making it less practical for everyday use on my current setup.

This highlighted one of the biggest challenges with local AI: larger models generally require significantly more hardware resources.

### Qwen 3.5 9B

Qwen is the model I plan to continue testing further.

So far, it appears to provide a better balance between response quality and performance compared to the other models I tested. Being a smaller model than Devstral allows it to generate responses faster while still providing useful answers.

The Odysseus interface also displays the model's thinking process, which was interesting to see. However, it also made the response time feel longer since I was waiting for the model to complete its reasoning before receiving the final answer.

I still need more testing, but Qwen currently seems like the best candidate for integrating into my daily development workflow.

## Comparing Local AI vs Cloud AI

One of the biggest lessons from this project was understanding that running AI locally is very different from using services like ChatGPT.

Cloud-based AI services have access to much larger models and significantly more computing resources. As a result, the quality and speed of responses are currently better compared to the smaller models I can run on my own hardware.

However, local AI provides other advantages:

- Privacy since data can remain on my machine
- Ability to experiment with different models
- More control over the AI environment
- No dependency on external services

The tradeoff is that local AI requires managing hardware limitations and selecting models that fit the available resources.

## What I Learned

- Local AI models are not equivalent to cloud AI services. Model size and available computing resources have a major impact on performance.
- Running AI locally is largely about balancing model capability with hardware limitations.
- Different models have different strengths depending on the task.
- Docker makes it easier to package and manage complex development environments.
- Experimenting with AI tools is an important part of understanding how they can improve my own workflow.

## Next Steps

My next step is to continue testing local models, especially Qwen, and see how well it performs when integrated into my actual development process.

I want to use it more actively for:

- Brainstorming project ideas
- Debugging programming issues
- Understanding unfamiliar concepts
- Supporting my personal projects

While cloud-based AI is still the more powerful option today, learning how local AI works gives me a better understanding of the technology and how it can be incorporated into future development workflows.