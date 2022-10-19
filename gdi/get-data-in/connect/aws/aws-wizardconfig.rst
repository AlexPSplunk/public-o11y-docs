.. _aws-wizardconfig:

*********************************************************************
Connect to AWS using the guided setup in Splunk Observability Cloud
*********************************************************************

.. meta::
  :description: Use guided setup to connect Splunk Observability Cloud to AWS through CloudWatch.

If you have administrator privileges for Splunk Observability Cloud and your Amazon Web Services (AWS) account, you can use guided setup to do the following:

- Connect AWS to Observability Cloud.

- Configure metrics and logs collection.

If you want to activate Metric Streams, :ref:`read the specific instructions <aws-wizard-metricstreams>`.

For other ways to connect Observability Cloud to AWS, see :ref:`Connect to AWS and send data to Observability Cloud <get-started-aws>`.


.. _aws-wizard:

Access the guided setup for AWS integration
============================================

To access the guided setup for AWS integration, perform the following steps:

#. Log in to Splunk Observability Cloud.
#. On the left navigation menu, select :menuselection:`Data Management`. 
#. Select :guilabel:`Add Integration` to open the :guilabel:`Integrate Your Data` page.
#. On the Integrate Your Data page, select the tile for :strong:`Amazon Web Services`.
#. Follow the steps provided in the guided setup. 
#. For the authentication step of establishing a connection between your AWS account and Splunk Observability Cloud, do one of the following:
    - In most AWS regions, use the Identity and Access Management (IAM) policy created through the guided setup.

    - For the GovCloud or China regions, select the option to authenticate using a secure token.

.. note:: While choosing data sources, you might encounter an option to import all data from built-in CloudWatch namespaces. In such a case, select that option to ensure that built-in dashboards display automatically.

6. Select the rate at which you want Splunk Observability Cloud to poll CloudWatch for metric data, with 1 minute as the minimum value, and 10 minutes as the maximum value. For example, a value of 300 polls metrics once every 5 minutes. Poll rate is expressed in seconds.  

If you run into a problem, the guided setup displays an error message in context at the step with the problem. The error message summarizes and suggests a fix for that problem. If more error detail is available, select the error summary to expand and display additional details.

To learn more about how to get AWS CloudWatch data into Observability Cloud, :new-page:`watch this video on the Splunk YouTube channel <https://youtu.be/MFs5Dge8t3A>`.


.. _aws-iam-policy:

Create an AWS IAM policy
-------------------------

The AWS IAM policy is a JSON object to which Observability Cloud refers for permission to collect data from every supported AWS service.

If this is the first time you have connected Observability Cloud to Amazon CloudWatch, or if you want to create a new AWS IAM policy, follow these steps. If you have already installed at least one AWS integration and want to reuse the same IAM policy, skip to the :ref:`aws-iam-role` section.

1. Log into your Amazon Web Services account.
2. From the Services list, select :strong:`Security, Identity, & Compliance > IAM` to open Identity and Access Management.
3. Select :strong:`Policies > Create Policy`, then select the :strong:`JSON` tab.
4. Replace the placeholder JSON with the :ref:`default AWS IAM policy JSON <review-aws-iam-policy>`.

    Alternatively, you can also get this default AWS IAM policy JSON in the :guilabel:`Prepare AWS Account` step of the guided setup in Observability Cloud. The default AWS IAM policy supports metrics and log collection. To learn how to add support for CloudWatch Metric Streams, see :ref:`aws-wizard-metricstreams`.

5. Follow the instructions, and go through :strong:`Next: Tags`, and :strong:`Next: Review`. Give the policy a name, and select :strong:`Create policy`.

While preparing your AWS account, guided setup prompts you to copy the default IAM policy to connect your AWS account to Splunk Observability Cloud.


.. _aws-iam-role:

Create an AWS IAM role
-------------------------

Your AWS account includes IAM in its list of services. After creating an AWS IAM policy, you assign that policy to a particular role by performing the following steps in the Amazon Web Services console:

1. Select :strong:`Roles > Create Role`.
2. Select :strong:`Another AWS account` as the type of trusted entity.
3. Copy and paste the Account ID displayed in guided setup into the :strong:`Account ID` field.
4. Select :strong:`Require external ID`. Copy and paste the External ID displayed in the guided setup into the :strong:`External ID` field.
5. Click :strong:`Next: Permissions`. Under :strong:`Policy name`, select the policy you made in the previous step.
6. Click through :strong:`Next: Tags` and :strong:`Next: Review`.
7. Name your new AWS IAM role. You also have the option of adding a short description for it. Select :strong:`Create role`.

Creating the AWS IAM role generates the ``Role ARN`` used to establish connection with AWS. Copy the created ARN role, and paste it into the :strong:`Role ARN` field in the guided setup.


.. _aws-wizard-metricstreams:

Enable Metric Streams
======================================

To enable Metric Streams, use the :ref:`guided setup <aws-wizard>`, and take the following considerations into account:

* On the :strong:`Choose AWS Console` screen, disable the CloudWatch Metrics polling box.
* When creating the new AWS IAM policy, :ref:`add these additional permissions <metricstreams_iampolicy>` to the ones already suggested in the guided setup.
* Follow the instructions to :ref:`enable Metric Streams <enable-cw-metricstreams>`.
* Update your settings and deploy the CloudFormation template following :ref:`these steps <enable-cw-metricstreams>`.


Review default AWS integration settings
==================================================

After creating an AWS IAM policy and assigning it to a particular role through the guided setup, you can modify your configuration as follows:

- Limit the scope of data collection in either of the following ways:

  - Use the check box options in the guided setup to limit the scope of your data collection.

    These are the available options:
      - Amazon Cost and Usage Metrics
      - CloudWatch Metrics polling (note: you can disable it altogether, or disable the polling but enable AWS Metric Streams instead)
      - CloudWatch Logs
      - AWS regions to fetch data from
      - AWS services to fetch data from

  - Use the AWS console to revise the contents of the ``Action`` and ``Resource`` fields.

- Select a :ref:`CloudFormation template <aws-cloudformation>` to collect logs or Metric Streams for each AWS region that you want to operate in.

After you connect Splunk Observability Cloud with AWS, you can use Observability Cloud to track metrics and analyze your AWS data in real time. See the AWS official documentation for a list of the available AWS resources.

Next steps
===========

After you connect Splunk Observability Cloud with AWS, you can use Observability Cloud to track a series of metrics and analyze your AWS data in real time. See :ref:`how to leverage data from integration with AWS <aws-post-install>` for more information.