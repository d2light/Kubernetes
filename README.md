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

6. demo 파일 생성 
https://start.spring.io/
의존성에 Spring Web / Spring Boot DevTools 추가

7. Dockerfile 생성

FROM openjdk:17-jdk

COPY build/libs/*SNAPSHOT.jar app.jar

ENTRYPOINT ["java", "-jar", "/app.jar"]

8. 빌드
gradlew clean build

9. image 생성
docker build -t spring-server .

10. iamge 생성 확인
docker image ls

11. sprint-pod.yaml 생성
apiVersion: v1
kind: Pod

metadate:
  name: spring-pod

spec:
  containers:
    - name: spring-container
      image: spring-server
      ports:
        - containerPort: 8080

12. kubectl apply -f sprint-pod.yaml

13. kubectl get pods
NAME         READY   STATUS             RESTARTS   AGE
spring-pod   0/1     ImagePullBackOff   0          19s

==========================================================
* 이미지 풀 정책 
1) Always: 로컬에서 이미지를 가져오지 않고, 무조건 레지스트리(Dockerhub)에서 가져옴
2) IfNotPresent: 로컬에서 먼저 이미지를 가져오고 없는 경우 레지스트리에서 가져옴
3) Never: 로컬에서만 이미지를 가져옴
- 이미지의 태그가 latest 이거나 명시되지 않은 경우: imagePullPolicy -> Always
- 이미지의 태그가 latest가 아닌경우: imagePullPolicy -> IfNotPresent
==========================================================

14. pod 제거
kubectl delete pod spring-pod

15. yaml 수정
apiVersion: v1
kind: Pod

metadata:
  name: spring-pod

spec:
  containers:
    - name: spring-container
      image: spring-server
      ports:
        - containerPort: 8080
      imagePullPolicy: IfNotPresent ## Policy 추가

16. Pod 내부로 들어가 요청
kubectl exec -it spring-pod -- bash

17. 포트포워팅 후 요청
kubectl port-forward pod/spring-pod 12345:8080
=> 접속포트 12345
```


### 3. Pod 생성요약

#### 1) 프로젝트 파일 생성

#### 2) Dockerfile 생성
```
docker build ~
```
#### 3) manifest 파일(yaml) 생성
```
kubectl apply -f ~
```
#### 4) 포트 포워팅
```
kubectl port-forward web-server-pod 5000:80
```

## 명령어 정리
#### 1. kubectl get pods : pod 정보 확인
#### 2. kubectl apply -f [pod 이름] : pod 생성
#### 3. kubectl delete pod [pod 이름] : pod 삭제

---
## 2일차
### 2025-09-03
### Zero to JupyterHub with Kubenetes

#### 선행 조건
#### 1) Kubernetes 설치
#### 2) Docker Desktop 설치

### 설치 과정

### 1. Helm 
#### 1) Chocolatey 설치
```
Set-ExecutionPolicy AllSigned
```
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
#### 2) Helm 설치
```
choco install kubernetes-helm
```
```
helm version
```

### 2. config 파일 생성
```
hub:
  config:
    Authenticator:
      admin_users:
        - adminuser1
        - adminuser2

scheduling:
  userScheduler:
    enabled: false

```

### 3. jupyterhub 설치
```
helm install jupyterhub jupyterhub/jupyterhub --create-namespace --namespace jupyterhub --values config.yaml
```

### 4. Port-Forwarding
```
kubectl --namespace=jupyterhub port-forward service/proxy-public 8090:http
```