# Deployment manifests for Geddes
This deployment is available at https://crucible.geddes.rcac.purdue.edu/ .

This release is [kustomized](https://kustomize.io/) off of (https://github.com/CrucibleSDS/k8s-base).

To deploy a new release, use `kubectl apply -k .` in the base directory of this repository.

Alternatively, deploy any of the components of this release. For example:
 - `kubectl apply -k postgres`
 - `kubectl apply -k meilisearch`
 - `kubectl apply -k web`
 - `kubectl apply -k web/merckury`
 - `kubectl apply -k web/silicon`

# Secrets
Because Geddes does not have secrets management, secrets are created on the Geddes Rancher dashboard. Please read the [base manifests](https://github.com/CrucibleSDS/k8s-base) for information on how to configure secrets.
