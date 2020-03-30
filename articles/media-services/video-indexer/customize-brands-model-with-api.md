---
title: Video Indexer API ile Bir Markalar modelini özelleştirme
titleSuffix: Azure Media Services
description: Video Indexer API ile Markalar modelini nasıl özelleştireceğimiz öğrenin.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 01/14/2020
ms.author: anzaman
ms.openlocfilehash: 79c3a7934e9152a4908f895c20ee6fbdc0f360cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128004"
---
# <a name="customize-a-brands-model-with-the-video-indexer-api"></a>Video Indexer API ile Bir Markalar modelini özelleştirme

Video Indexer, video ve ses içeriğinin dizinlenme ve yeniden dizine ekinleme sırasında konuşma ve görsel metinden marka algılamayı destekler. Marka algılama özelliği, Bing'in marka veritabanı tarafından önerilen ürünler, hizmetler ve şirketlerden bahsedildiğini tanımlar. Örneğin, Microsoft video veya ses içeriğinde adı geçiyorsa veya bir videoda görsel metinde görüntülenmişse, Video Indexer içeriği içerikte bir marka olarak algılar. Özel markalar modeli, belirli markaların algılanmamasını hariç tutmanızı ve Bing'in marka veritabanında olmayan modelinizin bir parçası olması gereken markaları eklemenize olanak tanır.

Ayrıntılı bir genel bakış için [bkz.](customize-brands-model-overview.md)

Video Dizinleyici API'lerini, bu konuda açıklandığı gibi, videoda algılanan özel Markalar modellerini oluşturmak, kullanmak ve bunları yeniden oluşturmak için kullanabilirsiniz. [Ayrıca, Video Indexer web sitesini kullanarak Markaları Özelleştir modelinde](customize-brands-model-with-api.md)açıklandığı gibi Video Indexer web sitesini de kullanabilirsiniz.

## <a name="create-a-brand"></a>Marka Oluşturma

[Bir marka API oluşturmak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Create-Brand) yeni bir özel marka oluşturur ve belirtilen hesap için özel Markalar modeline ekler.

> [!NOTE]
> (gövdede) doğru ayar, `enabled` markayı Video Indexer'ın algılaması için *Ekle* listesine koyar. Yanlış `enabled` ayarı, markayı *Dışlama* listesine koyar, böylece Video Indexer bunu algılamaz.

Gövdede ayarlayabildiğiniz diğer bazı parametreler:

* Değer, `referenceUrl` markanın Wikipedia sayfasına bağlantı gibi herhangi bir referans web sitesi olabilir.
* Değer, `tags` markanın etiketlerlistesidir. Bu etiket, Video Indexer web sitesinde markanın *Kategori* alanında gösterilmektedir. Örneğin, "Azure" markası "Bulut" olarak etiketlenebilir veya kategorize edilebilir.

### <a name="response"></a>Yanıt

Yanıt, aşağıdaki örneğin biçimini izleyerek oluşturduğunuz marka hakkında bilgi sağlar.

```json
{
  "referenceUrl": "https://en.wikipedia.org/wiki/Example",
  "id": 97974,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "created": "2018-04-25T14:59:52.7433333",
  "lastModified": "2018-04-25T14:59:52.7433333",
  "enabled": true,
  "description": "This is an example",
  "tags": [
    "Tag1",
    "Tag2"
  ]
}
```

## <a name="delete-a-brand"></a>Marka Silme

[Bir marka](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Brand?) API silme belirtilen hesap için özel Markalar modelinden bir marka kaldırır. Hesap `accountId` parametrede belirtilir. Başarıyla çağrıldıktan sonra, marka artık Marka *Ekle* veya *Hariç* Tut listelerinde yer almayacaktır.

### <a name="response"></a>Yanıt

Marka başarıyla silindiğinde iade edilen içerik yoktur.

## <a name="get-a-specific-brand"></a>Belirli bir Marka alın

Bir marka API [almak,](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brand?) marka kimliğini kullanarak belirtilen hesap için özel Markalar modelinde bir markanın ayrıntılarını aramanızı sağlar.

### <a name="response"></a>Yanıt

Yanıt, aşağıdaki örneğin biçimini izleyerek aradığınız marka (marka kimliğini kullanarak) hakkında bilgi sağlar.

```json
{
  "referenceUrl": "https://en.wikipedia.org/wiki/Example",
  "id": 128846,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "created": "2018-01-06T13:51:38.3666667",
  "lastModified": "2018-01-11T13:51:38.3666667",
  "enabled": true,
  "description": "This is an example",
  "tags": [
    "Tag1",
    "Tag2"
  ]
}
```

> [!NOTE]
> `enabled`markanın `true` *Algılaması* için Video Indexer'ın Ekle listesinde olduğunu belirtmek `enabled` üzere ayarlanmış olması ve yanlış olması markanın *Dışlayanlar* listesinde olduğunu belirtir, böylece Video Indexer bunu algılamaz.

## <a name="update-a-specific-brand"></a>Belirli bir markayı güncelleştirin

Bir [marka](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Brand?) API güncellemesi, marka kimliğini kullanarak belirtilen hesap için özel Markalar modelinde bir markanın ayrıntılarını aramanızı sağlar.

### <a name="response"></a>Yanıt

Yanıt, aşağıdaki örnek biçimi izleyerek güncellediğiniz marka hakkında güncelleştirilmiş bilgileri sağlar.

```json
{
  "referenceUrl": null,
  "id": 97974,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "Created": "2018-04-25T14:59:52.7433333",
  "lastModified": "2018-04-25T15:37:50.67",
  "enabled": false,
  "description": "This is an update example",
  "tags": [
    "Tag1",
    "NewTag2"
  ]
}
```

## <a name="get-all-of-the-brands"></a>Tüm Markaları Alın

[Get all brands](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brands?) API, markanın Marka *Ekle* veya *Hariç* tutma listesinde olması gerekip gerekmediğine bakılmaksızın, belirtilen hesap için özel Markalar modelindeki tüm markaları iade edin.

### <a name="response"></a>Yanıt

Yanıt, hesabınızdaki tüm markaların ve aşağıdaki örneğin biçimini izleyerek ayrıntılarının her birinin bir listesini sağlar.

```json
[
    {
        "ReferenceUrl": null,
        "id": 97974,
        "name": "Example",
        "accountId": "AccountId",
        "lastModifierUserName": "UserName",
        "Created": "2018-04-25T14:59:52.7433333",
        "LastModified": "2018-04-25T14:59:52.7433333",
        "enabled": true,
        "description": "This is an example",
        "tags": ["Tag1", "Tag2"]
    },
    {
        "ReferenceUrl": null,
        "id": 97975,
        "name": "Example2",
        "accountId": "AccountId",
        "lastModifierUserName": "UserName",
        "Created": "2018-04-26T14:59:52.7433333",
        "LastModified": "2018-04-26T14:59:52.7433333",
        "enabled": false,
        "description": "This is another example",
        "tags": ["Tag1", "Tag2"]
    },
]
```

> [!NOTE]
> *Örnek* adlı marka, Video Indexer'ı algılamak için *Ekle* listesinde ve *Örnek2* adlı marka *Dışlayanlar* listesinde olduğundan Video Indexer bunu algılamaz.

## <a name="get-brands-model-settings"></a>Markalar model ayarlarını alın

[Get brands settings](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brands) API, belirtilen hesaptaki Markalar modeli ayarlarını döndürür. Markalar modeli ayarları, Bing markaları veritabanından algılamanın etkin olup olmadığını gösterir. Bing markaları etkin değilse, Video Indexer yalnızca belirtilen hesabın özel Markalar modelinden markaları algılar.

### <a name="response"></a>Yanıt

Yanıt, Bing markalarının aşağıdaki örnekteki biçimiiz izleyerek etkinleştirilip etkinleştirilemediğini gösterir.

```json
{
  "state": true,
  "useBuiltIn": true
}
```

> [!NOTE]
> `useBuiltIn`gerçek olarak ayarlanma, Bing markalarının etkin olduğunu gösterir. `useBuiltin` Yanlışsa, Bing markaları devre dışı bırakılır. `state` Değer, amortismana hazır olduğundan yoksayılabilir.

## <a name="update-brands-model-settings"></a>Markalar model ayarlarını güncelleştirin

[Güncelleştirme markaları](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Brands-Model-Settings?) API, belirtilen hesaptaki Markalar modeli ayarlarını güncelleştirir. Markalar modeli ayarları, Bing markaları veritabanından algılamanın etkin olup olmadığını gösterir. Bing markaları etkin değilse, Video Indexer yalnızca belirtilen hesabın özel Markalar modelinden markaları algılar.

Doğru `useBuiltIn` ayarlanmış bayrak, Bing markalarının etkin olduğu anlamına gelir. `useBuiltin` Yanlışsa, Bing markaları devre dışı bırakılır.

### <a name="response"></a>Yanıt

Markalar modeli ayarı başarıyla güncelleştirildiğinde iade edilen içerik yoktur.

## <a name="next-steps"></a>Sonraki adımlar

[Web sitesini kullanarak Markalar modelini özelleştirin](customize-brands-model-with-website.md)
