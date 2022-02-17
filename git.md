# 1. git 커맨드 정리

1. git 원격 브랜치 로컬에 복사하기
`git checkout -t [원격 저장소의 branch 이름]`

1-1. git 원격 브랜치 새로 만들어서 로컬 파일 올리기
``

2. 작업 브랜치 따기
``

3. 작업 브랜치에서 메인 브랜치로 병합하기
``

4. 충돌날 경우 리베이스 하기

깃 리베이스
메인 브랜치 git pull
작업 브랜치 git rebase 메인브랜치이름
git status 로 충돌 파일 확인, 충돌 해결


rebase
작업 중이던 브랜치가 바라보는 위치를 재조정하는 것으로 이전 커밋부터 차례로 Conflict 를 해결한 뒤 force push 한다
마스터 브랜치 이동 후 git pull
작업 브랜치 이동 후 git rebase 마스터 브랜치 이름
충돌 제거, 저장
git add src
git rebase --continue
반복
git push origin -f 작업 브랜치 이름

5. 불필요하거나 사소한 커밋들 합치기
`git rebase -i HEAD~[커밋개수]`
pick -> s 변경 
:wq 혹은 ctrl + x -> y -> enter
커밋 메시지 설정
:wq 혹은 ctrl + x -> y -> enter
`git log`로 확인

6. 기타 팁
git fetch --all
git commit -amend
git diff --staged
`git log`

# 2. git 트러블 슈팅
* 최신버전 다운받으려고 하면 충돌 생길 때
  *  첫 번째 방법
      수정사항 정리 후 다른 브랜치로 이동
    git branch -D 브랜치이름
    git checkout 브랜치이름 하면 로컬 브랜치로 땡겨옴
   * 두 번째 방법
    git reset --hard origin/브랜치이름
    
* 뭔가 꼬여서 merge 실패 시 
  * 해결 방법 : 백업하고 리셋 후 필요한 커밋 체리픽
git checkout -b backup
깃 체리픽
refactor/apply_generic 브랜치를 origin/feature/mpm 으로 reset
git reset --hard origin/feature/mpm

그 다음에 작업한 commit들을 체리픽
git cherry-pick a06bafe6f7513ebc35c7d6101246ec75f74a6565 2a115496d126cdb9d905da19e6aa29db40d938e2 ...
