---
title: Makine öğrenimi modellerinde eşitliği değerlendirme ve azaltma
titleSuffix: Azure Machine Learning
description: Machine Learning modellerinde eşitliği hakkında bilgi edinin ve Fairlearn Python paketinin fairer modellerini oluşturmanıza nasıl yardımcı olabileceğini öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: luquinta
author: luisquintanilla
ms.date: 05/02/2020
ms.openlocfilehash: c21ec0329a7b5716a00262b7422296df3afe208b
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83598608"
---
# <a name="fairness-in-machine-learning-models"></a>Makine öğrenimi modellerinde eşitliği

Machine Learning 'de eşitliği hakkında bilgi edinin ve Fairlearn açık kaynaklı Python paketinin daha dengeli modeller oluşturmanıza nasıl yardımcı olabileceğini öğrenin.

## <a name="what-is-fairness-in-machine-learning-systems"></a>Machine Learning sistemlerinde eşitliği nedir?

Yapay zeka ve makine öğrenimi sistemleri, dengeli olmayan davranışları gösterebilir. Haksız davranışı tanımlamanın bir yolu, zarar veya insanlar üzerindeki etkilerden biridir. AI sistemlerinin artmasıyla ilgili birçok tür zarar vardır. Yaygın olarak kullanılan iki tür AI:

- Ayırma zarar: bir AI sistemi, fırsatları, kaynakları veya bilgileri genişletir veya barındırır. Örneğin, bir modelin belirli bir kişi grubu arasında diğer grupların arasından iyi aday çekme işlemleri yaparken çok daha iyi bir şekilde kullanıma hazır olabileceği örnek olarak işe alma, okul adi ve ödünç verme sayılabilir.

- Hizmet kalitesi: bir AI sistemi, başka bir kişi grubu için de çalışır. Örnek olarak, bir ses tanıma sistemi, adam için yaptığı gibi kadınlar için de başarısız olabilir.

AI sistemlerindeki haksız davranışı azaltmak için, bu Harms 'yi değerlendirmeniz ve azaltmanız gerekir.

>[!NOTE]
> Eşitliği, bir Socio-Technical Challenge. Eşitliği 'in ve son işlem gibi birçok yönü, nicel eşitliği ölçümleri içinde yakalanmaz. Ayrıca, birçok nicel eşitliği ölçümü aynı anda karşılanamayacak. Amaç, insanların farklı risk azaltma stratejilerini değerlendirmesi ve sonra senaryolarına uygun olan ve bunların kendi senaryosuna uygun hale getirme olanağı sağlamaktır.

## <a name="fairness-assessment-and-mitigation-with-fairlearn"></a>Fairlearn ile eşitliği değerlendirmesi ve risk azaltma

Fairlearn, makine öğrenimi geliştiricilerinin geliştiricilerin sistemlerini değerlendirmesini ve gözlemlenen eşitliği sorunlarını azaltmasını sağlayan açık kaynaklı bir Python paketidir.

Fairlearn iki bileşene sahiptir:

- Değerlendirme panosu: bir modelin tahminlerinin farklı grupları nasıl etkilediğini değerlendirmek için bir Jupyter Not defteri pencere öğesi. Ayrıca, eşitliği ve performans ölçümlerini kullanarak birden çok modeli karşılaştırmayı de mümkün kılar.
- Risk azaltma algoritmaları: ikili sınıflandırmada ve regresyonda unfairness azaltmaya yönelik bir algoritma kümesi.

Bu bileşenler birlikte, veri bilimcilerinin ve iş liderlerinin eşitliği ve performans arasında herhangi bir denge kurmasını ve ihtiyaçlarına en uygun risk azaltma stratejisini seçmesini sağlar.

## <a name="fairness-assessment"></a>Eşitliği değerlendirmesi

Fairlearn ' de, eşitliği, **Grup eşitliği**olarak bilinen bir yaklaşım olsa da, hangi kişi gruplarının Harms 'nin risk altında olduğunu belirten bir yaklaşıma sahiptir.

Alt oluşturmalar olarak da bilinen ilgili gruplar **gizli özellikler** veya gizli öznitelikler aracılığıyla tanımlanır. Hassas özellikler bir Fairlearn tahmin aracı 'a vektör veya çağrılan bir matris olarak geçirilir `sensitive_features` . Bu terim, Grup eşitliği değerlendirmek için sistem tasarımcısının bu özelliklere duyarlı olması önerilir. En küçük bir şey, kişisel bilgiler nedeniyle bu özelliklerin gizlilik etkilerini içerip içermediğini belirtir. Ancak "hassas" sözcüğü, bu özelliklerin tahminlerde kullanılması gerektiğini göstermez.

Değerlendirme aşaması sırasında, eşitliği, ayırt eşliği ölçülerine göre belirlenir. **Ayırt edilebilir ölçümler** , modelin farklı gruplar üzerindeki davranışlarını, oranlar veya farklar olarak değerlendirebilir ve karşılaştırabilir. Fairlearn, ayırt eşliği olmayan iki ölçüm sınıfını destekler:


- Model performansında ayırıcı: Bu ölçüm kümeleri, farklı alt gruplar genelinde seçili performans ölçümünün değerlerinde ayırt eşliği (fark) hesaplar. Bazı örnekler:

  - doğruluk hızında ayırt eşliği
  - hata hızında ayırt eşliği
  - duyarlılık halinde ayırt eşliği
  - geri çağırma sırasında ayırt eşliği
  - MAE içinde ayırt eşliği
  - diğer birçok

- Seçim hızında ayırıcı: Bu ölçüm, farklı alt gruplar arasındaki seçim oranından farkı içerir. Bu, kredi onayı ücretine eşlik eden bir örnektir. Seçim oranı, her sınıftaki veri noktalarının kesiri olarak 1 (ikili sınıflandırmada) veya tahmin değerlerinin dağılımı (gerileme) anlamına gelir.

## <a name="unfairness-mitigation"></a>Unfairness azaltma

### <a name="parity-constraints"></a>Eşlik kısıtlamaları

Fairlearn, çeşitli unfairness risk azaltma algoritmaları içerir. Bu algoritmalar, tahmine Tor 'un, **eşlik kısıtlamaları** veya ölçütleri olarak adlandırılan davranışındaki bir kısıtlama kümesini destekler. Eşlik kısıtlamaları, tahmine Tor davranışının bazı yönlerini gizli özelliklerin (ör. farklı çces) tanımlanduyduğu gruplar arasında karşılaştırılabilir olmasını gerektirir. Fairlearn 'in risk azaltma algoritmaları, gözlemlenen eşitliği sorunlarını azaltmak için bu tür eşlik kısıtlamalarını kullanır.

Fairlearn, aşağıdaki tür eşlik kısıtlamalarını destekler:

|Eşlik kısıtlaması  | Amaç  |Machine Learning görevi  |
|---------|---------|---------|
|Demografik eşliği     |  Ayırma şiddemini azaltma | İkili sınıflandırma, regresyon |
|Eşitlenmiş gürültü  | Dağıtım ve hizmet kalitesi güvenliğini aşmanızı 'leri tanılayın | İkili sınıflandırma        |
|Sınırlanmış grup kaybı     |  Hizmet kalitesi güvenliğini aşmanızı 'leri azaltma | Regresyon |

### <a name="mitigation-algorithms"></a>Risk azaltma algoritmaları

Fairlearn, özelleştirmediğiniz ve azaltma unfairness azaltma algoritmaları sağlar:

- Azaltma: Bu algoritmalar standart bir siyah kutu ml tahmin aracı (ör. bir lightgbm modeli) alır ve yeniden ağırlıklı eğitim veri kümeleri dizisi kullanarak bir dizi geri çekme modeli oluşturur. Örneğin, belirli bir cinsiyet olan başvuranlar, modellerin yeniden eğitilmesi ve farklı cinsiyet gruplarının genelinde farklılıkları azaltmak için yukarı ağırlıklı veya aşağı ağırlıklı olabilir. Kullanıcılar daha sonra doğruluk (veya diğer performans ölçümü) arasında en iyi ticareti sağlayan bir model seçebilir ve genellikle iş kurallarını ve maliyet hesaplamalarını temel alan olması gerekir.  
- İşlem sonrası: Bu algoritmalar mevcut bir sınıflandırıcının yanı sıra hassas özelliği giriş olarak alır. Ardından, belirtilen eşitliği kısıtlamalarını zorlamak için sınıflandırıcının tahminiyle bir dönüşüm türetirsiniz. Eşik iyileştirmesinin en büyük avantajı, modeli yeniden eğitmesinin gerekli olmadığı için kolaylık ve esneklik sağlar. 

| Algoritma | Açıklama | Machine Learning görevi | Hassas Özellikler | Desteklenen eşlik kısıtlamaları | Algoritma türü |
| --- | --- | --- | --- | --- | --- |
| `ExponentiatedGradient` | [Bir indirimde bir Indirimleri yaklaşımında](https://arxiv.org/abs/1803.02453) açıklanan, dengeli sınıflandırmada siyah kutu yaklaşımı | İkili sınıflandırma | Kategorik | [Demografik eşliği](#parity-constraints), [eşitlenmiş gürültü](#parity-constraints) | Azaltma |
| `GridSearch` | [Bir indirimde bir azaltmada](https://arxiv.org/abs/1803.02453) tanımlanan siyah kutu yaklaşımı| İkili sınıflandırma | İkili | [Demografik eşliği](#parity-constraints), [eşitlenmiş gürültü](#parity-constraints) | Azaltma |
| `GridSearch` | Kılavuzlu [gerileme: nicel tanımları ve azaltma tabanlı algoritmalarda](https://arxiv.org/abs/1905.12843) açıklanan sınırlı grup kaybı algoritmasıyla birlikte bir ızgara arama varyantı uygulayan siyah kutu yaklaşımı | Regresyon | İkili | [Sınırlanmış grup kaybı](#parity-constraints) | Azaltma |
| `ThresholdOptimizer` | [Denetimli öğreniminde fırsatın kağıt eşitliğine](https://arxiv.org/abs/1610.02413)göre postprocessing algoritması. Bu teknik, mevcut bir sınıflandırıcı ve hassas özelliği giriş olarak alır ve belirtilen eşlik kısıtlamalarını zorlamak için sınıflandırıcının tahminiyle tek tonlu bir dönüşüm türetiliyor. | İkili sınıflandırma | Kategorik | [Demografik eşliği](#parity-constraints), [eşitlenmiş gürültü](#parity-constraints) | İşlem sonrası |

## <a name="next-steps"></a>Sonraki adımlar

- Farklı bileşenlerin nasıl kullanılacağını öğrenmek için [Fairlearn GitHub deposuna](https://github.com/fairlearn/fairlearn/) ve [örnek not defterlerine](https://github.com/fairlearn/fairlearn/tree/master/notebooks)göz atın.
- [Değişiklik gizliliği ve yatay ses paketini](concept-differential-privacy.md)kullanarak veri gizliliğini koruma hakkında bilgi edinin.