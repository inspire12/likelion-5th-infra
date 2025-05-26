# Kubernetes (K8s)

# 구성요소
## node 
## namespace

## pod
## service
## deployment 


# k8s 시스템 상태 확인 
```
kubectl cluster-info
kubectl get nodes
kubectl get namespace
kubectl describe node docker-desktop
```

# Pod
imagePullPolicy: IfNotPresent 설정 필요

## Pod 생성 튜토리얼
```
kubectl create deployment kubia --image=luksa/kubia:1.0
```

local 빌드한 도커를 사용하는 방법 
```
kubectl create deployment my-deployment \
  --image=inspire12-quiz:latest \
  --dry-run=client -o json |
  jq '.spec.template.spec.containers[0].imagePullPolicy = "IfNotPresent"' |
  kubectl apply -f -
```

## load balancer 
```
kubectl expose deployment kubia --type=LoadBalancer --port 8080
```
