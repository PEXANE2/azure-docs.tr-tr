---
title: 'Hızlı başlangıç: bilgi bankasını yönetmek için kıvrımlı & REST kullanın-Soru-Cevap Oluşturma'
description: Bu hızlı başlangıçta, REST API 'Lerini kullanarak bilgi bankasını oluşturma, yayımlama ve sorgulama işlemlerinin nasıl yapılacağı gösterilir.
ms.date: 1/22/2021
ms.topic: quickstart
ms.openlocfilehash: d4c832c4a96ee1419b9e2f0accc1b86e9394ee02
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98701303"
---
[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

* Geçerli [kıvrın](https://curl.haxx.se/)sürümü. Birkaç komut satırı anahtarı, [kıvrımlı belgelerde](https://curl.haxx.se/docs/manpage.html)belirtilen hızlı başlangıçlarda kullanılır.
* Anahtar ve kaynak adını kullanabilmeniz için bir [soru-cevap oluşturma kaynağınız](../How-To/set-up-qnamaker-service-azure.md)olması gerekir. Kaynak oluşturma sırasında kaynak **adı** girdiğinizde, anahtar sizin için oluşturulmuştur. Kaynak adı, uç noktanız için alt etki alanı olarak kullanılır. Anahtarınızı ve kaynak adınızı almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin. Kaynak adı, uç nokta URL 'sinin ilk alt etki alanıdır:

    `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0`

> [!CAUTION]
> Aşağıdaki BASH örnekleri, `\` satır devamlılık karakterini kullanır. Konsol veya Terminal farklı bir satır devamlılık karakteri kullanıyorsa, bu karakteri kullanın.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

* Geçerli [kıvrın](https://curl.haxx.se/)sürümü. Birkaç komut satırı anahtarı, [kıvrımlı belgelerde](https://curl.haxx.se/docs/manpage.html)belirtilen hızlı başlangıçlarda kullanılır.
* Anahtar ve kaynak adını kullanabilmeniz için bir [soru-cevap oluşturma kaynağınız](../How-To/set-up-qnamaker-service-azure.md)olması gerekir. Kaynak oluşturma sırasında kaynak **adı** girdiğinizde, anahtar sizin için oluşturulmuştur. Kaynak adı, uç noktanız için alt etki alanı olarak kullanılır. Anahtarınızı ve kaynak adınızı almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin. Kaynak adı, uç nokta URL 'sinin ilk alt etki alanıdır:

    `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v5.0-preview.1`

> [!CAUTION]
> Aşağıdaki BASH örnekleri, `\` satır devamlılık karakterini kullanır. Konsol veya Terminal farklı bir satır devamlılık karakteri kullanıyorsa, bu karakteri kullanın.

---

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

REST API 'Leri ve kıvrımlı bir Bilgi Bankası oluşturmak için aşağıdaki bilgilere sahip olmanız gerekir:

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi bankasını açıklayan JSON|`-d` larına|JSON [örnekleri](/rest/api/cognitiveservices/qnamaker/knowledgebase/create#examples)|
|JSON boyutunun bayt cinsinden boyutu|`-h``Content-Size`üst bilgi için parametre||

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla, JSON değerlerinizle ve JSON boyutunda düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/create \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-H "Content-Size:107" \
-d '{ name: "QnA Maker FAQ",urls: [ "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs"]}'
```

Soru-Cevap Oluşturma arasındaki kıvrımlı yanıt, `operationId` [işlemin durumunu almak](#get-status-of-operation)için gerekli olan öğesini içerir.

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2020-02-27T04:11:22Z",
  "lastActionTimestamp": "2020-02-27T04:11:22Z",
  "userId": "9596077b3e0441eb93d5080d6a15c64b",
  "operationId": "95a4f700-9899-4c98-bda8-5449af9faef8"
}
```

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)


REST API 'Leri ve kıvrımlı bir Bilgi Bankası oluşturmak için aşağıdaki bilgilere sahip olmanız gerekir:

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi bankasını açıklayan JSON|`-d` larına|JSON [örnekleri](/rest/api/cognitiveservices/qnamaker/knowledgebase/create#examples)|
|JSON boyutunun bayt cinsinden boyutu|`-h``Content-Size`üst bilgi için parametre||

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla, JSON değerlerinizle ve JSON boyutunda düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v5.0-preview.1/knowledgebases/create \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-H "Content-Size:107" \
-d '{ name: "QnA Maker FAQ",urls: [ "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs"]}'
```

Soru-Cevap Oluşturma arasındaki kıvrımlı yanıt, `operationId` [işlemin durumunu almak](#get-status-of-operation)için gerekli olan öğesini içerir.

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2020-02-27T04:11:22Z",
  "lastActionTimestamp": "2020-02-27T04:11:22Z",
  "userId": "9596077b3e0441eb93d5080d6a15c64b",
  "operationId": "95a4f700-9899-4c98-bda8-5449af9faef8"
}
```
---

## <a name="get-status-of-operation"></a>İşlemin durumunu al

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Bir Bilgi Bankası oluşturduğunuzda, işlem zaman uyumsuz olduğundan, yanıt durumu belirlemede bilgiler içerir.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|İşlem kimliği|URL yolu|`/operations/REPLACE-WITH-YOUR-OPERATION-ID`|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve işlem KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/operations/REPLACE-WITH-YOUR-OPERATION-ID \
-X GET \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

Kıvrımlı yanıtı durumu içerir. İşlem durumu başarılı olursa, `resourceLocation` bilgi BANKASı kimliğini de içerir.

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
# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)


Bir Bilgi Bankası oluşturduğunuzda, işlem zaman uyumsuz olduğundan, yanıt durumu belirlemede bilgiler içerir.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|İşlem kimliği|URL yolu|`/operations/REPLACE-WITH-YOUR-OPERATION-ID`|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve işlem KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v5.0-preview.1/operations/REPLACE-WITH-YOUR-OPERATION-ID \
-X GET \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

Kıvrımlı yanıtı durumu içerir. İşlem durumu başarılı olursa, `resourceLocation` bilgi BANKASı kimliğini de içerir.

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

---

## <a name="publish-knowledge-base"></a>Bilgi bankası yayımlama

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Bilgi Bankası 'nı sorgulanmadan önce şunları yapmanız gerekir:
* Bilgi bankası yayımlama
* Çalışma zamanı uç noktası anahtarını al

Bu görev Bilgi Bankası 'nı yayımlar. Çalışma zamanı uç noktası anahtarını alma işlemi [ayrı bir görevdir](#get-published-knowledge-bases-runtime-endpoint-key).

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi Bankası kimliği|URL yolu|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve Bilgi Bankası KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-v \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
--data-raw ''
```

Yanıt durumu, sonuç olmadan 204 ' dir. `-v`Kıvrımlı komutuna ilişkin ayrıntılı çıktıyı görmek için komut satırı parametresini kullanın. Bu, HTTP durumunu içerir.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Bilgi Bankası 'nı sorgulanmadan önce şunları yapmanız gerekir:
* Bilgi bankası yayımlama
* Çalışma zamanı uç noktası anahtarını al

Bu görev Bilgi Bankası 'nı yayımlar. Çalışma zamanı uç noktası anahtarını alma işlemi [ayrı bir görevdir](#get-published-knowledge-bases-runtime-endpoint-key).

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi Bankası kimliği|URL yolu|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve Bilgi Bankası KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v5.0-preview.1/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-v \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
--data-raw ''
```

Yanıt durumu, sonuç olmadan 204 ' dir. `-v`Kıvrımlı komutuna ilişkin ayrıntılı çıktıyı görmek için komut satırı parametresini kullanın. Bu, HTTP durumunu içerir.

---

## <a name="get-published-knowledge-bases-runtime-endpoint-key"></a>Yayımlanan Bilgi Bankası 'nın çalışma zamanı uç noktası anahtarını al

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Bilgi Bankası 'nı sorgulanmadan önce şunları yapmanız gerekir:
* Bilgi bankası yayımlama
* Çalışma zamanı uç noktası anahtarını al

Bu görev, çalışma zamanı uç noktası anahtarını alır. Bilgi bankasını yayımlamak ayrı bir [görevdir](#publish-knowledge-base).

Çalışma zamanı uç noktası anahtarı, Soru-Cevap Oluşturma kaynağını kullanan tüm bilgi tabanları için aynı anahtardır.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|

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
# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Bilgi Bankası 'nı sorgulanmadan önce şunları yapmanız gerekir:
* Bilgi bankası yayımlama
* Çalışma zamanı uç noktası anahtarını al

Bu görev, çalışma zamanı uç noktası anahtarını alır. Bilgi bankasını yayımlamak ayrı bir [görevdir](#publish-knowledge-base).

Çalışma zamanı uç noktası anahtarı, Soru-Cevap Oluşturma kaynağını kullanan tüm bilgi tabanları için aynı anahtardır.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v5.0-preview.1/endpointkeys \
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

---

## <a name="query-for-answer-from-published-knowledge-base"></a>Yayımlanan bilgi bankasından yanıt sorgusu

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Bilgilerden bir yanıt almak, bilgi bankasını yönetmeden ayrı bir çalışma zamanından yapılır. Ayrı bir çalışma zamanı olduğundan, bir çalışma zamanı anahtarıyla kimlik doğrulaması yapmanız gerekir.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma çalışma zamanı anahtarı|`-h``Authorization`üst bilgi için parametre|Anahtar, sözcüğünü içeren bir dizenin parçasıdır `Endpointkey ` . Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi Bankası kimliği|URL yolu|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|
|Sorguyu açıklayan JSON|`-d` larına|[İstek gövdesi parametreleri](/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#request-body) ve JSON [örnekleri](/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#examples)|
|JSON boyutunun bayt cinsinden boyutu|`-h``Content-Size`üst bilgi için parametre||

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

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Bilgilerden bir yanıt almak, bilgi bankasını yönetmeden ayrı bir çalışma zamanından yapılır. Ayrı bir çalışma zamanı olduğundan, bir çalışma zamanı anahtarıyla kimlik doğrulaması yapmanız gerekir.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi Bankası kimliği|URL yolu|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|
|Sorguyu açıklayan JSON|`-d` larına|[İstek gövdesi parametreleri](/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#request-body) ve JSON [örnekleri](/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#examples)|
|JSON boyutunun bayt cinsinden boyutu|`-h``Content-Size`üst bilgi için parametre||

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve Bilgi Bankası KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v5.0-preview.1/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID/generateAnswer \
-X POST \
-H "Content-Type:application/json" \
-H "Content-Size:159" \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
-d '{"question": "How are QnA Maker and LUIS used together?","top": 6,"isTest": true,  "scoreThreshold": 20, "strictFilters": [], "userId": "sd53lsY="}'
```

Başarılı bir yanıt, bir sohbet bot gibi istemci uygulamalarının kullanıcıya yanıt görüntülemesi gereken diğer bilgilerle birlikte en iyi yanıtı içerir.


---

## <a name="delete-knowledge-base"></a>Bilgi bankasını Sil

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Bilgi Bankası ile işiniz bittiğinde silin.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi Bankası kimliği|URL yolu|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve Bilgi Bankası KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-X DELETE \
-v \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

Yanıt durumu, sonuç olmadan 204 ' dir. `-v`Kıvrımlı komutuna ilişkin ayrıntılı çıktıyı görmek için komut satırı parametresini kullanın. Bu, HTTP durumunu içerir.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Bilgi Bankası ile işiniz bittiğinde silin.

|Bilgi|Kıvrımlı yapılandırma|Amaç|
|--|--|--|
|Soru-Cevap Oluşturma kaynak adı|URL|URL oluşturmak için kullanılır|
|Soru-Cevap Oluşturma kaynak anahtarı|`-h``Ocp-Apim-Subscription-Key`üst bilgi için parametre|Soru-Cevap Oluşturma Hizmeti için kimlik doğrulama|
|Bilgi Bankası kimliği|URL yolu|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve Bilgi Bankası KIMLIĞINIZLE düzenleyin.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v5.0-preview.1/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-X DELETE \
-v \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

Yanıt durumu, sonuç olmadan 204 ' dir. `-v`Kıvrımlı komutuna ilişkin ayrıntılı çıktıyı görmek için komut satırı parametresini kullanın. Bu, HTTP durumunu içerir.

---

## <a name="additional-resources"></a>Ek kaynaklar

* [Yazma](/rest/api/cognitiveservices/qnamaker4.0/knowledgebase) Başvuru belgeleri
* [Çalışma zamanı](/rest/api/cognitiveservices/qnamaker4.0/runtime) Başvuru belgeleri
* [Kıvrımlı kullanarak örnek BASH betikleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/curl/QnAMaker)