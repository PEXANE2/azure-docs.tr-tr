---
title: 'Görsel arabirim örnek #3: fiyat ve karşılaştırma algoritmalarına gerileme'
titleSuffix: Azure Machine Learning service
description: Bu makalede, görsel arabirimi kullanarak tek bir kod satırı yazmadan karmaşık makine öğrenimi denemesinin nasıl oluşturulacağı gösterilmektedir. Bir otomobil 'nin teknik özelliklerine göre fiyatını tahmin etmek için birden çok regresyon modelini eğitme ve karşılaştırma hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: d05a601196ec4a5349a0acb4763098d9716c17f5
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990013"
---
# <a name="sample-2---regression-predict-price-and-compare-algorithms"></a>Örnek 2-gerileme: Fiyat ve karşılaştırma algoritmalarını tahmin etme

Görsel arabirimi kullanarak tek bir kod satırı yazmadan karmaşık makine öğrenimi denemesinin nasıl oluşturulacağını öğrenin. Bu örnek, bir arabasının teknik özelliklerine göre fiyatını tahmin etmek için birden çok regresyon modelini karşılaştırır. Kendi makine öğrenimi sorunlarınızı ortadan kaldırmak için bu deneçde yapılan seçenekler için ktionale sağlıyoruz.

Machine Learning 'i kullanmaya yeni başladıysanız, temel bir gerileme denemesi görmek için bu deneyin [temel sürümüne](ui-sample-regression-predict-automobile-price-basic.md) göz atabilirsiniz.

Bu deneme için tamamlanan grafik aşağıda verilmiştir:

[![Deneme grafiği](media/ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png)](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Örnek 2 deneme için **Aç** düğmesini seçin:

    ![Denemeyi açın](media/ui-sample-regression-predict-automobile-price-compare-algorithms/open-sample2.png)

## <a name="experiment-summary"></a>Deneme Özeti

Bu adımları, denemeyi oluşturmak için kullanırız:

1. Verileri alın.
1. Verileri önceden işleyin.
1. Modeli eğitme.
1. Modelleri test edin, değerlendirin ve karşılaştırın.

## <a name="get-the-data"></a>Verileri alma

Bu deneyde, kullanırsınız Machine Learning deposundan olan **otomobil fiyat verileri (ham)** veri kümesini kullanacağız. Bu veri kümesi, marka, model, Fiyat, araç özellikleri (silindir sayısı gibi), MPG ve sigorta risk puanı dahil olmak üzere otomobil 'ler hakkında bilgi içeren 26 sütun içerir. Bu denemenin hedefi, bir otomobil fiyatını tahmin etmek için kullanılır.

## <a name="pre-process-the-data"></a>Verileri önceden işleme

Ana veri hazırlama görevleri, veri temizleme, tümleştirme, dönüştürme, azaltma ve ayırma ya da değerleştirme dahil olmak üzere. Görsel arabirimde, bu işlemleri gerçekleştirmek için modüller ve sol paneldeki **veri dönüştürme** grubundaki diğer veri ön işleme görevlerini bulabilirsiniz.

Bu deneyde, çok sayıda eksik değeri olan normalleştirilmiş zararları hariç tutmak için **veri kümesindeki sütunları seçme** modülünü kullanırız. Daha sonra eksik değerleri olan satırları kaldırmak için **eksik verileri temizle** seçeneğini kullanıyoruz. Bu, temiz bir eğitim verisi kümesi oluşturulmasına yardımcı olur.

![Verileri önceden işleme](media/ui-sample-regression-predict-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Modeli eğitme

Machine Learning sorunları farklılık gösterir. Ortak makine öğrenimi görevleri, her biri farklı bir algoritma gerektirebilecek sınıflandırma, kümeleme, gerileme ve öneren sistemleri içerir. Algoritma seçiminiz genellikle kullanım durumunun gereksinimlerine bağlıdır. Bir algoritma seçtikten sonra, daha doğru bir modeli eğitmek için parametrelerini ayarlamanız gerekir. Daha sonra doğruluk, intelligibility ve verimlilik gibi ölçümlere göre tüm modelleri değerlendirmelisiniz.

Bu denemenin hedefi, otomobil fiyatlarını tahmin etmek ve etiket sütunu (fiyat) gerçek sayılar içerdiğinden, regresyon modeli iyi bir seçimdir. Özellik sayısının nispeten küçük (100 ' den az) olduğunu ve bu özelliklerin seyrek olmadığı düşünüldüğünde, karar sınırının doğrusal olması olasıdır.

Farklı algoritmaların performansını karşılaştırmak için, modellerden oluşan iki doğrusal algoritma kullanıyoruz, **karar ağacı gerileme** ve **karar ormanı gerilemesini**kullanırız. Her iki algoritmaların de değiştirebileceğiniz parametreleri vardır, ancak bu deneme için varsayılan değerleri kullanırız.

**Veri ayırma** modülünü, eğitim veri kümesinin orijinal verilerin% 70 ' i içermesi ve test veri kümesinin özgün verilerin% 30 ' ü içermesi adına, giriş verilerini rastgele bölmek için kullanırız.

## <a name="test-evaluate-and-compare-the-models"></a>Modelleri test edin, değerlendirin ve karşılaştırın

Önceki bölümde açıklandığı gibi, modeli eğitmek ve test etmek için iki farklı rastgele seçilmiş veri kümesi kullanıyoruz. Modeli eğitme ve test etmek için modeli eğitmek ve test etmek üzere farklı veri kümeleri kullandık.

Model eğitilirken, **puan modelini** kullanır ve tahmin edilen sonuçlar oluşturmak ve modelleri değerlendirmek için **model modüllerini değerlendirin** . **Puan modeli** , eğitilen modeli kullanarak test veri kümesi için tahminler oluşturur. Daha sonra değerlendirme ölçümleri oluşturmak için **modeli değerlendirmek** üzere puanları geçiririz.

Bu deneyde, iki model çiftini karşılaştırmak için iki ayrı **modeli değerlendir** örneği kullanırız.

İlk olarak, eğitim veri kümesindeki iki algoritmanızı karşılaştırıyoruz.
İkincisi, test veri kümesindeki iki algoritmanızı karşılaştırıyoruz.
Sonuçlar şunlardır:

![Sonuçları karşılaştırın](media/ui-sample-regression-predict-automobile-price-compare-algorithms/result.png)

Bu sonuçlar, **Artırılmış karar ağacı gerileme** ile oluşturulan modelin, **karar ormanı gerilemesi**üzerine inşa edilen modelden daha düşük bir kök ortalama kare hatası olduğunu gösterir.

Her iki algoritmadaki eğitim veri kümesinde görülmeyen test veri kümesinden daha düşük bir hata vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel arabirim için kullanılabilen diğer örnekleri keşfet:

- [Örnek 1-gerileme: Bir otomobil fiyatını tahmin edin](ui-sample-regression-predict-automobile-price-basic.md)
- [Örnek 3-sınıflandırma: Kredi riskini tahmin etme](ui-sample-classification-predict-credit-risk-basic.md)
- [Örnek 4-sınıflandırma: Kredi riskini tahmin etme (maliyet duyarlı)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Örnek 5-sınıflandırma: Dalgalanma tahmin etme](ui-sample-classification-predict-churn.md)
- [Örnek 6-sınıflandırma: Uçuş gecikmelerini tahmin etme](ui-sample-classification-predict-flight-delay.md)
