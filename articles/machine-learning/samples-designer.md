---
title: Örnek tasarımcı işlem hatları
titleSuffix: Azure Machine Learning
description: Makine öğrenimi işlem hatlarınızı atlayaöğrenmek için Azure Machine Learning tasarımcısında örnekleri kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 03/29/2020
ms.openlocfilehash: f9a8b0a4c51024d91e517db2f6ae10a4dba62384
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80389350"
---
# <a name="designer-sample-pipelines"></a>Tasarımcı örnek işlem hatları

Kendi makine öğrenimi işlem hatlarınızı oluşturmaya hemen başlamak için Azure Machine Learning tasarımcısında yerleşik örnekleri kullanın. Azure Machine Learning Designer [GitHub deposu](https://github.com/Azure/MachineLearningDesigner) , bazı yaygın makine öğrenimi senaryolarını anlamanıza yardımcı olacak ayrıntılı belgeler içerir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://aka.ms/AMLFree)oluşturun.
* Kurumsal SKU 'SU olan bir Azure Machine Learning çalışma alanı.


## <a name="how-to-use-sample-pipelines"></a>Örnek işlem hatlarını kullanma

Tasarımcı, örnek işlem hatlarının bir kopyasını Studio çalışma alanınıza kaydeder. İşlem hattını düzenleyerek gereksinimlerinize uyarlayabilir ve kendi kendinize kaydedebilirsiniz. Projelerinizi başlatmak için bunları bir başlangıç noktası olarak kullanın.

### <a name="open-a-sample-pipeline"></a>Örnek bir işlem hattı açın

1. <a href="https://ml.azure.com?tabs=jre" target="_blank">Ml.Azure.com</a>'de oturum açın ve birlikte çalışmak istediğiniz çalışma alanını seçin.

1. **Tasarımcı**' yı seçin.

1. **Yeni işlem hattı** bölümünde örnek bir işlem hattı seçin.

    Örneklerin tamamen listesi için **daha fazla örnek göster** ' i seçin.

### <a name="submit-a-pipeline-run"></a>İşlem hattı çalıştırması gönderme

Bir işlem hattını çalıştırmak için, önce varsayılan işlem hedefini, ardışık düzeni çalıştırmak için ayarlamanız gerekir.

1. Tuvalin sağındaki **Ayarlar** bölmesinde, **işlem hedefini seç**' i seçin.

1. Görüntülenen iletişim kutusunda, var olan bir işlem hedefini seçin veya yeni bir tane oluşturun. **Kaydet**’i seçin.

1. Bir işlem hattı çalıştırması göndermek için tuvalin en üstünde **Gönder** ' i seçin.

Örnek ardışık düzen ve işlem ayarlarına bağlı olarak, çalıştırmanın tamamlanması biraz zaman alabilir. Varsayılan işlem ayarlarının minimum düğüm boyutu 0 ' dır ve bu, tasarımcının boşta kaldıktan sonra kaynakları ayırması gerektiği anlamına gelir. İşlem kaynakları zaten ayrıldığından tekrarlanan işlem hattı çalıştırmaları daha az zaman alır. Ayrıca tasarımcı, verimliliği artırmak için her modül için önbelleğe alınmış sonuçları kullanır.


### <a name="review-the-results"></a>Sonuçları gözden geçirin

İşlem hattı çalışmaya başladıktan sonra, daha fazla bilgi edinmek için işlem hattını gözden geçirebilir ve her bir modülün çıktısını görüntüleyebilirsiniz.

Modül çıkışlarını görüntülemek için aşağıdaki adımları kullanın:

1. Tuvalde bir modül seçin.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, **çıktılar + Günlükler**' i seçin. Her modülün sonucunu görmek ![için grafik](./media/tutorial-designer-automobile-price-train-score/visualize-icon.png) simgesi Görselleştir simgesini seçin. 

En yaygın makine öğrenimi senaryolarından bazıları için örnekleri başlangıç noktaları olarak kullanın.

## <a name="regression-samples"></a>Gerileme örnekleri

Yerleşik gerileme örnekleri hakkında daha fazla bilgi edinin.

| Örnek başlık | Açıklama | 
| --- | --- |
| [Örnek 1: gerileme-otomobil fiyat tahmini (temel)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-basic.md) | Doğrusal regresyon kullanarak otomobil fiyatlarını tahmin edin. |
| [Örnek 2: gerileme-otomobil fiyat tahmini (Gelişmiş)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-compare-algorithms.md) | Karar ormanını kullanarak otomobil fiyatlarını tahmin edin ve karar ağacı gerilediğini kullanın. En iyi algoritmayı bulmak için modelleri karşılaştırın.

## <a name="classification-samples"></a>Sınıflandırma örnekleri

Yerleşik sınıflandırma örnekleri hakkında daha fazla bilgi edinin. Örnekleri açıp modül açıklamalarını görüntüleyerek, belge bağlantıları olmadan örnekler hakkında daha fazla bilgi edinebilirsiniz.

| Örnek başlık | Açıklama | 
| --- | --- |
| [Örnek 3: Özellik seçimi ile Ikili sınıflandırma-gelir tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-predict-income.md) | İki sınıf artırılmış bir karar ağacı kullanarak geliri yüksek veya düşük olarak tahmin edin. Özellikleri seçmek için Pearson bağıntısını kullanın.
| [Örnek 4: özel Python betiği ile Ikili sınıflandırma-kredi riski tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-credit-risk-cost-sensitive.md) | Kredi uygulamalarını yüksek veya düşük riskli olarak sınıflandırın. Verilerinizi ağırlığa yönelik Python betiği yürütme modülünü kullanın.
| [Örnek 5: Ikili sınıflandırma-müşteri Ilişkisi tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-churn.md) | İki sınıf artırılmış bir karar ağacının kullanıldığı müşteri karmaşıklığını tahmin edin. Taraflı verileri örneklemek için IÇTE 'yi kullanın.
| [Örnek 7: metin sınıflandırması-Vikipedi SP 500 veri kümesi](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-text-classification.md) | Birden çok Lass Lojistik gerileme ile Vikipedi makalelerinden şirket türlerini sınıflandırın. |
| Örnek 12: birden çok Lass sınıflandırması-harf tanıma | Yazılan harflerin sınıflandırılmasında ikili Sınıflandırıcıların bir kopyasını oluşturun. |

## <a name="recommender-samples"></a>Öneren örnekleri

Yerleşik öneren örnekleri hakkında daha fazla bilgi edinin. Örnekleri açıp modül açıklamalarını görüntüleyerek, belge bağlantıları olmadan örnekler hakkında daha fazla bilgi edinebilirsiniz.

| Örnek başlık | Açıklama | 
| --- | --- |
| Örnek 10: öneri-film derecelendirmesi arası | Film başlıklarından ve derecelendirmeden bir film öneren altyapısı oluşturun. |

## <a name="utility-samples"></a>Yardımcı program örnekleri

Machine Learning yardımcı programlarını ve özelliklerini gösteren örnekler hakkında daha fazla bilgi edinin. Örnekleri açıp modül açıklamalarını görüntüleyerek, belge bağlantıları olmadan örnekler hakkında daha fazla bilgi edinebilirsiniz.

| Örnek başlık | Açıklama | 
| --- | --- |
| [Örnek 6: özel R betiği kullanma-Uçuş gecikmesi tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-flight-delay.md) |
| Örnek 8: Ikili sınıflandırma için çapraz doğrulama-yetişkinlere yönelik gelir tahmini | Yetişkinlere yönelik gelir için ikili bir sınıflandırıcı oluşturmak üzere çapraz doğrulama kullanın.
| Örnek 9: permütasyon özelliği önem derecesi | Test veri kümesinin önem puanlarını hesaplamak için permütasyon özelliği önem derecesini kullanın. 
| Örnek 11: Ikili sınıflandırmayla ilgili parametreleri ayarlama-yetişkinlere yönelik gelir tahmini | İkili bir sınıflandırıcı oluşturmak için en uygun hiper parametreleri bulmak için model hiper parametrelerini ayarla ' yı kullanın. |

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Öğretici ile makine öğrenimi modelleri oluşturma ve dağıtma hakkında bilgi edinin [: tasarımcı ile otomobil fiyatlarını tahmin](tutorial-designer-automobile-price-train-score.md) etme
