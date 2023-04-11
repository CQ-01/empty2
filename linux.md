# I. 리눅스 기본 명령어
## 1. 명령어 라인 구성하기
### command(명령)
- 리눅스를 사용하기 위해 사용자가 입력하는 다양한 명령
### option(옵션)
- 옵션으로 명령어 세부기능 선택
- short 형태(-)와 long 형태(--) 존재
### argument(인자)
- 명령어에 전달되는 값
- 파일명이나 디렉토리명, 명령어 실행에 필요한 정보 등
```bash
$ date
2023. 04. 11. (화) 17:36:15 KST

$ echo Hello world
Hello world

$ ls -F
Desktop/ Downloads/ lab/ Pictures/ Templates/ Videos/ Documents/ ...
```
- 옵션의 경우 여러가지 기능을 함께 적용 가능
```bash
$ ls -F
test.sh*

$ ls -a
.  .bash_history .bash_profile  .cache  .lesshst  .viminfo
.. .bash_logout  .bashrc        .config .mozilla  .test.sh

$ ls -aF
./  .bash_history  .bash_profile  .cache/  .lesshst  .viminfo
../ .bash_logout   .bashrc        .config  .mozilla/  test.sh*
```
## 2. 사용자 정보 확인_id
### 사용자의 UID와 GID 확인
- 현재 시스템에 로그인한 사용자 또는 인자로 지정한 사용자의 정보 확인
- 사용법 : id [사용자 계정]
- 옵션
  - g : 기본 그룹의 GID만 출력
  - G : 기본 그룹과 추가 그룹의 GID들을 출력
```bash
$ id
uid = 1000(linux) gid=1000(linux) groups=1000(linux),4(adam),24(cdrom),27(sudo),30(dip),46(plugdev),113(lpadmin)
```
## 3. 로그인 세션_who
### 현재 로그인 한 모든 사용자 정보 출력(who)
- 현재 접속되어 있는 사용자계정의 이름, 접속회선, 접속시간, 접속위치등 확인
- 옵션
  - q : 로그인한 계정의 이름과 수를 출력
  - H : 출력 정보의 헤더를 표시
```bash
$ who
linux  tty7  2023-04-11 18:06 (:0)
linux  pts/3 2023-04-11 18:06 (192. 168. 1. 88)
```

## 4. 사용자 암호 변경하기_passwd
### 사용자 패스워드 지정(pw)
- 생성된 계정은 패스워드 설정이 필요함
- 사용법
  - passwd [옵션] [사용자ID]
```bash
$ passwd
Changing password for linux.
(current) UNIX password:
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

## 5. 시스템 정보 수집하기
### 현재 로그인 된 사용자 정보
```bash
$ users
$ who
$ finger
$ last
```
### 운영체제 버전과 하드웨어 정보
```bash
$ cat /etc/os-release
$ uname -a
$ free                 ($ less /proc/meminfo: 메모리 정보확인)
$ less /proc/cpuinfo   (processor 정보 확인)
$ lsscsi               ($ cat /proc/scsi/scsi: disk 정보 확인)
```
### 네트워크 정보
```bash
$ uname -a             ($ hostname, $ cat /etc/hostsname)
$ ip address           ($ ifconfig)
```

## 6. 사용자 전환
### 사용자 전환(Switch User: su)
- 다른 계정 전환을 위해 로그아웃 $\rightarrow$ 로그인 과정이 필요
- 일시적 다른 계정 전환 시 su 명령어 사용
```bash
$ su - root
Password:
# id
uid=0(root) gid=0(root) groups=0(root)
```
### 다른 사용자의 권한을 명령어 실행(Switch User DO: sudo)
- 명령어를 다른 사용자 권한으로 실행
```bash
$ sudo useradd hpedu
[sudo] password for linux:
```
# II. 파일 시스템 관리
## 1. 디렉토리 관리
### 디렉토리
- 윈도우의 폴더
- 하나 이상의 디렉토리나 파일을 저장할 수 있는 공간
- 아래에 계층적으로 하위 디렉토리를 가짐
- 트리 형태의 자료구조, 최상위를 /로 쓰고 root로 사용
- 윈도우 : 저장장치마다 개별 트리 구조, 리눅스 : 여러개의 저장장치가 연결되어 있어도 단일 트리구조
- 파일시스템 계층표준(FHS) : 시스템정보나 어플리케이션 정보가 어디쯤 있을지 예상 가능

### 리눅스 파일시스템 계층 표준 (FHS)
|디렉토리|주요기능|
|--|--|
|/|루트 디렉토리, 파일시스템의 시작점|
|/boot|리눅스 커널, 시작 RAM 디스크 이미지, 커널 로더 설정 파일이 위치|
|/dev|커널이 인식하고 있는 모든 디바이스 파일이 위치|
|/etc|시스템 전반의 환경설정 파일|
|/home|각 사용자 계정의 홈 디렉토리 생성, 일반사용자는 홈 dir내에서만 파일 편집|
|/opt|추가적인 소프트웨어의 기본 설치경로|
|/tmp|시스템이나 어플리케이션 운영에 필요한 임시 파일들이 생성|
|/usr|기본적인 시스템 명령어의 실행파일과 공용 라이브러리, manual 페이지 등|
|/var|상대적으로 자주 변경되는 데이터들, 주로 로그/사용자 메일박스/스풀링 정보|
|/proc, /sys|실시간으로 사용되는 리눅스 커널의 정보 확인, proc은 가상 시스템|

### 디렉토리 확인(pwd: print working directory)
- 자신이 현재 어느 디렉토리에 위치해 있는지 확인(현재 작업 디렉토리 확인)
```bash
$ pwd
/home/centos

$ pwd
/etc/sysconfig
```
### 경로(path)
- 사용자가 원하는 디렉토리나 파일의 트리 형태 논리적인 자료구조 안에서의 위치
- 특정 파일이나 디렉토리를 사용자나 어플리케이션이 식별하기 위한 정보
### 상대경로와 절대경로
- 절대경로(Absolute Path) : 파일의 위치를 최상위 노드(/)를 기준으로 정의한 경로
- 상대경로(Relative Path) : 현재 사용자의 위치를 기준으로 정의한 경로
### Special directory
- .(dot) 디렉토리 : 현재 디렉토리
- ..(dot dot) 디렉토리 : 현재 작업 디렉토리의 하나 상위 디렉토리
### 디렉토리 변경(cd)
- 현재 디렉토리에서 다른 디렉토리로 이동할 때 사용
- 사용법
  - cd .. : 한단계 상위 디렉토리 이동
  - cd / : 최상위 디렉토리로 이동
  - cd ~ : 홈 디렉토리로 이동, == cd
  - cd ~사용자계정 : 지정된 사용자 계정의 홈디렉토리로 이동
  - cd - : 작업디렉토리 이전 작업 디렉토리로 변경

### 디렉토리 생성(mkdir)
- 새로운 디렉토리 생성
- 동일한 계층에 존재하는 모든 디렉토리의 이름은 서로 다르게 생성
- 사용법 : mkdir [옵션] [디렉토리 이름]
- 옵션
  - p : 하나의 디렉토리와 그 하위 디렉토리를 동시에 생성
  - m : 디렉토리를 생성할 때 접근 권한을 지정
```bash
$ ls
$ mkdir class
$ ls -l
합계 0
drwxrwxr-x. 2 centos centos 6 7월 22 12:22 class

$ mkdir class/linux
$ mkdir class/linux/fundamentals
```

### 디렉토리 삭제
- 명령어로 삭제할 경우 복원불가
- 삭제하고자 하는 디렉토리는 반드시 비어있어야 함
- 현재 위치하고 있는 디렉토리는 삭제 불가
```bash
$ rm remove_file
$ rmdir class
rmdir : failed to remove 'class' : 디렉토리가 비어있지 않음
$ rm -r : 서브디렉토리나 파일을 포함하는 디렉토리 삭제

$ ls -R class
class:
linux

class/linux:
fundamentals

class/linux/fundamentals:
$ rmdir class/linux/fundamentals/ class/linux class
```

## 2. 파일관리
### 리눅스 파일의 특징
- 일반적으로 확장자를 갖지 않음
- 대소문자를 구분
- 엄격한 소유/허가 권한을 가짐

![리눅스파일](/img/img01.png)

### 파일내용 출력(cat)
- 텍스트 파일의 내용을 표준 출력 장치로 출력
- 파일 전체를 한꺼번에 출력, 긴 파일 출력 시 전체 내용 확인 어려움
- 사용법 : cat [옵션] 텍스트 파일명
- 옵션
  - b : 파일을 구성하는 각 문장에 1부터 시작하는 번호 출력
  - E : 각 문장의 끝에 $기호를 추가해 문장의 끝을 확인
  - n : 공백 문장을 포함하여 파일의 각 문장에 1부터 시작하는 번호 출력
  - s : 하나 이상의 연속된 공백 문장이 존재할 경우 하나의 공백 문장만 출력

### 파일 내용 출력(less)
- 긴 텍스트 파일을 나누어서 출력하는 기능
- 기존의 more보다 페이지 전환이나 이동이 편리
- 사용법 : less [옵션] 텍스트 파일명

### 파일 내용 출력(head)
- 파일 내용 중 앞부분의 일부 문장을 출력
- 출력할 문장의 수를 지정하지 않으면 기본 10줄 출력
- 사용법 : head [옵션] 텍스트 파일명
- 옵션
  - c단위 : 출력할 용량을 지정
  - n : 처음부터 n번째 문장까지만 출력

### 파일 내용 출력(tail)
- 파일의 내용 중 뒷부분의 일부 문장을 출력
- 출력할 문장의 수를 지정하지 않으면 기본 10줄 출력
- 사용법 : tail [옵션] 텍스트 파일명
- 옵션
  - c단위 : 출력할 용량을 지정
  - n : 처음부터 n번째 문장까짐나 출력
  - f : 파일이 오픈 된 세션을 열어놓은 채로 실시간으로 파일에 추가되는 내용을 확인, 주로 로그 등

### 파일의 시간 정보 갱신(touch)
- 파일의 날짜 시간 정보를 변경하는 명령어
- 아무 옵션 없이 사용되면 서버의 현재 시간으로 변경
- 존재하지 않는 파일명을 인수로 주면 사이즈가 0인 빈 파일을 생성
```bash
$ ||
-rwxrwxr-x. 1 centos centos 33 12월 3 16:14 test.sh

$ touch -t 12131500 test.sh
$ || test.sh
-rwxrwxr-x. 1 centos centos 33 12월 13 15:00 test.sh

$ touch emptyfile
$ ||
-rw-rw-r--. 1 centos centos 0 12월 18 21:37 emptyfile
-rwxrwxr-x. 1 centos centos 33 12월 13 15:00 test.sh
```

### 파일의 복사(cp)
- 파일이나 디렉토리를 복사
- 사용법 : cp [옵션]  source [...]  target
- 옵션
  - a : 파일의 모든 속성을 함께 복사
  - i : 복사하고자 하는 파일이 존재할 경우 사용자에게 복사 여부를 확인
  - r : 디렉토리 하위의 내용을 모두 복사
```bash
$ cp /etc/passwd /tmp/passwd.old
$ ls -l /tmp/passwd.old
-rw-r--r--. 1 root root 2192  7월 22 12:37 /tmp/passwd.old

$ mkdir backup
$ cp /etc/passwd /etc/group backup
$ ls backup
group passwd

$ cp backup backup-1
cp: omitting directory 'backup'

$ cp -r backup backup-1

$ || -d backup*
drwxr-xr-x. 2 root root 33 7월 22 12:38 backup
drwxr-xr.x. 2 root root 33 7월 22 12:38 backup-1
```

### 파일의 이동 및 이름 변경(mv)
- 파일이나 디렉토리의 위치를 이동하거나 현재 이름을 다른 이름으로 변경
- 사용법 : mv [옵션] source [...] target
- 옵션
  - b : 백업파일을 생성한 후 이동
  - f  : 이동하고자 하는 파일이 존재할 경우 기존 파일을 삭제하고 이동
  - i : 이동하고자 하는 파일이 존재할 경우 사용자에게 이동 여부를 확인
  - v : 파일의 이동 과정을 출력
```bash
$ cp /etc/passwd.
$ ls
anaconda-ks.cfg backup-1  passwd backup  initial-setup-ks.cfg shell

$ mv passwd passwd.old
$ ls
anaconda-ks.cfg backup-1  passwd.old backup  initial-setup-ks.cfg shell
$ mv passwd.old backup
$ ls backup
group passwd passwd.old
```

### 파일의 삭제
- 파일이나 디렉토리를 삭제
- 한번 삭제된 파일이나 디렉토리는 복구 불가능
- 사용법 : rm [옵션] 파일명/디렉토리명
- 옵션
  - f : 파일을 강제적으로 삭제하며, 삭제 시 확인 절차 없음(force)
  - i : 파일을 삭제하기 전 사용자에게 삭제 여부를 확인
  - r : 디렉토리인 경우 디렉토리 하위의 모든 내용을 제거
  - v : 파일이 삭제되는 과정을 출력
```bash
$ rm passwd group
rm: remove 일반 파일 'passwd'? y
rm: remove 일반 파일 'group'? y

$ rm -rf backup-1
$ ls
anaconda-ks.cfg  initial-setup-ks.cfg  backup  shell
```

### DF(disk free)
- 파일 시스템 단위로 사이즈와 사용량 정보를 확인
- 옵션
  - T : 파일 시스템의 종류를 함께 출력
  - h : 용량을 GB단위나 MB단위로 출력
```bash
$ df -hT
Filesystem                  Type    Size    Used    Avail   Use%    Mounted on
udev                        devtmpfs 971M   0       971M    0%      /dev
tmpfs                       tmpfs   200M    3.9M    196M    2%      /run
/dev/mapper/ubuntu--vg-root ext4    25G     4.7G    20G     20%     /
tmpfs                       tmpfs   997M    252K    997M    1%      /dev/shm
tmpfs                       tmpfs   5.0M    4.0K    5.0M    1%      /run/lock
tmpfs                       tmpfs   997M    0       997M    0%      /sys/fs/cgroup
/dev/sda1                   ext2    472M    124M    324M    28%     /boot
tmpfs                       tmpfs   200M    80K     200M    1%      /run/user/1000'

```
### 소유권
- 특정 파일이나 디렉토리가 누구의 소유인가를 나타냄

### 허가권
- 특정 파일이나 디렉토리가 누구에게 어떤 연산이 허락되었는가를 의미
![연산허가](/img/img03.png)

### 파일 소유자 변경(chown)
- 파일이나 디렉토리의 소유자를 변경
- 시스템 관리자에게만 소유자 정보 변경 가능
- 사용법 : chown [옵션] 소유자명 파일/디렉토리 이름
- 옵션
  - R : 디렉토리에 사용되며, 지정한 디렉토리와 하위의 모든 디렉토리, 파일의 소유자를 동시에 변경
```bash
$ || testfile
-rw-r--r--. 1 root root 2192  7월 22 12:45 testfile
$ chown centos testfile
$ || testfile
-rw-r--r--. 1 centos root 2192  7월 22 12:45 testfile

$ chown -R centos backup
$ || backup
합계 12
-rw-r--r--. 1 centos root 950  7월 22 12:38 group
-rw-r--r--. 1 centos root 2192 7월 22 12:38 passwd
-rw-r--r--. 1 centos root 2192 7월 22 12:39 passwd.old
```

### 파일 그룹 변경(chgrp)
- 파일이나 디렉토리의 소유 그룹 정보를 변경
- 시스템 관리자에게만 소유 그룹 변경 가능
- 사용법 : chgrp [옵션] 그룹명 파일/디렉토리 이름
- 옵션
  - R : 디렉토리에 사용되며, 지정한 디렉토리와 그 하위 모든 dir 파일의 그룹을 동시 변경
```bash
$ || testfile
-rw-r--r--. 1 centos root 2192  7월 22 12:45 testfile

$ chgrp centos testfile
$ || testfile
-rw-r--r--. 1 centos centos 2192  7월 22 12:45 testfile

$ chown root:root testfile
$ || testfile
-rw-r--r--. 1 root root 2192  7월 22 12:45 testfile
```

### 파일의 허가권의 개요
- 권한의 종류 및 내용

|허가권|대상|내용|
|--|--|--|
|읽기|파일|파일을 읽을 수 있는 권한|
|읽기|디렉토리|디렉토리에 존재하는 파일 목록을 읽을 수 있는 권한|
|쓰기|파일|파일을 수정할 수 있는 권한|
|쓰기|디렉토리|디렉토리를 수정할 수 있는 권한|
|실행|파일|파일을 명령어처럼 실행 할 수 있는 권한|
|실행|디렉토리|디렉토리로 들어갈 수 있는 권한|

### 파일 허가권 변경(chmod)
- 기호 모드를 이용한 지정법
- 사용법 : chmod [ugoa] [+|-|=] [rwx] 파일명(디렉토리명)
- 옵션
  - u : 파일의 소유자(user)
  - g : 파일의 소유 그룹(group)
  - o : 소유자도 그룹도 아닌 기타 사용자(other)
  - a : 모든 사용자(user + group + other)
  - r : 읽기 권한(read)
  - w : 쓰기 권한(write)
  - x : 실행 권한(execute)
```bash
$ || a.out
-rwxrwxr-x. 1 centos centos 63024  7월 22 12:27 a.out

$ chmod g-w,o-x a.out

$ || a.out
-rwxr-xr--. 1 centos centos 63024  7월 22 12:27 a.out
```
- 8진수 허가모드를 이용한 지정 방법
![허가모드](/img/img02.png)
```bash
$ || a.out
-rwxr-xr--. 1 centos centos 63024  7월 22 12:27 a.out
$ chmod 644 a.out
$ || a.out
-rw-r--r--. 1 centos centos 60324  7월 22 12:27 a.out
```
# III. 쉘 환경
# IV. 리눅스 파일 편집
## Vi 편집기
### Vi 편집기의 시작과 종료
- <esc> : q(quit), 파일을 저장하지 않고 종료
- <esc> : w(write), 파일을 저장하고 종료 안함
- <esc> : wq(write quit), 일을 저장하고 종료
# V. 패키지 관리