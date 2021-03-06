자주 사용하는 git command & 문제 해결 방법
===================================

## add / commit / push 취소하기
- HEAD : 항상 작업트리의 가자 최근 커밋을 가리킨다.

#### add 취소
  git status
- 파일 상태 확인
  git reset HEAD [filename]
- 파일 이름을 명시하지 않으면 staging area에 존재하는 모든 것이 unstaged 된다


#### commit 취소
  git log // or git shortlog
- 커밋 목록 확인
  git reset [option] HEAD[^ / ~{지울 커밋 개수}]
> option 종류
> 1. --soft : commit은 취소하고 해당 파일들은 staged 상태로 working directory에 보존
> 2. --mixed : commit은 취소하고 해당 파일들은 unstaged 상태로 working directory에 보존
> 3. --hard : commit은 취소하고 해당 파일들은 unstaged 상채로 working directory에서 삭제
> 
> 예시
> 
>     git reset --mixed HEAD^ // default
>     git reset HEAD^ // 위와 동일
>     git reset HEAD~1 // 마지막 1개의 commit 취소. 위와 동일
> 

만약 현재 디렉토리 (working directory)를 원격 저장소의 마지막 commit 상태로 되돌리고 싶다면?

    git reset --hard HEAD // 마지막 commit 이후로 추가한 파일들은 사라진다.


#### commit message 변경
  git commit --amend


#### push 취소 (사실상 새로 덮어쓰기: 아주 주의할 것. 동기화 문제가 생길 가능성이 매우 높다)
    
    git log -g // commit list 확인
    git reset HEAD@{number} or git reset [commit id] // 원하는 시점으로 working directory를 되돌린다.
    git commit -m "push force" // 원하는 상태에서 commit
    git push origin [branch name] -f or git push origin +[branch name] // 강제로 덮어쓰기
    
    
#### git clean
  git이 추적 중이지 않은 파일들 지운다. 
  그러나, .gitignore 에 명시되어 있는 파일은 지우지 않는다.
  
    git clean -f // delete files excepting directory
    git clean -f -d // delete files including directory
    git clean -f -d -x // .gitignore 에 명시된 파일들도 모두 삭제
    // -n option : 가상으로 실행해보고, 어떤 파일들이 지워질지 알려주는 옵션★
    
    
    
## fork repo와 origin repo 동기화하는 방법

1. 원본 저장소 등록하기
    
       git remote -v // 등록된 저장소 확인
       git remote add upstream [원격 저장소.git] // 원격 저장소를 upstream 으로 등록
    
2. 원본 저장소 변경분 로컬로 가져오기

       git fetch upstream [파일이 저장된 원본 저장소의 브랜치 이름] // 여기서는 main 이라 하겠음
     
3. 로컬에서 원본 저장소와 포크한 저장소 병합하기

       git checkout haneul // 포크한 저장소의 로컬 브랜치로 이동
       git merge upstream/main // 현재 저장소에(origin/haneul) 원본 브랜치를(upstream/main) merge
    
4. 포크한 저장소를 원격 git에 업데이트하기

       git push origin haneul
  
  
## git rm

    git rm [file] // file을 현재 working directory 에서도, 원격 저장소에서도 지운다.
    git rm --cached [file] // file을 원격 저장소에서만 지운다.
    
    git commit -m "delete file"
    git push origin main
    
    

