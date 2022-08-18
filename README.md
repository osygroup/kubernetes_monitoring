# How to setup Endpoint/Website monitoring on Kubernetes using Prometheus, Grafana and Blackbox Exporter

## Create namespace for all the resources needed

Create a namespace called 'monitoring' or any name of choice:

$ kubectl create namespace monitoring 

## Install Blackbox Exporter

This helm chart installs the Blackbox Exporter:

$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

$ helm repo update

$ helm install prometheus-blackbox-exporter prometheus-community/prometheus-blackbox-exporter -n monitoring

## Install Prometheus

A configmap is used to create the prometheus.yaml file that will be mounted in the /etc/prometheus/ directory in the prometheus pod.

From the kubernetes-prometheus-files directory, install the configmap. It has 'job_name: blackbox' block added (last job in the file), used for listing the endpoints to be monitored. Edit and add your endpoint(s), then install:

$ kubectl apply -f config-map.yaml

Then install the clusterRole, deployment and internal service:

$ kubectl apply -f clusterRole.yaml

$ kubectl apply -f prometheus-deployment.yaml

$ kubectl apply -f prometheus-service-internal

For external access, you can also install nodePort, LB or Ingress.

## Install Grafana

From the kubernetes-grafana-files directory, install the deployment and service files. For external access, you can use a nodePort, LB or Ingress. You can create the yaml files yourself and install in the 'monitoring' namespace.

To add prometheus as a datasource, use the DNS name of your Prometheus internal service (e.g. your-svc.your-namespace.svc.cluster.local:port) or public url (if you setup nodePort, LB or Ingress) as the endpoint.

Prometheus Blackbox Exporter dashboard: https://grafana.com/grafana/dashboards/7587

Go to Grafana -> + sign -> Import and enter 7587 number.

Don't forget to save the dashboard.





### Credits:

https://prometheus-community.github.io/helm-charts

https://devopscube.com/setup-prometheus-monitoring-on-kubernetes/

https://github.com/prometheus/blackbox_exporter#prometheus-configuration

https://grafana.com/docs/grafana/latest/setup-grafana/installation/kubernetes/
