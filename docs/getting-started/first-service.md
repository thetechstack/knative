# 部署你的第一个 Knative Service

**在本教程中，您将部署一个“Hello world” service**。

由于我们的“Hello world”服务被部署为 Knative Service，而不是 Kubernetes Service，因此它获得了一些**开箱即用的超能力** :rocket:.

## Knative Service: "Hello world!"

首先, 部署 Knative Service. 这个 service 接受环境变量,
`TARGET`, 并打印 `Hello ${TARGET}!`.

=== "kn"

    通过运行以下命令部署服务：

    ```bash
    kn service create hello \
    --image gcr.io/knative-samples/helloworld-go \
    --port 8080 \
    --env TARGET=World
    ```
    !!! 成功 "预期输出"
        ```{ .bash .no-copy }
        Service hello created to latest revision 'hello-world' is available at URL:
        http://hello.default.${LOADBALANCER_IP}.sslip.io
        ```
        上面`${LOADBALANCER_IP}`的值取决于你的集群类型。如果是`kind`, 值为`127.0.0.1`;
        如果是`minikube`，则取决于本地隧道（local tunnel）。

=== "YAML"
    1. 将以下内容复制到一个名为 `hello.yaml` 的文件:

        ```yaml
        apiVersion: serving.knative.dev/v1
        kind: Service
        metadata:
          name: hello
        spec:
          template:
            spec:
              containers:
                - image: gcr.io/knative-samples/helloworld-go
                  ports:
                    - containerPort: 8080
                  env:
                    - name: TARGET
                      value: "World"
        ```
    1. 通过运行以下命令部署 Knative Service：

        ```bash
        kubectl apply -f hello.yaml
        ```
        !!! 成功 "预期输出"
            ```{ .bash .no-copy }
            service.serving.knative.dev/hello created
            ```

## 列出你的Knative Service

要查看托管 Knative Service的 URL，请利用`kn` CLI：

=== "kn"
    通过运行以下命令查看 Knative services列表：

    ```bash
    kn service list
    ```
    !!! 成功 "预期输出"
        ```bash
        NAME    URL                                                LATEST        AGE   CONDITIONS   READY
        hello   http://hello.default.${LOADBALANCER_IP}.sslip.io   hello-00001   13s   3 OK / 3     True
        ```
=== "kubectl"
    通过运行以下命令查看 Knative 服务列表：

    ```bash
    kubectl get ksvc
    ```
    !!! 成功 "预期输出"
        ```bash
        NAME    URL                                                LATESTCREATED   LATESTREADY   READY   REASON
        hello   http://hello.default.${LOADBALANCER_IP}.sslip.io   hello-00001     hello-00001   True
        ```

## 访问你的 Knative Service

通过在浏览器中打开之前的 URL 或运行以下命令来访问您的 Knative Service：

```bash
echo "Accessing URL $(kn service describe hello -o url)"
curl "$(kn service describe hello -o url)"
```

!!! 成功 "预期输出"
    ```{ .bash .no-copy }
    Hello World!
    ```

??? 问题 "你遇到以下错误吗？ `curl: (6) Could not resolve host: hello.default.${LOADBALANCER_IP}.sslip.io`"

    在某些情况下你的DNS服务可能没有配置对`*.sslip.io`域名进行解析。如果您遇到此问题，可以通过使用不同的名字服务器来解析这些地址来解决此问题。

    确切的步骤将根据您的操作系统版本而有所不同。例如，对于使用`systemd-resolved`的Ubuntu 系统，您可以将以下条目添加到`/etc/systemd/resolved.conf`：

    ```ini
    [Resolve]
    DNS=8.8.8.8
    Domains=~sslip.io.
    ```

    Then simply restart the service with `sudo service systemd-resolved restart`.

    For MacOS users, you can add the DNS and domain using the network settings as explained [here](https://support.apple.com/en-gb/guide/mac-help/mh14127/mac).

恭喜 :tada:, 您刚刚创建了您的第一个 Knative 服务。接下来，自动伸缩！
