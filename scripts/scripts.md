# Scripts

git commit -m ' 

git push

kubectl get ns

kubectl get all -n demoapp-ns

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
