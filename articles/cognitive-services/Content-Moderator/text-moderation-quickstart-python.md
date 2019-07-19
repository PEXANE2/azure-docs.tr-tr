---
title: "Hızlı Başlangıç: Python 'da metin içeriğini analiz etme-Content Moderator"
titlesuffix: Azure Cognitive Services
description: Metin içeriği için içerik Moderator SDK'sını kullanarak Python için çeşitli içeriklere analiz etme
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 01c153f2f8836b7d99de57af60b8623e54c6d6fe
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311920"
---
[!code-python[import declarations](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=imports)]

# <a name="quickstart-analyze-text-content-for-objectionable-material-in-python"></a>Hızlı Başlangıç: Python 'da sakıncalı malzeme için metin içeriğini analiz etme

Bu makalede bilgiler sağlar ve yardımcı olması için kod örnekleri Content Moderator SDK'sı için Python'ı kullanmaya başlayın. Uygunsuz olabilecek malzemeleri yönetmek için terim tabanlı filtreleme ve metin içeriğini sınıflandırma işlemlerini yürütmeyi öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar
- Content Moderator abonelik anahtarı. Content Moderator'a abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergelerini izleyin.
- [Python 2.7+ veya 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/) aracı

## <a name="get-the-content-moderator-sdk"></a>Content Moderator SDK'sı Al

Content Moderator Python SDK'sı, bir komut istemini açın ve aşağıdaki komutu çalıştırarak yükleyin:

```bash
pip install azure-cognitiveservices-vision-contentmoderator
```

## <a name="import-modules"></a>Modülleri içeri aktarma

Adlı yeni bir Python betiği oluşturmak _ContentModeratorQS.py_ ve SDK'yı gerekli bölümlerini içeri aktarmak için aşağıdaki kodu ekleyin. Düzgün yazdırma modülü, JSON yanıtının okunması daha kolay bir hale gelir.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=imports)]


## <a name="initialize-variables"></a>Değişkenleri başlatma

Ardından, değişkenleri, Content Moderator abonelik anahtarını ve uç nokta URL'sini ekleyin. Adı `CONTENT_MODERATOR_SUBSCRIPTION_KEY` ortam değişkenlerinizin içine eklemeniz ve abonelik anahtarınızı değer olarak eklemeniz gerekir. Temel uç nokta URL 'niz için, `CONTENT_MODERATOR_ENDPOINT` kendi değeri olarak bölge özel URL 'niz ile ortam değişkenlerinizi ekleyin (örneğin `https://westus.api.cognitive.microsoft.com`,). Ücretsiz deneme aboneliği anahtarları oluşturulur **westus** bölge.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=authentication)]

Bir dosyadan çok satırlı metnin bir dizesi aracılı olur. [Content_moderator_text_moderation. txt](https://github.com/Azure-Samples/cognitive-services-content-moderator-samples/blob/master/documentation-samples/python/content_moderator_text_moderation.txt) dosyasını yerel kök klasörünüze ekleyin ve dosya adını değişkenlerinizin içine ekleyin:

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=textModerationFile)]

## <a name="query-the-moderator-service"></a>Denetleyici hizmetini sorgulama

Oluşturma bir **ContentModeratorClient** abonelik anahtarını ve uç nokta URL'nizi kullanarak örneği. 

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=client)]

Daha sonra, işleviyle `screen_text`birlikte denetleme API 'sini çağırmak için, kendi üye **textmoderationoperations** örneğiyle istemcinizi kullanın. Bkz: **[screen_text](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.textmoderationoperations?view=azure-python)** başvuru belgeleri çağırmak nasıl daha fazla bilgi için.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=textModeration)]

## <a name="check-the-printed-response"></a>Yazdırılan yanıtı denetleyin

Örneği çalıştırın ve yanıtı onaylayın. Başarıyla tamamlanmalıdır ve bir **ekran** örneği döndürdü. Başarılı bir sonuç aşağıda yazdırılır:

Bu hızlı başlangıçta sonuçları aşağıdaki çıktıda kullanılan örnek metni:

```console
{'auto_corrected_text': '" Is this a garbage email abide@ abed. com, phone: '
                        '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                        'Redmond, WA 98052. Crap is the profanity here. Is '
                        'this information PII? phone 3144444444\\ n"',
 'classification': {'category1': {'score': 0.00025233393535017967},
                    'category2': {'score': 0.18468093872070312},
                    'category3': {'score': 0.9879999756813049},
                    'review_recommended': True},
 'language': 'eng',
 'normalized_text': '" Is this a garbage email abide@ abed. com, phone: '
                    '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                    'Redmond, WA 98052. Crap is the profanity here. Is this '
                    'information PII? phone 3144444444\\ n"',
 'original_text': '"Is this a grabage email abcdef@abcd.com, phone: '
                  '6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, '
                  'WA 98052. Crap is the profanity here. Is this information '
                  'PII? phone 3144444444\\n"',
 'pii': {'address': [{'index': 82,
                      'text': '1 Microsoft Way, Redmond, WA 98052'}],
         'email': [{'detected': 'abcdef@abcd.com',
                    'index': 25,
                    'sub_type': 'Regular',
                    'text': 'abcdef@abcd.com'}],
         'ipa': [{'index': 65, 'sub_type': 'IPV4', 'text': '255.255.255.255'}],
         'phone': [{'country_code': 'US', 'index': 49, 'text': '6657789887'},
                   {'country_code': 'US', 'index': 177, 'text': '3144444444'}]},
 'status': {'code': 3000, 'description': 'OK'},
 'terms': [{'index': 118, 'list_id': 0, 'original_index': 118, 'term': 'crap'}],
 'tracking_id': 'b253515c-e713-4316-a016-8397662a3f1a'}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, verilen metni örnek ilgili bilgileri döndürmek için Content Moderator hizmet kullanan basit bir Python betiği geliştirdik. Bundan sonra, hangi verilere ihtiyacınız olduğuna ve uygulamanızın bunları nasıl işleyeceğine karar verebilmek için farklı bayrakların ve sınıflandırmaların anlamları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Metin moderasyonu kılavuzu](text-moderation-api.md)
