---

title:  "[Github-Pages]github Pages 설정 과정 - 수정중"
excerpt: "github계정부터 설정과정 정리하기"

date: 2021-07-25
last_modified_at: 2021-07-29

use_math: true
comments: true

categories:
  - Blog
---

## github Pages 설정

1. github 계정만들기

   - https://github.com/ 에 접속하여 계정을 만듭니다. 나중에 취직 또는 이직할 때 공개되어질 수 있으니 장난스러운 블로그명은 지양하도록 합니다.

2. Git 설치

   - Git이란 여러명이서 협력하여 프로젝트를 진행할 때 소스버전관리를 위해 필요한 software입니다. 그리고 Github은 Git을 쉽게 사용하도록 웹서비스를 제공하는 버전 관리 툴입니다.

   - Git을 설치하여 cmd창에서 git명령어로 포스팅 글들을 git저장소에 업로드하고 git저장소에서 가져오기도 할 것입니다.  
   - https://git-scm.com/에 접속하여 운영체제에 맞게 설치해줍니다.
   - 설치를 진행하고 나면 문서탐색기에서 마우스 우클릭을 해주면 다음과 같이 Git Bash Here라는 목록이 추가됩니다. 

   <img src="/assets/images/09_Github-Pages_1.png?raw=true" alt="XX_githubPages_1.png" style="zoom:67%;" />

   - Github 블로그를 관리할 폴더를 생성하고 폴더 내에서 Git Bash Here를 클릭하여 Git Bash창을 실행시켜줍니다.

     - 저는 C:\Users\Documents\GitHub 폴더 내에 생성하였습니다.

   - Git Bash창을 실행시키면 아래와 같은 명령어 창이 뜨는데 다음 명령어를 실행 시켜 사용자 등록을 합니다.

     - "username" 와  "user@gmail.com"를 알맞게 변경하세요!

     ```shell
     git config --global user.name "username"
     git config --global user.email "user@gmail.com"
     ```

<img src="/assets/images/09_Github-Pages_1.png?raw=true" alt="09_githubPages_1.png" style="zoom:67%;" />



### 원격 저장소 연결

로컬PC에 repository를 연결하려면 repository를 위치시킬 폴더로 이동합니다. 

```
$ cd Lecture
```



그리고 github페이지에서 repository의 clone - html링크를 복사합니다

![image-20210902134728154](/assets/images/09_Github-Pages_3.png)

git clone을 입력하고 뒤에 복사한 html링크를 붙여넣어 실행시킵니다.	

```
$ git clone 복사링크
```



### 원격 저장소 해제

해제하고자하는 원격 저장소 폴더에 cd "원격저장소경로"로 이동하고 다음 명령어를 실행시키면 연결이 해제됩니다. 해제될 때는 아무것도 출력되지 않습니다.

```
$ cd Lecture
$ git remote remove origin
```



   # 👈여기부터 작성👉

   

3. Thema Fork

   - 계정을 생성하였으면 자신이 원하는 블로그의 테마를 가져와야 합니다. 저는 가장 많이 쓰이는 [mmistakes/minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)를 선택했습니다.

4. git 설치

5. Ruby 와 Jekill설치

6. Markdown 지원에디터 설치

- 아래 에디터 중에 선택하여 설치하면 됩니다.
  - Visual Studio Code
  - Atom
  - Typora 
    - 저는 작성한 Markdown이 바로 보여지고 상대적으로 프로그램이 간결한거같아 Typora를 선택했습니다.
  - prose.io

5.  테마 fork하기
6.  취향에 맞게 style변경

<br>



## 앞으로 변경해야 할 것들

- toc(목차) 수정
- code block 의 style수정
- 포스팅본문의 page__inner-wrap의  width 늘리기
- Tags목록 정리
- 글씨체 변경





---

### category List

아래 링크를 참고하여 category List를 만들었습니다.

[https://ansohxxn.github.io/blog/category/](https://ansohxxn.github.io/blog/category/)

