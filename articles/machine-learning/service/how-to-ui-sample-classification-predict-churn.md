---
title: 'Görsel arabirim örnek #5: Karmaşıklığın tahmin edilmesi için sınıflandırma + uygulama-satış'
titleSuffix: Azure Machine Learning
description: Bu görsel arabirim örnek denemesi, müşteri ilişkileri yönetimi (CRM) için ortak bir görev olan ikili sınıflandırıcı kullanımını gösterir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: 260d94ddf2572979e819ee89dfcbd315ef3c4769
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71131941"
---
# <a name="sample-5---classification-predict-churn-appetency-and-up-selling"></a>Örnek 5-sınıflandırma: Dalgalanma, uygulama ve BT satışı tahmin etme 

Görsel arabirimi kullanarak tek bir kod satırı yazmadan karmaşık makine öğrenimi denemesinin nasıl oluşturulacağını öğrenin.

Bu deneme, müşteri ilişkileri yönetimi (CRM) sistemlerine yönelik genel görevleri tahmin etmek için üç, **iki sınıf önceden artırılmış karar ağacı** sınıflandırıcıları sağlar: dalgalanma, uygulama ve yukarı satış. Veri değerleri ve Etiketler birden çok veri kaynağına bölünür ve Anonimleştir müşteri bilgilerine göre karıştırır, ancak veri kümelerini birleştirmek ve karıştırılmış değerleri kullanarak bir modeli eğmek için görsel arabirimi kullanmaya devam edebilirsiniz.

"Bir tane mi?" sorusunu yanıtlamaya çalıştığınız için Buna bir sınıflandırma sorunu denir, ancak gerileme, sınıflandırma, kümeleme gibi her türlü makine öğrenimi sorununu çözmek için bu projede aynı mantığı uygulayabilirsiniz.

Bu deneme için tamamlanan grafik aşağıda verilmiştir:

![Deneme grafiği](./media/how-to-ui-sample-classification-predict-churn/experiment-graph.png)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Örnek 5 denemenin **Aç** düğmesini seçin.

    ![Denemeyi açın](media/how-to-ui-sample-classification-predict-churn/open-sample5.png)

## <a name="data"></a>Data

Bu deneme için veri, KDD Kupa 2009 ' den. 50.000 satır ve 230 Özellik sütunları vardır. Bu görev, bu özellikleri kullanan müşteriler için dalgalanma, uygulama ve BT satışı tahmin etmeye yöneliktir. Veriler ve görev hakkında daha fazla bilgi için, bkz. [kdd Web sitesi](https://www.kdd.org/kdd-cup/view/kdd-cup-2009).

## <a name="experiment-summary"></a>Deneme Özeti

Bu görsel arabirim örnek denemesi, müşteri ilişkileri yönetimi (CRM) için ortak bir görev olan değişim, uygulama ve iş satışı için ikili sınıflandırıcı tahminini gösterir.

İlk olarak, bazı basit veri işleme yapın.

- Ham veri kümesi çok sayıda eksik değer içeriyor. Eksik değerleri 0 ile değiştirmek için **Temizleme eksik veri** modülünü kullanın.

    ![Veri kümesini Temizleme](./media/how-to-ui-sample-classification-predict-churn/cleaned-dataset.png)

- Özellikler ve karşılık gelen dalgalanma, uygulama ve en çok satılan Etiketler farklı veri kümelerinde bulunur. Etiket sütunlarını Özellik sütunlarına eklemek için **sütun Ekle** modülünü kullanın. İlk sütun olan **Sütun1**, etiket sütunudur. Sütunların geri kalanı, **var1**, **Var2**vb., özellik sütunlarıdır.

    ![Sütun veri kümesini ekleme](./media/how-to-ui-sample-classification-predict-churn/added-column1.png)

- Veri kümesini eğitme ve test kümelerine bölmek için **verileri Böl** modülünü kullanın.

    Daha sonra, tahmin modellerini derlemek için varsayılan parametrelerle birlikte, daha önce artırılmış karar ağacı ikili sınıflandırıcısını kullanın. Her görev için bir model oluşturun, diğer bir deyişle, her biri için bir model oluşturun ve bunları tahmin edin, etkili bir şekilde ve karmaşıklığı tahmin edin.

## <a name="results"></a>Sonuçlar

Test kümesinde modelin performansını görmek için **modeli değerlendir** modülünün çıkışını görselleştirin. Yukarı satış görevi için ROC eğrisi, modelin rastgele bir modelden daha iyi olduğunu gösterir. Eğrinin altındaki alan (AUC) 0,857 ' dir. 0,5 eşiğinden duyarlık 0,7, geri çağırma ise 0,463 ve F1 puanı 0,545.

![Sonuçları değerlendirin](./media/how-to-ui-sample-classification-predict-churn/evaluate-result.png)

 **Eşik** kaydırıcısını taşıyabilir ve ikili sınıflandırma görevi için ölçüm değişikliğini görebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel arabirim için kullanılabilen diğer örnekleri keşfet:

- [Örnek 1-gerileme: Bir otomobil fiyatını tahmin edin](how-to-ui-sample-regression-predict-automobile-price-basic.md)
- [Örnek 2-gerileme: Otomobil fiyat tahmini için algoritmaları karşılaştırın](how-to-ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Örnek 3-sınıflandırma: Kredi riskini tahmin etme](how-to-ui-sample-classification-predict-credit-risk-basic.md)
- [Örnek 4-sınıflandırma: Kredi riskini tahmin etme (maliyet duyarlı)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Örnek 6-sınıflandırma: Uçuş gecikmelerini tahmin etme](how-to-ui-sample-classification-predict-flight-delay.md)
