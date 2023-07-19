# GPU-Benchmarks-on-LLM-Inference
Apple Silicon or NVIDIA GPUs for Large Language Model Inference?
## Description
Use [llama.cpp](https://github.com/ggerganov/llama.cpp) to test the [LLaMA](https://arxiv.org/abs/2302.13971) models inference speed of different GPUs on [RunPod](https://www.runpod.io/), M1 Max MacBook pro and M2 Ultra Mac studio.
## Model
Thanks to shawwn for LLaMA model weights (7B, 13B, 30B, 65B): [llama-dl](https://github.com/shawwn/llama-dl)
## Usage
- For NVIDIA GPUs:

    This provides BLAS acceleration using the CUDA cores of your Nvidia GPU. Multiple GPU works fine with no CPU bottleneck. `-ngl 10000` to make sure all layers are offloaded to GPU. (Thanks to: https://github.com/ggerganov/llama.cpp/pull/1827)
    ```bash
    make clean && LLAMA_CUBLAS=1 make -j
    ```
    Test arguments:
    ```bash
    ./main --color  -ngl 10000 -t 1 --temp 0.7 --repeat_penalty 1.1 -n 512 --ignore-eos -m ./models/7B/ggml-model-q4_0.bin  -p "I believe the meaning of life is"
    ```
- For Apple Silicon:

    Test arguments:
    ```bash
    ./main --color -n 128 -c 512 --no-mmap -ngl 1 --temp 0.7 --repeat_penalty 1.1 -n 512 --ignore-eos -m ./models/13B/ggml-model-q4_0.bin  -p "I believe the meaning of life is"
    ```
    It seems we need to allocate equal memory on the CPU when we run the model on GPU. This means there is only half of the unified memory is unable to be allocated for the GPU. However, the good news is the inference speed seems usable. Use `-ngl 0` or delete it to only use the CPU for inference. (Thanks to: https://github.com/ggerganov/llama.cpp/pull/1826)
    ```bash
    ./main --color -n 128 -c 512 --no-mmap --temp 0.7 --repeat_penalty 1.1 -n 512 --ignore-eos -m ./models/65B/ggml-model-f16.bin  -p "I believe the meaning of life is"
    ```
## Benchmarks
Test as many f16 and q4_0 quantization models as possible. Run three time for each model. No result if OOM (Out of Memory).
### NVIDIA GPUs (CPU: AMD EPYC, OS: Ubuntu 22.04.2 LTS, pytorch:2.0.1, py3.10, cuda11.8.0 on RunPod)
| GPU                       | Model (Q4_0 or f16)    | eval time (ms/token) |              |              |
|:--------------------------|:-----------------------|:---------------------|:-------------|:-------------|
| A4500 20GB                | 7B                     | 11.29                | 11.34        | 11.36        |
|                           | 13B                    | 19.65                | 19.67        | 19.7         |
|                           | 30B                    | OOM                  | OOM          | OOM          |
|                           | 7B_f16                 | 26.94                | 26.94        | 26.99        |
|                           | 13B_f16                | OOM                  | OOM          | OOM          |
| 3090 24GB                 | 7B                     | 8.83                 | 8.81         | 8.84         |
|                           | 13B                    | 14.41                | 14.42        | 14.41        |
|                           | 30B                    | 33.67                | 33.75        | 33.8         |
|                           | 65B                    | OOM                  | OOM          | OOM          |
|                           | 7B_f16                 | 19.54                | 19.55        | 19.56        |
|                           | 13B_f16                | OOM                  | OOM          | OOM          |
| 4090 24GB                 | 7B                     | 7.23                 | 7.23         | 7.26         |
|                           | 13B                    | 12.06                | 12.06        | 12.07        |
|                           | 30B                    | 27.4                 | 27.44        | 27.46        |
|                           | 65B                    | OOM                  | OOM          | OOM          |
|                           | 7B_f16                 | 16.84                | 16.82        | 16.83        |
|                           | 13B_f16                | OOM                  | OOM          | OOM          |
| A4500 20GB * 2            | 7B                     | 19.5                 | 19.63        | 19.51        |
|                           | 13B                    | 27.75                | 27.55        | 27.6         |
|                           | 30B                    | 50.95                | 50.73        | 51.08        |
|                           | 65B                    | OOM                  | OOM          | OOM          |
|                           | 7B_f16                 | 28.06                | 28.02        | 28.16        |
|                           | 13B_f16                | 43.15                | 43.18        | 43.11        |
|                           | 30B_f16                | OOM                  | OOM          | OOM          |
| A6000 48GB                | 7B                     | 9.62                 | 9.6          | 9.61         |
|                           | 13B                    | 15.96                | 16           | 16           |
|                           | 30B                    | 36.79                | 36.86        | 36.91        |
|                           | 65B                    | 70.37                | 70.47        | 70.59        |
|                           | 7B_f16                 | 23.23                | 23.23        | 23.24        |
|                           | 13B_f16                | 41.38                | 41.4         | 41.4         |
|                           | 30B_f16                | OOM                  | OOM          | OOM          |
| A6000ada 48GB (64 GB RAM) | 7B                     | 47.78                | 51.21        | 54.14        |
|                           | 13B                    | 116.38               | 125.55       | 127.52       |
|                           | 30B                    | 341.13               | 344.18       | 340.12       |
|                           | 65B                    | 723.65               | 743.35       | 741.19       |
|                           | 7B_f16                 | 164.97               | 168.95       | 164.95       |
|                           | 13B_f16                | 303.48               | 303.54       | 303.5        |
|                           | 30B_f16                | OOM                  | OOM          | OOM          |
| 3090 24GB * 2             | 7B                     | 17.99                | 17.93        | 17.92        |
|                           | 13B                    | 24.2                 | 24.23        | 24.28        |
|                           | 30B                    | 42.99                | 42.92        | 42.88        |
|                           | 65B                    | 66.82                | 66.84        | 67.16        |
|                           | 7B_f16                 | 22.7                 | 22.73        | 22.64        |
|                           | 13B_f16                | 33.93                | 33.83        | 33.97        |
|                           | 30B_f16                | OOM                  | OOM          | OOM          |
| 4090 24GB * 2             | 7B                     | 16.25                | 16.25        | 16.21        |
|                           | 13B                    | 22.23                | 22.2         | 22.43        |
|                           | 30B                    | 40.27                | 40.48        | 40.21        |
|                           | 65B                    | 62.71                | 62.56        | 62.35        |
|                           | 7B_f16                 | 21.58                | 21.62        | 21.64        |
|                           | 13B_f16                | 32.67                | 32.64        | 32.64        |
|                           | 30B_f16                | OOM                  | OOM          | OOM          |
|                           | 65B_f16                | OOM                  | OOM          | OOM          |
| 3090 24GB * 3             | 7B                     | 21.52                | 21.48        | 21.54        |
|                           | 13B                    | 28.42                | 28.42        | 28.45        |
|                           | 30B                    | 48.12                | 47.95        | 48.1         |
|                           | 65B                    | 72.68                | 72.59        | 72.84        |
|                           | 7B_f16                 | 25.64                | 25.57        | 25.66        |
|                           | 13B_f16                | 35.67                | 35.79        | 35.77        |
|                           | 30B_f16                | 63.79                | 63.73        | 63.88        |
|                           | 65B_f16                | OOM                  | OOM          | OOM          |
| 4090 24GB * 3             | 7B                     | 17.07                | 17.24        | 17.16        |
|                           | 13B                    | 22.95                | 22.93        | 22.73        |
|                           | 30B                    | 39.67                | 39.86        | 39.77        |
|                           | 65B                    | 59.65                | 59.78        | 59.7         |
|                           | 7B_f16                 | 21.05                | 21.07        | 21.04        |
|                           | 13B_f16                | 30.28                | 30.36        | 30.22        |
|                           | 30B_f16                | 57.06                | 57.06        | 57.15        |
|                           | 65B_f16                | OOM                  | OOM          | OOM          |
| A100 80GB                 | 7B                     | 11.61                | 11.54        | 11.49        |
|                           | 13B                    | 17.35                | 17.42        | 17.34        |
|                           | 30B                    | 35.29                | 35.16        | 35.26        |
|                           | 65B                    | 68.13                | 68.08        | 68.07        |
|                           | 7B_f16                 | 14.26                | 14.28        | 14.28        |
|                           | 13B_f16                | 23.58                | 23.49        | 23.49        |
|                           | 30B_f16                | 52.09                | 52.3         | 52.36        |
|                           | 65B_f16                | OOM                  | OOM          | OOM          |
| A6000 48GB * 2            | 7B                     | 21.67                | 21.45        | 21.35        |
|                           | 13B                    | 29.47                | 30.75        | 29.22        |
|                           | 30B                    | 55.22                | 56.35        | 56.44        |
|                           | 65B                    | 86.42                | 84.99        | 89.09        |
|                           | 7B_f16                 | 28.42                | 28.67c       | 27.55        |
|                           | 13B_f16                | 43.43                | 43.42        | 43.74        |
|                           | 30B_f16                | 84.32                | 87.28        | 87.41        |
|                           | 65B_f16                | OOM                  | OOM          | OOM          |
| 3090 24GB * 6             | 7B                     | 33.56                | 33.4         | 33.42        |
|                           | 13B                    | 42.75                | 43.02        | 42.9         |
|                           | 30B                    | 67.82                | 67.4         | 67.35        |
|                           | 65B                    | 96.15                | 96.1         | 95.85        |
|                           | 7B_f16                 | 35.12                | 35.05        | 35.06        |
|                           | 13B_f16                | 45.78                | 46.03        | 45.77        |
|                           | 30B_f16                | 75.76                | 75.84        | 75.98        |
|                           | 65B_f16                | 116.09               | 116.03       | 115.9        |

### Apple Silicon
| GPU                       | Model (Q4_0 or f16)    | eval time (ms/token) |              |              |
|:--------------------------|:-----------------------|:---------------------|:-------------|:-------------|
| M1 Max 24-Core GPU 32GB   | 7B                     | 31.99                | 32.17        | 32.14        |
|                           | 13B                    | 57.09                | 57.02        | 57.1         |
|                           | 30B (CPU)              | 180.16               | 187.48       | 181.44       |
|                           | 7B_f16                 | 67.75                | 67.33        | 67.25        |
|                           | 13B_f16 (CPU)          | 232.57               | 229.49       | 228.25       |
|                           | 30B_f16                | OOM                  | OOM          | OOM          |
| M2 Ultra 76-Core GPU 192GB| 7B                     | 14.48                | 14.55        | 14.43        |
|                           | 13B                    | 23.36                | 24.16        | 24.36        |
|                           | 30B                    | 51.9                 | 51.26        | 51.5         |
|                           | 65B                    | 94.63                | 93.29        | 91.21        |
|                           | 7B_f16                 | 38.34                | 38.27        | 39.02        |
|                           | 13B_f16                | 72.3                 | 72.7         | 73.16        |
|                           | 30B_f16                | 153.29               | 152.91       | 157.75       |
|                           | 65B_f16 (CPU)          | 574.8                | 588.38       | 584.53       |


