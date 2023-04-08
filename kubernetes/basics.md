# 쿠버네티스 정리

## 메인 컴포넌트

- abstraction level
  - deployment > replicaset > pod > container
  - once user manages deployment, components below deployment is
    automatically controlled by k8s

1. pod: abstraction of container, smallest unit
2. service: communication, load balance

- pod은 시작할 때마다 다른 ip주소를 가지므로 pod 의 ip를 사용하여 다른 pod 또는 노드와 통신하기에 번거로움.

3. ingress: route trafic to cluster, access only by domain name, http's'
4. configMap: external configuration
5. secrets: secure configration
6. volume: persist data using other storage either in node which pod is running or remote(outside of k8s cluster)
7. deployment: pod의 추상화

- blueprinting for creasting pods so that used to replicate mechanism
- 안정적인 서비스제공, 무중단 배포에 유용

8. statefulSet: replicate for stateful app or db

## 아키텍처

- 크게 master node와 slave(=worker) node 로 나뉨

### worker node

- actually run services
- 3 processes must be installed
- container runtime(ex.docker)
- kubelet
  - interact with container and node
- kubeproxy
  - forward request from services to pods

### masternode

- schedule/monitor/re-start pod/join new node
- 4 process must be installed
- api server
  - cluster gateway
  - authentication
- scheduler
  - where to put the pod (then kubelet on workernode actually starts)
- controller (manager)
  - detect state change (node die, crashes) and request scheduler to reschedule
- etcd
  - key-value store for cluster state changes

## minikube

- production k8s cluster setup
  - multiple master and worker nodes
  - separate vm or physical machines
- test/local k8s cluster setup (procuction 과 비교하여 간단)
  - master and worker as process in one machine(=node) w/ docker pre-installed
  - creates virutal box and node runs in this virtual box
- `minikube start`

## kubectl

- interact with cluster
- command line tool
- 3 type client of master node's api server
  - ui
  - api
  - cli -> kubectl, most powerful

### 주요 kubectl commands

- `kubectl get nodes/pod/services/deployment/replicatset`
- `kubectl create deployment exampledeployanme --image=exampeimage`
- `kubectl edit deployment exampledeployanme`
- `kubectl logs examplepodname`
- `kubectl exec -it examplepodname -- bin/bash`
- `kubectl delete deployment exampledeployanme`
- `kubectl apply | delete -f examplefilename`
  - kubectl create 의 옵션을 yaml 파일로 관리

## namespace

- organize or group resources
  - ex. database, monitoring, elastic stack, nginx-ingress
- cluster in cluster
- `kubectl get namespace`
- `kubectl get XXX -n examplenamespacename`

## ingress

### ingress yaml configuration

- service.yaml 유의
  - nodePort 필요없음
  - type 은 명시하지 않거나 default type(ClusterIP)
    - Loadbalancer (X)

```yaml
apiVersion: ...
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    - host: myapp.com
      http:
        paths:
          - backend:
              serviceName: my-service
              servicePort: 8080 # corresponds to service.yaml 's spec.ports.port
```

### ingress controller

- a pod which is entry point to cluster
- evaluates rules
- and then processs Ingress rules(=manage redirections)
- many third-party implementations besides k8s nginx ingress controller
- ingress controller in minikube
  - `minikube addons enable ingress`

## helm (chart)

- package manager
  - package: pod 별 statefulSet, etcd 등 컴포넌트 
- template and default value for similar yaml files
