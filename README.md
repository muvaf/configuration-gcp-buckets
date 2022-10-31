# Secure GCP Bucket Abstractions

This repo contains a Crossplane Configuration that defines a single API that can
be backed by multiple GCP Bucket compositions. You can find what compositions
are available in this [folder](./compositions).

## Getting Started

Pre-requisites:
* Kubernetes cluster.
* Crossplane or Universal Crossplane is already installed in the cluster.
* `up` CLI `>=v0.14.0` is installed in your computer.


Install the configuration.
```bash
up ctp configuration install xpkg.upbound.io/muvaf/configuration-gcp-buckets:v1.0.0
```

You will need a Google Service Account to provision the resources. Each
`Composition` has a YAML file under the `cloud.google.com/permissions` annotation
you can use to create a role with necessary permissions.

```bash
# An example composition could be encrypted.xgcpbuckets.storage.muvaf.com
kubectl get composition encrypted.xgcpbuckets.storage.muvaf.com \
  -o jsonpath="{.metadata.annotations.cloud\.google\.com/permissions}" > permissions.yaml
```
```bash
gcloud iam roles create encrypted-buckets --project=my-project-id \
  --file=permissions.yaml
```

Now you can assign this `encrypted-buckets` role to the Service Account you
intend to use in the `ProviderConfig` of Upbound GCP Provider to provision
resources on your behalf. More details on how to configure Upbound GCP provider [here](https://marketplace.upbound.io/providers/upbound/provider-gcp/latest/docs/quickstart).

Create your first claim!
```bash
kubectl apply -f examples/bucket.yaml
```

This should create a `GCPBucket` and a `Deployment` that mounts its connection
secret and uploads files to that bucket.
