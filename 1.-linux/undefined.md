---
description: 서버 운용시 필요한 내용들을 전반적으로 기술해 놓았습니다.
---

# 기타 : 서버 운용

### **/etc/passwd\(사용자 정보\)**

cat /etc/passwd 를 실행하면 정보가 있으며 이에 대한 설명 입니다.

| **필드** | **설명** |
| :--- | :--- |
| **사용자 계정명** | 맨 앞에 필드는 사용자의 계정명을 나타냅니다 |
| **패스워드** | 그 다음의 필드는 패스워드 필드인데, x가 의미하는 바는 사용자의 패스워드가 /etc/shadow에 암호화되어 저장되어있다는 뜻입니다. |
| **UID** | 사용자의 user id를 나타냅니다. 관리자 계정\(Root\)은 UID가 0입니다. |
| **GID** | 사용자의 그룹 ID를 나타냅니다. 관리자 그룹\(Root\)의 GID는 0입니다. |
| **comment** | 사용자와 관련한 기타 정보로 일반적으로 사용자의 이름을 나타냅니다. |
| **홈 디렉토리** | 사용자의 홈디렉토리를 의미합니다. 관리자 계정의 홈 디렉토리는 /root이며, 다른 사용자의 홈 디렉토리는 기본으로 /home/ 하위에 계정명으로 위치합니다. |
| **로그인 쉘** | 사용자가 로그인시에 사용할 쉘을 의미합니다. 보통 사용자의 쉘은 성능이 우수한 bash쉘을 사용합니다. 로그인이 불필요한 계정도 있는데요. 이때는 이 필드가 /usr/sbin/nologin, /bin/false, /sbin/nologin 등으로 표기됩니다. 이것은 사용자가 아니라 어플리케이션이기 때문이죠. 아래처럼 말이죠.  sshd:x:126:65534::/run/sshd:**/usr/sbin/nologin** gnome-initial-setup:x:124:65534::/run/gnome-initial-setup/:**/bin/false** gdm:x:125:130:Gnome Display Manager:/var/lib/gdm3:**/bin/false**  |



### **/etc/shadow**

/etc/shadow에는 암호화된 패스워드와 패스워드 설정 정책이 기재되어 있습니다. 여기서 관리자 계정과 관리자 그룹만이 이 파일을 읽을 수 있습니다. 이 파일이 만약 평문의 비밀번호의 정보를 가지고 있다면 모든 사용자의 비밀번호가 유출될 수 있습니다.  
****

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#xD544;&#xB4DC;</b>
      </th>
      <th style="text-align:left"><b>&#xC124;&#xBA85;</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>&#xC0AC;&#xC6A9;&#xC790; &#xACC4;&#xC815;&#xBA85;</b>
      </td>
      <td style="text-align:left">&#xB9E8; &#xCCAB; &#xD544;&#xB4DC;&#xB294; &#xC0AC;&#xC6A9;&#xC790;&#xC758;
        &#xACC4;&#xC815;&#xBA85;&#xC744; &#xB73B;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#xC554;&#xD638;&#xD654;&#xB41C; &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;</b>
      </td>
      <td style="text-align:left">
        <p>&#xB450;&#xBC88;&#xC9F8; &#xD544;&#xB4DC;&#xB294; &#xC554;&#xD638;&#xD654;&#xB41C;
          &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xB97C; &#xB73B;&#xD569;&#xB2C8;&#xB2E4;.
          &#xC774; &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xB294; &#xB610; $&#xB85C; &#xD544;&#xB4DC;&#xAC00;
          &#xAD6C;&#xBD84;&#xB418;&#xC5B4; &#xC788;&#xB294;&#xB370;&#xC694;. &#xC544;&#xB798;&#xCC98;&#xB7FC;
          &#xAD6C;&#xBD84;&#xC774; &#xB429;&#xB2C8;&#xB2E4;.
          <br />
          <br /><b>$algorithm_id$salt$encrypted_password</b>
          <br />
          <br /><b>algorithm_id</b> : &#xC554;&#xD638;&#xD559;&#xC801; &#xD574;&#xC2DC;&#xC758;
          id&#xB97C; &#xC758;&#xBBF8;&#xD558;&#xB294;&#xB370;&#xC694;. id&#xB294;
          &#xC544;&#xB798;&#xC640; &#xAC19;&#xC2B5;&#xB2C8;&#xB2E4;.
          <br />1 : MD5(&#xAC00;&#xC7A5; &#xCDE8;&#xC57D;&#xD55C; &#xC77C;&#xBC29;&#xD5A5;
          &#xD574;&#xC26C;&#xB85C; &#xC694;&#xC998;&#xC5D0;&#xB294; &#xC4F0;&#xC9C0;
          &#xC54A;&#xC2B5;&#xB2C8;&#xB2E4;.
          <br />2 : BlowFish
          <br />5 : SHA-256
          <br />6 : SHA-512
          <br />&#xC81C; &#xC2DC;&#xC2A4;&#xD15C;&#xC740; SHA-512&#xB97C; &#xC0AC;&#xC6A9;&#xD558;&#xB294;
          &#xAC83;&#xC744; &#xC54C; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.
          <br
          />
          <br /><b>salt</b> : &#xAC01; &#xD574;&#xC26C;&#xC5D0; &#xCCA8;&#xAC00;&#xD560;
          &#xB79C;&#xB364;&#xAC12;&#xC785;&#xB2C8;&#xB2E4;. &#xC774; &#xB79C;&#xB364;&#xAC12;&#xC5D0;
          &#xB530;&#xB77C;&#xC11C; &#xD574;&#xC2DC;&#xC758; &#xAC12;&#xC774; &#xBC14;&#xB00C;&#xAC8C;
          &#xB429;&#xB2C8;&#xB2E4;.
          <br /><b>encrypted_password</b> : &#xB9C8;&#xC9C0;&#xB9C9;&#xC740; &#xC54C;&#xACE0;&#xB9AC;&#xC998;&#xACFC;
          salt&#xB85C; &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xB97C; &#xC554;&#xD638;&#xD654;&#xD55C;
          &#xAC12;&#xC785;&#xB2C8;&#xB2E4;.
          <br />
          <br />&#xC774; &#xBFD0;&#xB9CC; &#xC544;&#xB2C8;&#xB77C; &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;
          &#xD544;&#xB4DC;&#xC5D0; *, !!, &#xB610;&#xB294; &#xBE48;&#xAC12;&#xC774;
          &#xC124;&#xC815;&#xB420; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;. &#xC774;&#xAC83;&#xC774;
          &#xC758;&#xBBF8;&#xD558;&#xB294; &#xBC14;&#xB97C; &#xC54C;&#xC544;&#xBCF4;&#xACA0;&#xC2B5;&#xB2C8;&#xB2E4;.
          <br
          /><b>*</b> : &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xAC00; &#xC7A0;&#xAE34; &#xC0C1;&#xD0DC;&#xC785;&#xB2C8;&#xB2E4;.
          &#xB85C;&#xADF8;&#xC778;&#xC740; &#xBD88;&#xAC00;&#xD569;&#xB2C8;&#xB2E4;.
          &#xBCC4;&#xB3C4;&#xC758; &#xC778;&#xC99D;&#xBC29;&#xC2DD;&#xC744; &#xC0AC;&#xC6A9;&#xD558;&#xC5EC;
          &#xB85C;&#xADF8;&#xC778;&#xC744; &#xD560; &#xC218;&#xB294; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.
          <br
          /><b>!</b> : &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xAC00; &#xC7A0;&#xAE34; &#xC0C1;&#xD0DC;&#xC774;&#xACE0;
          &#xB85C;&#xADF8;&#xC778;&#xC744; &#xD560; &#xC218; &#xC5C6;&#xC2B5;&#xB2C8;&#xB2E4;.
          &#xB610;&#xB294; &#xC0AC;&#xC6A9;&#xC790;&#xB97C; &#xC0DD;&#xC131;&#xD558;&#xACE0;
          &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xB97C; &#xC124;&#xC815;&#xD558;&#xC9C0;
          &#xC54A;&#xC740; &#xC0C1;&#xD0DC;&#xC774;&#xAE30;&#xB3C4; &#xD569;&#xB2C8;&#xB2E4;.
          <br
          />
          <img src="https://blog.kakaocdn.net/dn/mi4Ij/btqZyws1pM3/WKqBKiAE5zcgfyt4hnYmc1/img.png"
          alt/>
        </p>
        <p><b>empty</b> : &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xAC00; &#xC124;&#xC815;&#xB418;&#xC9C0;
          &#xC54A;&#xC558;&#xC9C0;&#xB9CC; &#xB85C;&#xADF8;&#xC778;&#xC774; &#xAC00;&#xB2A5;&#xD569;&#xB2C8;&#xB2E4;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#xB9C8;&#xC9C0;&#xB9C9; &#xBCC0;&#xACBD;</b>
      </td>
      <td style="text-align:left">&#xB9C8;&#xC9C0;&#xB9C9;&#xC73C;&#xB85C; &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xB97C;
        &#xBCC0;&#xACBD;&#xD55C; &#xB0A0;&#xC744; 1970&#xB144; 1&#xC6D4; 1&#xC77C;
        &#xAE30;&#xC900;&#xC73C;&#xB85C; &#xC77C;&#xC218;&#xB3C4; &#xD45C;&#xC2DC;&#xD569;&#xB2C8;&#xB2E4;.
        &#xC5EC;&#xAE30;&#xC11C; &#xB9C8;&#xC9C0;&#xB9C9;&#xC73C;&#xB85C; &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xB97C;
        &#xBCC0;&#xACBD;&#xD55C; &#xB0A0;&#xC740; 1970&#xB144; 1&#xC6D4; 1&#xC77C;
        &#xC774;&#xD6C4; 18693&#xC77C;&#xC774; &#xC9C0;&#xB0AC;&#xC74C;&#xC744;
        &#xC54C; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#xD328;&#xC2A4;&#xC6CC;&#xB4DC; &#xCD5C;&#xC18C; &#xC0AC;&#xC6A9;&#xAE30;&#xAC04;</b>
      </td>
      <td style="text-align:left">&#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xC758; &#xCD5C;&#xC18C; &#xC0AC;&#xC6A9;&#xAE30;&#xAC04;
        &#xC124;&#xC815;&#xC73C;&#xB85C; &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xB97C;
        &#xBCC0;&#xACBD;&#xD55C; &#xC774;&#xD6C4; &#xCD5C;&#xC18C; &#xC774; &#xC815;&#xB3C4;&#xC758;
        &#xAE30;&#xAC04;&#xC740; &#xC368;&#xC57C;&#xD55C;&#xB2E4;&#xB294; &#xAC83;&#xC744;
        &#xC758;&#xBBF8;&#xD569;&#xB2C8;&#xB2E4;. &#xADF8;&#xB798;&#xC11C; &#xB9C8;&#xC9C0;&#xB9C9;
        &#xBCC0;&#xACBD;&#xC774; &#xC77C;&#xC5B4;&#xB09C; &#xD6C4; &#xC774; &#xC77C;&#xC218;&#xAC00;
        &#xC9C0;&#xB098;&#xAE30; &#xC804;&#xC5D0; &#xC554;&#xD638;&#xB97C; &#xBCC0;&#xACBD;&#xD560;
        &#xC218; &#xC5C6;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#xD328;&#xC2A4;&#xC6CC;&#xB4DC; &#xCD5C;&#xB300; &#xC0AC;&#xC6A9;&#xAE30;&#xAC04;</b>
      </td>
      <td style="text-align:left">&#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xC758; &#xCD5C;&#xB300; &#xC0AC;&#xC6A9;&#xAE30;&#xAC04;
        &#xC124;&#xC815;&#xC73C;&#xB85C; &#xB9C8;&#xC9C0;&#xB9C9; &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;
        &#xBCC0;&#xACBD; &#xC774;&#xD6C4; &#xB9CC;&#xB8CC;&#xC77C;&#xC218;&#xB97C;
        &#xC758;&#xBBF8;&#xD569;&#xB2C8;&#xB2E4;. &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xB97C;
        &#xBCC0;&#xACBD;&#xD558;&#xC9C0; &#xC54A;&#xC73C;&#xBA74; &#xACF5;&#xACA9;&#xC790;&#xAC00;
        &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xB97C; &#xAE70; &#xC218;&#xB3C4; &#xC788;&#xC73C;&#xBBC0;&#xB85C;
        90&#xC77C;&#xC744; &#xAD8C;&#xC7A5;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#xACBD;&#xACE0;</b>
      </td>
      <td style="text-align:left">&#xD328;&#xC2A4;&#xC6CC;&#xB4DC; &#xB9CC;&#xB8CC; &#xC774;&#xC804;&#xC5D0;
        &#xACBD;&#xACE0;&#xD560; &#xACBD;&#xACE0; &#xC77C;&#xC218;&#xB97C; &#xC758;&#xBBF8;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#xBE44;&#xD65C;&#xC131;&#xD654;</b>
      </td>
      <td style="text-align:left">&#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xAC00; &#xB9CC;&#xB8CC;&#xB41C; &#xC774;&#xD6C4;&#xC5D0;
        &#xACC4;&#xC815;&#xC774; &#xC7A0;&#xAE30;&#xAE30; &#xC804;&#xAE4C;&#xC9C0;
        &#xBE44;&#xD65C;&#xC131; &#xC77C;&#xC218;(date)&#xC785;&#xB2C8;&#xB2E4;.
        &#xD574;&#xB2F9; &#xBE44;&#xD65C;&#xC131; &#xAE30;&#xAC04;&#xB3D9;&#xC548;&#xC5D0;
        &#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xB97C; &#xBCC0;&#xACBD;&#xD574;&#xC57C;
        &#xACC4;&#xC815;&#xC774; &#xC7A0;&#xAE30;&#xC9C0; &#xC54A;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#xB9CC;&#xB8CC;&#xC77C;</b>
      </td>
      <td style="text-align:left">&#xACC4;&#xC815; &#xB9CC;&#xB8CC;&#xC77C; &#xD544;&#xB4DC;&#xC785;&#xB2C8;&#xB2E4;.
        1970&#xB144; 1&#xC6D4; 1&#xC77C; &#xAE30;&#xC900;&#xC73C;&#xB85C; &#xC77C;&#xC218;&#xB85C;
        &#xD45C;&#xC2DC;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
  </tbody>
</table>

## **심볼릭 링크 / 하드 링크 설명**

### **inode의 개념 간략 : 파일들의 주소**

파일들의 정보는 바로 이 inode에 담겨있는데, 그만큼 파일에 있어서는 중요한 자료구조입니다. 파일은 각자의 inode를 가지고 있으며 inode 번호로 inode를 구분할 수 있습니다.  
  
일반적으로 툴을 돌리고 설정할때 Inode 에러가 나오기 시작하면 하드가 깨져가는것이니 빠른 백업이 필요 합니다.

### **심볼릭 링크\(Symbolic Link\)**

심볼링 링크는 윈도우즈의 바로가기와 같은 개념입니다. 즉, 원본 파일을 다른 경로나 이름을 써서 파일을 열 수 있는 개념이죠. 실습을 통해서 알아봅시다. 우선 빈 파일 origin을 touch명령어를 이용해서 만들어보세요.

![](https://blog.kakaocdn.net/dn/lJhmi/btqZ7KiGJnA/3kxpcPTmTwfVAKJZgsuDI0/img.png)

그리고 이 파일에 심볼릭 링크를 걸어보도록 하겠습니다.  명령은 아래와 같이 ln -s를 사용합니다.

**ln -s \[원본 파일\] \[심볼링 링크 파일\]**

링크 파일을 통해서 파일에 데이터를 기록하면 원본 파일도 같이 수정되는 것을 알 수 있습니다. 아래의 결과가 그것을 보여주지요.

![](https://blog.kakaocdn.net/dn/cBGRSz/btqZ0tCZ4D1/FKp8mJvV0ooC9D1oO42uk1/img.png)

만약 원본 파일을 삭제하면 어떻게 될까요? 링크 파일은 그 원본이 사라졌으니 파일을 찾을 수 없습니다. 링크된 파일은 원본의 파일에 의존적임을 알 수 있습니다.

![](https://blog.kakaocdn.net/dn/vZY2f/btqZ2bn4ccD/uoF04SKIL6aLGZNVITKK0k/img.png)

### **하드 링크\(Hard Link\)**

심볼릭 링크가 원본 파일에 의존적이었다고 한다면 하드링크는 원본 파일이 삭제되어도 링크된 파일은 여전히 그 정보를 갖고 있습니다. 그리고 링크 파일로 원본 파일을 변경할 수 있습니다. **여기서 하드링크와 심볼릭 링크와의 중요한 차이점은 같은 inode를 바라보느냐의 차이입니다.** 하드링크는 원본 파일과 같은 inode 블록을 가리키고 있기 때문에 원본 파일이 삭제되어도 링크된 파일은 여전히 파일의 내용을 보존할 수 있습니다. 하드 링크가 될때 inode에서 링크 수가 하나 늘어납니다.

![](https://blog.kakaocdn.net/dn/cU46Dy/btqZ3fKwY5W/ZeLGEKZQDGhLJLuuT7Iv50/img.png)

실제로 그런지 확인해보기 위해서 아래처럼 하드 링크된 파일에 내용을 기록하고 원본 파일을 열어보면 원본 파일이 하드 링크된 파일의 내용과 같게 변경이 되지요. 그리고 origin을 삭제해보고 링크 파일을 보면 여전히 그 내용을 볼 수 있습니다. 

![](https://blog.kakaocdn.net/dn/dE9dMo/btqZ0sRBHLu/KmlxSkLu2xnkT6Xz2yJvk1/img.png)

이때까지 설명한 내용은 아래의 그림으로 요약을 해봤습니다. 이해가 되셨길 바랍니다. 

![](https://blog.kakaocdn.net/dn/c9MA8Z/btqZ1JZDcZd/Jr5GXpP8luPTvPDuoRdMvK/img.png)







