# 自动扩缩容

Knative Serving为应用提供自动扩缩能力， 默认通过 Knative Pod Autoscaler（KPA）实现。

例如，如果一个应用没有接收到流量并且启用了缩容到零，Knative会将改应用的副本数缩容到零。如果缩容到零没有启用，则应用副本数会缩容到指定的最小副本数。 当然，应用也可以自动扩容以应对流量的增长。

如果您具有集群管理员权限，您可以为集群启用和禁用缩容到零的功能。参阅[配置缩容到零](scale-to-zero.md)。
<!--TODO: How can you check if you have it enabled if you're not a cluster admin?-->

为了自动扩缩你的应用，你还需要配置[并发](concurrency.md)和[扩缩容上下界](scale-bounds.md)。
<!--TODO: Include this in the basic config before other settings-->

## 其它资料

<!--TODO: Move KPA details, metrics to admin / advanced section; too in depth for intro)-->
* 尝试[Go自动扩缩示例应用](autoscale-go/README.md).
* 将Knative deployment配置成使用Kubernetes的Pod水平自动扩缩(Horizontal Pod Autoscaler，HPA) 而不是默认的KPA. 关于如何安装HPA, 参阅[安装可选的Serving扩展](../../install/yaml-install/serving/install-serving-with-yaml.md#install-optional-serving-extensions).
* 配置自动扩缩(Autoscaler)使用的[指标的类型](autoscaling-metrics.md).
* 配置Knative Service使用[container-freezer](container-freezer.md), 它会在 pod 的流量降至零时冻结正在运行的进程。其好处是减少了此配置中的冷启动时间。
