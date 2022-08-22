---
date: 2022-08-21 02:08
lastmod: 2022-08-21 02:08
tags:
- tag
---

# Docker

```bash
$ brew install --cask docker
Running `brew update --preinstall`...
==> Tapping homebrew/cask
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask'...
remote: Enumerating objects: 643554, done.
remote: Counting objects: 100% (124/124), done.
remote: Compressing objects: 100% (66/66), done.
remote: Total 643554 (delta 62), reused 117 (delta 58), pack-reused 643430
Receiving objects: 100% (643554/643554), 304.69 MiB | 1.16 MiB/s, done.
Resolving deltas: 100% (455656/455656), done.
Updating files: 100% (4069/4069), done.
Tapped 4008 casks (4,080 files, 325.3MB).
==> Downloading <https://desktop.docker.com/mac/main/amd64/80466/Docker.dmg>
######################################################################## 100.0%
==> Installing Cask docker
==> Moving App 'Docker.app' to '/Applications/Docker.app'
==> Linking Binary 'docker-compose.bash-completion' to '/usr/local/etc/bash_completion.d/docker-compose'
==> Linking Binary 'docker-compose.zsh-completion' to '/usr/local/share/zsh/site-functions/_docker_compose'
==> Linking Binary 'docker.fish' to '/usr/local/share/fish/vendor_completions.d/docker.fish'
==> Linking Binary 'docker-compose.fish-completion' to '/usr/local/share/fish/vendor_completions.d/docker-compose.fish'
🍺  docker was successfully installed!
$
```

# Java 설치

```bash

$ brew tap adoptopenjdk/openjdk
Running `brew update --preinstall`...
==> Auto-updated Homebrew!
Updated 2 taps (homebrew/core and homebrew/cask).
==> Updated Formulae
Updated 17 formulae.
==> Updated Casks
Updated 1 cask.

==> Tapping adoptopenjdk/openjdk
Cloning into '/usr/local/Homebrew/Library/Taps/adoptopenjdk/homebrew-openjdk'...
remote: Enumerating objects: 1996, done.
remote: Counting objects: 100% (60/60), done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 1996 (delta 44), reused 54 (delta 39), pack-reused 1936
Receiving objects: 100% (1996/1996), 372.55 KiB | 5.91 MiB/s, done.
Resolving deltas: 100% (1424/1424), done.
Tapped 47 casks (69 files, 522KB).

# openjdk 버전 검색
$ brew search openjdk   
==> Formulae
openjdk ✔                     openjdk@11                    openjdk@17                    openjdk@8                     openj9                        openvdb

==> Casks
adoptopenjdk                                                adoptopenjdk/openjdk/adoptopenjdk12-openj9-jre-large        adoptopenjdk/openjdk/adoptopenjdk15-openj9-jre
adoptopenjdk/openjdk/adoptopenjdk-jre                       adoptopenjdk/openjdk/adoptopenjdk12-openj9-large            adoptopenjdk/openjdk/adoptopenjdk15-openj9-jre-large
adoptopenjdk/openjdk/adoptopenjdk-openj9                    adoptopenjdk/openjdk/adoptopenjdk13                         adoptopenjdk/openjdk/adoptopenjdk15-openj9-large
adoptopenjdk/openjdk/adoptopenjdk-openj9-jre                adoptopenjdk/openjdk/adoptopenjdk13-jre                     adoptopenjdk/openjdk/adoptopenjdk16
adoptopenjdk/openjdk/adoptopenjdk-openj9-jre-large          adoptopenjdk/openjdk/adoptopenjdk13-openj9                  adoptopenjdk/openjdk/adoptopenjdk16-jre
adoptopenjdk/openjdk/adoptopenjdk-openj9-large              adoptopenjdk/openjdk/adoptopenjdk13-openj9-jre              adoptopenjdk/openjdk/adoptopenjdk16-openj9
adoptopenjdk/openjdk/adoptopenjdk10                         adoptopenjdk/openjdk/adoptopenjdk13-openj9-jre-large        adoptopenjdk/openjdk/adoptopenjdk16-openj9-jre
adoptopenjdk/openjdk/adoptopenjdk11                         adoptopenjdk/openjdk/adoptopenjdk13-openj9-large            adoptopenjdk/openjdk/adoptopenjdk8
adoptopenjdk/openjdk/adoptopenjdk11-jre                     adoptopenjdk/openjdk/adoptopenjdk14                         adoptopenjdk/openjdk/adoptopenjdk8-jre
adoptopenjdk/openjdk/adoptopenjdk11-openj9                  adoptopenjdk/openjdk/adoptopenjdk14-jre                     adoptopenjdk/openjdk/adoptopenjdk8-openj9
adoptopenjdk/openjdk/adoptopenjdk11-openj9-jre              adoptopenjdk/openjdk/adoptopenjdk14-openj9                  adoptopenjdk/openjdk/adoptopenjdk8-openj9-jre
adoptopenjdk/openjdk/adoptopenjdk11-openj9-jre-large        adoptopenjdk/openjdk/adoptopenjdk14-openj9-jre              adoptopenjdk/openjdk/adoptopenjdk8-openj9-jre-large
adoptopenjdk/openjdk/adoptopenjdk11-openj9-large            adoptopenjdk/openjdk/adoptopenjdk14-openj9-jre-large        adoptopenjdk/openjdk/adoptopenjdk8-openj9-large
adoptopenjdk/openjdk/adoptopenjdk12                         adoptopenjdk/openjdk/adoptopenjdk14-openj9-large            adoptopenjdk/openjdk/adoptopenjdk9
adoptopenjdk/openjdk/adoptopenjdk12-jre                     adoptopenjdk/openjdk/adoptopenjdk15                         microsoft-openjdk
adoptopenjdk/openjdk/adoptopenjdk12-openj9                  adoptopenjdk/openjdk/adoptopenjdk15-jre                     openkey
adoptopenjdk/openjdk/adoptopenjdk12-openj9-jre              adoptopenjdk/openjdk/adoptopenjdk15-openj9

# 17 설치
$ brew install openjdk@17
==> Downloading <https://ghcr.io/v2/homebrew/core/openjdk/17/manifests/17.0.3>
######################################################################## 100.0%
==> Downloading <https://ghcr.io/v2/homebrew/core/openjdk/17/blobs/sha256:b7cf051662b5d6a7839e6d65010adff4a0c980fa03b56447090996d6052aa569>
==> Downloading from <https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:b7cf051662b5d6a7839e6d65010adff4a0c980fa03b56447090996d6052aa569?se=2022-06-06T11%3A45%3A00Z&si########################################################################> 100.0%
==> Pouring openjdk@17--17.0.3.monterey.bottle.tar.gz
==> Caveats
For the system Java wrappers to find this JDK, symlink it with
  sudo ln -sfn /usr/local/opt/openjdk@17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-17.jdk

openjdk@17 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have openjdk@17 first in your PATH, run:
  echo 'export PATH="/usr/local/opt/openjdk@17/bin:$PATH"' >> ~/.zshrc

For compilers to find openjdk@17 you may need to set:
  export CPPFLAGS="-I/usr/local/opt/openjdk@17/include"

==> Summary
🍺  /usr/local/Cellar/openjdk@17/17.0.3: 639 files, 305.5MB
==> Running `brew cleanup openjdk@17`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).

# Java 가 설치된 곳 확인
$ /usr/libexec/java_home -V

# 시스템 java 래퍼가 jdk를 찾을 수 있게 링크 생성
$ sudo ln -sfn $(brew --prefix)/opt/openjdk@17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-17.jdk
Matching Java Virtual Machines (1):
    17.0.3 (x86_64) "Homebrew" - "OpenJDK 17.0.3" /usr/local/Cellar/openjdk@17/17.0.3/libexec/openjdk.jdk/Contents/Home
/usr/local/Cellar/openjdk@17/17.0.3/libexec/openjdk.jdk/Contents/Home

# java 확인
$ java --version 
openjdk 17.0.3 2022-04-19
OpenJDK Runtime Environment Homebrew (build 17.0.3+0)
OpenJDK 64-Bit Server VM Homebrew (build 17.0.3+0, mixed mode, sharing)

# javac 확인
$ javac --version
javac 17.0.3
```

# Dock 에 투명 블럭 추가

1.  터미널 실행
2.  `defaults write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}'` 입력
3.  `killall Dock` 입력

# 한글 입력 시 **₩ 대신 `입력하기**

1.  ``~/Library` 폴더로 이동
2.  KeyBindings 폴더 생성
3.  `~/Library/KeyBindings` 폴더에 `DefaultkeyBinding.dict` 파일 생성
4.  `DefaultkeyBinding.dict` 파일에 아래의 코드를 추가
```shell
{
    "₩" = ("insertText:", "`");
}
```
5.  입력중인 애플리케이션 재실행
