---
description: ICGC 분석 파이프라인 Wellcomm Trust Sanger
---

# CAVEMAN WRAPPER

### Caveman SNV Calling 소개

  


\(전처리 - FASTQ-BAM파일\)  
\(중간 처리 - BAM 파일\) [https://github.com/cancerit/CaVEMan](https://github.com/cancerit/CaVEMan)  
[https://github.com/cancerit/cgpCaVEManWrapper](https://github.com/cancerit/cgpCaVEManWrapper)





1\) Caveman SNV : [https://github.com/cancerit/cgpCaVEManWrapper](https://github.com/cancerit/cgpCaVEManWrapper)  
[1\) 기초적인 Running 방법 : https://bioinformatics-core-shared-training.github.io/cruk-summer-school-2017/Day3/somatic\_snv\_calling\_practical.html](https://bioinformatics-core-shared-training.github.io/cruk-summer-school-2017/Day3/somatic_snv_calling_practical.html)





DOCKER 기반의 실

```text
ds-cgpwgs.pl -r ~/core_ref_GRCh38_hla_decoy_ebv.tar.gz -a ~/VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91.tar.gz -si ~/SNV_INDEL_ref_GRCh38_hla_decoy_ebv-fragment.tar.gz -t ~/p33-CDDO0nM/p33-CDDO0nM_sorted.bam -tidx ~/p33-CDDO0nM/p33-CDDO0nM_sorted.bam.bai -n ~/p22-hFSiPS1/p22-hFSiPS1_sorted.bam -nidx ~/p22-hFSiPS1/p22-hFSiPS1_sorted.bam.bai -species human -assembly GRCh38 -cores 20 -exclude chrEBV,%_random,chrUn_% -o /home/ubuntu/ -cs ./CNV_SV_ref_GRCh38_hla_decoy_ebv_brass6+.tar.gz -qcset ./qcGenotype_GRCh38_hla_decoy_ebv.tar.gz -pl 3.65 -pu 1.0
```

RUNNING 화면

```text
Options loaded:
$VAR1 = {
          'mt_sm' => 'p33-CDDO0nM',
          'cs' => './CNV_SV_ref_GRCh38_hla_decoy_ebv_brass6+.tar.gz',
          'qc' => './qcGenotype_GRCh38_hla_decoy_ebv.tar.gz',
          'pi' => '3.65',
          'n' => '/home/ubuntu/p22-hFSiPS1/p22-hFSiPS1_sorted.bam',
          'si' => '/home/ubuntu/SNV_INDEL_ref_GRCh38_hla_decoy_ebv-fragment.tar.gz',
          'as' => 'GRCh38',
          'pu' => '1.0',
          'nidx' => '/home/ubuntu/p22-hFSiPS1/p22-hFSiPS1_sorted.bam.bai',
          'c' => 20,
          'o' => '/home/ubuntu/',
          'pc' => 8,
          'cr' => 350000,
          'r' => '/home/ubuntu/core_ref_GRCh38_hla_decoy_ebv.tar.gz',
          'tidx' => '/home/ubuntu/p33-CDDO0nM/p33-CDDO0nM_sorted.bam.bai',
          'e' => 'chrEBV,%_random,chrUn_%',
          'sp' => 'human',
          't' => '/home/ubuntu/p33-CDDO0nM/p33-CDDO0nM_sorted.bam',
          'wt_sm' => 'p22-hFSiPS1',
          'a' => '/home/ubuntu/VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91.tar.gz'
        };

core_ref_GRCh38_hla_decoy_ebv/genome.fa
core_ref_GRCh38_hla_decoy_ebv/genome.fa.fai
core_ref_GRCh38_hla_decoy_ebv/genome.fa.dict
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/exon_regions.indel.bed.gz
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/README.txt
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/exon_regions.sub.bed.gz.tbi
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/vagrent.cache.gz
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/exon_regions.indel.bed.gz.tbi
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/codingexon_regions.sub.bed.gz
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/gene_regions.bed.gz.tbi
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/vagrent.fa
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/codingexon_regions.indel.bed.gz
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/exon_regions.sub.bed.gz
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/gene_regions.bed.gz
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/codingexon_regions.sub.bed.gz.tbi
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/vagrent.cache.gz.tbi
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/codingexon_regions.indel.bed.gz.tbi
VAGrENT_ref_GRCh38_hla_decoy_ebv_ensembl_91/vagrent/vagrent.fa.fai
SNV_INDEL_ref_GRCh38_hla_decoy_ebv-fragment/pindel/
SNV_INDEL_ref_GRCh38_hla_decoy_ebv-fragment/pindel/softRules.lst
SNV_INDEL_ref_GRCh38_hla_decoy_ebv-fragment/pindel/pindel_np.gff3.gz.tbi
SNV_INDEL_ref_GRCh38_hla_decoy_ebv-fragment/pindel/WGS_Rules.lst
SNV_INDEL_ref_GRCh38_hla_decoy_ebv-fragment/pindel/HiDepth.bed.g

....
....
....

Start workflow: Fri Aug 13 04:17:12 UTC 2021

Loading user options from: /home/ubuntu//run.params
        BAM_MT : /home/ubuntu/p33-CDDO0nM/p33-CDDO0nM_sorted.bam
        BAM_WT : /home/ubuntu/p22-hFSiPS1/p22-hFSiPS1_sorted.bam
        NAME_MT : p33-CDDO0nM
        NAME_WT : p22-hFSiPS1
Setting up Parallel block 1
        [Parallel block 1] CaVEMan setup added...
        [Parallel block 1] Genotype Check added...
        [Parallel block 1] VerifyBam Normal added...
Starting Parallel block 1: Fri Aug 13 04:17:12 UTC 2021
        Starting geno
+ set +x
        Starting CaVEMan_setup
+ bash -c '/usr/bin/time -v compareBamGenotypes.pl   -o /home/ubuntu//WGS_p33-CDDO0nM_vs_p22-hFSiPS1/genotyped   -nb /home/ubuntu//tmp/p22-hFSiPS1.bam   -j /home/ubuntu//WGS_p33-CDDO0nM_vs_p22-hFSiPS1/genotyped/result.json   -tb /home/ubuntu//tmp/p33-CDDO0nM.bam   -s /home/ubuntu//reference_files/general.tsv   -g /home/ubuntu//reference_files/gender.tsv >& /home/ubuntu//timings/WGS_p33-CDDO0nM_vs_p22-hFSiPS1.time.geno ; echo '\''WRAPPER_EXIT: '\''$?'
+ set +x

```

## 에러처리

![bas &#xD30C;&#xC77C;&#xC774; &#xC5C6;&#xB294; &#xACBD;&#xC6B0;](../../.gitbook/assets/image%20%28104%29.png)

### :해결방법 

: bam\_stat -i \[bam파일\] -o \[bas파일\] 

![](../../.gitbook/assets/image%20%28102%29.png)





  
당 파이프라인은 복잡한 실행법 및 dependencies 등으로 해당 그룹에서 만들어놓은 docker pipeline 을 이용합니다.



Caveman Running 이후 결과 파일  
1\) 







[https://github.com/cancerit/dockstore-cgpwgs/tree/develop/examples](https://github.com/cancerit/dockstore-cgpwgs/tree/develop/examples)  
[https://github.com/cancerit/dockstore-cgpwgs/issues/50](https://github.com/cancerit/dockstore-cgpwgs/issues/50)  
docker 처리 -&gt; filteration stratagy



Calling 방법 설명이 매우 부





노

{% embed url="https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6097605/\#S21" %}





