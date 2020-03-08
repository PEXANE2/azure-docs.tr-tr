---
title: 'Hızlı başlangıç: bilgi bankasını yönetmek için kıvrımlı & REST kullanın-Soru-Cevap Oluşturma'
description: Bu hızlı başlangıçta, REST API 'Lerini kullanarak bilgi bankasını oluşturma, yayımlama ve sorgulama işlemlerinin nasıl yapılacağı gösterilir.
ms.date: 02/27/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: quickstart
ms.openlocfilehash: 00ec52fe20fb0e6a976f3e7142386e835713c98c
ms.sourcegitcommit: f5e4d0466b417fa511b942fd3bd206aeae0055bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78851198"
---
# <a name="quickstart-use-curl-and-rest-to-manage-knowledge-base"></a>Hızlı başlangıç: Bilgi Bankası 'Nı yönetmek için kıvrımlı ve REST kullanma

Bu hızlı başlangıçta bilgi bankasını oluşturma, yayımlama ve sorgulama işlemleri adım adım gösterilmektedir. Soru-Cevap Oluşturma, [veri kaynaklarından](../Concepts/knowledge-base.md) ve SSS gibi yarı yapılandırılmış içerikten soru ve cevapları otomatik olarak ayıklar. JSON ile tanımlanan bilgi bankası modeli API isteğinin gövdesinde gönderilir.

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Geçerli [kıvrın](https://curl.haxx.se/)sürümü. Birkaç komut satırı anahtarı, [kıvrımlı belgelerde](https://curl.haxx.se/docs/manpage.html)belirtilen hızlı başlangıçlarda kullanılır.
* Bir [soru-cevap oluşturma kaynağınız](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı ve kaynak adınızı almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin. Kaynak adı, uç nokta URL 'sinin ilk bölümüdür:

    `https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0`

> [!CAUTION]
> Aşağıdaki BASH örnekleri `\` satırı devamlılık karakterini kullanır. Konsol veya Terminal farklı bir satır devamlılık karakteri kullanıyorsa, bu karakteri kullanın.

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

REST API 'Leri ve kıvrımlı bir Bilgi Bankası oluşturmak için aşağıdaki bilgilere sahip olmanız gerekir:

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL'si|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`Ocp-Apim-Subscription-Key` üst bilgisi için `-h` parametresi|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi bankasını açıklayan JSON|`-d` param|JSON [örnekleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create#examples)|
|JSON boyutunun bayt cinsinden boyutu|`Content-Size` üst bilgisi için `-h` parametresi||

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla, JSON değerlerinizle ve JSON boyutunda düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/create \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-H "Content-Size:107" \
-d '{ name: "QnA Maker FAQ",urls: [ "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs"]}'
```

Soru-Cevap Oluşturma arasındaki kıvrımlı yanıt, [işlemin durumunu almak](#get-status-of-operation)için gerekli olan `operationId` içerir.

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2020-02-27T04:11:22Z",
  "lastActionTimestamp": "2020-02-27T04:11:22Z",
  "userId": "9596077b3e0441eb93d5080d6a15c64b",
  "operationId": "95a4f700-9899-4c98-bda8-5449af9faef8"
}
```

## <a name="get-status-of-operation"></a>İşlemin durumunu al

Bir Bilgi Bankası oluşturduğunuzda, işlem zaman uyumsuz olduğundan, yanıt durumu belirlemede bilgiler içerir.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL'si|URL oluşturmak için kullanılır|
|İşlem kimliği|URL yolu|`/operations/REPLACE-WITH-YOUR-OPERATION-ID`|
|Soru-Cevap Oluşturma kaynak anahtarı|`Ocp-Apim-Subscription-Key` üst bilgisi için `-h` parametresi|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve işlem KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/operations/REPLACE-WITH-YOUR-OPERATION-ID \
-X GET \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

Kıvrımlı yanıtı durumu içerir. İşlem durumu başarılı olursa `resourceLocation` Bilgi Bankası KIMLIĞINI içerir.

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

Bilgi Bankası 'nı sorgulanmadan önce şunları yapmanız gerekir:
* Bilgi bankası yayımlama
* Çalışma zamanı uç noktası anahtarını al

Bu görev Bilgi Bankası 'nı yayımlar. Çalışma zamanı uç noktası anahtarını alma işlemi [ayrı bir görevdir](#get-published-knowledge-bases-runtime-endpoint-key).

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL'si|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`Ocp-Apim-Subscription-Key` üst bilgisi için `-h` parametresi|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi Bankası kimliği|URL yolu|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve Bilgi Bankası KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-v \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
--data-raw ''
```

Yanıt durumu, sonuç olmadan 204 ' dir. Kıvrımlı komutuna ilişkin ayrıntılı çıktıyı görmek için `-v` komut satırı parametresini kullanın. Bu, HTTP durumunu içerir.

## <a name="get-published-knowledge-bases-runtime-endpoint-key"></a>Yayımlanan Bilgi Bankası 'nın çalışma zamanı uç noktası anahtarını al

Bilgi Bankası 'nı sorgulanmadan önce şunları yapmanız gerekir:
* Bilgi bankası yayımlama
* Çalışma zamanı uç noktası anahtarını al

Bu görev, çalışma zamanı uç noktası anahtarını alır. Bilgi bankasını yayımlamak ayrı bir [görevdir](#publish-knowledge-base).

Çalışma zamanı uç noktası anahtarı, Soru-Cevap Oluşturma kaynağını kullanan tüm bilgi tabanları için aynı anahtardır.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL'si|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`Ocp-Apim-Subscription-Key` üst bilgisi için `-h` parametresi|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/endpointkeys \
-X GET \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```


Kıvrımlı yanıtı, çalışma zamanı uç noktası anahtarlarını içerir. Bilgi bankasından yanıt almak için sorgulama yaparken anahtarlardan yalnızca birini kullanın.

```json
{
  "primaryEndpointKey": "93e88a14-694a-44d5-883b-184a68aa8530",
  "secondaryEndpointKey": "92c98c16-ca31-4294-8626-6c57454a5063",
  "installedVersion": "4.0.5",
  "lastStableVersion": "4.0.6"
}
```

## <a name="query-for-answer-from-published-knowledge-base"></a>Yayımlanan bilgi bankasından yanıt sorgusu

Bilgilerden bir yanıt almak, bilgi bankasını yönetmeden ayrı bir çalışma zamanından yapılır. Ayrı bir çalışma zamanı olduğundan, bir çalışma zamanı anahtarıyla kimlik doğrulaması yapmanız gerekir.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL'si|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma çalışma zamanı anahtarı|`Authorization` üst bilgisi için `-h` parametresi|Anahtar, `Endpointkey `sözcüğünü içeren bir dizenin parçasıdır. Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi Bankası kimliği|URL yolu|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|
|Sorguyu açıklayan JSON|`-d` param|[İstek gövdesi parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#request-body) ve JSON [örnekleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#examples)|
|JSON boyutunun bayt cinsinden boyutu|`Content-Size` üst bilgisi için `-h` parametresi||

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve Bilgi Bankası KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.azurewebsites.net/qnamaker/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID/generateAnswer \
-X POST \
-H "Authorization: EndpointKey REPLACE-WITH-YOUR-RUNTIME-KEY" \
-H "Content-Type:application/json" \
-H "Content-Size:159" \
-d '{"question": "How are QnA Maker and LUIS used together?","top": 6,"isTest": true,  "scoreThreshold": 20, "strictFilters": [], "userId": "sd53lsY="}'
```

Başarılı bir yanıt, bir sohbet bot gibi istemci uygulamalarının kullanıcıya yanıt görüntülemesi gereken diğer bilgilerle birlikte en iyi yanıtı içerir.

## <a name="delete-knowledge-base"></a>Bilgi bankasını Sil

Bilgi Bankası ile işiniz bittiğinde silin.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL'si|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`Ocp-Apim-Subscription-Key` üst bilgisi için `-h` parametresi|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi Bankası kimliği|URL yolu|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve Bilgi Bankası KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-X DELETE \
-v \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

Yanıt durumu, sonuç olmadan 204 ' dir. Kıvrımlı komutuna ilişkin ayrıntılı çıktıyı görmek için `-v` komut satırı parametresini kullanın. Bu, HTTP durumunu içerir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Yazma](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase) Başvuru belgeleri
* [Çalışma zamanı](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/) Başvuru belgeleri
* [Kıvrımlı kullanarak örnek BASH betikleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/curl/QnAMaker)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
