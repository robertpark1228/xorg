---
description: 서버 운용시 필요한 내용들을 전반적으로 기술해 놓았습니다.
---

# 기타 : 서버 운용

## **L**inux Archive Tools (tar, star, gzip, bzip2, zip, cpio)

This article discusses the archiving tools available in Linux, with specific reference to the information needed for the [RHCSA EX200](http://www.redhat.com/training/courses/ex200/examobjective) and [RHCE EX300](http://www.redhat.com/training/courses/ex300/examobjective) certification exams.

Remember, the exams are hands-on, so it doesn't matter which method you use to achieve the result, so long as the end product is correct.

* [tar](https://oracle-base.com/articles/linux/linux-archive-tools#tar)
* [star](https://oracle-base.com/articles/linux/linux-archive-tools#star)
* [gzip](https://oracle-base.com/articles/linux/linux-archive-tools#gzip)
* [bzip2](https://oracle-base.com/articles/linux/linux-archive-tools#bzip2)
* [zip](https://oracle-base.com/articles/linux/linux-archive-tools#zip)
* [cpio](https://oracle-base.com/articles/linux/linux-archive-tools#cpio)

All the commands in this article have many options in addition to the basic ones being used here. Please check the `man` pages for each command. The examples will use the following files.

```
mkdir -p /tmp/test-dir/subdir1
mkdir -p /tmp/test-dir/subdir2
mkdir -p /tmp/test-dir/subdir3
mkdir -p /tmp/extract-dir
touch /tmp/test-dir/subdir1/file1.txt
touch /tmp/test-dir/subdir1/file2.txt
touch /tmp/test-dir/subdir2/file3.txt
touch /tmp/test-dir/subdir2/file4.txt
touch /tmp/test-dir/subdir3/file5.txt
touch /tmp/test-dir/subdir3/file6.txt
```

Extracts assume the "/tmp/extract-dir" directory is empty.

### tar

Create an archive.

```
# cd /tmp
# tar -cvf archive1.tar test-dir
```

Check the contents.

```
# tar -tvf /tmp/archive1.tar
```

Extract it.

```
# cd /tmp/extract-dir
# tar -xvf /tmp/archive1.tar
```

### star

The `star` command may not be installed by default, but you can install it with the following command.

```
# yum install star
```

Create an archive.

```
# cd /tmp
# star -cv f=archive2.star test-dir
```

Check the contents.

```
# star -tv f=/tmp/archive2.star
```

Extract it.

```
# cd /tmp/extract-dir
# star -xv f=/tmp/archive2.star
```

### gzip

The `gzip` command compresses the specified files, giving them a ".gz" extension. In this case we will use it to compress a ".tar" file.

```
# cd /tmp
# tar -cvf archive3.tar test-dir
# gzip archive3.tar
```

The "-z" option of the `tar` command allows you to do this directly.

```
# cd /tmp
# tar -cvzf archive3.tar.gz test-dir
```

The files are uncompressed using the `gunzip` command.

```
# gunzip archive3.tar.gz
```

The "-z" option of the `tar` command allows you to directly `ungzip` and extract a ".tar.gz" file.

```
# cd /tmp/extract-dir
# tar -xvzf /tmp/archive3.tar.gz
```

### bzip2

The `bzip2` command is similar to the `gzip` command. It compresses the specified files, giving them a ".bz2" extension. In this case we will use it to compress a ".tar" file.

```
# cd /tmp
# tar -cvf archive4.tar test-dir
# bzip2 archive4.tar
```

The "-j" option of the `tar` command allows you to do this directly.

```
# cd /tmp
# tar -cvjf archive4.tar.bz2 test-dir
```

The files are uncompressed using the `bunzip2` command.

```
# bunzip2 archive4.tar.bz2
```

The "-j" option of the `tar` command allows you to directly `bunzip2` and extract a ".tar.bz2" file.

```
# cd /tmp/extract-dir
# tar -xvjf /tmp/archive4.tar.bz2
```

### zip

Create an archive.

```
# cd /tmp
# zip -r archive5.zip test-dir
```

Check the contents.

```
# unzip -l archive5.zip
```

Extract it.

```
# cd /tmp/extract-dir
# unzip /tmp/archive5.zip
```

### cpio

Create an archive.

```
# cd /tmp
# find test-dir | cpio -ov > archive6.cpio
```

Check the contents.

```
# cpio -t < /tmp/archive6.cpio
```

Extract it.

```
# cd /tmp/extract-dir
# cpio -idmv < /tmp/archive6.cpio
```

### ****

### **/etc/passwd(사용자 정보)**

cat /etc/passwd 를 실행하면 정보가 있으며 이에 대한 설명 입니다.

| **필드**      | **설명**                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **사용자 계정명** | 맨 앞에 필드는 사용자의 계정명을 나타냅니다                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **패스워드**    | 그 다음의 필드는 패스워드 필드인데, x가 의미하는 바는 사용자의 패스워드가 /etc/shadow에 암호화되어 저장되어있다는 뜻입니다.                                                                                                                                                                                                                                                                                                                                                                                                         |
| **UID**     | 사용자의 user id를 나타냅니다. 관리자 계정(Root)은 UID가 0입니다.                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| **GID**     | 사용자의 그룹 ID를 나타냅니다. 관리자 그룹(Root)의 GID는 0입니다.                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **comment** | 사용자와 관련한 기타 정보로 일반적으로 사용자의 이름을 나타냅니다.                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| **홈 디렉토리**  | 사용자의 홈디렉토리를 의미합니다. 관리자 계정의 홈 디렉토리는 /root이며, 다른 사용자의 홈 디렉토리는 기본으로 /home/ 하위에 계정명으로 위치합니다.                                                                                                                                                                                                                                                                                                                                                                                            |
| **로그인 쉘**   | <p>사용자가 로그인시에 사용할 쉘을 의미합니다. 보통 사용자의 쉘은 성능이 우수한 bash쉘을 사용합니다. 로그인이 불필요한 계정도 있는데요. 이때는 이 필드가 /usr/sbin/nologin, /bin/false, /sbin/nologin 등으로 표기됩니다. 이것은 사용자가 아니라 어플리케이션이기 때문이죠. 아래처럼 말이죠.<br><br>sshd:x:126:65534::/run/sshd:<strong>/usr/sbin/nologin</strong><br><strong></strong>gnome-initial-setup:x:124:65534::/run/gnome-initial-setup/:<strong>/bin/false</strong><br>gdm:x:125:130:Gnome Display Manager:/var/lib/gdm3:<strong>/bin/false</strong><br><strong></strong></p> |



### **/etc/shadow**

/etc/shadow에는 암호화된 패스워드와 패스워드 설정 정책이 기재되어 있습니다. 여기서 관리자 계정과 관리자 그룹만이 이 파일을 읽을 수 있습니다. 이 파일이 만약 평문의 비밀번호의 정보를 가지고 있다면 모든 사용자의 비밀번호가 유출될 수 있습니다.\
****

| **필드**           | **설명**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **사용자 계정명**      | 맨 첫 필드는 사용자의 계정명을 뜻합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **암호화된 패스워드**    | <p>두번째 필드는 암호화된 패스워드를 뜻합니다. 이 패스워드는 또 $로 필드가 구분되어 있는데요. 아래처럼 구분이 됩니다.<br><br><strong>$algorithm_id$salt$encrypted_password</strong><br><br><strong>algorithm_id</strong> : 암호학적 해시의 id를 의미하는데요. id는 아래와 같습니다.<br>1 : MD5(가장 취약한 일방향 해쉬로 요즘에는 쓰지 않습니다.<br>2 : BlowFish<br>5 : SHA-256<br>6 : SHA-512<br>제 시스템은 SHA-512를 사용하는 것을 알 수 있습니다.<br><br><strong>salt</strong> : 각 해쉬에 첨가할 랜덤값입니다. 이 랜덤값에 따라서 해시의 값이 바뀌게 됩니다.<br><strong>encrypted_password</strong> : 마지막은 알고리즘과 salt로 패스워드를 암호화한 값입니다.<br><br>이 뿐만 아니라 패스워드 필드에 *, !!, 또는 빈값이 설정될 수 있습니다. 이것이 의미하는 바를 알아보겠습니다.<br><strong>*</strong> : 패스워드가 잠긴 상태입니다. 로그인은 불가합니다. 별도의 인증방식을 사용하여 로그인을 할 수는 있습니다.<br><strong>!</strong> : 패스워드가 잠긴 상태이고 로그인을 할 수 없습니다. 또는 사용자를 생성하고 패스워드를 설정하지 않은 상태이기도 합니다.<br><img src="https://blog.kakaocdn.net/dn/mi4Ij/btqZyws1pM3/WKqBKiAE5zcgfyt4hnYmc1/img.png" alt=""></p><p><strong>empty</strong> : 패스워드가 설정되지 않았지만 로그인이 가능합니다. </p> |
| **마지막 변경**       | 마지막으로 패스워드를 변경한 날을 1970년 1월 1일 기준으로 일수도 표시합니다. 여기서 마지막으로 패스워드를 변경한 날은 1970년 1월 1일 이후 18693일이 지났음을 알 수 있습니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| **패스워드 최소 사용기간** | 패스워드의 최소 사용기간 설정으로 패스워드를 변경한 이후 최소 이 정도의 기간은 써야한다는 것을 의미합니다. 그래서 마지막 변경이 일어난 후 이 일수가 지나기 전에 암호를 변경할 수 없습니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| **패스워드 최대 사용기간** | 패스워드의 최대 사용기간 설정으로 마지막 패스워드 변경 이후 만료일수를 의미합니다. 패스워드를 변경하지 않으면 공격자가 패스워드를 깰 수도 있으므로 90일을 권장합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **경고**           | 패스워드 만료 이전에 경고할 경고 일수를 의미합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **비활성화**         | 패스워드가 만료된 이후에 계정이 잠기기 전까지 비활성 일수(date)입니다. 해당 비활성 기간동안에 패스워드를 변경해야 계정이 잠기지 않습니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **만료일**          | 계정 만료일 필드입니다. 1970년 1월 1일 기준으로 일수로 표시합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

## **심볼릭 링크 / 하드 링크 설명**

### **inode의 개념 간략 : 파일들의 주소**

파일들의 정보는 바로 이 inode에 담겨있는데, 그만큼 파일에 있어서는 중요한 자료구조입니다. 파일은 각자의 inode를 가지고 있으며 inode 번호로 inode를 구분할 수 있습니다.\
\
일반적으로 툴을 돌리고 설정할때 Inode 에러가 나오기 시작하면 하드가 깨져가는것이니 빠른 백업이 필요 합니다.

### **심볼릭 링크(Symbolic Link)**

심볼링 링크는 윈도우즈의 바로가기와 같은 개념입니다. 즉, 원본 파일을 다른 경로나 이름을 써서 파일을 열 수 있는 개념이죠. 실습을 통해서 알아봅시다. 우선 빈 파일 origin을 touch명령어를 이용해서 만들어보세요.

![](https://blog.kakaocdn.net/dn/lJhmi/btqZ7KiGJnA/3kxpcPTmTwfVAKJZgsuDI0/img.png)

그리고 이 파일에 심볼릭 링크를 걸어보도록 하겠습니다.  명령은 아래와 같이 ln -s를 사용합니다.

**ln -s \[원본 파일] \[심볼링 링크 파일]**

링크 파일을 통해서 파일에 데이터를 기록하면 원본 파일도 같이 수정되는 것을 알 수 있습니다. 아래의 결과가 그것을 보여주지요.

![](https://blog.kakaocdn.net/dn/cBGRSz/btqZ0tCZ4D1/FKp8mJvV0ooC9D1oO42uk1/img.png)

만약 원본 파일을 삭제하면 어떻게 될까요? 링크 파일은 그 원본이 사라졌으니 파일을 찾을 수 없습니다. 링크된 파일은 원본의 파일에 의존적임을 알 수 있습니다.

![](https://blog.kakaocdn.net/dn/vZY2f/btqZ2bn4ccD/uoF04SKIL6aLGZNVITKK0k/img.png)

### **하드 링크(Hard Link)**

심볼릭 링크가 원본 파일에 의존적이었다고 한다면 하드링크는 원본 파일이 삭제되어도 링크된 파일은 여전히 그 정보를 갖고 있습니다. 그리고 링크 파일로 원본 파일을 변경할 수 있습니다. **여기서 하드링크와 심볼릭 링크와의 중요한 차이점은 같은 inode를 바라보느냐의 차이입니다.** 하드링크는 원본 파일과 같은 inode 블록을 가리키고 있기 때문에 원본 파일이 삭제되어도 링크된 파일은 여전히 파일의 내용을 보존할 수 있습니다. 하드 링크가 될때 inode에서 링크 수가 하나 늘어납니다.

![](https://blog.kakaocdn.net/dn/cU46Dy/btqZ3fKwY5W/ZeLGEKZQDGhLJLuuT7Iv50/img.png)

실제로 그런지 확인해보기 위해서 아래처럼 하드 링크된 파일에 내용을 기록하고 원본 파일을 열어보면 원본 파일이 하드 링크된 파일의 내용과 같게 변경이 되지요. 그리고 origin을 삭제해보고 링크 파일을 보면 여전히 그 내용을 볼 수 있습니다. 

![](https://blog.kakaocdn.net/dn/dE9dMo/btqZ0sRBHLu/KmlxSkLu2xnkT6Xz2yJvk1/img.png)

이때까지 설명한 내용은 아래의 그림으로 요약을 해봤습니다. 이해가 되셨길 바랍니다. 

![](https://blog.kakaocdn.net/dn/c9MA8Z/btqZ1JZDcZd/Jr5GXpP8luPTvPDuoRdMvK/img.png)





