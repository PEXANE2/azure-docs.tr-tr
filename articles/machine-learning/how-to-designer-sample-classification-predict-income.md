---
title: 'Tasarımcı: sınıflandırma, gelir tahmin örneği'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning Designer ile gelir tahmin etmek için kod içermeyen bir sınıflandırıcı oluşturun Bu örneği izleyin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.date: 12/25/2019
ms.openlocfilehash: bfae0d8eed80a88475c447a141097022fed9adff
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76311147"
---
# <a name="build-a-classifier--use-feature-selection-to-predict-income-with-azure-machine-learning-designer"></a>Azure Machine Learning tasarımcısıyla gelir tahmin etmek için özellik seçimini kullanmak & bir sınıflandırıcı oluşturma

**Tasarımcı örneği 3**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Tasarımcıyı kullanarak tek bir kod satırı yazmadan makine öğrenimi sınıflandırıcısını oluşturmayı öğrenin. Bu örnek, Yetişkin görselleştirmenizdeki geliri (> = 50K veya < = 50K) tahmin etmek için iki sınıf bir öngörme **kararı ağacı** sağlar.

Soru yanıt veriyor mu? " Bu, sınıflandırma sorunu olarak adlandırılır. Bununla birlikte, herhangi bir makine öğrenimi sorunu (gerileme, sınıflandırma, kümeleme vb.) eklemek için aynı temel süreci uygulayabilirsiniz.

Bu örnek için nihai işlem hattı grafiği aşağıda verilmiştir:

![İşlem hattının grafiği](./media/how-to-designer-sample-classification-predict-income/overall-graph.png)

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. Örnek 3 ' e tıklayarak açın.



## <a name="data"></a>Veriler

Veri kümesi 14 özellik ve bir etiket sütunu içerir. Sayısal ve kategorik dahil olmak üzere birden çok özellik türü vardır. Aşağıdaki diyagramda veri kümesinden bir alıntı gösterilmektedir: ![verileri](media/how-to-designer-sample-classification-predict-income/sample3-dataset-1225.png)



## <a name="pipeline-summary"></a>Ardışık düzen Özeti

İşlem hattını oluşturmak için aşağıdaki adımları izleyin:

1. Yetişkin Census geliri Ikili veri kümesi modülünü ardışık düzen tuvaline sürükleyin.
1. Eğitim ve test kümelerini oluşturmak için bir **bölünmüş veri** modülü ekleyin. İlk çıkış veri kümesindeki satır kesirini 0,7 olarak ayarlayın. Bu ayar, verilerin %70 ' un, modülün sol bağlantı noktasına ve geri kalanı doğru bağlantı noktasına çıkış olacağını belirtir. Eğitim için sol veri kümesini ve test için doğru olanı kullanırız.
1. PearsonCorreclation tarafından 5 özellik seçmek için **filtre tabanlı özellik seçimi** modülünü ekleyin. 
1. Önceden maliyetli bir karar ağacı sınıflandırıcısını başlatmak için **Iki sınıf bir önceden maliyetli karar ağacı** modülü ekleyin.
1. **Eğitim modeli** modülü ekleme. Önceki adımdan gelen sınıflandırıcının, **tren modelinin**sol giriş bağlantı noktasına bağlanmasını sağlar. Filtrelenmiş veri kümesini filtre tabanlı özellik seçim modülünden eğitim veri kümesi olarak bağlayın.  **Eğitme modeli** sınıflandırıcısını eğitecektir.
1. Veri kümesini test etmek için aynı dönüştürmeyi (filtrelenmiş tabanlı özellik seçimi) uygulamak için sütun Seç dönüşümünü ve dönüştürme modülünü Uygula öğesini ekleyin.
![uygulama dönüştürme](./media/how-to-designer-sample-classification-predict-income/transformation.png)
1. **Puan modeli** modülünü ekleyin ve **model eğitme** modülünü buna bağlayın. Daha sonra test kümesini (özellik seçimini test kümesine çok fazla uygulanan dönüştürme modülünün çıktısı) **puan modeline**ekleyin. **Puan modeli** tahmine göre yapılır. Tahmine dayalı ve olumlu sınıf olasılıklarına bakmak için çıkış bağlantı noktasını seçebilirsiniz.


    Bu işlem hattının iki puan modülü vardır, tahmine dayalı olmadan önce sağ taraftaki etiket sütunu hariç tutulur. Bu, Web hizmeti girişi yalnızca etiket olmayan özellikler beklediğinden gerçek zamanlı bir uç nokta dağıtımı için hazırdır. 

1. **Model değerlendirme** modülünü ekleyin ve puanlanmış veri kümesini sol giriş bağlantı noktasına bağlayın. Değerlendirme sonuçlarını görmek için **modeli değerlendir** modülünün çıkış bağlantı noktasını seçin ve **Görselleştir**' i seçin.

## <a name="results"></a>Sonuçlar

![Sonuçları değerlendirme](media/how-to-designer-sample-classification-predict-income/sample3-evaluate-1225.png)

Değerlendirme sonuçlarında, ROC, duyarlık-geri çekme ve karışıklık ölçümleri gibi eğrilerin olduğunu görebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Tasarımcı için kullanılabilen diğer örnekleri keşfet:

- [Örnek 1-gerileme: bir otomobil fiyatını tahmin edin](how-to-designer-sample-regression-automobile-price-basic.md)
- [Örnek 2-gerileme: otomobil fiyatlandırma için algoritmaları karşılaştırın](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Örnek 4-sınıflandırma: kredi riskini tahmin etme (maliyet duyarlı)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Örnek 5-sınıflandırma: karmaşıklığı tahmin etme](how-to-designer-sample-classification-churn.md)
- [Örnek 6-sınıflandırma: uçuş gecikmelerini tahmin etme](how-to-designer-sample-classification-flight-delay.md)
- [Örnek 7-metin sınıflandırması: Vikipedi SP 500 veri kümesi](how-to-designer-sample-text-classification.md)
