:orphan:

.. include:: /_includes/slo/slo-preview-header.rst


.. _slo-intro:

******************************************************************************************
Introduction to service level objective (SLO) management in Splunk Observability Cloud
******************************************************************************************

.. meta::
    :description: Get started with service level objective (SLO) concepts and SLO in Splunk Observability Cloud


Service level objective (SLO) management is a service level monitoring experience that helps you align your business needs with your engineering reliability goals.

While you have always been able to use the data in Splunk Observability Cloud to monitor, troubleshoot, and optimize your services, including applications and infrastructure, SLO management provides a quantitative way to track the reliability and performance of your services. This allows teams to make the right investment trade-offs between product development and operational work.

What is service level monitoring?
=============================================

Service level monitoring is the process of measuring, tracking, and analyzing a variety of service level indicators (SLIs) associated with a given service in order to calculate the health of a deployed software or service and compare it against the service level agreement (SLA).

The following table defines key concepts in service level monitoring.

 .. list-table::
    :header-rows: 1
    :widths: 20 40 40

    * - :strong:`Concept`
      - :strong:`Definition`
      - :strong:`Examples`
        
    * - Service level indicator (SLI)
      - An SLI is a quantitative measurement showing some health of a service, expressed as a metric or combination of metrics.
      - * Availability SLI: Proportion of requests that resulted in a successful response
        * Performance SLI: Proportion of requests that loaded in < 100 ms

    * - Service level objective (SLO)
      - An SLO defines a target for an SLI and a compliance period over which that target must be met. An SLO contains 3 elements: an SLI, a target, and a compliance period. Compliance periods can be calendar, such as monthly, or rolling, such as past 30 days.
      - * Availability SLI over a calendar period: Our service must respond successfully to 95% of requests in a month
        * Performance SLI over a rolling period: Our service must respond to 99% of requests in < 100 ms over a 7-day period

    * - Service level agreement (SLA)
      - An SLA is a contractual agreement that indicates service levels your users can expect from your organization. If an SLA is not met, there can be financial consequences.
      - An customer service SLA indicates that 90% of support requests received on a normal support day must have a response within 6 hours.
    
    * - Error budget
      - A measurement of how your SLI performs relative to your SLO over a period of time. Error budget measures the difference between actual and desired performance. It determines how unreliable your service might be during this period and serves as a signal when you need to take corrective action.
      - Our service can respond to 1% of requests in >100 ms over a 7 day period

    * - Burn rate 
      - A unitless measurement of how quickly a service consumes the error budget during the compliance window of the SLO. Burn rate makes the SLO and error budget actionable, showing service owners when a current incident is serious enough to page an on-call responder.
      - For an SLO with a 30-day compliance window, a constant burn rate of 1 means your error budget is used up in exactly 30 days.

    
Create your first SLO
==========================================

To start using SLO, see :ref:`create-slo`.





