# 더 이상 쉬울 수는 없다. Git 1편. 개념, 기초 명령어

#왕초보 #개발자 #git개념 #git이란 #완벽이해 #더이상쉬울수없다 #git명령어 #핵심기능만 #gitbasic

---

## 0. Git이란?

> Git은 분산버전관리시스템 (Distributed Version Control System, DVCS)이다.
>
> 소스코드의 버전 및 이력을 관리할 수 있다. 

## 1. 준비하기 git config, git remote

1. 윈도우에서는 git을 활용하기 위해서 git bash를 설치한다. (mac에는 기본적으로 깔려있다)

2. git을 활용하기 위해서 GUI툴인 `source tee`, `github desktop` 등을 활용할 수도 있다. (사실상 비추. 장기적으로는 커맨드 라인을 치는 CLI로 하는게 좋다)

3. 초기 설치 완료 후 컴퓨터에 `author정보를 입력`한다. (컴퓨터에 사용자(즉, 나)가 누구인지 설정해주는 것. 게임에서 아이디 만들고 첫 로그인하는 것과 유사하다고 생각하면 이해하기 편하다) 

   ```bash
   $ git config --global user.name {사용자의 github username}
   $ git config --global user.email {github에 가입한 user email}
   ```

   *중괄호는 넣지 않고  user.name 뒤에 한 칸 띄어쓰기 후 아이디를 입력한다.

   

## 2. 로컬 저장소(repository) 활용하기 git init

```bash
$ git init
```

- `.git`폴더가 생성되며 여기에 git과 관련된 모든 정보가 저장된다.
- git bash에 (master)라고 표시되는데, 이는 현재 폴더가 git으로 관리되고 있다는 뜻이며, `master` branch에 있다는 뜻이다. (branch에 대해서는 뒷편에서 설명하겠다.)

### 3. add

> `working directory` 즉, 작업 공간에서 변경된 사항을 이력으로 저장하기 위해서는 반드시 staging area를 거쳐야 한다.

```bash
$ git add .            #모든 파일/폴더
```

- `git add .`은 모든 파일을 staging area(중간계)에 올려놓는다고 보면 된다.

```bash
$ git add markdown.md  #파일
$ git add images/      #폴더
```

- 하지만 때때로 git으로 관리하고 있는 폴더 전체 중에서 특정 파일 혹은 하위 폴더만 변경이력이 있을 경우 이렇게 따로 저장해줄 수 있다.
- 파일일 경우 add 뒤에 `파일명.확장자`로 적고 `enter` 해주면 되고 하위 폴더의 경우 `폴더명/` 으로 해준다 (예시는 images라는 폴더 전체를 업데이트 하겠다는 뜻).  

add 전 상태

```bash
$ 
```

add후 상태

```bash
$ 
```



### 4. git commit

> commit 은 <u>**이력을 확정짓는 명령어**</u>로 해당 시점의 스냅샷을 기록한다.
>
> commit시에는 반드시 메시지를 작성해야 하며, 메시지는 변경사항을 알 수 있도록 명확하게 작성한다.

```bash
$ git commit -m "마크다운 정리"
```



### 5. 원격 저장소(remote repository) 활용하기

원격 저장소 기능을 제공하는 다양한 서비스 중에 github을 기준으로 설명한다.

github, gitlab, bitbucket 등 다양한 서비스가 있다. 추후 git이 익숙해진다면 이 중에서 취향에 맞는 것을 선택해서 쓰면된다. 

각각의 차이점, 장단점은 나중에 다시 업로드 하겠다.

#### 5-1. 준비사항

github에 repository 생성



#### 5-2. 원격 저장소 등록

```bash
$ git remote add origin {github url}
```

- 원격저장소(remote)로 origin이라는 이름으로 github url을 등록(add)한다
- 등록된 원격 저장소를 확인하기 위해서는 아래의 명령어를 활용한다

```bash
$ git remote -v
```

그러면 아래와 같은 결과가 나온다

```bash
origin  https://github.com/{user.name}/{folder.name} (fetch)
origin  https://github.com/{user.name}/{folder.name} (push)
```



 ### 6. git push - 원격 저장소로 업로드

```bash
$ git push origin master
```

`origin`이라는 이름의 원격 저장소로 commit기록들을 업로드 한다.

이후 변경 사항이 생길 때마다, `add`, `commit`, `push`를 반복한다





### 2-3 git reset



## 3. Push/Pull/Clone

### 3-1. git push



### 3-2. git pull - 원격저장소로부터 불러오기

```bash
$ git pull origin master
```

`origin`이라는 원격 저장소로부터 새로운 commit기록들을 불러온다



### 3-3. git clone

```bash
$ git clone {github url}
```

`최초의 github에 업로드 되어있는 프로젝트`를 통째로 복사해서 새로운 로컬에 다운받는다.



## 4. 기타

### 4-1. git log

```bash
$ git log
```

위 `git log` 명령어를 입력하면 현재까지 commit의 모든 기록을 보여준다.

하지만 이게 여러명이 작업하고 시간이 오래걸리는 프로젝트라면 commit이 수십, 수백, 수천개까지 쌓일 수도 있다.... ㅎㄷㄷ

따라서 log들을 간단하게 한 줄로 보고 싶을 때 사용하는 명령어는 `git log --oneline`이다

```bash
$ git log --oneline
```



 

### 4-2. git status

staging area에 현재 뭐가 올라와 있는지 확인하는 명령어 이다.



### git reset

