# process_exporter

*Export Metrics About External Processes*

This prometheus-style metric exporter exposes the basic process-wide metrics
about a pre-configured set of other processes to watch.

## Configuration

It is configured by a YAML file that gives names and ways to find external
processes, most likely by reading a pidfile.

```yaml
processes:
  my-server:
    pidfile: "/var/run/my-server.pid"

```

## Exported Metrics

Process-wide metrics about the monitoring processes are exported under the
`exported_process` name prefix, using a label called `process_name` whose value
is the name given in the configuration file.

```
exported_process_cpu_seconds_total{process_name="my-server"} 1.234
```

# See Also

There are other process-metric exporters by the same name with different
behaviours.

 * https://github.com/ncabatoff/process-exporter

 * https://github.com/tokuhirom/process_exporter

This implementation differs from the above primarily because it has a
pre-determined set of processes it will watch, configured by the YAML config
file. These other exporters work by scraping *all* the processes found,
looking for patterns of behaviour that make them interesting enough to export.
