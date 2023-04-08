# 미니큐브로 배포하기
## 호스트 최소 환경
- 2 CPUs or more
- 2GB of free memory
- 20GB of free disk space
- Internet connection

## 과정
1. 호스트에 도커, 미니큐브, kubectl 설치
2. `minikube start`
3. 메니페스트 파일 작성
4. `kubectl apply -f 파일이름`

## 기타
- 클러스터 확인 `kubectl get po`
- 로컬에서만 접근 가능한지
  ```text
  Minikube is a tool that makes it easy to run Kubernetes locally. 
  Minikube runs a single-node Kubernetes cluster inside a Virtual Machine (VM) on your laptop for users looking to try out Kubernetes or develop with it day-to-day. 
  ```
