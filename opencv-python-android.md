# opencv-python Android 自动构建

## 触发方式

在 GitHub 仓库的 Actions 页面运行 `build-android-python-packages` 工作流。

可选输入参数：

- `opencv_python_ref`：`opencv/opencv-python` 的 tag 或分支，默认 `latest`，会自动解析到最新 release tag。

## 构建产物

工作流会产出 `cp314-android_arm64_v8a` 与 `cp314-android_x86_64` 轮子，并执行两种发布：

- Actions Artifact：`opencv-python-android-cp314-arm64-x86_64`
- GitHub Release：`opencv-python-android-cp314`

## numpy Android 自动构建

同样通过 `build-android-python-packages` 一次构建内完成。

可选输入参数：

- `numpy_ref`：`numpy/numpy` 的 tag 或分支，默认 `latest`，会自动解析到最新 release tag。

构建产物：

- Actions Artifact：`numpy-android-cp314-arm64-x86_64`
- GitHub Release：`numpy-android-cp314`

## 在 Chaquopy 中使用

先从 Release 拿到 wheel 的直接下载地址，再在 `engine/build.gradle.kts` 的 `pip` 中改成 URL 安装。建议先安装 numpy，再安装 opencv-python：

```kotlin
install("numpy @ https://github.com/<你的GitHub用户名>/android-pip/releases/download/numpy-android-cp314/<wheel文件名>.whl")
install("opencv-python @ https://github.com/<你的GitHub用户名>/android-pip/releases/download/opencv-python-android-cp314/<wheel文件名>.whl")
```

保留 `version = "3.14"`，并确保 ABI 与 wheel 一致：

```kotlin
android {
    defaultConfig {
        ndk {
            abiFilters += listOf("arm64-v8a", "x86_64")
        }
    }
}
```

## 常见失败点（Chaquopy）

- `No matching distribution found`：通常是 `version` 和 wheel 的 `cpXXX` 标签不一致。
- `dlopen failed`：通常是 ABI 不匹配，或 `.so` 缺少运行时依赖。
- OpenCV 导入失败：通常是先安装了 `opencv-python` 但没先安装 `numpy`。

当前工作流已经做了这些保护：

- 强制构建并校验 `android_arm64_v8a` 与 `android_x86_64` 两个 wheel。
- 对生成的 `.so` 检查并补齐 `libpython3.14.so` 依赖声明。
- 使用 `-Wl,-z,max-page-size=16384` 增强 Android 新设备兼容性。
