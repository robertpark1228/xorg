---
description: 'Structure Variation [구조변이 : 복제수 변이]'
---

# SV : CNV 분석

### Copy Number Variation \(복제수변이/CNV\)

* CNV 를 검출하는 방법은 다양한 알고리즘이 존재하나 결과값이 상이하다는 문제점이 있습니다.  해당 문제를 극복하기 위해 여러 툴을 겹쳐 사용할 수 있습니다.  

![](../../.gitbook/assets/image%20%2878%29.png)

  


## Structure Variation \(구조변이/SV\)

* 구조 변이 역시 건강한 사람에게도 나타나는 현상입니다.



![](../../.gitbook/assets/image%20%284%29.png)



## - 분석 파이프라인

#### 5~6개 동시 분석 후 합해주는 Parliament2 라는 툴을 사용하여 개별 CNV 분석

Docker 형태로 한번 설치하면 다량의 CNV/SV 툴을 사용하여 분석   
1. 논문 링크 : [https://academic.oup.com/gigascience/article/9/12/giaa145/6042728](https://academic.oup.com/gigascience/article/9/12/giaa145/6042728)\)  
2. 툴링크 : [https://github.com/dnanexus/parliament2](https://github.com/dnanexus/parliament2)\)  
3. docker install 로 설치 \(인터넷 필수\)  
4. 분석 lumpy, delly\(deletion, insertion, inversion, duplication\), breakdancer, cnvnator, manta  
  
실습 / 해당 파이프라인은 Docker Package 를 이용합니다  
  
실습, screen 설정 후

```text
$bash parliament2_main.sh /main/test_analysis/raw_data_wgrs/p51-CON/mapped/p51-CON_sorted.bam
```

스크립트 구조

```text
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

결과물  


![](../../.gitbook/assets/image%20%2867%29.png)

  
Annotation & Visualization

[https://github.com/mobidic/knotAnnotSV](https://github.com/mobidic/knotAnnotSV)  
[https://lbgi.fr/AnnotSV/](https://lbgi.fr/AnnotSV/)  


## - 참고해볼 만한 사이트 /  자료

1\) CNV/SV 분석  
[https://repository.kisti.re.kr/bitstream/10580/6331/1/2016-072%20%EC%95%94%EC%9C%A0%EC%A0%84%EC%B2%B4%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B6%84%EC%84%9D.pdf](https://repository.kisti.re.kr/bitstream/10580/6331/1/2016-072%20%EC%95%94%EC%9C%A0%EC%A0%84%EC%B2%B4%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B6%84%EC%84%9D.pdf)  
  
2\) Structural variant detection in cancer genomes: computational challenges and perspectives for precision oncology  
[https://www.nature.com/articles/s41698-021-00155-6](https://www.nature.com/articles/s41698-021-00155-6)  
  
3\) CNV/SV 상세 설  
[https://2wordspm.com/2018/05/02/ngs-%eb%8d%b0%ec%9d%b4%ed%84%b0%eb%a5%bc-%ec%9d%b4%ec%9a%a9%ed%95%9c-cnv-%eb%b6%84%ec%84%9d/](https://2wordspm.com/2018/05/02/ngs-%eb%8d%b0%ec%9d%b4%ed%84%b0%eb%a5%bc-%ec%9d%b4%ec%9a%a9%ed%95%9c-cnv-%eb%b6%84%ec%84%9d/)  
  
4\) exome CNV Benchmarking \(비교 테스트\)

[https://www.nature.com/articles/s41598-021-93878-2](https://www.nature.com/articles/s41598-021-93878-2)  
  
5\) CNV 관련  
[https://www.biocompare.com/Editorial-Articles/363086-CNV-Analysis-Shifts-Focus-to-NGS-Sequences/](https://www.biocompare.com/Editorial-Articles/363086-CNV-Analysis-Shifts-Focus-to-NGS-Sequences/)  
  
6\) 

