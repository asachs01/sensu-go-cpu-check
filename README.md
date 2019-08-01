# Sensu Go CPU Check
[![Bonsai Asset Badge](https://img.shields.io/badge/Sensu%20Go%20CPU%20Checks-Download%20Me-brightgreen.svg?colorB=89C967&logo=sensu)](https://bonsai.sensu.io/assets/asachs01/sensu-go-cpu-check) [![TravisCI Build Status](https://travis-ci.org/asachs01/sensu-go-cpu-check.svg?branch=master)](https://travis-ci.org/asachs01/sensu-go-cpu-check)

This plugin provides a check for system CPU utilization for Sensu Go. The `sensu-go-cpu-check` check takes the flags `-w` (warning) and `-c` (critical) and a desired  duration utilization percentage after each flag. By default, these are a warning value of 75% and a critical value of 90%.

## Installation

While it's generally recommended to use an asset, you can download a copy of the handler plugin from [releases][1],
or create an executable script from this source.

From the local path of the sensu-go-cpu-check repository:

**sensu-go-cpu-check**
```
go build -o /usr/local/bin/sensu-go-cpu-check main.go
```

## Configuration

### Asset Registration

Assets are the best way to make use of this check. If you're not using an asset, please consider doing so! You can find this asset on the [Bonsai Asset Index](https://bonsai.sensu.io/assets/asachs01/sensu-go-cpu-check).

You can download the asset definition there, or you can do a little bit of copy/pasta and use the one below:

```yml
---
type: Asset
api_version: core/v2
metadata:
  name: sensu-go-cpu-check
  namespace: CHANGEME
  labels: {}
  annotations: {}
spec:
  url: https://github.com/asachs01/sensu-go-cpu-check/releases/download/0.0.1/sensu-go-cpu-check_0.0.1_linux_amd64.tar.gz
  sha512: 
  filters:
  - entity.system.os == 'linux'
  - entity.system.arch == 'amd64'
```

**NOTE**: PLEASE ENSURE YOU UPDATE YOUR URL AND SHA512 BEFORE USING THE ASSET. If you don't, you might just be stuck on a super old version. Don't say I didn't warn you ¯\\_(ツ)_/¯

### Handler Configuration

Example Sensu Go definition:

**sensu-go-cpu-check**
```yml
type: CheckConfig
api_version: core/v2
metadata:
  name: sensu-go-cpu-check
  namespace: CHANGEME
spec:
  command: sensu-go-cpu-check -w 80 -c 95
  runtime_assets:
  - sensu-go-cpu-check
  interval: 60
  publish: true
  output_metric_format: nagios_perfdata
  output_metric_handlers:
  - infuxdb
  handlers:
  - slack
  subscriptions:
  - system
```

## Example Output


## Usage Examples

### Command line help

```
The Sensu Go check for system CPU usage

Usage:
  sensu-go-cpu-check [flags]

Flags:
  -c, --critical string   Critical value for system cpu (default "90")
  -h, --help              help for sensu-go-cpu-check
  -w, --warning string    Warning value for system cpu. (default "75")
```

## Supported Operating Systems

This project uses `gopsutil`, and is thus largely dependent on the systems that it supports. For this plugin, the following operating systems are supported:

* Linux
* FreeBSD
* OpenBSD
* Mac OS X
* Windows
* Solaris

## Contributing

See https://github.com/sensu/sensu-go/blob/master/CONTRIBUTING.md

[1]: https://github.com/asachs01/sensu-go-cpu-check/releases
