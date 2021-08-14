---
description: 분석 위주
---

# Bulk RNA Seq

## RNASeq 소개

RNA 분석은 매년 다양한 방법이 소개되고 있으며, 1년에 분석 파이프라인\(분석하는 전략\)이 수십개씩 쏟아져 나오고 있는 상황 입니다. 상대적으로 적은 용량에 적당한 PC가 있으면 분석이 가능하며, 클라우드에 업로드도 용이하여 NGS 분석 중 그나마 Workload 가 작은 분석 방식 입니다.  
  
이는 물론 Resequencing 이 가능한 Human 한정이며, 다른 포유류, 식물등은 Denovo 방식을 고려해야 하기 때문에 분석이 쉬운 편은 아닙니다.   

RNA 분석의 경우 통계를 기반으로 다양한 이론이 소개되고 있으며, 분석법 역시 많이 출시가 되어 있으나   
결국 기초 자료로 활용되는 GENE 대비 RAW COUNT 자료 이며, 대표적으로 활용되는 DESeq2 / EdgeR 등에서는 Raw Count 만을 활용 합니다.





<table>
  <thead>
    <tr>
      <th style="text-align:center"></th>
      <th style="text-align:center"></th>
      <th style="text-align:center">&#xC0AC;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">
        <p>CPM</p>
        <p>Counts Per Million</p>
      </td>
      <td style="text-align:center">&#xC804;&#xCCB4; Reads &#xC758; &#xC218;</td>
      <td style="text-align:center">
        <p>&#xB3D9;&#xC77C; &#xC0D8;&#xD50C; &#xADF8;&#xB8F9;&#xC758;
          <br />Replicates &#xC0D8;&#xD50C;&#xAC04;
          <br />Gene &#xC758; &#xBC1C;&#xD604;&#xB7C9; &#xBE44;&#xAD50;</p>
        <p></p>
      </td>
    </tr>
    <tr>
      <td style="text-align:center">TPM</td>
      <td style="text-align:center">Transcript / Reads</td>
      <td style="text-align:center">
        <p>&#xAC19;&#xC740; &#xC0D8;&#xD50C; &#xAC04;
          <br />&#xAC19;&#xC740; &#xC0D8;&#xD50C; &#xADF8;&#xB8F9; &#xB0B4; &#xC0D8;&#xD50C;&#xC5D0;&#xC11C;</p>
        <p>Gene &#xC758; &#xBC1C;&#xD604; &#xBE44;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:center">RPKM/FPKM</td>
      <td style="text-align:center"></td>
      <td style="text-align:center">&#xC0D8;&#xD50C; &#xB0B4; Gene &#xC758;
        <br />&#xBC1C;&#xD604; &#xBE44;&#xAD50;</td>
    </tr>
    <tr>
      <td style="text-align:center">DESeq2</td>
      <td style="text-align:center">Raw Count &#xAE30;&#xBC18; GLM &#xC774;</td>
      <td style="text-align:center">&#xC0D8;&#xD50C;&#xACFC; &#xC0D8;&#xD50C;&#xC758; &#xBE44;&#xAD50;
        <br
        />Gene &#xC758; &#xBC1C;&#xD604;&#xB7C9; &#xBE44;
        <br /><b>DEG &#xBD84;&#xC11D;</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:center">EdgeR</td>
      <td style="text-align:center">TMM &#xC774;</td>
      <td style="text-align:center">
        <p>&#xC0D8;&#xD50C;&#xACFC; &#xC0D8;&#xD50C;/&#xC0D8;&#xD50C; &#xB0B4;
          <br
          />Gene &#xC758; &#xBC1C;&#xD604;&#xB7C9; &#xBE44;
          <br /><b>DEG &#xBD84;&#xC11D;</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
      </td>
    </tr>
  </tbody>
</table>

\*\*\*\*



\(STAR - HTSEQ COUNT.txt 자료\) 

![](../../.gitbook/assets/image%20%2898%29.png)



## RNA 분석 전략

1. 목표에 맞는 분석 법 선정. 대부분 분석의 목표는 군집-군집을 비교하는 DEG 분석







## 대표적인 Pipeline

![](../../.gitbook/assets/image%20%2827%29.png)



## 분석 전략을 세울 시 참고 자료

![](../../.gitbook/assets/image%20%2897%29.png)





## - 참고자료

1\) [https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0881-8](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0881-8)  
2\) 상기 링크 번역본  
[https://ibric.org/myboard/read.php?Board=report&id=2776&Page=8&PARA9=3](https://ibric.org/myboard/read.php?Board=report&id=2776&Page=8&PARA9=3)  
3\) RPKM/FPKM/기타 발현량 등 benchmark  
[https://bioinformaticsandme.tistory.com/63](https://bioinformaticsandme.tistory.com/63)  
4\) RNAseq   
[https://www.rna-seqblog.com/](https://www.rna-seqblog.com/)  
  
5\) 강좌 :   
[https://www.youtube.com/watch?v=xh\_wpWj0AzM](https://www.youtube.com/watch?v=xh_wpWj0AzM)

6\) [http://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html](http://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html)

7\)[https://hackmd.io/@NFpEogXySTuWExLvDQQHig/SkwWM4WHv\#Gene-Ontology](https://hackmd.io/@NFpEogXySTuWExLvDQQHig/SkwWM4WHv#Gene-Ontology)

8\)[https://bioconductor.org/packages/release/workflows/vignettes/rnaseqGene/inst/doc/rnaseqGene.html\#transcript-quantification-and-tximport-txime](https://bioconductor.org/packages/release/workflows/vignettes/rnaseqGene/inst/doc/rnaseqGene.html#transcript-quantification-and-tximport-tximeta)

9\)[https://hbctraining.github.io/DGE\_workshop\_salmon\_online/lessons/03\_DGE\_QC\_analysis.html](https://hbctraining.github.io/DGE_workshop_salmon_online/lessons/03_DGE_QC_analysis.html)

10\)[https://angus.readthedocs.io/en/2016/\_static/DifferentialExpressionBasics\_NGS2016\_ID.pdf](https://angus.readthedocs.io/en/2016/_static/DifferentialExpressionBasics_NGS2016_ID.pdf)  


11\) [http://master.bioconductor.org/packages/release/workflows/html/rnaseqGene.html](http://master.bioconductor.org/packages/release/workflows/html/rnaseqGene.html)  
  
12\)[https://support.bioconductor.org/p/119776/](https://support.bioconductor.org/p/119776/)   
  
13\) [https://www.biostars.org/p/298272/](https://www.biostars.org/p/298272/)





