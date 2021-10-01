---
layout: post
title: Git을 이용해 GitHub에 내 프로젝트 파일 올리기
---

### 1\. Git

\- 어떤 프로젝트를 개발했다고 할 때, 개발과정 및 사후관리 때 프로젝트를 구성하는 각 파일들의 변경점을 일일이 기록하고 이를 관리하는 것이 중요하다. Git은 이처럼 프로젝트의 변경점을 관리하는 프로그램(이를 version control system이라 한다)의 하나로, 오픈소스로 공개되어 현재 널리 사용되고 있다. 

\- Git은 사용자의 PC에 설치해 사용할 수 있으며, 이를 통해 IDE 등을 통해 빌드된 프로젝트 파일들을 담은 **로컬 Git 저장소(repository)**를 생성할 수 있다. 이렇게 생성된 로컬 Git 저장소는 GitHub 같은 **온라인 Git 저장소에 push**할 수 있으며, 이처럼 git repository를 온라인에 push할 경우 여러 사람에게 그 프로젝트 파일을 널리 공유할 수 있다. 

\- 사용 환경에 따라 굳이 꼭 Git을 웹사이트에서 다운받아 설치해야 하는 것은 아닌데, Visual Studio, Eclipse 같은 IDE나 Visual Studio Code 같은 텍스트 편집기에서 직접 로컬 git repository를 생성하는 것은 물론이고 이를 온라인에 push하는 것도 할 수 있기 때문이다. 그러나 경우에 따라서 PC 콘솔창에서 직접 Git 명령어를 실행해야 하는 경우가 있는데, 이 경우에는 Git의 설치가 필요하다. 오픈소스이므로 최신 윈도우 설치파일을 [http://git-scm.com/download/win](http://git-scm.com/download/win) 에서 무료로 다운받아 설치할 수 있다. 

<br><br><br>

### 2\. GitHub

#### 1) 가입

\- GitHub는 대표적인 무료 온라인 Git 저장소이며, 누구든 가입 후 자유롭게 자신의 git repository를 온라인에 push할 수 있다. [https://github.com/](https://github.com/) 에서 간편하게 가입할 수 있다. 

#### 2) 새 repository 만들기

\- GitHub에 가입한다고 곧바로 내 프로젝트 파일을 GitHub에 업로드 할 수 있는 것은 아니고, 먼저 GitHub에 push할 로컬 git repository를 담을 온라인 git repository를 내 GitHub 계정에도 우선 생성해둬야 한다.

\- 내 GitHub 페이지의 repositories 탭에서 New 버튼을 누른 후 repository name을 쓰고 Create repository 버튼을 누르는 것으로 간단히 git repository를 만들 수 있다.

\-  Create repository 버튼을 누르기 전에 Add a README file 또는 Add .gitignore 옆에 체크를 하고 Create repository 버튼을 누르면 새로 생성된 repository 페이지에서 Add file - Upload files 버튼을 눌러 여러 프로젝트 파일을 업로드할 수 있다. (체크 않고 repository를 만들었어도 creating a new file 버튼을 누르면 repository에 새 파일을 작성 후 다른 프로젝트 파일들을 업로드할 수 있다.) 다만 이렇게 파일을 업로드 해봤자 그 프로젝트에 해당하는 로컬 git repository는 만들지 않았으므로 git repository의 원 취지와는 무관한 사용 방법이다. 단순히 과거 작성했던 소스파일을 GitHub의 repository에 보관하고자 하는 목적이라면 이렇게 소스파일을 업로드하는 것이 적절한 사용 방법일 수는 있다.
<br><br><br>

### 3\. 로컬 git repository 만들고 GitHub에 push하기

#### 1) cmd를 실행한 후, 내 프로젝트 파일이 있는 폴더로 이동한다.

#### 2) git init을 입력한다.

\- 이는 그 폴더에 빈 로컬 git repository를 새로 생성하는 명령어이다.

#### 3) git add .를 입력한다.

\- 그 폴더에 있는 모든 파일과 폴더가 git repository에 추가된다.

#### 4) git commit을 입력한다.

\- 프로젝트 파일의 변경사항을 git repository에 등록하는 것을 commit이라 한다. 처음 로컬 git repository를 만들 때는 commit까지 한 뒤에야 비로소 로컬 git repository 생성이 완료된다.

\- commit을 할 때에는 보통 이전 commit에 비할 때 어떤 변화가 있었는지를 기록하는 메시지를 쓰게 돼있으며, 이처럼 콘솔창에서 git commit 명령어를 입력했을 때에는 commit 메시지를 쓰는 창으로 넘어가게 된다. _(이 창에서는 ESC -> :(콜론) -> wq -> 엔터 순으로 입력하면 빠져나올 수 있다.)_ 굳이 새 창을 띄우면서까지 길게 쓸 커밋 메시지가 없다면 다음과 같이 옵션으로 -m "쓸 내용"을 추가하면 곧바로 커밋으로 넘어간다.

```HTML
git commit -m "첫 번째 커밋"
```

\- 만약 Git에 사용자의 이름과 이메일 주소 정보를 저장한 적 없다면 에러 메시지가 뜨면서 커밋이 진행되지 않는다. _(그 repository를 만든 사람의 정보를 그 repository에 추가할 수 없기 때문이다. 프로젝트 파일을 다른 사람들과 공유하는 것은 Git의 중요한 사용 목적이므로 당연히 그 정보가 프로젝트 파일에 추가될 수 있게 해야 한다.)_ 이 경우 다음 명령어로 이름과 이메일 주소 정보를 Git에 저장한 후 다시 커밋을 진행해야 한다. 

```HTML
git config user.name "lkwks"
git config user.email "lkwks@naver.com"
```

#### 5) git branch -M (새 브랜치 이름)을 입력한다.

\- 브랜치는 Git의 중요한 개념 중 하나로, 하나의 git repository 안에 있을 수 있는 수개의 분기를 뜻한다. 각 브랜치를 구성하는 프로젝트 파일은 서로 독립적일 수 있으며, 수개의 브랜치를 하나로 병합하는 것도 가능하다. 이는 프로젝트의 개발 과정에서 각 브랜치별로 개발하고자 하는 부분을 나눠서 따로 브랜치를 관리하며 개발을 진행하다 나중에 하나의 전체 프로젝트로 병합하는 식으로 프로젝트 개발을 관리할 때 유리하다.

\- 맨 처음 commit을 진행하면 프로젝트 파일들이 자동으로 master란 이름의 브랜치에 저장된다. GitHub에 push할 git repository에 master란 이름의 브랜치가 없다면 이를 수정하지 않아도 문제 없이 push가 되지만, 같은 이름의 브랜치가 이미 있다면 이를 수정하지 않을 경우 에러가 발생할 수 있다. _(같은 프로젝트 파일이고 이를 로컬 repository로 pull한 후에 온라인 repository로의 추가적 push가 없은 경우에 이를 수정한 로컬 repository를 push하는 경우에는 브랜치 이름이 동일해도 에러가 발생하지 않는다. 이 경우에는 브랜치 이름을 바꾸지 않는 것이 브랜치 시스템의 목적에 부합하는 사용이다.)_ 

```HTML
git branch -M new_branch_name
```

#### 6) git push -u "git repository의 URL" (브랜치 이름)을 입력한다.

\- 이 명령어를 통해 내 GitHub의 git repository에 프로젝트 파일들을 push할 수 있다. 여기서 "" 안에는 그 프로젝트의 로컬 git repository가 push될 내 온라인 git repository의 주소를 넣는다. git repository URL은 "\https://github.com/깃허브 아이디/git repository 이름.git"의 형식을 갖고 있다.

![.](https://user-images.githubusercontent.com/69514453/135396809-614d71fe-ba3d-4651-8eac-38e4a8ba767d.png)

_GitHub에서는 새로 git repository를 만들면 첫화면에 그 repository의 URL을 띄워준다._


```HTML
git push -u "https://github.com/lkwks/test.git" new_branch_name
```

\- ""안에 URL을 넣을 때, https:// 를 빼먹으면 push에 실패하는 경우가 있다. 브랜치 이름 부분을 틀리는 경우에도 push가 실패한다.


<br><Br>

#### * push때 적는 URL을 단축하기

\- 프로젝트를 진행하면서 진행 과정을 매번 push하는 경우에는 URL을 매번 일일이 적는 게 귀찮을 수 있다. 이 경우 다음 명령어를 통해 push할 온라인 git repository의 주소의 축약어를 지정할 수 있다.

```HTML
git remote add 축약어 "URL"
```
\- 이렇게 할 경우 지정된 온라인 git repository URL의 축약어가 이 축약어로 지정되며, 다음 push부터는 이 지정된 축약어를 URL 대신 쓸 수 있다.
