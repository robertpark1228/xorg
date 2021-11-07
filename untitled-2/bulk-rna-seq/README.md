---
description: 분석 위주
---

# \[RNA]Bulk RNA Seq

## RNASeq 소개

RNA 분석은 매년 다양한 방법이 소개되고 있으며, 1년에 분석 파이프라인(분석하는 전략)이 수십개씩 쏟아져 나오고 있는 상황 입니다. 상대적으로 적은 용량에 적당한 PC가 있으면 분석이 가능하며, 클라우드에 업로드도 용이하여 NGS 분석 중 그나마 Workload 가 작은 분석 방식 입니다.\
\
이는 물론 Resequencing 이 가능한 Human 한정이며, 다른 포유류, 식물등은 Denovo 방식을 고려해야 하기 때문에 분석이 쉬운 편은 아닙니다.  &#x20;

RNA 분석의 경우 통계를 기반으로 다양한 이론이 소개되고 있으며, 분석법 역시 많이 출시가 되어 있으나 \
결국 기초 자료로 활용되는 GENE 대비 RAW COUNT 자료 이며, 대표적으로 활용되는 DESeq2 / EdgeR 등에서는 Raw Count 만을 활용 합니다.\
\


[https://pubmed.ncbi.nlm.nih.gov/22988256/](https://pubmed.ncbi.nlm.nih.gov/22988256/)



|                계산방식                 |        활용모델         |                                        사용 하는 곳                                         |
| :---------------------------------: | :-----------------: | :------------------------------------------------------------------------------------: |
| <p>CPM</p><p>Counts Per Million</p> |     전체 Reads 의 수    |               <p>동일 샘플 그룹의 <br>Replicates 샘플간<br>Gene 의 발현량 비교</p><p></p>              |
|                 TPM                 | Transcript / Reads  |                 <p>같은 샘플 간  <br>같은 샘플 그룹 내 샘플에서 </p><p>Gene 의 발현 비</p>                 |
|              RPKM/FPKM              |  길이대비 normalization |                              <p>샘플 내 Gene 의 <br>발현 비교</p>                              |
|                DESeq2               |  Raw Count 기반 GLM   |              <p>샘플과 샘플의 비교<br>Gene 의 발현량 비<br><strong>DEG 분석</strong></p>              |
|                EdgeR                |        TMM 계산       | <p>샘플과 샘플/샘플 내<br>Gene 의 발현량 비 <br><strong>DEG 분석</strong></p><p><strong></strong></p> |

****



(STAR - HTSEQ COUNT.txt 자료)&#x20;

![](<../../.gitbook/assets/image (98).png>)



## RNA 분석 정리&#x20;

![](<../../.gitbook/assets/image (173).png>)

![](<../../.gitbook/assets/image (174).png>)

![](<../../.gitbook/assets/image (175).png>)

![](<../../.gitbook/assets/image (176).png>)

![](<../../.gitbook/assets/image (177).png>)

![](<../../.gitbook/assets/image (178).png>)

![](<../../.gitbook/assets/image (179).png>)

![](<../../.gitbook/assets/image (180).png>)

![](<../../.gitbook/assets/image (181).png>)

![](<../../.gitbook/assets/image (182).png>)

![](<../../.gitbook/assets/image (183).png>)

![](<../../.gitbook/assets/image (184).png>)

![](<../../.gitbook/assets/image (185).png>)

![](<../../.gitbook/assets/image (186).png>)

![](<../../.gitbook/assets/image (187).png>)

![](<../../.gitbook/assets/image (188).png>)

![](<../../.gitbook/assets/image (189).png>)

![](<../../.gitbook/assets/image (190).png>)

![](<../../.gitbook/assets/image (191).png>)



![](<../../.gitbook/assets/image (192).png>)

![](<../../.gitbook/assets/image (193).png>)

![](<../../.gitbook/assets/image (194).png>)

![](<../../.gitbook/assets/image (195).png>)

![](<../../.gitbook/assets/image (196).png>)



![](<../../.gitbook/assets/image (197).png>)

![](<../../.gitbook/assets/image (198).png>)



![](<../../.gitbook/assets/image (199).png>)

![](<../../.gitbook/assets/image (200).png>)



![](<../../.gitbook/assets/image (201).png>)

![](<../../.gitbook/assets/image (202).png>)

![](<../../.gitbook/assets/image (203).png>)

![](<../../.gitbook/assets/image (204).png>)

![](<../../.gitbook/assets/image (205).png>)

![](<../../.gitbook/assets/image (206).png>)

![](<../../.gitbook/assets/image (207).png>)

![](<../../.gitbook/assets/image (208).png>)

![](<../../.gitbook/assets/image (209).png>)

![](<../../.gitbook/assets/image (210).png>)

![](<../../.gitbook/assets/image (211).png>)

![](<../../.gitbook/assets/image (212).png>)

![](<../../.gitbook/assets/image (213).png>)

![](<../../.gitbook/assets/image (214).png>)

![](<../../.gitbook/assets/image (215).png>)

![](<../../.gitbook/assets/image (216).png>)

![](<../../.gitbook/assets/image (217).png>)

![](<../../.gitbook/assets/image (218).png>)

![](<../../.gitbook/assets/image (219).png>)

![](<../../.gitbook/assets/image (220).png>)

![](<../../.gitbook/assets/image (221).png>)



![](<../../.gitbook/assets/image (222).png>)







![](<../../.gitbook/assets/image (223).png>)

![](<../../.gitbook/assets/image (224).png>)

![](<../../.gitbook/assets/image (225).png>)

![](<../../.gitbook/assets/image (226).png>)

![](<../../.gitbook/assets/image (227).png>)



![](<../../.gitbook/assets/image (228).png>)

![](<../../.gitbook/assets/image (229).png>)

![](<../../.gitbook/assets/image (230).png>)

![](<../../.gitbook/assets/image (231).png>)



![](<../../.gitbook/assets/image (232).png>)

![](<../../.gitbook/assets/image (233).png>)

![](<../../.gitbook/assets/image (234).png>)

![](<../../.gitbook/assets/image (235).png>)

![](<../../.gitbook/assets/image (236).png>)

![](<../../.gitbook/assets/image (237).png>)

![](<../../.gitbook/assets/image (238).png>)

![](<../../.gitbook/assets/image (239).png>)

![](<../../.gitbook/assets/image (240).png>)



![](<../../.gitbook/assets/image (241).png>)

![](<../../.gitbook/assets/image (242).png>)

![](<../../.gitbook/assets/image (243).png>)

![](<../../.gitbook/assets/image (244).png>)



![](<../../.gitbook/assets/image (245).png>)



![](<../../.gitbook/assets/image (246).png>)

![](<../../.gitbook/assets/image (247).png>)

![](<../../.gitbook/assets/image (248).png>)































## 대표적인 Pipeline

![](<../../.gitbook/assets/image (74).png>)



## 분석 전략을 세울 시 참고 자료

![](<../../.gitbook/assets/image (97).png>)





## - 참고자료

1\) [https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0881-8](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0881-8)\
2\) 상기 링크 번역본\
[https://ibric.org/myboard/read.php?Board=report\&id=2776\&Page=8\&PARA9=3](https://ibric.org/myboard/read.php?Board=report\&id=2776\&Page=8\&PARA9=3)\
3\) RPKM/FPKM/기타 발현량 등 benchmark\
[https://bioinformaticsandme.tistory.com/63](https://bioinformaticsandme.tistory.com/63)\
4\) RNAseq \
[https://www.rna-seqblog.com/](https://www.rna-seqblog.com)\
\
5\) 강좌 : \
[https://www.youtube.com/watch?v=xh\_wpWj0AzM](https://www.youtube.com/watch?v=xh\_wpWj0AzM)

6\) [http://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html](http://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html)

7\)[https://hackmd.io/@NFpEogXySTuWExLvDQQHig/SkwWM4WHv#Gene-Ontology](https://hackmd.io/@NFpEogXySTuWExLvDQQHig/SkwWM4WHv#Gene-Ontology)

8\)[https://bioconductor.org/packages/release/workflows/vignettes/rnaseqGene/inst/doc/rnaseqGene.html#transcript-quantification-and-tximport-txime](https://bioconductor.org/packages/release/workflows/vignettes/rnaseqGene/inst/doc/rnaseqGene.html#transcript-quantification-and-tximport-tximeta)

9\)[https://hbctraining.github.io/DGE\_workshop\_salmon\_online/lessons/03\_DGE\_QC\_analysis.html](https://hbctraining.github.io/DGE\_workshop\_salmon\_online/lessons/03\_DGE\_QC\_analysis.html)

10\)[https://angus.readthedocs.io/en/2016/\_static/DifferentialExpressionBasics\_NGS2016\_ID.pdf](https://angus.readthedocs.io/en/2016/\_static/DifferentialExpressionBasics\_NGS2016\_ID.pdf)\


11\) [http://master.bioconductor.org/packages/release/workflows/html/rnaseqGene.html](http://master.bioconductor.org/packages/release/workflows/html/rnaseqGene.html)\
\
12\)[https://support.bioconductor.org/p/119776/](https://support.bioconductor.org/p/119776/) \
\
13\) [https://www.biostars.org/p/298272/](https://www.biostars.org/p/298272/)



