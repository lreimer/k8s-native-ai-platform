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
```

## Usage

```bash
# model deployment using CLI
kollama deploy llama3.1
kollama expose llama3.1 --service-name=ollama-model-llama31-lb --service-type=LoadBalancer

# model deployment via CRD
kubectl apply -f models/ollama-model-llama31.yaml
kollama expose llama3.1 --service-name=ollama-model-llama31
kollama expose llama3.1 --service-name=ollama-model-llama31-lb --service-type LoadBalancer

# to start a chat with ollama
# exchange localhost with the actual LoadBalancer IP
export OLLAMA_HOST=$(kubectl get service ollama-model-llama31-lb -o jsonpath='{.status.loadBalancer.ingress[0].ip}'):11434
ollama run llama3.1

# call the chat API of Ollama or OpenAI
# curl http://$OLLAMA_HOST/api/chat
curl http://$OLLAMA_HOST/v1/chat/completions  \
  -H "Content-Type: application/json"  \
  -d '{
    "model": "llama3.1",
    "messages": [
      {
        "role": "user",
        "content": "Say hi to the Continuous Lifecycle conference!"
      }
    ]
  }'

# call OpenAI proxy gateway
export OPENAI_PROXY_HOST=$(kubectl get service openai-proxy-service -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
curl http://$OPENAI_PROXY_HOST/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o-mini",
    "messages": [{"role": "user", "content": "Say hi to the Continuous Lifecycle conference!"}],
    "temperature": 0.7
  }'
```

## Maintainer

M.-Leander Reimer (@lreimer), <mario-leander.reimer@qaware.de>

## License

This software is provided under the MIT open source license, read the `LICENSE`
file for details.
