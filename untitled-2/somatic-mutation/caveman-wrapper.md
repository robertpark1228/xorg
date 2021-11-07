---
description: ICGC 분석 파이프라인 Wellcomm Trust Sanger
---

# CAVEMAN WRAPPER

### Caveman-Wrapper : Whole Genome & Exome SNV Calling&#x20;

| **Result** | **SNP(SNV)** | **INDEL**  | **CNV**   | **SV**    |
| ---------- | ------------ | ---------- | --------- | --------- |
| **Tools**  | **CAVEMAN**  | **PINDEL** | **ASCAT** | **BRASS** |

(전처리 - FASTQ-BAM파일)\
(중간 처리 - BAM 파일) [https://github.com/cancerit/CaVEMan](https://github.com/cancerit/CaVEMan)\
[https://github.com/cancerit/cgpCaVEManWrapper](https://github.com/cancerit/cgpCaVEManWrapper)

1\) Caveman SNV : [https://github.com/cancerit/cgpCaVEManWrapper](https://github.com/cancerit/cgpCaVEManWrapper)\
[1) 기초적인 Running 방법 : https://bioinformatics-core-shared-training.github.io/cruk-summer-school-2017/Day3/somatic\_snv\_calling\_practical.html](https://bioinformatics-core-shared-training.github.io/cruk-summer-school-2017/Day3/somatic\_snv\_calling\_practical.html)

## : DOCKER 기반의 caveman Wrapper 실행

* DOCKER 이미지 접속&#x20;

```
docker run -it -v /main/test_analysis/raw_data_wgrs/:/home/ubuntu/data:rw 1cae84e5cf07
```

{% hint style="info" %}
\-it :  -_it 옵션_은 특히 컨테이너의 쉘(shell)이나 CLI 도구를 사용할 때 매우 유용하게 사용됩니다.
{% endhint %}

```
ds-cgpwgs.pl -r /home/ubuntu/data/reference/core_ref_GRCh38_hla_decoy_ebv.tar.gz -a /home/ubuntu/data/reference/VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91.tar.gz -si /home/ubuntu/data/reference/SNV_INDEL_ref_GRCh38_hla_decoy_ebv-fragment.tar.gz -t /home/ubuntu/data/p41-CDDO5nM/mapped/p41-CDDO5nM_sorted.bam -tidx /home/ubuntu/data/p41-CDDO5nM/mapped/p41-CDDO5nM_sorted.bam.bai -n /home/ubuntu/data/p22-hFSiPS1/mapped/p22-hFSiPS1_sorted.bam -nidx /home/ubuntu/data/p22-hFSiPS1/mapped/p22-hFSiPS1_sorted.bam.bai -species human -assembly GRCh38 -cores 100 -exclude chrEBV,%_random,chrUn_% -o /home/ubuntu/data/ -cs /home/ubuntu/data/reference/CNV_SV_ref_GRCh38_hla_decoy_ebv_brass6+.tar.gz -qcset /home/ubuntu/data/reference/qcGenotype_GRCh38_hla_decoy_ebv.tar.gz -pl 3.65 -pu 1.0
```

## 구동 성공시 :

![](<../../.gitbook/assets/image (106).png>)

![](<../../.gitbook/assets/image (107).png>)





## :구동 성공시





##

## 에러처리

![bas 파일이 없는 경우](<../../.gitbook/assets/image (103).png>)

### :해결방법&#x20;

: bam\_stat -i \[bam파일] -o \[bas파일]&#x20;

![](<../../.gitbook/assets/image (104).png>)





\
당 파이프라인은 복잡한 실행법 및 dependencies 등으로 해당 그룹에서 만들어놓은 docker pipeline 을 이용합니다.



Caveman Running 이후 결과 파일\
1\)&#x20;







[https://github.com/cancerit/dockstore-cgpwgs/tree/develop/examples](https://github.com/cancerit/dockstore-cgpwgs/tree/develop/examples)\
[https://github.com/cancerit/dockstore-cgpwgs/issues/50](https://github.com/cancerit/dockstore-cgpwgs/issues/50)\
docker 처리 -> filteration stratagy







## 참고자료

1\) [https://bioinformatics-core-shared-training.github.io/cruk-summer-school-2017/Day3/somatic\_snv\_calling\_practical.html](https://bioinformatics-core-shared-training.github.io/cruk-summer-school-2017/Day3/somatic\_snv\_calling\_practical.html)\
2\) Caveman Wrapper 방법

{% embed url="https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6097605/#S21" %}



