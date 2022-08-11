# How to setup Endpoint/Website monitoring on Kubernetes using Prometheus, Grafana and Blackbox Exporter

## Create namespace for all the resources needed

Create a namespace called 'monitoring':

$ kubectl create namespace monitoring 

## Install Blackbox Exporter

This helm chart installs the Blackbox Exporter:

$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

$ helm repo update

$ helm install prometheus-blackbox-exporter prometheus-community/prometheus-blackbox-exporter -n monitoring

## Install Prometheus

A configmap is used to create the prometheus.yaml file that will be mounted in the /etc/prometheus/ directory in the prometheus pod.

From the kubernetes-prometheus-files directory, install the configmap. It has 'job_name: blackbox' block added, used for listing the endpoints to be monitored 