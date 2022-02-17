# 1. 케이스 정리

## 아무것도 없는 상태에서 새로 시작할 때

- git 원격 브랜치 새로 만들어서 로컬 파일 올리기
1. 로컬 파일이 있는 폴더로 이동
    
    `cd [폴더 경로]`
    
2. 유저 이름 설정
    
    `git config --global user.name "[유저네임]"`
    
3.  유저 이메일 설정
    
    `git config --global user.email "[유저이메일]"`
    
4.  깃 초기화
    
    `git init`
    
5. 원격 브랜치 생성
    
    `git remote add origin [브랜치 url]`
    
6. 작업을 한다
7. 스태이징
    
    `git add src/파일이름`
    
8. 커밋
    
    `git commit -m "[커밋 내용]"`
    
9. 원격에 올리기
    
    `git push origin master`
    

## 이미 있는 프로젝트 가져와서 협업하기

1. git 원격 브랜치 로컬에 복사하기
    
    `git checkout -t [원격 저장소의 branch 이름]`
    
2. 작업 브랜치 따고(만들고) 그 브랜치로 이동
    
    `git checkout -b [작업 브랜치 이름]`
    
3. 작업을 한다
4. 스태이징
    
    `git add src/파일이름`
    
5. 커밋
    
    `git commit -m "[커밋 내용]"`
    
6. 원격에 올리기
    
    `git push origin master`
    
7. 작업 브랜치를 메인 브랜치에 병합하기
    
    `git merge [메인 브랜치 이름]`
    
8. (필요한 경우) 리베이스 하기
    - rebase란?
        - 작업 중이던 브랜치가 바라보는 위치를 재조정하는 것
        - 작업브랜치가 처음에 파생된 메인 브랜치가 내가 작업하는 동안 다른 커밋에 의해 변경되는 경우
            - 작업브랜치가  파생된 상태를 현재 상태로 바꾸는 것
    
    8-1. 메인 브랜치 이동
    
    `git checkout [메인 브랜치 이름]`
    
    8-2. 메인 브랜치 원격에서 가져오기
    
    `git fetch -all [메인 브랜치 이름]`
    
    `git pull [메인 브랜치 이름]`  
    
    8-3. 작업 브랜치로 이동하기
    
    `git checkout [작업 브랜치 이름]`
    
    8-4. 작업 브랜치의 베이스를 조정하기
    
    `git rebase [메인브랜치 이름]`
    
    8-5. 충돌 여부, 위치 확인
    
    - 작업 브랜치 파생 이후 메인브랜치에 병합된 커밋과 작업 브랜치가 같은 곳을 변경한 경우 충돌 발생함
    
    `git status` 로 충돌 발생한 파일 확인, 없으면 4-8로 이동
    
    8-6. 충돌 해결
    
    - incoming change  : 작업 브랜치에서 변경한 것
    - current change : 메인 브랜치의 현재 상태
    - 코드를 충분히 이해하여 옳은 코드를 선택하는 것이 필요
    
    8-7. 소스코드 스테이징
    
    8-8. 5~7번 반복
    
    `git rebase --continue`
    
    8-8. 소스코드 원격에 올리기
    
    `git push -f [작업 브랜치 이름]`
    

# 2. git 트러블 슈팅

### 메인 브랜치 최신버전 다운받으려고 하면 충돌 생길 때

- 첫 번째 방법
수정사항 정리 후 다른 브랜치로 이동
git branch -D 브랜치이름
git checkout 브랜치이름 하면 로컬 브랜치로 땡겨옴
- 두 번째 방법
git reset --hard origin/브랜치이름

### 리베이스 중 뭔가 꼬여서 젠킨스 merge 실패 평가될 때

- 백업하고 리셋 후 필요한 커밋 골라서 체리픽
    - 작업 브랜치 이동
    - git checkout -b backup
    - 작업 브랜치를 파생된 메인 브랜치의 현재 상태로 리셋
    `git reset --hard origin/[메인 브랜치 이름]`
    - 그 다음에 작업한 commit들을 체리픽
    `git cherry-pick [커밋 아이디]`

# 3. 기타 팁

### 불필요하거나 사소한 커밋들 합치기

- `git rebase -i HEAD~[커밋개수]`
- pick -> s 변경
- :wq 혹은 ctrl + x -> y -> enter
- 커밋 메시지 설정
- :wq 혹은 ctrl + x -> y -> enter
- `git log`로 확인

### 최근 커밋에 합치기

`git commit -amend`

### 변경 사항 확인하기

`git diff --staged`

### 깃 커맨드 히스토리 확인하기

`git log`
