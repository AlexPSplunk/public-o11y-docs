.. _autodetect:

******************************************************
AutoDetect in Splunk Observability Cloud
******************************************************

.. meta::
   :description: AutoDetect alerts and detectors are read-only alerts and detectors that Splunk Observability Cloud automatically creates when you have supported integrations configured. This topic describes how to work with AutoDetect alerts and detectors.

AutoDetect alerts and detectors are read-only alerts and detectors that Splunk Observability Cloud automatically creates when you have supported integrations configured.

AutoDetect detectors don't count toward the maximum number of detectors you can have in your organization.

Prerequisites
==============================
To use AutoDetect alerts and detectors, you must first connect and send in data for supported integrations. Supported integrations include Amazon Relational Database (Amazon RDS), Kafka, and Kubernetes. AutoDetect detectors are also configured for some system settings of Splunk Observability Cloud.

See the following topics for more information on how to set up each integration:

- :ref:`Amazon <get-started-aws>`
- :new-page-ref:`Kafka <kafka>`
- :ref:`Kubernetes <get-started-k8s>`


Use AutoDetect alerts and detectors
===========================================

Once you set up the supported integrations, AutoDetect alerts and detectors automatically appear on the :strong:`Alerts` page, the :strong:`Detectors` page, and the corresponding navigators for your integrations. 

.. note:: By default, you are not subscribed to receive notifications from AutoDetect. To learn how to subscribe to AutoDetect notifications, see :ref:`autodetect-subscribe`.

View AutoDetect alerts and detectors
----------------------------------------

You can view AutoDetect detectors on the :strong:`Alerts` page, in a navigator for a service, or in a chart.

To view a complete list of all available AutoDetect alerts and detectors in your organization, do the following:

#. From the Splunk Observability Cloud home page, go to the :strong:`Alerts` page.
#. Select the :strong:`Active Alerts` or :strong:`Detectors` tab on the :strong:`Alerts` page. AutoDetect components are indicated by the :strong:`Auto` badge.

    .. image:: /_images/images-detectors-alerts/autodetect/autodetect-alerts-page.png
      :width: 90%
      :alt: This screenshot shows what an AutoDetect component looks like on the Alerts page.

To view AutoDetect alerts and detectors specific to a service, do the following:

#. From the Splunk Observability Cloud home page, go to the :strong:`Infrastructure` page.
#. Select the navigator you want to view.
#. On the navigator page, click :guilabel:`Alerts` or :guilabel:`Active Detectors` in the Filter bar.
    
    A sidebar with all alerts related to the content on the page appears, as shown in the following image. AutoDetect components are indicated by the :strong:`Auto` badge.

    .. image:: /_images/images-detectors-alerts/autodetect/autodetect-in-context.png
      :width: 60%
      :alt: This screenshot shows what an AutoDetect component looks like in the Alerts sidebar for a navigator.

If available, AutoDetect detectors are connected to a chart by default. To view AutoDetect detectors related to a chart, do the following:

#. From the Splunk Observability Cloud home page, go to the :strong:`Infrastructure` page or the :strong:`Dashboards` page.
#. Select the navigator or dashboard you want to view.
#. On the navigator or dashboard page, select the bell icon on a chart to see AutoDetect detectors linked to that chart. 

    A green dot indicates that there is no alert for the detector. A red dot indicates that the detector has an active alert.

    .. image:: /_images/images-detectors-alerts/autodetect/autodetect-linked-chart.png
      :width: 60%
      :alt: This screenshot shows where linked AutoDetect detectors are listed for a chart. In this example, there is one AutoDetect detector with a green dot, meaning there is no active alert.

.. _autodetect-subscribe:

Subscribe to AutoDetect notifications
---------------------------------------------------------

The procedures for subscribing to AutoDetect alerts and detectors are the same as those for interacting with other alerts and detectors.

To learn how to subscribe to a detector or alert for notifications, see :ref:`manage-notifications`.

.. _autodetect-customize:

Copy and customize an AutoDetect detector
---------------------------------------------------------

When you customize an AutoDetect detector, Splunk Observability Cloud creates a copy of the original detector for you to apply the customizations.

- Any changes you make to the customized detector don't apply to the original AutoDetect detector.
- Customized detectors created from AutoDetect detectors count toward the maximum numbers of detectors your organization can have. To learn more about detectors limit, see :ref:`maximum-number-of-detectors-per-org`.
- The default limit for customized detectors per AutoDetect detector is 15. If you want to increase this limit, contact support for help.

To customize a copy of an AutoDetect detector, do the following:

#. From the Splunk Observability Cloud home page, go to the :strong:`Alerts` page.
#. Select the :strong:`Detectors` tab on the :strong:`Alerts` page.
#. In the search field, enter the name of the detector you want to customize.
    
    For example, to search for the ``K8s Node Memory Utilization is high`` detector, start by typing ``K8s Node``. The result lists update automatically as you type.

    .. image:: /_images/images-detectors-alerts/autodetect/autodetect-search.png
      :width: 80%
      :alt: This screenshot shows what an searching for an AutoDetect looks like on the Alerts page.

#. Click the detector you want to customize to open it.
#. Click :guilabel:`Create a Customized Version`.

    .. image:: /_images/images-detectors-alerts/autodetect/autodetect-disable-customize.png
      :width: 60%
      :alt: This screenshot shows the position of the Create a Customized Version button.

#. Make customizations you want to the detector. For the full list of customizable arguments for each AutoDetect detector, see :ref:`autodetect-list`.
#. Rename your customized detector to distinguish it from the original detector and any other copy.
#. Click :guilabel:`Activate`.

Customized detectors created from AutoDetect detectors are indicated by the :strong:`Custom` badge.

    .. image:: /_images/images-detectors-alerts/autodetect/autodetect-custom.png
      :width: 90%
      :alt: This screenshot shows a customized detector indicated by the Custom badge.


Mute AutoDetect alerts and detectors
---------------------------------------------------------

The procedures for muting AutoDetect alerts and detectors are the same as those for interacting with other alerts and detectors.

To learn how to create muting rules for alerts and detectors, see :ref:`mute-notifications`.

Disable AutoDetect detectors
---------------------------------------------------------

To disable an AutoDetect detector, do the following:

#. From the Splunk Observability Cloud home page, go to the :strong:`Alerts` page.
#. Select the :strong:`Detectors` tab on the :strong:`Alerts` page.
#. In the search field, enter the name of the detector you want to disable.
    
    For example, to search for the ``K8s Node Memory Utilization is high`` detector, start by typing ``K8s Node``. The result lists update automatically as you type.

    .. image:: /_images/images-detectors-alerts/autodetect/autodetect-search.png
      :width: 80%
      :alt: This screenshot shows what an searching for an AutoDetect looks like on the Alerts page.

#. Click the detector you want to disable to open it.
#. Click :guilabel:`Disable Detector`.

    .. image:: /_images/images-detectors-alerts/autodetect/autodetect-disable-customize.png
      :width: 60%
      :alt: This screenshot shows the position of the Disable Detector button.

Once you disable a detector, you can no longer edit it. You need to re-enable a disabled detector before making new updates.


