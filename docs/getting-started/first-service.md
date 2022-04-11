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

## Access your Knative Service

Access your Knative Service by opening the previous URL in your browser or by running the command:

```bash
echo "Accessing URL $(kn service describe hello -o url)"
curl "$(kn service describe hello -o url)"
```

!!! Success "Expected output"
    ```{ .bash .no-copy }
    Hello World!
    ```

??? question "Are you seeing `curl: (6) Could not resolve host: hello.default.${LOADBALANCER_IP}.sslip.io`?"

    In some cases your DNS server may be set up not to resolve `*.sslip.io` addresses. If you encounter this problem, it can be fixed by using a different nameserver to resolve these addresses.

    The exact steps will differ according to your distribution. For example, with Ubuntu derived systems which use `systemd-resolved`, you can add the following entry to the `/etc/systemd/resolved.conf`:

    ```ini
    [Resolve]
    DNS=8.8.8.8
    Domains=~sslip.io.
    ```

    Then simply restart the service with `sudo service systemd-resolved restart`.

    For MacOS users, you can add the DNS and domain using the network settings as explained [here](https://support.apple.com/en-gb/guide/mac-help/mh14127/mac).

Congratulations :tada:, you've just created your first Knative Service. Up next, Autoscaling!
