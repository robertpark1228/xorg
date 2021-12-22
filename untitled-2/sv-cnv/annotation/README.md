# Annotation

[https://www.lbgi.fr/AnnotSV/runjob](https://www.lbgi.fr/AnnotSV/runjob)\
[https://github.com/mobidic/knotAnnotSV](https://github.com/mobidic/knotAnnotSV)\
[https://github.com/lgmgeo/AnnotSV](https://github.com/lgmgeo/AnnotSV)\
[https://www.researchgate.net/publication/351810134\_AnnotSV\_and\_knotAnnotSV\_a\_web\_server\_for\_human\_structural\_variations\_annotations\_ranking\_and\_analysis](https://www.researchgate.net/publication/351810134\_AnnotSV\_and\_knotAnnotSV\_a\_web\_server\_for\_human\_structural\_variations\_annotations\_ranking\_and\_analysis)

java -jar /disk1/oneomics\_analysis/alpha\_pipeline/modules/snpEff/SnpSift.jar extractFields ./share.vcf CHROM POS ID REF\[0] ALT\[0] FILTER\[0] AF\[0] AC DP MQ ANN\[0].ALLELE ANN\[0].EFFECT ANN\[0].IMPACT ANN\[0].GENE ANN\[0].GENEID ANN\[0].FEATURE ANN\[0].FEATUREID ANN\[0].BIOTYPE ANN\[0].RANK ANN\[0].HGVS\_C ANN\[0].HGVS\_P ANN\[0].CDNA\_POS ANN\[0].CDNA\_LEN ANN\[0].CDS\_POS ANN\[0].CDS\_LEN ANN\[0].AA\_POS ANN\[0].AA\_LEN ANN\[0].DISTANCE ANN\[0].ERRORS > ./share.vcf.tsv



