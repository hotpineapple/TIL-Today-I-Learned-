# 큐브어드민으로 배포하기

## 호스트 환경
- A compatible Linux host. 
- 2 GB or more of RAM per machine (any less will leave little room for your apps).
- 2 CPUs or more.
- Swap disabled. You MUST disable swap in order for the kubelet to work properly

## 과정
1. 모든노드에 도커, 컨테이너 런타임 인터페이스(dockerd, containerd ..), kubeadm, kubectl, kubelet 설치
2. 마스터노드에서 클러스터 생성(=kubeadm 초기화) `kubeadm init -- ...`
3. cni(calico, weave ..) 설정
4. 워커 노드에서 클러스터 조인 `kubeadm join ...`
5. 매니페스트 작성 및 kubectl apply

## 유의할 점
- cgroup 동기화
- aws ec2 에 클러스터 설치할 경우 보안그룹 설정

#### 참고 
- https://www.coachdevops.com/2020/06/how-to-setup-kubernetes-cluster-in.html
