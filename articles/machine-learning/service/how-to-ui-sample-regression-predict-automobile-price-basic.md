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
ms.openlocfilehash: a91f1446d8aab3db36499a9b5707d48d387b6081
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71131564"
---
# <a name="sample-1---regression-predict-price"></a>Örnek 1-gerileme: Fiyat tahmini

Visual Interface 'i kullanarak tek bir kod satırı yazmadan Machine Learning regresyon modeli oluşturmayı öğrenin.

Bu denemede, marka, model, horseüs ve boyut gibi teknik özelliklere göre bir otomobil 'nin fiyatını tahmin etmek için bir **karar verme ormanı gerileme** yapılır. "Ne kadar?" sorusunu yanıtlamaya çalıştığınız için Bu, regresyon sorunu olarak adlandırılır. Bununla birlikte, regresyon, sınıflandırma, kümeleme gibi her türlü makine öğrenimi sorununu çözmek için bu örnekteki temel adımları uygulayabilirsiniz.

Eğitim makinesi öğrenimi modelinin temel adımları şunlardır:

1. Verileri alma
1. Verileri önceden işleme
1. Modeli eğitme
1. Modeli değerlendirme

İşte çalışdığımız denemenin son, tamamlanmış grafiği aşağıdadır. Kendi kendinize benzer kararlar verebilmeniz için tüm modüller için daha fazla şey sağlayacağız.

![Deneme grafiği](media/how-to-ui-sample-regression-predict-automobile-price-basic/overall-graph.png)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Örnek 1 deneme için **Aç** düğmesini seçin:

    ![Denemeyi açın](media/how-to-ui-sample-regression-predict-automobile-price-basic/open-sample1.png)

## <a name="get-the-data"></a>Verileri alma

Bu örnek, UCI Machine Learning deposundan olan **otomobil fiyat verileri (ham)** veri kümesini kullanır. Veri kümesi, marka, model, Fiyat, araç özellikleri (silindir sayısı gibi), MPG ve bir sigorta risk puanı dahil olmak üzere otomobil 'ler hakkında bilgi içeren 26 sütun içerir. Bu denemenin hedefi, arabasının fiyatını tahmin etmek için kullanılır.

## <a name="pre-process-the-data"></a>Verileri önceden işleme

Ana veri hazırlama görevleri, veri temizleme, tümleştirme, dönüştürme, azaltma ve ayırma ya da değerleştirme dahil olmak üzere. Görsel arabirimde, bu işlemleri gerçekleştirmek için modüller ve sol paneldeki **veri dönüştürme** grubundaki diğer veri ön işleme görevlerini bulabilirsiniz.

Birçok eksik değeri olan normalleştirilmiş zararları hariç tutmak için **veri kümesinde sütunları seçme** modülünü kullanın. Daha sonra eksik değerleri olan satırları kaldırmak için **eksik verileri temizle** seçeneğini kullanın. Bu, temiz bir eğitim verisi kümesi oluşturulmasına yardımcı olur.

![Verileri önceden işleme](./media/how-to-ui-sample-regression-predict-automobile-price-basic/data-processing.png)

## <a name="train-the-model"></a>Modeli eğitme

Machine Learning sorunları farklılık gösterir. Ortak makine öğrenimi görevleri, her biri farklı bir algoritma gerektirebilecek sınıflandırma, kümeleme, gerileme ve öneren sistemleri içerir. Algoritma seçiminiz genellikle kullanım durumunun gereksinimlerine bağlıdır. Bir algoritma seçtikten sonra, daha doğru bir modeli eğitmek için parametrelerini ayarlamanız gerekir. Daha sonra doğruluk, intelligibility ve verimlilik gibi ölçümlere göre tüm modelleri değerlendirmelisiniz.

Bu denemenin hedefi, otomobil fiyatlarını tahmin etmek ve etiket sütunu (fiyat) gerçek sayılar içerdiğinden, regresyon modeli iyi bir seçimdir. Özellik sayısının nispeten küçük (100 ' den az) olduğunu ve bu özelliklerin seyrek olmadığı düşünüldüğünde, karar sınırının doğrusal olması olasıdır. Bu nedenle, bu deneme için **karar ormanı gerileme** kullanıyoruz.

Eğitim veri kümesinin orijinal verilerin% 70 ' i içermesi ve test veri kümesinin özgün verilerin% 30 ' ü içermesi için, **verileri ayırmak Için bölünmüş veri** modülünü kullanın.

## <a name="test-evaluate-and-compare"></a>Test edin, değerlendirin ve karşılaştırın

Modeli eğitme ve test etmek için modeli geliştirmek ve test etmek için veri kümesini bölme ve farklı veri kümeleri kullanma.

Model eğitilirken, **puan modelini** kullanabilir ve tahmin edilen sonuçlar oluşturmak ve modelleri değerlendirmek için **model modüllerini değerlendirin** .

**Puan modeli** , eğitilen modeli kullanarak test veri kümesi için tahminler oluşturur. Sonucu denetlemek için, **puan modelinin** çıkış bağlantı noktasını seçin ve ardından **Görselleştir**' i seçin.

![Puan sonucu](./media/how-to-ui-sample-regression-predict-automobile-price-basic/score-result.png)

Değerlendirme ölçümleri oluşturmak için puanları, **modeli değerlendir** modülüne geçirin. Sonucu denetlemek için, **değerlendirme modelinin** çıkış bağlantı noktasını seçin ve ardından **Görselleştir**' i seçin.

![Sonucu değerlendir](./media/how-to-ui-sample-regression-predict-automobile-price-basic/evaluate-result.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel arabirim için kullanılabilen diğer örnekleri keşfet:

- [Örnek 2-gerileme: Otomobil fiyat tahmini için algoritmaları karşılaştırın](how-to-ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Örnek 3-sınıflandırma: Kredi riskini tahmin etme](how-to-ui-sample-classification-predict-credit-risk-basic.md)
- [Örnek 4-sınıflandırma: Kredi riskini tahmin etme (maliyet duyarlı)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Örnek 5-sınıflandırma: Dalgalanma tahmin etme](how-to-ui-sample-classification-predict-churn.md)
- [Örnek 6-sınıflandırma: Uçuş gecikmelerini tahmin etme](how-to-ui-sample-classification-predict-flight-delay.md)
