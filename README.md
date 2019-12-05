[![Bonsai Asset Badge](https://img.shields.io/badge/Sensu%20Go%20CPU%20Check-Download%20Me-brightgreen.svg?colorB=89C967&logo=sensu)](https://bonsai.sensu.io/assets/asachs01/sensu-go-cpu-check) [![TravisCI Build Status](https://travis-ci.org/asachs01/sensu-go-cpu-check.svg?branch=master)](https://travis-ci.org/asachs01/sensu-go-cpu-check)

# Sensu Go CPU Check
- [Overview](#overview)
- [Usage examples](#usage-examples)
- [Configuration](#configuration)
  - [Asset registration](#asset-registration)
  - [Asset definition](#asset-definition)
  - [Check definition](#resource-definition)
- [Installation from source and contributing](#installation-from-source-and-contributing)
- [Additional notes](#additional-notes)

This plugin provides a check for system CPU utilization for Sensu Go. The `sensu-go-cpu-check` check takes the flags `-w` (warning) and `-c` (critical) and a desired  duration utilization percentage after each flag. By default, these are a warning value of 75% and a critical value of 90%. This check also outputs data as `nagios_perfdata`(for more information, see [this Nagios documentation article](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/3/en/perfdata.html). This allows for the check to be used as both a status check and a metric check. You can see an example of this in the [example check definition](#check-definition) below.

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

### Example Output

```bash
./sensu-go-cpu-check
CheckCPU OK - value = 39.50 | system_cpu=39.50
```

## Configuration

### Asset registration

Assets are the best way to make use of this check. If you're not using an asset, please consider doing so! You can find this asset on the [Bonsai Asset Index](https://bonsai.sensu.io/assets/asachs01/sensu-go-cpu-check).

### Asset definition

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

**NOTE**: ***PLEASE ENSURE YOU UPDATE YOUR URL AND SHA512 BEFORE USING THE ASSET***. If you don't, you might just be stuck on a super old version. Don't say I didn't warn you ¯\\_(ツ)_/¯

### Check definition

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

## Installation from source and contributing

While it's generally recommended to use an asset, you can download a copy of the handler plugin from [releases][1],
or create an executable script from this source.

From the local path of the sensu-go-cpu-check repository:

**sensu-go-cpu-check**
```
go build -o /usr/local/bin/sensu-go-cpu-check main.go
```
To contribute to this repo, see https://github.com/sensu/sensu-go/blob/master/CONTRIBUTING.md.

## Additional notes

### Supported operating systems

This project uses `gopsutil`, and is thus largely dependent on the systems that it supports. For this plugin, the following operating systems are supported:

* Linux
* FreeBSD
* OpenBSD
* Mac OS X
* Windows
* Solaris

[1]: https://github.com/asachs01/sensu-go-cpu-check/releases
