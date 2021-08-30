---
description: '참조)https://quadrant.tistory.com/3'
---

# \[RNA-Expression\]HISAT2 / Cufflinks / Cuffdiff

### **주의사**

BAM 파일과 GFF 또는 GTF 그리고 hs38 fasta 파일의 chr 존재 여부가 일치하는지 확인하고 진행 하셔야 합니다.



BAM 또는 SAM 파일 확인은 samtools view -h 

## \*\*\*\*

## 분석 파이프라인 간략

Rawdata Filtering -&gt; HISAT2 \(맵핑\) -&gt; Cufflink \(붙은 갯수 계산\) -&gt; Cuffdiff \(얼마나 다른지\) -&gt; R: cummDE / GSEA 분석 -&gt; REACTOME 분석  


### Cufflinks 파일 포맷 설명

[http://cole-trapnell-lab.github.io/cufflinks/file\_formats/](http://cole-trapnell-lab.github.io/cufflinks/file_formats/)



### HISAT2 MAPPING

{% page-ref page="hisat2-mapping-+-seqmonk.md" %}

### Cufflinks - FPKM / Expression 계산\([http://cole-trapnell-lab.github.io/cufflinks/tools/](http://cole-trapnell-lab.github.io/cufflinks/tools/)\)

### 래퍼런스 : [https://support.illumina.com/sequencing/sequencing\_software/igenome.html](https://support.illumina.com/sequencing/sequencing_software/igenome.html)

![](../../.gitbook/assets/image%20%28145%29.png)

```text
cufflinks -p 8 -G /disk1/oneomics_analysis/beta_pipeline/references/hg38/Homo_sapiens/NCBI/GRCh38/Annotation/Genes/genes.gtf -o ./p33 ../../hisat2_bam/sample_hfsips1-p33.sorted.bam
```

### 

### cuffmerge \(샘플을 합해줌\)

```text
cuffmerge
```



### cuffdiff \(합해진 샘플에서 DEG 데이터 제작\)

```text
cuffdiff -o ./ -b /disk1/oneomics_analysis/beta_pipeline/references/hg38/hs38.fa -u ./merged_asm/merged.gtf -p 40 -L Case,Control /disk1/oneomics_analysis/rna_rnaseq_pipeline/hisat2_bam/p23/sample_hfsips1-p23.sorted.bam /disk1/oneomics_analysis/rna_rnaseq_pipeline/hisat2_bam/p33/sample_hfsips1-p33.sorted.bam,/disk1/oneomics_analysis/rna_rnaseq_pipeline/hisat2_bam/sample_hfsips1-p33_cddo_5nm.sorted.bam,/disk1/oneomics_analysis/rna_rnaseq_pipeline/hisat2_bam/sample_hfsips1-p41_cddo_5nm.bam,/disk1/oneomics_analysis/rna_rnaseq_pipeline/hisat2_bam/sample_hfsips1-p41_con.sorted.bam
```



cummeRbund \( R Package\)







분석  중 화면   


![](../../.gitbook/assets/image%20%28146%29.png)



Cuffmerge







Cuffdiff







### 

### 

### 

### 

### 

### 

### 

### 

### 

### 

### 

### 

### 

### 

### 

### 

### 

### 리눅스 \(Ubuntu 16.0.4\) 운영체제에서 분석한 내용을 토대로 작성한 글입니다.

**\(Illumina short read RNA-seq, Paired-end reads\)**

**0. De novo vs. Reference based transcriptome analysis**

**Raw data preprocessing 이 끝난 후에는 자신의 샘플의 species 가 genome 이 있는지 없는지에 따라 위 제목처럼 de novo 와 reference based 2가지 분석방법으로 나뉩니다.**

**1\) 우선, Geonme 즉 reference 가 있는 경우로 RNA-seq reads 를 reference 에 mapping 하고 이를 기반으로 transcript assembly & gene expression quantification 을 할 수 있습니다.**

**2\). 그렇다면 genome 이 없을 때, 이때는 genome 역할을 할 수 있는 transcripts 들의 집합을 "transcriptome assembly" 라고 하며, 아무 기존의 정보 없이 새로 만들기 때문에 De novo transcriptome assembly 라고 합니다.**

**이글에선 Reference based transcriptome analysis 에 대해 알아보고,**

  
**다음글에서 De novo transcriptome analysis 를 알아봅시다.**

**Reference based transcriptome analysis 에서**

**필요한 tool은 hisat2, cufflinks, samtools 는 필수이며, bowtie, bowtie2 는 위 tool 설치 과정에서 필요할 수 있습니다.** 

**0. Preparing genome data**

**Genome data 는 NCBI, UCSC genome browser, Ensembl 등의 DB 에서 받을 수 있습니다.**

**여기서 필요한 data 는**

**Genome.fa :  Genome sequence file 입니다.**

**genes.gtf : Annotation 정보 file 입니다.**

**잘 연구가 된 model species 같은 경우에는 illumina 에서 제공하는** [**싸이트**](https://support.illumina.com/sequencing/sequencing_software/igenome.html) **에서 다운 받을 수 있습니다.**

**\(sequence, annotation 파일 외에도 많은 것들이 압축되어 있기 때문에 서버로 다운받는걸 추천\)**

**그러나 해당 싸이트에 없으면, 위에서 언급드린 DB 에서 다운로드 받으면 됩니다.**

**1. Mapping reads to genome**

**~Mapping 을 진행할 때는** [**HISAT2**](https://ccb.jhu.edu/software/hisat2/manual.shtml#indexing-a-reference-genome) **program \[1\] 을 사용하였습니다.~**

**Reference 에 reads 를 mapping 하기전에 indexing 을 먼저 해야합니다.** 

**\* indexing 이란?**

**책에 색인이 있는것처럼 reference genome 을 indexing 해놓게 되면 이후에 reads 를 mapping 할때 greedy 하게 탐색하는것이 아닌 훨씬 더 적은 메모리와 시간으로 mapping 을 수행할 수 있습니다.**

**Indexing command 입니다.**

  
**새로 디렉토리를 만들어서 그 안에 genome file 하나만 넣어두고 하는게 좋습니다.**

**hisat2-build -p 8 Genome.fna IndexName**

**-p : indexing 할 때 사용할 cpu 갯수 입니다.**

**Genome.fna: reference genome sequences**

**IndexName: indexing 될  파일 이름입니다.**

  
**hisat index 가 완료되면  8개의 파일이 생성됩니다**

  
**IndexName.1.ht2**

**IndexName.2.ht2**

**IndexName.3.ht2**

**IndexName.4.ht2**

**IndexName.5.ht2**

**IndexName.6.ht2**

**IndexName.7.ht2**

**IndexName.8.ht2**

  
  
**이제는 reads 를 mapping 해볼 차례 입니다.**

  
**hisat2  -p 8 -x IndexName -1 forward.fastq -2 reverse.fastq -S output.sam  --dta-cufflinks**

**-p : cpu 갯수 입니다.**

**-x : 미리 만들어 두었던 index 경로 및 IndexName**

**-1, -2 : Trimming 완료된 reads 를 넣어주면 됩니다.**

**-S : output 이름 입니다. Sam format 으로 생성됩니다.**

  
  
**\*\*\* 중요한 점** 

**hisat2 mapping 이 끝나면 alignment summary  가 출력 되는데**

  
  
**이 내용이 따로 저장되지 않고 standard error \("stderr"\) 로 출력 됩니다.**

**하지만, 이 내용은 중요한 정보이기 때문에 stderr 를 저장하는 명령어를 통해 저장합니다.**

  
**저는 command 뒤에  2&gt;&1  \|tee  alignmet.summary 이런식으로 지정을 해줍니다.**

  
**hisat2  -p 8 -x IndexName -1 forward.fastq -2 reverse.fastq -S output.sam  --dta-cufflinks  2&gt;&1  \|tee alignmet.summary** 

  
  
  
**ex\) alignment.summary 에서 overall alignment rate 계산하는 방법**

**37537336 reads; of these:**

  **37537336 \(100.00%\) were paired; of these:**

    **986245 \(2.63%\) aligned concordantly 0 times**

    **33036211 \(88.01%\) aligned concordantly exactly 1 time**

    **3514880 \(9.36%\) aligned concordantly &gt;1 times**

    **----**

    **986245 pairs aligned concordantly 0 times; of these:**

      **112422 \(11.40%\) aligned discordantly 1 time**

    **----**

    **873823 pairs aligned 0 times concordantly or discordantly; of these:**

      **1747646 mates make up the pairs; of these:**

        **1077358 \(61.65%\) aligned 0 times**

        **579867 \(33.18%\) aligned exactly 1 time**

        **90421 \(5.17%\) aligned &gt;1 times**

**98.56% overall alignment rate**

**98.56% =  {33036211 + 3514880 + 112422 + \(579867 + 90421\) / 2} / 37537336 x 100**

**2. Bam sorting**

**Generating a sort.bam file이후에 expression level quantification 하기 위해 cufflinks 를 돌릴 예정임이때 hisat2 결과로 나온 sam file 을 sorting 및 bam file 로 format 변경을 해주어야함.**

Command

samtools  sort  -@ 8   -o  output.sort.bam  output.sam

**-@ : cpu 갯수 입니다.**

###  

**3. Generating transcript assembly** 

**이후 과정은 Tuxedo protocol \[2\] 을 기반으로 진행합니다.**

Command

cufflinks  -p 8  -G genes.gtf  -o output\_directory  output.sort.bam

**-p : cpu 갯수 입니다.**

**-G : Annotation file**

**-o : cufflinks 결과는 디렉토리로 출력되기 때문에 이름을 지정해주면 됩니다.**

  
  
**완료되면, 디렉토리 내에** 

  
**genes.fpkm\_tracking, isoforms.fpkm\_tracking, skipped.gtf, transcripts.gtf**

  
**4개의 파일이 생성됩니다.**

  
**여기서 각 샘플별로 expression level 도 계산되기 때문에 참고 할수 있습니다.**

**4. Merging assemblies**

cufflinks assemblies 를 merge 하여 single annotation 파일을 만든다.

이후 샘플간 expression level 비교를 하기 위해서는 각각의 transcript assembly 가 아닌 하나로 만들어진 assembly 가 필요하기 때문입니다.

\* 우선 assembly 파일들의 경로가 필요함

vi  assemblies.txt

ex\) assemblies.txt

**../cufflinks\_out/C-1\_cuff\_out/transcripts.gtf**

**../cufflinks\_out/C-2\_cuff\_out/transcripts.gtf**

**../cufflinks\_out/C-3\_cuff\_out/transcripts.gtf**

**../cufflinks\_out/N-1\_cuff\_out/transcripts.gtf**

**../cufflinks\_out/N-2\_cuff\_out/transcripts.gtf**

**../cufflinks\_out/N-3\_cuff\_out/transcripts.gtf**

Command

cuffmerge  -p 8 -g genes.gtf  -s genome.fa  assemblies.txt

-p : cpu 갯수 입니다.

-g : Annotation file

-s : Genome sequence file 입니다. \(IndexName 아님\)

-o : cufflinks 결과는 디렉토리로 출력되기 때문에 이름을 지정해주면 됩니다.

**완료되면 merged\_asm 디렉토리 가 생성되고 그 안에 있는  merged.gtf 가 합쳐진 transcriptome assembly 입니다.**

**5. Differential expression analysis**

**마지막으로 샘플간의 differentially expressed genes \(DEGs\) 를 찾기 위해 cuffdiff 분석을 진행해야합니다.**

  
**Command**

  
**cuffdiff   -o diff\_out  -b genome.fa  -p 8  -L EB,ES  -u  merged.gtf    output-1.sort.bam  output-2.sort.bam**

**-o : cufdiff 결과는 디렉토리로 출력되기 때문에 이름을 지정해주면 됩니다  
-b : Genome sequence file 입니다. \(IndexName 아님\)  
-p : cpu 갯수 입니다.  
-L : 샘플의 이름을 지정해주면 됩니다. \(숫자만 쓰거나 첫글자가 숫자이면 오류 출력됩니다.\)  
-u : multi-read-correction 옵션으로 genome 상에 multiple locations 에 mapping 된 reads 들의 weight read mapping 을 계산합니다.  
output-1.sort.bam  output-2.sort.bam : 여기서는 샘플을 넣어주면 되는데 만약 replication 이 있는 샘플이라면!  
  
ex\)  
Control-1.sort.bam  
Control-2.sort.bam  
  
Cancer-1.sort.bam  
Cancer-2.sort.bam  
  
파일의 경로 및 이름을 쓸때 replication 끼리는 띄어쓰기 하지 않고 comma 로 구분합니다.**  
  


**Control-1.sort.bam,Control-2.sort.bam  띄고 Cancer-1.sort.bam,Cancer-2.sort.bam**

  
  
**이렇게 해서 완료가 되면 여러가지 파일이 생성되는데,  
  
여기서 유전자 발현값에 집중하는 연구라면, 주로 확인하는 파일은**  
  
  


**gene\_exp.diff, genes.read\_group\_tracking**

**요 2개입니다.**

  
  
**gene\_exp.diff 에서는 유전자 별로 샘플들의 발현값 뿐만 아니라 differentially expressed genes 의 여부도 확인 할수 있게 p-value, q-value 정보를 포함하고 있습니다.**

  
**genes.read\_group\_tracking 는 Replicates 의 발현값을 확인해보기 위해 사용할 수 있습니다.**

  
  
**이 결과를 통해 모든 유전자들의 발현값을 확인할 수 있으며, DEGs 또한 찾아낼 수 있습니다.**

  
  
  
**------------------------------------------------------------------------------------------------------------**

  
**References**

  
**\[1\] Kim D, Langmead B and Salzberg SL. HISAT: a fast spliced aligner with low memory requirements. Nature Methods 2015**

  
**\[2\] C. Trapnell et al., Nature Protocols \(2012\), Differential gene and transcript expression analysis of RNA-seq experiments with TopHat and Cufflinks**

\*\*\*\*

\*\*\*\*

\*\*\*\*

## 기타참조



[http://wiki.biouml.org/index.php/Quantification\_of\_RNA-seq\_with\_Cufflinks\_\(no\_de-novo\_assembly\)\_for\_FASTQ\_files\_\(workflow\)](http://wiki.biouml.org/index.php/Quantification_of_RNA-seq_with_Cufflinks_%28no_de-novo_assembly%29_for_FASTQ_files_%28workflow%29)

