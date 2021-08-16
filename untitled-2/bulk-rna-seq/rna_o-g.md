---
description: Salmon -> DEseq2 / edgeR Pipeline /
---

# RNA : Salmon-Deseq2 / edgeR

SALMON -&gt; READ COUNT 세는 방법





SALMON -&gt; DESEQ2

```text
############################
#USEFUL command
#utils::View
#fix
#display
#DESEQ2 정본 메뉴얼 
#http://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html
#basic manual
#
#  devtools::install_github('kevinblighe/EnhancedVolcano')
#install.packages("tidyverse")
#if (!requireNamespace("BiocManager", quietly = TRUE))
#  install.packages("BiocManager")
#BiocManager::install("DESeq2")
#BiocManager::install("tximport")
library("tidyverse")
library("tximport")
library("DESeq2")
#install.packages("pheatmap")
library(pheatmap)
library("tximeta")
library("dplyr")
library("ggplot2")

library("BiocParallel")
register(MulticoreParam(4))
register(SnowParam(4))
##############################ANNOTATION 다운로드#####################
library("AnnotationHub")
library("ensembldb")

##########Annotation Data Base##############################
# Connect to AnnotationHub
#ah <- AnnotationHub()

# Access the Ensembl database for organism
#ahDb <- query(ah, 
#              pattern = c("Homo sapiens", "EnsDb"), 
#              ignore.case = TRUE)

# Acquire the latest annotation files
#id <- ahDb %>%
#  mcols() %>%
#  rownames() %>%
#  tail(n = 1)


#ah_91 <- query(ah, "EnsDb.Hsapiens.v91")
#ah_91
#edb <- ah[[id]]


#tx2gene <- transcripts(edb, columns = c("tx_id_version", "gene_id", "tx_id"),
#                       return.type = "data.frame")



#샘플정의파일/Gene Annotation 파일 준비 
tx2gene_map <- read_tsv("D:/RNASEQ_SALMON_QUANTIFICATION/tx2gene_map")
#https://bioconductor.org/packages/devel/bioc/vignettes/AnnotationHub/inst/doc/AnnotationHub-HOWTO.html
##############################ANNOTATION 다운로드#####################
tx2gene_map <- read_tsv("D:/RNASEQ_SALMON_QUANTIFICATION/tx2gene_map")
samples <- read_csv("D:/RNASEQ_SALMON_QUANTIFICATION/sample.csv")
############################################################################

#txiimport->Deseq 형식으로 구조 변경
txi <- tximport(files = samples$quant_file, type = "salmon", tx2gene = tx2gene_map)
summary(txi)
#샘플 이름이 없어서 컬럼 붙여줌
colnames(txi$counts) <- samples$sample
#DESEQ2->DESEQ 데이터로 변경
#DESEQ형식으로 구조 변경
dds <- DESeqDataSetFromTximport(txi = txi, 
                                colData = samples, 
                                design = ~condition)
#DESEQ 계산
dds <- DESeq(dds)

#필터링/ 발현율 0 인 부분 필터링#####
keep <- rowSums(counts(dds)) > 1
#필터링 옵션, 적어도 3개 샘플에서 발현율 10리드 이상
#keep <- rowSums(counts(dds) >= 10) >= 3
dds <- dds[keep,]
nrow(dds)


#Standard workflow
#Note: if you use DESeq2 in published research, please cite:
  
#  Love, M.I., Huber, W., Anders, S. (2014) Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome Biology, 15:550. 10.1186/s13059-014-0550-8

#Other Bioconductor packages with similar aims are edgeR, limma, DSS, EBSeq, and baySeq.

#Case Control name 확인 results 함수에 그대로 넣음
resultsNames(dds)


#DESEQ 결과물
#coefficient 기반 log2fc 원래 값으로 진행
res <- results(dds, name="condition_Control_vs_Case")

#shrink 된 log2fc 원래 값으로 진행
res_LFC <- lfcShrink(dds, coef="condition_Control_vs_Case", type="apeglm")

summary(res)
summary(res_LFC)

#p-val 값으로 정렬
resOrdered <- res[order(res$pvalue),]


#Data transformations and visualization

vsd <- vst(dds, blind=FALSE)
assay(vsd)

rld <- rlog(dds, blind=FALSE)
head(assay(rld), 3)
#PCA
plotPCA(vsd, intgroup = c("condition","sample"))
pcaData <- plotPCA(vsd, intgroup = c( "condition","sample"), returnData = TRUE)
pcaData

percentVar <- round(100 * attr(pcaData, "percentVar"))

#https://rdrr.io/bioc/DESeq2/man/varianceStabilizingTransformation.html#:~:text=This%20function%20calculates%20a%20variance,homoskedastic%20(having%20constant%20variance%20along
ggplot(pcaData, aes(x = PC1, y = PC2, color = condition, shape = sample)) +
  geom_point(size =3) +
  xlab(paste0("PC1: ", percentVar[1], "% variance")) +
  ylab(paste0("PC2: ", percentVar[2], "% variance")) +
  coord_fixed() +
  ggtitle("PCA with VST data")

#GLM 모델 기반 PCA
library("glmpca")
gpca <- glmpca(counts(dds), L=2)
gpca.dat <- gpca$factors
gpca.dat$condition <- dds$condition
gpca.dat$sample <- dds$sample
ggplot(gpca.dat, aes(x = dim1, y = dim2, color = condition, shape = sample)) +
  geom_point(size =3) + coord_fixed() + ggtitle("glmpca - Generalized PCA")

#MDS PLOT 
mds <- as.data.frame(colData(vsd))  %>%
  cbind(cmdscale(sampleDistMatrix))
ggplot(mds, aes(x = `1`, y = `2`, color = condition, shape = sample)) +
  geom_point(size = 3) + coord_fixed() + ggtitle("MDS with VST data")

#MDS WITH VST
mdsPois <- as.data.frame(colData(dds)) %>%
  cbind(cmdscale(samplePoisDistMatrix))
ggplot(mdsPois, aes(x = `1`, y = `2`, color = condition, shape = sample)) +
  geom_point(size = 3) + coord_fixed() + ggtitle("MDS with PoissonDistances")

#
#PCA - GRAPH 
#http://www.sthda.com/english/articles/31-principal-component-methods-in-r-practical-guide/118-principal-component-analysis-in-r-prcomp-vs-princomp/

#HEATMAP
sampleDists <- dist(t(assay(rld)))
sampleDists
sampleDistMatrix <- as.matrix( sampleDists )
rownames(sampleDistMatrix) <- paste( rld$sample, rld$condition, sep = " - " )
colnames(sampleDistMatrix) <- NULL
colors <- colorRampPalette( rev(brewer.pal(9, "Blues")) )(255)
pheatmap(sampleDistMatrix,
         clustering_distance_rows = sampleDists,
         clustering_distance_cols = sampleDists,
         col = colors)



#HEATMAP 뽀아송
dds <- estimateSizeFactors(dds)
library("PoiClaClu")
poisd <- PoissonDistance(t(counts(dds)))
samplePoisDistMatrix <- as.matrix( poisd$dd )
rownames(samplePoisDistMatrix) <- paste( dds$sample, dds$condition, sep=" - " )
colnames(samplePoisDistMatrix) <- NULL
pheatmap(samplePoisDistMatrix,
         clustering_distance_rows = poisd$dd,
         clustering_distance_cols = poisd$dd,
         col = colors)


#기타 그래프
#HEATMAP_GENE
genes <- order(res_lfc$log2FoldChange, decreasing=TRUE)[1:20]
annot_col <-samples %>%
column_to_rownames('sample') %>%
dplyr::select(condition) %>% as.data.frame()

pheatmap(assay(vsd)[genes, ], cluster_rows=TRUE, show_rownames=TRUE,
         cluster_cols=FALSE, annotation_col=annot_col)



#결과물만들기
res <- results(dds)


#필요시
#filtering_part
res_sig <- subset(res, padj<.05)
res_lfc <- subset(res_sig, abs(log2FoldChange) > 1) 

#
res <- results(dds, contrast=c("condition","Control","Case"))
res_2<- results(dds, contrast = c("sample", "p23", "p24"))

mcols(res, use.names = TRUE)
summary(res)

# plot _MA
DESeq2::plotMA(res)



plotCounts(dds, gene="ABCC6", intgroup="condition")
plotCounts(dds, gene=which.min(res$padj), intgroup="condition")


vsd <- vst(dds)
# calculate sample distances
sample_dists <- assay(vsd) %>%
  t() %>%
  dist() %>%
  as.matrix() 

mdsData <- data.frame(cmdscale(sample_dists))
mds <- cbind(mdsData, as.data.frame(colData(vsd)))
plotPCA(vsd)


ggplot(mds, aes(X1, X2, shape = condition)) + 
  geom_point(size = 3) +
  theme_minimal()



d <- plotCounts(dds, gene=which.min(res$padj), intgroup="condition", 
                returnData=TRUE)
library("ggplot2")
ggplot(d, aes(x=condition, y=count)) + 
  geom_point(position=position_jitter(w=0.1,h=0)) + 
  scale_y_log10(breaks=c(25,100,400))


#기타
resApeT <- lfcShrink(dds, coef=2, type="apeglm", lfcThreshold=1)
DESeq2::plotMA(resApeT, ylim=c(-3,3), cex=.8)
abline(h=c(-1,1), col="dodgerblue", lwd=2)

#https://bioconductor.org/packages/release/bioc/vignettes/EnhancedVolcano/inst/doc/EnhancedVolcano.html
EnhancedVolcano(res,
                lab = rownames(res),
                x = 'log2FoldChange',
                y = 'pvalue',
                title = 'N061011 versus N61311',
                pCutoff = 10e-32,
                FCcutoff = 0.5,
                pointSize = 3.0,
                labSize = 6.0)
####################

genetable <- data.frame(gene.id = rownames(txi$counts))

```



[https://gettinggeneticsdone.blogspot.com/2012/09/deseq-vs-edger-comparison.html](https://gettinggeneticsdone.blogspot.com/2012/09/deseq-vs-edger-comparison.html)



[http://www.compbio.dundee.ac.uk/user/pschofield/Projects/teaching\_pg/workshops/biocDGE.html\#gc\_content](http://www.compbio.dundee.ac.uk/user/pschofield/Projects/teaching_pg/workshops/biocDGE.html#gc_content)



GO / GSEA 분석은 웹 및 Cytoscape 이용 추천





{% embed url="https://combine-lab.github.io/salmon/getting\_started/" %}



