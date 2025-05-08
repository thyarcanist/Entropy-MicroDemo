# Entropy-MicroDemo

Exploring the Impact of True Entropy vs. PRNG Seeding on LLM Behavior

## Summary

This project investigates and provides tangible evidence that the quality of random number generation significantly impacts Large Language Model (LLM) behavior. Through controlled experiments, we demonstrate that seeding text generation with high-quality true entropy from the ERIS API versus a standard fixed Pseudo-Random Number Generator (PRNG) seed leads to demonstrably distinct outcomes across various LLM architectures.

## Problem Investigated

Large Language Models, even as they become more accurate on benchmarks, can exhibit undesirable behaviors like hallucination or getting stuck in repetitive loops.

## Hypothesis

The core hypothesis explored is: What if issues like hallucination are not just noise, but influenced by "false pattern reinforcement" stemming from the lower-entropy, deterministic nature of standard PRNG seeds? Conversely, can seeding with high-quality true entropy (like ERIS) lead to different, potentially more desirable or revealing, explorations of an LLM's capabilities and internal structures? Essentially, could some LLM limitations be symptoms of "entropy starvation"?

## Methodology

The project employs an A/B testing methodology:
*   **Control:** LLM generation seeded with a fixed PRNG integer (42).
*   **Variable:** LLM generation seeded with a value derived from high-quality true entropy fetched from the ERIS API.
*   **Consistency:** Identical prompts, generation parameters (temperature, top-k, sampling enabled), and model architectures are used for each paired comparison.
*   **Models Tested:** Experiments were run across a range of models including GPT-2, EleutherAI/pythia-410m, TinyLlama/TinyLlama-1.1B-Chat-v1.0, and mistralai/Mistral-7B-Instruct-v0.1 (using 8-bit quantization).

## Key Findings & Observations

Initial results consistently show significant differences in output behavior based on the entropy source. The specific effects vary depending on the base model's capabilities:
*   With less capable models (GPT-2, Pythia), ERIS seeding sometimes led to more exploratory but unstable outputs, yet crucially, it also helped avoid certain catastrophic failure modes (like severe loops or topic derailment) observed with the fixed PRNG seed.
*   With more capable, instruction-tuned models (TinyLlama, Mistral-7B), ERIS seeding frequently facilitated more helpful, structured, nuanced, or creatively distinct outputs compared to the PRNG baseline (e.g., asking clarifying questions, breaking down tasks pedagogically, generating novel narrative twists, producing well-structured poems).
*   These findings suggest that high-quality entropy acts as a significant lever influencing the generative paths, style, depth, and interactive sophistication of LLMs, potentially unlocking higher performance and reliability.

## Detailed Write-up

For a full analysis, methodology details, experimental results, and per-model observations, please see the detailed write-up:

[Entropy MicroDemo - Detailed Analysis (Google Doc)](https://docs.google.com/document/d/e/2PACX-1vSpqNhn3tLQUsphCYYixYbFkQUqTThHUdHyS2n2_-32vNEy4QM2CwanaOpOiGVRFMX2gIH9dflMHJ4O/pub)

## Setup & Usage

The experiments are designed to be run in a Python environment, typically within a Jupyter Notebook or Google Colab.

**Dependencies:**
Ensure the following Python libraries are installed:
```bash
pip install torch transformers requests accelerate bitsandbytes --upgrade
```
*Note:* `bitsandbytes` and `accelerate` are required for running larger models like Mistral-7B with quantization. **Using `bitsandbytes` requires a CUDA-enabled GPU environment.** After installing these libraries, especially `bitsandbytes`, you may need to **restart your runtime/kernel**.

**Environment Variables / Secrets:**
The associated Jupyter Notebook requires the following environment variables or Colab secrets to be set:

*   `HUGGINGFACE_API_KEY`: Your Hugging Face Hub API token. Required for accessing certain models (like Mistral-7B) and potentially for higher download rate limits.
*   `OCCYBYTE_API_KEY`: Your API key for the ERIS Entropy API, used to fetch true entropy seeds.

**Running the Experiment:**
Configure the `MODEL_NAME`, `PROMPTS`, and generation parameters within the script/notebook and execute the Python code. Ensure you have selected a GPU runtime in your environment if using quantization (`load_in_8bit=True` or `BitsAndBytesConfig`).
