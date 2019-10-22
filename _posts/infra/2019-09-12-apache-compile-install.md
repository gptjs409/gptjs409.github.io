

## Apache(아파치) 서버

- = httpd

- 웹 서버

<br>
<br>

## Apache 서버 컴파일 설치

#### Linux 컴파일 설치 준비

- 리눅스에서 컴파일 설치를 하려면 gcc, make 등이 필요

- 설치 : yum install gcc make gcc-c++

<br>

#### 설치시 필요한 소스파일 최신버전 다운로드

{% highlight bash %}
$ cd /usr/local/src
$ wget http://mirror.apache-kr.org/apr/apr-1.5.2.tar.bz2
$ wget http://mirror.apache-kr.org/apr/apr-util-1.5.4.tar.bz2
$ wget http://mirror.apache-kr.org/httpd/httpd-2.4.23.tar.bz2
$ wget http://downloads.sourceforge.net/project/pcre/pcre/8.33/pcre-8.33.tar.bz2
$ ls -l
total 9100
-rw-r--r-- 1 root root  826885 Apr 29  2015 apr-1.5.2.tar.bz2
-rw-r--r-- 1 root root  694427 Sep 20  2014 apr-util-1.5.4.tar.bz2
-rw-r--r-- 1 root root 6351875 Jul  5 04:50 httpd-2.4.23.tar.bz2
-rw-r--r-- 1 root root 1440869 May 30  2013 pcre-8.33.tar.bz2
{% endhighlight %}

<br>

#### 소스파일 압축 해제

{% highlight bash %}
$ tar xvf apr-1.5.2.tar.bz2
$ tar xvf apr-util-1.5.4.tar.bz2
$ tar xvf httpd-2.4.23.tar.bz2
$ tar xvf pcre-8.33.tar.bz2
$ ls -l
total 9116
drwxr-xr-x 27 1000  1000    4096 Apr 25  2015 apr-1.5.2
-rw-r--r--  1 root root   826885 Apr 29  2015 apr-1.5.2.tar.bz2
drwxr-xr-x 19 1000  1000    4096 Sep 17  2014 apr-util-1.5.4
-rw-r--r--  1 root root   694427 Sep 20  2014 apr-util-1.5.4.tar.bz2
drwxr-xr-x 11  501 games    4096 Jul  1 02:15 httpd-2.4.23
-rw-r--r--  1 root root  6351875 Jul  5 04:50 httpd-2.4.23.tar.bz2
drwxr-xr-x  7 1169  1169    4096 May 28  2013 pcre-8.33
-rw-r--r--  1 root root  1440869 May 30  2013 pcre-8.33.tar.bz2
$ mv apr-1.5.2 ./httpd-2.4.23/srclib/apr
$ mv apr-util-1.5.4 ./httpd-2.4.23/srclib/apr-util
$ ls -l
total 9108
-rw-r--r--  1 root root   826885 Apr 29  2015 apr-1.5.2.tar.bz2
-rw-r--r--  1 root root   694427 Sep 20  2014 apr-util-1.5.4.tar.bz2
drwxr-xr-x 11  501 games    4096 Jul  1 02:15 httpd-2.4.23
-rw-r--r--  1 root root  6351875 Jul  5 04:50 httpd-2.4.23.tar.bz2
drwxr-xr-x  7 1169  1169    4096 May 28  2013 pcre-8.33
-rw-r--r--  1 root root  1440869 May 30  2013 pcre-8.33.tar.bz2
{% endhighlight %}

<br>

#### PCRE 설치

{% highlight bash %}
$ cd /usr/local/src/pcre-8.33
$ ./configure
$ make
$ make install
 ... (전략)
make[3]: Leaving directory `/usr/local/src/pcre-8.33'
make[2]: Leaving directory `/usr/local/src/pcre-8.33'
make[1]: Leaving directory `/usr/local/src/pcre-8.33'
{% endhighlight %}

<br>

#### Apache 설치

- configure : 아파치 웹 서버를 컴파일하고 설치하기 위해 소스 트리를 구성

- make : 소스코드를 컴파일해서 binary 파일을 만든다.

- make install : 만들어진 binary 파일을 지정된 경로로 이동시킨다

{% highlight bash %}
$ cd /usr/local/src/httpd-2.4.23
$ ./configure --prefix=/usr/local/httpd
$ make
$ make install
{% endhighlight %}

<br>

#### Apache 실행

- 에러 발생

{% highlight bash %}
$ /usr/local/httpd/bin/httpd -k start
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using localhost.localdomain. Set the 'ServerName' directive globally to suppress this message
{% endhighlight %}

- 설정파일 변경

  - #ServerName www.example.com:80 → ServerName www.example.com:80 (주석제거)
  
{% highlight bash %}
$ vi /usr/local/httpd/conf/httpd.conf

(변경 전)
#ServerName www.example.com:80

(변경 후)
ServerName www.example.com:80
{% endhighlight %}

- 재실행 → OK

{% highlight bash %}
$ /usr/local/httpd/bin/httpd -k start
{% endhighlight %}

<br>

#### 서비스(데몬) 등록

{% highlight bash %}
$ cd /usr/local/httpd/bin/
$ cp apachectl /etc/init.d/httpd
$ service httpd stop
httpd (no pid file) not running
$ ps –elf | grep httpd
$ lsof –ni | grep httpd
$ usr/local/httpd/bin/httpd –k stop
$ service http start
{% endhighlight %}

#### 끝. 80으로 접속하면 Apache 웹 서버로 접속

<br>
<br>

#### Apache 서버 설치시 경로 및 파일들 (참고)

{% highlight bash %}
$ cd /usr/local/httpd/modules
설치된 모듈들이 들어있는 파일

$ cd /usr/local/httpd
httpd 설치 경로

$ cd /usr/local/src
httpd 설치 관련 파일 다운로드한 경로

$ ./configure --help
configure에 관련된 도움말

$ make clean
컴파일한 파일들을 클린시켜줌

$ ./configure --prefix=/usr/local/httpd --enable-ssl
ssl 모듈 설치 (openssl, openssl-devel이 필요 yum으로 설치)

$ make && make install
컴파일 및 설치
{% endhighlight %}
