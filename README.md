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

# Bases

Base manifest references in each `kustomization` are frozen by commit SHA. To change the version of the base, please change the git ref.

# Images

These deployments freeze images using the `images` feature of `kustomize` independant of the base manifests. To change the versions of image, please edit the `kustomization` of each component.

# Additional Notes

- When deploying from scratch, it's necesary to fetch the Search and Admin API keys from the `/keys` endpoint of Meiliseach, as these keys are automatically generated on a fresh deployment.
- Merckury has a high startup cost, because the images Crucible provides build the static pages of the application on startup, so as to be agnostic to different environemnts (Next loads environment variables at build-time). This leads to high startup memory usage, so make sure it's satiated.
  - Testing has shown that a minimum of 2000 MiB is required. This may increase as additional features are added to Merckury.
  - With 100 mCPU, the startup time is about 350 seconds.
  - In order to reduce startup costs, future releases may want to employ custom builds of images, as opposed to the current environment-agnostic image, perhaps utilizing the [Geddes registry](https://geddes-registry.rcac.purdue.edu/).
- Because of limited resource quotas, all deployments are set to `Recreate`. This may lead to downtime during `apply` of new releases.
- This project requires access to an S3 bucket. Currently an external bucket is being used, pending provisioning from Geddes.
