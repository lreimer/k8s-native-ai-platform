# K8s-native AI Platform in GCP

Demo repository for a Kubernetes-native AI platform. Current demo is for GCP,
but could be deployed to other suitable cloud providers also.

## Setup

```bash
# create the Kubernetes cluster in GCP with GPU support
make create-gke-cluster

# bootstrap AI platform components and services using Flux2
# make sure you have set a valid GITHUB_TOKEN environment variable
make bootstrap-flux2

# required to configure Config Connector with Google Cloud ProjectID
kubectl annotate namespace default cnrm.cloud.google.com/project-id=data-engineering-lab-411011

# install required dependencies via Brewfile
brew bundle
task create-secrets

# the Kube Prometheus Stack is accessiable via Ingress
open http://grafana.127.0.0.1.sslip.io

# to test the OpenAI proxy, issue the following curl command
curl http://openai.127.0.0.1.sslip.io/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
     "model": "gpt-4o-mini",
     "messages": [{"role": "user", "content": "Say this is a test!"}],
     "temperature": 0.7
   }'

# the Langflow UI is accessible via Ingress
open http://langflow.127.0.0.1.sslip.io 

# the Jupyther Hub UI is accessible via Ingress
open http://jupyther.127.0.0.1.sslip.io 
```



## Maintainer

M.-Leander Reimer (@lreimer), <mario-leander.reimer@qaware.de>

## License

This software is provided under the MIT open source license, read the `LICENSE`
file for details.
