---
layout: "post"
title: "git-command-collections"
description: "git command 모음집"
date: "2017-04-12 18:01"
tags: [git, git-command, command, tip]
comments: true
---

# Git-tip

## git 유용한 명령어 모음

### 환경 설정
출처 - [아웃사이더님 블로그](https://blog.outsider.ne.kr/572)
```bash
$ git config --global --list
현재 설정정보 조회할 수 있습니다. --global옵션은 전역설정에 대한 옵션이며 현재 프로젝트에만 적용할때는 주지 않습니다.
$ git config --global user.name "사용자명"
사용자명을 등록합니다 (필수)
$ git config --global user.email "이메일주소"
이메일 주소를 등록합니다. (필수)
$ git config --global color.ui “auto”
터미널에 표시되는 메시지에 칼라를 표시해줌
```
#### 기본 명령어

`git --version`
* 현재 git의 버전을 확인합니다.

`git init`
* 현재 디렉토리에 git 저장소를 생성합니다.

`git add 파일명`
* 해당 파일을 추척함.

`git commit -m "커밋메시지"`

* 스테이징 영역에 올라가 있는 파일들을 커밋합니다.
* `-m` 은 커밋메시지를 주는 옵션으로 여러 줄의 커밋메시지를 쓸 경우 `-m` 을 여러개 사용할 수 있습니다.

`git commit -C HEAD -a --amend`
* 지정한 커밋의 로그메시지를 다시 사용하여 기존커밋을 수정합니다.
* `-c`를 사용하면 기존메시지를 수정할 수 있는 편집기를 실행해 줍니다.

`git status`
* 커밋되지 않은 변경사항을 조회합니다.

`git diff`
* 스테이징영역과 현재 작업트리의 차이점을 보여줍니다.
* `--cached` 옵션을 추가하면 스테이징영역과 저장소의 차이점을 볼 수 있다.
* `git diff HEAD`를 입력하면 저장소, 스테이징영역, 작업트리의 차이점을 모두 볼 수 있다.
* 파라미터로 log와 동일하게 범위를 지정할 수 있으며 `--stat`를 추가하면 변경사항에 대한 통계를 볼 수 있습니다.

`git mv 파일명 새파일명`
* 기존에 존재하는 파일을 새파일로 이동합니다.
* 변경이력은 그대로 유지합니다.

`git checkout -- 파일명`
* 아직 스테이징이나 커밋을 하지 않은 파일의 변경내용을 취소하고 이전 커밋상태로 돌립니다.
* svn에서 revert와 동일합니다.

#### Branch와 Tag

`git branch`
* 현재 존재하는 브랜치를 조회합니다.
* `-r` 옵션을 사용하면 원격저장소의 브랜치를 확인할 수 있습니다.

`git branch 브랜치명B 브랜치명A`
* 브랜치명A에서 새로운 브랜치 브랜치명B를 만듭니다. (git에서 기본 브랜치는 master라는 이름을 사용합니다.)

`git branch 브랜치명`
* 브랜치명의 새로운 브랜치를 만듭니다.(체크아웃은 하지 않습니다.)

`git branch -d 브랜치명`
* 브랜치를 삭제합니다.

`git push <repository> :<branch_name>`
* 원격 브랜치를 삭제합니다.
* ex) `git push origin :102-feature-seungdols`

`git branch -m 존재하는브랜치명 새로운브랜치명`
* 존재하는 브랜치를 새로운브랜치로 변경합니다.
* 이미 존재하는 브랜치명이 있을 경우에는 에러가 나는데 `-M` 옵션을 사용하면 이미 있는 브랜치의 경우에도 덮어씁니다.

`git tag 태그명 브랜치명`
* 브랜치명의 현재시점에 태그명으로 된 태그를 붙힙니다.
* git tag만 입력하면 현재 존재하는 태그 목록을 볼 수 있습니다.

`git checkout 브랜치명/태그명`
* 해당 브랜치나 태그로 작업트리를 변경합니다.

`git checkout -b 브랜치명B 브랜치명A`
* 브랜치명A에서 브랜치명B라는 새로운 브랜치를 만들면서 체크아웃을 합니다.

`git rebase 브랜치명`
* 브랜치명의 변경사항을 현재 브랜치에 적용합니다.

`git merge 브랜치명`
* 브랜치명의 브랜치를 현재 브랜치로 합칩니다.
* `--squash` 옵션을 주면 브랜치명의 모든 커밋을 하나의 커밋으로 만듭니다.

`git cherry-pick 커밋명`
* 커밋명의 특정 커밋만을 선택해서 현재 브랜치에 커밋으로 만듭니다.
* `-n` 옵션을 주면 작업트리에 합치지만 커밋은 하지 않기 때문에 여러개의 커밋을 합쳐서 커밋할 수 있습니다.

#### 로그 관리

`git log`

* 커밋로그들을 볼 수 있으면 `-1`나 `-2`같은 옵션을 주어 출력할 커밋로그의 갯수를 지정할 수 있습니다.
* `--pretty=oneline` 옵션을 주면 한줄로 간단히 보여주고 `--pretty=format:"%h %s"`처럼 형식을 정해줄 수 있습니다.
* `-p` 옵션을 사용하면 변경된 내용을 같이 보여줍니다.
* `--since="5 hours"` 이나 `--before="5 hours"`같은 옵션도 사용가능합니다.
* `--graph` 옵션을 주면 브랜치 트리를 볼 수 있습니다.

`git log 커밋명`
* 해당 커밋명의 로그를 볼 수 있습니다.
* 커밋명A..커밋명B (마침표2개)와 같이 입력하면 커밋명 A이후부터 커밋명 B까지의 로그를 볼 수 있습니다.
* `^`은 `-1`과 동일해서 `HEAD^`라고 하면 최신바로 이전 커밋이고 `HEAD^^^`와 같이 쓸 수 있으며 `HEAD~3`을 하면 HEAD의 3개 이전의 커밋을 뜻합니다.

`git blame 파일명`
* 갈 줄 앞에 커밋명과 커밋한 사람등의 정보를 볼 수 있습니다.

`git blame -L 10,15 파일명`
* `-L` 옵션을 사용하면 10줄부터 15줄로 범위를 지정해서 볼수 있고 `15`대신 `+5`와 같이 사용할 수 있습니다.
* 숫자의 범위 대신 정규식도 사용이 가능합니다.

`git blame -M 파일명`
* `-M` 옵션을 사용하면 반복되는 패턴을 찾아서 복사하거나 이동된 내용을 찾아줍니다.
* `-C` 옵션을 사용하면 파일간의 복사한 경우를 찾아줍니다.
* `-C`는 git log에서도 사용가능하며 내용의 복사를 찾을때는 `git log`에서 `-p`옵션을 사용합니다.

`git revert 커밋명`
* 기존의 커밋에서 변경한 내용을 취소해서 새로운 커밋을 만듭니다.
* `-n`옵션을 사용하면 바로 커밋하지 않기 때문에 revert를 여러번한 다음에 커밋할 수 있습니다.(항상 최신의 커밋부터 revert해야 합니다.)

`git reset 커밋명`
* 이전 커밋을 수정하기 위해서 사용합니다.
* `--soft` 옵션을 사용하면 이전 커밋을 스테이징하고 커밋은 하지 않으며 `--hard`옵션은 저장소와 작업트리에서 커밋을 제거합니다.
* `git reset HEAD^`와 같이 입력하면 최근 1개의 커밋을 취소할 수 있습니다.

`git rebase -i 커밋범위`
* `-i` 옵션으로 대화형모드로 커밋 순서를 변경하거나 합치는 등의 작업을 할 수 있습니다.

#### 원격저장소

`git clone 저장소주소 폴더명`
* 원격저장소를 복제하여 저장소를 생성합니다.
* 폴더명을 생략가능합니다.

`git fetch`
* 원격저장소의 변경사항 가져와서 원격브랜치를 갱신합니다.

`git pull`
* `git fetch`에서 하는 원격저장소의 변경사항을 가져와서 지역브랜치에 합치는 작업을 한꺼번에 합니다.
* 파라미터로 풀링할 원격저장소와 반영할 지역브랜치를 줄 수 있습니다.

`git push`
* 파라미터를 주지 않으면 origin 저장소에 푸싱하며 현재 지역브랜치와 같은 이름의 브랜치에 푸싱합니다.
* `--dry-run` 옵션을 사용하면 푸시된 변경사항을 확인할 수 있습니다.
* 로컬에서 `tag`를 달았을 경우에 기본적으로 푸시 하지 않기 때문에 `git push origin 태그명`이나 모든 태그를 올리기 위해서 `git push origin --tags`를 사용해야 합니다.

`git remote add 이름 저장소주소`
* 새로운 원격 저장소를 추가합니다.

`git remote`
* 추가한 원격저장소의 목록을 확인할 수 있습니다.

`git remote show 이름`
* 해당 원격저장소의 정보를 볼 수 있습니다.

`git remote rm 이름`
* 원격저장소를 제거합니다.

#### 서브 모듈

`git submodule`
* 연관된 하위모듈을 확인할 수 있습니다.

`git submodule add 저장소주소 서브모듈경로`
* 새로운 하위모듈을 해당경로에 추가합니다.
* 추가만하고 초기화 하지는 않으며 commit hash 앞에 마이너스(-) 표시가 나타납니다.

`git submodule init 서브모듈경로`
* 서브모듈을 초기화 합니다.

`git submodule update 서브모듈경로`
* 서브모듈의 변경사항을 적용합니다.(저장소의 최신커밋을 추적하지 않습니다.)

#### 기타 명령어

`git archive --format=tar --prefix=폴더명/ 브랜치혹은태그 | gzip > 파일명.tar.gz`

`git archive --format=zip --prefix=폴더명/ 브랜치혹은태그 > 파일명.zip`

* 해당 브랜치나 태그를 압축파일로 만듭니다.
* `--prefix`를 주면 압축하일이 해당폴더 안에 생성되도록 할 수 있습니다.

`git mergetool`
* 설정에 `merge.tool`의 값에 있는 머지툴을 찾아서 실행합니다.

`git gc`
* 저장소의 로그를 최적화 합니다.
* 로그가 변경되지는 않고 저장하는 방식만 최적화 합니다.
* `--aggressive` 옵션을 주면 더 자세하게 최적화합니다.

`git rev-parse --show-toplevel`
* git 저장소내에서 입력하면 루트디렉토리를 알려줍니다.

----

출처 - [꿀벌 개발 일지](http://ohgyun.com/414)

`$ git branch -va`
* 리모트를 포함한 브랜치 정보 보기

`$ git remote update`
* 리모트 정보가 업데이트 필요할 때

`$ git log`
* 로그 보기

`$ git log --graph`
* 그래프 형태로 보기

`$ git log --pretty=oneline`
* 한 줄로 보기

`$ git log -p -1`
* 마지막 1개 커밋과 변경된 diff 를 함께 보기

`$ git shortlog`
* 짧은 로그 보기

`$ git shortlog --no-merges`
* 머지 로그를 제외하고 보기

`$ git show 커밋번호`
* 특정 커밋의 로그 보기

`$ git show topic1`
* 브랜치는 커밋의 포인터이기 때문에 브랜치 이름을 사용해도 된다.

`$ git show HEAD^`
* 커밋 뒤에 ^를 붙이면 해당 부모를 찾는다.
* 위 코드는 바로 전 커밋을 보여준다.

`$ git log master..topic1`
* 아래 커밋은 master 에는 없지만, topic1 에는 있는 커밋을 보여준다.

`$ git log origin/master..topic1`
* 리모트 저장소를 대상으로도 비교할 수 있다.

`$ git commit --amend`
* 이전 커밋에 덮어쓰기
* `--amend` 옵션으로 커밋하면, 이전 커밋과 다른 커밋 번호가 생성된다.

##### 버그 찾기
git blame 명령을 사용하면, 해당 코드가 언제 수정되었는지 확인할 수 있다.

`$ git blame -L 11,12 README.md`

* 위 코드는 README.md 파일의 11,12 번째 라인이 언제 수정되었는지 추적한다.
* 결과 목록에서 ^가 붙은 것은 해당 줄이 처음 커밋되었다는 얘기다.


----

## git commit - 취소 관련 명령어 정리

출처 - [에코지오 블로그](http://ecogeo.tistory.com/276)

### 개별파일 원복

`git checkout <파일>`
* 워킹트리의 수정된 파일을 index에 있는 것으로 원복

`git checkout HEAD <파일명>`
* 워킹트리의 수정된 파일을 HEAD에 있는 것으로 원복(이 경우 --는 생략가능)

`git checkout FETCH_HEAD <파일명>`
* 워킹트리의 수정된 파일의 내용을 FETCH_HEAD에 있는 것으로 원복? merge?(이 경우 --는 생략가능)


### index 추가 취소

`git reset -- <파일명>`
* 해당 파일을 index에 추가한 것을 취소(unstage). 워킹트리의 변경내용은 보존됨. (--mixed 가 default)

`git reset HEAD <파일명>`
* 위와 동일


### commit 취소

`git commit --amend`
* commit 후 수정 안하고, 바로 명령어를 입력하면 커밋 메시지만 변경 된다.

`git reset HEAD^`
* 최종 커밋을 취소. 워킹트리는 보존됨. (커밋은 했으나 push하지 않은 경우 유용)

`git reset HEAD~2`
* 마지막 2개의 커밋을 취소. 워킹트리는 보존됨.

`git reset --hard HEAD~2`
* 마지막 2개의 커밋을 취소. index 및 워킹트리 모두 원복됨.

`git reset --hard ORIG_HEAD`
* 머지한 것을 이미 커밋했을 때,  그 커밋을 취소. (잘못된 머지를 이미 커밋한 경우 유용)

`git revert HEAD`
* HEAD에서 변경한 내역을 취소하는 새로운 커밋 발행(undo commit). (커밋을 이미 push 해버린 경우 유용)

### 워킹트리 전체 원복

`git reset --hard HEAD `
* 워킹트리 전체를 마지막 커밋 상태로 되돌림. 마지막 커밋이후의 워킹트리와 index의 수정사항 모두 사라짐. (변경을 커밋하지 않았다면 유용)

`git checkout HEAD . `
* 워킹트리의 모든 수정된 파일의 내용을 HEAD로 원복.

`git checkout -f`
* 변경된 파일들을 HEAD로 모두 원복(아직 커밋하지 않은 워킹트리와 index 의 수정사항 모두 사라짐. 신규추가 파일 제외)


참조 : reset 옵션

* `--soft` : index 보존, 워킹트리 보존. 즉 모두 보존.
* `--mixed` : index 취소, 워킹트리만 보존 (기본 옵션)
* `--hard` : index 취소, 워킹트리 취소. 즉 모두 취소.


##### untracked 파일 제거

* `git clean -f`
* `git clean -f -d` : 디렉토리까지 제거

----


## 유용한 Alias 지정하기

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.last 'log -1 HEAD'
```

기타 다른 별칭도 지정이 가능하니, 자주 사용하는 명령어는 alias 지정을 해두는 것이 좋다.

## Git remote branch 가져오기

* `git branch`
* `git branch -r` : 원격 브랜치 목록
* `git branch -a` : 모든 브랜치 목록
* `git checkout remote이름/branch이름`
* `git checkout -b 생성할브랜치이름 원격브랜치이름`
* `git checkout -t remote이름/branch이름`


## Reset 한 것을 취소하기

```bash
git reflog

#몇 번째 HEAD로 이동할지 확인한다.

git reset --hard HEAD@{num}

```

## 하던 일을 stashing 하기

작업을 막 하다가 갑자기 다른 일을 먼저 처리 해야 할 때가 있다.
그럴때, 현재 작업 트리를 temp로 저장시킬 수 있다.

```bash
$ git stash
```

위 명령을 하게 되면, 하던 일을 저장한다.

```bash
$ git stash list
```

위 명령어를 입력하면, stash의 목록을 확인 할 수 있다.

```bash
$ git stash apply
```

* 해당 명령어 입력시 가장 최근의 stash를 적용한다.
* 다른 것을 적용 하고자 한다면, stash 이름을 입력하면 된다.
* `--index` 옵션을 주어야 staged 상태까지 복원이 된다.

### Stash list 깔끔하게 정리하기

* stash list는 자동으로 삭제가 되지 않는다.
* 단, 한 경우를 제외하면 말이다.
* 바로 stash로 branch를 만들고, 그것이 성공 했을 때를 제외하면, list를 수동으로 삭제 해주어야 한다.

```bash
$ git stash drop stash@{0}
```

### **Stash 되돌리기**

* stash를 적용하고 나니, 아차...이건 아니다 싶을 때 다시 복원하는 방법이다.
* 중요한 점은 아무래도 `stash unapply`를 지원하지 않아 우회적으로 사용하는 방법이다.

```bash
$ git stash show -p stash@{0} | git apply -R
```

위 명령을 alias 지정하여 사용하면 편리하다.

```bash
git config --global alias.stash-unapply '!git stash show -p | git apply -R'
```

### Stash를 적용한 브랜치 만들기

```bash
git stash branch testchanges
```
