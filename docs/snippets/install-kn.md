Knative CLI (`kn`) 为创建 Knative 资源（例如 Knative 服务和事件源）提供了一个快速简单的工具，而无需直接创建或修改 YAML 文件。

`kn` CLI 还简化了其他复杂步骤的完成，例如自动伸缩和流量拆分。

=== "用 Homebrew"
    
    !!! 提示   
        国内安装Homebrew的方法，可参考知乎[这篇文章](https://zhuanlan.zhihu.com/p/111014448)

    执行以下操作之一：

    - 执行以下命令，用 [Homebrew](https://brew.sh){target=_blank} 安装`kn`:

        ```bash
        brew install kn
        ```

    - 执行以下命令，将 `kn` 更新到最新版本:

        ```bash
        brew upgrade kn
        ```

        ??? bug "使用Homebrew升级 `kn` 时遇到问题?"
            
            如果您在使用 Homebrew 升级`kn`时遇到问题，可能是由于CLI仓库的master分支被重命名为main了. 可以通过运行以下命令解决此问题：

            ```bash
            brew tap --repair
            brew update
            brew upgrade kn
            ```

=== "用二进制文件"

    您可以通过下载对应你的操作系统的`kn`可执行二进制文件并将其放置在系统路径中来安装`kn`。请注意，您将需要`kn` v0.25 或更高版本。

    1. 从 [`kn` 发布页面](https://github.com/knative/client/releases){target=_blank}. 下载对应你的操作系统的二进制文件。

    1. 通过运行以下命令将二进制文件重命名为`kn`并使其可执行：

        ```bash
        mv <path-to-binary-file> kn
        chmod +x kn
        ```

        其中 `<path-to-binary-file>` 是你在上一步下载的二进制文件的存放路径, 比如, `kn-darwin-amd64` 或 `kn-linux-amd64`.

    1. 通过运行以下命令将可执行二进制文件移动到 PATH 上的目录：

        ```bash
        mv kn /usr/local/bin
        ```

    1. 通过运行以下命令验证插件是否正常工作：

        ```bash
        kn version
        ```

=== "用 Go"

    1. Check out `kn` 客户端仓库:

          ```bash
          git clone https://github.com/knative/client.git
          cd client/
          ```

    1. 构建可执行二进制文件:

          ```bash
          hack/build.sh -f
          ```

    1. 将 `kn` 加入你的系统路径, 并验证`kn`命令是否正常工作。例如：

          ```bash
          kn version
          ```

=== "用docker镜像"

    镜像链接:

    - [Latest release](https://gcr.io/knative-releases/knative.dev/client/cmd/kn){target=_blank}

    你可以在容器镜像中运行`kn`命令，例如:

    ```bash
    docker run --rm -v "$HOME/.kube/config:/root/.kube/config" gcr.io/knative-releases/knative.dev/client/cmd/kn:latest service list
    ```

    !!! note
        从容器映像运行`kn`不会将二进制文件放在永久路径上。每次要使用`kn`时都必须重复此过程。
