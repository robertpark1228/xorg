---
description: Docker
---

# 도커 : 전처리 자동화 파이프라인

## 도커&#x20;

![https://pkh11.medium.com/docker-%EB%8F%84%EC%BB%A4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-8b93d1a46aa8](<../.gitbook/assets/image (99).png>)

{% embed url="https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=todoskr&logNo=221261803381" %}



장점) 미리 구성된 파이프라인을 기반으로 하며  래퍼런스와 처리할 파일만 확보시 바로 Running 가능\
단점) 시스템 권한이 필요하며 리눅스 시스템에 대한 이해도가 필요 / 가상화 폴더에 대한 약간의 이해 필요





## 새로운 이미지 설치 화면

![](<../.gitbook/assets/image (101).png>)

## 설치된 이미지 확인

```
$ docker images
```

![](<../.gitbook/assets/image (102).png>)

: 현재까지 설치된 이미지 리스트 확인 가능



## :도커 이미지의 실행

```
docker run -it -v /main/test_analysis/raw_data_wgrs/:/home/ubuntu/data:rw 1cae84e5cf07
```

{% hint style="info" %}
\-it : 가상 도커에 접속 유지\
\-v : 볼륨 설정 / 가상 이미지 <-> 서버상 파일 / rw 옵션  - 읽기쓰기, ro : 읽기만 \
마지막 : \[image ID]
{% endhint %}

![](<../.gitbook/assets/image (105).png>)





