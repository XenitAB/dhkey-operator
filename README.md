# dhkey-operator 

Operator to handle dh (diffie hellman) keys in Kubernetes clusters.

## Running locally

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

### Install

```shell
kubectl apply -f deploy/crds/xenit.io_dhkeys_crd.yaml
kubectl create ns dhkey-operator
kubectl -n dhkey-operator apply -f deploy/service_account.yaml
kubectl -n dhkey-operator apply -f deploy/role.yaml
kubectl -n dhkey-operator apply -f deploy/role_binding.yaml
kubectl -n dhkey-operator apply -f deploy/operator.yaml
```

## Uninstall

```shell
kubectl -n dhkey-operator delete -f deploy/service_account.yaml
kubectl -n dhkey-operator delete -f deploy/role.yaml
kubectl -n dhkey-operator delete -f deploy/role_binding.yaml
kubectl -n dhkey-operator delete -f deploy/operator.yaml
kubectl delete ns dhkey-operator
kubectl delete -f deploy/crds/xenit.io_dhkeys_crd.yaml
```