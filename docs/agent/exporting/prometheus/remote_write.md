

The Prometheus remote write exporting connector uses the exporting engine to send Netdata metrics to your choice of more
than 20 external storage providers for long-term archiving and further analysis.

## Prerequisites

To use the Prometheus remote write API with [storage
providers](https://prometheus.io/operating/integrations/#remote-endpoints-and-storage), install
[protobuf](https://developers.google.com/protocol-buffers/) and [snappy](https://github.com/google/snappy) libraries.
Next, re-install Netdata from the source, which detects that the required libraries and
utilities are now available.

## Configuration

To enable data exporting to a storage provider using the Prometheus remote write API, run `./edit-config exporting.conf`
in the Netdata configuration directory and set the following options:

```conf
[remote_write:my_instance]
    enabled = yes
    destination = example.domain:example_port
    remote write URL path = /receive
```

`remote write URL path` is used to set an endpoint path for the remote write protocol. The default value is `/receive`.
For example, if your endpoint is `http://example.domain:example_port/storage/read`:

```conf
    destination = example.domain:example_port
    remote write URL path = /storage/read
```

`buffered` and `lost` dimensions in the Netdata Exporting Connector Data Size operation monitoring chart estimate uncompressed
buffer size on failures.

## Notes

The remote write exporting connector does not support `buffer on failures`

