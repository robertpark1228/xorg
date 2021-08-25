---
description: HISAT2 MAPPING / SeqMonk 플랫폼을 이용한 RNAseq 분석
---

# HISAT2 Mapping + SeqMonk

## 분석 파이프라인 간략

Rawdata Filtering -&gt; HISAT2 Mapping -&gt; SeqMonk Data QC -&gt; DE / GSEA 분석 -&gt; REACTOME 분석

## 0. Seqmonk 소개



![](../../.gitbook/assets/image%20%28116%29.png)

![](../../.gitbook/assets/image%20%28115%29.png)

![](../../.gitbook/assets/image%20%28113%29.png)

![](../../.gitbook/assets/image%20%28120%29.png)

![](../../.gitbook/assets/image%20%28112%29.png)

![](../../.gitbook/assets/image%20%28117%29.png)

![](../../.gitbook/assets/image%20%28119%29.png)

![](../../.gitbook/assets/image%20%28118%29.png)

![](../../.gitbook/assets/image%20%28114%29.png)

![](../../.gitbook/assets/image%20%28121%29.png)

![](../../.gitbook/assets/image%20%28122%29.png)













## 

## 

## 

## 1. 전처리 작업 

### Rawdata QC 및 전처리 파트 참고\( [Rawdata QC 및 전처리](../untitled.md)\)

Fastq Rawdata filtering 작업은 동일 합니다.

## 2. 맵핑작업

서버내 래퍼런스 구축  - GRCh38 래퍼런스  
\(신규 래퍼런스 구축 시 - 기 제작된 래퍼런스 다운로드 요망[https://daehwankimlab.github.io/hisat2/download/](https://daehwankimlab.github.io/hisat2/download/) \)

```bash
$ /main/references/hg38/hisat2 -p 10 -x /main/references/hg38/genome -1 ${read_1} -2 ${read_2} -S /main/users/gilje/projects/teratocarcinoma/rna_seq/${output}.sam --dta-cufflinks
```

{% hint style="info" %}
HISAT2 Pipeline   
RNA 용 파이프라인 Mapper  
-p : mapping 시 사용할 core 의 갯수  
-1 : Read1  
-2 : Read1  
-S : Output samfile  
--dta-cufflinks : 후속 단에서 cufflinks 라는 quantification 툴 분석을 위한 옵션
{% endhint %}

## 3. SeqMonk - 기본 분석 / DEG 분석 / GSEA 분 

Mobaxterm 원격 X 모니터를 이용한 작업  
- 해당 작업은 서버상 화면을 불러와 작업 하는 방식 입니다.  
  
Seqmonk 파일 실행 

```bash
$ /main/program/SeqMonk/seqmonk
```

![&#xCD08;&#xAE30;&#xD654;&#xBA74;](../../.gitbook/assets/image%20%2821%29.png)

### Using custom genomes folder 문구 나올 시

```bash
$ /main/program/SeqMonk/Genomes
```

{% hint style="info" %}
File -&gt; NewProject : 신규 프로젝트 만듬 -&gt; 래퍼런스 선택

File -&gt; Import Data : SAM/BAM 파일 로딩

Plot -&gt; RNA-seq QC Plot -&gt; QC 확인

Plot -&gt; Duplication Plot -&gt; QC 확인, Reads 길이가 길수룩 Duplication 비율이 높아야 정상

Data -&gt; Define Probes -&gt; Feature Probe Generator  
\(CDS / Exon 선택\) / 각 샘플에 있는 Gene 들\(Probes\) 정보를 모아오며, 그 Gene 들에 얼마나 Mapping 이 되었는지 계산하기 위해 다음 화면에서 -&gt; Define Quantitation 을 어떤 방식으로 할지 결정.  
  
Illumina reads count 기반으로 계산을 하기 위해 기본 옵션으로 진행  
각 Probe 에 붙은 Reads 가 계산되며 눈으로 확인 가능.  
  
샘플 성향 / 분석 전략에 맞춰 Quantitation 을 다시 하며   
RNA-Seq Quatntitiation Pipeline 을 설정 하여 -&gt; Run pipeline  
Probe 기존 정보 덮어 쓰는 문구 -&gt; YES  
  
   
Plot -&gt; Data Store Similarity -&gt; PCA tSNE DataTree 등을 Selection 된 probe 에서 확인 가능

  
   
  
{% endhint %}

### RNA-QC

![](../../.gitbook/assets/image%20%2839%29.png)

![](../../.gitbook/assets/image%20%2878%29.png)



"Data -&gt; Define Probes -&gt; Feature Probe Generator 이후 Quantification 화면  "

![](../../.gitbook/assets/image%20%2859%29.png)



![Exon Region &#xC5D0; &#xBD99;&#xC740; Reads &#xB4E4; &#xD655;&#xC778; RNASeq Pipeline &#xC2E4;&#xD589; &#xC804;](../../.gitbook/assets/image%20%2835%29.png)

  


![RNASeq &#xC73C;&#xB85C; Quantification &#xD6C4;](../../.gitbook/assets/image%20%2890%29.png)





### 내가 원하는 Gene 리스트 만들기

Filtering -&gt; Probe Name Filter -&gt; Gene name   
해당 기능은 Regex 기능이 붙어 있어 \_gene, \_upstream,\_downstream 을 gene name 뒤에 붙어 주면 됩니다.



1\) 원하는 Gene set 클릭  
2\) Filtering -&gt; Probe name Filter   
3\) \[genename\]\_gene  
  


![](../../.gitbook/assets/image%20%28108%29.png)





![](../../.gitbook/assets/image%20%28109%29.png)



참

