---
title: Özel Konuşma Tanıma okuma hizmeti modeli ve uç nokta yaşam döngüsü
titleSuffix: Azure Cognitive Services
description: Özel Konuşma Tanıma, uyarlama için temel modeller sağlar ve verilerinizde özel modeller oluşturmanızı sağlar. Bu makalede, modeller ve bu modelleri kullanan uç noktalar için zaman çizelgeleri açıklanmaktadır.
services: cognitive-services
author: heikora
manager: dongli
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/10/2021
ms.author: heikora
ms.openlocfilehash: b8e02071eca139cde02a8bad1b0e0e443db6ab86
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103555701"
---
# <a name="model-and-endpoint-lifecycle"></a>Model ve uç nokta yaşam döngüsü

Özel Konuşma Tanıma hem *temel modelleri* hem de *özel modelleri* kullanır. Her dilin bir veya daha fazla temel modeli vardır. Genellikle, normal konuşma hizmetine yeni bir konuşma modeli sunulduğunda, yeni bir temel model olarak Özel Konuşma Tanıma hizmetine de aktarılır. Her 6 ayda bir güncelleştirilir. En yeni model genellikle daha fazla doğruluk sağladığından, eski modeller genellikle zaman içinde daha az yararlı hale gelir.

Buna karşılık, özel modeller belirli bir müşteri senaryoınızdan alınan verilerle seçili bir temel modeli uyarlayarak oluşturulur. Gereksinimlerinizi karşıladıktan sonra, belirli bir özel modeli uzun bir süre kullanmaya devam edebilirsiniz. Ancak, en son temel modele düzenli olarak güncelleştirmeniz ve ek verilerle zaman içinde yeniden eğitmeniz önerilir. 

Model yaşam döngüsüyle ilgili diğer önemli terimler şunlardır:

* **Uyarlama**: bir temel model alma ve metin verileri ve/veya ses verilerini kullanarak etki alanı/senaryonuz ile özelleştirme.
* **Kod çözme**: model kullanma ve konuşma tanımayı gerçekleştirme (sesin metin olarak kodunu çözme).
* **Uç noktası**: bir temel modelin veya *yalnızca* belirli bir kullanıcıya erişilebilen özel bir modelin kullanıcıya özgü bir dağıtımı.

### <a name="expiration-timeline"></a>Süre sonu zaman çizelgesi

Yeni modeller ve yeni işlevler kullanılabilir hale gelir ve daha eski, daha az doğru modeller kullanımdan kalkmışsa model ve uç nokta süre sonu için aşağıdaki zaman çizelgelerine bakın:

**Taban modeller** 

* Uyarlama: bir yıl boyunca kullanılabilir. Model alındıktan sonra, özel modeller oluşturmak için bir yıl kullanılabilir. Bir yıldan sonra, yeni özel modellerin daha yeni bir temel model sürümünden oluşturulması gerekir.  
* Kod çözme: içeri aktarma işleminden sonraki iki yıl boyunca kullanılabilir. Bu nedenle, bir uç nokta oluşturabilir ve bu modelle iki yıl boyunca toplu iş dökümü kullanabilirsiniz. 
* Uç noktalar: kod çözme ile aynı zaman çizelgesinde kullanılabilir.

**Özel modeller**

* Kod çözme: model oluşturulduktan iki yıl sonra kullanılabilir. Bu nedenle, özel modeli oluşturulduktan sonra iki yıl (Batch/gerçek zamanlı/test) kullanabilirsiniz. İki yıl sonra, temel model genellikle uyarlama için kullanım dışı bırakıldığı için *modelinize yeniden eğmelisiniz* .  
* Uç noktalar: kod çözme ile aynı zaman çizelgesinde kullanılabilir.

Bir temel model veya özel modelin süresi dolmuşsa, her zaman *en yeni temel model sürümüne* geri döner. Bu nedenle, uygulamanız hiçbir zaman kesintiye uğramayacaktır, ancak özel modellerin süresi dolduktan sonra *belirli verileriniz* için daha az doğru olabilir. Bir modelin kullanım süresini, konuşma Studio 'nun Özel Konuşma Tanıma alanında aşağıdaki konumlarda görebilirsiniz:

* Model Eğitimi Özeti
* Model eğitimi ayrıntısı
* Dağıtım özeti
* Dağıtım ayrıntısı

Model eğitimi özetinin bir örneği aşağıda verilmiştir:

![Model eğitimi özeti ](media/custom-speech/custom-speech-model-training-with-expiry.png) sayfasında ayrıca model Eğitimi Özeti sayfasından:

![Model eğitimi ayrıntısı](media/custom-speech/custom-speech-model-details-with-expiry.png)

Ayrıca, [`GetModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetModel) [`GetBaseModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetBaseModel) JSON yanıtında özelliği altındaki ve özel konuşma API 'leri aracılığıyla sona erme tarihlerini de denetleyebilirsiniz `deprecationDates` .

GetModel API çağrısından alınan süre sonu verilerine bir örnek aşağıda verilmiştir. "KULLANıMDAN kaldırıldı CATIONTARIHLERE" şunu gösterir: 
```json
{
    "SELF": "HTTPS://WESTUS2.API.COGNITIVE.MICROSOFT.COM/SPEECHTOTEXT/V3.0/MODELS/{id}",
    "BASEMODEL": {
    "SELF": HTTPS://WESTUS2.API.COGNITIVE.MICROSOFT.COM/SPEECHTOTEXT/V3.0/MODELS/BASE/{id}
    },
    "DATASETS": [
    {
        "SELF": https://westus2.api.cognitive.microsoft.com/speechtotext/v3.0/datasets/{id}
    }
    ],
    "LINKS": {
    "MANIFEST": "HTTPS://WESTUS2.API.COGNITIVE.MICROSOFT.COM/SPEECHTOTEXT/V3.0/MODELS/{id}/MANIFEST",
    "COPYTO": https://westus2.api.cognitive.microsoft.com/speechtotext/v3.0/models/{id}/copyto
    },
    "PROJECT": {
        "SELF": https://westus2.api.cognitive.microsoft.com/speechtotext/v3.0/projects/{id}
    },
    "PROPERTIES": {
    "DEPRECATIONDATES": {
        "ADAPTATIONDATETIME": "2022-01-15T00:00:00Z",     // last date this model can be used for adaptation
        "TRANSCRIPTIONDATETIME": "2023-03-01T21:27:29Z"   // last date this model can be used for decoding
    }
    },
    "LASTACTIONDATETIME": "2021-03-01T21:27:40Z",
    "STATUS": "SUCCEEDED",
    "CREATEDDATETIME": "2021-03-01T21:27:29Z",
    "LOCALE": "EN-US",
    "DISPLAYNAME": "EXAMPLE MODEL",
    "DESCRIPTION": "",
    "CUSTOMPROPERTIES": {
    "PORTALAPIVERSION": "3"
    }
}
```
Bir özel konuşma uç noktasındaki modeli, konuşma Studio 'nun dağıtım bölümünde uç nokta tarafından kullanılan modeli değiştirerek veya özel konuşma API 'SI aracılığıyla yükseltebileceğinizi unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Modeli eğitme ve dağıtma](how-to-custom-speech-train-model.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Verilerinizi hazırlayın ve test edin](./how-to-custom-speech-test-and-train.md)
* [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)