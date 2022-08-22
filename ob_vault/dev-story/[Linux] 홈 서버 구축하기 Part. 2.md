---
date: 2020-05-06 01:08
lastmod: 2020-05-06 01:08
tags:
- dev
- Linux
- TLS
- System
- Let's Encrypt
- Certbot
- NGINX
---

# 개요

지난번 글인 [홈 서버 구축하기 Part. 1]([Linux]%20홈%20서버%20구축하기%20Part.%201) 에서 Certbot을 통해 HTTPS 서버를 설정하는 것을 했다. 그러나 해당 방식을 통해 발급받은 인증서는 유효기간이 존재하고 이를 연장하기위한 방법을 알아보자.

# 인증서 자동 갱신

```shell
/etc/nginx$ sudo certbot renew --dry-run
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/${YOUR_DOMAIN_NAME}.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Cert not due for renewal, but simulating renewal for dry run
Plugins selected: Authenticator standalone, Installer None
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for ${YOUR_DOMAIN_NAME}
Cleaning up challenges
Attempting to renew cert (${YOUR_DOMAIN_NAME}) from /etc/letsencrypt/renewal/${YOUR_DOMAIN_NAME}.conf produced an unexpected error: Problem binding to port 80: Could not bind to IPv4 or IPv6.. Skipping.
All renewal attempts failed. The following certs could not be renewed:
  /etc/letsencrypt/live/${YOUR_DOMAIN_NAME}/fullchain.pem (failure)

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates below have not been saved.)

All renewal attempts failed. The following certs could not be renewed:
  /etc/letsencrypt/live/${YOUR_DOMAIN_NAME}/fullchain.pem (failure)
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates above have not been saved.)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1 renew failure(s), 0 parse failure(s)

IMPORTANT NOTES:
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.

```

```shell
$ sudo certbot renew --dry-run
 Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/${YOUR_DOMAIN_NAME}.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Cert not due for renewal, but simulating renewal for dry run
Plugins selected: Authenticator standalone, Installer None
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for ${YOUR_DOMAIN_NAME}
Waiting for verification...
Challenge failed for domain ${YOUR_DOMAIN_NAME}
http-01 challenge for ${YOUR_DOMAIN_NAME}
Cleaning up challenges
Attempting to renew cert (${YOUR_DOMAIN_NAME}) from /etc/letsencrypt/renewal/${YOUR_DOMAIN_NAME}.conf produced an unexpected error: Some challenges have failed.. Skipping.
All renewal attempts failed. The following certs could not be renewed:
  /etc/letsencrypt/live/${YOUR_DOMAIN_NAME}/fullchain.pem (failure)

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates below have not been saved.)

All renewal attempts failed. The following certs could not be renewed:
  /etc/letsencrypt/live/${YOUR_DOMAIN_NAME}/fullchain.pem (failure)
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates above have not been saved.)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1 renew failure(s), 0 parse failure(s)

IMPORTANT NOTES:
 - The following errors were reported by the server:

   Domain: ${YOUR_DOMAIN_NAME}
   Type:   caa
   Detail: CAA record for ${YOUR_DOMAIN_NAME} prevents issuance

```

```shell
$ sudo certbot renew --dry-run 
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/vwan.iptime.org.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Cert not due for renewal, but simulating renewal for dry run
Plugins selected: Authenticator standalone, Installer None
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for vwan.iptime.org
Waiting for verification...
Cleaning up challenges

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
new certificate deployed without reload, fullchain is
/etc/letsencrypt/live/vwan.iptime.org/fullchain.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates below have not been saved.)

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/vwan.iptime.org/fullchain.pem (success)
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates above have not been saved.)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

```

# 관련자료

-   [Let's Encrypt Official Website](https://letsencrypt.org/)
-   [Let's Encrypt Official Documents](https://letsencrypt.org/docs/)
-   [Let's Encrypt 공식 한글 가이드](https://letsencrypt.org/ko/docs/)
-   [Certbot Official Website](https://certbot.eff.org/)
-   [Certbot Official Documents - Nginx on Ubuntu 20.04](https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx)
-   [NGINX Official Documents - Configuring HTTPS servers](http://nginx.org/en/docs/http/configuring_https_servers.html)
