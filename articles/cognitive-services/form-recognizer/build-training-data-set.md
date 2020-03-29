---
title: Özel bir model için eğitim veri seti oluşturma - Form Tanıyın
titleSuffix: Azure Cognitive Services
description: Eğitim veri setinizin Form Tanıyıcısı modelini eğitmek için optimize edilmiş olduğundan nasıl emin olanın.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: pafarley
ms.openlocfilehash: 71ad7c5dd3ad74082da552cd3c45142bc0c2d624
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75380635"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Özel bir model için eğitim veri seti oluşturma

Form Recognizer özel modelini kullandığınızda, modelin sektöre özel formlarınıza göre eğitim görebilmeleri için kendi eğitim verilerinizi sağlarsınız. Beş doldurulmuş form veya boş bir forma sahip bir model eğitebilirsiniz (dosya adına "boş" sözcüğü eklemeniz gerekir) artı iki doldurulmuş form. Eğitebileceğiniz yeterli doldurulmuş formunuzun olsa bile, eğitim veri setinize boş bir form eklemek modelin doğruluğunu artırabilir.

El ile etiketlenmiş eğitim verilerini kullanmak istiyorsanız, aynı türden en az beş formla başlamalısınız. Yine de aynı veri kümesinde etiketlenmemiş formları ve boş bir formu kullanabilirsiniz.

## <a name="training-data-tips"></a>Eğitim veri ipuçları

Eğitim için optimize edilmiş bir veri kümesi kullanmak önemlidir. [Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync) işleminden en iyi sonuçları aldığınızdan emin olmak için aşağıdaki ipuçlarını kullanın:

* Mümkünse, resim tabanlı belgeler yerine metin tabanlı PDF belgeleri kullanın. Taranan PDF'ler görüntü olarak işlenir.
* Doldurulmuş formlar için, tüm alanlarının doldurulduğu örnekleri kullanın.
* Her alanda farklı değerlere sahip olan formlar kullanın.
* Form görüntüleriniz daha düşük kalitedeyse, daha büyük bir veri kümesi kullanın (örneğin, 10-15 görüntü).
* Eğitim veri kümesinin toplam boyutu 500 sayfaya kadar olabilir.

## <a name="general-input-requirements"></a>Genel giriş gereksinimleri

Eğitim veri setinizin tüm Form Tanıyıcı içeriğiiçin giriş gerekliliklerini de takip edeceğinden emin olun. 

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="upload-your-training-data"></a>Eğitim verilerinizi yükleyin

Eğitim için kullanacağınız form belgeleri kümesini bir araya getirdiğinizde, bunu bir Azure blob depolama kapsayıcısına yüklemeniz gerekir. Azure [portalı için Azure Depolama hızlı başlat'ını](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)izleyerek kapsayıcıyla nasıl bir Azure depolama hesabı oluşturabileceğinizi bilmiyorsanız.

### <a name="organize-your-data-in-subfolders-optional"></a>Verilerinizi alt klasörlerde düzenleme (isteğe bağlı)

Varsayılan olarak, [Tren Özel Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync) API'si yalnızca depolama kapsayıcınızın kökünde bulunan form belgelerini kullanır. Ancak, API çağrısında belirtirseniz alt klasörlerdeki verilerle eğitebilirsiniz. Normalde, Train Custom [Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync) aramasının gövdesi, konteynerinizin Paylaşılan erişim imza URL'si aşağıdaki `<SAS URL>` formu oluşturur:

```json
{
  "source":"<SAS URL>"
}
```

Aşağıdaki içeriği istek gövdesine eklerseniz, API alt klasörlerde bulunan belgelerle çalışır. Alan `"prefix"` isteğe bağlıdır ve eğitim veri kümesini, yolları verilen dizeyle başlayan dosyalarla sınırlandıracaktır. Bu nedenle, `"Test"`örneğin, API'nin yalnızca "Test" sözcüğüyle başlayan dosya veya klasörlere bakması için bir değer elde edecektir.

```json
{
  "source": "<SAS URL>",
  "sourceFilter": {
    "prefix": "<prefix string>",
    "includeSubFolders": true
  },
  "useLabelFile": false
}
```

## <a name="next-steps"></a>Sonraki adımlar

Artık bir eğitim veri kümesi oluşturmayı öğrendiğinize göre, özel bir Form Tanıyıcı modelini eğitmek için hızlı bir başlangıç yapın ve formlarınızda kullanmaya başlayın.

* [Quickstart: CURL kullanarak bir model ve ayıklama formu verileri tren](./quickstarts/curl-train-extract.md)
* [Quickstart: Python ile REST API'yi kullanarak bir model ve ayıklama formu verileri eğitin](./quickstarts/python-train-extract.md)
* [REST API ve Python kullanarak etiketlerle eğitme](./quickstarts/python-labeled-data.md)