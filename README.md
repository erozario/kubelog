# kubelog

[Fluentd](https://www.fluentd.org/) Kubernetes container logs & [journald](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html) log collector with [Graylog](https://www.graylog.org/) output.
Configurable through [ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/).

## Requirements

* Docker
* Kubernetes cluster access 
* Gelf input on Graylog

## Installation

Clone this repository and edit fluent.conf according to your need and build a docker image.

```sh
docker build -t <registry>/<image-name>:<version> .
```

Modify the docker image on file daemonset.yaml for your docker image generated in previously step.

Afther that execute:
 
```sh
kubectl create -f rbac.yaml

kubectl create configmap \
    --namespace kube-system kubelog \
    --from-file fluent.conf \
    --from-literal GELF_HOST=<server address> \
    --from-literal GELF_PORT=<port> \
    --from-literal GELF_PROTOCOL=<udp|tcp>

kubectl create -f daemonset.yaml
```