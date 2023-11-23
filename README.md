### Jekyll 설치

터미널에서 `sudo gem install jekyll bundler` 명령어를 입력하여 `jekyll`과 `bundler`를 설치합니다.

잘 되지 않는다면 공식 사이트([Windows](https://jekyllrb.com/docs/installation/windows/), [macOS](https://jekyllrb.com/docs/installation/macos/))를 참고해주세요!

1. [루비 공식 다운로드](https://rubyinstaller.org/downloads/)

### 로컬에 블로그 실행

```shell
$ gem install jekyll
# 프로젝트 경로로 이동
$ cd licheco.github.io
# 패키지 설치
$ bundle install
# 서버 실행
$ bundle exec jekyll serve
```

jekyll 설치가 안되고 Apple Silicon일 경우

```shell
$ rbenv install 3.1.2
$ rbenv global 3.1.2
$ rbenv local 3.1.2
$ sudo gem install jekyll
```

[http://127.0.0.1:4000/](http://127.0.0.1:4000/)로 접속 시 로컬에서 블로그가 실행되는 것을 볼 수 있습니다.

`jekyll serve` 명령어로도 블로그를 실행할 수 있습니다.

---
