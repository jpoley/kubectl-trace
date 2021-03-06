# kubectl trace

`kubectl trace` is a kubectl plugin that allows you to schedule the execution
of [bpftrace](https://github.com/iovisor/bpftrace) programs in your Kubernetes cluster.

![Screenshot showing the read.bt program for kubectl-trace](docs/img/intro.png)

## Installation

```
go get -u github.com/fntlnz/kubectl-trace/cmd/kubectl-trace
```

This will download and compile `kubectl-trace` so that you can use it as a kubectl plugin with `kubectl trace`

## Usage

You don't need to setup anything on your cluster before using it, please don't use it already
on a production system, just because this isn't yet 100% ready.

**Run a program from string literal:**

```
kubectl trace run ip-180-12-0-152.ec2.internal -e "tracepoint:syscalls:sys_enter_* { @[probe] = count(); }"
```


**Run a program from file:**

```
kubectl trace run ip-180-12-0-152.ec2.internal -f read.bt
```

Need more programs? Look [here](https://github.com/iovisor/bpftrace/tree/master/tools)

Some of them will not yet work because we don't attach with a TTY already, sorry for that but good news you can contribute it!

## Status of the project

:trophy: All the MVP goals are done!

To consider this project (ready) the goals are:

- [x] basic program run and attach
- [x] list command to list running traces - command: `kubectl trace get`
- [x] delete running traces
- [x] run without attach
- [x] attach command to attach only - command: `kubectl trace attach <program>`
- [x] allow sending signals (probably requires a TTY), so that bpftrace commands can be notified to stop by the user before deletion and give back results


**More things after the MVP:**

The program is now limited to run programs only on your nodes but the idea is to have the ability to attach only to the user namespace of a pod, like:

```
kubectl trace run pod/<pod-name> -f read.bt
```

And even on a specific container

```
kubectl trace run pod/<pod-name> -c <container> f read.bt
```

So I would say, the next thing is to run bpftrace programs at a pod scope other than at node scope.

**bpftrace work**

I also plan to contribute some IO functions to bpftrace to send data to a backend database like InfluxDB instead of only stdout
because that would enable having things like graphs showing 

## Contributing

Please just do it, this is MIT licensed so no reason not to!
