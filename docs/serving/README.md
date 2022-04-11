# Knative Serving

Knative Serving 提供支持以下功能的组件：

- 快速部署serverless容器。
- 自动伸缩，包括将 pod 数量缩小到零。
- 支持多种网络层，例如 Contour、Kourier 和 Istio，以集成到现有环境中。
- 已部署代码和配置的时间点快照（Point-in-time snapshots）。

Knative Serving 支持 HTTP 和 [HTTPS](using-a-tls-cert.md) 网络协议。

## 安装

您可以通过[安装页面](../install/README.md)上列出的方法安装 Knative Serving 。

## Serving 资源


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

## 入门

要开始使用 Serving，请查看[hello world](../samples/serving.md)示例项目之一 。这些项目使用Service资源，它为您管理所有细节。

通过使用`Service`资源，部署的服务将自动创建匹配的路由和配置。每次`Service`更新时，都会创建一个新版本

有关资源及其交互的更多信息，请参阅 Knative Serving 仓库中的[资源类型概述](https://github.com/knative/specs/blob/main/specs/serving/overview.md)。

## 更多示例和演示

- [Knative Serving 代码示例](../samples/serving.md)

## 调试 Knative Serving 问题

- [调试应用程序问题](troubleshooting/debugging-application-issues.md)

## 配置和网络

- [配置集群本地路由](services/private-services.md)
- [使用自定义域名](using-a-custom-domain.md)
- [流量管理](traffic-management.md)

## 可观测性

- [Serving 指标 API](observability/metrics/serving-metrics.md)

## 已知问题

有关已知问题的完整列表，请参阅[Knative Serving 问题页面](https://github.com/knative/serving/issues)。
