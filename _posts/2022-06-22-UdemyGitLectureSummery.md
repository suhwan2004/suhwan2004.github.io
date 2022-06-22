---
published: true
title: "Git - Git으로 만드는 전설의 레시피"
categories:
  - Git
tags:
  - Git
---

udemy의 Git으로 만드는 전설의 레시피 강의를 필기한 내용입니다!

## Section 1. git이란 대체 뭘까?

Git은 버전을 편리하게 관리하게 만들어주는 도구입니다.

예를들어 학교 과제를 할 때...

**최종, 진짜 최종, 진짜진짜 최종과 같은 파일명으로 버전관리를 하는 경우가 있다고 해볼까요?**

이러면 각 버전 별로, 어떤 수정 사항이 있는지 알 수 없기 때문에 하나하나 다 열어봐야 하는 문제가 발생합니다.

하지만, git을 쓴다면? 이런 단점을 부술 뿐 아니라 장점도 있습니다!

1.  최정 과정까지의 히스토리를 관리할 수 있다.
2.  이전 버전으로 타임머신을 타고 돌아갈 수 있다.
3.  동료와 협업할 수 있는 환경을 만들어 준다.

이런 git과 친구인 github도 많이 들어본 것일텐데요.

github는 git으로 저장된 내역들을 원격으로 관리하고 저장하는 공간을 제공하는 서비스입니다.

**git은 동영상이면 github는 유튜브**라고 볼 수 있습니다!

## Section2. Git으로 간단한 레시피 만들어보기

### 1. commit - Git 초기화와 로컬저장소, 최초의 커밋 만들기

git init

- git을 사용하겠다라는 명령을 내려주는 것.
- 로컬 저장소가 만들어짐.(숨겨진 폴더인 .git이 로컬 저장소임)

git add 파일이름

- 버전으로 관리할 파일을 추가하는 것.
- 아직 버전에 포함되어 있지 않음. 말 그대로 추가만 한 상태임.

git commit -m “쓰고싶은 메시지”

- 의미있는 변경사항을 묶어서 만드는 과정
- -m은 해당 버전 상태에 대한 기록을 남기겠다는 뜻임.
- git log —online을 통해 편하게 내역을 볼 수 있음.
- add 명령어는 사진기 앞에 물건을 두는 것이라면 commit은 사진을 찍는 것.

### 2. push - Github 원격 저장소에 내가 만든 레시피 업로드하기

git remote add origin 주소

- remote는 원격 저장소, add는 더해주기, origin은 원격저장소를 나타낼 주소입니다.

git remote -v

- 내가 등록한 원격 저장소를 알 수 있습니다.

git branch -M main

- 메인이 될 브랜치를 main으로 설정합니다.

git push -u origin main

- origin(원격저장소)의 main 브랜치에 push 한다는 뜻임.
- -u는 로컬저장소를 원격 저장소에 연결하기 위해 사용하는 것이며 한번만 사용하면 됩니다.

### 3. pull - 원격 저장소에 있는 변경 사항을 내 로컬 저장소와 동기화 시키기

github repository 웹 페이지에서 README.md파일을 수정했다면 그건 원격 저장소에서만 수정이 일어난 것입니다.
=> 한 마디로 로컬에는 적용되지 않은 상태라고 볼 수 있습니다.

그렇다면, 원격 저장소에서 수정된 내용을 로컬 저장소에 적용시켜야 되겠죠? 딱 맞는 명령어가 여기 있습니다.

git pull

- 원격 저장소에서 적용된 수정 내용을 로컬 저장소에 적용시켜줍니다.

## Section 3. Git으로 여러 실험을 해볼 수 있는 레시피 만들기

### 1. branch - 새로운 레시피를 어떻게 안전하게 개발할 수 있을까?

git branch 원하는이름

- 원하는이름을 가진 새로운 브랜치를 만듭니다.

git switch 원하는이름

- 원하는 이름을 가진 브랜치로 이동합니다.

자고로, 브랜치를 생성하고나서 추후에 원격 저장소로 push를 진행하면 오류가 뜹니다.

⇒ 왜냐? 당연히, 원격 저장소에는 저희가 새로 만든 브랜치가 없거든요.

이를 해결하기 위해서는 이걸 씁니다.

git push —set-upstream origin 원하는이름

- 원격 저장소에 원하는이름 브랜치를 넣어주면서 push 명령어를 진행합니다.

### 2. merge - 다른 브랜치에서의 작업한 내용을 합치고 싶다면 merge하세요!

merge의 경우, commit은 현재 브랜치에서 하되 commit을 하고나서는 기반이 되는 브랜치로 이동해야 됨!

예를들어 현재 새로 만든 브랜치인 비빔밥에 있다면?

```jsx
// 새 커밋 작성
git commit -m “이건 새로운 메시지야!”

// 주체가 되는 브랜치로 이동
git switch main

// branch 비빔밥과의 merge 실행
git merge 비빔밥
```

### 3. pull request - merge를 신중하게 하기 위해서 해야할 것

사실, 현업에서는 코드를 짰다고 바로 merge를 하는 경우는 없습니다. main 브랜치는 사용자가 이용하는 최종 브랜치이기 때문에 철저한 검증이 필요합니다.

내가 로컬에서 작업한 제 브랜치의 내용을 원격 저장소에 merge하기 위해 검증을 요청하는 것을 pull request라고 합니다.

절차를 시각 자료로 봅시다!

1.  예시로, 새로운 비빔밥이라는 브랜치를 넣어주고 나니 pull request를 생성할 수 있는 버튼이 뜹니다.
    ![image](https://user-images.githubusercontent.com/60723373/174852064-8557b8d9-e8d9-4eab-8aca-dfd42ed15971.png)

2.  이렇게 comment를 작성하고 Create pull request를 누르면 새로운 pull request가 생성됩니다.
    ![image](https://user-images.githubusercontent.com/60723373/174852183-940f8e09-a038-4b7d-8e77-2a9a18a17640.png)

3.  pull request가 되고 나서 그 내부에 file changed에서 특정 코드의 + 버튼 클릭 시 해당 코드에 대해 조언이나, 이 코드는 나쁘다던지 하는 코멘트를 달 수 있습니다.
    ![image](https://user-images.githubusercontent.com/60723373/174852300-c8a664e8-75f4-4bc5-b9d6-0f4a03521979.png)

4.  코멘트를 다 달았다면 저장해야겠죠? Finish your review를 클릭합니다.
    ![image](https://user-images.githubusercontent.com/60723373/174852372-e06d5ad6-be28-44d2-a236-4c6be105f625.png)

여기에서는 몇 가지 옵션들이 존재하는데요.

- Comment : 나는 그냥 코멘트만 남기기만 할래
- Approve : 이거 merge 해도 될 것 같은데?
- Request changes : 이 코드는 좀 바꿔야 될 필요가 있는것 같아

이런 의미들을 담고 있습니다.

5.  Finish your review가 끝났을 때 지금과 같은 경우는 새로운 Comment가 달렸습니다. 해당 Comment로 토론의 장도 열 수 있습니다.
    ![image](https://user-images.githubusercontent.com/60723373/174852580-e0af6ad1-df8f-4df3-8795-3ce6f6f88ba5.png)

6.  이러한 고민의 과정이 끝나면 하단의 Merge pull request를 통해 merge를 할 수 있습니다.
    ![image](https://user-images.githubusercontent.com/60723373/174852685-3d368d03-a077-4863-aaef-773eccf2b5e9.png)

## Section 4. Git으로 레시피 수정하기

### 1. amend - 깜빡 하고 수정하지 못한 파일이 있다면!

커밋을 하나 만들고 나서, 더 추가한 코드가 생각났을 때 새로운 commit을 만든다?

⇒ 그렇다며 다른 사람들이 혼동이 올 확률이 큼.

그걸 해결하기 위해 amend가 있습니다!

git commit —amend

- 전에 있던 commit을 수정하며 지금 수정한 내용을 전 commit에 포함시킵니다.

### 2. reset - 시간 여행을 하고 싶다면?

코드를 다 짜고 commit을 했는데 생각해보니 특정 commit 시점으로 돌아가는게 더 나은 상황이 생길 수도 있습니다. 그렇다면?

1.  **git log —oneline**으로 지금까지 commit 내역을 보며 가고 싶은 commit 지점의 id를 복사합니다.
2.  **git reset 복사한id** 를 하여 해당 지점으로 돌아갈 수 있습니다!

다만, 순수 reset을 한다고 바로 바뀌지 않습니다.

reset에는 많은 옵션이 있는데 그 중에 —mixed가 default option입니다. —mixed의 경우 변경한 내용을 그대로 남긴 채로 커밋의 이력만 reset 해주기 때문입니다.

만약 코드 자체를 과거의 commit에 맞추고 싶다면?

⇒ **git reset 복사한id —hard** 를 실행시키면 됩니다.

### 3. revert - 과거를 지우고 싶다면!

reset의 경우 되돌리면 장땡! 과 같은 느낌이라 되돌린 기록이 안 남네요.

만약, 되돌린 기록또한 남기고 싶다면? revert를 쓰면 됩니다!

**git revert 복사한id**

- 복사한id인 과거의 커밋을 현재 코드에 적용시킵니다.
- 적용시킨 상황에서 과거의 커밋으로 대체할지 그냥 이대로 쭉 갈지 정할 수 있습니다.

### 4. stash - 변경 사항을 잠시 지워두고 싶고, 커밋은 안 만들고 싶다면?

예를들어 비빔밥 레시피를 만들다가, 다른 레시피를 적고 싶을때 현재 작업한 건 커밋하기 싫다면??

⇒ stash를 써서 임시 저장소에 넣어둘 수 있습니다!

git stash push -m “~~~”

- 현재 작업한 것을 stash 영역에 넣습니다. 작업중인 코드는 전 push 때의 코드로 돌아갑니다.

git stash list

- stash 영역에는 여러 개를 저장할 수 있고 이는 리스트 형식으로 담깁니다.
- 그렇기 때문에, 이 리스트들을 확인할 수 있는 명령어 또한 존재합니다.
- stash 내에 저장된 내역들을 볼 수 있는 명령어입니다.

git stash show stash*id*값 -p

- stash*id*값에 들어있는 내용을 자세히 볼 수 있습니다.

git stash apply (원하는id)

- id를 입력하지 않는다면 가장 최근에 stash에 넣은 것으로 코드에 적용.
- id를 입력하면 원하는 id에 있는 것으로 적용이 됩니다.

### 5. cherry-pick - 다른 브랜치의 이력을 뚝 가져오고 싶다면?

**git cherry-pick 다른브랜치\_원하는commit_id**

- 다른 브랜치에서 작업한 commit을 나의 브랜치에 가져오고 반영 시킵니다.

만약, merge conflict가 났을 경우에는 다 수정하고, **git cherry-pick —continue**를 한 뒤에 커밋해주도록 합시다.

## Section 5, 6… 추가 내용들

### 1. Fork란?

오픈 소스의 경우 쓰기 권한이 없으나, fork를 하여 해당 오픈소스 리포지토리를 복사된 해당 리포지토리는 내 저장소이므로 쓰기 권한이 존재합니다.

그렇기 때문에, 내 저장소가 된 곳에서 작업을 하고 원본 오픈 소스 리포지토리에 pull request를 보내는 것이 오픈 소스에 기여하는 방법입니다.

### 2. Git은 누가 만들었나요?

우리가 아는 그분… 리누스 토발즈가 만들었습니다. 그 전, bitkeeper를 쓰고 있었으나 유료화가 되면서 성능을 향상시킨 git을 만들어서 내놓았다고 하네요.

### 3. git을 사용하는 방법은?

터미널을 이용하는 CLI 방식과, 버튼을 사용하는 GUI 방식이 있습니다.

예를들어 SourceTree라는 것을 사용하면 CLI보다는 더 편리한 GUI 방식으로 git을 쓸 수 있습니다.

그럼, 우리는 왜 처음부터 CLI로 했을까요? 숙련자는 CLI를 쓰고 초보자는 GUI를 쓴다는 것은 편견입니다. 둘 다 쓸 줄 알아야 되기 때문에 그렇다고 하네요.

### 4. 현업에서는 commit message를 어떻게 적나요?

각 회사마다 일정의 약속이 있다고 합니다. 그 것외에 개발자들이 공통을 사용하는 컨벤션이 있다고 하네요.

해당 정보글이 있는 사이트 : [](https://udacity.github.io/git-styleguide/)[https://udacity.github.io/git-styleguide/](https://udacity.github.io/git-styleguide/)

또한, 오픈소스에 기여할 때 좀 큰 오픈소스라면 특유의 커밋 컨벤션이 있을 확률이 높습니다. 꼭 찾아보고 커밋을 해주시면 좋습니다.

그리고 gitmoji라고 commit message 앞 type들을 이모티콘을 바꿔서 색다른 commit을 할 수도 있습니다.
