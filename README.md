### Jekyll 설치

터미널에서 `sudo gem install jekyll bundler` 명령어를 입력하여 `jekyll`과 `bundler`를 설치합니다.

잘 되지 않는다면 공식 사이트([Windows](https://jekyllrb.com/docs/installation/windows/), [macOS](https://jekyllrb.com/docs/installation/macos/))를 참고해주세요!

1. [루비 공식 다운로드](https://rubyinstaller.org/downloads/)

### gem v_3.0.0, rbenv v_2.7.7 이상

Mac M1칩일 경우

```shell
$ rbenv install 3.1.2
$ rbenv global 3.1.2
$ rbenv local 3.1.2
$ sudo gem install jekyll
```

### 로컬에 블로그 실행

터미널에서 프로젝트 경로로 이동한 뒤 하기 명령어들을 실행하여 로컬에 블로그를 띄워줍니다.

```shell
# 프로젝트 경로로 이동
$ cd licheco.github.io
# 패키지 설치
$ bundle install
# 블로그 서버 실행
$ jekyll serve 안되면 $bundle exec jekyll serve
```

[http://127.0.0.1:4000/](http://127.0.0.1:4000/)로 접속 시 로컬에서 블로그가 실행되는 것을 볼 수 있습니다.

`jekyll serve` 명령어로 블로그를 실행할 수 있습니다.

---
