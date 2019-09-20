---
title: 'Görsel arabirim örnek #3: fiyat ve karşılaştırma algoritmalarına gerileme'
titleSuffix: Azure Machine Learning
description: Bu makalede, görsel arabirimi kullanarak tek bir kod satırı yazmadan karmaşık makine öğrenimi denemesinin nasıl oluşturulacağı gösterilmektedir. Bir otomobil 'nin teknik özelliklerine göre fiyatını tahmin etmek için birden çok regresyon modelini eğitme ve karşılaştırma hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: c40d76b87ca7437e25c567176b0309f08f3ca9f2
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71131577"
---
# <a name="sample-2---regression-predict-price-and-compare-algorithms"></a>Örnek 2-gerileme: Fiyat ve karşılaştırma algoritmalarını tahmin etme

Görsel arabirimi kullanarak tek bir kod satırı yazmadan karmaşık makine öğrenimi denemesinin nasıl oluşturulacağını öğrenin. Bu örnek, bir arabasının teknik özelliklerine göre fiyatını tahmin etmek için birden çok regresyon modelini karşılaştırır. Kendi makine öğrenimi sorunlarınızı ortadan kaldırmak için bu deneçde yapılan seçenekler için ktionale sağlıyoruz.

Machine Learning 'i kullanmaya yeni başladıysanız, bu denemenin [temel sürümüne](how-to-ui-sample-regression-predict-automobile-price-basic.md) göz atın.

Bu deneme için tamamlanan grafik aşağıda verilmiştir:

[![Deneme grafiği](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png)](media/how-to-ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Örnek 2 deneme için **Aç** düğmesini seçin:

    ![Denemeyi açın](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/open-sample2.png)

## <a name="experiment-summary"></a>Deneme Özeti

Machine Learning denemesini derlemek için aşağıdaki adımları kullanın:

1. Verileri alın.
1. Verileri önceden işleyin.
1. Modeli eğitme.
1. Modelleri test edin, değerlendirin ve karşılaştırın.

## <a name="get-the-data"></a>Verileri alma

Bu örnek, UCI Machine Learning deposundan olan **otomobil fiyat verileri (ham)** veri kümesini kullanır. Bu veri kümesi, marka, model, Fiyat, araç özellikleri (silindir sayısı gibi), MPG ve sigorta risk puanı dahil olmak üzere otomobil 'ler hakkında bilgi içeren 26 sütun içerir.

## <a name="pre-process-the-data"></a>Verileri önceden işleme

Ana veri hazırlama görevleri, veri temizleme, tümleştirme, dönüştürme, azaltma ve ayırma ya da değerleştirme dahil olmak üzere. Görsel arabirimde, bu işlemleri gerçekleştirmek için modüller ve sol paneldeki **veri dönüştürme** grubundaki diğer veri ön işleme görevlerini bulabilirsiniz.

Birçok eksik değeri olan normalleştirilmiş zararları hariç tutmak için **veri kümesinde sütunları seçme** modülünü kullanın. Daha sonra eksik değerleri olan satırları kaldırmak için **eksik verileri temizle** seçeneğini kullanıyoruz. Bu, temiz bir eğitim verisi kümesi oluşturulmasına yardımcı olur.

![Verileri önceden işleme](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Modeli eğitme

Machine Learning sorunları farklılık gösterir. Ortak makine öğrenimi görevleri, her biri farklı bir algoritma gerektirebilecek sınıflandırma, kümeleme, gerileme ve öneren sistemleri içerir. Algoritma seçiminiz genellikle kullanım durumunun gereksinimlerine bağlıdır. Bir algoritma seçtikten sonra, daha doğru bir modeli eğitmek için parametrelerini ayarlamanız gerekir. Daha sonra doğruluk, intelligibility ve verimlilik gibi ölçümlere göre tüm modelleri değerlendirmelisiniz.

Bu denemenin hedefi, otomobil fiyatlarını tahmin etmek ve etiket sütunu (fiyat) gerçek sayılar içerdiğinden, regresyon modeli iyi bir seçimdir. Özellik sayısının nispeten küçük (100 ' den az) olduğunu ve bu özelliklerin seyrek olmadığı düşünüldüğünde, karar sınırının doğrusal olması olasıdır.

Bu denemenin hedefi, otomobil fiyatlarını tahmin etmek ve etiket sütunu (fiyat) gerçek sayılar içerdiğinden, regresyon modeli iyi bir seçimdir. Özellik sayısının nispeten küçük (100 ' den az) olduğunu ve bu özelliklerin seyrek olmadığı düşünüldüğünde, karar sınırının doğrusal olması olasıdır.

Farklı algoritmaların performansını karşılaştırmak için, modellerden oluşan iki doğrusal algoritma kullanıyoruz, **karar ağacı gerileme** ve **karar ormanı gerilemesini**kullanırız. Her iki algoritmaların de değiştirebileceğiniz parametreleri vardır, ancak bu örnek bu deneme için varsayılan değerleri kullanır.

Eğitim veri kümesinin orijinal verilerin% 70 ' i içermesi ve test veri kümesinin özgün verilerin% 30 ' ü içermesi için, **verileri ayırmak Için bölünmüş veri** modülünü kullanın.

## <a name="test-evaluate-and-compare-the-models"></a>Modelleri test edin, değerlendirin ve karşılaştırın

Önceki bölümde açıklandığı gibi, modeli eğitmek ve test etmek için iki farklı rastgele seçilmiş veri kümesi kullanırsınız. Modeli eğitme ve test etmek için modeli geliştirmek ve test etmek için veri kümesini bölme ve farklı veri kümeleri kullanma.

Model eğitilirken, **puan modeli** ' ni kullanın ve tahmin edilen sonuçlar oluşturmak ve modelleri değerlendirmek için **model modüllerini değerlendirin** . **Puan modeli** , eğitilen modeli kullanarak test veri kümesi için tahminler oluşturur. Ardından değerlendirme ölçümleri oluşturmak üzere **modeli değerlendirmek** için puanları geçirin.

Bu deneyde, iki model çiftini karşılaştırmak için iki ayrı **modeli değerlendir** örneği kullanırsınız.

İlk olarak, eğitim veri kümesindeki iki algoritı karşılaştırın.
İkinci olarak, test veri kümesindeki iki algoritı karşılaştırın.

Sonuçlar şunlardır:

![Sonuçları karşılaştırın](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/result.png)

Bu sonuçlar, **Artırılmış karar ağacı gerileme** ile oluşturulan modelin, **karar ormanı gerilemesi**üzerine inşa edilen modelden daha düşük bir kök ortalama kare hatası olduğunu gösterir.

Her iki algoritmadaki eğitim veri kümesinde görülmeyen test veri kümesinden daha düşük bir hata vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel arabirim için kullanılabilen diğer örnekleri keşfet:

- [Örnek 1-gerileme: Bir otomobil fiyatını tahmin edin](how-to-ui-sample-regression-predict-automobile-price-basic.md)
- [Örnek 3-sınıflandırma: Kredi riskini tahmin etme](how-to-ui-sample-classification-predict-credit-risk-basic.md)
- [Örnek 4-sınıflandırma: Kredi riskini tahmin etme (maliyet duyarlı)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Örnek 5-sınıflandırma: Dalgalanma tahmin etme](how-to-ui-sample-classification-predict-churn.md)
- [Örnek 6-sınıflandırma: Uçuş gecikmelerini tahmin etme](how-to-ui-sample-classification-predict-flight-delay.md)
