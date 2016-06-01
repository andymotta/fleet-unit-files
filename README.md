# Fleet Unit Files
This repository contains automated Fleet deployment files for https://github.com/andymotta/fleet-etcd2-microservices-cluster

## Using Fleet

### FleetUI
Visit the UI as noted in tf outputs, i.e. something like:
`  fleet_ui  = http://internal-FleetUI-elb-000000000.us-west-2.elb.amazonaws.com
`
cmd+click to visit on OS X
### fleetctl
For remote CLI access, FLEETCTL_TUNNEL is another tf output, i.e.
```
export FLEETCTL_TUNNEL=internal-worker-elb-000000000.us-west-2.elb.amazonaws.com:2222
export FLEETCTL_STRICT_HOST_KEY_CHECKING=false
```

Then add this to ~/.bashrc or ~/.zshrc:
```
eval $(ssh-agent -s)
ssh-add your-instance-key.pem
```

### Useful Commands:
```
fleetctl ssh login@2
fleetctl cat login-discovery@1
fleetctl journal --lines 200 service-name@1
```

Launching a unit with fleet is as simple as running `fleetctl start`:
```
$ fleetctl start examples/hello.service
Unit hello.service launched on 113f16a7.../172.17.8.103
```
To launch x amount of instances (example will launch 4):
```
fleetctl start service-name@{1..4}.service
```
The `fleetctl start` command waits for the unit to get scheduled and actually start somewhere in the cluster.
`fleetctl list-unit-files` tells you the desired state of your units and where they are currently scheduled:
```
$ fleetctl list-unit-files
UNIT            HASH     DSTATE    STATE     TMACHINE
hello.service   e55c0ae  launched  launched  113f16a7.../172.17.8.103
```
`fleetctl list-units` exposes the systemd state for each unit in your fleet cluster:
```
$ fleetctl list-units
UNIT            MACHINE                    ACTIVE   SUB
hello.service   113f16a7.../172.17.8.103   active   running
```

## Using etcd-browser
Visit the UI as noted in tf outputs, i.e.
`  etcd_browser  = http://internal-etcd-browser-elb-000000000.us-west-2.elb.amazonaws.com
`

See: https://github.com/henszey/etcd-browser/blob/master/README.md
