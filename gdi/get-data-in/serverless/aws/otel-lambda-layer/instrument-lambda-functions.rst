.. _instrument-aws-lambda-functions:

******************************************************************
Instrument your AWS Lambda function for Splunk Observability Cloud
******************************************************************

.. meta::
   :description: The Splunk OpenTelemetry Lambda Layer automatically instruments your AWS Lambda functions for many programming languages. Follow these steps to get started.

Use the Splunk OpenTelemetry Lambda Layer to automatically instrument your AWS Lambda functions for many programming languages. To get started, use the guided setup or follow the instructions manually.

To send data to a Splunk OTel Collector in EC2, see :ref:`ec2-otel-collector-serverless`.

Generate customized instructions using the guided setup
====================================================================

To generate a template that instruments your Lambda function using the Splunk OpenTelemetry Lambda Layer, use the AWS Lambda guided setup. To access the AWS Lambda guided setup, follow these steps:

#. Log in to Splunk Observability Cloud.

#. Open the :new-page:`AWS Lambda guided setup <https://login.signalfx.com/#/gdi/scripted/awslambda/step-1?category=product-apm&gdiState=%7B"integrationId":"awslambda"%7D>`. Optionally, you can navigate to the guided setup on your own:

   #. In the left navigation menu, select :menuselection:`Data Management` to open the Integrate Your Data page.

   #. In the integration filter menu, select :guilabel:`All`.

   #. In the :guilabel:`Search` field, search for :guilabel:`AWS Lambda`.

   #. Select the :guilabel:`AWS Lambda` tile to open the AWS Lambda guided setup.

Install the Splunk OpenTelemetry Lambda Layer manually
==================================================================

Follow these instructions to install the Splunk OpenTelemetry Lambda Layer:

- :ref:`otel-lambda-layer-requirements`
- :ref:`install-otel-lambda-layer`
- :ref:`set-env-vars-otel-lambda`

To instrument Go functions, also follow the steps in :ref:`go-serverless-instrumentation`.

For Python functions using the Serverless Framework, see :ref:`serverless-framework-support-aws`.

.. _otel-lambda-layer-requirements:

Check compatibility and requirements
----------------------------------------------

The Splunk OpenTelemetry Lambda Layer supports the following runtimes in AWS Lambda:

- Java 8 and 11
- Python 3.8 and 3.9
- Node.js 10, 12, and 14
- Ruby 2.7
- Go 1.18

The Lambda Layer requires 49 MB on-disk in standard x86_64 systems.

For more information, search for Lambda runtimes and other system requirements on the AWS documentation website.

.. _install-otel-lambda-layer:

Install the AWS Lambda layer
----------------------------------------------

Follow these steps to instrument your function using the Splunk OpenTelemetry Lambda Layer:

#. In the AWS Lambda console, select the function that you want to instrument.

#. In the :guilabel:`Layers` section, select :guilabel:`Add a layer`, then select :guilabel:`Specify an ARN`.

#. Copy the Amazon Resource Name (ARN) for the region of your Lambda function from the list matching your architecture:

   .. tabs::

      .. tab:: Standard x86_64

         .. github:: yaml
            :url: https://raw.githubusercontent.com/signalfx/lambda-layer-versions/main/splunk-apm/splunk-apm.md

      .. tab:: Graviton2 ARM64

         .. github:: yaml
            :url: https://raw.githubusercontent.com/signalfx/lambda-layer-versions/main/splunk-apm/splunk-apm-arm.md

#. Paste the selected ARN in the :guilabel:`Specify an ARN` field and select :guilabel:`Add`.

#. Check that the Splunk layer appears in the :guilabel:`Layers` table.

.. note:: You can automate the update of the Lambda layer using the AWS CLI. The following command, for example, retrieves the latest version of the Splunk layer for x86_64 and the ``us-east-1`` region:

   .. code-block:: bash

      aws lambda list-layer-versions --layer-name splunk-apm --region us-east-1 --query 'LayerVersions[0].LayerVersionArn'

.. _set-env-vars-otel-lambda:

Configure the Splunk OpenTelemetry Lambda Layer
----------------------------------------------------

Follow these steps to add the required configuration for the Splunk OpenTelemetry Lambda Layer:

1. In the AWS Lambda console, open the function that you are instrumenting.

2. Navigate to :guilabel:`Configuration` > :guilabel:`Environment variables`, then click :guilabel:`Edit`.

3. Add each of the following environment variables by clicking :guilabel:`Add environment variable`:

   .. list-table::
      :header-rows: 1
      :widths: 20 80
      :width: 100%

      * - Environment variable
        - Description

      * - ``SPLUNK_REALM``
        - To find the realm of your Splunk Observability Cloud account, open the navigation menu in Observability Cloud, select :menuselection:`Settings`, and select your username. The realm name appears in the :guilabel:`Organizations` section.

      * - ``SPLUNK_ACCESS_TOKEN``
        - A Splunk authentication token that lets exporters send data directly to Splunk Observability Cloud. See :ref:`Authentication token <admin-tokens>`. To send data to a Splunk OTel Collector in EC2, see :ref:`ec2-otel-collector-serverless`.

      * - ``AWS_LAMBDA_EXEC_WRAPPER``
        - Set the value for the ``AWS_LAMBDA_EXEC_WRAPPER`` environment variable:

            .. tabs::

               .. tab:: Java

                  .. code-block:: shell

                     # Select the most appropriate value

                     # Wraps regular handlers that implement RequestHandler
                     /opt/otel-handler

                     # Same as otel-handler, but proxied through API Gateway,
                     # with HTTP context propagation activated
                     /opt/otel-proxy-handler

                     # Wraps streaming handlers that implement RequestStreamHandler
                     /opt/otel-stream-handler

                  .. note:: Only AWS SDK v2 instrumentation is activated by default. To instrument other libraries, modify your code to include the corresponding library instrumentation from the OpenTelemetry Java SDK.

               .. code-tab:: shell Python

                  /opt/otel-instrument

               .. code-tab:: shell Node.js

                  /opt/nodejs-otel-handler

               .. code-tab:: shell Ruby

                  /opt/ruby-otel-handler

                  .. note:: The Graviton2 ARM64 architecture is not supported for Ruby Lambda functions.

               .. tab:: Go

                  Don't set the ``AWS_LAMBDA_EXEC_WRAPPER`` environment variable. See :ref:`go-serverless-instrumentation`.

      * - ``OTEL_SERVICE_NAME``
        - The name of your service.

      * - (Optional) ``OTEL_RESOURCE_ATTRIBUTES``
        - Define the name of the deployment environment of your function by setting this environment variable to ``deployment.environment=<name-of-your-environment>``.

4. Click :guilabel:`Save` and check that the environment variables appear in the table.

To configure the mode of metric ingest, see :ref:`metrics-configuration-lambda`.

.. note:: By default, the layer sends telemetry directly to Observability Cloud ingest endpoints. To send data to a Splunk OTel Collector in EC2, see :ref:`ec2-otel-collector-serverless`.

.. _go-serverless-instrumentation:

Instrument Go functions in AWS Lambda
-------------------------------------------

To instrument a Go function in AWS Lambda for Splunk APM, follow these additional steps:

#. Run the following commands to install the ``otellambda`` and the Splunk OTel Go distribution:

   .. code-block:: bash

      go get -u go.opentelemetry.io/contrib/instrumentation/github.com/aws/aws-lambda-go/otellambda
      go get -u github.com/signalfx/splunk-otel-go/distro

#. Create a wrapper for the OpenTelemetry instrumentation in your function's code. For example:

   .. code-block:: go

      package main

      import (
         "context"
         "fmt"

         "github.com/aws/aws-lambda-go/lambda"
         "github.com/signalfx/splunk-otel-go/distro"
         "go.opentelemetry.io/contrib/instrumentation/github.com/aws/aws-lambda-go/otellambda"
         "go.opentelemetry.io/otel"
      )

      func main() {
         distro.Run()
         flusher := otel.GetTracerProvider().(otellambda.Flusher)
         lambda.Start(otellambda.InstrumentHandler(HandleRequest, otellambda.WithFlusher(flusher)))
      }

      type MyEvent struct {
         Name string `json:"name"`
      }

      func HandleRequest(ctx context.Context, name MyEvent) (string, error) {
         return fmt.Sprintf("Hello %s!", name.Name), nil
      }

.. note:: For a full example, see :new-page:`https://github.com/signalfx/tracing-examples/blob/main/opentelemetry-tracing/opentelemetry-lambda/go/example.go <https://github.com/signalfx/tracing-examples/blob/main/opentelemetry-tracing/opentelemetry-lambda/go/example.go>` on GitHub.

.. _serverless-framework-support-aws:

Serverless Framework support
---------------------------------

Some features of the Serverless Framework might impact OpenTelemetry tracing of Python Lambda functions.

Python libraries compression
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``zip`` feature of ``pythonRequirements`` allows packing and deploying Lambda dependencies as compressed files. To instrument packages compressed using the Serverless Framework, set the ``SPLUNK_LAMBDA_SLS_ZIP`` environment variable to ``true``. For more information, see https://github.com/serverless/serverless-python-requirements#dealing-with-lambdas-size-limitations on GitHub.

Slim feature
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Slim feature reduces the size of Lambda packages by removing some files, including ``dist-info`` folders. Some of the files removed by the Slim feature are required by the OpenTelemetry Python autoinstrumentation. Deactivate the ``slim`` option in your ``serverless.yml`` file or define custom ``slimPatterns``. For more information, see https://github.com/serverless/serverless-python-requirements#slim-package on GitHub.

.. _check-otel-lambda-data:

Check that data appears in Splunk Observability Cloud
----------------------------------------------------------

Each time the AWS Lambda function runs, trace and metric data appears in Splunk Observability Cloud. If no data appears, see :ref:`troubleshooting-lambda-layer`.

.. _ec2-otel-collector-serverless:

Send serverless spans through the Splunk OpenTelemetry Collector
=====================================================================

By default, the Splunk OpenTelemetry Lambda Layer sends telemetry to Splunk Observability Cloud endpoints, without using a Collector. 

Though not required, deploying a Splunk OTel Collector in the same virtual private cloud (VPC) of your Lambda can reduce latency in some cases.

To send spans to the Splunk OTel Collector from an AWS Lambda function instrumented using the Splunk Lambda layer, follow these steps:

#. Deploy the Collector in Gateway mode in a service your Lambda can reach, for example EC2. See :ref:`collector-gateway-mode`.
#. Install the Splunk OTel Lambda layer. See :ref:`instrument-aws-lambda-functions`.
#. Navigate to :guilabel:`Configuration` > :guilabel:`Environment variables`, then select :guilabel:`Edit`.
#. As you're sending telemetry to the Collector, delete the ``SPLUNK_REALM`` environment variable.
#. If you've already set the access token in the Collector configuration, delete the ``SPLUNK_ACCESS_TOKEN`` environment variable.
#. Add the following environment variables:

   -  ``OTEL_TRACES_EXPORTER`` with the value ``otlp_proto_http``
   -  ``OTEL_EXPORTER_OTLP_ENDPOINT`` with the value ``<collector-gateway-ip-or-dns-name>:4318``
