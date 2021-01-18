Git
1. 저장소(repository) 만들기 - git이 관리하는 폴더(directory)
명령어 git init
2. 파일 스테이징하기
명령어 git add 파일명
3. 파일을 커밋하기
명령어 git commit -m ''
4. 상태 확인하기
명령어 git status
5. 내가 누군지 정보 입력하기
명령어 git config --global user.name 유저이름
명령어 git config --global user.email 이메일
6. 커밋 기록확인하기
명령어 git log --oneline

원격(remote) 저장소
- github - 무료, 개인
- gitlab(ssafy)-유료, 회사
- bitbucket

폴더(repository)단위로 관리됨

내컴퓨터의 저장소 -> github의 내가 만든 원격 저장소

$ git remote add origin https://github.com/kangjunluck/TIL.git
$ git remote add 다른이름 다른 주소                       다른 곳에 등록 방법
: 원격 저장소를 지정 : orgin 이라 하겠다 그 주소를 ----

$ git push origin master
: origin에 



원격 저장소 repo 컴퓨터로 다운로드

새 폴더에 우클릭 vscode나 git 띄우고, 

$ git clone 'github에 code 주소' 붙여넣기 (shift )



