# Page 1

 포스팅에서는 NGS 기법을 이용하여 microbiome에 대하여 어떤 분석을 수행할 수 있는지에 대해 이야기를 해보고자 합니다.\
\
 \
\
Microbiome\
\
우선 microbiome 이라 함은 미생물 (microbe)와 생태계 (biome)을 합친말로, 특정 환경 내에 서식하는 미생물 집단 전체의 유전체 총합을 의미합니다. 환경에 존재하는 미생물 균총의 조성을 파악하여 환경의 특성을 파악하거나, 장내 미생물 균총의 조성을 통해 질병/형질과 미생물 사이의 연관성을 알아보는 등 다양한 방면에서 microbiome 분석이 수행되고 있습니다.\
\
 \
\
\
\
인체 장내 미생물은 약 40조개로,\
사람의 장내에서 장내세포와 다양한 상호작용을 하고 있습니다.\
\
 \
\
\
\
\
\
Microbiome 분석 방법 종류\
\
방대한 microbiome의 유전정보를 파악하기 위해서는 대량, 병렬로 유전정보를 생산할 수 있는NGS 기반의 플랫폼을 사용합니다. 이때, 연구목적, \
\
비용, 샘플 종류 등에 따라서 다양한 방법으로 유전 정보를 생산할 수 있는데요. 현재 크게 3가지 정도의 분석 방법이 보편적으로 사용되고 있습니다.\
\




![](<../../.gitbook/assets/3 (1).png>)

\
\
\
대표적인 microbiome 분석 방법 개요도\
(Knight, Rob, et al. "Best practices for analysing microbiomes."\
Nature Reviews Microbiology (2018) )\
\
\
\
\
\
A.     Amplicon Sequencing (Marker gene)\
\
\
\
ü  앰플리콘 시퀀싱은 DNA서열에서 특정 유전자 부위를 PCR을 통해 증폭시키고, 그 서열부위를 통해 미생물 균총을 확인하는 방법입니다. 일반적으로 미생물의 경우 16S ribosomal RNA 유전자가 마커로서 많이 사용되어, 16S rRNA sequencing이라고도 많이 불립니다 (fungi의 경우, ITS라고 하는 DNA의 특정 부위를 마커로서 사용하기도 합니다). 16S rRNA의 일정부분을 PCR을 통해 증폭하고, 얻어지는 amplicon을 이용하여 시퀀싱을 진행하게 되고, 이후 생물정보학적 분석을 수행하게 됩니다.\
\
\
\
ü  16S rRNA 시퀀싱 데이터 분석에서 가장 중요하고 우선적으로 다뤄져야 하는 부분은 시퀀싱 에러를 제거하는 부분입니다. Illumina 데이터의 경우 에러율이 0.1% 미만으로 매우 낮다고 알려져 있음에도 불구하고, 16S rRNA 특성상 1 base pair만 달라지더라도 완전히 다른 group으로 해당 서열이 annotation 될 수 있기 때문에 최대한 에러를 줄이는 방향으로 분석을 진행해야 합니다. 전통적으로는 이러한 문제를 해결하기 위해 97% 이상의 유사도를 가지는 서열들을 하나로 묶어서 OTU (Operational Taxonomy Unit) 라고 하는 하나의 새로운 단위를 정의하는 Clustering 방법으로 이러한 에러를 해결하였습니다.\
\
\
\
ü  하지만 최근에는 에러를 줄이면서도 정밀하게 sequence 변이를 찾아낼 수 있는 알고리즘 (DADA2, Debular 등)의 발달로 인해 OTU 대신 sOTU (sub-OTU) 혹은 ASV (Amplicon Sequence Variant) 라는 개념이 생겨나게 되었고, 이를 이용해 분석을 수행하는 추세입니다.\
\
\
\
ü  이렇게 얻어낸 sOTU 혹은 ASV는 데이터베이스를 이용한 분류작업을 통해 어떠한 미생물군에 속하는지 알게 됩니다. 이때, 어떠한 데이터베이스를 이용하여 분석을 수행할 것인지와 어떠한 분류방법을 선택할 것인지에 따라 결과 양상이 상이할 수 있습니다.\
\
\
\
ü  이렇게 미생물 군집을 식별한 이후에는 alpha diversity, beta diversity 등 여러가지 분석을 통해 미생물 군집의 다양성을 분석하게 됩니다.\
\
\
\
ü  해당 방법은 16S rRNA 일정부분만을 이용해 분석이 진행되기 때문에, 다른 방법에 비해 비교적 sample prep 및 분석이 빠르고, 쉽고, 저렴하다는 장점이 있습니다. 또한, 거대한 public 데이터베이스가 존재하여 이를 이용한 손쉬운 분류군 확립을 수행할 수 있습니다.\
\
\
\
ü  하지만, 속 수준까지의 분류가 최선 (종 수준까지 내려가게 되면 정확도가 많이 하락한다고 알려져 있습니다)이며, 기능적 정보나 다른 유전체 내 변이 정보는 얻어낼 수 없고 오직 균총 분포에 대한 정보만 얻어낼 수 있다는 단점이 있습니다.\
\
\
\
\
\
\
\
\
\
B.      Whole Metagenome (Shotgun metagenome) analysis\
\
\
\
ü  Shotgun metagenome 방식은 특정 샘플 내에 모든 미생물 유전체 정보를 시퀀싱하는 방법으로서, 메타유전체 (metagenome)를 shearing 과정을 통해 잘게 조각내기 때문에 이렇게 이름 붙여졌습니다.\
\
\
\
ü  분석방법은 크게 두 가지가 존재합니다. 첫번째는 생산된 read를 미리 구축된 유전자 데이터베이스에 비교해보아 어떠한 분류군에 속하는지를 알아내는 방법입니다. 주로 k-mer에 기반한 방법을 많은 분석툴에서 사용하고 있으며, kraken, MEGAN, HUMAnN2 와 같은 것들이 이에 해당합니다. 해당 분석방법은 데이터베이스를 기반으로 진행되기 때문에 이미 밝혀져 있는 서열정보에 대해서는 비교적 정확한 정보를 얻을 수 있지만, 밝혀지지 않은 서열에 대해서는 분석이 어렵다는 단점이 있습니다.\
\
\
\
ü  또 다른 방법은, 짧은 read들을 여러 개 이어붙여서 contig 수준으로 sequence를 생성한 후 annotation 및 하위 분석을 수행하는 것입니다 (Assembly-based). 존재하는 데이터베이스와 무관하게 긴 서열을 만들어내는 것이기 때문에, 아직 밝혀지지 않은 서열에 대한 기능을 annotation 수행할 수 있다는 장점이 있지만, biodiversity가 높고 균총 분포가 너무 불규칙한 경우 assembly가 제대로 수행되지 않을 수 있다는 단점이 있습니다.\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
![](<../../.gitbook/assets/6 (1).png>)\
Shotgun metagenome 분석 흐름도 (Quince, Christopher, et al. "Shotgun metagenomics, from sampling to analysis." Nature biotechnology 35.9 (2017): 833.)\
\
\
\
\
\
ü  충분한 sequencing depth가 생산되었다는 가정하에, 해당 분석방법은 유전자의 상대적인 양을 파악할 수 있으며, species 및 strain level 까지의 분류도 가능하다는 것이 장점입니다. 또한, assembly에 기반한 방법의 경우 신규 유전자를 발굴해낼 수 있다는 것도 이점이 될 수 있습니다.\
\
\
\
ü  하지만, 상대적으로 16S rRNA amplicon sequencing에 비해 가격이 비싸고, 분석에 많은 시간이 소요될 수 있습니다.\
\
B.      Metatranscriptome analysis\
\
\
\
ü  Metatransciprome 분석은 RNA-sequencing을 이용하여 microbiome의 유전자 발현 정보를 profiling하고, 기능적 유전자가 어떤 것들이 있는지 파악할 수 있는 방법입니다.\
\
\
\
ü  분석 방법은 위의 shotgun metagenome 방법과 마찬가지로 K-mer 기반 profiling 방법 / Assembly 기반 방법 두 가지가 주요 분석 방법으로 사용되고 있습니다.\
\
\
\
ü  앞서 언급한 두 방법과는 다르게 RNA를 이용하기 때문에, 실제 활성화된 유전자에 대한 정보를 얻을 수 있어서 실제 기능적 정보를 파악할 수 있으며, 사균 등에 의한 bias를 방지할 수 있습니다.\
\
\
\
ü  하지만, Shotgun Metagenome 방법과 마찬가지로 가격이 비싸며, RNA의 경우 degradation의 가능성이 DNA에 비해 훨씬 높다는 것이 단점입니다.\
\
 \
\
 \
\
\
\
총 3가지 microbiome 분석 방법에 대하여 알아보았는데요, 연구자의 실험 디자인에 맞게 이들 세 가지를 모두 이용하여 분석하거나,\
\
 혹은 한 두가지만을 취사선택하여 분석할 수 있습니다.\
\
\
\
\
\
어려우신 점이나 궁금하신 점이 있으신 경우 언제든지 댓글로 문의 부탁드립니다.\
\
 \
\
감사합니다.\
\
 \
\
 \
\
\
\
\
\
\
\
\
\
 \
\
 \
\
\#Microbiome\
\#NGS\
\#미생물\
\#생태계\
\#WGS\
\#PCR\
\#sequence\
\#16srRNA\
\#Markergene\
\#metagenome
