# 支持的自动扩缩类型

Knative Serving 支持 Knative Pod Autoscaler (KPA) 和 Kubernetes的Pod水平自动扩缩 (HPA)。 本页面列出这些自动扩缩类型的特点和限制，以及如何配置它们。

!!! 重要
    如果你想要的使用HPA, 你必须在安装Knative Serving之后安装它.
关于如何安装HPA, 参阅 [安装可选的Serving扩展](../../install/yaml-install/serving/install-serving-with-yaml.md#install-optional-serving-extensions).

## Knative Pod Autoscaler (KPA)

* Knative Serving 核心的一部分，安装 Knative Serving 后默认启用。
* 支持缩容到零的功能。
* 不支持基于 CPU 的自动扩缩。

## Pod水平自动扩缩 (HPA)

* 不是 Knative Serving 核心的一部分，您必须先安装 Knative Serving。
* 不支持缩容到零的功能。
* 支持基于 CPU 的自动扩缩。

## 配置使用哪种类型的自动扩缩

自动扩缩的类型(KPA or HPA) 可以通过`class`配置。

* **全局设置使用键:** `pod-autoscaler-class`
* **对每个版本设置使用键:** `autoscaling.knative.dev/class`
* **可选值:** `"kpa.autoscaling.knative.dev"` or `"hpa.autoscaling.knative.dev"`
* **默认:** `"kpa.autoscaling.knative.dev"`

**例子:**

=== "对每个版本设置"
    ```yaml
    apiVersion: serving.knative.dev/v1
    kind: Service
    metadata:
      name: helloworld-go
      namespace: default
    spec:
      template:
        metadata:
          annotations:
            autoscaling.knative.dev/class: "kpa.autoscaling.knative.dev"
        spec:
          containers:
            - image: gcr.io/knative-samples/helloworld-go
    ```

=== "全局设置 (ConfigMap)"
    ```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
     name: config-autoscaler
     namespace: knative-serving
    data:
     pod-autoscaler-class: "kpa.autoscaling.knative.dev"
    ```

=== "全局设置 (Operator)"
    ```yaml
    apiVersion: operator.knative.dev/v1alpha1
    kind: KnativeServing
    metadata:
      name: knative-serving
    spec:
      config:
        autoscaler:
          pod-autoscaler-class: "kpa.autoscaling.knative.dev"
    ```

### Global versus per-revision settings

Configuring for autoscaling in Knative can be set using either global or per-revision settings.

1. If no per-revision autoscaling settings are specified, the global settings will be used.
1. If per-revision settings are specified, these will override the global settings when both types of settings exist.

#### Global settings

Global settings for autoscaling are configured using the `config-autoscaler` ConfigMap. If you installed Knative Serving using the Operator, you can set global configuration settings in the `spec.config.autoscaler` ConfigMap, located in the `KnativeServing` custom resource (CR).

##### Example of the default autoscaling ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
 name: config-autoscaler
 namespace: knative-serving
data:
 container-concurrency-target-default: "100"
 container-concurrency-target-percentage: "0.7"
 enable-scale-to-zero: "true"
 max-scale-up-rate: "1000"
 max-scale-down-rate: "2"
 panic-window-percentage: "10"
 panic-threshold-percentage: "200"
 scale-to-zero-grace-period: "30s"
 scale-to-zero-pod-retention-period: "0s"
 stable-window: "60s"
 target-burst-capacity: "200"
 requests-per-second-target-default: "200"
```

#### Per-revision settings

Per-revision settings for autoscaling are configured by adding _annotations_ to a revision.

**Example:**

```yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: helloworld-go
  namespace: default
spec:
  template:
    metadata:
      annotations:
        autoscaling.knative.dev/target: "70"
```

!!! important
    If you are creating revisions by using a service or configuration, you must set the annotations in the _revision template_ so that any modifications will be applied to each revision as they are created. Setting annotations in the top level metadata of a single revision will not propagate the changes to other revisions and will not apply changes to the autoscaling configuration for your application.
