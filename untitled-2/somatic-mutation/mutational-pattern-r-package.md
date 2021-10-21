---
description: Mutational Pattern R Package
---

# Mutational Pattern R Package

```
library(MutationalPatterns)
library("BSgenome.Hsapiens.UCSC.hg38")
library(SomaticSignatures)
library(BSgenome.Hsapiens.UCSC.hg19)
library(ggplot2)
library(Cairo)
library("gridExtra")

ref_genome <- "BSgenome.Hsapiens.UCSC.hg38"
vcf_files <- list.files("C:/Users/rober/Desktop/P-Serise_DATA/WGRS/SigProfiler/Case_Control_Mut", pattern="vcf$", full.names=TRUE)
sample_names <- c("CDDO5nm.p33","CDDO5nm.p41","CDDO5nm.p51","Control.p33","Control.p41","Control.p51")
sample_group <- c(rep("CDDO5nm", 3), rep("Control", 3))
grl <- read_vcfs_as_granges(vcf_files, sample_names, ref_genome)
#DINUC
dbs<- read_vcfs_as_granges(vcf_files, sample_names, ref_genome, type= "dbs")

type_occurrences <- mut_type_occurrences(grl, ref_genome)

#GRAPH
p1 <- plot_spectrum(type_occurrences,indv_points = TRU)
#CPGISLAND
p2 <- plot_spectrum(type_occurrences, CT = TRUE)
p3 <- plot_spectrum(type_occurrences, CT = TRUE, 
                    indv_points = TRUE, legend = TRUE)

grid.arrange(p1, p2, p3, ncol = 3, widths = c(3, 3, 1.75))
plot_spectrum(type_occurrences, CT = TRUE, legend = TRUE, indv_points = TRUE)
plot_spectrum_region(type_occurrences_region, indv_points = TRUE)
plot_spectrum_region(type_occurrences_region, mode = "relative_sample")
plot_lesion_segregation(grl[1:2])

#
p4 <- plot_spectrum(type_occurrences, by = sample_group, CT = TRUE, legend = TRUE,indv_points = TRUE)
p5 <- plot_spectrum(type_occurrences, CT = TRUE, 
                    legend = TRUE, error_bars = "stdev",indv_points = TRUE)
grid.arrange(p4, p5, ncol = 2, widths = c(4, 2.3))

plot_profile_heatmap(mut_mat_ext_context, by = sample_group)
plot_river(mut_mat_ext_context[,c(1,4)])


plot_spectrum_region(type_occurrences, error_bars = "stdev",indv_points = TRUE, legend = TRUE, by = sample)




#DBS 분석
dbs_grl <- get_dbs_context(dbs_grl)
dbs_counts <- count_dbs_contexts(dbs_grl)
plot_main_dbs_contexts(dbs_counts, same_y = TRUE)
```

## 설명

[https://bioconductor.org/packages/release/bioc/vignettes/MutationalPatterns/inst/doc/Introduction\_to\_MutationalPatterns.html](https://bioconductor.org/packages/release/bioc/vignettes/MutationalPatterns/inst/doc/Introduction\_to\_MutationalPatterns.html)

[https://bioconductor.org/packages/release/bioc/vignettes/MutationalPatterns/inst/doc/Introduction\_to\_MutationalPatterns.html](https://bioconductor.org/packages/release/bioc/vignettes/MutationalPatterns/inst/doc/Introduction\_to\_MutationalPatterns.html)







[https://bioconductor.org/packages/release/bioc/html/MutationalPatterns.html](https://bioconductor.org/packages/release/bioc/html/MutationalPatterns.html)
