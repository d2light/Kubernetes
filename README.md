# Kubernetes

## 1일차
### 2025-08-21
### 1. yaml 파일 생성
매니페스트 파일(Manifest File)
```
apiVersion: v1 # Pod -> v1
kind: Pod # 리소스 종류

metadata:
  name: nginx-pod # pod 명칭

spec:
  containers:
  - name: nginx-container # container 명칭
    image: nginx:latest # :latest 안쓰면 알아서 
    ports:
      - containerPort: 80 # 가독성을 위한 작성
```

### 2. 실행
``` CLI
1. Pod 생성
kubectl apply -f nginx-pod.yaml
2. Pod 정보 확인
kubectl get pods
3. kubectl exec -it nginx-pod -- bash
=> curl localhost:80
4. 포트 포워딩
(sudo) kubectl port-forward pod/nginx-pod 80:80
5. Pod 삭제
kubectl delete pod nginx-pod
```