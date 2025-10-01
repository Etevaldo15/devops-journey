# Lab 01: Hello World Kubernetes

## Objetivo

- Criar um Deployment com 2 réplicas do Nginx.
- Expor via Service do tipo ClusterIP.
- Isolar tudo em um namespace dedicado (`lab-01`).

## Comandos

```bash
# Criar namespace
kubectl create namespace lab-01

# Aplicar deployment e service
kubectl apply -f deployment.yaml -f service.yaml

# Verificar pods
kubectl get pods -n lab-01

# Verificar service
kubectl get svc -n lab-01

# Acessar o serviço (dentro do cluster)
kubectl run test -n lab-01 --image=busybox --rm -it --restart=Never -- wget -O- http://hello-nginx-service
```

> ⚠️ Nota sobre imagens locais:  
> Para evitar `ImagePullBackOff`, certifique-se de que a imagem está carregada no Docker do Minikube e use `imagePullPolicy: Never`.  
> Comandos úteis:
>
> ```bash
> eval $(minikube docker-env)
> docker images | grep nginx
> ```

## ⚠️ Solução para `ErrImageNeverPull`

Esse erro ocorre quando a imagem não está carregada no Docker do Minikube.  
Para corrigir:

```bash
eval $(minikube docker-env)
docker pull nginx:1.21
kubectl delete deployment nginx-deployment -n lab-01
kubectl apply -f deployment.yaml -n lab-01
```

## 🌐 Acesso Externo com NodePort

Para expor o serviço fora do cluster:

```yaml
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

---

## 🧪 DICA EXTRA: Como funciona no Windows?

Se você estiver no **Windows com Docker Desktop**, o Minikube geralmente usa `127.0.0.1` como IP:

```bash
curl http://127.0.0.1:30080
```
