# Seurat Pipeline

### Single Cell 예시 1\)

```text
###LIBRARY##
#remotes::install_github(repo = 'satijalab/seurat', ref = 'develop')
library(Seurat)
library(tidyverse)
library(sleepwalk)
library(SCINA)
library(monocle3)
library(SeuratWrappers)
library(ggplot2)
library(patchwork)
library(magrittr)

#LOADING_FILE
p24<-Read10X(data.dir = "D:/KNIHsnpDATAsnpSINGLECELL/scRNASeq_p_Serise/hFSiPS1p24.parent.filtered_feature_bc_matrix/filtered_feature_bc_matrix")
p33<-Read10X(data.dir = "D:/KNIHsnpDATAsnpSINGLECELL/scRNASeq_p_Serise/p33.case.file.filtered_feature_bc_matrix")
p41<-Read10X(data.dir = "D:/KNIHsnpDATAsnpSINGLECELL/scRNASeq_p_Serise/p41_case.filtered_feature_bc_matrix")
p54<-Read10X(data.dir = "D:/KNIHsnpDATAsnpSINGLECELL/scRNASeq_p_Serise/p54.case.filtered_feature_bc_matrix")



##### Seurat -> 변환 
Sample_1_count <- CreateSeuratObject(counts = p24, project = "p24", min.cells = 3, min.features = 200)
Sample_2_count <- CreateSeuratObject(counts = p33, project = "p33", min.cells = 3, min.features = 200)
Sample_3_count <- CreateSeuratObject(counts = p41, project = "p41", min.cells = 3, min.features = 200)
Sample_4_count <- CreateSeuratObject(counts = p54, project = "p54", min.cells = 3, min.features = 200)

data<-merge(Sample_1_count,y=c(Sample_2_count,Sample_3_count,Sample_4_count),add.cell.ids = c("p24", "p33","p41","p54"))


metadata<-data@meta.data
metadata$cells <-rownames(metadata)
metadata$sample <- NA
metadata$sample[which(str_detect(metadata$cells, "^p24_"))] <- "p24"
metadata$sample[which(str_detect(metadata$cells, "^p33_"))] <- "p33"
metadata$sample[which(str_detect(metadata$cells, "^p41_"))] <- "p41"
metadata$sample[which(str_detect(metadata$cells, "^p54_"))] <- "p54"

data@meta.data<-metadata

#head(colnames(co_Lo_merge))
#table(co_Lo_merge$orig.ident)
#options(scipen=999)

##############QC
#QC/MT
grep("^MT-",rownames(data@assays$RNA@counts),value = TRUE)
data$percent.MT <- PercentageFeatureSet(data,pattern="^MT-") 
#QC / PLOT, 10%
VlnPlot(data, features=c("nCount_RNA","percent.MT")) + scale_y_log10()
#QC

#DATA EXPAND
data$Percent.Largest.Gene <- apply(
  data@assays$RNA@counts,
  2,
  function(x)(100*max(x))/sum(x)
)
#경향성
FeatureScatter(data,feature1 = "nCount_RNA", feature2 = "Percent.Largest.Gene")

as_tibble(
  data[[c("nCount_RNA","nFeature_RNA","percent.MT","Percent.Largest.Gene")]],
  rownames="Cell.Barcode"
) -> qc.metrics

qc.metrics

#QC_PLOT
qc.metrics %>%
  arrange(percent.MT) %>%
  ggplot(aes(nCount_RNA,nFeature_RNA,colour=percent.MT)) + 
  geom_point() + 
  scale_color_gradientn(colors=c("black","blue","green2","red","yellow")) +
  ggtitle("Example of plotting QC metrics") +
  geom_hline(yintercept = 750) +
  geom_hline(yintercept = 2000) 

#Mito_portion

qc.metrics %>%
  ggplot(aes(percent.MT)) + 
  geom_histogram(binwidth = 0.5, fill="yellow", colour="black") +
  ggtitle("Distribution of Percentage Mitochondrion") +
  geom_vline(xintercept = 10)

#Largest Gene

qc.metrics %>%
  ggplot(aes(Percent.Largest.Gene)) + 
  geom_histogram(binwidth = 0.7, fill="yellow", colour="black") +
  ggtitle("Distribution of Percentage Largest Gene") +
  geom_vline(xintercept = 20)



#DATA FILTERING

subset(
  data,
  nFeature_RNA>750 & 
    nFeature_RNA < 2000 & 
    percent.MT < 10 & 
    Percent.Largest.Gene < 20
) -> data

data

  NormalizeData(data, normalization.method = "LogNormalize") -> data




FeatureScatter(data,feature1 = "nCount_RNA", feature2 = "Percent.Largest.Gene")

#HouseKeeping Gene
ggplot(mapping = aes(data@assays$RNA@data["GAPDH",])) + 
  geom_histogram(binwidth = 0.05, fill="yellow", colour="black") + 
  ggtitle("GAPDH expression")


as.tibble(
  data@assays$RNA@data[,1:100]
) %>%
  pivot_longer(
    cols=everything(),
    names_to="cell",
    values_to="expression"
  ) %>%
  ggplot(aes(x=expression, group=cell)) +
  geom_density() +
  coord_cartesian(ylim=c(0,0.6), xlim=c(0,3))






tibble(
  pc95 = apply(data[["RNA"]]@data,2,quantile,0.95),
  measured = apply(data[["RNA"]]@data,2,function(x)(100*sum(x!=0))/length(x))
) -> normalisation.qc


NormalizeData(data, normalization.method = "CLR") -> data

as.tibble(
  data@assays$RNA@data[,1:100]
) %>%
  pivot_longer(
    cols=everything(),
    names_to="cell",
    values_to="expression"
  ) %>%
  ggplot(aes(x=expression, group=cell)) +
  geom_density() +
  coord_cartesian(ylim=c(0,0.6), xlim=c(0,3))



tibble(
  pc95 = apply(data[["RNA"]]@data,2,quantile,0.95),
  measured = apply(data[["RNA"]]@data,2,function(x)(100*sum(x!=0))/length(x))
) -> normalisation.qc

normalisation.qc %>% 
  ggplot(aes(x=measured,y=pc95))+
  geom_point()+
  ggtitle("Normalisation of data")



FindVariableFeatures(
  data, 
  selection.method = "vst", 
  nfeatures=500
) -> data


as_tibble(HVFInfo(data),rownames = "Gene") -> variance.data

variance.data %>% 
  mutate(hypervariable=Gene %in% VariableFeatures(data)
  ) -> variance.data

head(variance.data, n=10)

variance.data %>% 
  ggplot(aes(log(mean),log(variance),color=hypervariable)) + 
  geom_point() + 
  scale_color_manual(values=c("black","red"))


ScaleData(data,features=rownames(data)) -> data

RunPCA(data,features=VariableFeatures(data)) -> data

DimPlot(data,reduction="pca")
DimPlot(data,reduction="pca", group.by = "sample")

DimPlot(data,reduction="pca", dims=c(3,4))

ElbowPlot

DimHeatmap(data,dims=1:15, cells=500)


RunTSNE(
  data,
  dims=1:15, 
  perplexity=10
) -> data


DimPlot(data,reduction = "tsne", pt.size = 1) + ggtitle("tSNE with Perplexity 10")
DimPlot(data,reduction = "tsne", pt.size = 1, group.by = "sample") + ggtitle("tSNE with Perplexity 10")
FindNeighbors(data,dims=1:15) -> data

DimPlot(data,reduction="tsne",pt.size = 1, label = TRUE, label.size = 7)


#MONOCLE3_Time_Trajectory

data <- SplitObject(data, split.by = "orig.ident")

for (i in seq_along(data)) {
  data [[i]] <- NormalizeData(data[[i]]) %>% FindVariableFeatures()
}

features <- SelectIntegrationFeatures(data)

for (i in seq_along(along.with = data)) {
  data[[i]] <- ScaleData(data[[i]], features = features) %>% RunPCA(features = features)
}


anchors <- FindIntegrationAnchors(data, reference = c(1, 2), reduction = "rpca", dims = 1:30)
#https://rdrr.io/cran/Seurat/man/IntegrateData.html

integrated <- IntegrateData(anchors, dims = 1:30, k.weight=40)
integrated <- ScaleData(integrated)
integrated <- RunPCA(integrated)
integrated <- RunUMAP(integrated, dims = 1:30, reduction.name = "UMAP")
integrated <- FindNeighbors(integrated, dims = 1:30)
integrated <- FindClusters(integrated)
DimPlot(integrated, group.by = c("orig.ident", "ident"))

#BUG_PACTCHING?
cds <- as.cell_data_set(integrated)
cds <- cluster_cells(cds)
cds <- estimate_size_factors(cds)
cds@rowRanges@elementMetadata@listData[["gene_short_name"]] <- rownames(integrated[["RNA"]])
cds <- cluster_cells(cds)
cds<-learn_graph(cds)
#cds <- cluster_cells(cds)

plot_cells(cds, label_groups_by_cluster = FALSE, label_leaves = FALSE, label_branch_points = FALSE)

max.avp <- which.max(unlist(FetchData(integrated@assays$RNA, "AVP")))
max.avp <- colnames(integrated@assays$RNA)[max.avp]
cds <- order_cells(cds, root_cells = max.avp)
plot_cells(cds, color_cells_by = "pseudotime", label_cell_groups = FALSE, label_leaves = FALSE, 
           label_branch_points = FALSE)




```

Seurat -&gt; Monocle3 Data 변환에 버그가 있어 수정.





