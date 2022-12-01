.. _gcp-infra-monitor:

*********************************************
Monitor Google Cloud Platform services
*********************************************

.. meta::
  :description: Get started monitoring GCP infrastructure resources with Splunk Observability Cloud.

You can monitor the following :ref:`Google Cloud Platform (GCP) services <gcp-integrations>` in Splunk Observability Cloud, which provides infrastructure monitoring features using Google Cloud Operations. See the Google Cloud documentation for more information.

You can also export and monitor data from these sources running in your GCP environment, as described in the table.

.. list-table::
  :header-rows: 1
  :widths: 30, 20, 50

  * - :strong:`Get data in`
    - :strong:`Monitor`
    - :strong:`Description`

  * - :ref:`get-started-k8s`
    - :ref:`infrastructure-k8s`
    - Collect metrics and logs from Kubernetes clusters running in Google Compute Engine (GCE) or Google Kubernetes Engine (GKE) instances.

  * - - :ref:`get-started-linux`
      - :ref:`get-started-windows`
    - :ref:`infrastructure-hosts`
    - Collect metrics and logs from Linux and Windows hosts running in GCE instances.

  * - :ref:`get-started-application`
    - :ref:`get-started-apm`
    - Collect application metrics and spans running in hosts or Kubernetes clusters.

.. _monitor-gcp-services:

Monitor GCP services and identify problems
=====================================================

View the health of GCP services at a glance from the Infrastructure page. This page provides a key metric for each service. You can also drill down into specific instances of a GCP service. For example, view key metrics for the GCE service, and filter for a specific ID to analyze a particular GCE instance.

Follow these steps to analyze problems with GCP services from the Infrastructure page:

  1. Select :strong:`Navigation menu > Infrastructure` and view the :strong:`Google Cloud Platform` category.

  2. Select the specific service you want to analyze. For example, select :strong:`Cloud Storage` to view metrics for a specific bucket. If you see “No Data Found,” you need to first configure an integration.

  3. Compare instances of the service along the following metrics with the :strong:`Color by` drop-down list. In the heat map, colors represent the health of instances based on the metrics you select. For example, a heat map that shows green and red, uses green to denote healthy and red to denote unhealthy instances. If your heat map has multiple colors, then the lighter gradient represents less activity, and the darker gradient represents more activity. 
  
  4. Optionally, you might apply visually accessible color palettes on custom dashboards and charts and throughout Infrastructure Monitoring, select :strong:`Account Settings > Color Accessibility.` You can color by metrics like CPU utilization and filter by dimensions like geographic region.
  
  5. Group instances based on metadata about each instance with the :strong:`Group by` drop-down list. You can group instances according to the region or resource group they are running in or the environment tag. Use this to see correlations between different parts of your infrastructure and its performance.

  6. Find outliers for your metrics with the :strong:`Find Outliers` setting. Specify the :strong:`Scope` and :strong:`Strategy`.
  
    - Set the :strong:`Scope` to analyze outliers from across the entire visible population of instances, or only within groups defined by the dimension or property you grouped instances by.
  
    - Select one of two :strong:`Strategies` to find outliers:
      
      - ``Deviation from Mean``: Instances appear as red that exceed the mean value of the metric by at least three standard deviations. Use this setting to find the most extreme outliers.
      - ``Deviation from Median``: Instances appear as red that exceed the median absolute deviation value by at least three absolute deviations. This setting does not weigh extreme outliers as heavily as the standard deviation.

  7. Select a specific instance you want to investigate further to view all the metadata and key metrics for the instance. For every instance, Observability Cloud provides a default dashboard. Analyze all the available metadata about the cloud service the instance is running in, the instance itself, and any custom tags associated with the instance. The default dashboard provides metric time series (MTS) for key metrics.

.. _gcp-dashboards:

Use default dashboards to monitor GCP services
=====================================================

Splunk Observability Cloud provides default dashboards for supported GCP services. Default dashboards are available in dashboard groups based on the GCP service that a dashboard represents data for.

To find default dashboards for GCP services, select :strong:`Navigation menu > Dashboards` and search for the GCP service you want to view dashboards for.

Explore built-in content
------------------------------

Observability Cloud collects data from many cloud services. To see all of the navigators provided for data collected in your organization, go to the Infrastructure page. To see all the pre-built dashboards for data collected in your organization, select :strong:`Dashboards > Built-in`.

.. note::

  GCP Compute Engine instances are powered by their respective public cloud service as well as the :ref:`Splunk Distribution of OpenTelemetry Collector <otel-intro>`. You need both for all the charts to display data in the built-in dashboards.

  - If you have only the public cloud service and the Smart Agent configured, some charts in the built-in dashboards for GCP Compute Engine instances display no data.
  - If you have only the public cloud service configured, you can see all the cards representing the services where data come from, but some charts in the built-in dashboards for GCP Compute Engine instances display no data.
  - If you have only Smart Agent configured, GCP Compute Engine instance navigator isn't available.

.. _gcp-unique-id:

Uniquely identifying Google Cloud Platform resources
=============================================================================

All of the metrics that the StackDriver integration sends contain a dimension called ``gcp_id``. The value of this dimension starts with the project ID that contains the resource followed by ``_`` (underscore) and then other properties specific to that resource. If you install collectd on a Compute Engine instance using the :new-page:`standard install script <https://github.com/signalfx/signalfx-collectd-installer>` this dimension is automatically added.

The simplest way to manually send metrics with this dimension to discover the unique ID value is to find a time series that contains this dimension using the Metadata Catalog. The time series should contain other dimensions that give a more friendly identification to the underlying GCP resource.

Dimensions
===========================================

The metric time series (MTS) associated with GCP metrics have the following generic dimensions, common to all services:

.. list-table::
  :header-rows: 1
  :width: 100

  * - :strong:`Dimension name`
    - :strong:`Description`

  * - ``gcp_id``
    - unique identifier for GCP objects

  * - ``project_id``
    - project ID of the monitored resource

  * - ``monitored_resource``
    - name of the monitored resource

  * - ``service``
    - service to which the metric belongs

Moreover, each service also has a dimension that identifies the resource to which the metric belongs to. For example, the ``instance_id`` dimension identifies compute instances, and the ``bucket_name`` dimension identifies storage buckets.

