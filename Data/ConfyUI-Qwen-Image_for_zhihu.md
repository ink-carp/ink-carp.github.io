# Qwen-Image模型部署教程

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754629612919.png" width="80%" alt="Qwen-Image生成示例">
  <p><em>Qwen-Image模型生成效果展示</em></p>
</div>

---

## 📋 目录

- [部署环境](#-部署环境)
- [硬件配置参考](#-硬件配置参考)
- [部署教程](#-部署教程)
  - [本地环境部署](#本地环境部署)
  - [容器部署](#容器部署)
- [运行效果](#-运行效果)
- [总结](#-总结)

---

## 🛠 部署环境

|      组件      |       规格       |

| :-------------: | :--------------: |

|  **操作系统**  | Ubuntu-24.04 LTS |

| **Python版本** |      3.12.3      |

|  **显卡型号**  |    Nvidia H20    |

| **ComfyUI版本** |      0.3.49      |

| **Pytorch版本** |   2.8.0+cu128   |


---

## ⚙ 硬件配置参考

|     资源     |                规格                |

| :----------: | :--------------------------------: |

|   **CPU**   |                4核                |

|   **RAM**   |                16GB                |

| **显存容量** |           24GB（虚拟化）           |

| **显卡算力** |           FP8 296 TFLOPS           |

| **生成速度** | 20步采样/1328×1328像素 ≈ 50秒/张 |


---

## 📥 部署教程

本教程提供两种部署方式：**本地环境部署**和**容器部署**，您可根据自身需求选择合适的方案。

### 本地环境部署

<details open>
<summary><b>📌 详细步骤（点击展开/折叠）</b></summary>

#### 1. 更新系统软件包

```bash
sudo apt update && sudo apt upgrade -y
```

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754625529657.png" width="80%" alt="系统更新">
  <p><em>系统更新过程</em></p>
</div>

#### 2. 安装Git（已安装可跳过）

```bash
sudo apt install git
```

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754625565956.png" width="80%" alt="安装Git">
  <p><em>Git安装过程</em></p>
</div>

#### 3. 安装Python 3.12（已安装可跳过）

```bash
sudo apt install python3 python3-pip
```

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754625692832.png" width="80%" alt="安装Python">
  <p><em>Python安装过程</em></p>
</div>

#### 4. 克隆ComfyUI官方仓库

```bash
git clone https://github.com/comfyanonymous/ComfyUI.git
```

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754625852299.png" width="80%" alt="克隆仓库">
  <p><em>仓库克隆过程</em></p>
</div>

#### 5. 创建并激活虚拟环境（强烈推荐）

```bash
sudo apt install python3.12-venv
python3 -m venv comfyui-env
source comfyui-env/bin/activate
```

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754626374395.png" width="80%" alt="创建虚拟环境">
  <p><em>创建虚拟环境</em></p>
</div>

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754626454397.png" width="80%" alt="激活虚拟环境">
  <p><em>激活虚拟环境</em></p>
</div>

#### 6. 安装ComfyUI依赖

如果使用虚拟环境：

```bash
cd ComfyUI && pip install -i https://mirrors.ustc.edu.cn/pypi/simple/ -r requirements.txt
```

如果未使用虚拟环境：

```bash
cd ComfyUI && pip install -i https://mirrors.ustc.edu.cn/pypi/simple/ -r requirements.txt --break-system-packages
```

> ⏱️ **注意**：依据网速不同，此步骤耗时约5-15分钟。

#### 7. 下载Qwen-Image模型文件

首先安装modelscope（已安装可跳过）：

```bash
pip install modelscope
```

下载模型文件到对应目录：

```bash
# 下载模型文件
modelscope download --model Comfy-Org/Qwen-Image_ComfyUI/split_files/diffusion_models/qwen_image_fp8_e4m3fn.safetensors --local_dir <ComfyUI目录>/models/diffusion_models/

# 下载Text Encoder模型
modelscope download --model Comfy-Org/Qwen-Image_ComfyUI/split_files/text_encoders/qwen_2.5_vl_7b_fp8_scaled.safetensors --local_dir <ComfyUI目录>/models/text_encoders/

# 下载VAE模型
modelscope download --model Comfy-Org/Qwen-Image_ComfyUI/split_files/vae/qwen_image_vae.safetensors --local_dir <ComfyUI目录>/models/vae/
```

> ⚠️ **注意**：请将`<ComfyUI目录>`替换为您实际的ComfyUI安装路径。

#### 8. 启动ComfyUI

```bash
cd <ComfyUI路径> && python3 main.py
```

启动成功后，访问 http://127.0.0.1:8188 即可看到ComfyUI的web界面。

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754628878173.png" width="80%" alt="ComfyUI启动">
  <p><em>ComfyUI成功启动</em></p>
</div>

#### 9. 加载Qwen-Image工作流

下载[ComfyUI官方Qwen-Image工作流](https://raw.githubusercontent.com/Comfy-Org/workflow_templates/refs/heads/main/templates/image_qwen_image.json)，将内容保存为JSON文件，然后拖动到ComfyUI界面加载。

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754629194209.png" width="80%" alt="工作流加载">
  <p><em>工作流加载页面</em></p>
</div>

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754629300343.png" width="80%" alt="工作流导入">
  <p><em>成功导入工作流</em></p>
</div>

如果遇到以下错误提示，表示模型文件未正确放置：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754629075136.png" width="80%" alt="模型错误">
  <p><em>模型文件未找到错误</em></p>
</div>

请确保：

- `qwen_image_fp8_e4m3fn.safetensors` 放在 `ComfyUI/models/diffusion_models/`
- `qwen_2.5_vl_7b_fp8_scaled.safetensors` 放在 `ComfyUI/models/text_encoders/`
- `qwen_image_vae.safetensors` 放在 `ComfyUI/models/vae/`

问题解决后的界面：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754629403125.png" width="80%" alt="错误解决">
  <p><em>模型加载成功</em></p>
</div>

#### 10. 测试模型效果

使用官方提供的示例提示词测试模型：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754629495286.png" width="80%" alt="测试模型">
  <p><em>使用示例提示词</em></p>
</div>

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754629612919.png" width="80%" alt="生成效果">
  <p><em>生成效果展示（初始设置，约50秒/张）</em></p>
</div>

</details>

### 容器部署

<details>
<summary><b>📌 详细步骤（点击展开/折叠）</b></summary>

#### 1. 安装Docker

如果尚未安装Docker，请参考：[Ubuntu Docker 安装教程](https://www.runoob.com/docker/ubuntu-docker-install.html)

#### 2. 下载Docker镜像

从ModelScope下载镜像文件：

```bash
# 默认路径
modelscope download --model inkcoi/ComfyUI-Qwen-Image

# 或指定下载路径
modelscope download --model inkcoi/ComfyUI-Qwen-Image --local_dir /path/you/want/place
```

> ⚠️ **注意**：镜像文件较大（约40GB，包含ComfyUI和Qwen-Image模型30GB），下载时间约1-3小时。

#### 3. 加载Docker镜像

```bash
docker load -i /模型下载路径/confyui-qwen-image-latest.tar
```

> ⏱️ **注意**：加载过程需要10-20分钟，请耐心等待。

#### 4. 验证镜像加载

```bash
docker images
```

加载成功后显示如下：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754636157740.png" width="80%" alt="Docker镜像">
  <p><em>Docker镜像加载成功</em></p>
</div>

#### 5. 部署镜像

```bash
docker run -d --name comfyui -p 8188:8188 comfyui-qwen-image:latest python3 main.py --listen 0.0.0.0
```

如果没有GPU，可以添加`--cpu`参数：

```bash
docker run -d --name comfyui -p 8188:8188 comfyui-qwen-image:latest python3 main.py --listen 0.0.0.0 --cpu
```

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754636520615.png" width="80%" alt="部署镜像">
  <p><em>部署镜像命令</em></p>
</div>

#### 6. 访问ComfyUI界面

部署成功后，访问 http://127.0.0.1:8188 即可看到ComfyUI运行界面：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754636592638.png" width="80%" alt="ComfyUI界面">
  <p><em>ComfyUI运行界面</em></p>
</div>

#### 7. 加载预置工作流

在工作流中找到预置的Qwen-Image工作流：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754636665930.png" width="80%" alt="预置工作流">
  <p><em>预置的Qwen-Image工作流</em></p>
</div>

#### 8. 开始创作

修改提示词内容，即可开始创作各种图像。

</details>

---

## 🖼 运行效果

### 1. 高画质图片生成

生成四张1328×1328像素的高画质图片，总耗时约200秒：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754630322452.png" width="90%" alt="高画质图片">
  <p><em>高画质图片生成效果</em></p>
</div>

### 2. 诗词意境图生成

模型能够较好地理解诗词中的意境并生成相应图片：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754632433519.png" width="90%" alt="诗词意境">
  <p><em>诗词意境图生成效果</em></p>
</div>

### 3. 动漫风格图生成

经典动漫风格图像生成效果：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754632906777.png" width="90%" alt="动漫风格">
  <p><em>动漫风格图生成效果</em></p>
</div>

### 4. 文字生成能力

模型的文字生成能力测试：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754633826090.png" width="90%" alt="文字生成">
  <p><em>文字生成能力展示</em></p>
</div>

### 5. 科幻风格图生成

科幻场景的生成效果：

<div align="center">
  <img src="https://raw.githubusercontent.com/ink-carp/ink-carp.github.io/master/Data/ConfyUI-Qwen-Image/1754634232029.png" width="90%" alt="科幻场景">
  <p><em>科幻场景生成效果</em></p>
</div>

---

## 📝 总结

本教程详细介绍了在ComfyUI上部署Qwen-Image模型的两种方法：

|     部署方式     | 优点                   | 缺点               |

| :--------------: | :--------------------- | :----------------- |

| **本地环境部署** | 部署速度快，可定制性高 | 过程相对繁琐       |

|   **容器部署**   | 操作简单，环境一致性好 | 镜像下载和加载较慢 |


您可以根据自己的需求选择合适的部署方式。如果您是开发者，希望进行二次开发或定制化配置，建议使用本地部署；如果您主要关注使用体验和稳定性，容器部署可能是更好的选择。

希望这篇教程对您有所帮助！如有任何问题，欢迎在评论区留言讨论。

---

<div align="center">
  <p>作者：inkcoi | 更新日期：2025年8月8日</p>
</div>