---
title: Modelinizde hata ayıklama
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) içinde model eğitme ve model modülleri Puanlama tarafından oluşturulan hatalarda hata ayıklama.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 03/14/2017
ms.openlocfilehash: 6c672e57c0c1c0b62b79afbacec3f842770e3d61
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169243"
---
# <a name="debug-your-model-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio modelinizde hata ayıklama (klasik)

Bir model çalıştırırken aşağıdaki hatalarla karşılaşabilirsiniz:

* [model eğitme][train-model] modülü bir hata üretir 
* [puan modeli][score-model] modülü hatalı sonuçlar oluşturuyor 

Bu makalede bu hataların olası nedenleri açıklanmaktadır.


## <a name="train-model-module-produces-an-error"></a>Train Model modülü bir hata oluşturur.

![image1](./media/debug-models/train_model-1.png)

[Model eğitme][train-model] modülü iki giriş bekler:

1. Azure Machine Learning Studio (klasik) tarafından sunulan modeller koleksiyonundan makine öğrenimi modelinin türü.
2. Tahmin edilecek değişkeni belirten belirtilen bir etiket sütunuyla eğitim verileri (diğer sütunların özellikler olduğu varsayılır).

Bu modül, aşağıdaki durumlarda bir hata oluşturabilecek:

1. Etiket sütununda hatalı şekilde belirtildi. Bu, birden fazla sütun etiketi olarak seçilir ya da yanlış sütun dizini seçili oluşabilir. Örneğin, ikinci durum, yalnızca 25 sütun içeren bir giriş veri kümesiyle 30 ' un bir sütun dizini kullanılıyorsa geçerlidir.

2. Veri kümesi herhangi bir özellik sütunu içermiyor. Örneğin, giriş veri kümesi etiketi sütun olarak işaretlenmiş, yalnızca bir sütun varsa olacaktır, modeli oluşturmak üzere hiçbir özellik. Bu durumda, [model eğitimi][train-model] modülü bir hata üretir.

3. Giriş veri kümesi (Özellikler veya etiket), sonsuz bir değeri içerir.

## <a name="score-model-module-produces-incorrect-results"></a>Score Model modülü hatalı sonuçlar üretir.

![image2](./media/debug-models/train_test-2.png)

Denetimli öğrenme için tipik bir eğitim/test denemesinde, [bölünmüş veri][split] modülü özgün veri kümesini iki parçaya böler: bir bölüm modeli eğmek için, bir bölüm ise eğitilen modelin ne kadar iyi performans olduğunu öğrenmek için kullanılır. Eğitilen modelin sonra sonuçları sonra model doğruluğunu belirlemek için değerlendirilir test verileri puanlamak için kullanılır.

[Puan modeli][score-model] modülü iki giriş gerektirir:

1. Eğitim [modeli][train-model] modülünden eğitilen bir model çıkışı.
2. Modeli eğitmek için kullanılan veri kümesinden farklı bir Puanlama veri kümesi.

Deneme başarılı olsa bile, [puan modeli][score-model] modülü hatalı sonuçlar üretir. Birkaç senaryo bu sorunun oluşmasına neden olabilir:

1. Belirtilen etiket kategorik ise ve bir regresyon modeli veri üzerinde eğitilmiş ise, [puan modeli][score-model] modülü tarafından yanlış bir çıktı oluşturulur. Regresyon sürekli yanıt değişkeni gerektirdiğinden budur. Bu durumda, bir sınıflandırma modelini kullanmak daha uygun olacaktır. 

2. Benzer şekilde, etiket sütununda kayan nokta numarası sahip bir veri kümesi üzerinde bir sınıflandırma modeli eğitilir, istenmeyen sonuçlara neden olabilir. Bunun nedeni, sınıflandırmanın yalnızca sonlu ve küçük bir sınıf kümesi üzerinde yer alan değerlere izin veren ayrık bir yanıt değişkeni gerektirmesidir.

3. Puanlama veri kümesi, modeli eğitmek için kullanılan tüm özellikleri içermiyorsa, [puan modeli][score-model] bir hata üretir.

4. Puanlama veri kümesindeki bir satır, özelliklerinin herhangi biri için eksik bir değer veya sonsuz bir değer içeriyorsa, [puan modeli][score-model] söz konusu satıra karşılık gelen bir çıktı oluşturmaz.

5. [Puan modeli][score-model] , Puanlama veri kümesindeki tüm satırlarda aynı çıktıları üretebilir. Bu, örneğin, örnek yaprak düğümü başına en düşük sayısı olması sayıdan fazla eğitim örnekleri kullanılabilir seçilirse karar ormanları kullanarak sınıflandırma çalışılırken gerçekleşebilir.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

