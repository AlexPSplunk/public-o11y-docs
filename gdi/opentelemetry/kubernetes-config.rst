.. _otel-kubernetes-config:

*********************************************************************************
Configure Helm for Kubernetes
*********************************************************************************

.. meta::
      :description: Optional configurations for the Splunk Distribution of OpenTelemetry Collector for Kubernetes.

After you've :ref:`installed the Collector for Kubernetes <otel-install-k8s>`, these are the available settings you can configure. Additionally, see also :ref:`the advanced configuration options <otel-kubernetes-config-advanced>` such as :ref:`configuring Prometheus <otel-kubernetes-config-resources>`.

The :new-page:`values.yaml <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/helm-charts/splunk-otel-collector/values.yaml>` lists all supported configurable parameters for the Helm chart, along with a detailed explanation of each parameter. Review ``values.yaml`` to understand how to configure this chart.

The Helm chart can also be configured to support different use cases, such as trace sampling and sending data through a proxy server. See :new-page:`Examples of chart configuration <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/examples/README.md>` for more information.

.. _otel-kubernetes-config-distro:

Configure the Kubernetes distribution
============================================

If applicable, use the ``distribution`` parameter to provide information about the underlying Kubernetes deployment. This parameter allows the connector to automatically scrape additional metadata. The following options are supported:

* ``aks`` for Azure AKS
* ``eks`` for Amazon EKS
* ``eks/fargate`` for Amazon EKS with Fargate profiles
* ``gke`` for Google GKE or Standard mode
* ``gke/autopilot`` for Google GKE or Autopilot mode
* ``openshift`` for Red Hat OpenShift

Apply the following to configure your distribution:

.. code-block:: bash

  # aks deployment
  --set distribution=aks,cloudProvider=azure 

  # eks deployment
  --set distribution=eks,cloudProvider=aws 

  # eks/fargate deployment (with recommended gateway)
  --set distribution=eks/fargate,gateway.enabled=true,cloudProvider=aws 

  # gke deployment
  --set distribution=gke,cloudProvider=gcp 

  # gke/autopilot deployment
  --set distribution=gke/autopilot,cloudProvider=gcp 

  # openshift deployment (openshift can run on multiple cloud providers, so cloudProvider is excluded here)
  --set distribution=openshift   

For example:

.. code-block:: yaml

  splunkObservability:
    accessToken: xxxxxx
    realm: us0
  clusterName: my-k8s-cluster
  distribution: gke   

.. _otel-kubernetes-config-environment:

Configure the deployment environment
===========================================

If applicable, use the ``environment`` parameter to specify an additional ``deployment.environment`` attribute to be added to all telemetry data. This attribute helps Observability Cloud users investigate data coming from different sources separately. Example values include ``development``, ``staging``, and ``production``.

.. code-block:: yaml

  splunkObservability:
    accessToken: xxxxxx
    realm: us0
  environment: production

.. _otel-kubernetes-config-cloud:

Configure a cloud provider
=================================

If applicable, use the ``cloudProvider`` parameter to provide information about your cloud provider. The following options are supported:

* ``aws`` for Amazon Web Services
* ``gcp`` for Google Cloud Platform
* ``azure`` for Microsoft Azure

To set your cloud provider and configure ``cloud.platform`` for the resource detection processor, use: 

.. code-block:: bash

  --set cloudProvider={azure|gcp|eks|openshift} 

For example:

.. code-block:: yaml

  splunkObservability:
    accessToken: xxxxxx
    realm: us0
  clusterName: my-k8s-cluster
  cloudProvider: aws

Deactivate particular types of telemetry
============================================

By default, OpenTelemetry sends only metrics and traces to Observability Cloud and sends only logs to Splunk Platform. You can activate or deactivate any kind of telemetry data collection for a specific destination. For example, with the following configuration, the Collector sends all collected telemetry data to Observability Cloud and Splunk Platform, assuming you've properly configured them.

.. code-block:: yaml

   splunkObservability:
     metricsEnabled: true
     tracesEnabled: true
     logsEnabled: true
   splunkPlatform:
     metricsEnabled: true
     logsEnabled: true

Configure Windows worker nodes
===============================================

The Splunk Distribution of OpenTelemetry Collector for Kubernetes supports collecting metrics, traces, and logs (using OpenTelemetry native logs collection only) from Windows nodes. All Windows images are available in the ``quay.io/signalfx/splunk-otel-collector-windows`` repository.

Use the following values.yaml configuration to install the Helm chart on Windows worker nodes:

.. code-block:: yaml

   isWindows: true
   image:
     otelcol:
       repository: quay.io/signalfx/splunk-otel-collector-windows
   logsEngine: otel
   readinessProbe:
     initialDelaySeconds: 60
   livenessProbe:
     initialDelaySeconds: 60

If you have both Windows and Linux worker nodes in your Kubernetes cluster, you need to install the Helm chart twice. One of the installations with the default configuration set to ``isWindows: false`` is applied on Linux nodes. The second installation with the values.yaml configuration (shown in the previous example) is applied on Windows nodes.

Deactivate the ``clusterReceiver`` on one of the installations to avoid cluster-wide metrics duplication. To do this, add the following lines
to the values.yaml configuration of one of the installations:

.. code-block:: yaml

   clusterReceiver:
     enabled: false

Configure Google Kubernetes Engine Autopilot
===========================================================

To run the Collector in Google Kubernetes Engine Autopilot mode, set the ``distribution`` option to ``gke/autopilot``, as shown in the following example:

.. code-block:: yaml

   distribution: gke/autopilot

Search for "Autopilot overview" on the :new-page:`Google Cloud documentation site <https://cloud.google.com/docs>` for more information.

.. note::
  Native OpenTelemetry logs collection is not yet supported in Google Kubernetes Engine Autopilot mode.

The Collector agent daemonset can have problems scheduling in Autopilot mode. If this happens, do the following to assign the daemonset a higher priority class to ensure that the daemonset pods are always present on each node:

1. Create a new priority class for the Collector agent:

  .. code-block:: yaml

    cat <<EOF | kubectl apply -f -
    apiVersion: scheduling.k8s.io/v1
    kind: PriorityClass
    metadata:
      name: splunk-otel-agent-priority
    value: 1000000
    globalDefault: false
    description: "Higher priority class for the Splunk Distribution of OpenTelemetry Collector pods."
    EOF

2. Use the created priority class in the helm install/upgrade command using the ``--set="priorityClassName=splunk-otel-agent-priority"`` argument, or add the following line to your custom values.yaml:

  .. code-block:: yaml

    priorityClassName: splunk-otel-agent-priority


.. _config-eks-fargate:

Configure EKS Fargate
===============================

To run the Collector in the Amazon Elastic Kubernetes Service with Fargate profiles, set the required ``distribution`` value to ``eks/fargate``, as shown in the following example:

.. code-block:: yaml

   distribution: eks/fargate

.. note::
   Fluentd and native OpenTelemetry logs collection are not yet automatically configured in EKS with Fargate profiles.

This distribution operates similarly to the ``eks`` distribution, but with the following distinctions:

* The Collector agent daemonset is not applied since Fargate does not support daemonsets. Any desired Collector instances running as agents must be configured manually as sidecar containers in your custom deployments. This includes any application logging services like Fluentd. Set ``gateway.enabled`` to ``true`` and configure your instrumented applications to report metrics, traces, and logs to the gateway ``<installed-chart-name>-splunk-otel-collector`` service address. Any desired agent instances that would run as a daemonset should instead run as sidecar containers in your pods.
* Since Fargate nodes use a VM boundary to prevent access to host-based resources used by other pods, pods are not able to reach their own kubelet. The cluster receiver for the Fargate distribution has two primary differences between regular ``eks`` to work around this limitation:
   * The configured cluster receiver is deployed as a two-replica StatefulSet instead of a Deployment, and uses a Kubernetes Observer extension that discovers the cluster's nodes and, on the second replica, its pods for user-configurable receiver creator additions.Using this observer dynamically creates the Kubelet Stats receiver instances that report kubelet metrics for all observed Fargate nodes. The first replica monitors the cluster with a ``k8s_cluster`` receiver, and the second cluster monitors all kubelets except its own (due to an EKS/Fargate networking restriction).
   * The first replica's Collector monitors the second's kubelet. This is made possible by a Fargate-specific ``splunk-otel-eks-fargate-kubeletstats-receiver-node`` node label. The Collector ClusterRole for ``eks/fargate`` allows the ``patch`` verb on ``nodes`` resources for the default API groups to allow the cluster receiver's init container to add this node label for designated self monitoring.

Override the underlying OpenTelemetry agent configuration
==============================================================

You can override the underlying OpenTelemetry agent configuration to use your own OpenTelemetry Agent configuration. To do this, include a custom configuration in the ``agent.config`` parameter in the values.yaml configuration. This custom configuration is merged into the default agent configuration. Parts of the configuration (for example, ``service``, ``pipelines``, ``logs``, and ``processors`` need to be fully re-defined after the files are merged.

The following example shows a ``values.yaml`` file with custom gateway values:

.. code-block:: yaml

   clusterName: my-cluster
   splunkObservability:
     realm: us0
     accessToken: my-access-token

   agent:
     config:
       exporters:
         otlp:
           endpoint: <custom-gateway-url>:4317
           insecure: true
         signalfx:
           ingest_url: http://<custom-gateway-url>:9943
           api_url: http://<custom-gateway-url>:6060
       service:
         pipelines:
           traces:
             exporters: [otlp, signalfx]
           metrics:
             exporters: [otlp]
           logs:
             exporters: [otlp]

   clusterReceiver:
     config:
       exporters:
         signalfx:
           ingest_url: http://<custom-gateway-url>:9943
           api_url: http://<custom-gateway-url>:6060
