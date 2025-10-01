# Lab 02: Exposição com NodePort

## Objetivo

- Expor uma aplicação Kubernetes para fora do cluster usando `Service` do tipo `NodePort`.
- Acessar a aplicação via navegador.

## Passos

```bash
# Criar namespace
kubectl create namespace lab-02

# Aplicar manifests
kubectl apply -f deployment.yaml -f service.yaml -n lab-02

# Obter IP do Minikube
minikube ip

# Acessar no navegador: http://<IP>:30080
```
