# STAR2 - HT-seq

*  STAR2 Expression 용 Mapping
* Reference Genome 및 Transcriptome 만들기 

```text
$ STAR --runThreadN 6 --runMode genomeGenerate --genomeDir ./genome --genomeFastaFiles ./Homo_sapiens.GRCh38.dna.primary_assembly.fa --sjdbGTFfile ./Homo_sapiens.GRCh38.104.gtf --sjdbOverhang 150

```

{% hint style="info" %}
--runThreadsN : CPU 갯수  
--runMode : 어떤 작업을 할지 genomeGenerate\(래퍼런스 제작\)   
--genomeDir : 래퍼런스 저장될 폴더 \(STAR 용\)  
--genomeFastaFiles : 래퍼런스 데이터 FASTA  
--sjdbGTF : 래퍼런스 데이터 GTF
{% endhint %}

{% hint style="info" %}
Reference Data Download:

GTF : [ftp://ftp.ensembl.org/pub/release-104/gtf/homo\_sapiens/](ftp://ftp.ensembl.org/pub/release-104/gtf/homo_sapiens/)

FASTA : [ftp://ftp.ensembl.org/pub/release-104/fasta/homo\_sapiens/dna/Homo\_sapiens.GRCh38.dna.primary\_assembly.fa.gz](ftp://ftp.ensembl.org/pub/release-104/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz)
{% endhint %}

