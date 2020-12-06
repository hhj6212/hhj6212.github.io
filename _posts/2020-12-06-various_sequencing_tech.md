---
layout: post
title: NGS 가 적용된 다양한 기술들을 알아봅시다!
comments: true
categories: [biology, tech]
---

안녕하세요. 한헌종입니다.
오늘은 NGS (Next generation sequencing) 가 적용된 다양한 기술들을 알아보고자 합니다.
특히 NGS 자체의 설명보다도, 어떤 종류의 데이터들이 있고 어떻게 활용되는지를 정리해봤습니다.

### NGS가 적용된 기술에는 어떤 종류가 있나요?
오늘 다루게 될 데이터를 아래와 같이 정리해봤습니다.
각 기술들이 어떤 결과를 볼 수 있고, 어떤 target molecule 들을 검출해내는지 한번 알아볼까요?

|Technologies|Result|Target molecule|
|--|--|--|
|Whole Genome Sequencing (WGS)|Genome sequence|Genomic DNA|
|Whole Exome Sequencing (WES)|Exome sequence|DNA around exon regions|
|Chromatin Immunoprecipitation Sequencing (ChIP-seq)|Protein-DNA interaction profiles|Protein-bound DNA|
|Methylated DNA Immunoprecipitation Sequencing (MeDIP-seq)|CpG methylation profiles|Methylated DNA|
|Reduced Representation Bisulfite Sequencing (RRBS-seq)|CpG methylation profiles|Methylated DNA|
|Whole Genome Bisulfite Sequencing (WGBS)|CpG methylation profiles|Methylated DNA|
|High-throughput Chromatin Conformation Capture (Hi-C)|Long-range chromatin interaction profiles|DNA-bound DNA|
|DNase I hypersensitive sites sequencing (DNase-seq)|Chromatin accesibility profiles|DNA around open chromatin regions|
|Assay for Transposase-Accessible Chromatin using sequencing (ATAC-seq)|Chromatin accesibility profiles|DNA around open chromatin regions|
|RNA Sequencing (RNA-seq)|Transcriptome profiles|mRNA|
|RNA-Immunoprecipitation Sequencing (RIP-seq)|Protein-RNA interaction profiles|protein-bound mRNA|

### 1. DNA 염기서열을 알아내고 싶을 때: WES, WGS
WES, WGS 기술은 흔히 말하는 DNA 염기서열을 알아내기 위해 사용됩니다.
새로운 종의 유전체 정보를 알아낼 때나, 환자의 염기서열에서 돌연변이를 발견하고자 할 때 쓰이죠.

이 기술에서는 샘플의 DNA 를 추출해서 cDNA library 로 만들고, 이를 NGS 기술을 통해 읽어냅니다.
WGS 는 모든 유전체 서열을 얻을 때 쓰이지만, WES 는 Exome 영역, 즉 coding region 에 있는 DNA 만을 추출해냅니다.
Exome 을 추출하기 위해서는 특정 region에만 붙는 probes 를 사용하고, 이후에 이 probes 들에게 붙어있는 bead 를 끌어당겨서 원하는 region의 DNA만 얻어내는 겁니다.

결과로 나온 fastq files 은 reference genome에 대한 alignment 단계를 거쳐 bam file 을 만들어냅니다.
보통 이런 데이터는 돌연변이를 찾기 위해서 variant calling을 진행해 vcf 파일이 만들어지죠.
혹은, 새로운 종의 데이터라면 De novo alignment 로 새로운 genome sequence를 만들 수도 있습니다.

### 2. 단백질-DNA 상호작용을 알아내고 싶을 때: ChIP-seq
ChIP-seq 기술은 특정 단백질이 붙어있는 DNA 영역을 알아낼 때 쓰입니다.
구체적인 적용은 두가지인데요, 첫째로는 Transcription factor 와 같은 DNA-binding 단백질들이 어디에서 조절작용을 하는지 알아낼 때 입니다
두번째로는 histone modification (H3K4me3, H3K27ac) 와 같은 Epigenetic modification 이 어디에 일어났는지 알아낼 때 입니다.
중요한 건 둘 모두 regulatory elements 를 알아낼 때 사용하는 기술이라는 점이죠.

이 기술에서는, 먼저 Protein-DNA binding 을 고정하기 위해 formaldehyde 를 사용합니다. 이를 cross-linking 단계라고 하죠.
이후 target protein을 특정 antibody 를 사용해 침전시킵니다. 이 단계가 immuno-precipitation 인거죠.
이렇게 원하는 protein-bound DNA 조각들이 나오면 이를 NGS 기술로 읽어냅니다.

결과로 나온 fastq files 은 마찬가지로 reference genome에 대한 alignment 단계를 거쳐 bam file 을 만들어냅니다.
이 다음으로는 peak-calling algorithm을 사용해서 어떤 영역에 단백질이 붙어있는지 알아냅니다.
그 결과로 bed 파일이 나오게 되죠.

### 3. Methylation profile 을 알아내고 싶을 때: MeDIP-seq, RRBS-seq, WGBS
다음으로 설명드릴 기술은 DNA-methylation profile 을 알아내는 기술들입니다.
DNA 의 cytosine 서열의 경우 methyl기가 붙기도 하는데요, 이런 부분들은 보통 DNA가 서로 엉겨서 inactive region 이 됩니다.
즉 주변의 유전자들이 repression, 비활성화 된다는 뜻인데요.
이렇게 DNA-methylation 작용은 유전자 조절에 중요하기 때문에, methylated sites 를 알아내기 위한 연구가 계속되어 왔습니다.
작성해놓은 기술보다 더 다양한 기술들이 많은데요, 관심있으신 분들은 [링크](https://dx.doi.org/10.1186%2Fs13072-016-0075-3)를 참조해보세요!

각 기술들의 차이를 간단히 살펴보면 다음과 같습니다.
MeDIP-seq 은 ChIP-seq 과 유사하게, anti-methylcytosine antibody 를 사용해 methylated DNA를 침전시켜 얻어냅니다.
가격이 저렴하지만, resolution 이 약 100bp 로 어떤 cytosine 이 methyl기를 가지고 있는지는 알기 힘듭니다.
RRBS-seq 의 경우, MSP1 이라는 제한효소로 DNA fragment 를 만드는데, 이러면 각 끝부분이 CpG를 갖게 됩니다.
이후 bisulfite conversion 을 통해 unmethylated C 가 Uracil 로 바뀌게 되고, 이후 cytosine 으로 남아있는 fragments를 골라 sequencing을 진행합니다.
RRBS 는 비교적 저렴하고 민감도가 높은 기술로 알려져 있지만, intergenic 부분이나 distal element region의 경우는 잘 알아내지 못할 수 있다고 하네요.
WGBS 는 Whole-genome scale 로 모든 methylated C 를 알아내는 기술입니다.
비싸다는게 흠이지만, 거의 모든 CpG site 의 methylation 상태를 알아낼 수 있다는 장점이 있죠.

### 4. 염색체의 구조를 알아내고 싶을 때: Hi-C
Hi-C 는 Chromatin Conformation Capture 기술에서 발전된 기술입니다.
이 기술을 사용하면, genome 상에서 어떤 DNA region이 또다른 DNA region과 붙어있다는 정보를 알아낼 수 있습니다.
이를 통해 염색체가 전체적으로 어떤 구조를 가지고 있는지, 조절 작용이 어떻게 일어나고 있는지를 알 수 있죠.
ChIP-seq 에서와 마찬가지로, DNA-DNA interaction 이 일어나는 부분을 formaldehyde로 고정하고 (cross-linking) 염기서열을 시퀀싱으로 알아냅니다.

이 기술은 대신, genome 상의 모든 chromatin interaction 을 얻으려 하다보니 resolution이나 depth가 좀 모자라긴 합니다.
여기서 조금 더 진보한 CHi-C 혹은 PCHi-C 는 promoter 와의 상호작용만 뽑아낸 기술입니다.
대부분의 중요한 유전자 조절이 promoter 와 작용하기 때문에, 이 부분에 집중해서 좀 더 높은 resolution 과 depth 를 얻어낼 수 있는 거죠.

### 5. Open chromatin region 을 알고 싶을 떄: DNase-seq, ATAC-seq
Chromatin accesibility 는 유전자 조절에서 중요한 역할을 합니다.
DNA 는 평소에 histone 단백질들을 감싸고 밀집된 구조로 존재하지만, 어떤 부분은 느슨하게 풀려 있습니다.
이런 부분을 open chromatin region 이라고 하는데요, 이런 영역에는 다양한 단백질들이 조절작용을 위해 몰려듭니다.
때문에 이런 영역을 알아내기 위해 DNase-seq, ATAC-seq 과 같은 기술들이 발전해왔죠.

DNase-seq 의 경우, DNase I 이라는 endonuclease 를 사용합니다.
이 때, DNase I 효소는 밀집된 지역보다 느슨한 지역에 더 쉽게 접근해 DNA 조각을 만들어냅니다. (이런 영역을 DNase I hypersensitive region 이라고도 합니다.)
이후 얻어진 조각들을 cDNA library 로 만들고 읽어내면 open chromatin region 을 알아낼 수 있는거죠.
ATAC-seq 에서는 '변형된' Tn5 라는 Transposase 를 사용합니다. 
기존 Tn5 효소와 달리, 변형된 Tn5 는 hyperactive transposase 로써 DNA 를 조각내고 adaptor 를 붙여줍니다. (tagmentation 이라는 과정입니다.)
이 기술 역시 accessible region 의 DNA 조각들을 얻어낼 수 있고, 결과적으로 open chromatin region 을 알아내게 됩니다.

결과로 나온 fastq files 은 마찬가지로 reference genome에 대한 alignment 단계를 거쳐 bam file 을 만들어냅니다.
그리고 ChIP-seq 기술과 비슷하게, peak calling algorithm을 통해 region 정보를 알아냅니다.

최근에는 ATAC-seq 기술이 더 발전되어 single-cell ATAC-seq 기술이 나타났습니다.
이 기술을 이용하면, 약 10,000개 이상의 세포가 각각 가지고 있는 open chromatin region을 알아낼 수 있게 됩니다.
정말 single cell 기술의 발전이 대단하네요...

### 6. 유전자 발현량을 알고 싶을 때: RNA-seq
RNA-seq 은 많은 연구자들이 사용하고 있는 기술이죠.
주로 유전자들의 발현량을 알고자 할 때 이 기술을 씁니다.
먼저 세포에서 mRNA 를 추출해 내는데요, 1) poly-A tail 이 있는 서열만 뽑는 방법과 2) rRNA 를 제거하는 방법 두가지가 있습니다.
이렇게 뽑힌 RNA 들을 cDNA library 로 만들고 NGS 기술로 읽어내게 됩니다.

결과로 나온 fastq files 은 마찬가지로 reference genome에 대한 alignment 단계를 거쳐 bam file 을 만들어냅니다.
이후 quantification 과정을 거치는데요, 여러 알고리즘을 통해 유전자 발현량을 FPKM, TPM, CPM 등의 숫자로 나타내게 됩니다.

최근에는 전 세계적으로 single-cell RNA-seq (scRNAseq) 기술이 유행하고 있죠.
각 세포마다 유전자의 발현량을 알아내는 기술로써, 주로 면역 항암제 발굴을 위한 연구에 쓰이고 있습니다.
적게는 몇백 개, 많게는 십만개 정도의 세포 발현량을 한번에 알아낼 수 있는 기술입니다.
이 기술을 통해 기존에는 알지 못했던 cell subtype 들과 그들의 새로운 marker를 알아낼 수 있고, cell subtype 간의 분화 과정을 관찰할 수 있게 되었습니다.

### 7. 단백질-RNA 상호작용을 알고 싶을 때: RIP-seq
RIP-seq 은 ChIP-seq 과 비슷하지만, 이번에는 DNA 가 아니라 RNA 를 target molecule 로 씁니다.
RNA에 붙는 protein은 post-transcriptional modification에 매우 중요하게 작용합니다.
때문에 연구자들은 이런 상호작용을 관찰하기 위해 RIP-seq 과 같은 기술을 개발해왔습니다.
전체적인 분석 방법은 ChIP-seq 과 동일하다고 보시면 됩니다.

---

여기까지 NGS 를 적용시킨 다양한 기술들 및 활용을 알아보았습니다.
물론 여기서 다루지 않은 기술도 매우 많습니다만, 이 글을 통해 전체적인 그림을 얻으셨길 바랍니다.
특히 관심있으신 연구에 어떤 기술을 사용할 수 있을지 고민하실 때 도움이 될거라 생각합니다.
그럼, 다음에 더 좋은 내용으로 돌아오겠습니다!