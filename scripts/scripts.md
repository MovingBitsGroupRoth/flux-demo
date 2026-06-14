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

flux get source git && echo "\nkust:" && flux get kustomization

k get all -n demons --show-labels

## demo5 - Helm
flux create source git flux-demo5-source-git --url=https://github.com/MovingBitsGroupRoth/Argo-CD-for-the-Absolute-Beginners --branch=main --export > flux-demo5-source.yaml

flux create helmrelease flux-demo5-helmrelease \
--chart=demotest \
--source=GitRepository/flux-demo5-source-git \
--export > flux-demo5-helmrelease.yaml
