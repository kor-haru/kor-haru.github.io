---
date: 2020-05-05 01:08
lastmod: 2020-05-05 01:08
tags:
- dev
- Linux
- TLS
- System
- Let's Encrypt
- Certbot
- NGINX
---

## 개요

끄아아아... 연휴동안 정신놓고 게으른(~~무인도~~) 생활을 보내버렸다... 계속해서 자신과의 약속을 어기는 중... :(

다시금 정신을 가다듬고... 최근에 뭔가 가지고 놀만한게 없을까 하다가 새로운 LTS 버전인 `Ubuntu 20.04`도 나오기도 했고 ~~(OS 기본 Python 이 3.8 버전으로 바꼈다!!)~~  회사에서는 `CentOS` 위주로 사용하다보니 오랜만에 `Ubuntu`를 쓰고 싶기도 하고 뭔가 개인 프로젝트를 해야겠다는 압박감(?)에 집에 놀고있는 구형 맥북을 꺼내어 포맷을 하고 `Ubuntu 20.04 LTS`를 설치 해버렸다...

기왕 개인 서버를 구성해서 토이 프로젝트를 하기로 한 김에 기본부터 다시 공부한다는 생각으로 예전에 해봤던 것들을 다시 해보기로 다짐하면서 과거에 잠깐 해보면서 정리하고 다시 꺼내보지 않은 내용들을 하나하나 다시 보기로 했다. ~~(이번에도 열심히 서버 구성만 하고 프로젝트는 안하겠지...)~~

---

## 암호화 하자! (Let's Encrypt)

우선, 모던 WEB 서비스라 하면 `HTTP/2`는 아니더라도 최소한 `HTTPS`는 적용을 해야 브라우저 API를 모두 활용할 수 있는 서비스를 제공할 수 있기 때문에 이것 저것 토이 프로젝트를 해보려고 할 때 항상 문제가 되는 녀석부터 해결해야한다.

예전에 사용한 [Let's Encrypt](https://letsencrypt.org/) 라는 ~~무료~~ 인증 서비스를 다시 적용하기 위해 [공식 문서](https://letsencrypt.org/docs/)([한글 버전](https://letsencrypt.org/ko/docs/)도 있다)를 참조했다. 가장 일반적인 대상 서버의 Shell에 접속할 수 있는 상황에서 적용하는 방법인 `Certbot ACME` 클라이언트를 사용하는 것으로 인증서를 적용할 것이다. Certbot 외의 ACME 클라이언트를 활용하거나 Shell에 접속할 수 없는 등의 다양한 상황의 적용방법을 제공 받을 수 있다.

[Let's Encrypt](https://letsencrypt.org/)와 관련된 내용은 찾아보면 많이 있고 [공식 문서](https://letsencrypt.org/docs/)([한글 버전](https://letsencrypt.org/ko/docs/))도 굉장히 잘 작성되어 있기 때문에 굳이 정리하지 않겠다. ~~(뭐.. 사실 적용 방법도 찾아보면 많은데...)~~

---

## 인증서 발급 받기 (feat. Certbot)

일단은 사용할 환경의 Host OS는 `Ubuntu 20.04`이고  WEB 서버는 `Nginx`를 이용할 것이기 때문에 [Nginx on Ubuntu 20.04 가이드](https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx)를 참고하였고, 이와 다른 운영체제 및 WEB 서버의 경우엔 [certbot 공식 홈페이지](https://certbot.eff.org/)에서 원하는 조합에 따라 가이드를 제공받을 수 있다. **즉, 아래 내용은 `Ubuntu 20.04 + Nginx` 조합에 해당하는 내용으로 다른 환경과 차이가 있을 수 있다.**

일단 [Nginx on Ubuntu 20.04 가이드](https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx)를 보면 아래와 같이 7가지의 절차가 있는데:

> 1. sudo 권한이 있는 사용자로 웹사이트를 실행할  서버에 SSH로 접속하기
> 2. universe 레포지토리 활성화
> 3. Certbot 설치
> 4. Certbot 실행 방법 선택
> 5. 인증서(certificate) 설치
> 6. 자동 리뉴얼 테스트
> 7. Certbot 동작 확인

이중에서 다른 과정은 그냥 따라하면 되는데 `과정 4`에서 현재 서버 상황에 따라 선택이 요구된다.

> 1. 현재 서버에서 WEB 서버가 실행되고 있지 않거나 서비스 중단이 가능한 경우
> 2. WEB 서버를 중단하지 않고 계속 실행해야하는 경우

1번의 경우 Certbot이 임시적으로 WEB 서버를 작동시켜 인증서를 발급 받는 방식이고 2번은 현재 동작중인 WEB 서버를 중단하지 않고 `Nginx`의 `webroot plugin`을 사용하여 특정 파일을 쓰는 것으로 인증을 받는 방식으로 아마도 WEB 서버를 실행하는 시점에 해당 플러그인을 사용하도록 설정을 해줘야 하는 모양이다....

하지만 본 글은 초기 셋팅을 기준으로 하기 때문에 가이드의 `과정 3.`까지 수행하여 `Certbot`을 설치하고 `과정 4-1.`을 수행하면 깔끔하게...! :

```shell
vwan@valhalla:~$ sudo certbot certonly --standalone
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator standalone, Installer None
Please enter in your domain name(s) (comma and/or space separated)  (Enter 'c'
to cancel): ${INPUT_DOMAIN_NAME}
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for ${YOUR_DOMAIN_NAME}
Waiting for verification...
Challenge failed for domain ${YOUR_DOMAIN_NAME}
http-01 challenge for ${YOUR_DOMAIN_NAME}
Cleaning up challenges
Some challenges have failed.       # 응...? failed....?

IMPORTANT NOTES:
 - The following errors were reported by the server:

   Domain: ${YOUR_DOMAIN_NAME}
   Type:   caa
   Detail: CAA record for ${YOUR_DOMAIN_NAME} prevents issuance
```

실패한다... :( 설명을 보고 Certbot에 내장된 WEB 서버가 있을거라 예상했는데... 그건 아닌거 같고 실행할 WEB 서버의 설치가 필요한것 같아서 `Nginx`를 설치하고 다시 시도하면:

```shell
$ sudo apt install nginx
패키지 목록을 읽는 중입니다... 완료
의존성 트리를 만드는 중입니다
상태 정보를 읽는 중입니다... 완료
다음 새 패키지를 설치할 것입니다:
  nginx
0개 업그레이드, 1개 새로 설치, 0개 제거 및 4개 업그레이드 안 함.
3,616 바이트 아카이브를 받아야 합니다.
이 작업 후 45.1 k바이트의 디스크 공간을 더 사용하게 됩니다.
받기:1 http://kr.archive.ubuntu.com/ubuntu focal/main amd64 nginx all 1.17.10-0ubuntu1 [3,616 B]
내려받기 3,616 바이트, 소요시간 1초 (3,789 바이트/초)
Selecting previously unselected package nginx.
(데이터베이스 읽는중 ...현재 175240개의 파일과 디렉터리가 설치되어 있습니다.)
Preparing to unpack .../nginx_1.17.10-0ubuntu1_all.deb ...
Unpacking nginx (1.17.10-0ubuntu1) ...
nginx (1.17.10-0ubuntu1) 설정하는 중입니다 ...

$ sudo certbot certonly --standalone
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator standalone, Installer None
Enter email address (used for urgent renewal and security notices) (Enter 'c' to
cancel): ${INPUT_EMAIL_ADDRESS}

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server at
https://acme-v02.api.letsencrypt.org/directory
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(A)gree/(C)ancel: a

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing to share your email address with the Electronic Frontier
Foundation, a founding partner of the Let's Encrypt project and the non-profit
organization that develops Certbot? We'd like to send you email about our work
encrypting the web, EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y
Please enter in your domain name(s) (comma and/or space separated)  (Enter 'c'
to cancel): ${INPUT_DOMAIN_NAME}
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for ${YOUR_DOMAIL_NAME}
Cleaning up challenges
Problem binding to port 80: Could not bind to IPv4 or IPv6.

IMPORTANT NOTES:
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
```

성공적인 설치를 축하하는 메세지가 ㄷㄷㄷㅈ. 메세지를 참조하여 `/etc/letsencrypt` 경로를 살펴보면:

``` shell
$ sudo tree -L 3 /etc/letsencrypt/
/etc/letsencrypt/
├── accounts
│   └── acme-v02.api.letsencrypt.org
│       └── directory
├── archive
│   └── ${YOUR_DOMAIN_NAME}
│       ├── cert1.pem
│       ├── chain1.pem
│       ├── fullchain1.pem
│       └── privkey1.pem
├── cli.ini
├── csr
│   ├── 0000_csr-certbot.pem
│   ├── 0001_csr-certbot.pem
│   ├── 0002_csr-certbot.pem
│   └── 0003_csr-certbot.pem
├── keys
│   ├── 0000_key-certbot.pem
│   ├── 0001_key-certbot.pem
│   ├── 0002_key-certbot.pem
│   └── 0003_key-certbot.pem
├── live
│   ├── README
│   └── ${YOUR_DOMAIN_NAME}
│       ├── README
│       ├── cert.pem -> ../../archive/${YOUR_DOMAIN_NAME}/cert1.pem
│       ├── chain.pem -> ../../archive/${YOUR_DOMAIN_NAME}/chain1.pem
│       ├── fullchain.pem -> ../../archive/${YOUR_DOMAIN_NAME}/fullchain1.pem
│       └── privkey.pem -> ../../archive/${YOUR_DOMAIN_NAME}/privkey1.pem
├── renewal
│   └── ${YOUR_DOMAIN_NAME}.conf
└── renewal-hooks
    ├── deploy
    ├── post
    └── pre

14 directories, 20 files
```

관련 인증서들이 발급되어 저장된 것을 볼 수 있다.

---

## 인증서 사용해서 HTTPS 적용하기

위에서 발급받은 인증서 중 서버에 사용할 녀석들은 `/etc/letsencrypt/live` 하위에 있는 `fullchain.pem`과 `privkey.pem`이다. 우선은 `Nginx` 설정에 인증서를 추가해서 HTTPS가 잘 되는지 확인해보자.

> [!warning] 주의사항
>  nginx의 구성에 따라 설정해야되는 파일이 다를 수 있는데 `/etc/nginx/nginx.conf` 혹은 `/etc/nginx/sites-available` 하위의 파일을 수정해야한다.  본 글은 별다른 설정 없이 패키지 매니저를 사용한 기본 설치를 통한 구성을 기준으로 한다.

```shell
# 버전 확인
$ sudo nginx -v
nginx version: nginx/1.17.10 (Ubuntu)

$ sudo tree -L 3 /etc/nginx/
/etc/nginx/
├── conf.d
├── fastcgi.conf
├── fastcgi_params
├── koi-utf
├── koi-win
├── mime.types
├── modules-available
├── modules-enabled
│   ├── 50-mod-http-image-filter.conf -> /usr/share/nginx/modules-available/mod-http-image-filter.conf
│   ├── 50-mod-http-xslt-filter.conf -> /usr/share/nginx/modules-available/mod-http-xslt-filter.conf
│   ├── 50-mod-mail.conf -> /usr/share/nginx/modules-available/mod-mail.conf
│   └── 50-mod-stream.conf -> /usr/share/nginx/modules-available/mod-stream.conf
├── nginx.conf
├── proxy_params
├── scgi_params
├── sites-available
│   └── default
├── sites-enabled
│   └── default -> /etc/nginx/sites-available/default
├── snippets
│   ├── fastcgi-php.conf
│   └── snakeoil.conf
├── uwsgi_params
└── win-utf

6 directories, 18 files
```

설치 방식에 따라 조금씩 다를 수 있기에... 본 글에서 구성된 환경은 기본 설정이 위와 같이 `sites-available` 경로 하위에 `default` 파일로 설정 되어있다. 설정파일을 열어보면 `Nginx`에서 다루는 단위인 `server`의 설정 블록을 볼 수 있다. 우선은 인증서를 적용해서 HTTPS를 설정하는 방법은 [공식 문서](http://nginx.org/en/docs/http/configuring_https_servers.html)를 참조하여 설정하면 된다. 이외의 `server`별 자세한 설정 방법은 [공식 위키](https://www.nginx.com/resources/wiki/start/)를 참고하거나 검색을 해보면... 굉장히 많이 있다.

```shell
...
server {
    ...
    listen 443 ssl default_server;     # 주석 제거
    listen [::]443 ssl default_server; # 주석 제거
    ...
    # Let's Encrypt로 발급받은 인증서 설정 추가
    ssl_certificate /etc/letsencrypt/live/${YOUR_DOMAIN_NAME}/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/${YOUR_DOMAIN_NAME}/privkey.pem;
    ...
}
...
```

위와 같이 인증서를 적용하고 `nginx -s reload` 로 `Nginx`를 재시작을 한 뒤 `https://${YOUR_DOMAIN_NAME}`으로 접속을 하면...?

![브라우저에서 인증서 확인](_res/20220821015928.png)

짜잔~ ~~내가 돌아왔다~~  `Ley's Encrypt`를 통해 발급받은 인증서를 사용하여 HTTPS가 잘 적용된것을 볼 수 있다. 일단은 여기까지...

---

## 관련자료

* [Let's Encrypt Official Website](https://letsencrypt.org/)
* [Let's Encrypt Official Documents](https://letsencrypt.org/docs/)
* [Let's Encrypt 공식 한글 가이드](https://letsencrypt.org/ko/docs/)
* [Certbot Official Website](https://certbot.eff.org/)
* [Certbot Official Documents - Nginx on Ubuntu 20.04](https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx)
* [NGINX Official Documents - Configuring HTTPS servers](http://nginx.org/en/docs/http/configuring_https_servers.html)
