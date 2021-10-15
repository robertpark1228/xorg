---
description: HISAT2 MAPPING / SeqMonk 플랫폼을 이용한 RNAseq 분석
---

# \[RNA-Expression]HISAT2/STAR Mapping + SeqMonk

## 분석 파이프라인 간략

Rawdata Filtering -> HISAT2/STAR2 Mapping -> SeqMonk Data QC -> DE / GSEA 분석 -> REACTOME 분석

## Reference Genome 만들기(1회성) / 

hisat2는 만들어진 버전을 사용하시는게 좋습니다.\
[https://daehwankimlab.github.io/hisat2/download/#version-hisat2-221](https://daehwankimlab.github.io/hisat2/download/#version-hisat2-221)

```bash
$ hisat2  -p 16 -x ./hisat2_ref_genome/grch38/genome -1 ./hfsips1_p23_1.fq.gz -2 ./hfsips1_p23_2.fq.gz -S hfsip1.sam --dta-cufflinks
```

{% hint style="info" %}
\-p : 쓰레드 / 프로세서 수 / 작업할 워커 수\
\-1 : read1\
\-2 : read2\
\-S : 샘플명\
\--dta-cufflinks : cufflinks 호환모드
{% endhint %}

### STAR2 MAPPING 

{% content-ref url="untitled.md" %}
[untitled.md](untitled.md)
{% endcontent-ref %}

초기 부분 참조





## 0. Seqmonk 소개



![](<../../.gitbook/assets/image (119).png>)

![](<../../.gitbook/assets/image (117).png>)

![](<../../.gitbook/assets/image (114).png>)

![](<../../.gitbook/assets/image (129).png>)

![](<../../.gitbook/assets/image (113).png>)

![](<../../.gitbook/assets/image (123).png>)

![](<../../.gitbook/assets/image (128).png>)

![](<../../.gitbook/assets/image (127).png>)

![](<../../.gitbook/assets/image (115).png>)

![](<../../.gitbook/assets/image (132).png>)

![](<../../.gitbook/assets/image (133).png>)

![](<../../.gitbook/assets/image (126).png>)

![](<../../.gitbook/assets/image (124).png>)

![](<../../.gitbook/assets/image (125).png>)



![](<../../.gitbook/assets/image (121).png>)

![](<../../.gitbook/assets/image (131).png>)

![](<../../.gitbook/assets/image (130).png>)

![](<../../.gitbook/assets/image (116).png>)

![](<../../.gitbook/assets/image (118).png>)



![](<../../.gitbook/assets/image (134).png>)

![](<../../.gitbook/assets/image (120).png>)

![](<../../.gitbook/assets/image (112).png>)



![](<../../.gitbook/assets/image (122).png>)



## 1. 전처리 작업 

### Rawdata QC 및 전처리 파트 참고( [Rawdata QC 및 전처리](../untitled.md))

Fastq Rawdata filtering 작업은 동일 합니다.

## 2. 맵핑작업

서버내 래퍼런스 구축  - GRCh38 래퍼런스\
(신규 래퍼런스 구축 시 - 기 제작된 래퍼런스 다운로드 요망[https://daehwankimlab.github.io/hisat2/download/](https://daehwankimlab.github.io/hisat2/download/) )

```bash
$ /main/references/hg38/hisat2 -p 10 -x /main/references/hg38/genome -1 ${read_1} -2 ${read_2} -S /main/users/gilje/projects/teratocarcinoma/rna_seq/${output}.sam --dta-cufflinks
```

{% hint style="info" %}
HISAT2 Pipeline \
RNA 용 파이프라인 Mapper\
\-p : mapping 시 사용할 core 의 갯수\
\-1 : Read1\
\-2 : Read1\
\-S : Output samfile\
\--dta-cufflinks : 후속 단에서 cufflinks 라는 quantification 툴 분석을 위한 옵션
{% endhint %}

## 3. SeqMonk - 기본 분석 / DEG 분석 / GSEA 분 

Mobaxterm 원격 X 모니터를 이용한 작업\
\- 해당 작업은 서버상 화면을 불러와 작업 하는 방식 입니다.\
\
Seqmonk 파일 실행 

```bash
$ /main/program/SeqMonk/seqmonk
```

![초기화면](<../../.gitbook/assets/image (21).png>)

### Using custom genomes folder 문구 나올 시

```bash
$ /main/program/SeqMonk/Genomes
```

{% hint style="info" %}
File -> NewProject : 신규 프로젝트 만듬 -> 래퍼런스 선택

File -> Import Data : SAM/BAM 파일 로딩

Plot -> RNA-seq QC Plot -> QC 확인

Plot -> Duplication Plot -> QC 확인, Reads 길이가 길수룩 Duplication 비율이 높아야 정상

Data -> Define Probes -> Feature Probe Generator\
(CDS / Exon 선택) / 각 샘플에 있는 Gene 들(Probes) 정보를 모아오며, 그 Gene 들에 얼마나 Mapping 이 되었는지 계산하기 위해 다음 화면에서 -> Define Quantitation 을 어떤 방식으로 할지 결정.\
\
Illumina reads count 기반으로 계산을 하기 위해 기본 옵션으로 진행\
각 Probe 에 붙은 Reads 가 계산되며 눈으로 확인 가능.\
\
샘플 성향 / 분석 전략에 맞춰 Quantitation 을 다시 하며 \
RNA-Seq Quatntitiation Pipeline 을 설정 하여 -> Run pipeline\
Probe 기존 정보 덮어 쓰는 문구 -> YES\
\
 \
Plot -> Data Store Similarity -> PCA tSNE DataTree 등을 Selection 된 probe 에서 확인 가능

\
 \
\

{% endhint %}

### RNA-QC

![](<../../.gitbook/assets/image (39).png>)

![](<../../.gitbook/assets/image (78).png>)



"Data -> Define Probes -> Feature Probe Generator 이후 Quantification 화면  "

![](<../../.gitbook/assets/image (59).png>)



![Exon Region 에 붙은 Reads 들 확인 RNASeq Pipeline 실행 전](<../../.gitbook/assets/image (35).png>)

\


![RNASeq 으로 Quantification 후](<../../.gitbook/assets/image (90).png>)





### 내가 원하는 Gene 리스트 만들기

Filtering -> Probe Name Filter -> Gene name \
해당 기능은 Regex 기능이 붙어 있어 \_gene, \_upstream,\_downstream 을 gene name 뒤에 붙어 주면 됩니다.



1\) 원하는 Gene set 클릭\
2\) Filtering -> Probe name Filter \
3\) \[genename]\_gene\
\


![](<../../.gitbook/assets/image (108).png>)





![](<../../.gitbook/assets/image (109).png>)



참
