# Spark on Kubernetes Setup Guide

This guide walks you through setting up Apache Spark on a Kubernetes cluster using Minikube and Helm.

<div style="display: flex; justify-content: space-around; align-items: center;">
  <img src="https://raw.githubusercontent.com/kubernetes/minikube/master/images/logo/logo.png" alt="Minikube Logo" width="200"/>
  <img src="[https://kubernetes.io/images/favicon.png](https://upload.wikimedia.org/wikipedia/commons/3/39/Kubernetes_logo_without_workmark.svg)" alt="Kubernetes Logo" width="200"/>
  <img src="https://spark.apache.org/images/spark-logo-trademark.png" alt="Apache Spark Logo" width="200"/>
</div>

## Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Helm](https://helm.sh/)
- `kubectl` command-line tool

## Setup Steps

1. **Install Minikube and Helm**
   Follow the installation guides for [Minikube](https://minikube.sigs.k8s.io/docs/start/) and [Helm](https://helm.sh/).

2. **Start Minikube**
   ```bash
   minikube start --cpus=4 --memory=8096
   ```

3. **Verify Installation**
   ```bash
   minikube status
   kubectl cluster-info
   kubectl get all
   ```

4. **Create Namespace**
   ```bash
   kubectl create namespace spark
   ```

5. **Install Spark using Helm**
   ```bash
   helm repo add bitnami https://charts.bitnami.com/bitnami
   helm install spark-301 --namespace spark bitnami/spark
   ```

6. **Check Spark Installation Status**
   ```bash
   minikube dashboard
   ```

7. **Run Spark Example (Pi Calculation)**
   ```bash
   export EXAMPLE_JAR=$(kubectl exec -ti --namespace spark spark-301-worker-0 -- find examples/jars/ -name 'spark-example*\.jar' | tr -d '\r')
   kubectl exec -ti --namespace spark spark-301-worker-0 -- spark-submit --master spark://spark-301-master-svc:7077 \
   --class org.apache.spark.examples.SparkPi \
   $EXAMPLE_JAR 5
   ```

8. **Port-forward Spark Web UI**
   ```bash
   kubectl port-forward --namespace spark svc/spark-301-master-svc 8080:80
   ```

9. **Access Spark Web UI**
   Open a web browser and visit: [http://localhost:8080](http://localhost:8080)

## Troubleshooting

If you encounter any issues, please check the Minikube and Spark documentation or open an issue in this repository.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is open source and available under the [MIT License](LICENSE).
