---
layout: post
title:  "Shell Script 기본"
date:   2019-10-15 12:43:27
author: Choi HyeSun
categories: Infra
tags:
  - Shell Script
  - ShellScript
  - Script
  - Shell
  - bash
---

## Bash Shell
CentOS의 기본 쉘은 Bash(Bourne Again SHell : "배쉬 쉘")

<br>

Bash Shell의 특징

  - Alias 기능(명령어 단축 기능)

  - History 기능(↑/↓)

  - 연산 기능

  - Job Control 기능
  
  - 자동 이름 완성 기능(Tab키)
  
  - 프롬프트 제어 기능
  
  - 명령 편집 기능
  
<br>

Shell의 명령문 처리 방법

- (프롬프트) 명령어 [옵션...] [인자...]

- 예) # rm -rf /sun

<br>
<br>

## 환경 변수
`echo $환경변수이름`으로 확인 가능

{% highlight bash %}
[sun@localhost ~]$ echo $HOME
/home/sun
{% endhighlight %}

<br>

`export 환경변수=값`으로 환경 변수의 값 변경 가능 - 단, 로그인 세션에서만 유지

{% highlight bash %}
[sun@localhost ~]$ echo $WOW
[sun@localhost ~]$ export WOW=1234
[sun@localhost ~]$ echo $WOW
1234
{% endhighlight %}

<br>

주요 환경 변수

|환경 변수|설명|예|
|---|---|---|
|HOME|현재 사용자의 홈 디렉토리|`/home/sun`|
|LANG|현재 지원되는 언어|`ko_KR.UTF-8`|
|TERM|로그인 터미널 타입|`xterm`|
|USER|현재 사용자 이름|`sun`|
|COLUMNS|현재 터미널의 컬럼 수|`80`|
|PS1|1차 명령 프롬프트 변수|`[\u@\h \W]\$`|
|BASH|bash 셀의 경로|`/bin/bash`|
|HISTFILE|히스토리 파일 경로|`/home/sun/.bash_history`|
|HOSTNAME|호스트의 이름|`localhost.localdomain`|
|LOGNAME|로그인 이름|`sun`|
|MAIL|메일 보관 경로|`/var/spool/mail/sun`|
|PATH|실행 파일을 찾는 디렉터리 경로|`/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/sun/.local/bin:/home/sun/bin`|
|PWD|사용자의 현재 작업 디렉터리|`/home/sun`|
|SHELL|로그인해서 사용중인 쉘|`/bin/bash`|
|LINES|현재 터미널 라인 수|`24`|
|PS2|2차 명령 프롬프트(대개 `>`)|`>`|
|BASH_VERSION|bash 버전|`4.2.46(2)-release`|
|HISTSIZE|히스토리 파일에 저장되는 개수|`1000`|
|OSTYPE|운영체제 타입|`linux-gnu`|

<br>
<br>

## 쉘 스크립트 프로그래밍 특징

- C언어와 유사하게 프로그래밍 가능

- 변수, 반복문, 제어문 등의 사용이 가능

- 별도로 컴파일하지 않고 텍스트 파일 형태로 바로 실행

- vi 등으로 작성이 가능

- 리눅스의 많은 부분이 쉘 스크립트로 작성되어 있음

<br>
<br>

## 쉘 스크립트의 작성과 실행

작성시

- vi로 작성

- 쉘 스크립트 파일의 확장명은 되도록 `*.sh`로

<br>

작성 내용

{% highlight bash %}
[sun@localhost ~]$ vi name.sh
#!/bin/sh
echo "호스트명": $HOSTNAME
exit 0
{% endhighlight %}

- 첫줄 #!/bin/sh → 사용할 언어 선택(sh 쉘)

<br>

실행 방법 1. `sh <스크립트파일>`

{% highlight bash %}
[sun@localhost ~]$ ll
합계 4
-rw-rw-r--. 1 sun sun 48 10월 15 11:33 name.sh
[sun@localhost ~]$ sh name.sh
호스트명: localhost.localdomain
{% endhighlight %}

<br>

실행 방법 2. 실행 가능 속성 부여 `chmod +x <스크립트파일>` 후 `./<스크립트파일>`로 실행

{% highlight bash %}
[sun@localhost ~]$ chmod +x name.sh
[sun@localhost ~]$ ll
합계 4
-rwxrwxr-x. 1 sun sun 48 10월 15 11:33 name.sh
[sun@localhost ~]$ ./name.sh
호스트명: localhost.localdomain
{% endhighlight %}

<br>

> 스크립트 파일의 속성을 `755`로 변경해주면 모든 사용자가 스크립트를 사용할 권한이 생김
>
> 스크립트 파일을 /user/local/bin 디렉토리에 복사해두면 모든 사용자가 접근할 수 있음
>
> 보안상 root 권한으로 수행

<br>
<br>

## 변수의 기본

- 사용 전 미리 선언하지 않음. 처음 값이 할당되면서 자동으로 변수가 생성

- 모든 변수는 문자열(String)로 취급

- 대소문자를 구분

- 변수 대입시 `=` 좌우에 공백이 없어야 함

- 값에 띄어쓰기(공란)가 있다면, `"` / `'` 등으로 묶어줘야 함

- `$` 문자가 들어간 글자를 사용하려면 `' '`로 묶어주거나 앞에 `\`를 붙이면 됨

{% highlight bash %}
[sun@localhost ~]$ vi var1.sh
#!/bin/sh
myvar="Hello Sun"
echo $myvar
echo "$myvar"
echo '$myvar'
echo \$myvar
echo 값 입력 :
read myvar
echo '$myvar' = myvar
exit 0
[sun@localhost ~]$ sh var1.sh
Hello Sun
Hello Sun
$myvar
$myvar
값 입력 :
아무 값을 입력해보고 있음
$myvar = 아무 값을 입력해보고 있음
{% endhighlight %}

<br>
<br>

## 숫자 계산

- 변수에 대입된 값은 모두 문자열로 취급

- 변수에 들어있는 값을 숫자로 해서 `+`, `-`, `*`, `/` 등의 연산을 하려면 `` `expr (연산식)` ``을 사용

- 수식에 괄호`()`나 곱하기`*`의 경우 그 앞에 꼭 역슬래쉬`\`를 붙임

{% highlight bash %}
[sun@localhost ~]$ vi num1.sh
#!/bin/sh
num1=100
num2=$num1+200
echo $num2
num3=`expr $num1 + 200`
echo $num3
num4=`expr \( $num1 + 200 \) / 10 \* 2`
echo $num4
exit 0
[sun@localhost ~]$ sh num1.sh
100+200
300
60
{% endhighlight %}

<br>
<br>

## 파라미터(Parameter) 변수

- 파라미터 변수는 $0, $1, $2...의 형태

- 전체 파라미터는 $*

{% highlight bash %}
[sun@localhost ~]$ vi para1.sh
#!/bin/sh
echo "실행파일 이름은 <$0>"
echo "첫 번째 파라미터 <$1>"
echo "두 번째 파라미터 <$2>"
echo "세 번째 파라미터 <$3>"
echo "전체 파라미터 <$*>"
exit 0
[sun@localhost ~]$ sh para1.sh 값1 값2 값3
실행파일 이름은 <para1.sh>
첫 번째 파라미터 <값1>
두 번째 파라미터 <값2>
세 번째 파라미터 <값3>
전체 파라미터 <값1 값2 값3>
{% endhighlight %}

<br>

- 예

|파라미터변수|$0|$1|$2|$3|
|---|---|---|---|---|
|명령어1|yum|-y|install|gftp|
|명령어2|para1.sh|값1|값2|값3|

<br>
<br>

## 조건문 - 기본 if문

형식

{% highlight bash %}
if [ 조건 ]    
then           
  참일경우 실행
fi
{% endhighlight %}

> " 조건 "의 사이의 각 단어에는 모두 공백이 있어야 함

{% highlight bash %}
[sun@localhost ~]$ vi if1.sh
#!/bin/sh
if [ "sun" = "sun" ]
then
  echo "참입니다"
fi
exit 0
[sun@localhost ~]$ sh if1.sh
참입니다
{% endhighlight %}


<br>
<br>

## 조건문 - if~else문

형식

{% highlight bash %}
if [ 조건 ]    
then           
  참일경우 실행
else
  거짓인 경우 실행
fi
{% endhighlight %}

> 중복 if문을 위해서 else if가 합쳐진 elif문도 사용할 수 있음

{% highlight bash %}
[sun@localhost ~]$ vi if2.sh
#!/bin/sh
if [ "sun" != "sun" ]
then
  echo "참입니다"
else
  echo "거짓입니다"
fi
exit 0
[sun@localhost ~]$ sh if2.sh
거짓입니다
{% endhighlight %}

<br>
<br>

## 조건문에 들어가는 비교 연산자

문자열 비교

|문자열 비교|결과|
|---|---|
|"문자열1" = "문자열2"|두 문자열이 같으면 참|
|"문자열1" != "문자열2"|두 문자열이 같지 않으면 참|
|-n "문자열"|문자열이 NULL(빈 문자열)이 아니면 참|
|-z "문자열"|문자열이 NULL(빈 문자열)이면 참|

<br>

산술 비교

|산술 비교|결과|
|---|---|
|수식1 -eq 수식2|두 수식(또는 변수)이 같으면 참|
|수식1 -ne 수식2|두 수식(또는 변수)이 같지 않으면 참|
|수식1 -gt 수식2|수식 1이 크면 참|
|수식1 -ge 수식2|수식 1이 크거나 같으면 참|
|수식1 -lt 수식2|수식 1이 작으면 참|
|수식1 -le 수식2|수식 1이 작거나 같으면 참|
|!수식|수식이 거짓이라면 참|

{% highlight bash %}
[sun@localhost ~]$ vi if3.sh
#!/bin/sh
if [ 100 -eq 200 ]
then
  echo "100과 200은 같음"
else
  echo "100과 200은 다름"
fi
exit 0
[sun@localhost ~]$ sh if3.sh
100과 200은 다름
{% endhighlight %}

<br>
<br>

## 파일과 관련된 조건

파일 조건

|파일 조건|결과|
|---|---|
|-d 파일명|디렉터리면 참|
|-e 파일명|존재하면 참|
|-f 파일명|일반파일이면 참|
|-g 파일명|set-group-id가 설정되어있으면 참|
|-r 파일명|읽기 가능이면 참|
|-s 파일명|크기가 0이 아니면 참|
|-u 파일명|set-user-id가 설정되어있으면 참|
|-w 파일명|쓰기 가능이면 참|
|-x 파일명|실행 가능이면 참|

{% highlight bash %}
[sun@localhost ~]$ pwd
/home/sun
[sun@localhost ~]$ ls
if1.sh  if2.sh  if3.sh  name.sh  num1.sh  para1.sh  var1.sh
[sun@localhost ~]$ vi if4.sh
#!/bin/bash
fname=/home/sun/num1.sh
if [ -f $fname ]
then
   head -3 $fname
else
   echo "num1.sh 파일이 없습니다."
fi
exit 0
[sun@localhost ~]$ sh if4.sh
#!/bin/sh
num1=100
num2=$num1+200
{% endhighlight %}

<br>
<br>

## 조건문 - case~esac문(1)
- if문은 참 또는 거짓일 경우에만 사용 가능(이중분기)
- case문은 여러가지 경우의 수가 있다면 사용하면 좋음(다중분기)

{% highlight bash %}
[sun@localhost ~]$ vi case1.sh
#!/bin/sh
case "$1" in
  start)
    echo "시작";;
  stop)
    echo "중지";;
  restart)
    echo "재시작";;
  *)
    echo "알 수 없는 파라미터";;
esac
exit 0
[sun@localhost ~]$ sh case1.sh stop
중지
{% endhighlight %}

<br>
<br>

## 조건문 - case~esac문(2)

{% highlight bash %}
[sun@localhost ~]$ vi case2.sh
#!/bin/sh
echo "쉘스크립트가 재밌어요? (yes/no)"
read answer
case $answer in
  yes | y | Y | Yes | YE)
    echo "거짓말쟁이";;
  [nN]*)
    echo "진실된 사람이군요";;
  *)
    echo "yes or no 입니다"
    exit 1;;
esac
exit 0
[sun@localhost ~]$ sh case2.sh st
쉘스크립트가 재밌어요? (yes/no)
No
진실된 사람이군요
{% endhighlight %}

<br>
<br>

## AND / OR 관계 연산자

- and는 `&&` 사용
- or는 `||` 사용

{% highlight bash %}
[sun@localhost ~]$ vi andor1.sh
#!/bin/sh
echo "보고싶은 파일명을 입력해보세요"
ls
read fname
if [ -f $fname ] && [ -s $fname ] ; then
  head -5 $fname
else
  echo "파일이 없거나 크기가 0입니다."
fi
exit 0
[sun@localhost ~]$ sh andor1.sh
보고싶은 파일명을 입력해보세요
andor1.sh  case2.sh  if2.sh  if4.sh   num1.sh   var1.sh
case1.sh   if1.sh    if3.sh  name.sh  para1.sh
case1.sh
#!/bin/sh
case "$1" in
  start)
    echo "시작";;
  stop)
{% endhighlight %}

<br>
<br>

## 반복문 - for문(1)

형식

{% highlight bash %}
for 변수 in 값1 값2 값3 ...
do
  반복할 문장
done
{% endhighlight %}

> for i in 1 2 3 4 5 6 7 8 9 10
>
> = for((i=1;i<=10;i++))
>
> = for i in \`seq 1 10\`

{% highlight bash %}
[sun@localhost ~]$ vi for1.sh
#!/bin/sh
hap=0
for i in 1 2 3 4 5 6 7 8 9 10
#for((i=1;i<=10;i++))
#for i in `seq 1 10`
do
  hap=`expr $hap + $i`
done
echo "1~10까지의 합: "$hap
exit 0
[sun@localhost ~]$ sh for1.sh
1~10까지의 합: 55
{% endhighlight %}


<br>
<br>

## 반복문 - for문(2)

- 현재 디렉터리에 있는 쉘 스크립트 파일(\*.sh)의 파일명과 앞 3줄씩을 출력하는 프로그램

{% highlight bash %}
[sun@localhost ~]$ vi for2.sh
#!/bin/sh
for fname in $(ls *.sh)
do
  echo "--------$fname--------"
  head -3 $fname
done
exit 0
[sun@localhost ~]$ sh for2.sh
--------andor1.sh--------
#!/bin/sh
echo "보고싶은 파일명을 입력해보세요"
ls
--------case1.sh--------
#!/bin/sh
case "$1" in
  start)
--------case2.sh--------
#!/bin/sh
echo "쉘스크립트가 재밌어요? (yes/no)"
read answer
--------for1.sh--------
#!/bin/sh
hap=0
#for i in 1 2 3 4 5 6 7 8 9 10
--------for2.sh--------
#!/bin/sh
for fname in $(ls *.sh)
do
--------if1.sh--------
#!/bin/sh
if [ "woo" = "woo" ]
then
--------if2.sh--------
#!/bin/sh
if [ "woo" != "woo" ]
then
--------if3.sh--------
#!/bin/sh
if [ 100 -eq 200 ]
then
--------if4.sh--------
#!/bin/bash
fname=/home/sun/num1.sh
if [ -f $fname ]
--------name.sh--------
#!/bin/sh
echo "호스트명": $HOSTNAME
exit 0
--------num1.sh--------
#!/bin/sh
num1=100
num2=$num1+200
--------para1.sh--------
#!/bin/sh
echo "실행파일 이름은 <$0>"
echo "첫 번째 파라미터 <$1>"
--------var1.sh--------
#!/bin/sh
myvar="Hello Sun"
echo $myvar
{% endhighlight %}

<br>
<br>

## 반복문 - while문(1)

- 조건문이 참인 동안 계속 반복

{% highlight bash %}
[sun@localhost ~]$ vi while1.sh
#!/bin/sh
while [ 1 ]
do
  echo "Sun"
done
exit 0
[sun@localhost ~]$ sh while1.sh
Sun
Sun
Sun
Sun
Sun
Sun
Sun
Sun
Sun
Sun
Sun
Sun
(무한반복... Ctrl+C로 끊어주기)
{% endhighlight %}

> while은 조건에 [ 1 ] 또는 [ : ]가 오면 항상 참

<br>
<br>

## 반복문 - while문(2)

- 1 ~ 10까지의 합계 출력(for1.sh와 동일한 결과)

{% highlight bash %}
[sun@localhost ~]$ vi while2.sh
#!/bin/sh
hap=0
i=1
while [ $i -le 10 ]
do
  hap=`expr $hap + $i`
  i=`expr $i + 1`
done
echo "1~10까지의 합: "$hap
exit 0
[sun@localhost ~]$ sh while2.sh
1~10까지의 합: 55
{% endhighlight %}

> until문은 조건식이 참일때까지(거짓인 동안) 계속 반복
>
> while [ $i -le 10 ] = until [ $i -gt 10 ]

<br>
<br>

## 반복문 - while문(3)
- 비밀번호를 입력받고, 비밀번호가 맞을 때까지 계속 입력받는 스크립트

{% highlight bash %}
[sun@localhost ~]$ vi while3.sh
#!/bin/bash
echo "비밀번호를 입력하시오"
read mypass
while [ $mypass != "1234" ]
do
  echo "틀렸음. 재입력 요망"
  read mypass
done
echo "통과"
exit 0
[sun@localhost ~]$ sh while3.sh
비밀번호를 입력하시오
1
틀렸음. 재입력 요망
2
틀렸음. 재입력 요망
3
틀렸음. 재입력 요망
1234
통과
{% endhighlight %}

<br>
<br>

## 반복문 - until문

- while문과 용도가 거의 같음

- until문은 조건식이 참일때까지(=거짓일 동안) 계속 반복

<br>
<br>

## break, continue, exit, return 문

- break : 주로 반복문을 종료할 때 사용

- continue : 반복문의 조건식으로 돌아가도록

- exit : 해당 프로그램을 완전히 종료

- return : 함수 안에서 사용될 수 있으며, 함수를 호출한 곳으로 돌아가도록

{% highlight bash %}
[sun@localhost ~]$ vi bce.sh
#!/bin/sh
echo "무한 반복 입력 시작(b: break, c: continue, e: exit)"
while [ 1 ] ; do
read input
case $input in
  b | B )
    break ;;
  c | C )
    echo "continue를 누르면 while의 조건으로 돌아감"
    continue ;;
  e | E )
    echo "exit를 누르면 프로그램(함수)를 완전히 종료"
    exit 1 ;;
esac;
done
echo "break를 누르면 while을 빠져나와 지금 이 문장이 출력됨"
exit 0
[sun@localhost ~]$ sh bce.sh
무한 반복 입력 시작(b: break, c: continue, e: exit)
a
b
break를 누르면 while을 빠져나와 지금 이 문장이 출력됨
[sun@localhost ~]$ sh bce.sh
무한 반복 입력 시작(b: break, c: continue, e: exit)
a
c
continue를 누르면 while의 조건으로 돌아감
e
exit를 누르면 프로그램(함수)를 완전히 종료
{% endhighlight %}

<br>
<br>

## 사용자 정의 함수

형식

{% highlight bash %}
함수이름() { # 함수를 정의
  내용들...
}
함수이름 # 함수 호출
{% endhighlight %}

{% highlight bash %}
[sun@localhost ~]$ vi func1.sh
#!/bin/sh
myFunction() {
  echo "myFunction 함수"
  return
}
echo "프로그램 시작"
myFunction
echo "프로그램 종료"
exit 0
[sun@localhost ~]$ sh func1.sh
프로그램 시작
myFunction 함수
프로그램 종료
{% endhighlight %}

<br>
<br>

## 함수의 파라미터 사용

형식

{% highlight bash %}
함수이름() { # 함수를 정의
  $1, $2 ... 등을 사용
}
함수이름 파라미터1 파라미터2 ... # 함수 호출
{% endhighlight %}

{% highlight bash %}
[sun@localhost ~]$ vi func2.sh
#!/bin/sh
hap () {
    echo `expr $1 + $2`
}
echo "10 + 20 실행"
hap 10 20
exit 0
[sun@localhost ~]$ sh func2.sh
10 + 20 실행
30
{% endhighlight %}

<br>
<br>

## eval

- 문자열을 명령문으로 인식하고 실행

{% highlight bash %}
[sun@localhost ~]$ vi eval.sh
#!/bin/sh
str="ls -a"
echo $str
eval $str
exit 0
[sun@localhost ~]$ sh eval.sh
ls -a
.              .bash_profile  case1.sh  for2.sh   if2.sh   num1.sh    while2.sh
..             .bashrc        case2.sh  func1.sh  if3.sh   para1.sh   while3.sh
.bash_history  andor1.sh      eval.sh   func2.sh  if4.sh   var1.sh
.bash_logout   bce.sh         for1.sh   if1.sh    name.sh  while1.sh
{% endhighlight %}

<br>
<br>

## export

- 외부 변수로 선언

- 선언한 변수를 다른 프로그램에서도 사용할 수 있도록 해 줌

{% highlight bash %}
[sun@localhost ~]$ vi exp1.sh
#!/bin/sh
echo $var1
echo $var2
exit 0
[sun@localhost ~]$ vi exp2.sh
#!/bin/sh
var1="지역변수"
export var2="외부변수"
sh exp1.sh
exit 0
[sun@localhost ~]$ sh exp2.sh

외부변수
[sun@localhost ~]$ echo $var2

{% endhighlight %}

<br>
<br>

## printf

- C언어의 printf() 함수와 비슷하게 형식을 지정하여 출력

{% highlight bash %}
#!/bin/sh
var1=100.5
var2="재미있는건가 쉘스크립트"
printf "%5.2f \n\n \t %s \n" $var1 "$var2"
exit
[sun@localhost ~]$ sh printf.sh
100.50

         재미있는건가 쉘스크립트
{% endhighlight %}

<br>
<br>

## set과 $(명령어)

- 리눅스 명령어를 결과로 사용하기 위해서는 $(명령어) 형식을 사용

- 결과를 파라미터로 사용하고자 할 때는 set과 함께 사용

{% highlight bash %}
[sun@localhost ~]$ vi set.sh
#!/bin/sh
echo "오늘 날짜는 $(date) 입니다."
set $(date)
echo "오늘은 $4 요일 입니다."
exit 0
[sun@localhost ~]$ sh set.sh
오늘 날짜는 2019. 10. 15. (화) 13:21:20 KST 입니다.
오늘은 (화) 요일 입니다.
{% endhighlight %}

<br>
<br>

## shift

- 파라미터 변수를 왼쪽으로 한 단계씩 아래로 쉬프트 시킴

- 10개가 넘는 파라미터 변수에 접근할 때 사용

- 단 $0 파라미터 변수는 변경되지 않음

- 원하지 않는 결과의 소스

{% highlight bash %}
[sun@localhost ~]$ vi shift1.sh
#!/bin/sh
myfunc () {
  echo $1 $2 $3 $4 $5 $6 $7 $8 $9 $10 $11
}
myfunc AAA BBB CCC DDD EEE FFF GGG HHH III JJJ KKK
exit 0
[sun@localhost ~]$ sh shift1.sh
AAA BBB CCC DDD EEE FFF GGG HHH III AAA0 AAA1
{% endhighlight %}

- Shift 이용

{% highlight bash %}
[sun@localhost ~]$ vi shift2.sh
#!/bin/sh
myfunc() {
  str=""
  while [ "$1" != "" ] ; do
    str="$str $1"
    shift
  done
  echo $str
}
myfunc AAA BBB CCC DDD EEE FFF GGG HHH III JJJ KKK
exit 0
[sun@localhost ~]$ sh shift2.sh
AAA BBB CCC DDD EEE FFF GGG HHH III JJJ KKK
{% endhighlight %}

<br>
<br>

## 끝

매번 쉘 스크립트를 처음부터 만드는게 아니라, 기존에 사용했던 부분을 재활용해서 만들다보니

기초 문법을 점점 잊어갔었는데.. 되새김질 겸 정리
