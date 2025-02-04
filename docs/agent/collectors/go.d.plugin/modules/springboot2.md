---
title: "Java Spring Boot 2 application monitoring with Netdata"
description: "Monitor the health and performance of Spring Boot 2-compatible apps with zero configuration, per-second metric granularity, and interactive visualizations."
custom_edit_url: https://github.com/netdata/go.d.plugin/edit/master/modules/springboot2/README.md
sidebar_label: "Java Spring Boot 2 applications"
---



This module monitors one or more Java Spring-boot 2 applications depending on configuration. Netdata can be used to
monitor running Java [Spring Boot 2](https://spring.io/) applications that expose their metrics with the use of the **
Spring Boot Actuator** included in Spring Boot library.

Springboot2 module looks up `http://localhost:8080/actuator/prometheus` and `http://127.0.0.1:8080/actuator/prometheus`
to detect Spring Boot application by default.

## Charts

- Response Codes in `requests/s`
- Threads in `threads`
- Heap Memory Usage Overview in `bytes`
- Heap Memory Usage Eden Space in `bytes`
- Heap Memory Usage Survivor Space in `bytes`
- Heap Memory Usage Old Space in `bytes`
- Uptime in `seconds`

## Configuration

Edit the `go.d/springboot2.conf` configuration file using `edit-config` from the
Netdata [config directory](/docs/configure/nodes), which is typically at `/etc/netdata`.

```bash
cd /etc/netdata # Replace this path with your Netdata config directory
sudo ./edit-config go.d/springboot2.conf
```

The Spring Boot Actuator exposes these metrics over HTTP and is very easy to use:

- add `org.springframework.boot:spring-boot-starter-actuator` and `io.micrometer:micrometer-registry-prometheus` to your
  application dependencies
- set `management.endpoints.web.exposure.include=*` in your `application.properties`

Please refer to
the [Spring Boot Actuator: Production-ready features](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready.html)
and [81. Actuator - Part IX. ‘How-to’ guides](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-actuator.html)
for more information.

Here is an example for 2 servers:

```yaml
jobs:
  - name: local
    url: http://localhost:8080/actuator/prometheus

  - name: remote
    url: http://203.0.113.10:8080/actuator/prometheus
```

For all available options please see
module [configuration file](https://github.com/netdata/go.d.plugin/blob/master/config/go.d/springboot2.conf).

## Troubleshooting

To troubleshoot issues with the `springboot2` collector, run the `go.d.plugin` with the debug option enabled. The output
should give you clues as to why the collector isn't working.

First, navigate to your plugins directory, usually at `/usr/libexec/netdata/plugins.d/`. If that's not the case on your
system, open `netdata.conf` and look for the setting `plugins directory`. Once you're in the plugin's directory, switch
to the `netdata` user.

```bash
cd /usr/libexec/netdata/plugins.d/
sudo -u netdata -s
```

You can now run the `go.d.plugin` to debug the collector:

```bash
./go.d.plugin -d -m springboot2
```
