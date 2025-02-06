+++
date = '2025-02-03T12:14:54+03:00'
draft = false
title = 'From Host to Container'
+++
## It's never too late to learn something, right?..
So basically, you can run programs from your host machine in a Docker container by executing them in the container’s namespace ([more about linux namespaces and containers here](https://securitylabs.datadoghq.com/articles/container-security-fundamentals-part-2/)).
The first thing to do is to find out the container’s PID.
```bash
pid="$(docker inspect -f '{{ .State.Pid }}' container_name)"
```
Then you can use `nsenter` to execute commands in the required namespace by using the PID (in this example, I'm executing `ip addr` in the network namespace)."
```bash
sudo nsenter --network --target $pid ip addr
```
## Dedicated container
There is also a possibility to run a container in the same PID namespace as another container.
```bash
docker run --name nginx nginx
docker run network-utils --pid=container:nginx
```
We can specify namespace if we want.
```bash
docker run network-utils --net=container:nginx
```