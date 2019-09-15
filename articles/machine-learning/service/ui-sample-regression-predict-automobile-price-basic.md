---
title: 'Görsel arabirim örnek #1: tahmin fiyatına yönelik regresyon'
titleSuffix: Azure Machine Learning
description: Tek bir kod satırı yazmadan bir otomobil fiyatını tahmin etmek için bir makine öğrenimi modeli oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: 4d41efcaeb93c0843686f9e8ef1b62da5367e071
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70996544"
---
# <a name="sample-1---regression-predict-price"></a>Örnek 1-gerileme: Fiyat tahmini

Visual Interface 'i kullanarak tek bir kod satırı yazmadan Machine Learning regresyon modeli oluşturmayı öğrenin.

Bu denemede, marka, model, horseüs ve boyut gibi teknik özelliklere göre bir otomobil 'nin fiyatını tahmin etmek için bir **karar verme ormanı gerileme** yapılır. "Ne kadar?" sorusunu yanıtlamaya çalışıyoruz. Bu, regresyon sorunu olarak adlandırılır. Bununla birlikte, gerileme, sınıflandırma, kümeleme gibi her türlü makine öğrenimi sorununu çözmek için bu deneyde aynı temel adımları uygulayabilirsiniz.

Eğitim makinesi öğrenimi modelinin temel adımları şunlardır:

1. Verileri alma
1. Verileri önceden işleme
1. Modeli eğitme
1. Modeli değerlendirme

İşte çalışdığımız denemenin son, tamamlanmış grafiği aşağıdadır. Kendi kendinize benzer kararlar verebilmeniz için tüm modüller için daha fazla şey sağlayacağız.

![Deneme grafiği](media/ui-sample-regression-predict-automobile-price-basic/overall-graph.png)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Örnek 1 deneme için **Aç** düğmesini seçin:

    ![Denemeyi açın](media/ui-sample-regression-predict-automobile-price-basic/open-sample1.png)

## <a name="get-the-data"></a>Verileri alma

Bu deneyde, kullanırsınız Machine Learning deposundan olan **otomobil fiyat verileri (ham)** veri kümesini kullanacağız. Veri kümesi, marka, model, Fiyat, araç özellikleri (silindir sayısı gibi), MPG ve bir sigorta risk puanı dahil olmak üzere otomobil 'ler hakkında bilgi içeren 26 sütun içerir. Bu denemenin hedefi, arabasının fiyatını tahmin etmek için kullanılır.

## <a name="pre-process-the-data"></a>Verileri önceden işleme

Ana veri hazırlama görevleri, veri temizleme, tümleştirme, dönüştürme, azaltma ve ayırma ya da değerleştirme dahil olmak üzere. Görsel arabirimde, bu işlemleri gerçekleştirmek için modüller ve sol paneldeki **veri dönüştürme** grubundaki diğer veri ön işleme görevlerini bulabilirsiniz.

Birçok eksik değeri olan normalleştirilmiş zararları hariç tutmak için **veri kümesindeki sütunları seçme** modülünü kullanıyoruz. Daha sonra eksik değerleri olan satırları kaldırmak için **eksik verileri temizle** seçeneğini kullanıyoruz. Bu, temiz bir eğitim verisi kümesi oluşturulmasına yardımcı olur.

![Verileri önceden işleme](./media/ui-sample-regression-predict-automobile-price-basic/data-processing.png)

## <a name="train-the-model"></a>Modeli eğitme

Machine Learning sorunları farklılık gösterir. Ortak makine öğrenimi görevleri, her biri farklı bir algoritma gerektirebilecek sınıflandırma, kümeleme, gerileme ve öneren sistemleri içerir. Algoritma seçiminiz genellikle kullanım durumunun gereksinimlerine bağlıdır. Bir algoritma seçtikten sonra, daha doğru bir modeli eğitmek için parametrelerini ayarlamanız gerekir. Daha sonra doğruluk, intelligibility ve verimlilik gibi ölçümlere göre tüm modelleri değerlendirmelisiniz.

Bu denemenin hedefi, otomobil fiyatlarını tahmin etmek ve etiket sütunu (fiyat) gerçek sayılar içerdiğinden, regresyon modeli iyi bir seçimdir. Özellik sayısının nispeten küçük (100 ' den az) olduğunu ve bu özelliklerin seyrek olmadığı düşünüldüğünde, karar sınırının doğrusal olması olasıdır. Bu nedenle, bu deneme için **karar ormanı gerileme** kullanıyoruz.

**Veri ayırma** modülünü, eğitim veri kümesinin orijinal verilerin% 70 ' i içermesi ve test veri kümesinin özgün verilerin% 30 ' ü içermesi adına, giriş verilerini rastgele bölmek için kullanırız.

## <a name="test-evaluate-and-compare"></a>Test edin, değerlendirin ve karşılaştırın

 Modeli eğitme ve test etmek için modeli eğitmek ve test etmek üzere farklı veri kümeleri kullandık.

Model eğitilirken, **puan modelini** kullanır ve tahmin edilen sonuçlar oluşturmak ve modelleri değerlendirmek için **model modüllerini değerlendirin** .

**Puan modeli** , eğitilen modeli kullanarak test veri kümesi için tahminler oluşturur. Sonucu denetlemek için, **puan modelinin** çıkış bağlantı noktasını seçin ve ardından **Görselleştir**' i seçin.

![Puan sonucu](./media/ui-sample-regression-predict-automobile-price-basic/score-result.png)

Daha sonra değerlendirme ölçümleri oluşturmak için puanları **modeli değerlendir** modülüne geçiririz. Sonucu denetlemek için, **değerlendirme modelinin** çıkış bağlantı noktasını seçin ve ardından **Görselleştir**' i seçin.

![Sonucu değerlendir](./media/ui-sample-regression-predict-automobile-price-basic/evaluate-result.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel arabirim için kullanılabilen diğer örnekleri keşfet:

- [Örnek 2-gerileme: Otomobil fiyat tahmini için algoritmaları karşılaştırın](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Örnek 3-sınıflandırma: Kredi riskini tahmin etme](ui-sample-classification-predict-credit-risk-basic.md)
- [Örnek 4-sınıflandırma: Kredi riskini tahmin etme (maliyet duyarlı)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Örnek 5-sınıflandırma: Dalgalanma tahmin etme](ui-sample-classification-predict-churn.md)
- [Örnek 6-sınıflandırma: Uçuş gecikmelerini tahmin etme](ui-sample-classification-predict-flight-delay.md)
