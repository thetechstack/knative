# 创建一个Service

你可用通过apply YAML文件或使用`kn service create`来创建Knative service。

## 前提条件

要创建 Knative 服务，您需要：

* 安装了 Knative Serving 的 Kubernetes 集群。有关更多信息，请参阅[安装 Knative Serving](../../install/yaml-install/serving/install-serving-with-yaml.md)。
* 可选：要使用该`kn service create`命令，您必须[安装`kn` CLI]((../../install/client/configure-kn.md).)。

## 步骤

!!! 提示
    
    以下命令创建`helloworld-go`示例service。您可以修改这些命令，包括容器镜像 URL，以将您自己的应用程序部署为 Knative 服务。

创建示例服务：

=== "Apply YAML"

    1. 使用以下示例创建 YAML 文件:

        ```yaml
        apiVersion: serving.knative.dev/v1
        kind: Service
        metadata:
          name: helloworld-go
          namespace: default
        spec:
          template:
            spec:
              containers:
                - image: gcr.io/knative-samples/helloworld-go
                  env:
                    - name: TARGET
                      value: "Go Sample v1"
        ```

    1. 通过运行以下命令应用 YAML 文件：

        ```bash
        kubectl apply -f <filename>.yaml
        ```
        
        `<filename>`您在上一步中创建的文件的名称。

=== "kn CLI"

    ```
    kn service create helloworld-go --image gcr.io/knative-samples/helloworld-go
    ```

创建service后，Knative 会执行以下任务：

* 为此版本的应用程序创建一个新的不可变的revision。
* 执行网络相关操作以为您的应用程序创建路由、ingress、(kubernetes)service和负载均衡器。
* 根据流量自动缩放您的 Pod，包括缩容到零。
