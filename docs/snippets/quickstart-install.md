# 使用quickstart安装 Knative

本主题介绍如何使用 Knative `quickstart`插件安装 Knative Serving 和 Eventing 的本地部署（local deployment）。

该插件在本地 Kubernetes 集群上安装预配置的 Knative 部署（deployment）。

!!! 警告
    Knative `quickstart` 环境仅供实验使用。
    关于生成环境的安装，请参阅 [基于YAML的安装](/knative/install/yaml-install/)
    或 [Knative Operator安装](/knative/install/operator/knative-with-operators/)

## 开始之前

在开始使用 Knative `quickstart`部署之前，您必须安装：

- [kind](https://kind.sigs.k8s.io/docs/user/quick-start){target=_blank} (Kubernetes in Docker)
or [minikube](https://minikube.sigs.k8s.io/docs/start/){target=_blank} 以 使用 Docker 容器节点运行本地 Kubernetes 集群
- [Kubernetes CLI (`kubectl`)](https://kubernetes.io/docs/tasks/tools/install-kubectl){target=_blank} 
以在Kubernetes集群运行命令。 你可以使用`kubectl`来部署应用，检查和管理集群资源以及查看日志。
- Knative CLI (`kn`). 有关说明，请参阅下一节.
- 您需要至少 3 个 CPU 和 3 GB 的 RAM 才能创建集群

### Install the Knative CLI

--8<-- "install-kn.md"

## 安装Knative quickstart 插件

=== "用 Homebrew"
    
    执行以下操作之一：
    
    - 执行以下命令，用 [Homebrew](https://brew.sh){target=_blank} 安装`quickstart`:

        ```bash
        brew install knative-sandbox/kn-plugins/quickstart
        ```

    - 执行以下命令，将 `quickstart` 更新到最新版本:

        ```bash
        brew upgrade knative-sandbox/kn-plugins/quickstart
        ```
=== "Using a binary"

    1. Download the binary for your system from the [`quickstart` release page](https://github.com/knative-sandbox/kn-plugin-quickstart/releases){target=_blank}.

    1. Rename the file to remove the OS and architecture information. For example, rename `kn-quickstart-amd64` to `kn-quickstart`.

    1. Make the plugin executable. For example, `chmod +x kn-quickstart`.

    1. Move the executable binary file to a directory on your `PATH`, for example, in `/usr/local/bin`.

    1. Verify that the plugin is working by running the command:

        ```bash
        kn quickstart --help
        ```

=== "Using Go"
    1. Check out the `kn-plugin-quickstart` repository:

          ```bash
          git clone https://github.com/knative-sandbox/kn-plugin-quickstart.git
          cd kn-plugin-quickstart/
          ```

    1. Build an executable binary:

          ```bash
          hack/build.sh
          ```

    1. Move the executable binary file to a directory on your `PATH`:

          ```bash
          mv kn-quickstart /usr/local/bin
          ```

     1. Verify that the plugin is working by running the command:

          ```bash
          kn quickstart --help
          ```

## Run the Knative quickstart plugin

The `quickstart` plugin completes the following functions:

1. **Checks if you have the selected Kubernetes instance installed**
1. **Creates a cluster called `knative`**
1. **Installs Knative Serving** with Kourier as the default networking layer, and sslip.io as the DNS
1. **Installs Knative Eventing** and creates an in-memory Broker and Channel implementation


To get a local deployment of Knative, run the `quickstart` plugin:

=== "Using kind"


    1. Install Knative and Kubernetes on a local Docker daemon by running:

        ```bash
        kn quickstart kind
        ```

    1. After the plugin is finished, verify you have a cluster called `knative`:

        ```bash
        kind get clusters
        ```

=== "Using minikube"

    1. Install Knative and Kubernetes in a minikube instance by running:

        !!! note
            The minikube cluster will be created with 6&nbsp;GB of RAM. If you don't have enough memory, you can change to a
            different value not lower than 3&nbsp;GB by running the command `minikube config set memory 3078` before this command.
        ```bash
        kn quickstart minikube
        ```

    1. The output of the previous command asked you to run minikube tunnel.
       Run the following command to start the process in a secondary terminal window, then return to the primary window and press enter to continue:
        ```bash
        minikube tunnel --profile knative
        ```
        The tunnel must continue to run in a terminal window any time you are using your Knative `quickstart` environment.

        The tunnel command is required because it allows your cluster to access Knative ingress service as a LoadBalancer from your host computer.

        !!! note
            To terminate the tunnel process and clean up network routes, enter `Ctrl-C`.
            For more information about the `minikube tunnel` command, see the [minikube documentation](https://minikube.sigs.k8s.io/docs/handbook/accessing/#using-minikube-tunnel).

    1. After the plugin is finished, verify you have a cluster called `knative`:

        ```bash
        minikube profile list
        ```
