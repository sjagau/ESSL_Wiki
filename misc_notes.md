`vscode://vscode.github-authentication/did-authenticate?windowid=1&code=6a02f2ec2b2cad4d3f91&state=4e3bce35-8035-41ef-9ef5-340dd13af209

[Create and Deploy a Docker Container Image to a Kubernetes Cluster](https://www.linode.com/docs/guides/deploy-container-image-to-kubernetes/)

1. Create Hugo site
1. Log-in to GitHub
1. create '.gitignore' and '.dockerignore' files
1. Push and save Hugo site files
1. login to Docker hub using Docker-Desktop
1. sudo docker build -t codebird/hugo-site:v1 .


`cat ~/docker-config.json | base64

```
alias mk8sctl=“sudo microk8s kubectl”
microk8s config > ~/.kube/config

Note: follow instructions on <https://fluxcd.io/docs/get-started/>

export GITHUB_USER=xCodeOwl
export GITHUB_TOKEN=
flux check --pre
flux bootstrap github --owner=$GITHUB_USER \
  --repository=fleet-infra --branch=main \
  --path=./clusters/my-cluster --personal

flux bootstrap github \
  --components-extra=image-reflector-controller,image-automation-controller \
  --owner=$GITHUB_USER \
  --repository=flux-image-updates \
  --branch=main --path=clusters/my-cluster \
  --read-write-key --personal

flux create source git essl-lab-doc \
  --url=https://github.com/xCodeOwl/essl-lab-docs \
  --branch=master --interval=30s \
  --export > ./clusters/my-cluster/essl-lab-docs-source.yaml

flux create kustomization podinfo --target-namespace=hugo-site \
  --source=essl-lab-doc --path="./kustomize" \
  --prune=true --interval=5m \
  --export > ./clusters/my-cluster/essl-lab-doc-kustomization.yaml

flux create image repository essl-docs \
--image=docker.io/codebird/essl-docs:v1 --interval=1m \
--export > ./clusters/my-cluster/podinfo-registry.yaml

flux create image policy essl-docs \
--image-ref=essl-docs --select-semver=5.0.x \
--export > ./clusters/my-cluster/essl-docd-policy.yaml
```