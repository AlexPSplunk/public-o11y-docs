.. _ruby-manual-instrumentation:

**********************************************************************
Manually instrument Ruby applications for Splunk Observability Cloud
**********************************************************************

.. meta:: 
   :description: Manually instrument your Ruby application when you need to add custom attributes to spans or want to manually generate spans. Keep reading to learn how to manually instrument your Ruby application for Splunk Observability Cloud. 

Instrumenting applications automatically using the agent of the Splunk Distribution of OpenTelemetry Ruby covers most needs. Manually instrumenting your application is only necessary when, for example, you need to add custom attributes to spans or need to manually generate spans.

Libraries installation
=========================================

With manual instrumentation, you can install and enable instrumentation libraries separately. This lets you control which gems Ruby retrieves when building your project. 

To learn how to manually instrument a Ruby application, see  :new-page:`https://opentelemetry.io/docs/instrumentation/ruby/manual/` at OpenTelemetry.io.

To install and enable an instrumentation library manually:

#. Install the instrumentation library using ``gem install`` or by including it in the project's Gemfile. For example, to install the Sinatra instrumentation, add the following to your Gemfile:

   .. code-block:: bash
      
      gem "opentelemetry-instrumentation-sinatra", "~> 0.21"

#. In a block passed to the ``Splunk::Otel.configure`` method, configure the SDK to use each of the instrumentation libraries. In the case of the Sinatra instrumentation, the block would look like the following example:

   .. code-block:: ruby

      require "splunk/otel"
      Splunk::Otel.configure do |c|
      c.use "OpenTelemetry::Instrumentation::Sinatra", { opt: "value" }
      end

Manual instrumentation for spans and events
===========================================

For examples of manual instrumentation for Ruby, see the official OpenTelemetry documentation at :new-page:`https://opentelemetry.io/docs/instrumentation/ruby/manual/ <https://opentelemetry.io/docs/instrumentation/ruby/manual/>`.

.. note:: Manual OTel instrumentation is fully compatible with Splunk automatic Ruby instrumentation and is fully supported by Splunk.