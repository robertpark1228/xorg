# Metagenome

### 1\) 16s Amplicon Sequencing : DADA2 파이프라인

```text
library("dada2")
#DUAL
fnFs <- c("C:/Users/rober/Desktop/X201SC21081623-Z01-F001/Rawdata/PRM12_0041/PRM12_0041_raw_1.fq.gz")
fnRs <- c("C:/Users/rober/Desktop/X201SC21081623-Z01-F001/Rawdata/PRM12_0041/PRM12_0041_raw_2.fq.gz")

#QC
plotQualityProfile(fnFs[1:2])
plotQualityProfile(fnRs[1:2])

#
filtFs <- file.path("C:/Users/rober/Desktop/X201SC21081623-Z01-F001/Rawdata/PRM12_0041/PRM12_0041_raw_1.filtered.fq.gz")
filtRs <- file.path("C:/Users/rober/Desktop/X201SC21081623-Z01-F001/Rawdata/PRM12_0041/PRM12_0041_raw_2.filtered.fq.gz")



#truncLen QC 레포트 참고해서 수치 바꿔야함(Fwd,Rev)
out <- filterAndTrim(fnFs, filtFs, fnRs, filtRs, truncLen=c(240,240),
                     maxN=0, maxEE=c(2,2), truncQ=1, rm.phix=TRUE,
                     compress=TRUE, multithread=TRUE) # On Windows set multithread=FALSE


#Error Rate
errF <- learnErrors(filtFs, multithread=TRUE)
errR <- learnErrors(filtRs, multithread=TRUE)

plotErrors(errF, nominalQ=TRUE)
plotErrors(errR, nominalQ=TRUE)

dadaFs <- dada(filtFs, err=errF, multithread=TRUE)
dadaRs <- dada(filtRs, err=errR, multithread=TRUE)

mergers <- mergePairs(dadaFs, filtFs, dadaRs, filtRs, verbose=TRUE)
seqtab <- makeSequenceTable(mergers)

seqtab.nochim <- removeBimeraDenovo(seqtab, method="consensus", multithread=TRUE, verbose=TRUE)

getN <- function(x) sum(getUniques(x))

track <- cbind(out, getN(dadaFs), getN(dadaRs), getN(mergers), rowSums(seqtab.nochim))
colnames(track) <- c("input", "filtered", "denoisedF", "denoisedR", "merged", "nonchim")
rownames(track) <- sample.names
#silva_16s reference
#https://www.nature.com/articles/s41598-020-60564-8
#https://benjjneb.github.io/dada2/tutorial.html
#SILVA REference
#https://benjjneb.github.io/dada2/training.html
taxa <- assignTaxonomy(seqtab.nochim, "C:/Users/rober/Desktop/X201SC21081623-Z01-F001/silva_nr_v132_train_set.fa.gz", multithread=TRUE)
taxa <- addSpecies(taxa,"C:/Users/rober/Desktop/X201SC21081623-Z01-F001/silva_species_assignment_v132.fa.gz") 

taxa.print <- taxa 
rownames(taxa.print) <- NULL
write.csv(taxa.print,"C:/Users/rober/Desktop/X201SC21081623-Z01-F001/Rawdata/PRM12_0041_result.csv")



load("C:/Users/rober/Desktop/X201SC21081623-Z01-F001/SILVA_SSU_r138_2019.RData")

ranks <- c("domain", "phylum", "class", "order", "family", "genus", "species")

```

[https://www.bioconductor.org/packages/release/bioc/html/DECIPHER.html](https://www.bioconductor.org/packages/release/bioc/html/DECIPHER.html)

[https://www.frontiersin.org/files/Articles/644662/fmicb-12-644662-HTML/image\_m/fmicb-12-644662-g001.jpg](https://www.frontiersin.org/files/Articles/644662/fmicb-12-644662-HTML/image_m/fmicb-12-644662-g001.jpg)

[https://benjjneb.github.io/dada2/index.html](https://benjjneb.github.io/dada2/index.html)

[https://www.nature.com/articles/nbt.3935/figures/1](https://www.nature.com/articles/nbt.3935/figures/1)

[http://bioinformatics-ca.github.io/analysis\_of\_metagenomic\_data\_module6\_lab\_2016/](http://bioinformatics-ca.github.io/analysis_of_metagenomic_data_module6_lab_2016/)

[https://currentprotocols.onlinelibrary.wiley.com/doi/epdf/10.1002/cpz1.59](https://currentprotocols.onlinelibrary.wiley.com/doi/epdf/10.1002/cpz1.59)

{% embed url="https://github.com/ablab/spades/blob/spades\_3.15.3/README.md" %}

{% embed url="http://gensoft.pasteur.fr/docs/diamond/0.8.29/diamond\_manual.pdf" %}









