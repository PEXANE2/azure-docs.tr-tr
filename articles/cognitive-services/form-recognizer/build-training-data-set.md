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
ms.openlocfilehash: 513d98ae274c668197b066c742119055248f37f9
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92911724"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Özel model için eğitim veri kümesi oluşturma

Form tanıyıcı özel modelini kullandığınızda, modelin sektöre özgü formlarınıza eğmesi için kendi eğitim verilerinizi [özel modeli eğitme](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync) işlemine sağlarsınız. Modeli verimli bir şekilde eğitme amacıyla verileri nasıl toplayacağınızı ve hazırlanacağınızı öğrenmek için bu kılavuzu izleyin.

El ile etiketleriniz olmadan eğitim elde ediyorsanız, beş doldurulmuş form veya boş bir form kullanabilirsiniz ("boş" sözcüğünü dosya adı ile birlikte dahil etmeniz gerekir) ve iki doldurulmuş form oluşturabilirsiniz. Yeterli sayıda doldurulmuş formunuz olsa da, eğitim veri kümesine boş bir form eklemek, modelin doğruluğunu iyileştirebilir.

El ile veri etiketli eğitim verileri kullanmak istiyorsanız, aynı türde en az beş doldurulmuş form ile başlamanız gerekir. Etiketli formları ve gerekli veri kümesine ek olarak boş bir formu kullanmaya devam edebilirsiniz.

## <a name="custom-model-input-requirements"></a>Özel model giriş gereksinimleri

İlk olarak, eğitim verileri ayarlamış olduğunuz form tanıyıcı için giriş gereksinimlerini izlediğinden emin olun.

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="training-data-tips"></a>Eğitim verileri ipuçları

Eğitim için veri kümesini daha iyi hale getirmeye yönelik bu ek ipuçlarını izleyin.

* Mümkünse, görüntü tabanlı belgeler yerine metin tabanlı PDF belgelerini kullanın. Taranan PDF 'Ler görüntü olarak işlenir.
* Doldurulmuş formlarda, tüm alanlarının doldurulduğu örnekleri kullanın.
* Her alanda farklı değerlere sahip olan formlar kullanın.
* Form görüntüleriniz daha düşük kaliteden fazlaysa daha büyük bir veri kümesi kullanın (örneğin, 10-15 görüntü).

## <a name="upload-your-training-data"></a>Eğitim verilerinizi karşıya yükleyin

Eğitim için kullanacağınız form belgelerinin bir kümesini birlikte yerleştirdiğinizde, bir Azure Blob depolama kapsayıcısına yüklemeniz gerekir. Bir kapsayıcı ile Azure depolama hesabı oluşturmayı bilmiyorsanız [Azure Portal Için Azure Storage hızlı](../../storage/blobs/storage-quickstart-blobs-portal.md)başlangıcını takip edin. Standart performans katmanını kullanın.

El ile etiketlenmiş verileri kullanmak istiyorsanız eğitim belgelerinize karşılık gelen dosyalara *.labels.js* ve *.ocr.js* yüklemeniz gerekir. Bu dosyaları oluşturmak için [örnek etiketleme aracını](./quickstarts/label-tool.md) (veya kendi Kullanıcı arabiriminizi) kullanabilirsiniz.

### <a name="organize-your-data-in-subfolders-optional"></a>Verilerinizi alt klasörlerde düzenleme (isteğe bağlı)

Varsayılan olarak, [özel model eğitme](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync) API 'si yalnızca depolama kapsayıcının kökünde bulunan form belgelerini kullanacaktır. Ancak, API çağrısında belirtirseniz, alt klasörlerdeki verilerle eğitebilirsiniz. Normal olarak, [eğitme özel model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync) çağrısının gövdesi aşağıdaki biçime sahiptir; burada, `<SAS URL>` kapsayıcının paylaşılan erişim imzası URL 'sidir:

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

* [İstemci kitaplığını kullanarak bir modeli eğitme ve form verilerini ayıklama](./quickstarts/client-library.md)
* [Bir modeli eğitme ve kıvrımlı kullanarak form verilerini ayıklama](./quickstarts/curl-train-extract.md)
* [REST API ve Python kullanarak bir modeli eğitme ve form verilerini ayıklama](./quickstarts/python-train-extract.md)
* [Örnek etiketleme aracını kullanarak etiketlerle eğitme](./quickstarts/label-tool.md)
* [REST API ve Python kullanarak etiketlerle eğitme](./quickstarts/python-labeled-data.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Form Tanıma nedir?](./overview.md)