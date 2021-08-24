# 가상화 환경 기반의 분석 시스템

* 분석 중 일부 자동화 될 수 있는 부분은 자동화 하는 것이 좋으며 Human / Mouse 의 경우 중점 연구 모델이기 때문에 래퍼런스 및 파이프라인화가 잘되어 있어 신뢰를 가지고 사용할 수 있습니다.  분석 중 자주 중복되는 부분은, 1. Raw Data Trimming 2. Data Mapping : GRCh37/GRCh38 3. Mapping 된 데이터 기반으로   1\) SNP 변이 : WGS/WES Variants Calling,   2\) 구조변이 : WGS/WES CNV\|SV Calling,   3\) RNA Variants Calling / RNA QUANTIFICATION   4\) Methylation CpG island 의 존재 유/무  등이 있습니다. 
* 연구 수행 중 가장 허들이 되는 부분은 일반적으로 Linux Script 등을 다뤄야 하는 부분이며 해당 파트의 burden 을 줄이기 위해 다양한 방법이 개발되고 있으나, 방법을 사용하기 위해 또 공부를 해야 하는 문제가 발생합니다. 
* 또한, 가상화 환경을 사용하기 위해 요구되는 지식이 설치/권한/폴더 셋팅/옵션 셋팅 같은 Informatics 툴을 다루는 지식보다 Linux System 을 다루는 방법이 요구 되기 때문에  크게 활용은 되지 못하고 있으나, 몇 가지 개념과 사용 방법만 알면 굳이 스크립트를 제작하지 않더라도 Downstream analysis 를 바로 시작할 수 있을만한 데이터를 얻을 수 있습니다.    

 

