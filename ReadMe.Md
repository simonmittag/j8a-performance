# Why
Establish performance baseline for J8a on common AWS hardware.

# What
Record telemetry data for J8a process, including upstream, downstream
connections, memory, CPU consumption, data throughput and roundtrip latency
while under load from multiple downstream clients and proxying to multiple
upstream resources

# How
This project uses ansible to configure multiple http client machines, one
proxy machine and multiple upstream machines to run a performanc test suite.
Client connections are created using [![p0d](https://github.com/simonmittag/p0d)]

## Pre-test Phase
Validates test set-up

pt-p0d1 2x c5n.4xlarge	$1.128	16	42 GiB	EBS Only	Up to 25 Gigabit
pt-p0d2 2x c5n.4xlarge	$1.128	16	42 GiB	EBS Only	Up to 25 Gigabit
pt-j8a1 2x c5n.4xlarge	$1.128	16	42 GiB	EBS Only	Up to 25 Gigabit



### Mse6 Instance
1x c5n.4xlarge	$1.128	16	42 GiB	EBS Only	Up to 25 Gigabit
n

## Test Phase

### J8a Instance
1x c5n.18xlarge	$5.076	72	192 GiB	EBS Only	100 Gigabit

### P0d Instance
8x c5n.4xlarge	$1.128	16	42 GiB	EBS Only	Up to 25 Gigabit

### Mse6 Instance
8x c5n.4xlarge	$1.128	16	42 GiB	EBS Only	Up to 25 Gigabit
