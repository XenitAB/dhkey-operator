# dhkey-operator 

Operator to handle dh (diffie hellman) keys in Kubernetes clusters.

## Running locally

### Installing operator-sdk cli

```shell
brew install operator-sdk
```

### Prepare python environment

```shell
python3 -m venv .venv
source .venv/bin/activate 
pip install -U pip
pip install ansible ansible-runner ansible-runner-http
pip install -r requirements.txt
ansible-galaxy collection install -r requirements.yml
```

### Running operator

```shell
kubectl apply -f deploy/crds/xenit.io_dhkeys_crd.yaml
kubectl apply -f deploy/crds/xenit.io_v1alpha1_dhkey_cr.yaml
source .venv/bin/activate
operator-sdk run local
```

### Reverting after local runs

```shell
kubectl delete -f deploy/crds/xenit.io_dhkeys_crd.yaml
kubectl delete -f deploy/crds/xenit.io_v1alpha1_dhkey_cr.yaml
```

## Deploying in cluster

### Helm

#### Helm Repo

```
helm repo add dhkey-operator https://xenitab.github.io/dhkey-operator/
helm repo update
```

#### Helm install (cluster scoped)

```
kubectl create namespace dhkey-operator
helm upgrade --namespace dhkey-operator --version 0.0.4 --install dhkey-operator dhkey-operator/dhkey-operator
kubectl apply -f deploy/crds/xenit.io_v1alpha1_dhkey_cr.yaml
```

#### Helm install (namespace scoped)

```
kubectl create namespace dhkey-operator
helm upgrade --namespace dhkey-operator --version 0.0.4 --install dhkey-operator --set operator.clusterScoped=false dhkey-operator/dhkey-operator
kubectl --namespace dhkey-operator apply -f deploy/crds/xenit.io_v1alpha1_dhkey_cr.yaml
```

#### Helm uninstall

```
helm uninstall --namespace dhkey-operator dhkey-operator
kubectl delete CustomResourceDefinition dhkeys.xenit.io
kubectl delete namespace dhkey-operator
```

### Manifests

#### Install manifests (cluster scoped)

```shell
kubectl apply -f deploy/crds/xenit.io_dhkeys_crd.yaml
kubectl create ns dhkey-operator
kubectl -n dhkey-operator apply -f deploy/service_account.yaml
kubectl -n dhkey-operator apply -f deploy/cluster_role.yaml
kubectl -n dhkey-operator apply -f deploy/cluster_role_binding.yaml
kubectl -n dhkey-operator apply -f deploy/operator.yaml
```

#### Uninstall manifests (cluster scoped)

```shell
kubectl -n dhkey-operator delete -f deploy/service_account.yaml
kubectl -n dhkey-operator delete -f deploy/cluster_role.yaml
kubectl -n dhkey-operator delete -f deploy/cluster_role_binding.yaml
kubectl -n dhkey-operator delete -f deploy/operator.yaml
kubectl delete ns dhkey-operator
kubectl delete -f deploy/crds/xenit.io_dhkeys_crd.yaml
```