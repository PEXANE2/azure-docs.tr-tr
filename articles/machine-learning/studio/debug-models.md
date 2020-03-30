---
title: Modelinizi hata ayıklama
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'da (klasik) Train Model ve Score Model modülleri tarafından üretilen hataları nasıl ayıklama.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 03/14/2017
ms.openlocfilehash: 910e788830ec55b610a9234a8c8ac75dda1ea189
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218095"
---
# <a name="debug-your-model-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'da modelinizi hata ayıklama (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bir modeli çalıştırırken, aşağıdaki hatalarla karşılaşabilirsiniz:

* [Tren Modeli][train-model] modülü bir hata üretir 
* [Score Model][score-model] modülü yanlış sonuçlar üretir 

Bu makalede, bu hataların olası nedenleri açıklanmaktadır.


## <a name="train-model-module-produces-an-error"></a>Tren Modeli Modülü bir hata üretir

![image1](./media/debug-models/train_model-1.png)

[Tren Modeli][train-model] Modülü iki giriş bekler:

1. Azure Machine Learning Studio (klasik) tarafından sağlanan model koleksiyonundan makine öğrenme modeli türü.
2. Tahmin etmek için değişkeni belirten belirli bir Etiket sütununa sahip eğitim verileri (diğer sütunların Özellikler olduğu varsayılır).

Bu modül aşağıdaki durumlarda bir hata üretebilir:

1. Etiket sütunu yanlış belirtilir. Bu, Etiket olarak birden fazla sütun seçilirse veya yanlış sütun dizini seçilirse bu durum olabilir. Örneğin, yalnızca 25 sütunu olan bir giriş veri kümesiyle 30 sütun dizini kullanılıyorsa ikinci büyük/küçük harf uygulanır.

2. Veri kümesi herhangi bir Özellik sütunu içermez. Örneğin, giriş veri kümesinde Etiket sütunu olarak işaretlenmiş yalnızca bir sütun varsa, modeli oluşturacak özellik olmaz. Bu durumda, [Tren Modeli][train-model] modülü bir hata üretir.

3. Giriş veri kümesi (Özellikler veya Etiket) değer olarak Sonsuzluk içerir.

## <a name="score-model-module-produces-incorrect-results"></a>Puan Modeli Modülü yanlış sonuçlar üretir

![image2](./media/debug-models/train_test-2.png)

Denetimli öğrenme için tipik bir eğitim/test denemesinde, [Bölünmüş Veri][split] modülü orijinal veri kümesini iki bölüme ayırır: bir parça modeli eğitmek için kullanılır ve bir parça eğitimli modelin ne kadar iyi performans gösterdiğini puanlamak için kullanılır. Daha sonra eğitilen model, test verilerini puanlamak için kullanılır ve ardından sonuçlar modelin doğruluğunu belirlemek için değerlendirilir.

[Puan Modeli][score-model] modülü iki giriş gerektirir:

1. [Tren Modeli][train-model] modülünden eğitimli bir model çıkışı.
2. Modeli eğitmek için kullanılan veri kümesinden farklı bir puanlama veri kümesi.

Deneme başarılı olsa bile, [Score Model][score-model] modülü yanlış sonuçlar üretir. Birkaç senaryo bu sorunun gerçekleşmesine neden olabilir:

1. Belirtilen Etiket kategorikse ve veriler üzerinde bir regresyon modeli eğitilirse, [Skor Modeli][score-model] modülü tarafından yanlış bir çıktı üretilir. Bunun nedeni, gerilemenin sürekli bir yanıt değişkeni gerektirmesidir. Bu durumda, bir sınıflandırma modeli kullanmak daha uygun olacaktır. 

2. Benzer şekilde, bir sınıflandırma modeli Etiket sütununda kayan nokta numaraları olan bir veri kümesi üzerinde eğitilirse, istenmeyen sonuçlar üretebilir. Bunun nedeni, sınıflandırmanın yalnızca sonlu ve küçük sınıflar kümesi üzerinde değişen değerlere izin veren ayrı bir yanıt değişkeni gerektirmesidir.

3. Puanlama veri kümesi modeli eğitmek için kullanılan tüm özellikleri içermiyorsa, [Puan Modeli][score-model] bir hata üretir.

4. Puanlama veri kümesindeki bir satır eksik bir değer veya özelliklerinden herhangi biri için sonsuz bir değer içeriyorsa, [Puan Modeli][score-model] bu satıra karşılık gelen herhangi bir çıktı üretmez.

5. [Puan Modeli,][score-model] puanlama veri kümesindeki tüm satırlar için aynı çıktıları üretebilir. Bu durum, örneğin, yaprak düğümü başına en az sayıda örnek kullanılabilir eğitim örneklerinden daha fazla olarak seçilirse, Karar Ormanları kullanılarak sınıflandırmaya çalışırken oluşabilir.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

