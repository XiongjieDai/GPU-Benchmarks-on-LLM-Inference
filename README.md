# GPU-Benchmarks-on-LLM-Inference

Multiple NVIDIA GPUs or Apple Silicon for Large Language Model Inference? 🧐

## Description

Use [llama.cpp](https://github.com/ggerganov/llama.cpp) to test the [LLaMA](https://arxiv.org/abs/2302.13971) models inference speed of different GPUs on [RunPod](https://www.runpod.io/), M1 Max MacBook Pro and M2 Ultra Mac Studio.

## Overview

Average eval time (ms/token) by GPUs. Less eval time is better.

| GPU                        | 7B_q4_0   | 7B_f16   | 65B_q4_0   | 65B_f16   |
|:---------------------------|:----------|:---------|:-----------|:----------|
| A4500 20GB                 | 11.33     | 26.96    | OOM        | OOM       |
| 3090 24GB                  | 8.83      | 19.55    | OOM        | OOM       |
| 4090 24GB                  | **7.24**  | **16.83**| OOM        | OOM       |
| A4500 20GB * 2             | 19.55     | 28.08    | OOM        | OOM       |
| A6000 48GB                 | 9.61      | 23.23    | 70.48      | OOM       |
| A6000ada 48GB              | 51.04     | 166.29   | 736.06     | OOM       |
| 3090 24GB * 2              | 17.95     | 22.69    | 66.94      | OOM       |
| 4090 24GB * 2              | 16.24     | 21.61    | 62.54      | OOM       |
| 3090 24GB * 3              | 21.51     | 25.62    | 72.7       | OOM       |
| 4090 24GB * 3              | 17.16     | 21.05    | **59.71**  | OOM       |
| A100 80GB                  | 11.55     | 14.27    | 68.09      | OOM       |
| A6000 48GB * 2             | 21.49     | 28.21    | 86.83      | OOM       |
| 3090 24GB * 6              | 33.46     | 35.08    | 96.03      | **116.01**|
| M1 Max 24-Core GPU 32GB    | 20.78     | 71.6     | OOM        | OOM       |
| M2 Ultra 76-Core GPU 192GB | 12.04     | 34.96    | 70.21      | 262.87    |

Average prompt eval time (ms/token) by GPUs.

| GPU                        | 7B_q4_0   | 7B_f16   | 65B_q4_0   | 65B_f16   |
|:---------------------------|:----------|:---------|:-----------|:----------|
| A4500 20GB                 | 2.15      | 1.63     | OOM        | OOM       |
| 3090 24GB                  | 1.52      | 1.57     | OOM        | OOM       |
| 4090 24GB                  |**0.92**   | **0.98** | OOM        | OOM       |
| A4500 20GB * 2             | 4.42      | 4.37     | OOM        | OOM       |
| A6000 48GB                 | 1.25      | 1.36     | 9.58       | OOM       |
| A6000ada 48GB              | 4.66      | 6.21     | 79.78      | OOM       |
| 3090 24GB * 2              | 5.37      | 4.83     | 16.91      | OOM       |
| 4090 24GB * 2              | 3.44      | 3.42     | 12.29      | OOM       |
| 3090 24GB * 3              | 7.95      | 7.94     | 24.94      | OOM       |
| 4090 24GB * 3              | 6.87      | 6.66     | 20.72      | OOM       |
| A100 80GB                  | 1.04      | 1.01     | **6.5**    | OOM       |
| A6000 48GB * 2             | 4.96      | 4.86     | 18.66      | OOM       |
| 3090 24GB * 6              | 18.17     | 18.16    | 49.24      | 49.43     |
| M1 Max 24-Core GPU 32GB    | 5.02      | 4.68     | OOM        | OOM       |
| M2 Ultra 76-Core GPU 192GB | 1.65      | 1.57     | 11.91      | **11.07** |


## Model

Thanks to shawwn for LLaMA model weights (7B, 13B, 30B, 65B): [llama-dl](https://github.com/shawwn/llama-dl). Access LLaMA 2 from [Meta AI](https://ai.meta.com/llama/).

## Usage

- For NVIDIA GPUs:

    This provides BLAS acceleration using the CUDA cores of your Nvidia GPU. Multiple GPU works fine with no CPU bottleneck. `-ngl 10000` to make sure all layers are offloaded to GPU. (Thanks to: https://github.com/ggerganov/llama.cpp/pull/1827)
    ```bash
    make clean && LLAMA_CUBLAS=1 make -j
    ```
    Test arguments: (Switch to `.gguf` models after 21 Aug 2023. See: https://github.com/ggerganov/llama.cpp/pull/2398)
    ```bash
    ./main --color  -ngl 10000 -t 1 --temp 0.7 --repeat_penalty 1.1 -n 512 --ignore-eos -m ./models/7B/ggml-model-q4_0.bin  -p "I believe the meaning of life is"
    ```
- For Apple Silicon:

    Using Metal allows the computation to be executed on the GPU for Apple devices:
    ```bash
    make clean && LLAMA_METAL=1 make -j
    ```
    Test arguments:
    ```bash
    ./main --color --no-mmap -ngl 1 --temp 0.7 --repeat_penalty 1.1 -n 512 --ignore-eos -m ./models/7B-v2/ggml-model-q4_0.gguf  -p "I believe the meaning of life is"
    ```
    Check the `recommendedMaxWorkingSetSize` in the result to see how much memory can be allocated on GPU and maintain its performance. Only 65% of unified memory can be allocated to the GPU on 32GB M1 Max, and we expect 75% of usable memory for the GPU on larger memory. (Source: https://developer.apple.com/videos/play/tech-talks/10580/?time=346) To utilize the whole memory, use `-ngl 0` or delete it to only use the CPU for inference. (Thanks to: https://github.com/ggerganov/llama.cpp/pull/1826)
    ```bash
    ./main --color --no-mmap --temp 0.7 --repeat_penalty 1.1 -n 512 --ignore-eos -m ./models/13B-v2/ggml-model-f16.gguf  -p "I believe the meaning of life is"
    ```

## Total VRAM Requirements

### NIVIDA GPUs (snapshots in July 2023)

| Model | Quantized size (4-bit) | Original size (f16) |
|------:|--------------------:|-----------------------:|
|    7B |              4.8 GB |                13.6 GB |
|   13B |              8.7 GB |                25.9 GB |
|   30B |             20.5 GB |                63.7 GB |
|   65B |               40 GB |                 127 GB |

### Apple Silicon (snapshots in Aug 2023)

| Model | Quantized size (4-bit) | Original size (f16) |
|------:|--------------------:|-----------------------:|
|    7B |              4.1 GB |                13.1 GB |
|   13B |              7.6 GB |                  25 GB |
|   30B |             18.3 GB |                61.8 GB |
|   65B |             36.2 GB |               123.6 GB |
|   70B |               38 GB |               130.3 GB |

## Benchmarks

The whole original data, including the 13B and 30B models. Run three times for each model. Add LLaMA 2 for Apple Silicon.

### NVIDIA GPUs (CPU: AMD EPYC, OS: Ubuntu 22.04.2 LTS, pytorch:2.0.1, py3.10, cuda11.8.0 on RunPod) (snapshots in July 2023)

#### LLaMA 🦙:

| GPU | Model | eval time |     |     | prompt eval time |     |     | mean eval time | mean prompt eval time |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A4500 20GB | 7B_q4_0 | 11.29 | 11.34 | 11.36 | 1.52 | 1.52 | 3.4 | 11.33 | 2.15 |
|  | 13B_q4_0 | 19.65 | 19.67 | 19.7 | 2.89 | 2.88 | 2.9 | 19.67 | 2.89 |
|  | 30B_q4_0 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 65B_q4_0 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 7B_f16 | 26.94 | 26.94 | 26.99 | 1.62 | 1.63 | 1.63 | 26.96 | 1.63 |
|  | 13B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 30B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| 3090 24GB | 7B_q4_0 | 8.83 | 8.81 | 8.84 | 1.56 | 1.5 | 1.5 | 8.83 | 1.52 |
|  | 13B_q4_0 | 14.41 | 14.42 | 14.41 | 2.53 | 2.49 | 2.49 | 14.41 | 2.5 |
|  | 30B_q4_0 | 33.67 | 33.75 | 33.8 | 5.78 | 5.78 | 5.78 | 33.74 | 5.78 |
|  | 65B_q4_0 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 7B_f16 | 19.54 | 19.55 | 19.56 | 1.57 | 1.57 | 1.57 | 19.55 | 1.57 |
|  | 13B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 30B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| 4090 24GB | 7B_q4_0 | 7.23 | 7.23 | 7.26 | 0.92 | 0.92 | 0.93 | 7.24 | 0.92 |
|  | 13B_q4_0 | 12.06 | 12.06 | 12.07 | 1.64 | 1.64 | 1.64 | 12.06 | 1.64 |
|  | 30B_q4_0 | 27.4 | 27.44 | 27.46 | 3.88 | 3.88 | 3.89 | 27.43 | 3.88 |
|  | 65B_q4_0 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 7B_f16 | 16.84 | 16.82 | 16.83 | 0.98 | 0.98 | 0.98 | 16.83 | 0.98 |
|  | 13B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 30B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| A4500 20GB * 2 | 7B_q4_0 | 19.5 | 19.63 | 19.51 | 4.44 | 4.39 | 4.42 | 19.55 | 4.42 |
|  | 13B_q4_0 | 27.75 | 27.55 | 27.6 | 5.79 | 5.81 | 5.81 | 27.63 | 5.8 |
|  | 30B_q4_0 | 50.95 | 50.73 | 51.08 | 10.07 | 10.02 | 10.06 | 50.92 | 10.05 |
|  | 65B_q4_0 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 7B_f16 | 28.06 | 28.02 | 28.16 | 4.37 | 4.39 | 4.34 | 28.08 | 4.37 |
|  | 13B_f16 | 43.15 | 43.18 | 43.11 | 5.71 | 5.74 | 5.74 | 43.15 | 5.73 |
|  | 30B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| A6000 48GB | 7B_q4_0 | 9.62 | 9.6 | 9.61 | 1.26 | 1.25 | 1.25 | 9.61 | 1.25 |
|  | 13B_q4_0 | 15.96 | 16 | 16 | 2.1 | 2.11 | 2.11 | 15.99 | 2.11 |
|  | 30B_q4_0 | 36.79 | 36.86 | 36.91 | 4.83 | 4.84 | 4.86 | 36.85 | 4.84 |
|  | 65B_q4_0 | 70.37 | 70.47 | 70.59 | 9.55 | 9.58 | 9.6 | 70.48 | 9.58 |
|  | 7B_f16 | 23.23 | 23.23 | 23.24 | 1.36 | 1.36 | 1.36 | 23.23 | 1.36 |
|  | 13B_f16 | 41.38 | 41.4 | 41.4 | 2.31 | 2.31 | 2.3 | 41.39 | 2.31 |
|  | 30B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| A6000ada 48GB | 7B_q4_0 | 47.78 | 51.21 | 54.14 | 4.4 | 4.69 | 4.89 | 51.04 | 4.66 |
|  | 13B_q4_0 | 116.38 | 125.55 | 127.52 | 12.46 | 13.44 | 13.9 | 123.15 | 13.27 |
|  | 30B_q4_0 | 341.13 | 344.18 | 340.12 | 37.76 | 38.48 | 37.74 | 341.81 | 37.99 |
|  | 65B_q4_0 | 723.65 | 743.35 | 741.19 | 77.63 | 80.68 | 81.04 | 736.06 | 79.78 |
|  | 7B_f16 | 164.97 | 168.95 | 164.95 | 6.29 | 6.18 | 6.16 | 166.29 | 6.21 |
|  | 13B_f16 | 303.48 | 303.54 | 303.5 | 15.06 | 15.52 | 15.62 | 303.51 | 15.4 |
|  | 30B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| 3090 24GB * 2 | 7B_q4_0 | 17.99 | 17.93 | 17.92 | 6.21 | 4.78 | 5.11 | 17.95 | 5.37 |
|  | 13B_q4_0 | 24.2 | 24.23 | 24.28 | 6.82 | 6.91 | 6.82 | 24.24 | 6.85 |
|  | 30B_q4_0 | 42.99 | 42.92 | 42.88 | 11.63 | 11.62 | 11.82 | 42.93 | 11.69 |
|  | 65B_q4_0 | 66.82 | 66.84 | 67.16 | 17.56 | 16.23 | 16.95 | 66.94 | 16.91 |
|  | 7B_f16 | 22.7 | 22.73 | 22.64 | 4.69 | 4.8 | 5 | 22.69 | 4.83 |
|  | 13B_f16 | 33.93 | 33.83 | 33.97 | 6.82 | 6.8 | 6.74 | 33.91 | 6.79 |
|  | 30B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| 4090 24GB * 2 | 7B_q4_0 | 16.25 | 16.25 | 16.21 | 3.47 | 3.45 | 3.4 | 16.24 | 3.44 |
|  | 13B_q4_0 | 22.23 | 22.2 | 22.43 | 4.77 | 4.82 | 4.79 | 22.29 | 4.79 |
|  | 30B_q4_0 | 40.27 | 40.48 | 40.21 | 8.34 | 8.33 | 8.22 | 40.32 | 8.3 |
|  | 65B_q4_0 | 62.71 | 62.56 | 62.35 | 12.41 | 12.18 | 12.27 | 62.54 | 12.29 |
|  | 7B_f16 | 21.58 | 21.62 | 21.64 | 3.47 | 3.4 | 3.4 | 21.61 | 3.42 |
|  | 13B_f16 | 32.67 | 32.64 | 32.64 | 4.79 | 4.83 | 4.77 | 32.65 | 4.8 |
|  | 30B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| 3090 24GB * 3 | 7B_q4_0 | 21.52 | 21.48 | 21.54 | 8 | 7.92 | 7.92 | 21.51 | 7.95 |
|  | 13B_q4_0 | 28.42 | 28.42 | 28.45 | 11.04 | 10.69 | 10.96 | 28.43 | 10.9 |
|  | 30B_q4_0 | 48.12 | 47.95 | 48.1 | 17.17 | 17.57 | 17.29 | 48.06 | 17.34 |
|  | 65B_q4_0 | 72.68 | 72.59 | 72.84 | 24.91 | 24.87 | 25.03 | 72.7 | 24.94 |
|  | 7B_f16 | 25.64 | 25.57 | 25.66 | 7.97 | 7.94 | 7.91 | 25.62 | 7.94 |
|  | 13B_f16 | 35.67 | 35.79 | 35.77 | 10.76 | 10.66 | 10.73 | 35.74 | 10.72 |
|  | 30B_f16 | 63.79 | 63.73 | 63.88 | 17.55 | 17.56 | 17.25 | 63.8 | 17.45 |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| 4090 24GB * 3 | 7B_q4_0 | 17.07 | 17.24 | 17.16 | 7.08 | 6.73 | 6.8 | 17.16 | 6.87 |
|  | 13B_q4_0 | 22.95 | 22.93 | 22.73 | 8.4 | 8.78 | 8.96 | 22.87 | 8.71 |
|  | 30B_q4_0 | 39.67 | 39.86 | 39.77 | 14.38 | 14.01 | 14.18 | 39.77 | 14.19 |
|  | 65B_q4_0 | 59.65 | 59.78 | 59.7 | 20.77 | 20.22 | 21.16 | 59.71 | 20.72 |
|  | 7B_f16 | 21.05 | 21.07 | 21.04 | 6.68 | 6.51 | 6.8 | 21.05 | 6.66 |
|  | 13B_f16 | 30.28 | 30.36 | 30.22 | 8.59 | 9 | 8.34 | 30.29 | 8.64 |
|  | 30B_f16 | 57.06 | 57.06 | 57.15 | 14.32 | 14.43 | 14.17 | 57.09 | 14.31 |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| A100 80GB | 7B_q4_0 | 11.61 | 11.54 | 11.49 | 1.11 | 1.01 | 1 | 11.55 | 1.04 |
|  | 13B_q4_0 | 17.35 | 17.42 | 17.34 | 1.65 | 1.65 | 1.65 | 17.37 | 1.65 |
|  | 30B_q4_0 | 35.29 | 35.16 | 35.26 | 3.62 | 3.56 | 3.58 | 35.24 | 3.59 |
|  | 65B_q4_0 | 68.13 | 68.08 | 68.07 | 6.5 | 6.5 | 6.51 | 68.09 | 6.5 |
|  | 7B_f16 | 14.26 | 14.28 | 14.28 | 1.01 | 1.01 | 1.01 | 14.27 | 1.01 |
|  | 13B_f16 | 23.58 | 23.49 | 23.49 | 1.64 | 1.65 | 1.64 | 23.52 | 1.64 |
|  | 30B_f16 | 52.09 | 52.3 | 52.36 | 3.55 | 3.62 | 3.57 | 52.25 | 3.58 |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| A6000 48GB * 2 | 7B_q4_0 | 21.67 | 21.45 | 21.35 | 5.05 | 5.11 | 4.73 | 21.49 | 4.96 |
|  | 13B_q4_0 | 29.47 | 30.75 | 29.22 | 6.22 | 6.17 | 7.06 | 29.81 | 6.48 |
|  | 30B_q4_0 | 55.22 | 56.35 | 56.44 | 11.42 | 11.58 | 11.02 | 56 | 11.34 |
|  | 65B_q4_0 | 86.42 | 84.99 | 89.09 | 19.3 | 18.6 | 18.08 | 86.83 | 18.66 |
|  | 7B_f16 | 28.42 | 28.67 | 27.55 | 4.97 | 5.08 | 4.52 | 28.21 | 4.86 |
|  | 13B_f16 | 43.43 | 43.42 | 43.74 | 6.23 | 6.29 | 6.26 | 43.53 | 6.26 |
|  | 30B_f16 | 84.32 | 87.28 | 87.41 | 12.03 | 12.45 | 11.97 | 86.34 | 12.15 |
|  | 65B_f16 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| 3090 24GB * 6 | 7B_q4_0 | 33.56 | 33.4 | 33.42 | 18.06 | 18.38 | 18.07 | 33.46 | 18.17 |
|  | 13B_q4_0 | 42.75 | 43.02 | 42.9 | 23.1 | 23.26 | 23.12 | 42.89 | 23.16 |
|  | 30B_q4_0 | 67.82 | 67.4 | 67.35 | 35.61 | 35.7 | 35.66 | 67.52 | 35.66 |
|  | 65B_q4_0 | 96.15 | 96.1 | 95.85 | 49.61 | 49.18 | 48.93 | 96.03 | 49.24 |
|  | 7B_f16 | 35.12 | 35.05 | 35.06 | 18.26 | 18.08 | 18.15 | 35.08 | 18.16 |
|  | 13B_f16 | 45.78 | 46.03 | 45.77 | 22.99 | 23.14 | 23 | 45.86 | 23.04 |
|  | 30B_f16 | 75.76 | 75.84 | 75.98 | 35.63 | 35.81 | 35.62 | 75.86 | 35.69 |
|  | 65B_f16 | 116.09 | 116.03 | 115.9 | 49.27 | 49.43 | 49.6 | 116.01 | 49.43 |

### Apple Silicon (snapshots in Aug 2023)

#### LLaMA 🦙:

| GPU | Model | eval time |     |     | prompt eval time |     |     | mean eval time | mean prompt eval time |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| M1 Max 24-Core GPU 32GB    | 7B_q4_0       | 20.46       | 20.45        | 21.44        | 5.02               | 5.02         | 5.01         | 20.78            | 5.02                    |
|                            | 13B_q4_0      | 34.97       | 36.85        | 40.58        | 8.93               | 10.42        | 10.76        | 37.47            | 10.04                   |
|                            | 30B_q4_0      | 75.88       | 76.15        | 76.21        | 20.29              | 20.27        | 20.29        | 76.08            | 20.28                   |
|                            | 65B_q4_0      | OOM         | OOM          | OOM          | OOM                | OOM          | OOM          | OOM              | OOM                     |
|                            | 7B_f16        | 71.5        | 71.65        | 71.64        | 4.68               | 4.69         | 4.68         | 71.6             | 4.68                    |
|                            | 13B_f16 (CPU) | 222.71      | 230.77       | 222.93       | 37.66              | 35.52        | 53.63        | 225.47           | 42.27                   |
|                            | 30B_f16       | OOM         | OOM          | OOM          | OOM                | OOM          | OOM          | OOM              | OOM                     |
|                            | 65B_f16       | OOM         | OOM          | OOM          | OOM                | OOM          | OOM          | OOM              | OOM                     |
| M2 Ultra 76-Core GPU 192GB | 7B_q4_0       | 12.04       | 12.03        | 12.04        | 1.64               | 1.65         | 1.65         | 12.04            | 1.65                    |
|                            | 13B_q4_0      | 19.52       | 19.08        | 19.07        | 2.91               | 2.9          | 2.9          | 19.22            | 2.9                     |
|                            | 30B_q4_0      | 40.66       | 39.54        | 40.75        | 6.49               | 6.47         | 6.47         | 40.32            | 6.48                    |
|                            | 65B_q4_0      | 71.12       | 69.87        | 69.65        | 11.94              | 11.87        | 11.92        | 70.21            | 11.91                   |
|                            | 7B_f16        | 34.86       | 34.99        | 35.02        | 1.57               | 1.57         | 1.56         | 34.96            | 1.57                    |
|                            | 13B_f16       | 61.47       | 62.13        | 63.01        | 2.71               | 2.73         | 2.72         | 62.2             | 2.72                    |
|                            | 30B_f16       | 138.91      | 137.71       | 139.48       | 6                  | 6.03         | 6.01         | 138.7            | 6.01                    |
|                            | 65B_f16       | 262.82      | 262.95       | 262.83       | 11.08              | 11.06        | 11.06        | 262.87           | 11.07                   |

#### LLaMA 2 🦙🦙:

| GPU | Model | eval time |     |     | prompt eval time |     |     | mean eval time | mean prompt eval time |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| M1 Max 24-Core GPU 32GB    | 7B_q4_0       | 20.49       | 20.65        | 20.53        | 5.01               | 5.01         | 5.01         | 20.56            | 5.01                    |
|                            | 13B_q4_0      | 38.31       | 37.36        | 38.01        | 8.95               | 9.9          | 10.97        | 37.89            | 9.94                    |
|                            | 70B_q4_0      | OOM         | OOM          | OOM          | OOM                | OOM          | OOM          | OOM              | OOM                     |
|                            | 7B_f16        | 71.52       | 72.63        | 72.83        | 4.7                | 4.68         | 4.7          | 72.33            | 4.69                    |
|                            | 13B_f16 (CPU) | 227.05      | 225.6        | 228.64       | 36.28              | 37.9         | 40.11        | 227.1            | 38.1                    |
|                            | 70B_f16       | OOM         | OOM          | OOM          | OOM                | OOM          | OOM          | OOM              | OOM                     |
| M2 Ultra 76-Core GPU 192GB | 7B_q4_0       | 12.07       | 12.05        | 12           | 1.65               | 1.64         | 1.66         | 12.04            | 1.65                    |
|                            | 13B_q4_0      | 19.09       | 19.09        | 19.11        | 2.91               | 2.9          | 2.91         | 19.1             | 2.91                    |
|                            | 70B_q4_0      | 73.55       | 72.95        | 73.06        | 13.76              | 13.75        | 13.74        | 73.19            | 13.75                   |
|                            | 7B_f16        | 34.18       | 34.19        | 34.22        | 1.55               | 1.55         | 1.55         | 34.2             | 1.55                    |
|                            | 13B_f16       | 61.48       | 61.53        | 61.54        | 2.71               | 2.71         | 2.71         | 61.52            | 2.71                    |
|                            | 70B_f16       | 277.83      | 278.09       | 277.73       | 12.94              | 12.93        | 12.93        | 277.88           | 12.93                   |

## Conclusion

Same performance on LLaMA and LLaMA 2 of the same size and quantization. Multiple NVIDIA GPUs might affect the performance.

For LLM inference, buy 3090s to save money. Buy 4090s if you want to speed up. Buy A100s if you are rich. Buy Mac Studio if you want to put your computer on your desk, save energy, be quiet, and don't wanna maintenance. (If you want to **train** LLM, choose NIVIDA.)

If you find this information helpful, please give me a star. Feel free to contact me if you have any advice. Thank you. 🤗

## Appendix

All the latest results are welcome! 🥰 (Thanks to: MichaelDays ❤️)

### Mac

#### LLaMA 2 🦙🦙:

| GPU | Model | eval time |     |     | prompt eval time |     |     | mean eval time | mean prompt eval time |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Mac Pro (2019 intel/16 core/384GB) CPU | 7B_q4_0| 54.78 | 61.74 | 59.77 | 35.72 | 37.29 | 35.88 | 58.76 | 36.30 |
| | 13B_q4_0 | 102.20 | 101.97 | 100.26 | 63.28 | 62.77 | 62.93 | 101.48 | 62.99 |
| | 70B_q4_0 | 547.89 | 456.45 | 457.12 | 295.31 | 296.16 | 295.12 | 487.15 | 295.53 |
| M2 Mini Pro (12/19/32GB) CPU | 7B_q4_0 | 48.04 | 45.78 | 49.61 | 17.34 | 16.81 | 16.82 | 47.81 | 16.99 |
| | 13B_q4_0 | 75.93 | 72.56 | 71.66 | 29.39 | 28.50 | 28.46 | 73.38 | 28.78 |
| | 70B_q4_0 | 225956.54 | OOM | OOM | 27220.19 | OOM | OOM | OOM | OOM |
| | 7B.f16 | 118.10 | 116.46 | 118.95 | 17.11 | 16.62 | 17.84 | 117.84 | 17.19 |
| M2 Mini Pro (12/19/32GB) GPU | 7B_q4_0 | 28.92 | 28.54 | 29.27 | 5.89 | 5.95 | 5.86 | 28.91 | 5.90 |
| | 13B_q4_0 | 50.76 | 50.71 | 50.63 | 10.30 | 10.29 | 10.28 | 50.70 | 10.29 |
| | 70B_q4_0 | OOM | OOM | OOM | OOM | OOM | OOM | OOM | OOM |
| | 7B.f16 | 92.43 | 91.91 | 92.11 | 5.56 | 5.53 | 5.56 | 92.15 | 5.55 |

