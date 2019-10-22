---
title: 'Görsel arabirim örnek #3: kredi riskini tahmin etmek için sınıflandırma'
titleSuffix: Azure Machine Learning
description: Görsel arabirimi kullanarak tek bir kod satırı yazmadan makine öğrenimi sınıflandırıcısını oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 09/23/2019
ms.openlocfilehash: 4649303b8ee643130b8e254f01bfffbe8ad9eb2b
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693494"
---
# <a name="sample-3---classification-predict-credit-risk"></a>Örnek 3-sınıflandırma: kredi riskini tahmin etme

Görsel arabirimi kullanarak tek bir kod satırı yazmadan makine öğrenimi sınıflandırıcısını oluşturmayı öğrenin. Bu örnek işlem hattı kredi geçmişi, yaş ve kredi kartı sayısı gibi kredi uygulama bilgilerine göre kredi riskini tahmin etmek için (yüksek veya düşük) **iki sınıf bir karar ağacı** .

Soru yanıt veriyor mu? " Bu, sınıflandırma sorunu olarak adlandırılır. Ancak, aynı temel süreci, herhangi bir makine öğrenimi sorununun, gerileme, sınıflandırma, kümeleme gibi bir tür olup olmadığını bir şekilde uygulayabilir.

Bu örnek için nihai işlem hattı grafiği aşağıda verilmiştir:

![İşlem hattının grafiği](media/how-to-ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Örnek 3 ardışık düzeni için **Aç** düğmesini seçin:

    ![İşlem hattını açma](media/how-to-ui-sample-classification-predict-credit-risk-basic/open-sample3.png)

## <a name="related-sample"></a>İlgili örnek

[Örnek 4-sınıflandırma: kredi riski tahmini (maliyet duyarlı)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md) , bu örnekle aynı sorunu çözen gelişmiş bir işlem hattı sağlar. Bir **Python betik modülünü yürütme** ve iki ikili sınıflandırma algoritmalarının performansını karşılaştırma ile *Maliyet duyarlı* sınıflandırmanın nasıl gerçekleştirileceğini gösterir. Sınıflandırma işlem hatları oluşturma hakkında daha fazla bilgi edinmek istiyorsanız buna başvurun.


## <a name="data"></a>Veriler

Örnek, UC Irvine deposundaki Alman kredi kartı veri kümesini kullanır. 20 özellik ve 1 etiketle 1.000 örnek içerir. Her örnek bir kişiyi temsil eder. Özellikler sayısal ve kategorik özellikler içerir. Kategorik özelliklerinin anlamı için [UCI Web sitesine](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) bakın. Son sütun, kredi riskini belirten ve yalnızca iki olası değere sahip olan etikettir: yüksek kredi riski = 2 ve düşük kredi riski = 1.

## <a name="pipeline-summary"></a>Ardışık düzen Özeti

İşlem hattını oluşturmak için aşağıdaki adımları izleyin:

1. Alman kredi kartı UCı veri kümesi modülünü ardışık düzen tuvaline sürükleyin.
1. Her sütun için anlamlı adlar ekleyebilmemiz için bir **veri düzenleme** modülü ekleyin.
1. Eğitim ve test kümelerini oluşturmak için bir **bölünmüş veri** modülü ekleyin. İlk çıkış veri kümesindeki satır kesirini 0,7 olarak ayarlayın. Bu ayar, verilerin %70 ' un, modülün sol bağlantı noktasına ve geri kalanı doğru bağlantı noktasına çıkış olacağını belirtir. Eğitim için sol veri kümesini ve test için doğru olanı kullanırız.
1. Önceden maliyetli bir karar ağacı sınıflandırıcısını başlatmak için **Iki sınıf bir önceden maliyetli karar ağacı** modülü ekleyin.
1. **Eğitim modeli** modülü ekleme. Önceki adımdan gelen sınıflandırıcının, **tren modelinin**sol giriş bağlantı noktasına bağlanmasını sağlar. Eğitim kümesini ( **bölünmüş verilerin**sol çıkış bağlantı noktası) **tren modelinin**sağ giriş bağlantı noktasına ekleyin. **Eğitme modeli** sınıflandırıcısını eğitecektir.
1. Bir **puan modeli** modülü ekleyin ve **model eğitme** modülünü buna bağlayın. Sonra test kümesini ( **bölünmüş verilerin**sağ bağlantı noktası) **puan modeline**ekleyin. **Puan modeli** tahmine göre yapılır. Tahmine dayalı ve olumlu sınıf olasılıklarına bakmak için çıkış bağlantı noktasını seçebilirsiniz.
1. **Model değerlendirme** modülünü ekleyin ve puanlanmış veri kümesini sol giriş bağlantı noktasına bağlayın. Değerlendirme sonuçlarını görmek için **modeli değerlendir** modülünün çıkış bağlantı noktasını seçin ve **Görselleştir**' i seçin.

## <a name="results"></a>Sonuçlar

![Sonuçları değerlendirin](media/how-to-ui-sample-classification-predict-credit-risk-basic/evaluate-result.png)

Değerlendirme sonuçlarında, modelin AUC 'nin 0,776 olduğunu görebilirsiniz. 0,5 eşiğinden duyarlık 0,621, geri çağırma ise 0,456 ve F1 puanı 0,526.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel arabirim için kullanılabilen diğer örnekleri keşfet:

- [Örnek 1-gerileme: bir otomobil fiyatını tahmin edin](how-to-ui-sample-regression-predict-automobile-price-basic.md)
- [Örnek 2-gerileme: otomobil fiyatlandırma için algoritmaları karşılaştırın](how-to-ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Örnek 4-sınıflandırma: kredi riskini tahmin etme (maliyet duyarlı)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Örnek 5-sınıflandırma: karmaşıklığı tahmin etme](how-to-ui-sample-classification-predict-churn.md)
- [Örnek 6-sınıflandırma: uçuş gecikmelerini tahmin etme](how-to-ui-sample-classification-predict-flight-delay.md)
- [Örnek 7-metin sınıflandırması: kitaplar İncelemeleri](how-to-ui-sample-text-classification.md)