# 큐브어드민으로 배포하기

## 호스트 환경
- A compatible Linux host. 
- 2 GB or more of RAM per machine (any less will leave little room for your apps).
- 2 CPUs or more.
- Swap disabled. You MUST disable swap in order for the kubelet to work properly

## 과정
1. 모든노드에 도커, dockerd(컨테이너 런타임 인터페이스), kubeadm, kubectl, kubelet 설치
2. 마스터노드에서 클러스터 생성(=kubeadm 초기화) `kubeadm init -- ...`
3. 워커 노드에서 클러스터 조인 `kubeadm join ...`
4. 플라넬 설정
5. 랜처 설정

## 기타
- cidr
- gpg 저장소
- 가상 네트워크

#### 참고 
https://www.youtube.com/watch?v=o6bxo0Oeg6o
https://github.com/Mirantis/cri-dockerd
