# 도커 정리

## Basics
#### 서버 운영
* ㅇ

#### Why 도커?
* 이미지를 이용하여 서버 운영 편리
* 서버 이미지를 다운로드해서 컨테이너로 실행하면
 * msa 의 경우 버전관리 용이
 * 개발용 db 사용 용이
 * 가상 운영체제 

## 용어
* 이미지 : 프로세스가 실행되는 파일의 집합
  * 구성
   * 베이스 이미지 : 읽기전용
   * rootfs : 추가/수정/삭제 가능
    * 기존 이미지 `run` -> 수정 -> `commit` -> 새 이미지 생성 가능 
   * 이름 규칙
    * `이름공간/이미지이름:태그`
* 컨테이너 : 이미지를 이용해서 실행하는 독립된 실행환경 

## 도커 명령어
* 도커이미지 (새로) 만들기
`docker build -t 이름 .`
 * 현재 디렉터리의 도커파일을 이용해서 만들어짐
 * 도커파일 : 도커 이미지를 만들 때 사용되는 파일
* 도커 이미지 (다운로드 받고) 실행하기
`docker run 이미지 이름 .`
* 도커 이미지 다운로드 받기
`docker pull`
* 다운로드 받은 도커 이미지 목록 확인
`docker images`
* 실행중인 컨테이너 목록 확인
* `docker ps [-a]`


## 도커 컴포즈
* 도커 명령어와 옵션들을 yml 파일로 만들어 `docker-compose up` 으로 컨테이너 실행할 수 있음
  * CLI 보다 편리함
* 여러 개의 컨테이너를 같이 실행할 수도 있음

### 참고
[Dockerfile best practices](https://sysdig.com/blog/dockerfile-best-practices/)
[인프런 초보를 위한 도커 안내서(subicura)](https://www.inflearn.com/course/%EB%8F%84%EC%BB%A4-%EC%9E%85%EB%AC%B8/dashboard)
