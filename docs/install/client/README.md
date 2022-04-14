# CLI 工具

以下 CLI 工具支持与 Knative 一起使用。

## kubectl

您可以通过`kubectl apply` YAML 文件来安装Knative 组件 ，也可以apply YAML文件来创建 Knative 资源，例如services和事件源。

参阅 [安装和设置`kubectl`](https://kubernetes.io/docs/tasks/tools/install-kubectl/){target=_blank}.

## kn

`kn`为创建Service和事件源等 Knative 资源提供了一个快速简单的方式，无需直接创建或修改 YAML 文件。`kn`还简化了其他复杂过程的完成，例如自动扩缩和流量拆分。

!!! 注意
    `kn` 不能用于安装 Serving 或 Eventing 等 Knative 组件。

参阅 [安装 `kn`](install-kn.md).

## 将 CLI 工具连接到集群

安装好kubectl或kn后，这些工具会在`$HOME/.kube/config`设置的位置找到你的 `kubeconfig`文件，并使用该文件连接到集群。`kubeconfig`文件通常在你创建 Kubernetes 集群自动创建。

您还可以设置环境变量 `$KUBECONFIG`，将其指向 kubeconfig 文件。

使用 `kn` CLI，您可以指定以下选项以连接到集群：

- `--kubeconfig`: 使用此选项指向kubeconfig文件。这相当于设置`$KUBECONFIG`环境变量。
- `--context`: 使用此选项指定现有kubeconfig文件中的上下文名称。使用`kubectl config get-contexts`的输出中的上下文之一。

您还可以通过以下方式指定配置文件：

- 使用`kn` CLI  `--config`选项，例如 `kn service list --config path/to/config.yaml`. 默认配置位于`~/.config/kn/config.yaml`

有关kubeconfig文件的更多信息，请参阅 [使用 kubeconfig 文件访问集群](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/){target=_blank}。


### 在您的平台上使用 kubeconfig 文件

以下平台提供了使用kubeconfig文件的说明：

- [Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html){target=_blank}
- [Google GKE](https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl){target=_blank}
- [IBM IKS](https://cloud.ibm.com/docs/containers?topic=containers-getting-started){target=_blank}
- [Red Hat OpenShift Cloud Platform](https://docs.openshift.com/container-platform/4.6/cli_reference/openshift_cli/administrator-cli-commands.html#create-kubeconfig){target=_blank}
- 启动[minikube](https://minikube.sigs.k8s.io/docs/start/){target=_blank} 会自动写入此文件，或在现有配置文件中提供适当的上下文。
