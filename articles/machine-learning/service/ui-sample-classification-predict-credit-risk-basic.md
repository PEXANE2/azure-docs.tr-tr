---
title: 'Görsel arabirim örnek #3: Kredi riskini tahmin etmek için sınıflandırma'
titleSuffix: Azure Machine Learning
description: Görsel arabirimi kullanarak tek bir kod satırı yazmadan makine öğrenimi sınıflandırıcısını oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: 26e3f5f237cdd148d12f0a413eb97ed52cb84b49
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70997051"
---
# <a name="sample-3---classification-predict-credit-risk"></a>Örnek 3-sınıflandırma: Kredi riski tahmini

Görsel arabirimi kullanarak tek bir kod satırı yazmadan makine öğrenimi sınıflandırıcısını oluşturmayı öğrenin. Bu örnek kredi geçmişi, yaş ve kredi kartı sayısı gibi kredi uygulama bilgilerine göre kredi riskini (yüksek veya düşük) tahmin etmek için **iki sınıflı bir karar ağacına** sahiptir.

Soruyu "bir tane mi?" sorusunu yanıtlamaya çalışıyoruz. Bu, sınıflandırma sorunu olarak adlandırılır. Ancak, aynı temel süreci, herhangi bir makine öğrenimi sorununun, gerileme, sınıflandırma, kümeleme gibi bir tür olup olmadığını bir şekilde uygulayabilir.

Bu deneme için tamamlanan grafik aşağıda verilmiştir:

![Deneme grafiği](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Örnek 3 deneme için **Aç** düğmesini seçin:

    ![Denemeyi açın](media/ui-sample-classification-predict-credit-risk-basic/open-sample3.png)

## <a name="related-sample"></a>İlgili örnek

[Örnek 4-sınıflandırma: Kredi riski tahmini (maliyet duyarlı)](ui-sample-classification-predict-credit-risk-cost-sensitive.md) , bu deneyle aynı sorunu çözen gelişmiş bir deneme sağlar. Bir **Python betik modülünü yürütme** ve iki ikili sınıflandırma algoritmalarının performansını karşılaştırma ile _Maliyet duyarlı_ sınıflandırmanın nasıl gerçekleştirileceğini gösterir. Sınıflandırma denemeleri oluşturma hakkında daha fazla bilgi edinmek istiyorsanız buna başvurun.

## <a name="data"></a>Data

UC Irvine deposundan Almanya kredi kartı veri kümesini kullanıyoruz.
Veri kümesi 20 özellik ve 1 etiketli 1.000 örnek içerir. Her örnek bir kişiyi temsil eder. Özellikler sayısal ve kategorik özellikler içerir. Kategorik özelliklerinin anlamı için [UCI Web sitesine](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) bakın. Son sütun, kredi riskini belirten ve yalnızca iki olası değere sahip olan etikettir: yüksek kredi riski = 2 ve düşük kredi riski = 1.

## <a name="experiment-summary"></a>Deneme Özeti

Denemeyi oluşturmak için aşağıdaki adımları izliyoruz:

1. Alman kredi kartı UCı veri kümesi modülünü deneme tuvaline sürükleyin.
1. Her sütun için anlamlı adlar ekleyebilmemiz için bir **veri düzenleme** modülü ekleyin.
1. Eğitim ve test kümelerini oluşturmak için bir **bölünmüş veri** modülü ekleyin. İlk çıkış veri kümesindeki satır kesirini 0,7 olarak ayarlayın. Bu ayar, verilerin% 70 ' un, modülün sol bağlantı noktasına ve geri kalanı doğru bağlantı noktasına çıkış olacağını belirtir. Eğitim için sol veri kümesini ve test için doğru olanı kullanırız.
1. Önceden maliyetli bir karar ağacı sınıflandırıcısını başlatmak için **Iki sınıf bir önceden maliyetli karar ağacı** modülü ekleyin.
1. **Eğitim modeli** modülü ekleme. Önceki adımdan gelen sınıflandırıcının, **tren modelinin**sol giriş bağlantı noktasına bağlanmasını sağlar. Eğitim kümesini ( **bölünmüş verilerin**sol çıkış bağlantı noktası) **tren modelinin**sağ giriş bağlantı noktasına ekleyin. **Eğitme modeli** sınıflandırıcısını eğitecektir.
1. Bir **puan modeli** modülü ekleyin ve **model eğitme** modülünü buna bağlayın. Sonra test kümesini ( **bölünmüş verilerin**sağ bağlantı noktası) **puan modeline**ekleyin. **Puan modeli** tahmine göre yapılır. Tahmine dayalı ve olumlu sınıf olasılıklarına bakmak için çıkış bağlantı noktasını seçebilirsiniz.
1. **Model değerlendirme** modülünü ekleyin ve puanlanmış veri kümesini sol giriş bağlantı noktasına bağlayın. Değerlendirme sonuçlarını görmek için **modeli değerlendir** modülünün çıkış bağlantı noktasını seçin ve **Görselleştir**' i seçin.

İşte tüm deneme grafiği:

![Deneme grafiği](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="results"></a>Sonuçlar

![Sonuçları değerlendirin](media/ui-sample-classification-predict-credit-risk-basic/evaluate-result.png)

Değerlendirme sonuçlarında, modelin AUC 'nin 0,776 olduğunu görebilirsiniz. 0,5 eşiğinden duyarlık 0,621, geri çağırma ise 0,456 ve F1 puanı 0,526.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel arabirim için kullanılabilen diğer örnekleri keşfet:

- [Örnek 1-gerileme: Bir otomobil fiyatını tahmin edin](ui-sample-regression-predict-automobile-price-basic.md)
- [Örnek 2-gerileme: Otomobil fiyat tahmini için algoritmaları karşılaştırın](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Örnek 4-sınıflandırma: Kredi riskini tahmin etme (maliyet duyarlı)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Örnek 5-sınıflandırma: Dalgalanma tahmin etme](ui-sample-classification-predict-churn.md)
- [Örnek 6-sınıflandırma: Uçuş gecikmelerini tahmin etme](ui-sample-classification-predict-flight-delay.md)
