# 쿠버네티스 인트로

## 배경
- 도커: 컨테이너 런타임(실행환경) 중 하나
  - 그외: 컨테이너디, cri-o   
- 컨테이저 런타임이 설치된 여러 대의 호스트를 중앙에서 통합,관리,동작시키고 싶음 = 컨테이너 오케스트레이션
- 쿠버네티스: 컨테이너 오케스트레이션 엔진
  - 그외: 도커스웜, 아파치 메소스

## 쿠버네티스는 사실상의 표준
- CNCF (클라우드 네이티브 컴퓨팅 파운데이션) 로 이관되어 Graduated로 분류됨
- gcp, azure, aws 등 대형 클라우드 사업자가 쿠버네티스 관리형 서비스를 제공

## 주요 장점, 기능
- 선언적 코드(=메니페스트)를 사용한 인프라 관리 (infra as code, iac)
- (오토) 스케일링
  - 같은 이미지로 여러 컨테이너(=레플리카)를 배포하면 부하 부난 및 다중화 구조 가능 
- 스케줄링
  - 컨테이너를 어떤 노드에 배포할 것인지 결정
  - 워크로드 특징이나 쿠버네티스 노드의 성능, 여유 리소스 상태 등을 기준으로 
- 자동화 복구
  - 프로세스 정지를 감지하면 자동으로 재배포
  - 프로세스 모니터링 외에 헬스체크 설정도 가능 
- 로드밸런싱
  - 서비스 또는 인그레스가 로드밸런서,엔드포인트 기능
- 서비스 디스커버리
  - MSA 환경에서 각각의 마이크로서비스가 서로를 참조할때 유용 
- 데이터 관리
  - 컨테이너가 사용하는 설정이나 데이터베이스 인증 정보 등을 안전하게 관리 가능   

## 환경
- 로컬 (1개 머신)
  - 미니큐브, 도커데스크탑, 카인드
- 구축 도구
  - 큐브어드민, 랜처 
- 퍼블릭 클라우드 관리형    
  - gke, aks, eks
 
#### 참고 
- 쿠버네티스 완벽가이드
- - 유튜브 44bits https://www.youtube.com/watch?v=Ia8IfowgU7s
