---
description: scp 명령어 사용법
---

# Linux:scp 사용법

1\) SCP 의 기능은 서버 투 서버로 카피하는 기능.

2\) 대용량 파일은 screen 기능 이용필요 (중도 끊어짐) &#x20;

사용법

{% hint style="info" %}
#### scp -rP \[포트번호] \[유저]@\[IP주소]:(폴더 또는 파일) (카피될 폴더)
{% endhint %}

1\) bamdisk  폴더를 hy 210.117.210.232 disk1 풀더아래로 옮긴다고 할 &#x20;

scp -rP 3030 ./bamdisk/ hy@210.117.210.232:/disk1/

&#x20;

2\) hy 210.117.210.232 disk1 폴더를 ./bamdisk 아래로 옮긴다고 할때 &#x20;

scp -rP 3030 hy@210.117.210.232:/disk1/ ./bamdisk/&#x20;



