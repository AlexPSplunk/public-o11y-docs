.. _apm-index-span-tags:

************************************************************
Index span tags to generate Troubleshooting MetricSets
************************************************************

.. Metadata updated: 1/23/23

.. meta::
   :description: Learn how to use index tags to create Troubleshooting MetricSets that help you troubleshoot services with Splunk Observability Cloud.

Thanks to Splunk APM full-fidelity tracing, which captures every span from every trace, you can use span tags to break down services and inter-service calls along any characteristic or attribute associated with any given trace. This lets you customize data visualizations and metrics for your monitoring and troubleshooting requirements. 

To get additional value from a particular span tag, a Splunk APM administrator can run an action known as indexing, which enables additional automatic analysis of the indexed span tag. One benefit of this action is to get aggregated metrics, or MetricSets, across all spans that contain a specified indexed tag.

What are MetricSets?
=========================

MetricSets are metric time series you can use to track the performance of specific metrics over time. To help you analyze your service performance, APM provides two kinds of MetricSets: Troubleshooting MetricSets and Monitoring MetricSets. Troubleshooting MetricSets are for high-cardinality troubleshooting, while Monitoring MetricSets leverage the Infrastructure Monitoring platform for real-time monitoring and alerting. 

APM provides a collection of MetricSets by default, and you can index additional span tags to generate additional Troubleshooting MetricSets, and optionally add additional custom dimensionalized Monitoring MetricSets. See :ref:`apm-MetricSets` for an overview of the types of MetricSets in APM and a list of the MetricSets you receive by default. 

Use this topic to learn how to index a span tag and generate additional Troubleshooting MetricSets. Then see :ref:`cmms` to learn how to add custom dimensionalized Monitoring MetricSets. 

Prerequisites
====================================================================================

To index new span tags and generate Troubleshooting MetricSets, you need the following:

* Have an administrator in your organization to index tags and create Troubleshooting MetricSets.
* (Optional) Before you start indexing span tags, see :ref:`apm-index-tag-tips` for guidance on choosing span tags to index. 

.. _apm-tms-details:

Troubleshooting MetricSets
=================================================================

Every Troubleshooting MetricSet generates the following metrics, also known as Request, Error, and Duration (RED) metrics. The following metrics appear when you select a service from the :ref:`service map <service-map>` in the :strong:`Troubleshooting` view:

- Request rate
- Error rate
- Root cause error rate
- p50, p90, and p99 latency

The measurement precision of Troubleshooting MetricSets is 10 seconds. Splunk APM reports quantiles from a distribution of metrics for each 10-second reporting window. 

Automatically indexed span tags
--------------------------------

Splunk APM automatically indexes and generates Troubleshooting MetricSets for the following span tags:

  - Environment
  - Endpoint
  - Operation
  - HTTP Method
  - Kind
  - Service

For more details about each of these tags, see :ref:`apm-default-span-tags`. You can't modify or stop APM from indexing these span tags, but you can index additional span tags. See :ref:`index-span-tags-instructions` to learn how. 

Cardinality contribution of indexed span tags
------------------------------------------------------------

When you index a new span tag to generate Troubleshooting MetricSets, Splunk APM runs a cardinality contribution analysis to calculate the potential total cardinality contribution after indexing the span tag. This gives you control of what you index and helps account for any limits you have to stay within.

If you try to index a span tag that might increase the total cardinality contribution beyond your limit, you can change the existing cardinality contribution of indexed tags by modifying or removing indexed span tags.

.. _index-span-tags-instructions:

Index a new span tag
=======================

Follow these steps to index a span tag and create a Troubleshooting MetricSet for it. 

1. To open the :guilabel:`APM MetricSets` page, do one of the following: 
  
   a. In Splunk APM, select :guilabel:`APM Configuration` and select :guilabel:`APM MetricSets` from the menu. The APM MetricSets page opens.
   b. From anywhere in Splunk Observability Cloud, select :guilabel:`Settings` in the left navigation bar and select :guilabel:`APM MetricSets` under :guilabel:`Data Configuration`.   

2. On the :guilabel:`APM MetricSets` page, select :strong:`New MetricSet`.

3. Enter the :strong:`Name` of a span tag you want to index.

4. The :strong:`Scope` determines how APM associates the span tag with services in a trace:

   - Select :strong:`Service` to associate the span tag with services. This means the value of the span tag might change across services in a given trace. Specify ``All Services`` to index the span tag for every service. Select specific services to index the span tag for only those services.

   - Select :strong:`Global` to associate the span tag with traces. This means the value of the span tag is the same for all services in a given trace.

   For more information about span tag scope, see :ref:`apm-index-tag-types`.

5. (Optional) If your span tag is indexed at the service level, you can also use it as a custom dimension in Monitoring MetricSets. See :ref:`cmms` for instructions.

6. Select :strong:`Start Analysis` to submit the configuration. When you submit a configuration, Splunk APM runs an analysis of the span tag to calculate the potential cardinality contribution of indexing it and determine whether it generates Troubleshooting MetricSets that exceed your limit.

7. Wait a few moments for the cardinality check to run, and then check under :strong:`Pending MetricSet` to view the status of the span tag you're trying to index. See the following table for possible status options for pending MetricSets and actions for each status. 

   .. list-table::
      :header-rows: 1
      :widths: 15, 40, 45

      * - :strong:`Status`
        - :strong:`Description`
        - :strong:`Next step`

      * - Analyzing
        - The application is currently running the cardinality contribution analysis. When this is the status for a span tag you want to index, you can't create or modify any other span tags.
        - Wait until the cardinality contribution analysis is complete. 

      * - Ready
        - The cardinality contribution analysis is complete: you can index the span tag without any issue.
        - Select the checkmark in the :guilabel:`Actions` column to manually index the span tag and start generating Troubleshooting MetricSets that include the span tag. 

      * - Failed
        - The cardinality contribution analysis is complete, but you can't index the span tag because you reached an entitlement or system limit.
        - Consider pausing or deleting existing Custom MetricSets to open space for another indexed span tag, or reach out to your Splunk Observability Cloud account manage to increase your account limit. See :ref:`apm-limits-metricsets` to learn more about these limits. 

      * - Timeout
        - If more than one hour passes for a pending MetricSet in a ``Ready`` status, the status changes to ``Timeout``. 
        - Rerun the analysis to try indexing the span tag again. 

.. _manage-TMS:

Manage existing indexed span tags and Troubleshooting MetricSets
=================================================================

After you've successfully indexed a span tag, Splunk APM saves the configuration in the :strong:`APM MetricSets` page in :strong:`Data Configuration`. Go to this page to view the indexing scope of the span tag and its current status.

You can modify the configuration for existing indexed tags, including adding and removing services for specific indexed tags and modifying the scope. You can also pause or stop indexing without deleting their configuration. This is useful when you want to temporarily stop indexing a span tag, but don't want to remove the configuration.

To review or modify existing indexed span tags, do the following:

1. Go to :strong:`Settings > Data Configuration > APM MetricSets` 
2. Find the indexed span tag you want to view under the :strong:`Custom MetricSets` section of the configuration table.
3. See :ref:`tms-status` to interpret the status of each indexed span tag.
4. Make any desired changes using the buttons in the :guilabel:`Actions` column:

    - Use the pencil button to edit the scope of an indexed span tag.
    - Use the pause button to pause generating MetricSets for a given span tag. 
    - Use the trash button to delete a MetricSet configuration.

.. _tms-status:

Status of configured Troubleshooting MetricSets
-------------------------------------------------

After you've configured custom Troubleshooting MetricSets you can see their status. The following table lists all available statuses:

.. list-table::
   :header-rows: 1
   :widths: 15, 85

   * - :strong:`Status`
     - :strong:`Description`

   * - Active
     - The application is indexing the span tag and generating Troubleshooting MetricSets for it.

   * - Paused
     - You or another administrator paused indexing for the span tag. The application isn't generating Troubleshooting MetricSets for the span tag, and you can't view any data you previously indexed for it.
    
   * - Stopped
     - The application stopped indexing the span tag and is no longer generating Troubleshooting MetricSets for the span tag. You can't view any data you previously indexed for this span tag.

.. _db-metricsets:

Status of Database MetricSets
--------------------------------------------------

You can pause and resume indexing of Database Query Performance data at any time from the :guilabel:`Standard MetricSets` section of :guilabel:`APM MetricSets`. You can also control the indexing of SQL and NoSQL tags separately.

To pause the indexing of Database Query Performance data, select :guilabel:`Pause indexing` next to the database type. To resume, select :guilabel:`Resume indexing`.


