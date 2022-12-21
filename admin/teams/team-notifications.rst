.. _admin-team-notifications:

*****************************************************************
Manage team notifications in Splunk Observability Cloud
*****************************************************************

.. meta::
      :description: Configure a general team notification policy, or notification by alert severity, to help your team stay aware of alerts from detectors.


Configure a team notification policy to help your team stay aware of alerts from detectors.

To learn more about which roles can configure a team notification policy, see :ref:`about-team-roles`.

When you specify a team as the recipient for a detector, the detector sends alert notifications to the team based on the team's notification policy. A team's notification policy applies to all detectors where the team is a notification recipient. You can't specify different team notification policies for different detectors.

To learn more about specifying detector recipients, see :ref:`manage-subs`.

To configure a team notification policy, follow these steps:

#. Log in to Splunk Observability Cloud.

#. In the left navigation menu, select :menuselection:`Settings > Teams`.

#. A table of current teams appears in the main panel.

#. Click :guilabel:`Notification Policy` for your team.

#. Click :guilabel:`Configure a single policy for alerts of any severity` to define a single notification policy for alerts of all severities. Click :guilabel:`Configure separate notification tiers for different severity alerts` to define a separate notification policy for each severity level.

#. Add and remove notification recipients as needed.

#. Click :guilabel:`Save`.
