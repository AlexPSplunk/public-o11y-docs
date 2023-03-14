.. _cmms:

*******************************************************
Generate a Monitoring MetricSet with a custom dimension
*******************************************************

.. meta::
   :description: Learn how to generate a Monitoring MetricSet with a custom dimension. 

Generate Monitoring MetricSets (MMS) with custom dimensions to glean meaningful insights about your services in real time. Splunk APM automatically provides a set of Monitoring MetricSets by default, but you can configure additional MetricSets using indexed span tags as custom dimensions. You can use your Monitoring MetricSets to create custom charts, dashboards, and detectors to keep track of what matters most to your team.


How custom Monitoring MetricSets add value to your organization
======================================================================

APM generates a collection of Monitoring MetricSets (MMS) by default. See :ref:`mms-dimensions` for that list. By creating Monitoring MetricSets with custom dimensions, you can filter and aggregate the generated metrics by a specific indexed span tag such as ``version.id``, ``version``, or ``cloud.provider``.

You can create custom Monitoring MetricSets at the service level and optionally at the endpoint (span) level. When you create a custom dimension for a service-level MMS, APM generates MMS that include the 6 service-level metrics listed in :ref:`service-mms`, with your chosen indexed span tag as a custom dimension. If you select to add endpoint-level metrics as well, APM generates MMS that include the 6 span-level metrics listed in :ref:`endpoint-mms`, with your chosen indexed span tag as a custom dimension. 

When you create a Monitoring MetricSet with a custom dimension, you can use this custom dimension to create charts, dashboards, and alerts for the Infrastructure Monitoring platform. 

To learn more about a specific use case for custom Monitoring MetricSets, see :ref:`custom-metricset`.

Prerequisites 
==============

* In order to create a Monitoring MetricSet based on a span tag, you need to first index the span tag. See :ref:`apm-index-span-tags`. 

* You need to be an Administrator. 

* (Optional) If you are unfamiliar with Monitoring Metric Sets and Troubleshooting Metric Sets, see:  :ref:`apm-metricsets`. 

* (Optional) Review :ref:`metrics-dimensions-mts` for an overview of the concepts that make up MetricSets. 


How to generate a Monitoring MetricSet with a custom dimension 
====================================================================

When you generate a Monitoring MetricSet, the MetricSet configuration moves to the pending state where APM checks the cardinality of your data to see if it falls within your organization's limit . Before you generate a Monitoring MetricSet, first consider how you want to allocate the cardinality of your data to make your Monitoring MetricSets most efficient.

If the cardinality check fails, the new MetricSet exceeds the cardinality limit for your organization. See :ref:`troubleshoot-mms` for guidance on editing your MetricSet configuration to optimize your overall cardinality. If the cardinality of your MetricSet is within your limits then you can activate the MetricSet. 

.. _mms-conf:

Select how you want to configure your Monitoring MetricSet
------------------------------------------------------------------

You can generate a Monitoring MetricSet in three ways:

* service-level MMS only
* endpoint-level MMS for specific endpoints within a given service
* orient both service-level MMS and endpoint-level MMS for all endpoints within a service

Choose the best option for your needs to allocate your organization's cardinality entitlement efficiently. See :ref:`reduce-cardinality` to learn more. 

You can also filter by specific tag values to further reduce the cardinality of your Monitoring MetricSet. All custom MMS include service level metrics. In addition, you can generate endpoint level metrics.


.. list-table::
   :header-rows: 1
   :widths: 15 50

   * - :strong:`Option`
     - :strong:`Summary`
   * - Service-level MMS and endpoint-level MMS for all endpoints within a service
     - Creates a custom dimensionalized MMS for each of the selected service, as well as an MMS for each endpoint in each selected service. This is the highest cardinality option. 
   * - Service-level MMS and endpoint-level MMS for specific endpoints
     - Creates custom dimensionalized MMS for each of the selected service and an MMS for specific endpoints you select. In the box labeled :guilabel:`Endpoint Filters`, you can provide a list of endpoints or a regular expression pattern to generate MMS for those endpoints.
   * - Service-level MMS only
     - Creates an MMS for each of the selected service and no endpoint-level MMS. This is the lowest cardinality option. 


Scope of Monitoring MetricsSets 
---------------------------------------------------------------

You can create custom MMS for endpoints (metrics starting with ``span*``) and services (metrics starting with ``service.request.*``), but not for workflows (metrics starting with ``workflow*``) or traces (metrics starting with ``trace*``) at this time. Custom MMS aren't supported for global tags.

Add a Monitoring MetricSet
------------------------------

Follow these steps to create a Monitoring MetricSet. 

1. Navigate to the APM MetricSets configuration page. There are two ways to get there: 
    
    a. From the left navigation panel, select: :strong:`APM` > :strong:`APM Configurations` > :strong:`APM MetricSets`.
    b. From the APM landing page, click :strong:`APM Configuration` and select :strong:`APM MetricSets`. 

2. Once in the MetricSets Configuration page, you have two options:
    
    a. If you have already indexed the span tag you are interested in, it appears in the list of MetricSets and is already generating Troubleshooting MetricSets. Select the edit icon for that span tag to open the :guilabel:`Edit MetricSet` dialog box and add a Monitoring MetricSet to your configuration using the following steps. 
    b. If you haven't already indexed the tag, follow steps 1-6 in :ref:`Index a new span tag <index-span-tags-instructions>`, then continue with the following steps in the :guilabel:`Add MetricSet` dialog box.

3. In the :guilabel:`Service` field, enter the service or services for which you want to create a Monitoring MetricSet. You can only create custom dimensionalized MMS for service-level indexed span tags. You can't create custom MMS for globally indexed span tags. 

4. In the :guilabel:`Add MetricSet` or :guilabel:`Edit MetricSet` dialog box, select the check box for :strong:`Also Create Monitoring MetricSet`.

      .. image:: /_images/apm/span-tags/cmms-modal.png
        :width: 70%
        :alt: This image shows the MetricSet creation dialog box. 


5. Select how you want to add tag data to your Monitoring MetricSet from the dropdown. See :ref:`mms-conf` for more details. 
    a. :guilabel:`Service and all endpoint MMS:` Create an MMS for each of the selected service, as well as an MMS for each endpoint in each selected service. 
    b. :guilabel:`Service and specific endpoint MMS:` Create an MMS for each of the selected service and an MMS for specific endpoints you select. To add :guilabel:`Endpoint Filters`, provide a list of endpoints or a regular expression pattern to generate MMS for specific endpoints you're interested in. 
    c. :guilabel:`Service MMS only:` Create an MMS for each of the selected service and no endpoint-level MMS. 

6. (Optional) In the field labeled :guilabel:`Filter by tag values`, add tag filters to narrow the scope of your MMS to include only data associated with the tag values you enter. This can help further reduce cardinality in your MetricSets.

7. Select :strong:`Start Analysis` and wait for the cardinality check to run. See :ref:`review-cardinality` to assess the new MetricSet's cardinality contribution and enable the MetricSet if you're happy with it.  

.. note:: Splunk APM replaces dots with underscores in dimension names for MMS time series.

.. _review-cardinality: 

Review the cardinality of your MetricSet
-------------------------------------------

When the cardinality check completes, you can see the estimated total cardinality.

 * If you are happy with the cardinality, select the check mark under :strong:`Actions` to enable your Monitoring MetricSet. The cardinality check expires after one hour. 
 * If the check fail to run, run the check again. 
 * If the check runs and returns a failure, your cardinality is too high to create the new MMS. See :ref:`troubleshoot-mms` to adjust accordingly.
 * After you accept or discard the pending MetricSet, you can continue editing your other MetricSets. 

The following example shows a cardinality check. 

.. image:: /_images/apm/span-tags/cardinality-check-APM.png
   :width: 60%
   :alt: This image shows the cardinality check for a Monitoring MetricSet. 
   

Set up charts, dashboards, and alerts for custom dimensions
==================================================================
You can create charts, dashboards, and alerts based on your custom Monitoring MetricSet by leveraging the Infrastructure Monitoring platform. 

To use the custom dimensionalized Monitoring MetricSets you have created, apply the filter ``sf_dimensionalized:true``. This filters out the metrics generated by the default Monitoring Metricset. To filter your metrics even more, use the new dimension you have created which is the tag name.

.. list-table::
   :header-rows: 1
   :widths: 15, 50

   * - :strong:`Task`
     - :strong:`Instructions`
   * - Create charts
     - :ref:`create-charts`
   * - Create dashboards
     - :ref:`dashboard-create-customize`
   * - Create an alert 
     - :ref:`apm-alerts`
   * - Monitor services in APM dashboards 
     - :ref:`apm-dashboards`
