# 安装Knative

你可以通过以下方法在集群中安装Serving组件和Eventing组件：

- 使用[Knative Quickstart插件](quickstart-install.md)安装一个预配置的Knative本地版作为试验。

- 使用基于 YAML 的方法，这可以用于生产环境:
    - [通过YAML安装Knative Serving](yaml-install/serving/install-serving-with-yaml.md)
    - [通过YAML安装Knative Eventing](yaml-install/eventing/install-eventing-with-yaml.md)

- 通过[Knative Operator](operator/knative-with-operators.md)来安装，这同样适用于生产环境。

- 查看其它厂商提供的[Knative环境](knative-offerings.md).

你也可以[升级已安装的Knative](upgrade/README.md).

!!! 注意
    Knative 安装说明假设您的系统为Mac 或 Linux，并且使用bash shell
<!-- TODO: Link to provisioning guide for advanced installation -->
