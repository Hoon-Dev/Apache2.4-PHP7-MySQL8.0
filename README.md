# Apache2.4-PHP7-MySQL8.0

Window 10 환경에서 A.P.M(Apache2.4-PHP7-MySQL8.0)을 수작업으로 연결하는 과정을 안내합니다.

## Install
> [Apache2.4](https://www.apachelounge.com/download/)
```
Apache 2.4.46 Win64
[Apache VS16 Binary] httpd-2.4.46-win64-VS16.zip

02 Oct '20 10.323k
```

> [PHP7](https://windows.php.net/download/)

**IIS 서버**를 사용하는게 아니니 `Thread Safe`중에서 **운영체제 비트**에 맞는 버전을 설치

```
VC15 x64 Thread Safe (2020-Sep-29 16:55:18)

Zip [24.93MB]
sha256: 413f497294ceb5fd1192dc5689f08481d3302900cad7be7a85b7faf72bdc9434

Debug Pack [21.96MB]
sha256: b59be82ae29f6bfa93739a1d4937903b867d170ace2026f48a896432b73dd4d0

Development package (SDK to develop PHP extensions) [1.08MB]
sha256: c4f2028b411e1e6ace748eaea8fa7cdd01ce1eb7a8b9fac0a0b204d028b44ad8
```

> [MySQL8.0](https://dev.mysql.com/downloads/mysql/)

## Setting
> **Apache2.4 - (1)**

1. 설치한 Apache2.4의 압축을 해제 후 내부에 `Apache2.4 폴더`만 원하는 위치로 이동합니다.<br>
***(recommended directory path: C:\Apache2.4)***

2. 이동한 Apache2.4 내부에 `C:/Apache24/conf/httpd.conf`을 열어서 아래와 같이 수정합니다.

```
#ServerName www.example.com:80
- 위에 있는 텍스트를 찾아서 그 자리에

localhost
- 위 처럼 원하는 도메인으로 수정
```

> **PHP7**

1. 설치한 PHP7의 압축을 해제 후 파일 이름을 php로 변경한 후 원하는 위치로 이동합니다.<br>
***(recommended directory path: C:\php)***

2. 이동한 PHP7 내부에 `php.ini-production`을 `php.init`으로 변경하고 아래와 같이 수정합니다.

```
;extension=mysqli
;extension=oci8_12c  ; Use with Oracle Database 12c Instant Client
;extension=odbc
;extension=openssl
;extension=pdo_firebird
;extension=pdo_mysql
- 위와 같은 형식의 텍스트를 찾습니다.

extension=mysqli
extension=pdo_mysql
- 위에 있는 두개만 맨 앞에 세미콜론을 제거합니다.
```

> **Apache2.4 - (2)**

1. `C:/Apache24/conf/httpd.conf`을 열어서 맨밑으로 내려가 **아래의 코드**를 추가합니다.

```
AddHandler application/x-httpd-php .php
AddType application/x-httpd-php .php .html
LoadModule php7_module "C:/php/php7apache2_4.dll"
PHPiniDir "c:/php"
```

2. 계속해서 아래처럼 수정합니다.

```
DirectoryIndex index.html
- 위와 같은 형식의 텍스트를 찾습니다.

DirectoryIndex index.php index.html
- 위와 같이 index.php 를 추가합니다.
```

3. **명렁프롬프트**를 실행하여 아래의 커맨드로 `HTTP서버`를 설치합니다.<br>
***( Windows의 PC보호 창이 뜨는데 추가정보 버튼을 눌러서 계속진행 )***

```
cd C:\Apache24\bin
httpd -k install
```

> **MySQL8.0**

1. PHP의 **mysqli_connect 함수**가 지원하는 암호화 방식이랑 MySQL8.0의 암호화 방식이<br>
다르기 때문에 MySQL8.0의 계정 암호화 방식을 **아래의 커맨드**로 수정합니다.

```SQL
alter user '사용할 계정명' identified with mysql_native_password by '변경할 비밀번호';
```
