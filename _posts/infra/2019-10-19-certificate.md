---
layout: post
title:  "[OpenSSL] SSL/TLS 통신을 위한 인증서 발급받기"
date:   2019-10-19 21:33:53
author: Choi HyeSun
categories: infra
tags:
  - 
---

## Key 쌍 생성

- SSL 호스트에서 사용할 RSA key pair(public, private key) 생성

  - 공개키 기반(PKI) = private key(개인키) + public key(공개키)

- 비밀번호를 사용하는 key 생성(imsi.key)

{% highlight bash %}
$ openssl genrsa -aes256 -out imsi.key 2048
Generating RSA private key, 2048 bit long modulus
...................................................................+++
....................................+++
e is 65537 (0x10001)
Enter pass phrase for imsi.key:[사용할 비밀번호]
Verifying - Enter pass phrase for imsi.key:[사용할 비밀번호]

### imsi.key 생성 확인
$ ls
imsi.key
{% endhighlight %}

- 비밀번호를 사용하지 않는 key 생성(cert.key)

  - 비밀번호를 사용하면 Nginx 기동/재기동시마다 Password를 입력해야 함
  
{% highlight bash %}
### 비밀번호 없이 구동하기 1 (2과 택1)
$ openssl rsa -in imsi.key -out cert.key
Enter pass phrase for imsi.key:
writing RSA key

### cert.key 생성 확인
$ ls
cert.key  imsi.key
{% endhighlight %}

- 사용하지 않는 key(imsi.key) 삭제 및 key 유출 방지를 위한 권한 설정

  - group / other에서 접근 못하도록 600 권한

{% highlight bash %}
### 사용하지 않는 imsi.key 제거
$ ls
cert.key  imsi.key

$ rm imsi.key

$ ls
cert.key

### key 유출 방지를 위한 권한 설정
$ ll
total 12
-rw-r--r-- 1 irteam irteam 1675 Oct 19 21:14 cert.key

$ chmod 600 cert.key

$ ll
total 12
-rw------- 1 irteam irteam 1675 Oct 19 21:14 cert.key
{% endhighlight %}

<br>
<br>

## CSR(Certificate Signing Request)란?

- 인증기관에 인증서 발급 요청을 하는 특별한 ASN.1 형식의 파일이며 그 안에는 내 공개키 정보와 사용하는 알고리즘 정보등이 들어 있음.

- 개인키는 외부에 유출되면 안 되므로 CSR같은 특별한 형식의 파일을 만들어서 인증기관에 전달하여 인증서를 발급 받음.

- 생성한 cert.key를 기반으로 cert.csr(인증서 정보) 생성

{% highlight bash %}
### 아래 내용 원래 입력해야하지만, 테스트용이기 때문에 PASS하였음 (그냥 다 엔터)
$ openssl req -new -key cert.key -out cert.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:
State or Province Name (full name) []:
Locality Name (eg, city) [Default City]:
Organization Name (eg, company) [Default Company Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (eg, your name or your server's hostname) []:
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:

### csr 생성 확인
$ ls
cert.csr  cert.key  imsi.key
{% endhighlight %}

<br>
<br>

## 자체 서명된 SSL 인증서를 생성

- 생성한 cert.key와 cert.csr 기반으로 crt(자체 서명된 SSL기반의 인증서) 생성

{% highlight bash %}
openssl x509 -req -days 365 -in cert.csr -signkey cert.key -out cert.crt
Signature ok
subject=/C=XX/L=Default City/O=Default Company Ltd
Getting Private key

### crt(웹서버 인증서 파일) 생성 확인
$ ls
cert.crt  cert.csr  cert.key  imsi.key

### 비밀번호 없이 구동하기 2 (1과 택1)
# 서버가 구동될때마다 개인 키에 설정된 PassPhrase를 물어보게 되므로 자동으로 모두 처리되도록 하기 위해 PassPhrase 제거
# cp cert.key cert.key.secure
# openssl rsa -in <파일명>.key.secure -out <파일명>.key
# writing RSA key
{% endhighlight %}

