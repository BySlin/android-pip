# opencv-python Android 自动构建

## 触发方式

在 GitHub 仓库的 Actions 页面运行 `build-opencv-python-android` 工作流。

可选输入参数：

- `opencv_python_ref`：`opencv/opencv-python` 的 tag 或分支，默认 `latest`，会自动解析到最新 release tag。

## 构建产物

工作流会产出 `cp314-android_arm64_v8a` 与 `cp314-android_x86_64` 轮子，并执行两种发布：

- Actions Artifact：`opencv-python-android-cp314-arm64-x86_64`
- GitHub Release：`opencv-python-android-cp314`

## 在 Chaquopy 中使用

先从 Release 拿到 wheel 的直接下载地址，再在 `engine/build.gradle.kts` 的 `pip` 中改成 URL 安装：

```kotlin
install("opencv-python @ https://github.com/<你的GitHub用户名>/android-pip/releases/download/opencv-python-android-cp314/<wheel文件名>.whl")
```

保留 `version = "3.14"`。
