# kubelog

[Fluentd](https://www.fluentd.org/) Kubernetes container logs & [journald](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html) log collector with [Graylog](https://www.graylog.org/) output.
Configurable through [ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/).

## Requirements

* Docker
* Kubernetes cluster access 
* Gelf input on Graylog

## Installation

1. Clone this repository and build a docker image:

```sh
docker build -t <registry>/<image-name>:<version> .
```

2. Modify the docker image on file daemonset.yaml for your docker image generated in previously step.

        spec:
            serviceAccountName: kubelog
            dnsPolicy: ClusterFirst
            containers:
            - name: agent
                image:<registry>/<image-name>:<version>

3. Edit fluent.conf according to your need
4. Execute:
 
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