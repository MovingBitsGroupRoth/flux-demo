# Scripts

git commit -m ' 

git push

k get ns

k config set-context --current --namespace=demoapp-ns  
k get pods  
k port-forward
k port-forward deployment/demoapp-dep 8089:80

k get pods -n demoapp-ns --watch

## demo2

mkdir demo2
pwd
/w/Projekte/flux-demo/clusters/flux-cluster

flux create source git flux-demo2-source-git --url=https://github.com/MovingBitsGroupRoth/KubernetesVotingApp --branch=master --target-namespace=devopsinaction --export > flux-demo2-source.yaml

cd "W:/Projekte/flux-demo/clusters/flux-cluster"
flux create kustomization flux-demo2-kustomization \
--source=GitRepository/flux-demo2-source-git \
--namespace=flux-system \
--path=./ \
--prune=true \
--export > flux-demo2-kustomization.yaml
path => Die Dateien liegen in der Root des Repositories, daher der Pfad ./
prune => Alle Ressourcen, die nicht mehr in der Git-Repository definiert sind, werden gelöscht. Das ist wichtig, damit die Ressourcen immer mit dem Git-Repository synchronisiert bleiben.

flux get source git && flux get kustomization

k get all -n devopsinaction

k config set-context --current --namespace=devopsinaction
k port-forward deployment/vote 8090:80
http://localhost:8090

## Private Repos

k get secret -n flux-system

k create secret generic flux-demo2-git-credentials --from-literal=username=movingbitsroth --from-literal=password=gh_xyz --namespace=flux-system && k get secret -n flux-system

k api-resources | grep -i flux

## demo4 - Kustomize

flux create source git flux-demo4-source-git --url=https://github.com/MovingBitsGroupRoth/kustomizedemo --branch=main --export > flux-demo4-source.yaml

flux create kustomization flux-demo4-kustomization \
--source=GitRepository/flux-demo4-source-git \
--namespace=flux-system \
--path=./ \
--prune=true \
--export > flux-demo4-kustomization.yaml

flux get source git && echo && echo "kust:" && flux get kustomization

k get all -n demons --show-labels

## demo5 - Helm
flux create source git flux-demo5-source-git --url=https://github.com/MovingBitsGroupRoth/Argo-CD-for-the-Absolute-Beginners --branch=main --export > flux-demo5-source.yaml

flux create helmrelease flux-demo5-helmrelease \
--chart=Section10_Helm_Charts/demotest \
--source=GitRepository/flux-demo5-source-git \
--export > flux-demo5-helmrelease.yaml

flux get sources all

flux get hr

k get all -n demotest-ns

## demo6 - Helm from Helm-Repository
https://learning.oreilly.com/videos/flux-cd-for/9781806706457/9781806706457-video7_5/

## OCI Registry

### K8s Artifacts Manifest
https://github.com/MovingBitsGroupRoth/ocidemo

docker login ghcr.io
User = MovingBitsGroupRoth
Password = ghp_xyz....

git clone https://github.com/MovingBitsGroupRoth/ocidemo

cd "W:\Projekte_externe_GitHub_Referenzen\Flux CD\ocidemo\manifests"

TAG="$(git rev-parse --short HEAD)" && \
REPO="ghcr.io/movingbitsgrouproth/ocidemo" && \
flux push artifact "oci://${REPO}:${TAG}" \
--path=./ \
--source="$(git config --get remote.origin.url)" \
--revision="$(git branch --show-current)@sha1:$(git rev-parse HEAD)"

https://github.com/MovingBitsGroupRoth?tab=packages

### Helm
cd "W:\Projekte_externe_GitHub_Referenzen\Flux CD\ocidemo\charts"

helm package ocidemo-chart

helm registry login ghcr.io --username MovingBitsGroupRoth 
Password = ghp_xyz....

helm push ocidemo-chart-1.1.1.tgz oci://ghcr.io/movingbitsgrouproth

### Deploying K8s from OCI Registry
2 Code-Zeilen im docx 

cd "W:\Projekte\flux-demo\clusters\flux-cluster\demo7"

flux create source oci demo7-source-oci --url=oci://ghcr.io/movingbitsgrouproth/ocidemo \
--tag=872d790 \
--secret-ref=ghcr-secret \
--export > demo7-source-oci.yaml

flux create kustomization demo7-kustomize-oci \
--source=OCIRepository/demo7-source-oci \
--target-namespace=ocidemo-ns \
--prune=true \
--export > demo7-kustomize-oci.yaml

flux get sources oci  
flux get kustomization  
kubectl get namespace  
kubectl get all -n ocidemo-ns

### Deploying Helm Chart from OCI Registry

cd "W:\Projekte\flux-demo\clusters\flux-cluster\demo8"

flux create source oci demo8-source-oci \
--url=oci://ghcr.io/movingbitsgrouproth/ocidemo-chart \
--tag=1.1.1 \
--secret-ref=ghcr-secret \
--export > demo8-source-oci.yaml

flux create hr demo8-helm-oci \
--chart-ref=OCIRepository/demo8-source-oci \
--export > demo8-helm-oci.yaml

flux get sources oci  
flux get kustomization  
flux get hr  
k get namespace  
k get all -n ocidemo-chart-ns

## demo9 - Image Controller

k get all -n flux-system

flux bootstrap github \
--token-auth \
--owner=movingbitsgrouproth \
--repository=flux-demo \
--branch=main \
--path=clusters/flux-cluster \
--private=false \
--personal=true \
--components-extra=image-reflector-controller,image-automation-controller 

k get all -n flux-system

k api-resources | grep -i flux

git pull

W:\Projekte\flux-demo\clusters\flux-cluster\flux-system\gotk-components.yaml ist im Bereich components geändert

k get namespace

### Reparatur

kubectl -n flux-system patch gitrepository flux-system --type=json \
-p='[{"op":"remove","path":"/spec/secretRef"}]'

