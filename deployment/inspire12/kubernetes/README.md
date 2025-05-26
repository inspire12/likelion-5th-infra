# Kubernetes (K8s)

# 구성요소
## node 
## namespace

- pod
- service
- deployment 


# k8s 시스템 상태 확인 
```
kubectl cluster-info
kubectl get nodes
kubectl get namespace
kubectl describe node docker-desktop
```

### metric server 설치 
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl edit deployment metrics-server -n kube-system
        - --metric-resolution=15s 아래에 - --kubelet-insecure-tls # 추가
kubectl get pods -n kube-system -l k8s-app=metrics-server
```

# Pod
imagePullPolicy: IfNotPresent 설정 필요

## Pod 생성 튜토리얼
```
kubectl create deployment kubia --image=luksa/kubia:1.0
```

local 빌드한 도커를 사용하는 방법 
```
kubectl create deployment kubia \
  --image=luksa/kubia:1.0 \
  --dry-run=client -o json |
  jq '.spec.template.spec.containers[0].imagePullPolicy = "IfNotPresent"' |
  kubectl apply -f 
```

## Kubectl expose 
### Load balancer 
```
kubectl expose deployment kubia --type=LoadBalancer --port 8080
```

### ClusterIP

### ingress nginx
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.0/deploy/static/provider/cloud/deploy.yaml

```

## 수평적 확장 (Horizontal Scaling)
```
kubectl scale deployment kubia --replicas=3
```

## 자동 확장 (HPA)
```
kubectl autoscale deployment kubia \
  --cpu-percent=50 --min=1 --max=5
```