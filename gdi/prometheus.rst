.. _prometheus:

********************************************************************************
Configure application exporters and receivers with Prometheus metrics
********************************************************************************

.. meta::
   :description: Landing for application receivers for monitoring applications in Splunk Observability Cloud.
   
.. toctree::
   :maxdepth: 4
   :hidden:

   monitors-prometheus/prometheus-generic
   opentelemetry/components/prometheus-receiver
   monitors-prometheus/prometheus-exporter
   monitors-prometheus/prometheus-go
   monitors-prometheus/prometheus-nginx-ingress
   monitors-prometheus/prometheus-nginx-vts
   monitors-prometheus/prometheus-node
   monitors-prometheus/prometheus-velero

You can use the OpenTelementry native :ref:`prometheus-receiver` to gather metrics from any software, tool or service that exposes metrics in the Prometheus format, such as Ansible Tower (AWX), CockroachDB, Docker Daemon, Doorman, Etcd, Flink, Grafana, PATROL, Traefik, or Zipkin. 

* :ref:`prometheus-generic`
* :ref:`prometheus-receiver`
* See a complete list of third-party applications compatible with Prometheus in :new-page:`Prometheus' official documentation <https://prometheus.io/docs/instrumenting/exporters/>`. 

Splunk Observability Cloud also offers the following Prometheus integrations:

* :ref:`prometheus-exporter`
* :ref:`prometheus-go`
* :ref:`prometheus-nginx-ingress`
* :ref:`prometheus-nginx-vts`
* :ref:`prometheus-node`
* :ref:`prometheus-velero`

