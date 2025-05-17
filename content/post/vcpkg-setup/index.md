---
title: "vcpkgを使ってC++ライブラリを管理する手順（Windows + Visual Studio）"
description: "Windows環境でvcpkgを用いてライブラリをインストールする方法"
slug: vcpkg-setup
date: 2025-05-17 17:00:00+0900
image:
math: false
license: false
hidden: false
comments: false
draft: false
categories:
    - C++
    - 環境構築
tags:
    - vcpkg
    - Visual Studio
    - Windows
    - パッケージ管理
---

本記事は、Windows + Visual Studio 環境で Microsoft 製のパッケージマネージャ『**vcpkg**』を使い、C++ライブラリを管理する手順の備忘録です。

## 前提条件

- OS：Windows11
- Visual Studio 2022（C++デスクトップ開発）
- Gitインストール済み

## 📌 vcpkgとは？

vcpkg は Microsoft が提供するオープンソースのC++ライブラリ用パッケージマネージャです。  
依存関係の解決やライブラリ導入が簡単に行えます。

公式リポジトリ：[vcpkg (GitHub)](https://github.com/microsoft/vcpkg)

## 📌 作業手順

1. vcpkgをクローン
2. ブートストラップ
3. Visual Studioと統合
4. ライブラリのインストール
5. Visual Studioで動作確認

---

1. vcpkgをクローン

PowerShellやコマンドプロンプトで以下を実行します。

```powershell
cd C:\Tools
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg
```

※パス(C:\Tools)は任意で変更可能

1. ブートストラップ

vcpkg.exeを作成します。

```powershell
.\bootstrap-vcpkg.bat
```

成功すると vcpkg.exe が生成されます。

1. Visual Studioとの統合

Visual Studioでvcpkgのライブラリを自動認識します。

```powershell
.\vcpkg integrate install
```

1. ライブラリのインストール

例として、以下のライブラリをインストールします。

- GLEW
- GLFW3
- GLM
- Eigen3

```powershell
.\vcpkg install glew:x64-windows glfw3:x64-windows glm:x64-windows eigen3:x64-windows
```

1. Visual Studioで動作確認

以下のコードでビルド・実行確認をします。

```cpp
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <glm/glm.hpp>
#include <Eigen/Core>

int main() {
    if (!glfwInit()) return -1;

    GLFWwindow* window = glfwCreateWindow(640, 480, "vcpkg Test", nullptr, nullptr);
    glfwMakeContextCurrent(window);
    
    if (glewInit() != GLEW_OK) return -1;

    glm::vec3 glm_vec(1.0f, 2.0f, 3.0f);
    Eigen::Vector3f eigen_vec(4.0f, 5.0f, 6.0f);

    while (!glfwWindowShouldClose(window)) {
        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    glfwDestroyWindow(window);
    glfwTerminate();
    return 0;
}
```

## 📚 参考リンク

- [vcpkg (GitHub)](https://github.com/microsoft/vcpkg)
- [Visual Studio](https://visualstudio.microsoft.com/ja/downloads/)
- [GLEW](http://glew.sourceforge.net/)
- [GLFW](https://www.glfw.org/)
- [GLM](https://github.com/g-truc/glm)
- [Eigen](https://eigen.tuxfamily.org/)
