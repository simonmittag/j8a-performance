# Why?
Establish performance baseline for [J8a](https://github.com/simonmittag/j8a) on common AWS hardware.

# What?
Record telemetry data for J8a process, including upstream, downstream
connections, memory, CPU consumption, data throughput and roundtrip latency
while under load from multiple downstream clients and proxying to multiple
upstream resources.

# How?
This project uses ansible to configure virtual servers as http client machines,
proxy server and multiple upstream machines to run a performance test suite.
Client connections are created using [p0d](https://github.com/simonmittag/p0d)
and [J8a](https://github.com/simonmittag/j8a) routes to an upstream instance of [mse6](https://github.com/simonmittag/mse6).

## Pre-test Phase
In this phase, the test set-up is validated, including the ansible scripts. It
lives on the `pre-test` branch

| Machine | Instance Type | Price | CPU Cores | RAM | Network Speed |
|-|-|-|-|-|-|
| pt-p0d1  | c5n.4xlarge | $1.128 | 16 | 42 GiB | 25 Gigabit |
| pt-p0d2  | c5n.4xlarge | $1.128 | 16 | 42 GiB | 25 Gigabit |
| pt-j8a1  | c5n.4xlarge | $1.128 | 16 | 42 GiB | 25 Gigabit |
| pt-mse61 | c5n.4xlarge | $1.128 | 16 | 42 GiB | 25 Gigabit |

### How to?
1. Update the `hosts` file IP addresses of AWS machines and make them publicly accessible on
port 22, 8080 and 8443
