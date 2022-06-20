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
and [J8a](https://github.com/simonmittag/j8a) routes to an upstream HTTP servers
running [mse6](https://github.com/simonmittag/mse6).

## Pre-test Phase
In this phase, the test set-up is validated, including the ansible scripts. It
lives on the `pre-test` branch

| Machine | Instance Type | CPU Cores | RAM | Network Speed |
|-|-|-|-|-|
| pt-p0d1  | c5n.2xlarge | 8 | 21 GiB | 25 Gigabit |
| pt-p0d2  | c5n.2xlarge | 8 | 21 GiB | 25 Gigabit |
| pt-j8a1  | c5n.2xlarge | 8 | 21 GiB | 25 Gigabit |
| pt-mse61 | c5n.2xlarge | 8 | 21 GiB | 25 Gigabit |

### Instructions

1. Configure ansible to use the hosts file, by adding the below to `~/.ansible.cfg`
```bash
[defaults]
inventory = $PWD/hosts
```
2. Update the `./hosts` file IP addresses of AWS machines.
```bash
...
[j8a]
pt-j8a1 ip={{your ip}}
...
```
Make sure these machines are available on port 8080 and 8443 publicly.
Ideally they are placed in the same subnet to avoid measuring network lag. Don't
forget to update your ssh aliases under `~/.ssh/config` if necessary.

3. `ansible-playbook homebrew.yml` configures base install for virtual servers.
This sets up homebrew on linux and increases open file descriptors to avoid default
ceilings for connections.

4. `ansible-playbook mse6.yml` installs upstream mse6 servers and starts the
process

5. `ansible-playbook j8a.yml` installs j8a proxy server at default log level and
starts the process.

6. `ansible-playbook p0d.yml` install p0d http clients

7. `ansible-playbook boontaeve.yml --extra-vars runid=n` executes a test run and
fetches data from remote machines, stored locally under `/results`.
