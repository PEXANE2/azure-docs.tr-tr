---
title: "Quickstart: Bilgi tabanını yönetmek için cURL & REST'i kullanın - QnA Maker"
description: Bu hızlı başlangıç, REST API'lerini kullanarak bilgi tabanınızı nasıl oluşturabileceğinizi, yayımlayacağınızı ve sorgulayacağınızı gösterir.
ms.date: 04/13/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: quickstart
ms.openlocfilehash: facc45ab8f916181f7eeceb65c5102a60ae7d7e9
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261712"
---
# <a name="quickstart-use-curl-and-rest-to-manage-knowledge-base"></a>Quickstart: Bilgi tabanını yönetmek için cURL ve REST'i kullanın

Bu hızlı başlangıç, bilgi tabanınızı oluşturma, yayımlama ve sorgulama konusunda size yol gösterir. Soru-Cevap Oluşturma, [veri kaynaklarından](../Concepts/knowledge-base.md) ve SSS gibi yarı yapılandırılmış içerikten soru ve cevapları otomatik olarak ayıklar. JSON ile tanımlanan bilgi bankası modeli API isteğinin gövdesinde gönderilir.

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [cURL](https://curl.haxx.se/)geçerli sürümü . [CURL belgelerinde](https://curl.haxx.se/docs/manpage.html)belirtilen quickstarts'da çeşitli komut satırı anahtarları kullanılır.
* Anahtar ve kaynak adını kullanmak için bir [QnA Maker kaynağına](../How-To/set-up-qnamaker-service-azure.md)sahip olmalısınız. Kaynak oluşturma sırasında kaynak **adı** girdiniz, ardından anahtar sizin için oluşturuldu. Kaynak adı bitiş noktanızın alt etki alanı olarak kullanılır. Anahtar ve kaynak adınızı almak için Azure portalındaki kaynağınız için **Hızlı Başlat'ı** seçin. Kaynak adı bitiş noktası URL'sinin ilk alt etki alanıdır:

    `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0`

> [!CAUTION]
> Aşağıdaki BASH örnekleri `\` satır devamı karakterini kullanır. Konsol veya terminal farklı bir satır devam karakteri kullanıyorsa, bu karakteri kullanın.

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

REST API'leri ve cURL'si ile bir bilgi tabanı oluşturmak için aşağıdaki bilgilere sahip olmanız gerekir:

|Bilgi|cURL yapılandırması|Amaç|
|--|--|--|
|QnA Maker kaynak adı|URL'si|URL oluşturmak için kullanılan|
|QnA Maker kaynak anahtarı|`-h`üstbilgi `Ocp-Apim-Subscription-Key` için param|QnA Maker hizmetine kimlik doğrulaması|
|JSON bilgi tabanını tanımlıyor|`-d`param|JSON [örnekleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create#examples)|
|Baytlarda JSON boyutu|`-h`üstbilgi `Content-Size` için param||

CURL komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınız, kaynak anahtarınız ve JSON değerleriniz ve JSON boyutuyla edin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/create \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-H "Content-Size:107" \
-d '{ name: "QnA Maker FAQ",urls: [ "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs"]}'
```

QnA Maker cURL yanıtı `operationId` içerir , [operasyonun durumunu almak](#get-status-of-operation)için gerekli olan .

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2020-02-27T04:11:22Z",
  "lastActionTimestamp": "2020-02-27T04:11:22Z",
  "userId": "9596077b3e0441eb93d5080d6a15c64b",
  "operationId": "95a4f700-9899-4c98-bda8-5449af9faef8"
}
```

## <a name="get-status-of-operation"></a>Operasyonun durumunu alın

Bir bilgi tabanı oluşturduğunuzda, işlem eşit olduğundan, yanıt durumu belirlemek için bilgiler içerir.

|Bilgi|cURL yapılandırması|Amaç|
|--|--|--|
|QnA Maker kaynak adı|URL'si|URL oluşturmak için kullanılan|
|Operasyon Kimliği|URL rotası|`/operations/REPLACE-WITH-YOUR-OPERATION-ID`|
|QnA Maker kaynak anahtarı|`-h`üstbilgi `Ocp-Apim-Subscription-Key` için param|QnA Maker hizmetine kimlik doğrulaması|

CURL komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınız, kaynak anahtarınız ve çalışma kimliğinizle birlikte edin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/operations/REPLACE-WITH-YOUR-OPERATION-ID \
-X GET \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

CURL yanıtı durumu içerir. İşlem durumu başarılı olursa, bilgi `resourceLocation` bankası kimliği içerir.

```json
{
   "operationState": "Succeeded",
   "createdTimestamp": "2020-02-27T04:54:07Z",
   "lastActionTimestamp": "2020-02-27T04:54:19Z",
   "resourceLocation": "/knowledgebases/fe3971b7-cfaa-41fa-8d9f-6ceb673eb865",
   "userId": "f596077b3e0441eb93d5080d6a15c64b",
   "operationId": "f293f218-d080-48f0-a766-47993e9b26a8"
}
```

## <a name="publish-knowledge-base"></a>Bilgi bankası yayımlama

Bilgi tabanını sorgulamadan önce şunları yapmanız gerekir:
* Bilgi bankası yayımlama
* Çalışma zamanı bitiş noktası anahtarını alın

Bu görev bilgi tabanını yayımlar. Çalışma zamanı bitiş noktası anahtarını almak ayrı bir [görevdir.](#get-published-knowledge-bases-runtime-endpoint-key)

|Bilgi|cURL yapılandırması|Amaç|
|--|--|--|
|QnA Maker kaynak adı|URL'si|URL oluşturmak için kullanılan|
|QnA Maker kaynak anahtarı|`-h`üstbilgi `Ocp-Apim-Subscription-Key` için param|QnA Maker hizmetine kimlik doğrulaması|
|Bilgi bankası Kimliği|URL rotası|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

CURL komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınız, kaynak anahtarınız ve bilgi bankası kimliğinizle birlikte edin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-v \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
--data-raw ''
```

Yanıt durumu 204 ve sonuç yok. cURL `-v` komutu için ayrıntılı çıktıgörmek için komut satırı parametresini kullanın. Bu, HTTP durumunu içerir.

## <a name="get-published-knowledge-bases-runtime-endpoint-key"></a>Yayınlanmış bilgi tabanının çalışma zamanı bitiş noktası anahtarını alın

Bilgi tabanını sorgulamadan önce şunları yapmanız gerekir:
* Bilgi bankası yayımlama
* Çalışma zamanı bitiş noktası anahtarını alın

Bu görev, çalışma zamanı bitiş noktası anahtarını alır. Bilgi tabanını yayımlama ayrı bir [görevdir.](#publish-knowledge-base)

Çalışma süresi bitiş noktası anahtarı, QnA Maker kaynağını kullanan tüm bilgi tabanları için aynı anahtardır.

|Bilgi|cURL yapılandırması|Amaç|
|--|--|--|
|QnA Maker kaynak adı|URL'si|URL oluşturmak için kullanılan|
|QnA Maker kaynak anahtarı|`-h`üstbilgi `Ocp-Apim-Subscription-Key` için param|QnA Maker hizmetine kimlik doğrulaması|

CURL komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınız, kaynak anahtarınızla edin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/endpointkeys \
-X GET \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```


cURL yanıtı çalışma zamanı bitiş noktası anahtarlarını içerir. Bilgi tabanından bir yanıt almak için sorgu yaparken anahtarlardan sadece birini kullanın.

```json
{
  "primaryEndpointKey": "93e88a14-694a-44d5-883b-184a68aa8530",
  "secondaryEndpointKey": "92c98c16-ca31-4294-8626-6c57454a5063",
  "installedVersion": "4.0.5",
  "lastStableVersion": "4.0.6"
}
```

## <a name="query-for-answer-from-published-knowledge-base"></a>Yayınlanan bilgi tabanından yanıt için sorgu

Bilgiden yanıt almak, bilgi tabanını yönetmekten çok ayrı bir çalışma zamanından yapılır. Ayrı bir çalışma zamanı olduğundan, çalışma zamanı anahtarıyla kimlik doğrulamanız gerekir.

|Bilgi|cURL yapılandırması|Amaç|
|--|--|--|
|QnA Maker kaynak adı|URL'si|URL oluşturmak için kullanılan|
|QnA Maker çalışma zamanı anahtarı|`-h`üstbilgi `Authorization` için param|Anahtar, sözcüğü `Endpointkey `içeren bir dize parçasıdır. QnA Maker hizmetine kimlik doğrulaması|
|Bilgi bankası Kimliği|URL rotası|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|
|JSON sorgu açıklayan|`-d`param|[İstek vücut parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#request-body) ve JSON [örnekleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#examples)|
|Baytlarda JSON boyutu|`-h`üstbilgi `Content-Size` için param||

CURL komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınız, kaynak anahtarınız ve bilgi bankası kimliğinizle birlikte edin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.azurewebsites.net/qnamaker/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID/generateAnswer \
-X POST \
-H "Authorization: EndpointKey REPLACE-WITH-YOUR-RUNTIME-KEY" \
-H "Content-Type:application/json" \
-H "Content-Size:159" \
-d '{"question": "How are QnA Maker and LUIS used together?","top": 6,"isTest": true,  "scoreThreshold": 20, "strictFilters": [], "userId": "sd53lsY="}'
```

Başarılı bir yanıt, bir istemci uygulamasının kullanıcıya yanıt göstermesi gereken diğer bilgilerle birlikte en üst yanıtı içerir.

## <a name="delete-knowledge-base"></a>Bilgi tabanını silme

Bilgi tabanı ile bittiğinde, silin.

|Bilgi|cURL yapılandırması|Amaç|
|--|--|--|
|QnA Maker kaynak adı|URL'si|URL oluşturmak için kullanılan|
|QnA Maker kaynak anahtarı|`-h`üstbilgi `Ocp-Apim-Subscription-Key` için param|QnA Maker hizmetine kimlik doğrulaması|
|Bilgi bankası Kimliği|URL rotası|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

CURL komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınız, kaynak anahtarınız ve bilgi bankası kimliğinizle birlikte edin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-X DELETE \
-v \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

Yanıt durumu 204 ve sonuç yok. cURL `-v` komutu için ayrıntılı çıktıgörmek için komut satırı parametresini kullanın. Bu, HTTP durumunu içerir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Yazma](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase) Referans belgeleri
* [Çalışma zamanı](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/) Referans belgeleri
* [CURL kullanarak örnek BASH komut dosyaları](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/curl/QnAMaker)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
