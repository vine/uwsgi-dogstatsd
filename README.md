uwsgi-dogstatsd
===============

[uWSGI] plugin to emit [DogStatsD] metrics to [Datadog] via the [Datadog Agent]


INSTALL
=======

The plugin is 2.x friendly, so installation can be done directly from the repo:

```sh
uwsgi --build-plugin https://github.com/Datadog/uwsgi-dogstatsd
```

USAGE
=====

Depends on a Datadog Agent to be installed and by default listens for dogstatsd on port 8125.

Configure your `.ini` file to enable the metrics subsystem, and enable the dogstatsd plugin.

Here's a

```ini
[uwsgi]
master = true
processes = 8
threads = 4

http = :9090

# DogStatsD plugin configuration
enable-metrics = true
plugin = dogstatsd
stats-push = dogstatsd:127.0.0.1:8125,myapp

# Application to load
wsgi-file = app.py
...
```

This will begin producing metrics with the prefix defined in the configuration, `myapp` here:

```console
myapp.core.avg_response_time
myapp.core.busy_workers
myapp.core.idle_workers
myapp.core.overloaded
myapp.core.routed_signals
myapp.core.total_rss
myapp.core.total_tx
myapp.core.total_vsz
myapp.core.unrouted_signals
myapp.rss_size
myapp.socket.listen_queue
myapp.vsz_size
myapp.worker.avg_response_time
myapp.worker.core.exceptions
myapp.worker.core.offloaded_requests
myapp.worker.core.read_errors
myapp.worker.core.requests
myapp.worker.core.routed_requests
myapp.worker.core.static_requests
myapp.worker.core.write_errors
myapp.worker.delta_requests
myapp.worker.failed_requests
myapp.worker.requests
myapp.worker.respawns
myapp.worker.rss_size
myapp.worker.total_tx
myapp.worker.vsz_size
```

The metrics are tagged and split where there are more than one occurrence, such as CPU core, worker.

Read more on the [uWSGI Metrics subsystem] for further explanation on metrics provided.

[Datadog]: http://www.datadog.com/
[Datadog Agent]: https://github.com/DataDog/dd-agent
[DogStatsD]: http://docs.datadoghq.com/guides/dogstatsd/
[uWSGI]: http://uwsgi-docs.readthedocs.org/
[uWSGI Metrics subsystem]: http://uwsgi-docs.readthedocs.org/en/latest/Metrics.html
