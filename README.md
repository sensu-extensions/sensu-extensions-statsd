# Sensu StatsD Extension

A StatsD implementation for the Sensu client. This check extension
creates a StatsD TCP & UDP listener, receives StatsD metrics, parses
them, and flushes them to the Graphite plaintext format for Sensu to
send to Graphite or another TSDB.

## Installation

This extension requires Sensu version >= 0.26.

On a Sensu client machine.

```
sensu-install -e statsd:0.0.1
```

Edit `/etc/sensu/conf.d/extensions.json` to load it.

``` json
{
  "extensions": {
    "statsd": {
      "version": "0.0.1"
    }
  }
}
```

Restart the Sensu client.

``` shell
sudo service sensu-client restart
```

## Configuration

Edit `/etc/sensu/conf.d/statsd.json` to change its configuration.

``` json
{
  "statsd": {
    "bind": "0.0.0.0"
  }
}
```

|attribute|type|default|description|
|----|----|----|---|
|bind|string|"127.0.0.1"|IP to bind the StatsD sockets to|
|port|integer|8125|Port to bind the StatsD sockets to|
|flush_interval|integer|10|The StatsD flush interval|
|send_interval|integer|30|How often Graphite metrics are sent to Sensu|
|percentile|integer|90|The percentile to calculate for StatsD metrics|
|path_prefix|string|"statsd"|The optional Graphite metric path prefix|
|handler|string|"graphite"|Handler to use for the Graphite metrics|

## Example

Test the StatsD TCP socket:

``` shell
echo "orders:1|c" | nc 127.0.0.1 8125
```
