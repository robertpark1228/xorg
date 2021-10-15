# \[DNA]Whole Genome/Exome/Target Seq. 소개

## Whole Genome/Exome/Target Sequencing

### 1. 연구 주제 정의 

1\) Population Study :  Cohort 에서 나타나는 Variants 에 대한 양상 파악, 군집 Vs 군집 대 양상 파악 \
2\) Individual Case(Cancer)-Control(Normal Tissue) : 한 사람(Individual)에서 여러 샘플을 채취 -> 분석\
일반적으로 암샘플 분석시 사용\
3\) 기타 : Trio analysis : 가계 분석 등 ( Rare Disease) 



### 2. 분석 방법 정의

1\) Population Study : Individual 의 Germline 을 모아서 여러명의 데이터가 포함된 VCF 를 제작\
2\) Individual Case(Cancer)-Control(Normal Tissue) : 유전체 변화로 인해 나타나는 Individual 내부에서 일어나는 현상을 알아내기 위함\
3\) Trio 분석 : 1) 번 Population Study 와 방식은 비슷하나 가계로 VCF 를 만들어 대대로 어디서 내려오는지 확인하기 위함.(현존 NGS 기술로는 Phasing 이 불가)

등이 있습니다.\
\
개념.1) zygosity, [https://verd-antique.tistory.com/162](https://verd-antique.tistory.com/162)\
개념.2) haplotype, [https://2wordspm.com/2020/04/27/haplotype-%EC%9D%98%EB%AF%B8%EC%99%80-linkage-disequilibrium-ld-haplotype-phasing-%EA%B2%80%EC%82%AC-%EB%B0%A9%EB%B2%95/](https://2wordspm.com/2020/04/27/haplotype-%EC%9D%98%EB%AF%B8%EC%99%80-linkage-disequilibrium-ld-haplotype-phasing-%EA%B2%80%EC%82%AC-%EB%B0%A9%EB%B2%95/)\
\
Haplotype 알츠하이머 예시 APOE SNP 형에 따른 알츠하이머 유병

![](<../../.gitbook/assets/image (85).png>)

### 3. 분석 방법 정의 후 

### 파이프라인 선정

#### 1. Population Study

대표적으로 학술용으로는 GATK Germline Pipeline 을 사용하며 간혹 과거 Samtools Mpileup, 이외 Illumina Strelka Germline Calling 파이프라인 등을 사용 합니다.

기본적인 컨셉은 모든 샘플을 Calling 한 후 Multisample VCF 로 제작하여 분석 하여 SNV 에 대한 조사, Gene 에 대한 조사 및 보고를 합니다. 보통 통계적인 기법이 추가로 들어가 Filtering 을 하며 Network 분석을 하는 경우도 있습니다.  추가적으로 Genome Wide Association 분석을 하는 경우도 있습니다.(어떤 SNP 이 Phenotype 과 연관을 가지는지 통계적인 파워를 조사)

Germline 분석 후 연구 목적에 맞춰 maf 값 < 0.05, 0.05 > maf > 0.95 : 일반질환(GWAS) 또는 국가별 population 비교 연구의 경우 conserved 한 snp 을 찾어 비교

참)[https://docs.gdc.cancer.gov/Data/Bioinformatics_Pipelines/DNA_Seq_Variant_Calling_Pipeline/](https://docs.gdc.cancer.gov/Data/Bioinformatics_Pipelines/DNA_Seq_Variant_Calling_Pipeline/)

####

#### 2. Case-Control : Cancer Study, Rare Disease Study

Mutect2 / Caveman 파이프라인 등 두 개의 bam 파일을 비교 분석 할수 있는 파이프라인 사용\
\


#### 3.  Trio 분석 : 1번 분석의 확장이며 가계도 중 유전적으로 희귀질환 환자가 있는 경우 

Germline 분석 후 VCF 를 합쳐 Manual 분석, 일반적으로 maf 값 0.01 이하의 mutation 을 검사.\


### 4. 대표 파이프라인 소개

### 1. GATK4 Germline 파이프라인 

참고) [https://gatk.broadinstitute.org/hc/en-us/categories/360002302312](https://gatk.broadinstitute.org/hc/en-us/categories/360002302312)

#### 간략) FASTQ -> QC된 FASTQ -> BAM 파일 MAPPING -> MarkDuplicate(PCR 여부) -> Base Quality Score Recalibration(BQSR)-> Haplotype Calling -> Variants Calling -> 추가분

![](<../../.gitbook/assets/image (3).png>)

###

### 2. GATK4 Mutect 파이프라

#### 간략) FASTQ -> QC된 FASTQ -> BAM 파일 MAPPING -> MarkDuplicate(PCR 여부) -> Base Quality Score Recalibration(BQSR)-> N-T Paired 또는 Normal 환자군 수가 되는 경우 PoN 제작 또는 Tumor Sample 만 있는 경우 공개된 PoN 사용(보증X)  -> 추가분석

![](<../../.gitbook/assets/image (48).png>)

### 5. 기타

Exome 및 Target Seq은 최종 분석 후 bed 또는 gff 를 이용하여 초기 실험 선정시 선택하지 않은 region 을 제거해야 합니다. Short reads 의 특성상 Genome 전체에 mapping 을 하는 구조이기 때문에  목표하지 않는 부분에도 reads 가 붙는 가능성이 높습니다. 

해당 reads 는 False Positive 일 가능성이 크기 때문에 제거를 하나 Exon 에 가깝게 붙은 Variants 의 경우 경험적인 판단이 필요할 수 있습니다.

상용 Exome Kit 의 경우 회사별로 bed 파일이 제공되는 경우가 일반적이며 Exome kit 마다 Target 으로 하는 Coverage Size 가 다르기 때문에 Reads Depth 선정이 잘되어야 합니다.

![](<../../.gitbook/assets/image (83).png>)



참고.)[https://www.frontiersin.org/articles/10.3389/fgene.2020.00082/full](https://www.frontiersin.org/articles/10.3389/fgene.2020.00082/full)

참고.)[https://www.biostars.org/p/282022/](https://www.biostars.org/p/282022/)

참고.)[https://2wordspm.com/2019/08/26/exome-sequencing%EC%9D%84-%EC%9C%84%ED%95%B4-%EA%B3%A0%EB%A0%A4%ED%95%A0-%EC%9A%94%EC%86%8C%EB%93%A4-capture-kit%EC%99%80-target-coverage-%EC%84%A0%ED%83%9D/](https://2wordspm.com/2019/08/26/exome-sequencing%EC%9D%84-%EC%9C%84%ED%95%B4-%EA%B3%A0%EB%A0%A4%ED%95%A0-%EC%9A%94%EC%86%8C%EB%93%A4-capture-kit%EC%99%80-target-coverage-%EC%84%A0%ED%83%9D/)



\[분석방법 파이프라인]

참고.) [http://bigd.big.ac.cn/gvm/analysis_standards](http://bigd.big.ac.cn/gvm/analysis_standards)\


\[Calling 방법]\
참고.) [https://www.pnas.org/content/114/40/E8320](https://www.pnas.org/content/114/40/E8320) 









### 주요 결과물

1. BAM : Mapping 파일
2. VCF : SNP/INDEL/CNV(Copy Number Variation)/SV(Structure Variation)/
3. MAF : Cancer 용 VCF의 변
4. 기타 : Annotation 된 데이터 (VCF내부 또는 따로 분리)\
