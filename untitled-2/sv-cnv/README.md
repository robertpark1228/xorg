---
description: 'Structure Variation [구조변이 : 복제수 변이]'
---

# \[DNA]SV : CNV 분석

### Copy Number Variation (복제수변이/CNV)

* CNV 를 검출하는 방법은 다양한 알고리즘이 존재하나 결과값이 상이하다는 문제점이 있습니다. \
  해당 문제를 극복하기 위해 여러 툴을 겹쳐 사용할 수 있습니다.\
  \


![](<../../.gitbook/assets/image (79).png>)

\


## Structure Variation (구조변이/SV)

* 구조 변이 역시 건강한 사람에게도 나타나는 현상입니다.



![](<../../.gitbook/assets/image (4).png>)



## - 분석 파이프라인

#### 5\~6개 동시 분석 후 합해주는 Parliament2 라는 툴을 사용하여 개별 CNV 분석

Docker 형태로 한번 설치하면 다량의 CNV/SV 툴을 사용하여 분석 \
1\. 논문 링크 : [https://academic.oup.com/gigascience/article/9/12/giaa145/6042728](https://academic.oup.com/gigascience/article/9/12/giaa145/6042728))\
2\. 툴링크 : [https://github.com/dnanexus/parliament2](https://github.com/dnanexus/parliament2))\
3\. docker install 로 설치 (인터넷 필수)\
4\. 분석 lumpy, delly(deletion, insertion, inversion, duplication), breakdancer, cnvnator, manta\
\
실습 / 해당 파이프라인은 Docker Package 를 이용합니다\
\
실습, screen 설정 후

```
$bash parliament2_main.sh /main/test_analysis/raw_data_wgrs/p51-CON/mapped/p51-CON_sorted.bam
```

스크립트 구조

```
#!/bin/bash
#===========================================================#
#For universal purpose, destination folder should be changed"
#===========================================================#

#bamfile_input
bam=${1}
#bamfile->baifile
bai=$(echo ${bam} | sed '{s/bqsr.bam/bqsr.bai/g}' | sed '{s/bam/bam.bai/g}')
#ref_38=
#ref_38_bai=

folder_1="/main/test_analysis/raw_data_wgrs/CNV_Parliament"

bam_analysis=$(echo ${bam} | awk -F '/' '{ print $NF }')
bai_analysis=$(echo ${bai} | awk -F '/' '{ print $NF }')

bam_name=$(echo ${bam_analysis} | sed '{s/_sorted_MarkDuplicate_bqsr.bam//g}')




##echo "Copying sample to analysis folder"
cp ${bam} ${PWD}/
cp ${bai} ${PWD}/

echo "create ${bam_name}_output as output folder "
echo ${bam_name}
        mkdir -p ${folder_1}/${bam_name}_output

echo "Starting analysis"
docker run -v ${folder_1}:/home/dnanexus/in -v ${folder_1}/${bam_name}_output:/home/dnanexus/out dnanexus/parliament2:v0.1.11-0-gb492db6d --bam ${bam_analysis} --bai ${bai_analysis} -r hs38.fa --fai hs38.fa.fai --lumpy --delly_deletion --delly_insertion --delly_inversion --delly_duplication --breakdancer --cnvnator --manta --svviz --genotype --prefix ${bam_name}

```

결과물\


![](<../../.gitbook/assets/image (68).png>)

\
Annotation & Visualization

[https://github.com/mobidic/knotAnnotSV](https://github.com/mobidic/knotAnnotSV)\
[https://lbgi.fr/AnnotSV/](https://lbgi.fr/AnnotSV/)\


## B-Allele Freq 설명 

### [https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true\&blogId=jinp7\&logNo=220951636178](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true\&blogId=jinp7\&logNo=220951636178)

**1. allelic imbalance & allelic specific expression (ASE)**\
\- allelic imbalance란? A difference in the expression between two alleles\
\- allelic imbalance를 통해서 small changes in the genetic information between alleles알 수 있다. \
\- 인간은 diploid organisms이기 때문에 two copies are expressed at the same level한다.\
\- the ratio of the expression levels이 일대일이 아닌경우 allelic imbalance라고 부른다.\
\- only one allele 존재하면 balance between the two alleles 결정할 수 없다.\
\- allelic imbalance의 원인은? Gene imprinting, Cis-acting mutations등이 있다. \
![](https://mblogthumb-phinf.pstatic.net/MjAxNzAzMDZfMjY5/MDAxNDg4ODAzOTQyMjAx.QW2u0ZlbNbI8kEenCMQFMkXGr8-B6eKo-pmY70-RkU8g.suRqnJs7Rm9R0r8dPBtqBG5KbQQzz_PRO7bBwChjapkg.PNG.jinp7/allelicimblance.png?type=w800)

\- Examination of allele-specific expression을 통해서 allelic imbalance 확인 할 수 있다. \
\- individual is a heterozygote for a variant할 때만 identify allele specific expression할 수 있다.\
\
**2. allele frequency & B-allele frequency**\
\- 정의:  the relative frequency of an allele at a particular locus in a population\
\- 값: fraction or percentage로 나타내어 진다.\
\- 계산법: counting the number of times an allele occurs in a population / the total number of occurrences of all alleles for that gene.\
\- B-allele frequency (BAF)\
\- 정의: a normalized measure of the allelic intensity ratio of two alleles (A and B), \
\- A형 allele이라고 말한다면, 이 말에는 A형과 상반되는 다른 allele, 즉 B형이 형질이 있음을 내포\
![](https://mblogthumb-phinf.pstatic.net/MjAxNzAzMDZfMjcg/MDAxNDg4ODA0NTIxNTg0.JqUoXuMr17h8ZKeoOEcuyhgX2sVQOCro20JF5lmi_rYg.2l4Mt1gKvBnwxB0Ia_hWtSmNlXWjQFb2jVHHd6VIxjYg.PNG.jinp7/monobi2.png?type=w800)

\- BAF값 0: BB, 0.5: AB, 1:BB, 0.66: BBA\
\
**3. Mono-allelic & Bi-allelic**\
![](https://mblogthumb-phinf.pstatic.net/MjAxNzAzMDZfMjQ1/MDAxNDg4ODA0MDA0MDcy.TnJdK1bMq1XcjcFxFfFZ8ghvdTQtq4epj5\_lYfGTsZUg.6mGFAcHNCYn0obspXVUuawosXdlXf-FopuESY5CyMTgg.PNG.jinp7/monobi.png?type=w800)

\- monoallelic의 정의: only one of the two copies of a gene is active, while the other is silent. \
\- Biallelic은 both alleles of a gene are transcribed\
![](https://mblogthumb-phinf.pstatic.net/MjAxNzAzMDZfMjMy/MDAxNDg4ODA0MDIwMjg1.U_rcij90eTDdNV8JMHaBFt5hvBCwWq9gDF7TgpPyMz4g.YVeh0Q2YMUSO24XQ0KWyFwX09gx1jlaRpcHheP9\_sokg.PNG.jinp7/biallelicmonoallelic.png?type=w800)

[https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true\&blogId=jinp7\&logNo=220951636178](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true\&blogId=jinp7\&logNo=220951636178)

## - 참고해볼 만한 사이트 /  자료

1\) CNV/SV 분석\
[https://repository.kisti.re.kr/bitstream/10580/6331/1/2016-072%20%EC%95%94%EC%9C%A0%EC%A0%84%EC%B2%B4%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B6%84%EC%84%9D.pdf](https://repository.kisti.re.kr/bitstream/10580/6331/1/2016-072%20%EC%95%94%EC%9C%A0%EC%A0%84%EC%B2%B4%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B6%84%EC%84%9D.pdf)\
\
2\) Structural variant detection in cancer genomes: computational challenges and perspectives for precision oncology\
[https://www.nature.com/articles/s41698-021-00155-6](https://www.nature.com/articles/s41698-021-00155-6)\
\
3\) CNV/SV 상세 설\
[https://2wordspm.com/2018/05/02/ngs-%eb%8d%b0%ec%9d%b4%ed%84%b0%eb%a5%bc-%ec%9d%b4%ec%9a%a9%ed%95%9c-cnv-%eb%b6%84%ec%84%9d/](https://2wordspm.com/2018/05/02/ngs-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-cnv-%EB%B6%84%EC%84%9D/)\
\
4\) exome CNV Benchmarking (비교 테스트)

[https://www.nature.com/articles/s41598-021-93878-2](https://www.nature.com/articles/s41598-021-93878-2)\
\
5\) CNV 관련\
[https://www.biocompare.com/Editorial-Articles/363086-CNV-Analysis-Shifts-Focus-to-NGS-Sequences/](https://www.biocompare.com/Editorial-Articles/363086-CNV-Analysis-Shifts-Focus-to-NGS-Sequences/)\
\
6\) 
