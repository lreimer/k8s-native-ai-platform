# K8s-native AI Platform in GCP

Demo repository for a Kubernetes-native AI platform. Current demo is for GCP,
but could be deployed to other suitable cloud providers also.

## Setup

```bash
# create the Kubernetes cluster in GCP with GPU support
# bootstrap AI platform components and services using Flux2
make create-gke-cluster
make bootstrap-flux2

# required to configure Config Connector with Google Cloud ProjectID
kubectl annotate namespace default cnrm.cloud.google.com/project-id="data-engineering-lab-411011"
```

## Maintainer

M.-Leander Reimer (@lreimer), <mario-leander.reimer@qaware.de>

## License

This software is provided under the MIT open source license, read the `LICENSE`
file for details.
