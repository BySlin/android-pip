# android-pip

用于构建 Android 平台可用的 Python wheel，并发布到 GitHub Release，供 Chaquopy 通过 `pip` 安装。

## 当前目标

- Python: 3.14
- 架构: arm64-v8a, x86_64
- 包: opencv-python, numpy

## 触发构建

在 GitHub Actions 手动运行以下工作流：

- `build-opencv-python-android`，可选输入：
  - `opencv_python_ref`: `opencv/opencv-python` 的 tag 或分支，默认 `latest`。
- `build-numpy-python-android`，可选输入：
  - `numpy_ref`: `numpy/numpy` 的 tag 或分支，默认 `latest`。

## 输出

- Artifact: `opencv-python-android-cp314-arm64-x86_64`
- Release Tag: `opencv-python-android-cp314`
- Artifact: `numpy-android-cp314-arm64-x86_64`
- Release Tag: `numpy-android-cp314`
