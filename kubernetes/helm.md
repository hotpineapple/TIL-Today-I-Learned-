# Helm

## content
- main concept
- basic common principles and use cases
- +) changes a lot between versions, so that additional search will be needed further

## Overview
- Helm
- Helm chart
- 사용법
- 언제 사용하는지
- Tiller

## Helm
### functionality #1 package manager for Kubernetes
- yaml file 를 package 하고 public/private repository에 distribute 함
- 예) elastic stack for logging 을 클러스터에 추가하고 싶을 때
  - stateful set, config map, k8s users with permission, secret, services 에 대한 yaml 파일을 직접 찾아서 추가하는 것은 귀찮고, 사실상 표준화 되어 있기 때문에 다른 많은 사람들이 똑같은 방식으로 한다
  - 누군가가 yaml 파일들을 모아 package로 만들어 다른 사람들이 사용할 수 있도록 한다면?(sharing helm charts) good
  - 이 때 이 buncle of yaml files 를 helm chart 라고 함

### Helm chart
- bundle of yaml files
- create your own Helm charts with helm
- push them into helm repositoy
- download and use existing ones
- 예) database apps, monitoring apps(prometheus) ... are avaiable! you can reuse that configuration
  - `helm search <keyword>` on command line OR
  - visit helm hub (public repository)

### functionality #2 Templating engine
- 여러개의 microservices 로 이루어진 앱을 쿠버네티스 클러스터에 배포하고 싶을 때
- 각각의 microservice 의 yaml 파일은 이름, 버전 등 빼고는 대부분 비슷할 것(apiVersion, kind, metadate 등)
- => helm을 이용하여 common blueprint 를 정의할 수 있다
  - dynamic values 는 placeholder `{{.Values..}}` 로 작성함
  - placeholder는 실제 값이 정의돈 values.yaml 파일을 이용하거나 command line의 --set 플래그를 이용하여 넣어준다
  - 즉 여러개의 yaml 파일 대신 한개의 yaml 파일(template)과 여러개의 value 파일로 구성할 수 있음
  - CI/CD 관점에서, 새로 빌드할 필요 없이 value 만 on the fly 로 바꿔서 배포할 수 있다는 징점

#### additional functionality: deploy same app across different k8s cluster environment (immutablity)
- development, staging, production cluster 가 있음
- 각각의 클러스터에 분리된 yaml 파일을 배포하는 대신
- own chart로 package 해서 one command 로 사용할 수 있다

## Helm chart 구조
- 차트이름/
  - chart.yaml: 차트에 대한 meta 정보(이름, 버전, 의존성)
  - values.yaml: 템플릿 파일을 위한 values, 이 값은 default value로, override 할 수 있다
  - charts/: 차트 의존성을 포함하는 폴더
  - templates/: 템플릿 파일을 포함하는 폴더
  - +) readmn, license 파일도 추가할 수 있다
- `helm install <chartname>` 실행하면 values 파일을 이용하여 template 파일이 채워짐
- `helm install --values=my-values.yaml <chartname>` 을 사용하면 values 파일을 이용하여 template 파일이 채워진 다음ㄴ my-values로 override 됨

### functionality #3 Release management
- helm version 2
  - client(helm cli) - server(tiller) 구조
  - 클라이언트가 요청을 보내면 tiller 가 서버가 복사본을 저장해놓고 k8s 클러스터에 처리함
  - once create or change depolyment => keep trackint of all chart executions
  - rollback 가능
  - 단점: too much power inside of k8s cluster
  - security issue
- helm version 3
  - remove tiller
  - simple binary
  - lose release management func.

#### 출처
https://www.youtube.com/watch?v=-ykwb1d0DXU
