# Running DeepSeek-R1 Locally Using Ollama

This page briefly overviews the benefits of running DeepSeek-R1 locally and provides the steps for doing so with Ollama.

Although you can run DeepSeek-R1 through either the web or API calls, running the model locally can be beneficial for:

- controlling your data and security.
- fine-tuning performance and controls.
- adjusting the model for your needs and projects.

Because DeepSeek-R1 is open-source, Ollama - a free tool for running LLMs locally - already has the model available for public use.

## Procedure

1. Install Ollama through its [download page](https://ollama.com/download).
   - Select your OS, if the page has not automatically identified your OS.
   - Download and install Ollama.
> [!IMPORTANT]
> As of February 5th, 2025, for Windows and macOS, Ollama cannot run on operating systems that are older than Windows 10 or macOS 11 Big Sur, respectively.

2. Open your OS terminal, and pull the DeepSeek-R1 model that best matches your needs and computer specifications:
   ```
   ollama pull <DeepSeek-R1 Model>
   ```

> [!IMPORTANT]
> To ensure that your computer can properly run whichever model you choose, check that your computer hardware specifications match the required specifications for the model.
>
> Afterwards, replace `<DeepSeek-R1 Model>` with your chosen model.

3. Once installed, run your chosen model from the terminal:
   ```
   ollama run <DeepSeek-R1 Model>
   ```
     - A prompt line should appear, in which you can converse with the AI. Press enter after any prompt to see DeepSeek-R1's response.

4. When finished with using R1, type and enter `/bye` to exit the model interface. Additionally, stop your computer from running the model:
   ```
   ollama stop <DeepSeek-R1 Model>
   ```

## DeepSeek-R1 Models and Requirements
The original DeepSeek-R1 is large and is computationally intensive. For users with computers that are not designed to run LLMs, DeepSeek has distilled the original model into smaller, easier-to-run models. Though the models are up to hundreds of times smaller than the original, tests have indicated that the models demonstrate good performance and accuracy, though not to the same capabilities as the original. 

The following table provides details on and approximate requirements for each model. Replace `<DeepSeek-R1 Model>` for the previous command lines with the model name provided in the table.

| Model Name in Ollama | Parameters (in billions) | VRAM Requirement(in GB) | Recommended GPU |
| :--- | :--- | :--- | :--- | 
| `deepseek-r1:1.5b` | 1.5 | ~0.7 | NVIDIA RTX 3060 12GB or higher |
| `deepseek-r1:7b` | 7 | ~3.3 | NVIDIA RTX 3070 8GB or higher |
| `deepseek-r1:8b` | 8 | ~3.7 | NVIDIA RTX 3070 8GB or higher
| `deepseek-r1:14b` | 14 | ~6.5 | NVIDIA RTX 3080 10GB or higher |
| `deepseek-r1:32b` | 32 | ~14.9 | NVIDIA RTX 4090 24GB or higher |
| `deepseek-r1:70b` | 70 | ~32.7 | NVIDIA RTX 4090 24GB ×2 |
| `deepseek-r1:671b` | 671 | ~1,342 | NVIDIA H100 80GB (6x or more) |
