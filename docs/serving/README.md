# Knative Serving

Knative Serving 提供支持以下功能的组件：

- 快速部署serverless容器。
- 自动伸缩，包括将 pod 数量缩小到零。
- 支持多种网络层，例如 Contour、Kourier 和 Istio，以集成到现有环境中。
- 已部署代码和配置的时间点快照（Point-in-time snapshots）。

Knative Serving 支持 HTTP 和 [HTTPS](using-a-tls-cert.md) 网络协议。

## 安装

您可以通过[安装页面](../install/README.md)上列出的方法安装 Knative Serving 。

## Serving resources


Knative Serving 将一组对象定义为 Kubernetes 自定义资源 (Kubernetes Custom Resource
Definitions，CRD)。这些对象用于定义和控制serverless工作负载在集群上的行为方式：

- [服务](https://github.com/knative/specs/blob/main/specs/serving/knative-api-specification-1.0.md#service):
  `service.serving.knative.dev`资源自动管理工作负载的整个生命周期。它控制其他对象的创建，以确保您的应用程序具有路由、
  配置和每次更新服务后拥有新的版本(revision)。可以将服务定义为始终将流量路由到最新版版或特定版本
- [路由](https://github.com/knative/specs/blob/main/specs/serving/knative-api-specification-1.0.md#route):
  `route.serving.knative.dev` 资源将网络端点(network endpoint)映射到一个或多个版本。您可以通过多种方式管理流量，
   包括按比例分配流量（fractional traffic）和命名路由（named routes）。
- [配置](https://github.com/knative/specs/blob/main/specs/serving/knative-api-specification-1.0.md#configuration):
  `configuration.serving.knative.dev` 资源保持部署(deployment)所需的状态。它在代码和配置之间提供了清晰的分离，
  并遵循十二因素应用方法论（Twelve-Factor App methodology）。修改配置会创建一个新版本。
- [版本](https://github.com/knative/specs/blob/main/specs/serving/knative-api-specification-1.0.md#revision):
  `revision.serving.knative.dev` 资源是对工作负载进行的每次修改的代码和配置的时间点快照。版本是不可变的对象，
  只要有用就可以保留（can be retained for as long as useful）。Knative Serving Revisions 可以根据传入的流量自动伸缩。
  有关详细信息，请参阅[配置自动伸缩](autoscaling/README.md)。 

![显示服务资源如何相互协调的关系图。](https://github.com/knative/serving/raw/main/docs/spec/images/object_model.png)

## Getting Started

To get started with Serving, check out one of the [hello world](../samples/serving.md)
sample projects. These projects use the `Service` resource, which manages all of
the details for you.

With the `Service` resource, a deployed service will automatically have a
matching route and configuration created. Each time the `Service` is updated, a
new revision is created.

For more information on the resources and their interactions, see the [Resource Types Overview](https://github.com/knative/specs/blob/main/specs/serving/overview.md) in the Knative Serving repository.

## More samples and demos

- [Knative Serving code samples](../samples/serving.md)

## Debugging Knative Serving issues

- [Debugging application issues](troubleshooting/debugging-application-issues.md)

## Configuration and Networking

- [Configuring cluster local routes](services/private-services.md)
- [Using a custom domain](using-a-custom-domain.md)
- [Traffic management](traffic-management.md)

## Observability

- [Serving Metrics API](observability/metrics/serving-metrics.md)

## Known Issues

See the [Knative Serving Issues](https://github.com/knative/serving/issues) page
for a full list of known issues.
