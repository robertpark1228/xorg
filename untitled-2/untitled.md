---
description: Rawdata Preprocessing
---

# \[공통]Rawdata QC 및 전처리

## 1. Trim_galore

1. 설치 시 필수로 요구하는 툴 : \
   1\) cutadapt\
   [https://cutadapt.readthedocs.io/en/stable/installation.html](https://cutadapt.readthedocs.io/en/stable/installation.html)\
   2\) fastqc \
   [https://www.bioinformatics.babraham.ac.uk/projects/fastqc/](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)\
   \
   1\. cutadapt 설치 ( 계정 별로 설치 필요할 수 있습니다.)

```
sudo apt install cutadapt
```

        2\. fastqc 설치 ( 계정 별로 필요할 수 있습니다.)

```bash
sudo apt install fastqc
```

       3\. Running

```bash
trim_galore --trim1 --illumina --paired [1.fq] [2.fq] -j [core]
```

## 2. Trimmomatic

[http://www.usadellab.org/cms/?page=trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic)\
\
DOWNLOAD : [http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.39.zip](http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.39.zip)\
\
jar 파일로 끝나는 파일들은 java -jar 로 동작을 합니다. 

```bash
java -jar trimmomatic-0.39.jar PE input_forward.fq.gz input_reverse.fq.gz output_forward_paired.fq.gz output_forward_unpaired.fq.gz output_reverse_paired.fq.gz output_reverse_unpaired.fq.gz ILLUMINACLIP:TruSeq3-PE.fa:2:30:10:2:keepBothReads LEADING:3 TRAILING:3 MINLEN:36
```

## 3. 기타

Scythe, Sickle, Atropos 등 다양한 툴이 있습니다.

참조 ) [https://www.hadriengourle.com/tutorials/qc/](https://www.hadriengourle.com/tutorials/qc/) 







