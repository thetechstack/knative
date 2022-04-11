Knative CLI (`kn`) 为创建 Knative 资源（例如 Knative 服务和事件源）提供了一个快速简单的工具，而无需直接创建或修改 YAML 文件。

`kn` CLI 还简化了其他复杂步骤的完成，例如自动伸缩和流量拆分。

=== "用 Homebrew"

    执行以下操作之一：

    - 执行以下命令，用 [Homebrew](https://brew.sh){target=_blank} 安装`kn`:

        ```bash
        brew install kn
        ```

    - 执行以下命令，将已安装的 `kn` 更新到最新版本:

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

    You can install `kn` by downloading the executable binary for your system and placing it in the system path. Note that you will need `kn` v0.25 or later.

    1. Download the binary for your system from the [`kn` release page](https://github.com/knative/client/releases){target=_blank}.

    1. Rename the binary to `kn` and make it executable by running the commands:

        ```bash
        mv <path-to-binary-file> kn
        chmod +x kn
        ```

        Where `<path-to-binary-file>` is the path to the binary file you downloaded in the previous step, for example, `kn-darwin-amd64` or `kn-linux-amd64`.

    1. Move the executable binary file to a directory on your PATH by running the command:

        ```bash
        mv kn /usr/local/bin
        ```

    1. Verify that the plugin is working by running the command:

        ```bash
        kn version
        ```

=== "用 Go"

    1. Check out the `kn` client repository:

          ```bash
          git clone https://github.com/knative/client.git
          cd client/
          ```

    1. Build an executable binary:

          ```bash
          hack/build.sh -f
          ```

    1. Move `kn` into your system path, and verify that `kn` commands are working properly. For example:

          ```bash
          kn version
          ```

=== "用docker镜像"

    Links to images are available here:

    - [Latest release](https://gcr.io/knative-releases/knative.dev/client/cmd/kn){target=_blank}

    You can run `kn` from a container image. For example:

    ```bash
    docker run --rm -v "$HOME/.kube/config:/root/.kube/config" gcr.io/knative-releases/knative.dev/client/cmd/kn:latest service list
    ```

    !!! note
        Running `kn` from a container image does not place the binary on a permanent path. This procedure must be repeated each time you want to use `kn`.
