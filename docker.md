# 도커 정리

## Basics
### 정의
* **컨테이너** 기반의 오픈소스 가상화 플랫폼

#### 서버 개발과 관리의 어려운 점
* 서버 컴퓨터에 프로그램 설치 - 예) DBMS, gitlab 등
* 계속해서 서버 환경이 바뀜 - 예) 인프라 변경
* 계속해서 개발 환경이 바뀜 - 예) node.js, python 등

#### 해결하기 위한 노력들과 한계
* 문서 - 관리가 어려움
* 설정 파일 - 한 서버에 다른 버전 여러개 설치 어려움
* 리눅스를 이용한 자원 격리 - 배우기 어려움
* 가상머신 - 용량이 크고 느림

#### 도커가 어떻게 해결했나?
* 리눅스 커널 활용
* 서버를 코드와 설정으로 관리하여 재현 및 수정 가능

#### why 도커?
* 이미지를 이용
 * 이미지를 이용하여 컨테이너를 생헝함
 * Dockerfile 스크립트를 이용
 * 빌드서버에서 이미지 생성 및 저장 -> 운영서버에서 이미지를 다운로드
* 서버 이미지를 다운로드해서 컨테이너로 실행하면
 * msa 의 경우 버전관리 용이
 * 개발용 db 사용 용이
 * 가상 운영체제 
* 배포 과정 표준화
* 자원관리
 * aws S3 같은 별도 저장소 필요, 세션 및 캐시는 redis 등으로 분리 

## 용어
* 이미지 : 프로세스가 실행되는 파일의 집합
  * 구성
   * 베이스 이미지 : 읽기전용
   * rootfs : 추가/수정/삭제 가능
    * 기존 이미지 `run` -> 수정 -> `commit` -> 새 이미지 생성 가능 
   * 이름 규칙
    * `이름공간/이미지이름:태그`
* 컨테이너 : 이미지를 이용해서 실행하는 독립된 실행환경 또는 프로세스

## 도커 명령어
* 도커이미지 (새로) 만들기
`docker build -t 이미지이름 .`
 * 현재 디렉터리의 도커파일을 이용해서 만들어짐
 * 도커파일 : 도커 이미지를 만들 때 사용되는 파일
* 도커 이미지 (다운로드 받고) 실행하기
`docker run 이미지 이름 .`
* 도커 이미지 다운로드 받기
`docker pull 이미지이름`
* 다운로드 받은 도커 이미지 목록 확인
`docker images`
* 실행중인 컨테이너 목록 확인
`docker ps [-a]`
* 컨테이너에 접속
`docker exec 이미지이름`

## 도커 컴포즈
* 도커 명령어와 옵션들을 yml 파일로 만들어 `docker-compose up` 으로 컨테이너 실행할 수 있음
  * CLI 보다 편리함
* 여러 개의 컨테이너를 같이 실행할 수도 있음

### 참고
[Dockerfile best practices](https://sysdig.com/blog/dockerfile-best-practices/)
[인프런 초보를 위한 도커 안내서(subicura)](https://www.inflearn.com/course/%EB%8F%84%EC%BB%A4-%EC%9E%85%EB%AC%B8/dashboard)
