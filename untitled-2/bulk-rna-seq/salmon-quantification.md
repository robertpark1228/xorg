# SALMON - QUANTIFICATION 방법

### 1\) 변환코드\(R\)

```text
library("tximport")
library("tidyverse")

#임의로 만든 Annotation Reference 파일 (하기 다운로드)
tx2gene_map <- read_tsv("D:/RNASEQ_SALMON_QUANTIFICATION/tx2gene_map")
samples <- read_csv("D:/RNASEQ_SALMON_QUANTIFICATION/sample.csv")
#변환작업
txi <- tximport(files = samples$quant_file, type = "salmon", tx2gene = tx2gene_map)
#샘플 명 달아주는 작업
colnames(txi$counts) <- samples$sample
#as.data.frame(txi$counts)
#파일저장
write.csv(as.data.frame(txi$counts),"D:/data.csv")
```

{% file src="../../.gitbook/assets/sample.csv" caption="예제 CSV" %}

### 2\) Transcriptome ID Vs. Gene ID

![](../../.gitbook/assets/image%20%28299%29.png)

{% file src="../../.gitbook/assets/image \(299\).png" caption="tx2gene\_map" %}

