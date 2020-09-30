---
layout: post
title: ACMG guideline 이란 무엇일까
comments: true
categories: [biology,clinical genomics]
---

### ACMG guideline 이란 무엇일까
안녕하세요. 한헌종입니다.
오늘은 임상유전학 분야에서 거의 교과서처럼 쓰이고 있는 ACMG guideline에 대해 알아보려 합니다.

---
환자의 유전 질환을 진단하려면 어디서부터 시작해야 할까요?
먼저 환자의 샘플을 채취하고, 시퀀싱 실험을 통해 환자의 DNA 염기서열을 알아내야겠죠.
그 다음 Reference genome 과 비교해 환자에게 어떤 돌연변이 (variants) 가 나타났는지 알아내야 합니다.
만약 환자에게서 기존에 알려진 유전병의 원인이 되는 변이가 발견된다면, 그 환자의 질병과 원인에 대해 알아낼 수 있겠죠.

| ![-]({{"/assets/200927/figure1.jpg"| relative_url}}) | 
|:--:| 
| *환자에게서 변이를 찾는 과정* |

문제는, 한명의 환자에게서 발견되는 돌연변이가 매우 많다는 것입니다.
Whole exome sequencing 실험을 통해 얻은 데이터를 살펴보면, 한 환자마다 약 7만~8만 개의 변이가 나타난다는 사실을 알 수 있습니다.
이렇게 많은 변이들 중에서 단 몇 개의 변이만 환자의 질병을 일으킵니다.
그럼 나머지 변이들은 뭘까요? 대부분의 변이는 질환과 상관없거나, 개인마다 그 위험성이 다르기 때문에 질병에 영향을 주지 않는 경우입니다.

그렇다면 이렇게 많은 변이들 중 '실제로' 질병을 일으키는 변이를 어떻게 찾아낼 수 있을까요?
그 방법을 고안하기 위해 각지에서 전문가들이 모였습니다:
American College of Medical Genetics and Genomics,
Association for Molecular Pathology,
College of American Pathologist 에서 말이죠.
이렇게 모인 전문가들이 고안해낸 지침서가 바로 ACMG/AMP guideline 입니다.

| ![-]({{"/assets/200927/ACMG_main.png"| relative_url}}) | 
|:--:| 
| *2015년에 출판된 ACMG/AMP guidelines 논문. 보통 줄여서 ACMG guideline 이라고 합니다.<br>DOI: 10.1038/gim.2015.30* |

### 그래서 ACMG guideline 이 뭐라구요?
한 마디로 변이의 '병원성' (pathogenicity) 를 판단하는 기준들 입니다.
물론 위의 논문을 읽어보시면, 변이 판별 외에도 사용되는 용어, 명명법, 사용하는 데이터베이스에 대한 설명, 변이를 보고할 때의 고려사항 등 세부적인 내용이 많이 포함되어 있습니다.
(이 부분들 역시 그리 간단하지가 않습니다..)
오늘은 그 내용 중에서도, 변이를 판단하는 기준에 집중해볼까요?

변이에 대한 판별은 두 단계로 나누어집니다.
**첫 번째 단계는 Evidence 를 할당하는 과정,**
**두 번째 단계는 Pathogenicity 를 구분하는 과정입니다.**
이 과정을 거치게 되면, 이중 어떤 변이가 정말로 병을 일으킬 만한지 알 수 있게 됩니다.

| ![-]({{"/assets/200927/figure2.jpg"| relative_url}}) | 

첫 번째 단계를 먼저 살펴볼까요?
어떤 변이에 대해 evidence 를 할당한다는 것은, 다시 말해서 병원성에 대한 증거를 붙여놓는 것입니다.
이를 위해서 ACMG guideline 에서는 **28 개의 '규칙 (rules)'** 들을 정해놓았습니다.

| ![-]({{"/assets/200927/figure3.jpg"| relative_url}}) | 

이 규칙들은 크게 병원성/비병원성 (Pathogenic/benign) 으로 나뉩니다.
또한 그 경중에 따라 크게 4가지로 나뉩니다. (Very strong/Strong/Moderate/Supporting)
예를 들어서, PS3 rule 은 특정 변이가 Pathogenic 하다는 것에 관련된 rule 중 하나이며, 그 강도가 Strong 으로 매우 강력합니다.
BP7 rule 은 특정 변이가 Benign 하다는 것에 관련된 규칙이고, 그 강도가 Supporting으로 약합니다.
이 규칙들에 따라서 특정 변이에 대해서 evidence 들을 할당하는 과정이 첫 번째 단계라고 할 수 있습니다.

그런데 대체 그 rule 이 뭐죠?
아래 그림에 28개 rule에 대한 설명 중 일부를 가져와봤습니다.

| ![-]({{"/assets/200927/figure4.jpg"| relative_url}}) | 

예를 들어, PS3 evidence 다음과 같은 경우에 할당한다고 하네요:
PS3: Well-established in vitro or in vivo functional studies supportive of a damaging effect on the gene or gene 
product
즉, in vitro/in vivo 실험을 통해 특정 변이가 유전자에 손상을 줄 수 있다는 연구가 제시된 경우, 그 변이에 PS3를 할당할 수 있습니다.
한 변이가 여러 규칙에 해당된다면, 여러 evidence 들을 할당할 수도 있는거죠.
이렇게 각 규칙에 따라서 변이에 여러가지 evidence 들을 적용하는 과정을 거치면, 1단계가 완성됩니다.

---

두 번째 단계를 살펴볼까요?
변이에 대해 evidence들을 할당하고 나면, 어떤 변이들은 Pathogenic evidence를 매우 많이 가지고 있을 수도 있습니다.
혹은 몇몇 강력한 evidence를 가지고 있는 변이도 있겠죠.
이 evidence를 종합적으로 판단해서 변이를 다섯 등급 중 하나로 구분합니다:
Pathogenic, Likely pathogenic, Uncertain signifiacne, Likely benign, Benign

사실 각 evidence를 정량적으로 판단할 수는 없지만, ACMG guideline에서 방법을 제시하고 있습니다.
예를 들어 Pathogenic strong evidence 를 두 개 가지고 있는 변이는 마침내 'Pathogenic' 변이로 구분될 수 있습니다.
이렇게 evidence 들의 조합으로 변이가 정말 pathogenic 인지 확인하는 과정이 바로 두 번째 단계입니다.

| ![-]({{"/assets/200927/figure5.jpg"| relative_url}}) | 
|:--:| 
| *논문에서 발췌한 Pathogenicity class 구분표 입니다.* |

---

이렇게 ACMG guidelines 의 일부분을 살펴보았습니다.
물론 이렇게 변이의 pathogenicity를 구분한 뒤에도, 아직도 매우 많은 변이가 Pathogenic 하다고 나올 수 있습니다.
또한, Pathogenic 으로 구분된 변이라고 무조건 질병을 일으킨다는 보장이 없으며, Uncertain 변이로 구분되었다고 해서 질병과 상관없다고 단정지을 수도 없습니다.
그리고 실제 진단까지 가려면 이 뿐만 아니라 환자의 증상이나 가족력 등 더 자세한 정보가 필요할 것입니다.
그럼에도 불구하고, 적어도 선택해야 할 변이의 수를 줄여줄 뿐만 아니라 그 변이들에 대한 설명을 제시해준다는 점에서 ACMG guideline은 큰 의미를 가진다고 할 수 있습니다.

오늘도 매우 어려운 내용의 겉핥기를 해보았습니다.
ACMG guidelines 는 2015년에 나온 이후로 계속 발전하고 있습니다.
향후 이런 내용에 대해서도 나중에 자세히 다뤄볼까 합니다.

그럼 여러분, 다음 글에서 만나요!
