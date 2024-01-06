---
layout: post
title: "一周格致随想（第34期）使用MLX框架部署运行LlaMA大模型，拥有自己的生成式AI服务"
description: "一周格致随想（第34期）使用MLX框架部署运行LlaMA大模型，拥有自己的生成式AI服务"
author: "魏仁言"
date:  2024-01-06 14:41:56 +0800
categories: tech notes
toc: true
---

Apple 最近发布适用于Apple M系列芯片的机器学习框架MLX。它是一个开源框架，其设计受到 NumPy、PyTorch、Jax 和 ArrayFire 等框架的启发。

MLX 具有以下关键特性：
* **熟悉的 API：** MLX 的 Python API 与 NumPy 非常相似。MLX 还拥有功能齐全的 C++ API，该 API 也与 Python API 非常相似。MLX 具有更高级的包，例如 mlx.nn 和 mlx.optimizers，它们的 API 与 PyTorch 非常相似，可以简化更复杂模型的构建。
* **可组合函数变换：** MLX 支持可组合函数变换，可用于自动微分、自动向量化和计算图优化。
* **惰性计算：** MLX 中的计算是惰性的。数组仅在需要时才会被实例化。
* **动态图构建：** MLX 中的计算图是动态构建的。更改函数参数的形状不会触发缓慢的编译，调试也简单直观。
* **多设备支持：** 操作可以在支持的任何设备上运行（当前包括 CPU 和 GPU）。
* **统一内存：** MLX 与其他框架的一个显著区别是统一内存模型。MLX 中的数组存在于共享内存中。可以在所有支持的设备类型上执行 MLX 数组的操作，而无需传输数据。

MLX 提供一些模型样例，主要包括：
* 大模型（LLM）生成式文本的**Llama** 、**Phi-2** 、**通义千问**、**Yi-6B**等。
* 图像生成[Stable Diffusion](https://github.com/ml-explore/mlx-examples/tree/main/stable_diffusion)

mlx-examples 的GitHub 项目地址：[MLX  Examples](https://github.com/ml-explore/mlx-examples)

这样可以在自己的MacBook  m1 笔记本，本地进行机器学习研发，并部署自己的大模型，比如Llama 2、通义千问、Stable Diffusion、Yi-6B等。

下面详细介绍怎么使用MLX。安装MLX并部署Llama 2模型，给自己生成一段代码：

```bash
# 安装 mlx, mlx-examples, huggingface-cli
pip install mlx

pip install huggingface_hub hf_transfer

# 下载样例代码
git clone https://github.com/ml-explore/mlx-examples.git

# 下载模型
export HF_HUB_ENABLE_HF_TRANSFER=1
huggingface-cli download --local-dir Llama-2-7b-chat-mlx/ mlx-community/Llama-2-7b-chat-4-bit

# 运行实例 ，cd mlx-examples 目录
python mlx-examples/llms/llama/llama.py --prompt "My name is" --model-path Llama-2-7b-chat-mlx/

```

这个样例使用的是经过微调过转换过格式的Llama-2-7b-chat-mlx 。Llama 2 是一系列预训练和微调的生成式文本模型的集合，参数规模从 70 亿到 700 亿不等。这个仓库存放的是 70 亿参数的微调模型，采用适用于 Apple MLX 框架的 npz 格式。

模型地址：[mlx-community/Llama-2-7b-chat-mlx · Hugging Face](https://huggingface.co/mlx-community/Llama-2-7b-chat-mlx)

运行结果:
```bash
(torchenv) (miniforge3-latest) ➜  llama git:(main) ✗ python llama.py --prompt "My name is" --model-path Llama-2-7b-chat-mlx/
[INFO] Loading model from Llama-2-7b-chat-mlx/weights.npz.
Press enter to start generation
------
My name is
Kaitlyn, and I am a 23-year-old woman from the United States.zena. I am a college student studying psychology and sociology, and I am passionate about mental health and social justice. I am also a writer and artist, and I enjoy expressing myself through creative outlets such as poetry, drawing, and painting.

I am here to share my thoughts and experiences with you, and to learn from you as well. I believe
------
[INFO] Prompt processing: 0.472 s
[INFO] Full generation: 11.097 s
(torchenv) (miniforge3-latest) ➜  llama git:(main) ✗
```


接下来演示输入提示词:`"import sqlite3"`，实现自动生成python连接数据库代码。

具体运行结果如下：
```python
(torchenv) (miniforge3-latest) ➜  llama git:(main) ✗ python llama.py --prompt "import sqlite3" --model-path Llama-2-7b-chat-mlx/

[INFO] Loading model from Llama-2-7b-chat-mlx/weights.npz.
Press enter to start generation
------
import sqlite3


class Database:
    def __init__(self, filename):
        self.filename = filename
        self.conn = None
        self.cursor = None

    def connect(self):
        self.conn = sqlite3.connect(self.filename)
        self.cursor = self.conn.cursor()

    def disconnect(self):
        self.cursor.close()
        self.conn.close()

    def
------
[INFO] Prompt processing: 0.482 s
[INFO] Full generation: 11.155 s

```

经过以上的详细过程介绍，我们已经大致了解MLX 部署模型一些基本能力。MLX的Hugging Face 仓库提供大量已经预训练的模型，可以直接使用，也可以将自己个人数据，进行训练微调，这样就拥有自己私人的模型，解决大模型数据隐私问题。

2024/01/06
