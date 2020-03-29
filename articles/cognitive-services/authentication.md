---
title: Kimlik doğrulaması
titleSuffix: Azure Cognitive Services
description: 'Azure Bilişsel Hizmetler kaynağındaki isteğin kimliğini doğrulamanın üç yolu vardır: abonelik anahtarı, taşıyıcı belirteci veya çok hizmetli abonelik. Bu makalede, her yöntem ve nasıl bir istek teshisi hakkında bilgi edineceksiniz.'
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 11/22/2019
ms.author: erhopf
ms.openlocfilehash: 1c13c2cc4d4e562d3512de90338d874091dfeef6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74423932"
---
# <a name="authenticate-requests-to-azure-cognitive-services"></a>Azure Bilişsel Hizmetleri'ndeki istekleri doğrulama

Azure Bilişsel Hizmeti'ne yapılan her istek bir kimlik doğrulama üstbilgisini içermelidir. Bu üstbilgi, bir hizmet veya hizmet grubu aboneliğinizi doğrulamak için kullanılan bir abonelik anahtarı veya erişim jetonu boyunca geçer. Bu makalede, bir isteği doğrulamanın üç yolu ve her biri için gereksinimleri öğreneceksiniz.

* [Tek hizmet li abonelik anahtarıyla kimlik doğrulaması](#authenticate-with-a-single-service-subscription-key)
* [Çok hizmetli abonelik anahtarıyla kimlik doğrulaması](#authenticate-with-a-multi-service-subscription-key)
* [Belirteçle kimlik doğrulaması](#authenticate-with-an-authentication-token)
* [Azure Etkin Dizin (AAD) ile kimlik doğrulaması](#authenticate-with-azure-active-directory)

## <a name="prerequisites"></a>Ön koşullar

İstekte bulunmadan önce bir Azure hesabı ve Azure Bilişsel Hizmetler aboneliğine ihtiyacınız var. Zaten bir hesabınız varsa, devam edin ve bir sonraki bölüme atlayın. Hesabınız yoksa, dakikalar içinde kurulum yaptırmanız için bir kılavuzumuz var: [Azure için Bir Bilişsel Hizmetler hesabı oluşturun.](cognitive-services-apis-create-account.md)

Hesabınızı oluşturduktan veya [ücretsiz](https://azure.microsoft.com/try/cognitive-services/my-apis)deneme sürümünü etkinleştirdikten sonra abonelik anahtarınızı [Azure portalından](cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) alabilirsiniz.

## <a name="authentication-headers"></a>Kimlik doğrulama üstbilgi

Azure Bilişsel Hizmetleri ile kullanılabilen kimlik doğrulama üstaylarını hızla gözden geçirelim.

| Üst bilgi | Açıklama |
|--------|-------------|
| Ocp-Apim-Subscription-Key | Belirli bir hizmet veya çok hizmetli abonelik anahtarı için bir abonelik anahtarıyla kimlik doğrulaması yapmak için bu üstbilgiyi kullanın. |
| Ocp-Apim-Abonelik-Bölge | Bu üstbilgi yalnızca [Çevirmen Metin API'si](./Translator/reference/v3-0-reference.md)ile çok hizmetli bir abonelik anahtarı kullanırken gereklidir. Abonelik bölgesini belirtmek için bu üstbilgikullanın. |
| Yetkilendirme | Kimlik doğrulama belirteci kullanıyorsanız bu üstbilgikullanın. Belirteç değişimi gerçekleştirmek için adımlar aşağıdaki bölümlerde ayrıntılı olarak açıklanır. Sağlanan değer aşağıdaki biçimi `Bearer <TOKEN>`izler: . |

## <a name="authenticate-with-a-single-service-subscription-key"></a>Tek hizmet li abonelik anahtarıyla kimlik doğrulaması

İlk seçenek, Çevirmen Metni gibi belirli bir hizmet için abonelik anahtarıyla bir isteğin kimliğini doğrulamaktır. Anahtarlar, oluşturduğunuz her kaynak için Azure portalında kullanılabilir. Bir isteğin kimliğini doğrulamak için abonelik anahtarını kullanmak için `Ocp-Apim-Subscription-Key` üstbilgi olarak geçirilmesi gerekir.

Bu örnek istekleri üstbilginin `Ocp-Apim-Subscription-Key` nasıl kullanılacağını gösterir. Bu örneği kullanırken geçerli bir abonelik anahtarı eklemeniz gerektiğini unutmayın.

Bu Bing Web Arama API için örnek bir çağrı:
```cURL
curl -X GET 'https://api.cognitive.microsoft.com/bing/v7.0/search?q=Welsch%20Pembroke%20Corgis' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' | json_pp
```

Bu, Çevirmen Metin API'sine yapılan örnek bir çağrıdır:
```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

Aşağıdaki video, Bilişsel Hizmetler anahtarının kullanılmasını göstermektedir.

## <a name="authenticate-with-a-multi-service-subscription-key"></a>Çok hizmetli abonelik anahtarıyla kimlik doğrulaması

>[!WARNING]
> Şu anda, bu hizmetler çoklu hizmet tuşlarını **desteklemez:** QnA Maker, Konuşma Hizmetleri, Özel Görüş ve Anomali Dedektörü.

Bu seçenek, istekleri doğrulamak için bir abonelik anahtarı da kullanır. Temel fark, bir abonelik anahtarının belirli bir hizmete bağlı olmaması, bunun yerine, birden çok Bilişsel Hizmet isteklerini doğrulamak için tek bir anahtarın kullanılabilmesidir. Bölgesel kullanılabilirlik, desteklenen özellikler ve fiyatlandırma hakkında bilgi için [Bilişsel Hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/) ile ilgili bilgilere bakın.

Abonelik anahtarı her istekte `Ocp-Apim-Subscription-Key` üstbilgi olarak sağlanır.

[![Bilişsel Hizmetler için çoklu hizmet abonelik anahtar gösterimi](./media/index/single-key-demonstration-video.png)](https://www.youtube.com/watch?v=psHtA1p7Cas&feature=youtu.be)

### <a name="supported-regions"></a>Desteklenen bölgeler

Çok hizmetli abonelik anahtarını kullanarak istekte bulunmanız `api.cognitive.microsoft.com`gerekir, bölgeyi URL'ye eklemeniz gerekir. Örneğin: `westus.api.cognitive.microsoft.com`.

Çevirmen Metin API ile çok hizmetli abonelik anahtarını kullanırken, `Ocp-Apim-Subscription-Region` üstbilgiyle abonelik bölgesini belirtmeniz gerekir.

Çok hizmetli kimlik doğrulama bu bölgelerde desteklenir:

| | | |
|-|-|-|
| `australiaeast` | `brazilsouth` | `canadacentral` |
| `centralindia` | `eastasia` | `eastus` |
| `japaneast` | `northeurope` | `southcentralus` |
| `southeastasia` | `uksouth` | `westcentralus` |
| `westeurope` | `westus` | `westus2` |


### <a name="sample-requests"></a>Örnek istekler

Bu Bing Web Arama API için örnek bir çağrı:

```cURL
curl -X GET 'https://YOUR-REGION.api.cognitive.microsoft.com/bing/v7.0/search?q=Welsch%20Pembroke%20Corgis' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' | json_pp
```

Bu, Çevirmen Metin API'sine yapılan örnek bir çağrıdır:

```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Ocp-Apim-Subscription-Region: YOUR_SUBSCRIPTION_REGION' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="authenticate-with-an-authentication-token"></a>Kimlik doğrulama belirteciyle kimlik doğrulama

Bazı Azure Bilişsel Hizmetleri, kimlik doğrulama belirteci kabul ve bazı durumlarda gerektirir. Şu anda, bu hizmetler kimlik doğrulama belirteçlerini destekler:

* Metin Çevirisi API
* Konuşma Hizmetleri: Konuşma-metin REST API
* Konuşma Hizmetleri: Metinden konuşmaya REST API

>[!NOTE]
> QnA Maker yetkilendirme üstbilgisini de kullanır, ancak bir bitiş noktası anahtarı gerektirir. Daha fazla bilgi için [QnA Maker: Bilgi tabanından yanıt alın.](./qnamaker/quickstarts/get-answer-from-knowledge-base-using-url-tool.md)

>[!WARNING]
> Kimlik doğrulama belirteçlerini destekleyen hizmetler zaman içinde değişebilir, lütfen bu kimlik doğrulama yöntemini kullanmadan önce bir hizmet için API başvurularını denetleyin.

Hem tek hizmet hem de çok hizmetli abonelik anahtarları kimlik doğrulama belirteçleri ile değiştirilebilir. Kimlik doğrulama belirteçleri 10 dakika süreyle geçerlidir.

Kimlik doğrulama belirteçleri `Authorization` üstbilgi olarak bir istek dahildir. Sağlanan belirteç değeri, örneğin `Bearer`aşağıdakilerden `Bearer YOUR_AUTH_TOKEN`önce olmalıdır: .

### <a name="sample-requests"></a>Örnek istekler

Bir abonelik anahtarını kimlik doğrulama belirteci `https://YOUR-REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken`yle değiştirmek için bu URL'yi kullanın: .

```cURL
curl -v -X POST \
"https://YOUR-REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
-H "Content-type: application/x-www-form-urlencoded" \
-H "Content-length: 0" \
-H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

Bu çok hizmet li bölgeler belirteç değişimini destekler:

| | | |
|-|-|-|
| `australiaeast` | `brazilsouth` | `canadacentral` |
| `centralindia` | `eastasia` | `eastus` |
| `japaneast` | `northeurope` | `southcentralus` |
| `southeastasia` | `uksouth` | `westcentralus` |
| `westeurope` | `westus` | `westus2` |

Kimlik doğrulama belirteci aldıktan sonra, her istekte üstbilgi `Authorization` olarak geçirmeniz gerekir. Bu, Çevirmen Metin API'sine yapılan örnek bir çağrıdır:

```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Authorization: Bearer YOUR_AUTH_TOKEN' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

[!INCLUDE [](../../includes/cognitive-services-azure-active-directory-authentication.md)]

## <a name="see-also"></a>Ayrıca bkz.

* [Bilişsel Hizmetler nedir?](welcome.md)
* [Bilişsel Hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/)
* [Özel alt etki alanları](cognitive-services-custom-subdomains.md)
