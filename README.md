# cos-local-oberservablity-setup
Setup Grafana and Prometheus on local minikube setup

# Enable Ingress addOn on Minikube
```
minikube addons enable ingress
```

# Enable Metric Server addOn on Minikube
```
minikube addons enable metrics-server
```

# Clone `kube-state-metrics` repo
```
git clone git@github.com:kubernetes/kube-state-metrics.git
```

# Deploy `kube-state-metrics`
```
kubectl apply -f kube-state-metrics/examples/standard
```

# Clone `cAdvisor` repo
```
git clone git@github.com:google/cadvisor.git 
```

# Deploy `cAdvisor`
```
kubectl apply -k cadvisor/deploy/kubernetes/base
kubectl apply -f cos-local-oberservablity-setup/cadvisor-service.yaml
```

# Create `monitoring` namespace
```
kubectl apply -f cos-local-oberservablity-setup/prometheus/namespace.yaml
```

# Deploy `node-exporter`
```
kubectl apply -f cos-local-oberservablity-setup/prometheus/node-exporter/nodeExporter-daemonset.yaml
kubectl apply -f cos-local-oberservablity-setup/prometheus/node-exporter/nodeExporter-service.yaml
```

# Install OLM
```
kubectl create -f https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.23.1/crds.yaml
kubectl create -f https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.23.1/olm.yaml
```

# Switch to `redhat-openshift-connectors` namespace


# Install Prometheus Operator
```
kubectl apply -f cos-local-oberservablity-setup/operatorgroup.yaml
kubectl apply -f cos-local-oberservablity-setup/prometheus/install-prometheus-operator.yaml
```

# Create PodMonitor CR instance of each RHOC service
```
kubectl apply -f cos-local-oberservablity-setup/prometheus/selectors
```

# Create Prometheus CR instance
```
kubectl apply -f cos-local-oberservablity-setup/prometheus/prometheus-service-account.yaml
kubectl apply -f cos-local-oberservablity-setup/prometheus/prometheus-cluster-role.yaml
kubectl apply -f cos-local-oberservablity-setup/prometheus/prometheus-cluster-role-binding.yaml
kubectl apply -f cos-local-oberservablity-setup/prometheus/prometheus-server.yaml
```

# Expose Prometheus Server
```
kubectl apply -f cos-local-oberservablity-setup/prometheus/prometheus-service.yaml
kubectl apply -f cos-local-oberservablity-setup/prometheus/prometheus-ingress.yaml
```

# Check Ingress load-balancer IP in Ingress status
```
kubectl get ingress
```

# Add Host name entry into system host file
```
sudo vim/etc/hosts
```

# Insert below line
```
192.168.58.2 prometheus-server
```

# After that you could access Prometheus console using local web-browser @http://prometheus-server

# Install Grafana Operator
```
kubectl apply -f cos-local-oberservablity-setup/grafana/install-grafana-operator.yaml
```

# Create GrafanaDataSource CR instance
```
kubectl apply -f cos-local-oberservablity-setup/grafana/grafana-datasource.yaml
```

# Create Grafana CR instace
```
kubectl apply -f cos-local-oberservablity-setup/grafana/grafana-server.yaml
```

# Expose Grafana server
```
kubectl apply -f cos-local-oberservablity-setup/grafana/grafana-ingress.yaml
```

# Add Host name entry into system host file
```
sudo vim/etc/hosts
```

# Insert below line
```
192.168.58.2 prometheus-server grafana-server
```

# After that you could access Grafana console using local web-browser @http://grafana-server