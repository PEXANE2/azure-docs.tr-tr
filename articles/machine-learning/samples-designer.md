---
title: Örnek tasarımcı boru hatları
titleSuffix: Azure Machine Learning
description: Makine öğrenimi boru hatlarınızı başlatmak için Azure Machine Learning tasarımcısındaki örnekleri kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 03/29/2020
ms.openlocfilehash: f9a8b0a4c51024d91e517db2f6ae10a4dba62384
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80389350"
---
# <a name="designer-sample-pipelines"></a>Tasarımcı örnek boru hatları

Kendi makine öğrenimi boru hatlarınızı oluşturmaya hızla başlamak için Azure Machine Learning tasarımcısındaki yerleşik örnekleri kullanın. Azure Machine Learning tasarımcısı [GitHub deposu,](https://github.com/Azure/MachineLearningDesigner) bazı yaygın makine öğrenimi senaryolarını anlamanıza yardımcı olacak ayrıntılı belgeler içerir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://aka.ms/AMLFree)oluşturun.
* Enterprise SKU ile Azure Machine Learning çalışma alanı.


## <a name="how-to-use-sample-pipelines"></a>Örnek boru hatları nasıl kullanılır?

Tasarımcı, örnek ardışık hatların bir kopyasını stüdyo çalışma alanınıza kaydeder. Boru hattını gereksinimlerinize uyarlamak ve kendi gereksinimleriniz gibi kaydetmek için edinebilirsiniz. Projelerinizi başlatmak için bunları başlangıç noktası olarak kullanın.

### <a name="open-a-sample-pipeline"></a>Örnek bir boru hattı açma

1. <a href="https://ml.azure.com?tabs=jre" target="_blank">ml.azure.com</a>oturum açın ve çalışmak istediğiniz çalışma alanını seçin.

1. **Tasarımcı'yı**seçin.

1. **Yeni ardışık hatlar** bölümü altında örnek bir ardışık hat lar seçin.

    Örneklerin tam listesi için **daha fazla örnek göster'i** seçin.

### <a name="submit-a-pipeline-run"></a>Bir ardışık hat lar çalıştır

Bir ardışık ardışık hattı çalıştırmak için öncelikle ardışık hattı çalıştırmak için varsayılan işlem hedefini ayarlamanız gerekir.

1. Tuvalin sağındaki **Ayarlar** bölmesinde, **bilgi işlem hedefini seçin'i**seçin.

1. Görünen iletişim kutusunda, varolan bir işlem hedefini seçin veya yeni bir hedef oluşturun. **Kaydet'i**seçin.

1. Bir boru hattı çalıştırmak için tuvalin üst kısmında **Gönder'i** seçin.

Örnek ardışık işlem ve bilgi işlem ayarlarına bağlı olarak, çalıştırmaların tamamlanması biraz zaman alabilir. Varsayılan işlem ayarlarının en az düğüm boyutu 0'dır, bu da tasarımcının boşta kaldıktan sonra kaynak ayırması gerektiği anlamına gelir. Yinelenen ardışık işlem çalışır işlem kaynakları zaten tahsis olduğundan daha az zaman alacaktır. Ayrıca, tasarımcı verimliliği daha da artırmak için her modül için önbelleğe alınmış sonuçlar kullanır.


### <a name="review-the-results"></a>Sonuçları gözden geçirin

Ardışık hatlar çalışma bittikten sonra, ardışık hattı gözden geçirebilir ve daha fazla bilgi edinmek için her modülün çıktısını görüntüleyebilirsiniz.

Modül çıktılarını görüntülemek için aşağıdaki adımları kullanın:

1. Tuvalde bir modül seçin.

1. Tuvalin sağındaki modül ayrıntıları bölmesinde **Çıktılar + günlükleri**seçin. Her modülün ![sonuçlarını](./media/tutorial-designer-automobile-price-train-score/visualize-icon.png) görmek için grafik simgesini visualize simgesini seçin. 

En yaygın makine öğrenimi senaryolarından bazıları için örnekleri başlangıç noktası olarak kullanın.

## <a name="regression-samples"></a>Regresyon örnekleri

Yerleşik regresyon örnekleri hakkında daha fazla bilgi edinin.

| Örnek başlık | Açıklama | 
| --- | --- |
| [Örnek 1: Regresyon - Otomobil Fiyat Tahmini (Temel)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-basic.md) | Doğrusal regresyon kullanarak araç fiyatlarını tahmin edin. |
| [Örnek 2: Regresyon - Otomobil Fiyat Tahmini (Gelişmiş)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-compare-algorithms.md) | Karar ormanı ve artırılmış karar ağacı regresörlerini kullanarak araç fiyatlarını tahmin edin. En iyi algoritmayı bulmak için modelleri karşılaştırın.

## <a name="classification-samples"></a>Sınıflandırma örnekleri

Yerleşik sınıflandırma örnekleri hakkında daha fazla bilgi edinin. Örnekleri açarak ve bunun yerine modül yorumlarını görüntüleyerek dokümantasyon bağlantıları olmadan örnekler hakkında daha fazla bilgi edinebilirsiniz.

| Örnek başlık | Açıklama | 
| --- | --- |
| [Örnek 3: Özellik Seçimi ile İkili Sınıflandırma - Gelir Tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-predict-income.md) | İki sınıfartırılmış karar ağacını kullanarak geliri yüksek veya düşük olarak tahmin edin. Özellikleri seçmek için Pearson korelasyonünü kullanın.
| [Örnek 4: Özel Python komut dosyası ile İkili Sınıflandırma - Kredi Risk Tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-credit-risk-cost-sensitive.md) | Kredi başvurularını yüksek veya düşük risk olarak sınıflandırın. Verilerinizi ağırlıklamak için Python Script'i Çalıştır modüllerini kullanın.
| [Örnek 5: İkili Sınıflandırma - Müşteri İlişkisi Tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-churn.md) | İki sınıf artırılmış karar ağaçlarını kullanarak müşteri karmaşası tahmin edin. Yanlışverileri örneklemek için SMOTE' yi kullanım.
| [Örnek 7: Metin Sınıflandırması - Vikipedi SP 500 Dataset](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-text-classification.md) | Çok sınıflı lojistik regresyon ile Vikipedi makalelerinden şirket türlerini sınıflandırın. |
| Örnek 12: Çok Sınıflı Sınıflandırma - Harf Tanıma | Yazılı harfleri sınıflandırmak için ikili sınıflandırıcılardan oluşan bir topluluk oluşturun. |

## <a name="recommender-samples"></a>Tavsiye örnekleri

Yerleşik tavsiye örnekleri hakkında daha fazla bilgi edinin. Örnekleri açarak ve bunun yerine modül yorumlarını görüntüleyerek dokümantasyon bağlantıları olmadan örnekler hakkında daha fazla bilgi edinebilirsiniz.

| Örnek başlık | Açıklama | 
| --- | --- |
| Örnek 10: Tavsiye - Film Derecelendirme Tweets | Film başlıklarından ve derecelendirmesinden bir film tavsiye motoru oluşturun. |

## <a name="utility-samples"></a>Yardımcı program örnekleri

Makine öğrenimi yardımcı programları ve özelliklerini gösteren örnekler hakkında daha fazla bilgi edinin. Örnekleri açarak ve bunun yerine modül yorumlarını görüntüleyerek dokümantasyon bağlantıları olmadan örnekler hakkında daha fazla bilgi edinebilirsiniz.

| Örnek başlık | Açıklama | 
| --- | --- |
| [Örnek 6: Özel R komut dosyası kullanın - Uçuş Gecikme Tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-flight-delay.md) |
| Örnek 8: İkili Sınıflandırma için Çapraz Doğrulama - Yetişkin Gelir Tahmini | Yetişkin geliri için ikili bir sınıflandırıcı oluşturmak için çapraz doğrulamayı kullanın.
| Örnek 9: Permütasyon Özelliği Önemi | Test veri kümesiiçin önem puanlarını hesaplamak için permütasyon özelliğinin önemini kullanın. 
| Örnek 11: İkili Sınıflandırma için Tune Parametreleri - Yetişkin Gelir Tahmini | İkili bir sınıflandırıcı oluşturmak için en uygun hiperparametreleri bulmak için Tune Model Hiperparametrelerini kullanın. |

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Tutorial: Designer ile otomobil fiyatını tahmin](tutorial-designer-automobile-price-train-score.md) etme ile makine öğrenimi modellerini nasıl oluşturup dağıtacağımız hakkında bilgi edinin
