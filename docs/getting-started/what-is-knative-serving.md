# 什么是Knative Serving

!!! 说明
  本文由[面向问题编程](https://www.heapoverflow.cn/)翻译自[What is Knative Serving? A Friendly Guide](https://dev.to/wiggitywhitney/9-waa-w-what-is-knative-serving-a-friendly-guide-28f6)

简单地说，Knative 是一种**简化**和**增强**应用程序在 Kubernetes 上的运行方式的技术。 Knative 本身在 Kubernetes 上运行，
主要有两个组件：Knative Serving 和 Knative Eventing。 本文是关于 Knative Serving 的。

[![图片1](https://res.cloudinary.com/practicaldev/image/fetch/s--T0ER7_jd--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2egl13zn35nybn4jonz9.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--T0ER7_jd--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2egl13zn35nybn4jonz9.png)

## [](#vanilla-kubernetes-is-complicated) Kubernetes 很复杂

Knative 如何简化在 Kubernetes 上运行应用程序的过程？要理解这一点，您必须首先了解这一点：**在Kubernetes 上部署应用程序是复杂的**。您首先创建一个 _Deployment_，
它实际管理许多 _ReplicaSet_ （每个版本的应用程序都有一个），每个 ReplicaSet 运行一个或多个 _pod_ ，这是您的应用程序容器运行的地方，通常每个 pod 有一个工作负载（workload）容器。

[![图片2](https://res.cloudinary.com/practicaldev/image/fetch/s--1DSsaste--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/i4f04m2rczb2xy7qwr4o.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--1DSsaste--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/i4f04m2rczb2xy7qwr4o.png)

但是，您还需要创建一个 _Service_ 以在集群内部暴露您的应用程序（以便其它应用程序可以访问它），并创建一个 _Ingress_ 以使您的应用程序在集群外部可用（以便最终用户可以访问它）。如果要自动扩缩，则需要创建一个 _HorizontalPodAutoscaler_ (HPA)。您还需要为您的管理您的配置和密钥。

[![图3](https://res.cloudinary.com/practicaldev/image/fetch/s--4KVHACeU--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zou81yts471jhw4nv0s3.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--4KVHACeU--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zou81yts471jhw4nv0s3.png)

这是有太多需要考虑的。对于开发人员来说，这可能是一个很高的进入门槛——无论如何，开发人员应该将时间花在编写应用程序代码上。

Knative闪亮登场。

[![图4](https://res.cloudinary.com/practicaldev/image/fetch/s--Wg7KUyzz--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d3alcm3sl4zakgc7l4b2.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Wg7KUyzz--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d3alcm3sl4zakgc7l4b2.png)

使用 Knative，您只需创建一个资源 \- Knative Service \- 然后 Knative 为您协调一组资源，以完成我们在上面的 Kubernetes 示例中介绍的所有内容！
通过默认设置, 使用一个开箱即用的命令即可让您运行您的应用程序并从外部访问它。

## [](#knative-simplifies-application-deployment)Knative 简化了应用部署

它是如何工作的？

`kn service create sunshine –image=rainbows`

运行`kn service create`命令，提供您的应用程序名称、应用程序镜像和其它配置（环境变量、首选端口等），Knative 将为您创建一个 Knative Service。

[![图6](https://res.cloudinary.com/practicaldev/image/fetch/s--VVsHjJ_Q--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/10fzzzr7smbdmh5620c6.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--VVsHjJ_Q--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/10fzzzr7smbdmh5620c6.png)

Knative Service 会自动为您的 Service 创建 _配置_ 和 _路由_ 。配置管理一系列 _修订版本(Revision)_，每个Revision都与一个 Kubernetes Deployment和一个 _Knative Pod Autoscaler_ (KPA) 相关联。

[![图7](https://res.cloudinary.com/practicaldev/image/fetch/s--Z-M7NpjV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9fsnnin7b2o0rxmu3pia.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Z-M7NpjV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9fsnnin7b2o0rxmu3pia.png)

我将会解释这些组件中的每一个。但现在我想让你知道两件事：

1.  当您运行**kn service create**命令时，Knative会为您创建所有这些对象，并且该命令会向您返回一个 URL，您可以通过它访问正在运行的应用程序。就这么容易。
2.  您还可以查看、访问和操作 Kubernetes 集群中的所有这些 Knative 资源 或 Knatiev资源依赖的Kubernetes资源（Deployment等）。 Knative 构建在 Kubernetes 之上，但它并没有掩盖它们。

## [](#knative-scales-to-zero) 缩容到零

流量如何通过这些 Knative 抽象到达 Kubernetes Deployment中正在运行的应用程序？

目前，我们的系统只有一个Revision， 所以当前流量像这样移动：

[![图片8](https://res.cloudinary.com/practicaldev/image/fetch/s--kLUJ0ENy--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hxmhpvzdiyj2e6azdai1.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--kLUJ0ENy--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hxmhpvzdiyj2e6azdai1.png)

流量通过 Knative Route 进入集群。默认情况下，路由将 100\% 的流量发送到最新版本，但这是可配置的，我们将在本文后面讨论。

Knative Pod Autoscaler \(KPA\) 正在监视 Revision 收到的请求数量，并将根据需要自动扩展 Kubernetes Deployment中的 pod 数量。（我们后面讨论什么是Revision）。

[![图 9](https://res.cloudinary.com/practicaldev/image/fetch/s--27O_M-m---/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/snlucjcje503yxc3mu8i.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--27O_M-m---/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/snlucjcje503yxc3mu8i.png)

而当没有流量流向您的应用程序时，Knative 会将 pod 的数量缩容到零！ 这是与 Kubernetes 不同的地方，在原始的Kubernetes, 您需要始终启动并运行至少一个 pod 实例才能为应用程序提供服务，而 Knative 可以**缩容到零**。
然后，当客户端请求访问您的应用程序时， Knative 才会实际运行的应用程序的Pod。 对于大多数时间没有请求的应用程序，这可以节省不少钱。

[![图片10](https://res.cloudinary.com/practicaldev/image/fetch/s--UwibvKxI--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2ic2qww6qwhyl27fg97u.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--UwibvKxI--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2ic2qww6qwhyl27fg97u.png)

## [](#tidier-release-management-with-knative-revisions) 通过Knative Revision 实现更好的发布管理

正如我之前提到的，Knative Configuration 对象管理着系列Revision。什么是 Knative Revision？
**Revision是您当前代码和配置的时间快照**，包含运行该特定版本的应用程序所需的所有信息。

您想升级您的应用程序以使用最新的容器映像？ 砰！Knative会产生一个新的Revision。
您想更新应用程序的配置？砰！又是一个新的Revision。这些Revisions是有序的、不可变的，并且可存储到磁盘。

### [](#traffic-splitting) 流量切分

使用Revision管理您的版本有两个主要好处。首先，您可以轻松地在Revision之间切分流量。
这在推出新版本时特别有用。假设您目前有 100\% 的流量流向Revision 2，并且您准备升级到更新版本，Revision 3。首先，您可以将少量流量发送到Revision 3，例如 10\%。然后您运行一些用户测试，感觉更有信心，现在将 20\% 的流量发送到新应用程序。
您可以以这种方式继续，直到 100\% 的流量流向Revision 3，而 0\% 的流量流向Revision 2。

[![图片11](https://res.cloudinary.com/practicaldev/image/fetch/s--Y-Eqsk_T--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hjwn3cv55ka7csojkp11.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Y-Eqsk_T--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hjwn3cv55ka7csojkp11.png)

### [](#rollbacks) 回滚

Revision的第二个主要好处是回滚。由于 Knative 版本代表时间快照，因此您可以跳回您喜欢的任何版本的应用程序。如果您发现您的某个应用程序依赖项存在漏洞，您可以将流量引导回不使用该依赖项的最新版本——即使该漏洞是去年引入的，并且自那时以来已经发布了 20 个版本。
当然，使用 Kubernetes，您也可以回滚，但使用 Kubernetes，回滚可能会很复杂。 跟踪与每个版本相关的配置是一个难题，回滚超过一两个周期可能特别困难。

[![图12](https://res.cloudinary.com/practicaldev/image/fetch/s--YkiQfatR--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yodh8ncrnc0vvja42r3p.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--YkiQfatR--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yodh8ncrnc0vvja42r3p.png)

## [](#knative-gives-finegrained-control-over-routes-and-access)Knative 提供对路由和访问的细粒度控制

更重要的是，使用 Knative 的 Route 抽象，您可以设置路由以提供对完全实验性且不打算向公众公开的Revision的内部访问。你可以设置一个站点和一个内部路径来对不同的 UI 模式进行测试，或者评估一个激进的新想法的可行性。
Knative Route 组件使您可以对路由和访问进行细粒度控制。

[![图13](https://res.cloudinary.com/practicaldev/image/fetch/s--ZQlldsU1--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/i1n4rxq769vqgjk47n5v.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--ZQlldsU1--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/i1n4rxq769vqgjk47n5v.png)

总之，以下是 Knative Serving 的诸多好处：

* 易于部署
* 缩容到零
* 流量切分
* 回滚
* 路由和访问控制
