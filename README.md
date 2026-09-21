# LaTeX OCR Windows

一个面向 Windows 10/11 64 位的 LaTeX 公式 OCR 桌面工具。基于 [pix2tex / LaTeX-OCR](https://github.com/lukas-blecher/LaTeX-OCR) 封装，目标是让普通 Windows 用户无需配置 Python、PyTorch 或 CUDA，安装后直接使用。

## 功能

- **截图识别**：按 `Alt + S`，拖动框选公式区域后自动识别
- **清晰选区反馈**：选区外压暗，选区保持原亮度，并显示高对比边框
- **粘贴图片**：`Ctrl + V` 直接识别剪贴板图片
- **打开本地图片**：支持 PNG / JPG / JPEG / BMP / WebP
- **自动复制结果**：识别后自动复制 LaTeX 到剪贴板
- **输出格式**：LaTeX、Raw、`$$...$$`
- **CPU 通用版**：不要求 NVIDIA 显卡或 CUDA
- **首次自动下载模型**：模型保存在用户 LocalAppData，后续可离线运行

## 下载与安装

推荐从仓库右侧的 **Releases** 下载最新版：

`LaTeX-OCR-Setup-v1.0.2-x64.exe`

安装完成后直接从桌面快捷方式启动。首次运行会从上游 LaTeX-OCR GitHub Release 自动下载约 120 MB 模型文件；模型下载完成后，之后启动无需再次下载。

> 当前版本：**v1.0.2**

## 使用方法

### 截图识别

1. 启动 LaTeX OCR。
2. 点击 **截图识别**，或按 `Alt + S`。
3. 鼠标拖动框选公式区域。
4. 松开鼠标后自动进行 OCR。
5. 识别出的 LaTeX 会显示在右侧，并按设置自动复制到剪贴板。

### 粘贴图片

复制一张包含公式的图片后，在程序内按 `Ctrl + V`，即可直接识别。

### 打开图片

点击 **打开图片**，选择本地公式图片即可识别。

## 模型文件位置

程序不会把模型写入安装目录，而是保存到当前 Windows 用户目录：

```text
%LOCALAPPDATA%\LaTeX-OCR\models\
├─ weights.pth
└─ image_resizer.pth
```

这样无需管理员权限，也避免安装目录不可写的问题。

## 从源码构建

本项目的 Windows 安装包使用：

- Python 3.10 x64
- CPU 版 PyTorch
- PyInstaller
- Inno Setup

主要文件：

```text
.
├─ app.py
├─ latexocr.spec
├─ installer.iss
├─ requirements-build.txt
├─ LICENSE-THIRD-PARTY.txt
└─ .github/
   └─ workflows/
      └─ windows-build.yml
```

GitHub Actions 会在 Windows Server 构建机上完成：

1. 安装 CPU 版 PyTorch 与依赖
2. PyInstaller 打包
3. 对打包后的 EXE 执行 GUI 启动自检
4. Inno Setup 生成 `Setup.exe`
5. 上传 Windows 安装包 Artifact
6. 当推送 `v*` 标签时，将安装包发布到 GitHub Releases

## 与上游项目的关系

本仓库不是 pix2tex / LaTeX-OCR 官方 Windows 发行版。

OCR 核心来自：

- [lukas-blecher/LaTeX-OCR](https://github.com/lukas-blecher/LaTeX-OCR)

本项目主要做了 Windows 桌面封装、安装器、模型存储路径、截图交互和自动构建等适配。

## 授权说明

上游 **LaTeX-OCR 源代码**使用 MIT License。

需要特别注意：上游作者明确说明 **模型权重**使用 **CC BY-NC-SA 4.0**（署名-非商业性使用-相同方式共享）。本安装包**不内置模型权重**，首次启动时直接从上游 GitHub Release 下载原始模型文件。

如果准备将本项目用于商业发行、收费软件或其他商业场景，请先确认模型权重许可是否满足你的用途。

详细第三方许可信息见 [LICENSE-THIRD-PARTY.txt](LICENSE-THIRD-PARTY.txt)。

## 版本

### v1.0.2

- 修复截图拖拽时选区不明显的问题
- 选区外压暗、选区内保持原亮度
- 增加高对比双层选区边框
- 保留截图、粘贴、打开图片与自动复制 LaTeX 功能

### v1.0.1

- 修复 Windows Tkinter 启动布局错误
- 构建流程加入完整 GUI 启动自检

### v1.0.0

- 首个 Windows 安装版

---

Made for Windows by GhostForgeLab.
