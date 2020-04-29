---
title: Özel model formu tanıyıcı için eğitim verileri kümesi oluşturma
titleSuffix: Azure Cognitive Services
description: Eğitim veri kümesinin, form tanıyıcı modeli eğitimi için iyileştirildiğini nasıl sağlayacağınızı öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: pafarley
ms.openlocfilehash: 71ad7c5dd3ad74082da552cd3c45142bc0c2d624
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75380635"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Özel model için eğitim veri kümesi oluşturma

Form tanıyıcı özel modelini kullandığınızda, modelin sektöre özgü formlara eğmesi için kendi eğitim verilerinizi sağlarsınız. Bir modeli beş doldurulmuş form veya boş bir form ile eğitebilirsiniz (dosya adında "boş" sözcüğünü ve iki doldurulmuş formu dahil etmeniz gerekir. Eğecek yeterli sayıda doldurulmuş formunuz olsa da, eğitim veri kümesine boş bir form eklemek, modelin doğruluğunu iyileştirebilir.

El ile veri etiketli eğitim verileri kullanmak istiyorsanız, aynı türde en az beş form ile başlamanız gerekir. Etiketli formları ve boş bir formu aynı veri kümesinde kullanmaya devam edebilirsiniz.

## <a name="training-data-tips"></a>Eğitim verileri ipuçları

Eğitim için en iyi duruma getirilmiş bir veri kümesi kullanılması önemlidir. [Özel modeli eğitme](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync) işlemindeki en iyi sonuçları aldığınızdan emin olmak için aşağıdaki ipuçlarını kullanın:

* Mümkünse, görüntü tabanlı belgeler yerine metin tabanlı PDF belgelerini kullanın. Taranan PDF 'Ler görüntü olarak işlenir.
* Doldurulmuş formlarda, tüm alanlarının doldurulduğu örnekleri kullanın.
* Her alanda farklı değerlere sahip olan formlar kullanın.
* Form görüntüleriniz daha düşük kaliteden fazlaysa daha büyük bir veri kümesi kullanın (örneğin, 10-15 görüntü).
* Eğitim veri kümesinin toplam boyutu en fazla 500 sayfa olabilir.

## <a name="general-input-requirements"></a>Genel giriş gereksinimleri

Eğitim verileri ayarlamış olduğunuz tüm form tanıyıcı içeriği için giriş gereksinimlerini de takip ettiğinizden emin olun. 

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="upload-your-training-data"></a>Eğitim verilerinizi karşıya yükleyin

Eğitim için kullanacağınız form belgelerinin bir kümesini birlikte yerleştirdiğinizde, bir Azure Blob depolama kapsayıcısına yüklemeniz gerekir. Bir kapsayıcı ile Azure depolama hesabı oluşturmayı bilmiyorsanız [Azure Portal Için Azure Storage hızlı](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)başlangıcını takip edin.

### <a name="organize-your-data-in-subfolders-optional"></a>Verilerinizi alt klasörlerde düzenleme (isteğe bağlı)

Varsayılan olarak, [özel model eğitme](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync) API 'si yalnızca depolama kapsayıcının kökünde bulunan form belgelerini kullanacaktır. Ancak, API çağrısında belirtirseniz, alt klasörlerdeki verilerle eğitebilirsiniz. Normal olarak, [eğitme özel model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync) çağrısının gövdesi aşağıdaki biçimdedir ve burada `<SAS URL>` kapsayıcının paylaşılan erişim imzası URL 'sidir:

```json
{
  "source":"<SAS URL>"
}
```

İstek gövdesine aşağıdaki içeriği eklerseniz, API alt klasörlerde bulunan belgelerle eğitecektir. Bu `"prefix"` alan isteğe bağlıdır ve eğitim verilerini, yolları verilen dizeyle başlayan dosyalar olarak sınırlandırır. Bu nedenle, örneğin `"Test"`DEĞERI, API 'nin yalnızca "test" kelimesiyle başlayan dosya veya klasörlere bakmasına neden olur.

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

Eğitim veri kümesi oluşturmayı öğrendiğinize göre, özel bir form tanıyıcı modelini eğtirecek ve formlarınızda kullanmaya başlamak için hızlı başlangıç izleyin.

* [Hızlı başlangıç: bir modeli eğitme ve kıvrımlı kullanarak form verilerini ayıklama](./quickstarts/curl-train-extract.md)
* [Hızlı başlangıç: Python ile REST API bir modeli eğitme ve form verilerini ayıklama](./quickstarts/python-train-extract.md)
* [REST API ve Python kullanarak etiketlerle eğitme](./quickstarts/python-labeled-data.md)