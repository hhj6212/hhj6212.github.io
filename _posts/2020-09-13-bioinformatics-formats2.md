---
layout: post
title: 파일 형식을 알아보자! 두번째 (VCF, BED, GTF, GFF)
comments: true
categories: [biology,tech]
---

### 생명정보학 분석에서 자주 만나는 파일 형식을 알아봅시다 (2)

안녕하세요, 한헌종입니다.
오늘은 저번 포스팅에 이어서 생명정보학 분야에서 자주 만나게 되는 파일 형식을 알아보려 합니다.
오늘은 VCF, BED, GTF, GFF를 알아봅시다.

### VCF (Variant Call Format)

VCF 파일은 Variants 즉 변이 정보를 기록할 때 사용되는 형식입니다.
Genome 상에서 어느 위치에 원래는 어떤 서열인데 어떤 유전변이가 일어난건지, genotype은 무엇인지에 대한 정보를 가지고 있죠.
이 VCF 파일 형식은 1000 genome project와 같이 대규모 시퀀싱 프로젝트가 진행되면서 만들어졌다고 해요.
VCF 파일은 아래와 같이 생겼습니다. 크게 두 부분으로 나뉘어져 있죠. header 와 body 입니다. 

~~~shell
##fileformat=VCFv4.3
##fileDate=20090805
##source=myImputationProgramV3.1
##reference=file:///seq/references/1000GenomesPilot-NCBI36.fasta
##contig=<ID=20,length=62435964,assembly=B36,md5=f126cdf8a6e0c7f379d618ff66beb2da,species="Homo sapiens",taxonomy=x>
##phasing=partial
##INFO=<ID=NS,Number=1,Type=Integer,Description="Number of Samples With Data">
##INFO=<ID=DP,Number=1,Type=Integer,Description="Total Depth">
##INFO=<ID=AF,Number=A,Type=Float,Description="Allele Frequency">
##INFO=<ID=AA,Number=1,Type=String,Description="Ancestral Allele">
##INFO=<ID=DB,Number=0,Type=Flag,Description="dbSNP membership, build 129">
##INFO=<ID=H2,Number=0,Type=Flag,Description="HapMap2 membership">
##FILTER=<ID=q10,Description="Quality below 10">
##FILTER=<ID=s50,Description="Less than 50% of samples have data">
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
##FORMAT=<ID=GQ,Number=1,Type=Integer,Description="Genotype Quality">
##FORMAT=<ID=DP,Number=1,Type=Integer,Description="Read Depth">
##FORMAT=<ID=HQ,Number=2,Type=Integer,Description="Haplotype Quality">
#CHROM POS      ID         REF   ALT    QUAL  FILTER   INFO                             FORMAT       NA00001         NA00002          NA00003
20     14370    rs6054257  G     A      29    PASS    NS=3;DP=14;AF=0.5;DB;H2           GT:GQ:DP:HQ  0|0:48:1:51,51  1|0:48:8:51,51   1/1:43:5:.,.
20     17330    .          T     A      3     q10     NS=3;DP=11;AF=0.017               GT:GQ:DP:HQ  0|0:49:3:58,50  0|1:3:5:65,3     0/0:41:3
20     1110696  rs6040355  A     G,T    67    PASS    NS=2;DP=10;AF=0.333,0.667;AA=T;DB GT:GQ:DP:HQ  1|2:21:6:23,27  2|1:2:0:18,2     2/2:35:4
20     1230237  .          T     .      47    PASS    NS=3;DP=13;AA=T                   GT:GQ:DP:HQ  0|0:54:7:56,60  0|0:48:4:51,51   0/0:61:2
20     1234567  microsat1  GTC   G,GTCT 50    PASS    NS=3;DP=9;AA=G                    GT:GQ:DP     0/1:35:4        0/2:17:2         1/1:40:3
~~~
*VCF 파일의 예시. header 20줄과 body 6줄로 이루어져 있군요.*

**header** 는 샵 두개 (##)으로 시작하는 VCF의 가장 윗부분을 말합니다.
header 부분은 VCF 파일의 전반적인 정보를 가지고 있습니다.
파일 형식, 날짜, 그리고 body의 INFO/FORMAT column에서 사용되는 단어들에 대한 설명이 포함되어 있죠.
예를 들어 body 의 INFO column에 있는 정보 중 NS=3 이란 정보가 있는데, header의 설명을 보면 INFO=<ID=NS, ... Description="Number of Samples With Data"> 라고 되어있으니 NS가 샘플 수라는 걸 알 수 있습니다.

**body** 부분은 샵 하나 (#)로 시작하는 컬럼 제목부분과 실제 데이터부분으로 되어있습니다.
body에는 꼭 들어가야 하는 8개의 컬럼이 있는데요, 순서대로 다음과 같습니다:
1. #CHROM: 염색체 정보
1. POS: position. 해당 염색체에서 변이가 일어난 염기서열 위치를 말합니다.
1. ID: 변이를 나타내는 고유 ID가 있는 경우 표시합니다. 보통 dbSNP (rs로 시작하는) ID를 적거나 '.' 으로만 적어둡니다.
1. REF: reference genome 상에서의 원래 염기서열
1. ALT: 돌연변이로 바뀐 서열
1. QUAL: 해당 variant를 찾아낸 것에 대한 quality score 
1. FILTER: 해당 변이가 특정 필터기준을 통과했는지에 대한 표시 (flag)
1. INFO: 각 변이에 대한 자세한 설명이 적힌 컬럼.

위 컬럼들은 모두 탭 (tab)으로 나뉘어져 있죠.
이 외의 컬럼은 없어도 VCF파일이라 할 수 있습니다. 그러나 보통 FORMAT 컬럼, sample 컬럼 하나씩이 더 추가되어 있는 경우가 많습니다.
보통 한 개의 샘플 정보가 적혀있지만, 가끔 2개 이상의 샘플 정보가 있는 경우 그만큼 컬럼 수도 늘어나게 됩니다.
1. FORMAT: 각 샘플에서 변이 정보를 나타내는 필드 종류. colon (:) 으로 나뉘어져 있습니다. 보통 genotype (GT), AD (allele depth), DP (Total read depth) 등이 적혀 있습니다.
1. sample (실제 샘플 이름이 적힘): FORMAT 에 적힌 필드들의 각 샘플에서의 수치. colon (:)으로 나뉘어져 있습니다.

### BED (Browser Extensible Data)
BED 파일 형식은 Human Genome Project 가 진행되면서 만들어진 형식이라고 해요.
bed 파일은 염색체의 특정 '영역'에 대한 정보 (annotation) 를 기록하고 사용하기 위해 만들어 졌습니다.
예를 들어, ChIP-seq 실험 결과를 통해 얻은 protein binding region 을 표시하고자 할 때 쓰일 수 있어요.
탭 (tab)으로 나뉘어져 있고 염기서열과 같이 큰 정보가 없어서 파싱하기 용이한 형식으로 널리 쓰이게 되었죠.

bed 파일은 최소 3개의 컬럼만 있으면 되는데, 이에 컬럼이 추가되어서 총 12개까지 늘어날 수 있습니다.
용도에 따라 필요한 컬럼 수가 다른거죠.
그래서 컬럼 수에 따라서 확장자 이름을 bed3, bed4, bed12 등으로 나타내기도 합니다.

~~~shell
track name=pairedReads description="Clone Paired Reads" useScore=1
chr22 1000 5000 cloneA 960 + 1000 5000 0 2 567,488, 0,3512
chr22 2000 6000 cloneB 900 - 2000 6000 0 2 433,399, 0,3601
~~~
*일반적인 bed 파일의 예시.*

bed 파일도 header 와 body 로 나뉘어져 있습니다.
**header** 는 기본적으로 track name, description 등이 적혀져 있는 한 줄입니다.
그런데 이 bed 파일이 UCSC genome browser 등에서 쓰이는 경우, header 에는 추가적인 정보가 들어가기도 합니다.
추가적인 정보는 browser에서 이 파일을 사용했을 때, 어느 위치를 보여줄 것인지, 어떤 포맷으로 보여줄지 등 다양합니다.

~~~shell
browser position chr7:127471196-127495720
browser hide all
track name="ItemRGBDemo" description="Item RGB demonstration" visibility=2 itemRgb="On"
chr7    127471196  127472363  Pos1  0  +  127471196  127472363  255,0,0
chr7    127472363  127473530  Pos2  0  +  127472363  127473530  255,0,0
~~~
*추가 정보가 header 에 들어있는 bed 파일의 예시.*

**body** 부분은 탭으로 나뉘어져 있습니다.
컬럼 수는 3개짜리 파일도 있고, 6개, 9개 등 다양합니다.
각 컬럼에는 다음과 같은 정보들이 포함되어 있습니다.
1. chrom: 염색체 정보
1. chromStart: 해당 영역의 시작서열 위치
1. chromEnd: 해당 영역의 끝서열 위치
1. name: 영역의 이름
1. score: 영역의 점수 0~1000
1. strand: DNA strand (+/-)
1. thickstart: 해당 영역이 시각화 될 때 두껍게 보여질 부분의 시작위치
1. thickend: 해당 영역이 시각화 될 때 두껍게 보여질 부분의 끝위치
1. itemRgb: 해당 영역이 시각화 될 때 나타날 색깔
1. blockCount: 해당 영역의 Exon 과 같은 block의 수
1. blockSizes: comma (,) 로 나뉘어진 블록들의 크기
1. blockStarts: comma (,) 로 나뉘어진 start 위치 목록. 이는 두번째 컬럼인 chromStart로 부터의 상대적 위치 (zero-base) 입니다.

### GFF (General Feature Format)
GFF 는 DNA/RNA/protein 서열을 설명할 때 사용되는 파일 형식입니다.
한 줄에는 한 영역 (feature) 에 대한 설명이 9개 컬럼에 걸쳐서 적혀있습니다.
아래는 그 예시입니다:
~~~shell
chr22	TeleGene	enhancer	10000000	10001000	500	+	.	touch1
chr22	TeleGene	promoter	10010000	10010100	900	+	.	touch1
~~~

각 컬럼이 어떤건지 살펴볼까요?
1. sequence: 해당 서열의 이름
1. source: 해당 feature 이 어디서 왔는지에 대한 설명 (feature를 작성한 프로그램이나 연구기관 등)
1. feature: 해당 영역의 이름 (gene 혹은 exon 등)
1. start: 해당 영역의 시작위치 (one-base)
1. end: 해당 영역의 끝 위치 (one-base)
1. score: 영역에 대한 신뢰도 점수
1. strand: '+' (forward) 혹은 '-' (reverse) 가닥
1. frame: 0,1,2 혹은 dot(.). 0은 해당 feature 의 첫 번째 서열이 reference 의 codon start 서열이라는 뜻이며, 1은 해당 feature의 두 번째 서열이 reference 의 codon start 서열이라는 뜻입니다. 만약 feature 가 exon 이 아닌 경우에는 '.' 으로 되어있어요.
1. group: feature 가 속한 group. 같은 group 인 line 들은 같은 group 값을 가집니다.

이 중 frame 컬럼은 phase라고도 불리는데, [Sequence Ontology (SO) 문서](https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md) 에 더 자세히 설명되어 있으니 한번 살펴보세요.

### GTF (Gene Transfer Format)
GTF 파일 형식은 GFF 와 매우 유사한 형식이에요.
실제로 GFF - GTF 변환도 가능하죠.
GTF 형식의 처음 8개 컬럼은 GFF 와 동일합니다.

그러나 차이점이 있습니다.
GTF 파일에는 gene, exon 등의 feature 말고도 5'UTR, 3'UTR, inter, intron 등이 포함되어 있기도 합니다.
또한, GFF 형식의 9번째 컬럼 'group'이 semicolon으로 나뉜 'attribute' 컬럼으로 바뀝니다.
각 attribute 는 해당 feature에 대한 설명이 type-value 짝을 이루어서 적혀 있어요.

9번째 컬럼만 보면 다음과 같이 되어 있습니다.
~~~shell
gene_id "ENSG00000223972"; gene_name "DDX11L1"; gene_source "havana"; gene_biotype "transcribed_unprocessed_pseudogene"; 
gene_id "ENSG00000223972"; transcript_id "ENST00000456328"; gene_name "DDX11L1"; gene_sourc e "havana"; gene_biotype "transcribed_unprocessed_pseudogene"; transcript_name "DDX11L1-002"; transcript_source "havana";
~~~

GTF 에는 해당 feature에 대해 더 자세한 설명이 적혀있는 걸 알 수 있습니다.

---

오늘 설명드린 파일들은 제가 자주 사용해오던 파일 형식들입니다.
그런데도 각 컬럼에 대한 세세한 설명까지 알고 있지는 못했던 것 같아요.
이번 기회에 공부해볼 수 있어서 좋았던 것 같습니다.

다음에는 또 다른 내용으로 포스팅 해보겠습니다.
그럼 다음에 만나요!
