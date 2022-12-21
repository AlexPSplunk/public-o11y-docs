.. include:: /_includes/incident_intelligence/incident-intelligence-preview-header.rst

.. _ii-create-configure-services:

************************************************************************
Create and configure services
************************************************************************

.. meta::
   :description: Steps to create and configure services to organize incidents for Incident Intelligence in Splunk Observability Cloud.

Use services to organize incidents depending on the impacted environmental component, for example, your web application or checkout service. Begin by creating a service. Then, route alerts to the service. Next, specify which alerts create an incident and how alerts are grouped into incidents. Finally, create incident workflows with escalating steps to determine who is notified to respond when a new incident is triggered.

.. _ii-create-service:

Create a service
====================

#. In Incident Intelligence, select :guilabel:`Incident Management`.
#. Select :guilabel:`Services` > :guilabel:`Create service`.
#. Give your service a unique name and a description. 
#. Select :guilabel:`Create service`.

After you create your service, you are directed to configure which alerts are routed to your service. 

.. note:: If you have more than one service, organize them in the order of their importance (top to bottom) to your infrastructure. Alerts are only routed to one service even if they match multiple services. The service that alerts are routed to is based on your service’s alert routing conditions and service rank order. To rank your services go to :guilabel:`Incident Management` > :guilabel:`Services` > :guilabel:`Service ranking`. 

.. _ii-configure-alert-routing:

Configure the alerts that are routed to your service
============================================================

Use alert routing to associate alerts with a service. If an alert matches your alert filter conditions, it is routed to the service. To set up your alert routing for the service, follow these steps:

#. In Incident Intelligence, select :guilabel:`Incident Management`.
#. Select :guilabel:`Services` and then the service you want to add alert routing conditions to.
#. Select the :guilabel:`Alert Routing` tab to see the list of alerts that are currently routed to the service.
#. To filter the alerts routed to the service, select :guilabel:`Add Filters`. 
    #. Select a filter field. Use :guilabel:`source` to route alerts based on a detector name. 
    #. Select the :guilabel:`=` (equal to) or :guilabel:`!=` (not equal to) operator.
    #. Select a filter value. 
    #. Select :guilabel:`Enter` to save your condition. 
#. Repeat these steps for any additional alert routing conditions that you want to set up. By default, multiple conditions are joined by an ``OR`` operator. To switch an ``OR`` operator to ``AND``, select the ``OR`` operator and select ``AND``.
#. Review the list of alerts that are currently routed to the service to confirm your filter conditions are correct. 
#. Select :guilabel:`Save alert routing` when you are finished setting up your alert routing conditions.

After you configure which alerts are routed to your service, configure how alerts are grouped into incidents.

.. _ii-configure-alert-grouping:

Configure how alerts are grouped
====================================

Use alert grouping to manage which alerts create an incident and how alerts are grouped into incidents. Alert grouping is specific to each service and you can customize it to create the workflow that works for you. You can use alert severity to determine if an incident is created and also group alerts by time period. To configure alert grouping, follow these steps:

#. In Incident Intelligence, select :guilabel:`Incident Management`.
#. Select :guilabel:`Services` and then the service you want to add alert grouping conditions to. Each service can have one alert grouping rule.
#. On the :guilabel:`Alert grouping` tab, select the minimum severity level you want to require for an incident to be triggered in the drop-down list next to :guilabel:`Trigger an incident when alerts reach severity level`.
#. If you want to group alerts into incidents, select :guilabel:`Group alerts from the same time period into incidents`, and then select a time period between 10 minutes and 24 hours from the drop-down list next to :guilabel:`Create a new incident if there is a pause in alerts for`.
#. Select :guilabel:`Save alert grouping`.

After you manage which alerts create an incident and how alerts are grouped into incidents, configure incident workflows for your service.  

.. _ii-configure-incident-workflows:

Configure incident workflows for your service
==================================================

Use incident workflows to determine who is notified when a new incident is triggered. To create an automatic incident workflow, add escalating steps to notify responders of the incident. To add an incident workflow, follow these steps:

#. In Incident Intelligence, select :guilabel:`Incident Management`.
#. Select :guilabel:`Services` and then the service where you want to create an incident workflow.
#. Select the :guilabel:`Incident workflows` tab. 
#. To add responders, select :guilabel:`Configure invite` under :guilabel:`Immediately`. 
#. In the :guilabel:`Configure invite` window, add responders by name or by schedule. If you don't have an on-call schedule, see :ref:`ii-create-manage-on-call-schedules`.
   
    .. list-table::
        :header-rows: 1
        :widths: 25, 75

        * - :strong:`Add responder option`
          - :strong:`Steps`

        * - Add responders by name
          - Enter user names in the :guilabel:`Search people` field and select the user when they appear. 
  
        * - Add responders by schedule
          - Enter a schedule name in the :guilabel:`Search schedules` field and select the schedule when it appears. Adding a schedule to a workflow step notifies the user that is on call when that workflow step is triggered. 

#. Repeat these steps until you have all the responders you want to invite to incidents for this step in the workflow. 
#. Select :guilabel:`Add responders`.
#. Select :guilabel:`Add New Step` to add additional escalating steps with additional responders to your incident workflow.
#. Select an elapsed time period in the drop-down list next to :guilabel:`If unacknowledged after`.
#. Select :guilabel:`Configure invite` to add responders.
#. Repeat these steps until you have a complete incident workflow for the service. 

.. _ii-service-maintenance:

Mute notifications using service maintenance
==================================================

Use service maintenance to mute notifications while you are making changes to the service. 

To put your service in maintenance, select the :guilabel:`Actions` menu on the service you want to put in maintenance and select :guilabel:`Maintenance`. The service status will show as :guilabel:`Maintenance`. 

All incidents that are associated with the service that are triggered while the service is in maintenance are created in a muted state. No responders are notified when a muted incident is triggered. Muted incidents don't show in your incident list by default. To see your muted incidents, select the :guilabel:`Incidents` tab in Incident Intelligence and add a ``Status = Muted`` filter.  Muted incidents are read-only and can't be acknowledged, resolved, or rejected.  

Take a service out of maintenance
-------------------------------------

To take a service out of maintenance and resume triggering incidents, select the :guilabel:`Actions` menu on the service you want to take out of maintenance and select :guilabel:`Enable`. The service status shows as :guilabel:`Enabled`. This resumes triggering incidents associated with the service.   

Next step
============

If you are setting up Incident Intelligence for the first time, next you need to create an on-call schedule. See :ref:`ii-create-manage-on-call-schedules`.