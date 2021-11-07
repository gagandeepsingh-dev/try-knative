# try-knative
Try Knative

In this repo I will share how you can quickly get started with Knative on a local environment. I tried following the Knative documentation, which although very good  has made some critical assumptions which may cause difficulties to a beginner.

Pre - Requisites

You will need  
kubernetes cluster 
istio installion

Let's create the kuberentes cluster . Simple run the command below

minikube start --cpus 4 --memory 4096 --insecure-registry registry.dev.svc.cluster.local:5000 --driver=virtualbox --disk-size=10000mb

Next step is to install istio 
1) We need to download istioctl first
   ```bash
     curl -L https://istio.io/downloadIstio | sh -
     cd istio-1.11.4
     export PATH=$PWD/bin:$PATH
   ```

2) Create a istio-minimal-operator.yaml file using the following template:

```yaml
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
spec:
  values:
    global:
      proxy:
        autoInject: disabled
      useMCP: false
      # The third-party-jwt is not enabled on all k8s.
      # See: https://istio.io/docs/ops/best-practices/security/#configure-third-party-service-account-tokens
      jwtPolicy: first-party-jwt

  addonComponents:
    pilot:
      enabled: true

  components:
    ingressGateways:
      - name: istio-ingressgateway
        enabled: true
```
3) Apply the YAML file by running the command:
   ```bash
   istioctl install -f istio-minimal-operator.yaml
   ```

Verify your install 
Run 
```bash
kubectl get pods --namespace istio-system
```

Next we need to install 

Choose the operator based installation and follow the steps from link below
https://knative.dev/docs/install/operator/knative-with-operators/

You need to install both serving and eventing

Running a Sample service
https://knative.dev/docs/getting-started/first-service/

