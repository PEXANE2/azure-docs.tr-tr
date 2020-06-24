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
ms.openlocfilehash: ffa09293ad2ff02e104ce285b6b0aaca7d4744a2
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85212675"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Özel model için eğitim veri kümesi oluşturma

Form tanıyıcı özel modelini kullandığınızda, modelin sektöre özgü formlara eğmesi için kendi eğitim verilerinizi sağlarsınız. 

El ile etiketleriniz olmadan eğitim elde ediyorsanız, beş doldurulmuş form veya boş bir form kullanabilirsiniz ("boş" sözcüğünü dosya adı ile birlikte dahil etmeniz gerekir) ve iki doldurulmuş form oluşturabilirsiniz. Yeterli sayıda doldurulmuş formunuz olsa da, eğitim veri kümesine boş bir form eklemek, modelin doğruluğunu iyileştirebilir.

El ile veri etiketli eğitim verileri kullanmak istiyorsanız, aynı türde en az beş doldurulmuş form ile başlamanız gerekir. Etiketli formları ve gerekli veri kümesine ek olarak boş bir formu kullanmaya devam edebilirsiniz.

## <a name="training-data-tips"></a>Eğitim verileri ipuçları

Eğitim için en iyi duruma getirilmiş bir veri kümesi kullanılması önemlidir. [Özel modeli eğitme](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/TrainCustomModelAsync) işlemindeki en iyi sonuçları aldığınızdan emin olmak için aşağıdaki ipuçlarını kullanın:

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

El ile etiketlenmiş verileri kullanmak istiyorsanız eğitim belgelerinize karşılık gelen dosyalara *.labels.js* ve *.ocr.js* yüklemeniz gerekir. Bu dosyaları oluşturmak için [örnek etiketleme aracını](./quickstarts/label-tool.md) (veya kendi Kullanıcı arabiriminizi) kullanabilirsiniz.

### <a name="organize-your-data-in-subfolders-optional"></a>Verilerinizi alt klasörlerde düzenleme (isteğe bağlı)

Varsayılan olarak, [özel model eğitme](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/TrainCustomModelAsync) API 'si yalnızca depolama kapsayıcının kökünde bulunan form belgelerini kullanacaktır. Ancak, API çağrısında belirtirseniz, alt klasörlerdeki verilerle eğitebilirsiniz. Normal olarak, [eğitme özel model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/TrainCustomModelAsync) çağrısının gövdesi aşağıdaki biçime sahiptir; burada, `<SAS URL>` kapsayıcının paylaşılan erişim imzası URL 'sidir:

```json
{
  "source":"<SAS URL>"
}
```

İstek gövdesine aşağıdaki içeriği eklerseniz, API alt klasörlerde bulunan belgelerle eğitecektir. `"prefix"`Bu alan isteğe bağlıdır ve eğitim verilerini, yolları verilen dizeyle başlayan dosyalar olarak sınırlandırır. Bu nedenle, `"Test"` Örneğin değeri, API 'nin yalnızca "test" kelimesiyle başlayan dosya veya klasörlere bakmasına neden olur.

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

* [Bir modeli eğitme ve kıvrımlı kullanarak form verilerini ayıklama](./quickstarts/curl-train-extract.md)
* [REST API ve Python kullanarak bir modeli eğitme ve form verilerini ayıklama](./quickstarts/python-train-extract.md)
* [Örnek etiketleme aracını kullanarak etiketlerle eğitme](./quickstarts/label-tool.md)
* [REST API ve Python kullanarak etiketlerle eğitme](./quickstarts/python-labeled-data.md)
