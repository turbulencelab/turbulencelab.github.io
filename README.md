# Turbulence Lab

## 가이드
### WSL 설치하기
[공식 매뉴얼](https://learn.microsoft.com/ko-kr/windows/wsl/install)

1. 시작 메뉴에서 Powershell 검색
2. Powershell을 **관리자 권한**으로열기
3. 다음 명령어 입력 후 엔터
```shell
wsl --install
```
4. 기본적으로 Ubuntu (25년 3월 기준 24.04)를 받고 Username과 password 설정
5. 리눅스 쉘이 뜨면 성공

### SSH 키 설정하기
1. WSL로 실행된 리눅스 쉘(터미널)이라고 가정
2. 다음 명령어로 ssh key생성
```shell
ssh-keygen -t ed25519
```
3. 엔터만 누름
4. 다음 명령어를 통해 `.ssh` 디렉토리로 이동
```shell
cd ~/.ssh
```
5. 다음 명렁어를 통해 파일명 변경
```shell
mv id_ed25519 github
mv id_ed25519.pub github.pub
```
6. 다음 명령어를 통해 출력된 값(Public Key)을 복사
```shell
cat github.pub
```
7. GitHub 홈페이지 오른쪽 위 프로필 아이콘 클릭 후 Settings 클릭
8. 왼쪽 메뉴에서 **SSH and GPG keys**로 이동
9. 오른쪽에 **New SSH key** (초록색 버튼)를 클릭
10. Title은 아무거나(WSL-Lab 등의 이름)을 넣고, Key에 아까 복사한 키값을 붙여넣기
11. 아래 **Add SSH Key**(초록색 버튼) 클릭
12. 다시 터미널로 돌아와서 `.ssh`디렉토리의 안의 `config` 파일 열기
```shell
vim config
```
13. 다음과 같은 내용을 추가 (이미 다른 config가 있으면 수정하지 않는다)
```shell
Host github.com
      HostName github.com
      User git
      IdentityFile ~/.ssh/github
```
14. 다음과 같이 권한을 설정한다.
```shell
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config
chmod 600 ~/.ssh/github
chmod 600 ~/.ssh/github.pub
```
15. 다음과 같이 테스트. 마지막 쯤에 Welcome 어쩌구 저쩌구 나오면 성공.
```shell
ssh -vT git@github.com
```

### GitHub Repo클론
1. 보통 `src` (source의 약자)디렉토리를 만든다.
```shell
mkdir -p ~/src
```
2. `src` 디렉토리로 이동
```shell
cd src
```

3. `src` 디렉토리 내에서 다음과 같이 저장소 클론
```shell
git clone git@github.com:turbulencelab/turbulencelab.github.io.git --depth 10
```

### 편집
아래 부분을 제외한 나머지 부분은 교수님 퇴임하실 때까지 절대 건드릴 일이 없을거라고 예상하였다.
변경하고 싶으면 HTML/CSS/JS + Boostrap 5 배워서 넣으면 된다.

#### Publication 수정하기
1. `_bilbliography` 디렉토리에서 적절한 파일을 열어 수정하면 된다. latex의 그 `.bib`파일 맞다.
2. 파일 목록
    1. `clee-conf-aps.bib` : APS Conference
    2. `clee-conf-int.bib` : International Conference
    3. `clee-journal-domestic.bib` : Domestic Journal
    4. `clee-journal-int.bib` : International Journal
    5. `clee-talks.bib` : Talks
    6. `clee.bib` : 아무것도 아님
3. arxiv 및 doi, url 추가하기
    1. doi 항목을 추가, doi 중 주소만 뗀 부분을 추가. 하지만 doi는 url과 중복되어 안보이게 해놨음

        ```latex
        doi={10.1007/s12206-021-0817-4},
        ```

    2. arxiv는 다음 두개를 추가, archivePrefix는 arxiv로 고정, arxivid만 arxiv.org에서 찾아서 추가. 아래는 원래 https://arxiv.org/abs/2106.03032 를 추가한 것이다.

        ```latex
        archivePrefix = {arXiv},
        arxivId = {2106.03032},
        ```

    3. URL은 doi 전체 주소를 추가한다.

        ```bash
        url={https://doi.org/10.1007/s12206-021-0817-4}
        ```

4. 나머지는 적절하게 bib 항목 추가하듯이 넣으면 된다.

#### People 수정하기
 1. 신입생을 추가하는 경우
     1. 이미지 추가하기
         1. `images/people/이름-성.jpg` 경로로 저장
         2. 파일은 200x200 (픽셀)!
         3. 사진은 흰배경의 얼굴이 크게 나온 사진을 넣어야 보기도 좋고 전체적으로 일관성이 있다.
     2. 항목 추가하기
         1. `people/_posts/입학연도-입학월-입학일.md`로 빈 파일 생성. 2021년 1학기 입학이면, [2021-03-01.md](http://2021-03-01.md), 2021년 2학기 입학이면, [2021-09-01.md](http://2021-09-01.md) 처럼 저장
         2. 위에서 만든 파일을 열어 다음과 같이 입력한다. 마지막에 빈줄을 넣어야 git에서 관리가 편하다. https://blog.coderifleman.com/2015/04/04/text-files-end-with-a-newline/

             ```markdown
             ---
             layout: member
             title: FirstName LastName
             position: PhD student
             image: /images/people/FirstName-LastName.jpg
             ---

             ```
             - layout 부분은 변경하면 안된다.
             - title은 성(LastName)과 이름(FirstName)을 나타낸다.
             - position은 PhD student, MS student, 혹은 Postdoc 등을 나타낸다.
             - image는 위에서 저장한 이미지 경로를 넣어주면 된다.
1. Alumni로 변경
   1. image는 지우고 people에 있는 md파일도 다음과 같이 수정한다

   ```markdown
   ---
   layout: member
   title: FirstName LastName
   position: 직장이름
   alumni: true
   ---
   ```

2. 교수님 CV 교체
   1. 파일을 받아서 `chanhoonlee-cv.pdf` 로 이름을 변경한 다음 pdfs/people 안에 있는 같은 이름의 파일을 교체

#### Research 수정하기
1. Research는 이왕이면 수정하기 않기를 권장한다.  좀 어렵다. Boostrap 5를 좀 이해하고 있어야한다. [https://getbootstrap.com/docs/5.3/getting-started/introduction/](https://getbootstrap.com/docs/5.3/getting-started/introduction/) 그리고 이는 HTML/CSS/JS에 대한 기본적인 이해가 있어야한다.
2. 그래도 정 research를 수정하고 싶으면 research/index.html을 열고 각 <div class="d-flex flex-column...> 부터 </div> 까지 복사해서 추가하거나 기존 것을 수정해야한다.
    1. div는 왼쪽에 그림/영상이 있는 경우와 오른쪽에 그림/영상이 있는 두가지 버전이 있다. 적절하게 복사해서 붙여넣거나 수정해야한다.
    2. 내용은 알아서 넣기
        1. research-head 부분  제목
        2. research-note 부분 : 설명
    3. 그림은 img 태그로 지정되어있고, images/research에 저장되어 있다. @1x, @2x, @3x는 HiDPI 디스플레이 예를 들면 맥북의 레티나 디스플레이를 위한 이미지이다.
        1. 포토샵에서 내보내기 → 내보내기 형식 클릭 후 크기 및 접미어를 수정해서 넣으면 된다.
        2. 크기 접미어 목록 ()
            1. 2x, @2x
            2. 3x, @3x
            3. 4x, @4x

    4. 영상은 썸네일 및 mp4와 webm으로 변환하였다.
        1. 썸네일 및 영상은 video 디렉토리에 한꺼번에 넣었다.
        2. 변환방법은 다음과 같다.
            1. WSL에서 ffmpeg설치

              ```bash
              sudo apt install ffmpeg
              ```

            2. webm 파일 변환 (1080p)

              ```bash
              ffmpeg -i 오리지널_영상파일_경로 -c:v libvpx -qmin 0 -qmax 25 -crf 4 -b:v 1M -vf scale=-2:1080 -an -threads 0 영상파일명.webm
              ```

            3. mp4 파일 변환 (1080p): mp4 파일 변환 (1080p), 기존 파일이 mp4여도 다음 스크립트를 써야 웹에서 재생이 잘 된다.

              ```bash
              ffmpeg -i 오리지널_영상파일_경로 -c:v libx264 -pix_fmt yuv420p -profile:v baseline -level 3.0 -crf 22 -preset veryslow -vf scale=-2:1080 -an -movflags +faststart -threads 0 영상파일명.mp4
              ```
            4. 썸네일 스크립트

              ```bash
              ffmpeg -i 영상파일명.mp4 -vframes 1 -vf scale=-2:1080 -q:v 1 원하는_썸네일_파일_이름.jpg
              ```

#### Devcontainer를 이용한 확인
* 커밋하기 전에 제대로 보이는 지 확인해야한다.
* [Devcontainer](https://learn.microsoft.com/ko-kr/training/modules/use-docker-container-dev-env-vs-code/) 기능을 사용해야한다.
* 필요한 기본적인 Dockerfile(Ruby 3.3.4)과 설정은 넣어두었다.

1. [Docker Desktop](https://www.docker.com/products/docker-desktop/) 을 설치
1. VSCode에 Remote: WSL과 Remote: Devcontainer 확장기능을 설치
2. Ctrl+Shift+P를 누르고 Connect to WSL로 WSL로 접속
3. WSL안에서 Open Folder를 누르고 이 repo 디렉토리(`src/turbulencelab.github.io`)로 이동
4. 처음인 경우 Ctrl+Shift+P를 누르고 "Dev Containers: Rebuild Container Without Cache"를 입력한 후 엔터 (시간이 좀 걸림)
5. 아마 소스가 있는 repo디렉토리이겠지만 아닌 경우 Ctrl+Shift+P를 누르고 "Dev Containers: Open Folder in Container"를 입력후 엔터
6. Ctrl+J를 누르고 Terminal을 클릭하면 다음과 같은 쉘이 보일 것
```shell
root@어쩌구저쩌구/workspaces/turbulencelab.github.io#
```
7. 패키지 설치
```shell
bundle install
```
8. 라이브서버를 띄운다.
```shell
bundle exec jekyll serve
```
9. 다음과 같이 뜰텐데 클릭을 해도 되고 "http://127.0.0.1:4000"를 직접 브라우저에서 입력해서 들어가면 현재 수정한 내용에 대한 웹페이지 모습이 보일 것.
```shell
...
Server address: http://127.0.0.1:4000
...
```

10. 종료는 터미널에서 Ctrl + C를 누르면 됨

#### 편집후 Git Commit
인터넷에 "git 기초" 이렇게 찾으면 많이 나올테니 참고하면 된다.
1. git add 명령어를 이용해서 편집한 내용을 stage상태로 만든다.
```shell
git add .
```

2. 다음과 같이 커밋한다. (메시지는 알아서)
```shell
git commit
```

3. 커밋 후 GitHub에 "Push"한다.
```shell
git push -u origin main
```

## Tip

1. Clone 시간 줄이기
    * 전체 clone하면 오래걸릴 수 있다. 다음 방법을 통해 최신 n개(예제에서는 10개)만 clone하면 좀 더 빠르게 clone가능하다.

```shell
git clone git@github.com:turbulencelab/turbulencelab.github.io.git --depth 10
```

2. Devcontainer에서 개발하기
    * Ruby깔기가 귀찮다면

3. 라이브서버에서 테스트해보기
    * Devcontainer 안에 있다고 가정한다.
    * yarn 으로 node관련 패키지 설치 (sass때문)
        ```shell
        yarn install
        ```
    * bundle install을 통해 패키지 설치
        ```shell
        bundle install
        ```
    * live server에서 열기
        ```shell
        bundle exec jekyll serve
        ```

## License

All source code in this repository, consisting of files with extensions `.html`, `.css`, `.less`, `.rb` or `.js`, is freely available under an MIT license, unless otherwise noted within a file. You're welcome to borrow / repurpose code to build your own site, but I would very much appreciate attribution and a link back to [bedford.io](http://bedford.io) from your `about` page.

**The MIT License (MIT)**

Copyright (c) 2013-2019 Trevor Bedford

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
