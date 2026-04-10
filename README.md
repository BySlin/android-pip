# android-pip

用于构建 Android 平台可用的 Python wheel，并发布到 GitHub Release，供 Chaquopy 通过 `pip` 安装。

## 当前目标

- Python: 3.14
- 架构: arm64-v8a, x86_64
- 包: opencv-python, numpy

## 触发构建

在 GitHub Actions 手动运行工作流 `build-android-python-packages`，可选输入：

- `numpy_ref`: `numpy/numpy` 的 tag 或分支，默认 `latest`。
- `opencv_python_ref`: `opencv/opencv-python` 的 tag 或分支，默认 `latest`。

## 输出

- Artifact: `opencv-python-android-cp314-arm64-x86_64`
- Release Tag: `opencv-python-android-cp314`
- Artifact: `numpy-android-cp314-arm64-x86_64`
- Release Tag: `numpy-android-cp314`

## Chaquopy 使用要点

- Gradle 的 `python.version` 必须和 wheel 的 CP 标签一致（当前是 `cp314`，即 `3.14`）。
- Android ABI 建议只保留你构建的架构：`arm64-v8a`、`x86_64`。
- 安装顺序建议先 `numpy` 再 `opencv-python`。

示例（`app/build.gradle.kts`）：

```kotlin
android {
    defaultConfig {
        ndk {
            abiFilters += listOf("arm64-v8a", "x86_64")
        }
    }
}

chaquopy {
    defaultConfig {
        version = "3.14"
        pip {
            install("numpy @ https://github.com/<你的GitHub用户名>/android-pip/releases/download/numpy-android-cp314/<numpy-wheel>.whl")
            install("opencv-python @ https://github.com/<你的GitHub用户名>/android-pip/releases/download/opencv-python-android-cp314/<opencv-wheel>.whl")
        }
    }
}
```
