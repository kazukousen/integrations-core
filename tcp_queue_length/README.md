# Agent Check: TCP Queue Length

## Overview

This check monitors the size of the Linux TCP receive and send queues.

## Setup

### Installation

`tcp_queue_length` is a core Agent 6/7 check that relies on an eBPF part implemented in `system-probe`.

The eBPF program used by `system-probe` is compiled at runtime and requires you to have access to the proper kernel headers.

On Debian-like distributions, install the kernel headers like this:
```sh
apt install -y linux-headers-$(uname -r)
```

On RHEL-like distributions, install the kernel headers like this:
```sh
yum install -y kernel-headers-$(uname -r)
```

### Configuration

Enabling it requires to enable it both for `system-probe` and the core agent.

Inside the `system-probe.yaml` configuration file, the following parameter must be set:
```yaml
system_probe_config:
  enable_tcp_queue_length: true
```

For the “core” agent, a `tcp_queue_length.d/conf.yaml` file is needed in the `conf.d` directory [with the following content][1].

The `only_count_nb_contexts` parameter controls whether:
* the check should collect all the data—that is, the size of the TCP queues for every single connection (`false`)
* the check should collect only the number of timeseries it would normally collect (`true`). This is the default.

### Configuration with Helm

With the [Datadog Helm chart][2], the `datadog.systemProbe.enableTCPQueueLength` parameter is the only thing to set in `values.yaml` to enable the check.

### Validation

[Run the Agent's `status` subcommand][3] and look for `tcp_queue_length` under the checks section.

## Data Collected

### Metrics

For each TCP connection, `tcp_queue_length` collects the minimum and the maximum amount of data in the send and receive buffers between each data collection time point.

See [metadata.csv][4] for a list of metrics provided by this integration.


[1]: https://github.com/DataDog/datadog-agent/blob/master/cmd/agent/dist/conf.d/tcp_queue_length.d/conf.yaml.example
[2]: https://github.com/helm/charts/tree/master/stable/datadog
[3]: https://docs.datadoghq.com/agent/guide/agent-commands/#start-stop-and-restart-the-agent
[4]: https://github.com/DataDog/integrations-core/blob/master/tcp_queue_length/metadata.csv