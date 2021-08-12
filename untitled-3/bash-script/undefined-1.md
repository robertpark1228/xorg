---
description: AWK
---

# 파싱 명령어 소개

## AWK

![AWK &#xC758; &#xAE30;&#xBCF8;](../../.gitbook/assets/image%20%2850%29.png)



 AWK에서는 레**코드가 $0**, 그리고 **$1, ..., $N 은 필드**를 나타내는 열을 나타냅니다. 우리들이 사용할 파일은 위 내용은 아니고 아래의 내용을 담고 있습니다. 

예제파일 만들기

복사 후 -&gt; cat &gt; filelist.txt -&gt; 붙여넣기 -&gt; ctrl+d

```text
name    phone           birth           sex     score
reakwon 010-1234-1234   1981-01-01      M       100
sim     010-4321-4321   1999-09-09      F       88
nara    010-1010-2020   1993-12-12      M       20
yut     010-2323-2323   1988-10-10      F       59
kim     010-1234-4321   1977-07-17      M       69
nam     010-4321-7890   1996-06-20      M       75
```

#### 

## column 을 출력 : R 의 column 기능과 비슷한 기능.

```text
cat filelist.txt | awk '{  print $1 }' 
```

#### 결과물\)

```text
awk '{ print $1,$2 }' ./filelist.txt
```

\*\*\*\*

### **특정 pattern이 포함된 레코드 출력**

```text
awk '/rea/' ./filelist.txt
```

#### **결과물\)**

```text
reakwon 010-1234-1234   1981-01-01      M       100
```



### 내용을 더해서 출력하기

```text
awk '{ print ("name : " $1, ", "  "phone : " $2) }' ./filelist.txt
```

#### **결과물\)**

```text
name : name , phone : phone
name : reakwon , phone : 010-1234-1234
name : sim , phone : 010-4321-4321
name : nara , phone : 010-1010-2020
name : yut , phone : 010-2323-2323
name : kim , phone : 010-1234-4321
name : nam , phone : 010-4321-7890

```

\*\*\*\*

### **특정 Record를 검색하기 - if 구문 \(AWK 의 IF 구문은 AWK 내에서 따로 동작\)**

```text
awk '{ if ( $5 >= 80 ) print ($0) }' ./filelist.txt
```

**결과물\)**

```text
name    phone           birth           sex     score
reakwon 010-1234-1234   1981-01-01      M       100
sim     010-4321-4321   1999-09-09      F       88
```



### 원하는 데이터 열만 출력하기 / 4번쨰 열중 M 문자가 있는 경우 예

```text
awk '{ if ( $4 == "M" ) print ($0) }' ./filelist.txt
```

#### 결과물\)

```text
reakwon 010-1234-1234   1981-01-01      M       100
nara    010-1010-2020   1993-12-12      M       20
kim     010-1234-4321   1977-07-17      M       69
nam     010-4321-7890   1996-06-20      M       75
```



###  **다중 조건을 활용하기 / 4번째 줄이 M 이며 5번째 줄이 80점 이상**

```text
awk '{ if ( $4 == "M" && $5 >= 80) print ($0) }' ./filelist.txt
```

#### 결과물\)

```text
reakwon 010-1234-1234   1981-01-01      M       100
```



### **내장 함수**

```text
awk '{ print ("name leng : " length($1), "substr(0,3) : " substr($1,0,3)) }' ./filelist.txt
```

#### **결과물\)**

```text
name leng : 4 substr(0,3) : nam
name leng : 7 substr(0,3) : rea
name leng : 3 substr(0,3) : sim
name leng : 4 substr(0,3) : nar
name leng : 3 substr(0,3) : yut
name leng : 3 substr(0,3) : kim
name leng : 3 substr(0,3) : nam
```



### 반복문

```text
awk '{
for(i=0;i<2;i++)
 print( "for loop :" i "\t" $1, $2, $3)
}' ./filelist.txt
```

#### 결과물\)

```text
for loop :0     name phone birth
for loop :1     name phone birth
for loop :0     reakwon 010-1234-1234 1981-01-01
for loop :1     reakwon 010-1234-1234 1981-01-01
for loop :0     sim 010-4321-4321 1999-09-09
for loop :1     sim 010-4321-4321 1999-09-09
for loop :0     nara 010-1010-2020 1993-12-12
for loop :1     nara 010-1010-2020 1993-12-12
for loop :0     yut 010-2323-2323 1988-10-10
for loop :1     yut 010-2323-2323 1988-10-10
for loop :0     kim 010-1234-4321 1977-07-17
for loop :1     kim 010-1234-4321 1977-07-17
for loop :0     nam 010-4321-7890 1996-06-20
for loop :1     nam 010-4321-7890 1996-06-20

```

### \*\*\*\*

### **BEGIN, END pattern**

```text
awk '
BEGIN {
  sum = 0
  cnt = -1
 }

 {
  sum += $5
  cnt++
 }

 END {
  avg = sum/cnt
  print ("sum :" sum ", average :" avg)
 }' ./filelist.txt
```

결과물 \) 

```text
sum :411, average :82.2
```



{% hint style="info" %}

{% endhint %}



\*\*\*\*









