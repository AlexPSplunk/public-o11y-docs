(collectd-uptime)=

# Collectd uptime
<meta name="Description" content="Use this Splunk Observability Cloud integration for the Collectd Uptime monitor. See benefits, install, configuration, and metrics">

## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the `collectd/uptime` monitor type for the Smart Agent Receiver.

Use this integration to send a single metric of the total number of seconds the host has been up, using the collectd uptime plugin.

```{note}
This monitor is not available on Windows as collectd plugins are only supported in Linux and Kubernetes. 
```

### Benefits

```{include} /_includes/benefits.md
```

## Installation

```{include} /_includes/collector-installation-linux.md
```

## Configuration

```{include} /_includes/configuration.md
```

```
receivers:
  smartagent/collectd/uptime:
    type: collectd/uptime
    ... # Additional config
```

To complete the integration, include the Smart Agent receiver using this monitor in a metrics pipeline. To do this, add the receiver to the service > pipelines > metrics > receivers section of your configuration file.

```
service:
  pipelines:
    metrics:
      receivers: [smartagent/collectd/uptime]
```

## Metrics

The following metrics are available for this integration.

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/collectd/uptime/metadata.yaml"></div>

## Get help

```{include} /_includes/troubleshooting.md
```
