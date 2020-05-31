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
kubectl apply -f deploy/crds/dhkey.xenit.io_dhkeys_crd.yaml
kubectl apply -f deploy/crds/dhkey.xenit.io_v1alpha1_dhkey_cr.yaml
operator-sdk run local
```

### Reverting after local runs

```shell
kubectl delete -f deploy/crds/dhkey.xenit.io_dhkeys_crd.yaml
kubectl delete -f deploy/crds/dhkey.xenit.io_v1alpha1_dhkey_cr.yaml
```