---
layout: post
title: GitHub에 블로그를 설치하고 Jekyll로 블로그 구현하기
---


#### 1. GitHub에 블로그 파일을 담을 새 repository를 만든다.

\- GitHub의 repository에 블로그를 구현하는 소스파일을 업로드해 블로그를 구현할 수 있다. 

\- Jekyll의 경우 이때 만드는 repository의 이름은 반드시 'GitHub 계정명.github.io'이어야 한다. 이 경우 URL이 'GitHub 계정명.github.io' 인 블로그가 만들어진다.



#### 2. Ruby를 설치한다.

\- Jekyll은 Ruby로 구현된 블로그 설치 툴로, 설치를 위해서는 일단 PC에 Ruby를 설치해야 한다.

\- 최신 윈도우 설치 파일을 <https://rubyinstaller.org/> 에서 구할 수 있다.

  
  

#### 3. cmd 창을 띄우고, gem install jekyll bundler 를 입력한다.

```HTML
gem install jekyll bundler
```

\- Ruby에 내장된 라이브러리 관리 툴인 gem을 통해 Jekyll과 Bundler를 PC에 설치시키는 명령어로, 이 명령어를 수행하면 Jekyll이 PC에 설치된다.



  
#### 4. 새 폴더에 Jekyll 블로그 파일을 설치한다.


```
md <새 폴더명>
jekyll new <새 폴더명>
```

\- Jekyll을 GitHub 블로그에 설치하기 위해서는 PC에 설치된 Jekyll으로부터 GitHub 블로그에 업로드할 Jekyll 파일을 만들어내야 하는데, 이 파일들은 빈 폴더에서만 만들 수 있으므로 새 폴더를 만들어야 한다.


  

#### 6. 로컬 Git repository를 만들고 이를 GitHub 블로그가 저장되는 git repository에 push한다.


```
cd <새 폴더명>
git init
git add .
git commit -m "installed jekyll."
git push "https://github.com/user_id/user_id.github.io.git" master
```

\- PC에 Git을 설치하고 cmd에서 Jekyll이 설치된 폴더에 접근한 후 순서대로 위 명령어를 입력하면 Jekyll 파일들이 GitHub의 repository에 push된다.


#### 7. 블로그 repository 페이지의 Settings 탭에서 블로그가 웹에 공개되게 설정한다.

\- Settings 탭의 Pages 메뉴에 들어가면 Source 항목에 None으로 설정된 셀렉트 박스가 있는데, 이를 클릭해 (블로그가 저장된 브랜치인) jekyll로 변경 후 Save 버튼을 누르면 설정이 끝난다. 





### \* 로컬 서버에서 Jekyll 실행시키기


GitHub의 repository에 직접 접근하여 Jekyll을 수정할 수도 있으나, GitHub의 특성상 수정사항이 웹에 바로바로 반영되지 않는다(수분 이상 반영되지 않는 경우도 있다). 이 경우 개발하는 입장에서
상당히 불편을 겪을 수 있으므로, GitHub의 Jekyll repository를 pull한 다음에 로컬 서버에다 그 Jekyll을 설치하고 수정할 때마다 바로바로 로컬 서버에 접속해 수정사항을 확인하는 게 편하다.

다음 과정을 통해 로컬 서버에 Jekyll을 설치할 수 있다.


#### 1) 블로그 repository를 pull 또는 clone한다.

\- 종전에 로컬 서버에서 작업하던 로컬 repository가 있다면 pull을 하면 되고, 없다면 clone을 하면 된다.

(1) clone 방법

- 빈 폴더에 들어가 git clone "https://github.com/user_id/user_id.github.io.git"을 입력한다.

- 만약 특정 브랜치만 가져오고 싶은 거라면 pull을 해도 된다. 빈 폴더에서 git init 후 그 브랜치를 pull을 하면 된다.

(2) pull 방법

- 종전 작업하던 폴더에 들어가 git pull "https://github.com/user_id/user_id.github.io.git" \<브랜치 이름\>을 입력한다.

\- pull 또는 clone 후 그 브랜치에서 새로운 브랜치를 뻗어서 그 새로운 브랜치에서 작업하고 싶다면 다음 명령어를 입력한다.

```
git branch <새 브랜치명>
git checkout <새 브랜치명>
```

다음 명령어는 위 두 명령어의 기능을 동시에 수행한다.

```
git checkout -b <새 브랜치명>
```


#### 2) bundle add webrick 명령어를 실행한다.

\- Ruby 3.0.0부터는 webrick이 기본 gem에서 빠져 있어 이를 별도로 설치하지 않고 바로 Jekyll 서버 실행 명령어를 실행하면 에러가 난다. 


 #### 3) jekyll serve 명령어를 실행한다.

\- Jekyll을 로컬 서버에 설치하는 명령어로, 이 명령어가 정상적으로 실행되면 다음과 같은 메시지가 뜬다.

```HTML
    Server address: http://0.0.0.0:4000
  Server running... press ctrl-c to stop.
```

\- 이제 브라우저의 주소창에 http://127.0.0.1:4000을 입력하면 로컬 서버에 설치된 Jekyll 블로그에 접속할 수 있다. 로컬 git repository의 파일들을 수정하면 그것의 서버 반영 여부가 즉각 콘솔창에 뜨므로, 이를 확인하면서 실시간으로 Jekyll의 여러 기능을 수정할 수 있다.

\- \--host=0.0.0.0 옵션을 추가하여 같은 LAN에 속한 다른 기기에서도 이 로컬 서버에 설치된 블로그에 접속 가능하게 할 수 있다. 콘솔창에서 ipconfig를 입력하면 이 로컬서버의 LAN상에서의 주소를 알 수 있는데(보통 192.168.0.x 형식으로 돼있다) 다른 기기의 브라우저 URL 부분에 이 주소 뒤에 :4000을 붙여 접속함으로써 블로그에 접속할 수 있다.

\- 수정을 완료했다면 콘솔창에서 ctrl-c를 누르고 Y를 입력하여 로컬 서버를 종료시킬 수 있다. 그 다음에는 다음 명령어를 입력하여 수정사항을 커밋 후 온라인 git repository에 push할 수 있다.

```
git add .
git commit -m "modified details."
git push "https://github.com/user_id/user_id.github.io.git" <브랜치 이름>
```
