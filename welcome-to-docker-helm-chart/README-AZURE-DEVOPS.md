# Helm chart for welcome-to-docker

This ZIP contains a `helm/welcome-to-docker/` chart. Extract it and commit the `helm` folder into the root of the SAME repository and branch checked out by your Azure DevOps build pipeline. Do not commit the ZIP alone: Helm package needs the extracted chart directory.

## Classic pipeline: Package and deploy Helm charts

- Command: `package`
- Chart Path: `$(Build.SourcesDirectory)/helm/welcome-to-docker`
- Version: `1.0.0`
- Destination: `$(Build.ArtifactStagingDirectory)`
- Update Dependency: unchecked (no chart dependencies)
- Save: unchecked

Your existing Publish Build Artifacts task should publish `$(Build.ArtifactStagingDirectory)` as `drop`.

## Image settings

The default values point to `rose1.azurecr.io/repo1:saffronrose` (from the existing `riku.yml` manifest), with application container port 3000. Change `image.repository` and `image.tag` in `values.yaml` if your Docker push task uses a different image or tag. Ensure AKS can pull from the Azure Container Registry (e.g. ACR attach or image pull secret).

## Validation and deployment

`helm lint helm/welcome-to-docker`

`helm template welcome-to-docker helm/welcome-to-docker`

`helm package helm/welcome-to-docker`

Deployment is a separate step, e.g. after configuring AKS access:

`helm upgrade --install welcome-to-docker helm/welcome-to-docker --namespace default`

Helm package only creates a `.tgz` chart archive; it does not deploy to AKS.
