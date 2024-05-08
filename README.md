# GPU-Benchmarks-on-LLM-Inference

Multiple NVIDIA GPUs or Apple Silicon for Large Language Model Inference? 🧐

## Description

Use [llama.cpp](https://github.com/ggerganov/llama.cpp) to test the [LLaMA](https://arxiv.org/abs/2302.13971) models inference speed of different GPUs on [RunPod](https://www.runpod.io/), 16-inch M1 Max MacBook Pro, M2 Ultra Mac Studio, 14-inch M3 MacBook Pro and 16-inch M3 Max MacBook Pro for LLaMA & LLaMA 2; and 13-inch M1 MacBook Air, 14-inch M1 Max MacBook Pro, M2 Ultra Mac Studio and 16-inch M3 Max MacBook Pro for LLaMA 3.

## Overview

Average eval speed (tokens/s) by GPUs on LLaMA 2. A higher eval speed is better.

| GPU                        | 7B_q4_0   | 7B_f16   | 70B_q4_0   | 70B_f16   |
|:---------------------------|:----------|:---------|:-----------|:----------|
| 4080 16GB                  | 118.01    | 44.2     | OOM        | OOM       |
| RTX 4000 Ada 20GB          | 64.53     | 22.96    | OOM        | OOM       |
| 3090 24GB                  | 120.6     | 51.85    | OOM        | OOM       |
| 4090 24GB                  | **149.37**| 60.78    | OOM        | OOM       |
| RTX 5000 Ada 32GB          | 99.19     | 36.31    | OOM        | OOM       |
| RTX 4000 Ada 20GB * 2      | 60.68     | 31.83    | OOM        | OOM       |
| 3090 24GB * 2              | 53.25     | 38.73    | 14.28      | OOM       |
| 4090 24GB * 2              | 66.26     | 49.2     | 18.5       | OOM       |
| RTX A6000 48GB             | 111.57    | 44.01    | 15.18      | OOM       |
| RTX 6000 Ada 48GB          | 127.45    | 50.56    | 17.02      | OOM       |
| 3090 24GB * 3              | 46.14     | 35.83    | 13.64      | OOM       |
| 4090 24GB * 3              | 62.14     | 50.39    | 14.92      | OOM       |
| RTX 4000 Ada 20GB * 4      | 55.41     | 40.52    | 15.48      | OOM       |
| A100 80GB                  | 136.24    | 73.83    | 25.68      | OOM       |
| H100 PCIe 80GB             | 133.77    | **83.32**| **25.88**  | OOM       |
| RTX A6000 48GB * 2         | 74.43     | 47.08    | 17.31      | OOM       |
| 3090 24GB * 6              | 27.71     | 27.37    | 10.15      | 7.54      |
| 4090 24GB * 6              | 38.19     | 37.5     | 15.59      | **10.24** |
| M1 Max 24-Core GPU 32GB    | 48.65     | 13.83    | OOM        | OOM       |
| M2 Ultra 76-Core GPU 192GB | 91.89     | 40.68    | 14.37      | 4.81      |
| M3 10-Core GPU 16GB        | 20.79     | 3.61     | OOM        | OOM       |
| M3 Max 40-Core GPU 48GB    | 63.11     | 24.76    | 3.11       | OOM       |

Average prompt eval speed (tokens/s) by GPUs on LLaMA 2.

| GPU                        | 7B_q4_0   | 7B_f16   | 70B_q4_0   | 70B_f16   |
|:---------------------------|:----------|:---------|:-----------|:----------|
| 4080 16GB                  | 3774.79   | 5158.19  | OOM        | OOM       |
| RTX 4000 Ada 20GB          | 1819.86   | 2369.09  | OOM        | OOM       |
| 3090 24GB                  | 2601.81   | 3075.37  | OOM        | OOM       |
| 4090 24GB                  |**5531.19**| 7348.69  | OOM        | OOM       |
| RTX 5000 Ada 32GB          | 3525.7    | 4936.59  | OOM        | OOM       |
| RTX 4000 Ada 20GB * 2      | 522.31    | 499.6    | OOM        | OOM       |
| 3090 24GB * 2              | 252.15    | 247.31   | 48.46      | OOM       |
| 4090 24GB * 2              | 1711.03   | 1729.35  | 256.97     | OOM       |
| RTX A6000 48GB             | 2979.12   | 3707.88  | 405.97     | OOM       |
| RTX 6000 Ada 48GB          | 3911.18   | 5403.6   | 480.93     | OOM       |
| 3090 24GB * 3              | 236.49    | 231.55   | 41.27      | OOM       |
| 4090 24GB * 3              | 996.25    | 1006.19  | 178.36     | OOM       |
| RTX 4000 Ada 20GB * 4      | 280.49    | 293.29   | 46.76      | OOM       |
| A100 80GB                  | 3443.98   | 5105.77  | 612.41     | OOM       |
| H100 PCIe 80GB             | 4868.59   | **7367** | **855.73** | OOM       |
| RTX A6000 48GB * 2         | 1309.83   | 1145.15  | 198.24     | OOM       |
| 3090 24GB * 6              | 89.74     | 87.62    | 14.8       | 14.8      |
| 4090 24GB * 6              | 437.77    | 425.87   | 88.47      | 85.1      |
| M1 Max 24-Core GPU 32GB    | 199.65    | 213.14   | OOM        | OOM       |
| M2 Ultra 76-Core GPU 192GB | 1217.03   | 1379.54  | 133.18     | **150.98**|
| M3 10-Core GPU 16GB        | 184.51    | 25.21    | OOM        | OOM       |
| M3 Max 40-Core GPU 48GB    | 740.97    | 761.4    | 8.68       | OOM       |


## Model

Thanks to shawwn for LLaMA model weights (7B, 13B, 30B, 65B): [llama-dl](https://github.com/shawwn/llama-dl). Access LLaMA 2 from [Meta AI](https://ai.meta.com/llama/). Access LLaMA 3 from [Meta Llama 3](https://huggingface.co/collections/meta-llama/meta-llama-3-66214712577ca38149ebb2b6) on Hugging Face or my Hugging Face repos: [Xiongjie Dai](https://huggingface.co/JaaackXD).

## Usage

### Build

- For NVIDIA GPUs, this provides BLAS acceleration using the CUDA cores of your Nvidia GPU:

    ```bash
    !make clean && LLAMA_CUBLAS=1 make -j
    ```

- For Apple Silicon, Metal is enabled by default:

    ```bash
    !make clean && make -j
    ```

### Text completion

Use argument `-ngl 0` to only use the CPU for inference, `-ngl 10000` to make sure all layers are offloaded to GPU.

```bash
!./main -ngl 10000 -m ./models/8B-v3/ggml-model-Q4_K_M.gguf --color --temp 1.1 --repeat_penalty 1.1 -c 0 -n 1024 -e -s 0 -p """\
First Citizen:\n\n\
Before we proceed any further, hear me speak.\n\n\
\n\n\
All:\n\n\
Speak, speak.\n\n\
\n\n\
First Citizen:\n\n\
You are all resolved rather to die than to famish?\n\n\
\n\n\
All:\n\n\
Resolved. resolved.\n\n\
\n\n\
First Citizen:\n\n\
First, you know Caius Marcius is chief enemy to the people.\n\n\
\n\n\
All:\n\n\
We know't, we know't.\n\n\
\n\n\
First Citizen:\n\n\
Let us kill him, and we'll have corn at our own price. Is't a verdict?\n\n\
\n\n\
All:\n\n\
No more talking on't; let it be done: away, away!\n\n\
\n\n\
Second Citizen:\n\n\
One word, good citizens.\n\n\
\n\n\
First Citizen:\n\n\
We are accounted poor citizens, the patricians good. What authority surfeits on would relieve us: if they would yield us but the superfluity, \
while it were wholesome, we might guess they relieved us humanely; but they think we are too dear: the leanness that afflicts us, the object of \
our misery, is as an inventory to particularise their abundance; our sufferance is a gain to them Let us revenge this with our pikes, \
ere we become rakes: for the gods know I speak this in hunger for bread, not in thirst for revenge.\n\n\
\n\n\
"""
```

**Note:** For Apple Silicon, check the `recommendedMaxWorkingSetSize` in the result to see how much memory can be allocated on GPU and maintain its performance. Only **70%** of unified memory can be allocated to the GPU on 32GB M1 Max right now, and we expect around **78%** of usable memory for the GPU on larger memory. (Source: https://developer.apple.com/videos/play/tech-talks/10580/?time=346) To utilize the whole memory, use `-ngl 0` to only use the CPU for inference. (Thanks to: https://github.com/ggerganov/llama.cpp/pull/1826)

### Chat template for LLaMA 3

```bash
!./main -ngl 10000 -m ./models/8B-v3-instruct/ggml-model-Q4_K_M.gguf --color -c 0 -n -2 -e -s 0 --mirostat 2 -i --no-display-prompt --keep -1 \
-r '<|eot_id|>' -p '<|begin_of_text|><|start_header_id|>system<|end_header_id|>\n\nYou are a helpful assistant.<|eot_id|><|start_header_id|>user<|end_header_id|>\n\nHi!<|eot_id|><|start_header_id|>assistant<|end_header_id|>\n\n' \
--in-prefix '<|start_header_id|>user<|end_header_id|>\n\n' --in-suffix '<|eot_id|><|start_header_id|>assistant<|end_header_id|>\n\n'
```

### Benchmark

```bash
!./llama-bench -p 512,1024,4096,8192 -n 512,1024,4096,8192 -m ./models/8B-v3/ggml-model-Q4_K_M.gguf
```

## Total VRAM Requirements

### LLaMA 3 🦙🦙🦙:

| Model | Quantized size (Q4_K_M) | Original size (f16) |
|------:|--------------------:|-----------------------:|
|    8B |             4.58 GB |               14.96 GB |
|   70B |            39.59 GB |              131.42 GB |

### LLaMA 🦙 & 2 🦙🦙:

#### NIVIDA GPUs (snapshots in Dec 2023)

| Model | Quantized size (Q4_0) | Original size (f16) |
|------:|--------------------:|-----------------------:|
|    7B |             3.82 GB |               12.63 GB |
|   13B |             7.24 GB |               24.41 GB |
|   30B |            17.84 GB |                63.7 GB |
|   65B |             35.5 GB |              122.48 GB |
|   70B |            36.37 GB |               128.3 GB |

#### Apple Silicon (snapshots in Dec 2023)

| Model | Quantized size (Q4_0) | Original size (f16) |
|------:|--------------------:|-----------------------:|
|    7B |             3.89 GB |               12.88 GB |
|   13B |             7.33 GB |               24.71 GB |
|   30B |            17.96 GB |               61.45 GB |
|   65B |            35.64 GB |              123.47 GB |
|   70B |            36.51 GB |              129.27 GB |

## Benchmarks

`PP` means "prompt processing," and `TG` means "text-generation." # for total processing/generated tokens. Average speed in tokens/s.

### LLaMA 3 🦙🦙🦙:

#### Apple Silicon (snapshots in May 2024)

| GPU                        | Model     | tg 512 | tg 1024 | tg 4096 | tg 8192 | pp 512  | pp 1024 | pp 4096 | pp 8192 |
|----------------------------|-----------|--------|---------|---------|---------|---------|---------|---------|---------|
| M1 7‑Core GPU 8GB          | 8B Q4_K_M | 10.20  | 9.72    | 11.77   | OOM     | 94.48   | 87.26   | 96.53   | OOM     |
| M1 Max 32‑Core GPU 64GB    | 8B Q4_K_M | 35.73  | 34.49   | 31.18   | 26.84   | 408.23  | 355.45  | 329.84  | 302.92  |
|                            | 8B F16    | 18.75  | 18.43   | 16.33   | 15.03   | 517.34  | 418.77  | 374.09  | 351.46  |
|                            | 70B Q4_K_M| 4.34   | 4.09    | 4.09    | 3.71    | 34.96   | 33.01   | 32.64   | 30.97   |
| M2 Ultra 76-Core GPU 192GB | 8B Q4_K_M | 78.81  | 76.28   | 64.58   | 54.13   | 994.04  | 1023.89 | 979.47  | 913.55  |
|                            | 8B F16    | 36.90  | 36.25   | 33.67   | 30.68   | 1175.40 | 1202.74 | 1194.21 | 1103.44 |
|                            | 70B Q4_K_M| 12.48  | 12.13   | 10.75   | 9.34    | 118.79  | 117.76  | 109.53  | 108.57  |
|                            | 70B F16   | 4.76   | 4.71    | 4.48    | 4.23    | 147.58  | 145.82  | 133.75  | 135.15  |
| M3 Max 40‑Core GPU 64GB    | 8B Q4_K_M | 48.97  | 50.74   | 44.21   | 36.12   | 693.32  | 678.04  | 573.09  | 505.32  |
|                            | 8B F16    | 22.04  | 22.39   | 20.72   | 18.74   | 769.84  | 751.49  | 609.97  | 515.15  |
|                            | 70B Q4_K_M| 7.65   | 7.53    | 6.58    | 5.60    | 70.19   | 62.88   | 64.90   | 61.96   |

### LLaMA 🦙:

#### NVIDIA GPUs (CPU: AMD EPYC, OS: Ubuntu 22.04.2 LTS, pytorch:2.1.1, py: 3.10, cuda: 12.1.1 or 11.8.0 on RunPod) (snapshots in Dec 2023)

| GPU                        | Model          | TG [t/s]   |     |     | PP [t/s]   |     |     | mean TG [t/s]   | mean PP [t/s]   |
|:---------------------------|:---------------|:-----------|:-------------|:-------------|:-----------|:-------------|:-------------|:----------------|:----------------|
| 4080 16GB                  | 7B_q4_0        | 118.26     | 117.91       | 117.98       | 3730.53    | 3765.79      | 3781.64      | 118.05          | 3759.32         |
|                            | 7B_f16         | 44.22      | 44.22        | 44.21        | 5127.89    | 5299.86      | 5216.58      | 44.22           | 5214.78         |
|                            | 13B_q4_0       | 67.85      | 67.91        | 67.99        | 2230.36    | 2271.01      | 2236.63      | 67.92           | 2246            |
|                            | 13B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 30B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX 4000 Ada 20GB          | 7B_q4_0        | 64.83      | 64.56        | 64.7         | 1485.86    | 1819.13      | 1814.07      | 64.7            | 1706.35         |
|                            | 7B_f16         | 22.98      | 22.97        | 22.96        | 2365.84    | 2355.97      | 2357.47      | 22.97           | 2359.76         |
|                            | 13B_q4_0       | 36.48      | 36.38        | 36.39        | 1044.6     | 1038.25      | 1039.71      | 36.42           | 1040.85         |
|                            | 13B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 30B_q4_0       | 15.54      | 15.55        | 15.54        | 466.82     | 463.14       | 463.11       | 15.54           | 464.36          |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 3090 24GB                  | 7B_q4_0        | 120.84     | 121.12       | 121.55       | 2497.43    | 2625.36      | 2623.24      | 121.17          | 2582.01         |
|                            | 7B_f16         | 51.82      | 51.86        | 51.76        | 3141.79    | 3224.82      | 3140.9       | 51.81           | 3169.17         |
|                            | 13B_q4_0       | 75.09      | 74.99        | 74.95        | 1679.16    | 1596.61      | 1628.07      | 75.01           | 1634.61         |
|                            | 13B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 30B_q4_0       | 33.71      | 33.7         | 33.64        | 769.23     | 767.21       | 784.02       | 33.68           | 773.49          |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 4090 24GB                  | 7B_q4_0        | 149.67     | 149.21       | 149.35       | 4113.14    | 5427.62      | 2343.25      | 149.41          | 3961.34         |
|                            | 7B_f16         | 60.86      | 61.04        | 60.87        | 7178.04    | 7268.87      | 7286.89      | 60.92           | 7244.6          |
|                            | 13B_q4_0       | 88.77      | 88.84        | 89.02        | 3141.23    | 3143.35      | 3140.72      | 88.88           | 3141.77         |
|                            | 13B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 30B_q4_0       | 40.13      | 40.04        | 40.1         | 1415.81    | 1414.77      | 1412.93      | 40.09           | 1414.5          |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX 5000 Ada 32GB          | 7B_q4_0        | 100.01     | 99.77        | 99.93        | 3812.96    | 3792.65      | 3714.7       | 99.9            | 3773.44         |
|                            | 7B_f16         | 36.24      | 36.38        | 36.39        | 4770.23    | 5191.42      | 5201.48      | 36.34           | 5054.38         |
|                            | 13B_q4_0       | 56.68      | 55.96        | 56.61        | 2202.27    | 2048.25      | 2175.12      | 56.42           | 2141.88         |
|                            | 13B_f16        | 19.21      | 19.23        | 19.23        | 3131.34    | 3117.55      | 3130.19      | 19.22           | 3126.36         |
|                            | 30B_q4_0       | 24.45      | 24.46        | 24.46        | 978.34     | 971.96       | 973.47       | 24.46           | 974.59          |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX 4000 Ada 20GB * 2      | 7B_q4_0        | 60.53      | 60.47        | 57.69        | 521.68     | 502.74       | 497.34       | 59.56           | 507.25          |
|                            | 7B_f16         | 31.79      | 31.03        | 31.9         | 499.21     | 489.35       | 504.63       | 31.57           | 497.73          |
|                            | 13B_q4_0       | 39.52      | 39.7         | 39.43        | 325.2      | 323.59       | 325.5        | 39.55           | 324.76          |
|                            | 13B_f16        | 18.37      | 18.34        | 18.34        | 308.01     | 307.61       | 306.49       | 18.35           | 307.37          |
|                            | 30B_q4_0       | 19.85      | 19.86        | 19.83        | 158.89     | 159.31       | 158.77       | 19.85           | 158.99          |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 3090 24GB * 2              | 7B_q4_0        | 53.58      | 53.14        | 53.18        | 252.66     | 252.07       | 251.92       | 53.3            | 252.22          |
|                            | 7B_f16         | 38.91      | 38.73        | 38.8         | 247.9      | 247.42       | 248.03       | 38.81           | 247.78          |
|                            | 13B_q4_0       | 38.66      | 38.76        | 38.51        | 173.66     | 173.38       | 173.26       | 38.64           | 173.43          |
|                            | 13B_f16        | 26.21      | 25.77        | 25.78        | 169.89     | 169.88       | 169.93       | 25.92           | 169.9           |
|                            | 30B_q4_0       | 21.98      | 22.51        | 22.18        | 89.65      | 89.54        | 89.49        | 22.22           | 89.56           |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | 14.39      | 14.44        | 14.41        | 54.41      | 54.38        | 54.45        | 14.41           | 54.41           |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 4090 24GB * 2              | 7B_q4_0        | 64.36      | 64.25        | 64.49        | 1504.06    | 1488.26      | 1504.63      | 64.37           | 1498.98         |
|                            | 7B_f16         | 48.49      | 48.49        | 48.41        | 1530.1     | 1532.7       | 1523.79      | 48.46           | 1528.86         |
|                            | 13B_q4_0       | 47.46      | 47.62        | 47.6         | 936.21     | 951.01       | 953.27       | 47.56           | 946.83          |
|                            | 13B_f16        | 31.73      | 31.72        | 31.75        | 919.07     | 906.36       | 917.53       | 31.73           | 914.32          |
|                            | 30B_q4_0       | 27.38      | 27.33        | 27.32        | 423.95     | 452.05       | 455.48       | 27.34           | 443.83          |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | 17.38      | 17.28        | 17.97        | 269.39     | 271.29       | 266.59       | 17.54           | 269.09          |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX A6000 48GB             | 7B_q4_0        | 111.68     | 111.39       | 111.24       | 2985.87    | 2987.86      | 3002.24      | 111.44          | 2991.99         |
|                            | 7B_f16         | 44.02      | 43.94        | 43.91        | 3743.16    | 3694.23      | 3712.89      | 43.96           | 3716.76         |
|                            | 13B_q4_0       | 67.14      | 67.03        | 66.98        | 1790.1     | 1785.51      | 1777.19      | 67.05           | 1784.27         |
|                            | 13B_f16        | 24.65      | 24.66        | 24.65        | 2375.49    | 2390.33      | 2376.3       | 24.65           | 2380.71         |
|                            | 30B_q4_0       | 29.72      | 29.7         | 29.69        | 797.64     | 794.67       | 793.23       | 29.7            | 795.18          |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | 15.7       | 15.68        | 15.68        | 424.77     | 421.5        | 418.24       | 15.69           | 421.5           |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX 6000 Ada 48GB          | 7B_q4_0        | 127.93     | 128.23       | 128.09       | 4011.08    | 4213.76      | 4329.84      | 128.08          | 4184.89         |
|                            | 7B_f16         | 50.54      | 50.54        | 50.6         | 5278.34    | 5004.36      | 5010.6       | 50.56           | 5097.77         |
|                            | 13B_q4_0       | 74.39      | 73.58        | 74.23        | 2269.22    | 2298.02      | 2345.37      | 74.07           | 2304.2          |
|                            | 13B_f16        | 26.88      | 26.88        | 26.87        | 2982.1     | 2913.13      | 2943.37      | 26.88           | 2946.2          |
|                            | 30B_q4_0       | 32.68      | 32.64        | 32.64        | 931.86     | 940.87       | 943.7        | 32.65           | 938.81          |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | 17.42      | 17.37        | 17.39        | 487.49     | 472.7        | 470.28       | 17.39           | 476.82          |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 3090 24GB * 3              | 7B_q4_0        | 44.68      | 45.84        | 44.94        | 236.87     | 236.61       | 237.62       | 45.15           | 237.03          |
|                            | 7B_f16         | 35.74      | 35.31        | 35.2         | 232.51     | 230.31       | 232.83       | 35.42           | 231.88          |
|                            | 13B_q4_0       | 34.09      | 34.36        | 34.14        | 147.56     | 147.53       | 147.41       | 34.2            | 147.5           |
|                            | 13B_f16        | 25.72      | 25.72        | 25.46        | 145.04     | 145.04       | 144.45       | 25.63           | 144.84          |
|                            | 30B_q4_0       | 20.36      | 20.31        | 20.33        | 74.76      | 74.64        | 74.51        | 20.33           | 74.64           |
|                            | 30B_f16        | 14         | 14.19        | 14.02        | 72.09      | 71.95        | 71.96        | 14.07           | 72              |
|                            | 65B_q4_0       | 13.62      | 13.59        | 13.85        | 47.22      | 47.1         | 47.22        | 13.69           | 47.18           |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 4090 24GB * 3              | 7B_q4_0        | 62.14      | 61.79        | 62.03        | 1008.36    | 694.59       | 1008.21      | 61.99           | 903.72          |
|                            | 7B_f16         | 50.7       | 50.73        | 50.56        | 1002.56    | 961.68       | 1001.81      | 50.66           | 988.68          |
|                            | 13B_q4_0       | 47.05      | 46.99        | 47.18        | 612.06     | 610.24       | 609.81       | 47.07           | 610.7           |
|                            | 13B_f16        | 35         | 35.05        | 35.11        | 596.67     | 597.92       | 594.88       | 35.05           | 596.49          |
|                            | 30B_q4_0       | 28.28      | 28.31        | 28.27        | 316.99     | 317.8        | 310.94       | 28.29           | 315.24          |
|                            | 30B_f16        | 18.87      | 18.86        | 18.86        | 299.08     | 299.93       | 299.45       | 18.86           | 299.49          |
|                            | 65B_q4_0       | 19.08      | 19.07        | 19.06        | 192.37     | 192.98       | 193.45       | 19.07           | 192.93          |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX 4000 Ada 20GB * 4      | 7B_q4_0        | 54.17      | 54.86        | 53.5         | 227.07     | 230.08       | 233.99       | 54.18           | 230.38          |
|                            | 7B_f16         | 40.25      | 40.19        | 40.25        | 236.98     | 239.86       | 243.33       | 40.23           | 240.06          |
|                            | 13B_q4_0       | 41.72      | 41.72        | 41.54        | 145.14     | 142.7        | 147.7        | 41.66           | 145.18          |
|                            | 13B_f16        | 24.37      | 24.3         | 24.3         | 141.78     | 140.48       | 142.59       | 24.32           | 141.62          |
|                            | 30B_q4_0       | 24.41      | 24.39        | 24.2         | 91.87      | 92.32        | 74.44        | 24.33           | 86.21           |
|                            | 30B_f16        | 12.08      | 12.06        | 12.08        | 73.13      | 70.13        | 73           | 12.07           | 72.09           |
|                            | 65B_q4_0       | 15.24      | 15.22        | 15.24        | 47.54      | 45.29        | 46.61        | 15.23           | 46.48           |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| A100 80GB                  | 7B_q4_0        | 136.66     | 136.71       | 136.73       | 3779.5     | 3981.72      | 4109.75      | 136.7           | 3956.99         |
|                            | 7B_f16         | 73.96      | 73.91        | 73.86        | 5099.41    | 5285.4       | 5555.24      | 73.91           | 5313.35         |
|                            | 13B_q4_0       | 92.21      | 92.27        | 92.19        | 2506.69    | 2507.4       | 2474.8       | 92.22           | 2496.3          |
|                            | 13B_f16        | 45         | 45.01        | 45.03        | 3600.85    | 3584.78      | 3691.64      | 45.01           | 3625.76         |
|                            | 30B_q4_0       | 46.83      | 46.82        | 46.82        | 1204.77    | 1219.03      | 1207.09      | 46.82           | 1210.3          |
|                            | 30B_f16        | 20.18      | 20.21        | 20.19        | 1661.54    | 1893.04      | 1897.04      | 20.19           | 1817.21         |
|                            | 65B_q4_0       | 26.61      | 26.62        | 26.59        | 637.52     | 637.87       | 628.66       | 26.61           | 634.68          |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| H100 PCIe 80GB             | 7B_q4_0        | 131.53     | 135.76       | 130.86       | 4919.98    | 4993.08      | 4987.58      | 132.72          | 4966.88         |
|                            | 7B_f16         | 83.16      | 82.09        | 81.95        | 7146.27    | 7148.23      | 7361.38      | 82.4            | 7218.63         |
|                            | 13B_q4_0       | 90.46      | 90.55        | 92.56        | 3238.79    | 3200.25      | 2700.34      | 91.19           | 3046.46         |
|                            | 13B_f16        | 50.88      | 51.07        | 50.79        | 5045.45    | 5114.67      | 5100.83      | 50.91           | 5086.98         |
|                            | 30B_q4_0       | 48.92      | 48.89        | 48.95        | 1578.69    | 1570.08      | 1572.97      | 48.92           | 1573.91         |
|                            | 30B_f16        | 23.15      | 23.06        | 23.1         | 2498.44    | 2503.4       | 2497.84      | 23.1            | 2499.89         |
|                            | 65B_q4_0       | 26.81      | 27.16        | 26.74        | 876.01     | 879.23       | 872.77       | 26.9            | 876             |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX A6000 48GB * 2         | 7B_q4_0        | 72.83      | 72.18        | 72.61        | 1139       | 1112.94      | 1115.61      | 72.54           | 1122.52         |
|                            | 7B_f16         | 47.44      | 47.01        | 47           | 999.03     | 1091.13      | 1085.11      | 47.15           | 1058.42         |
|                            | 13B_q4_0       | 51.18      | 51.07        | 50.84        | 727.93     | 726.87       | 723.13       | 51.03           | 725.98          |
|                            | 13B_f16        | 30.06      | 30.38        | 29.81        | 692.74     | 764.51       | 670.4        | 30.08           | 709.22          |
|                            | 30B_q4_0       | 28.38      | 28.26        | 28.21        | 353.16     | 379          | 343.37       | 28.28           | 358.51          |
|                            | 30B_f16        | 14.45      | 14.43        | 14.4         | 335.53     | 335.07       | 334.34       | 14.43           | 334.98          |
|                            | 65B_q4_0       | 17.51      | 17.22        | 17.26        | 215.63     | 206.24       | 205.88       | 17.33           | 209.25          |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 3090 24GB * 6              | 7B_q4_0        | 28.03      | 28.4         | 28.71        | 1208.49    | 89.73        | 89.94        | 28.38           | 462.72          |
|                            | 7B_f16         | 26.16      | 27.04        | 25.86        | 88.51      | 88.52        | 88.29        | 26.35           | 88.44           |
|                            | 13B_q4_0       | 21.44      | 20.98        | 21.38        | 49.34      | 48.93        | 49.29        | 21.27           | 49.19           |
|                            | 13B_f16        | 20.24      | 20.32        | 20.53        | 47.97      | 48.06        | 47.95        | 20.36           | 47.99           |
|                            | 30B_q4_0       | 14.17      | 14.37        | 14.35        | 24.88      | 24.82        | 24.92        | 14.3            | 24.87           |
|                            | 30B_f16        | 11.99      | 12.13        | 11.94        | 23.92      | 24.48        | 23.84        | 12.02           | 24.08           |
|                            | 65B_q4_0       | 10.37      | 10.29        | 10.33        | 16.11      | 16.11        | 15.58        | 10.33           | 15.93           |
|                            | 65B_f16        | 7.87       | 7.88         | 7.79         | 15.81      | 15.84        | 15.84        | 7.85            | 15.83           |
| 4090 24GB * 6              | 7B_q4_0        | 35.85      | 37.17        | 37.01        | 443.26     | 455.17       | 455.58       | 36.68           | 451.34          |
|                            | 7B_f16         | 30.47      | 37.63        | 37.75        | 463.42     | 446.24       | 431.44       | 35.28           | 447.03          |
|                            | 13B_q4_0       | 30.13      | 30.47        | 30.01        | 289.08     | 285.65       | 288.98       | 30.2            | 287.9           |
|                            | 13B_f16        | 29.24      | 29           | 29.03        | 282.11     | 267.74       | 267.48       | 29.09           | 272.44          |
|                            | 30B_q4_0       | 19.88      | 21.4         | 21.5         | 154.9      | 155.45       | 152.41       | 20.93           | 154.25          |
|                            | 30B_f16        | 18.93      | 18.9         | 18.76        | 150.76     | 136.95       | 137.75       | 18.86           | 141.82          |
|                            | 65B_q4_0       | 15.94      | 15.88        | 15.79        | 95.75      | 95.21        | 95.29        | 15.87           | 95.42           |
|                            | 65B_f16        | 11.31      | 12.47        | 12.6         | 82.26      | 85.95        | 84.87        | 12.13           | 84.36           |

#### Apple Silicon (snapshots for M1 Max in Aug 2023, others in Dec 2023)

| GPU                        | Model          | TG [t/s]   |     |     | PP [t/s]   |     |     | mean TG [t/s]   | mean PP [t/s]   |
|:---------------------------|:---------------|:-----------|:-------------|:-------------|:-----------|:-------------|:-------------|:----------------|:----------------|
| M1 Max 24-Core GPU 32GB    | 7B_q4_0        | 48.88      | 48.9         | 46.65        | 199.33     | 199.19       | 199.43       | 48.14           | 199.32          |
|                            | 7B_f16         | 13.99      | 13.96        | 13.96        | 213.61     | 213.37       | 213.8        | 13.97           | 213.59          |
|                            | 13B_q4_0       | 28.59      | 27.14        | 24.64        | 111.92     | 95.93        | 92.92        | 26.79           | 100.26          |
|                            | 13B_f16 (CPU)  | 4.49       | 4.33         | 4.49         | 26.55      | 28.15        | 18.65        | 4.44            | 24.45           |
|                            | 30B_q4_0       | 13.18      | 13.13        | 13.12        | 49.29      | 49.33        | 49.3         | 13.14           | 49.31           |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| M2 Ultra 76-Core GPU 192GB | 7B_q4_0        | 91.81      | 92.37        | 92.22        | 1208.49    | 1212.66      | 1211.63      | 92.13           | 1210.93         |
|                            | 7B_f16         | 41.18      | 41.36        | 41.41        | 1378.83    | 1377.31      | 1380.34      | 41.32           | 1378.83         |
|                            | 13B_q4_0       | 55.86      | 55.79        | 55.75        | 660.85     | 660.91       | 660.65       | 55.8            | 660.8           |
|                            | 13B_f16        | 22.14      | 22.17        | 22.17        | 749.67     | 748.86       | 749.73       | 22.16           | 749.42          |
|                            | 30B_q4_0       | 26.75      | 26.72        | 26.69        | 273.56     | 272.89       | 272.67       | 26.72           | 273.04          |
|                            | 30B_f16        | 9.87       | 9.81         | 9.83         | 310.12     | 309.66       | 309.94       | 9.84            | 309.91          |
|                            | 65B_q4_0       | 15         | 14.59        | 14.61        | 139.04     | 139.06       | 139.22       | 14.73           | 139.11          |
|                            | 65B_f16        | 5.01       | 5.01         | 5.02         | 158.01     | 157.91       | 157.7        | 5.01            | 157.87          |
| M3 10-Core GPU 16GB        | 7B_q4_0        | 19.48      | 19.54        | 20.03        | 182.59     | 184.21       | 184.26       | 19.68           | 183.69          |
|                            | 7B_f16 (CPU)   | 4.09       | 2.89         | 2.67         | 28         | 29.56        | 30.36        | 3.22            | 29.31           |
|                            | 13B_q4_0       | 11.05      | 11.11        | 11.1         | 97.14      | 97.03        | 96.87        | 11.09           | 97.01           |
|                            | 13B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 30B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| M3 Max 40-Core GPU 48GB    | 7B_q4_0        | 63.68      | 62.95        | 62.56        | 743.8      | 741.39       | 741.28       | 63.06           | 742.16          |
|                            | 7B_f16         | 24.77      | 24.7         | 24.5         | 767.62     | 722.39       | 686.75       | 24.66           | 725.59          |
|                            | 13B_q4_0       | 35.99      | 35.94        | 36.01        | 378.94     | 370.76       | 373.19       | 35.98           | 374.3           |
|                            | 13B_f16        | 13.3       | 13.36        | 13.34        | 360.11     | 376.68       | 365.01       | 13.33           | 367.27          |
|                            | 30B_q4_0       | 16.2       | 16.24        | 16.27        | 152.22     | 153.36       | 154.6        | 16.24           | 153.39          |
|                            | 30B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 65B_q4_0       | 3.17       | 3.13         | 3.12         | 10.08      | 9.73         | 9.91         | 3.14            | 9.91            |
|                            | 65B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |

### LLaMA 2 🦙🦙:

#### NVIDIA GPUs (CPU: AMD EPYC, OS: Ubuntu 22.04.2 LTS, pytorch:2.1.1, py: 3.10, cuda: 12.1.1 or 11.8.0 on RunPod) (snapshots in Dec 2023)

| GPU                        | Model          | TG [t/s]   |     |     | PP [t/s]   |     |     | mean TG [t/s]   | mean PP [t/s]   |
|:---------------------------|:---------------|:-----------|:-------------|:-------------|:-----------|:-------------|:-------------|:----------------|:----------------|
| 4080 16GB                  | 7B_q4_0        | 118.05     | 117.98       | 118.01       | 3763.15    | 3777.83      | 3783.38      | 118.01          | 3774.79         |
|                            | 7B_f16         | 44.21      | 44.21        | 44.19        | 5153.89    | 5125.39      | 5195.3       | 44.2            | 5158.19         |
|                            | 13B_q4_0       | 68.1       | 68.13        | 67.97        | 2240.63    | 2253         | 2271.43      | 68.07           | 2255.02         |
|                            | 13B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX 4000 Ada 20GB          | 7B_q4_0        | 64.07      | 64.64        | 64.89        | 1788.29    | 1836.82      | 1834.47      | 64.53           | 1819.86         |
|                            | 7B_f16         | 22.89      | 23           | 22.99        | 2285.26    | 2409.71      | 2412.29      | 22.96           | 2369.09         |
|                            | 13B_q4_0       | 36.58      | 36.32        | 36.59        | 1064.43    | 1047.79      | 1053.87      | 36.5            | 1055.36         |
|                            | 13B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 3090 24GB                  | 7B_q4_0        | 120.48     | 120.7        | 120.62       | 2605.87    | 2612.02      | 2587.55      | 120.6           | 2601.81         |
|                            | 7B_f16         | 51.87      | 51.82        | 51.85        | 3090.47    | 3096.25      | 3039.38      | 51.85           | 3075.37         |
|                            | 13B_q4_0       | 75.11      | 74.62        | 74.64        | 1652.83    | 1638.8       | 1667.26      | 74.79           | 1652.96         |
|                            | 13B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 4090 24GB                  | 7B_q4_0        | 150.09     | 148.67       | 149.35       | 5560.5     | 5528.08      | 5504.98      | 149.37          | 5531.19         |
|                            | 7B_f16         | 60.75      | 61           | 60.6         | 7331.23    | 7338.41      | 7376.44      | 60.78           | 7348.69         |
|                            | 13B_q4_0       | 88.41      | 88.51        | 88.75        | 3163.32    | 3162.81      | 3156.39      | 88.56           | 3160.84         |
|                            | 13B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX 5000 Ada 32GB          | 7B_q4_0        | 99.07      | 99.23        | 99.27        | 3525.95    | 3702.68      | 3348.47      | 99.19           | 3525.7          |
|                            | 7B_f16         | 36.41      | 36.22        | 36.29        | 5021.4     | 4915.77      | 4872.61      | 36.31           | 4936.59         |
|                            | 13B_q4_0       | 56.06      | 56.25        | 56.38        | 2158.19    | 2025.15      | 2172.96      | 56.23           | 2118.77         |
|                            | 13B_f16        | 19.21      | 19.23        | 19.22        | 2715.48    | 3212.32      | 3187.01      | 19.22           | 3038.27         |
|                            | 70B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX 4000 Ada 20GB * 2      | 7B_q4_0        | 60.83      | 60.24        | 60.98        | 510.63     | 527.62       | 528.69       | 60.68           | 522.31          |
|                            | 7B_f16         | 31.88      | 31.75        | 31.87        | 503.6      | 488.38       | 506.83       | 31.83           | 499.6           |
|                            | 13B_q4_0       | 39.72      | 39.6         | 39.64        | 324.77     | 324.84       | 324.78       | 39.65           | 324.8           |
|                            | 13B_f16        | 18.37      | 18.33        | 18.36        | 308.54     | 303.42       | 306.68       | 18.35           | 306.21          |
|                            | 70B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 3090 24GB * 2              | 7B_q4_0        | 53.44      | 52.98        | 53.34        | 251.78     | 252.35       | 252.33       | 53.25           | 252.15          |
|                            | 7B_f16         | 38.68      | 38.72        | 38.8         | 247.18     | 247.32       | 247.42       | 38.73           | 247.31          |
|                            | 13B_q4_0       | 38.6       | 38.65        | 38.25        | 173.08     | 172.65       | 173.01       | 38.5            | 172.91          |
|                            | 13B_f16        | 25.72      | 25.74        | 25.8         | 169.65     | 169.76       | 170.14       | 25.75           | 169.85          |
|                            | 70B_q4_0       | 14.27      | 14.2         | 14.36        | 48.37      | 48.34        | 48.68        | 14.28           | 48.46           |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 4090 24GB * 2              | 7B_q4_0        | 66.37      | 66.21        | 66.21        | 1720.22    | 1696.08      | 1716.8       | 66.26           | 1711.03         |
|                            | 7B_f16         | 49.25      | 49.04        | 49.31        | 1730.6     | 1725.1       | 1732.34      | 49.2            | 1729.35         |
|                            | 13B_q4_0       | 48.8       | 48.8         | 48.86        | 1015.44    | 660.63       | 1072.3       | 48.82           | 916.12          |
|                            | 13B_f16        | 32.4       | 32.43        | 32.43        | 1035.41    | 1037.49      | 1035.18      | 32.42           | 1036.03         |
|                            | 70B_q4_0       | 18.49      | 18.55        | 18.46        | 254.72     | 258.2        | 258          | 18.5            | 256.97          |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX A6000 48GB             | 7B_q4_0        | 111.52     | 111.67       | 111.52       | 2970.46    | 2979.12      | 2987.78      | 111.57          | 2979.12         |
|                            | 7B_f16         | 44.06      | 44.02        | 43.96        | 3729.26    | 3704.12      | 3690.26      | 44.01           | 3707.88         |
|                            | 13B_q4_0       | 67.1       | 67.14        | 67.08        | 1787.32    | 1799.81      | 1793.19      | 67.11           | 1793.44         |
|                            | 13B_f16        | 24.68      | 24.68        | 24.66        | 2393.86    | 2411.16      | 2386.01      | 24.67           | 2397.01         |
|                            | 70B_q4_0       | 15.18      | 15.18        | 15.17        | 404.13     | 408.22       | 405.55       | 15.18           | 405.97          |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX 6000 Ada 48GB          | 7B_q4_0        | 125.92     | 128.18       | 128.24       | 3689.46    | 4021.62      | 4022.47      | 127.45          | 3911.18         |
|                            | 7B_f16         | 50.58      | 50.58        | 50.53        | 5443.89    | 5494.02      | 5272.88      | 50.56           | 5403.6          |
|                            | 13B_q4_0       | 74.29      | 74.32        | 74.42        | 2317.18    | 2227.36      | 2251.14      | 74.34           | 2265.23         |
|                            | 13B_f16        | 26.92      | 26.91        | 26.89        | 3063.26    | 3028.63      | 3029.11      | 26.91           | 3040.33         |
|                            | 70B_q4_0       | 17.03      | 17.01        | 17.02        | 506.74     | 474.26       | 461.78       | 17.02           | 480.93          |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 3090 24GB * 3              | 7B_q4_0        | 46.11      | 46.06        | 46.25        | 236.75     | 235.82       | 236.91       | 46.14           | 236.49          |
|                            | 7B_f16         | 35.84      | 35.83        | 35.83        | 232.06     | 230.81       | 231.78       | 35.83           | 231.55          |
|                            | 13B_q4_0       | 34.45      | 34.28        | 33.67        | 147.44     | 147.06       | 147.45       | 34.13           | 147.32          |
|                            | 13B_f16        | 25.15      | 25.2         | 25.21        | 145.01     | 144.67       | 144.73       | 25.19           | 144.8           |
|                            | 70B_q4_0       | 13.52      | 13.76        | 13.65        | 41.26      | 41.32        | 41.24        | 13.64           | 41.27           |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 4090 24GB * 3              | 7B_q4_0        | 62.07      | 62.2         | 62.15        | 1011.64    | 970.65       | 1006.45      | 62.14           | 996.25          |
|                            | 7B_f16         | 50.04      | 50.59        | 50.55        | 1007.3     | 1004.61      | 1006.65      | 50.39           | 1006.19         |
|                            | 13B_q4_0       | 47.02      | 32.64        | 47.27        | 433.19     | 594.15       | 473.47       | 42.31           | 500.27          |
|                            | 13B_f16        | 34.75      | 35.03        | 35.16        | 594.69     | 588.52       | 596.4        | 34.98           | 593.2           |
|                            | 70B_q4_0       | 14.9       | 15.02        | 14.83        | 180.08     | 177.07       | 177.92       | 14.92           | 178.36          |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX 4000 Ada 20GB * 4      | 7B_q4_0        | 55.3       | 54.83        | 56.1         | 279.26     | 280.56       | 281.66       | 55.41           | 280.49          |
|                            | 7B_f16         | 40.55      | 40.49        | 40.52        | 293.28     | 293.5        | 293.08       | 40.52           | 293.29          |
|                            | 13B_q4_0       | 40.36      | 41.7         | 41.82        | 146.25     | 147.51       | 147.36       | 41.29           | 147.04          |
|                            | 13B_f16        | 24.41      | 24.44        | 24.28        | 177.98     | 143.95       | 147.04       | 24.38           | 156.32          |
|                            | 70B_q4_0       | 15.46      | 15.48        | 15.49        | 41.81      | 47.4         | 51.07        | 15.48           | 46.76           |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| A100 80GB                  | 7B_q4_0        | 136.09     | 136.17       | 136.47       | 3224.44    | 3613.25      | 3494.25      | 136.24          | 3443.98         |
|                            | 7B_f16         | 73.86      | 73.79        | 73.84        | 4892.15    | 5166.66      | 5258.51      | 73.83           | 5105.77         |
|                            | 13B_q4_0       | 92.13      | 92.1         | 92.12        | 2343.26    | 2335.05      | 2335.44      | 92.12           | 2337.92         |
|                            | 13B_f16        | 44.97      | 45.02        | 45.03        | 3662.05    | 3672.21      | 3757.6       | 45.01           | 3697.29         |
|                            | 70B_q4_0       | 25.72      | 25.67        | 25.65        | 618.99     | 621.06       | 597.18       | 25.68           | 612.41          |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| H100 PCIe 80GB             | 7B_q4_0        | 131.44     | 134.81       | 135.07       | 4773.18    | 4975.78      | 4856.8       | 133.77          | 4868.59         |
|                            | 7B_f16         | 83.19      | 83.36        | 83.4         | 7464.15    | 7287.64      | 7349.22      | 83.32           | 7367            |
|                            | 13B_q4_0       | 90.42      | 89.93        | 90.44        | 3040.04    | 2984.15      | 3012.51      | 90.26           | 3012.23         |
|                            | 13B_f16        | 50.86      | 51.33        | 50.78        | 5000.66    | 5052.73      | 5090.47      | 50.99           | 5047.95         |
|                            | 70B_q4_0       | 26.08      | 25.8         | 25.76        | 863.08     | 855.95       | 848.17       | 25.88           | 855.73          |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| RTX A6000 48GB * 2         | 7B_q4_0        | 74.87      | 74.06        | 74.37        | 1321.94    | 1309.73      | 1297.81      | 74.43           | 1309.83         |
|                            | 7B_f16         | 47.8       | 46.28        | 47.15        | 1275.84    | 1073.5       | 1086.1       | 47.08           | 1145.15         |
|                            | 13B_q4_0       | 52.08      | 51           | 51.71        | 806.56     | 810.14       | 822.28       | 51.6            | 812.99          |
|                            | 13B_f16        | 30.39      | 30.16        | 30.3         | 777.89     | 770.82       | 773.55       | 30.28           | 774.09          |
|                            | 70B_q4_0       | 17.32      | 17.3         | 17.32        | 198.61     | 198.21       | 197.89       | 17.31           | 198.24          |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| 3090 24GB * 6              | 7B_q4_0        | 26.61      | 28.23        | 28.28        | 89.71      | 90.15        | 89.36        | 27.71           | 89.74           |
|                            | 7B_f16         | 27.21      | 27.43        | 27.46        | 87.8       | 87.61        | 87.45        | 27.37           | 87.62           |
|                            | 13B_q4_0       | 22.61      | 22.36        | 21.99        | 49.56      | 49.57        | 49.81        | 22.32           | 49.65           |
|                            | 13B_f16        | 20.3       | 20.23        | 20.15        | 48.36      | 48.4         | 48.31        | 20.23           | 48.36           |
|                            | 70B_q4_0       | 10.18      | 10.12        | 10.15        | 14.78      | 14.81        | 14.8         | 10.15           | 14.8            |
|                            | 70B_f16        | 7.57       | 7.59         | 7.47         | 14.78      | 14.81        | 14.8         | 7.54            | 14.8            |
| 4090 24GB * 6              | 7B_q4_0        | 38.34      | 38           | 38.24        | 431.76     | 440.64       | 440.91       | 38.19           | 437.77          |
|                            | 7B_f16         | 37.5       | 37.58        | 37.43        | 425.93     | 424.68       | 426.99       | 37.5            | 425.87          |
|                            | 13B_q4_0       | 30.34      | 30.24        | 29.97        | 279.47     | 290.72       | 283.99       | 30.18           | 284.73          |
|                            | 13B_f16        | 29.43      | 29.26        | 29.09        | 275.42     | 275.61       | 265.37       | 29.26           | 272.13          |
|                            | 70B_q4_0       | 15.59      | 15.62        | 15.57        | 88.16      | 88.64        | 88.62        | 15.59           | 88.47           |
|                            | 70B_f16        | 9.75       | 8.63         | 12.34        | 86.7       | 87.36        | 81.25        | 10.24           | 85.1            |

#### Apple Silicon (snapshots for M1 Max in Aug 2023, others in Dec 2023)

| GPU                        | Model          | TG [t/s]   |     |     | PP [t/s]   |     |     | mean TG [t/s]   | mean PP [t/s]   |
|:---------------------------|:---------------|:-----------|:-------------|:-------------|:-----------|:-------------|:-------------|:----------------|:----------------|
| M1 Max 24-Core GPU 32GB    | 7B_q4_0        | 48.81      | 48.43        | 48.71        | 199.6      | 199.66       | 199.7        | 48.65           | 199.65          |
|                            | 7B_f16         | 13.98      | 13.77        | 13.73        | 212.96     | 213.6        | 212.85       | 13.83           | 213.14          |
|                            | 13B_q4_0       | 26.1       | 26.77        | 26.31        | 111.73     | 100.97       | 91.17        | 26.39           | 101.29          |
|                            | 13B_f16 (CPU)  | 4.4        | 4.43         | 4.37         | 27.57      | 26.38        | 24.93        | 4.4             | 26.29           |
|                            | 70B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| M2 Ultra 76-Core GPU 192GB | 7B_q4_0        | 92         | 91.96        | 91.72        | 1213.54    | 1219.65      | 1217.91      | 91.89           | 1217.03         |
|                            | 7B_f16         | 40.7       | 40.66        | 40.69        | 1379.46    | 1379.77      | 1379.39      | 40.68           | 1379.54         |
|                            | 13B_q4_0       | 55.3       | 55.41        | 55.32        | 660.36     | 659.66       | 659.47       | 55.34           | 659.83          |
|                            | 13B_f16        | 22.14      | 22.14        | 22.16        | 749.52     | 748.59       | 749.4        | 22.15           | 749.17          |
|                            | 70B_q4_0       | 14.37      | 14.37        | 14.37        | 133.27     | 133.24       | 133.03       | 14.37           | 133.18          |
|                            | 70B_f16        | 4.81       | 4.81         | 4.81         | 150.66     | 151.22       | 151.05       | 4.81            | 150.98          |
| M3 10-Core GPU 16GB        | 7B_q4_0        | 20.86      | 20.74        | 20.77        | 184.59     | 184.49       | 184.45       | 20.79           | 184.51          |
|                            | 7B_f16 (CPU)   | 4.03       | 4.08         | 2.73         | 31.49      | 31.62        | 12.51        | 3.61            | 25.21           |
|                            | 13B_q4_0       | 11.26      | 10.96        | 11.32        | 97.08      | 96.97        | 97.14        | 11.18           | 97.06           |
|                            | 13B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_q4_0       | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |
| M3 Max 40-Core GPU 48GB    | 7B_q4_0        | 63.51      | 63.39        | 62.42        | 741.79     | 740.78       | 740.35       | 63.11           | 740.97          |
|                            | 7B_f16         | 24.77      | 24.77        | 24.73        | 767.87     | 766.13       | 750.19       | 24.76           | 761.4           |
|                            | 13B_q4_0       | 36.55      | 36.32        | 36.61        | 345.06     | 351.87       | 387.76       | 36.49           | 361.56          |
|                            | 13B_f16        | 13.28      | 13.35        | 13.38        | 379.93     | 406.5        | 409.13       | 13.34           | 398.52          |
|                            | 70B_q4_0 (CPU) | 3.11       | 3.11         | 3.11         | 9.27       | 8.42         | 8.35         | 3.11            | 8.68            |
|                            | 70B_f16        | OOM        | OOM          | OOM          | OOM        | OOM          | OOM          | OOM             | OOM             |

## Conclusion

Same performance on LLaMA and LLaMA 2 of the same size and quantization. Multiple NVIDIA GPUs might affect the performance.

For LLM inference, buy 3090s to save money. Buy 4090s if you want to speed up. Buy A100s if you are rich. Buy Mac Studio if you want to put your computer on your desk, save energy, be quiet, and don't wanna maintenance. (If you want to **train** LLM, choose NIVIDA.)

If you find this information helpful, please give me a star. ⭐️ Feel free to contact me if you have any advice. Thank you. 🤗

## Appendix

All the latest results are welcome! 🥰 (Thanks to: MichaelDays ❤️)

### Mac

#### LLaMA 2 🦙🦙:

| GPU | Model | eval time (ms/token)|     |     | prompt eval time (ms/token) |     |     | mean eval time (ms/token) | mean prompt eval time (ms/token) |
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

