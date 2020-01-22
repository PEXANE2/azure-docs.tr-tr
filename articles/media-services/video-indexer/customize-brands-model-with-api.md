---
title: Markalar modelini özelleştirmek için Azure Video Indexer kullanma
titleSuffix: Azure Media Services
description: Bu makalede, Azure Video Indexer markaların modelini özelleştirmek için nasıl kullanılacağı gösterilmektedir.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 01/14/2020
ms.author: anzaman
ms.openlocfilehash: 81ba4cc7be5f9361d21aaea2ba78d0fd6f0f8c95
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76289926"
---
# <a name="customize-a-brands-model-with-the-video-indexer-api"></a>Video Indexer API ile bir markalar modeli özelleştirme

Video Indexer, video ve ses içeriğinin dizin oluşturma ve yeniden dizin oluşturma sırasında konuşma ve görsel metinden marka algılamayı destekler. Marka algılama özelliği, Bing 'ün markalar veritabanı tarafından önerilen ürünlerin, hizmetlerin ve şirketlerin bahsetmelerini belirler. Örneğin, Microsoft bir video veya ses içeriğinde bahsedildiğinde veya videoda görsel metin gösteriyorsa, Video Indexer içeriği bir marka olarak algılar. Özel bir markalar modeli, belirli markaların algılanmadan dışlanmasını ve kuruluşunuzun markalar veritabanında bulunmayabilir, modelinizin bir parçası olması gereken markalar dahil etmenizi sağlar.

Ayrıntılı bir genel bakış için bkz. [genel bakış](customize-brands-model-overview.md).

Bu konuda açıklandığı gibi, videoda algılanan özel markalar modellerini oluşturmak, kullanmak ve düzenlemek için Video Indexer API 'Lerini kullanabilirsiniz. Ayrıca, [video Indexer Web sitesini kullanarak markalar modelini özelleştirme](customize-brands-model-with-api.md)bölümünde açıklandığı gibi video Indexer Web sitesini de kullanabilirsiniz.

## <a name="create-a-brand"></a>Marka oluşturma

Marka API 'si [oluşturma](https://api-portal.videoindexer.ai/docs/services/operations/operations/Create-Brand) yeni bir özel marka oluşturur ve belirtilen hesap Için özel markalar modeline ekler. 

> [!NOTE]
> **Enabled** (gövdesinde) ayarı true olarak ayarlanırsa, marka video Indexer saptamak için *ekleme* listesine ekler. **Enabled** özelliği false olarak ayarlandığında, markayı *dışlama* listesine koyar, bu nedenle video Indexer algılamamasını sağlar.

Gövdede ayarlayabileceğiniz bazı diğer parametreler:

* **Referenceurl** değeri, bir markasının visela sayfasına yönelik bağlantı gibi herhangi bir başvuru Web sitesi olabilir.
* **Etiketler** değeri, markala ilgili etiketlerin bir listesidir. Bu, Video Indexer Web sitesindeki marka 'in *Kategori* alanında görüntülenir. Örneğin, "Azure" markası, "Cloud" olarak etiketlenebilir veya kategorilere ayrılmıştır.

### <a name="response"></a>Yanıt

Yanıt, yeni oluşturduğunuz markada aşağıdaki örnekte yer alan bilgileri sağlar.

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

## <a name="delete-a-brand"></a>Marka silme

Marka API 'si [silme](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Brand?) , belirtilen hesap Için özel markalar modelinden bir marka kaldırır. Hesap **AccountID** parametresinde belirtilir. Başarılı bir şekilde çağrıldıktan sonra, marka artık markalar *ekleme* veya *hariç tutma* listelerinde yer alır.

### <a name="response"></a>Yanıt

Marka başarıyla silindiğinde döndürülen içerik yok.

## <a name="get-a-specific-brand"></a>Belirli bir marka al

[Marka al](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brand?) API 'si, marka kimliğini kullanarak belirtilen hesap Için özel markalar modelinde bir markaların ayrıntılarını aramanıza olanak tanır.

### <a name="response"></a>Yanıt

Yanıt, aşağıdaki örnekte belirtilen biçimde, aradığınız markada (marka KIMLIĞI kullanarak) ilgili bilgiler sağlar.

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
> **true** olarak **ayarlanmakta olan** marka, markasının tespit edilecek video Indexer için *dahil* olduğunu ve yanlış olarak **etkinleştirildiğini** , markasının *hariç tutma* listesinde olduğunu belirtir ve video Indexer bunu algılamaz.

## <a name="update-a-specific-brand"></a>Belirli bir marka güncelleştirme

Marka API 'si [güncelleştirme](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Brand?) , marka kimliğini kullanarak belirtilen hesap Için özel markalar modelinde bir markaların ayrıntılarını aramanıza olanak tanır.

### <a name="response"></a>Yanıt

Yanıt, aşağıdaki örnek biçimini izleyerek güncelleştirdiğiniz marka hakkındaki güncelleştirilmiş bilgileri sağlar.

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

## <a name="get-all-of-the-brands"></a>Tüm markalar alın

[Tüm markalar al](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brands?) API 'si, markaların *dahil etme* veya *hariç tutma* listesinde olup olmamasından bağımsız olarak, belirtilen hesap için özel markalar modelindeki tüm markalarını döndürür.

### <a name="response"></a>Yanıt

Yanıt, hesabınızdaki tüm markaların bir listesini ve aşağıdaki örnekte belirtilen biçimdeki ayrıntıları sağlar.

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
> Marka adlı *örnek* , algılamak Için video Indexer *Içerme* listesinde ve *example2* adlı marka, *hariç tutma* listesinde yer alır, bu nedenle video Indexer bunu algılamaz.

## <a name="get-brands-model-settings"></a>Markalar model ayarlarını al

[Marka ayarları al](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brands) API 'si, belirtilen hesaptaki markalar model ayarlarını döndürür. Markalar model ayarları, Bing markalar veritabanı 'nın etkin olup olmadığını temsil eder. Bing markalar etkinleştirilmemişse, Video Indexer yalnızca belirtilen hesabın özel markalar modelinden markalar algılar.

### <a name="response"></a>Yanıt

Yanıt, Bing markaların aşağıdaki örnekte belirtilen biçimde etkinleştirilip etkinleştirilmediğini gösterir.

```json
{
  "state": true,
  "useBuiltIn": true
}
```

> [!NOTE]
> doğru olarak ayarlanan **Usebuiltin** , Bing markaların etkinleştirildiğini temsil eder. *Usebuiltin* false Ise, Bing markalar devre dışı bırakılır. **Durum** değeri kullanım dışı olduğu için yok sayılabilir.

## <a name="update-brands-model-settings"></a>Markalar model ayarlarını Güncelleştir

[Güncelleştirme markalar](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Brands-Model-Settings?) , belirtilen hesaptaki markalar modeli ayarlarını güncelleştirir. Markalar model ayarları, Bing markalar veritabanı 'nın etkin olup olmadığını temsil eder. Bing markalar etkinleştirilmemişse, Video Indexer yalnızca belirtilen hesabın özel markalar modelinden markalar algılar.

**Usebuiltin** bayrağı true olarak ayarlanmış, Bing markaların etkinleştirildiğini temsil eder. *Usebuiltin* false Ise, Bing markalar devre dışı bırakılır.

### <a name="response"></a>Yanıt

Markalar model ayarı başarıyla güncelleştirildiği zaman döndürülen içerik yok.

## <a name="next-steps"></a>Sonraki adımlar

[Web sitesi kullanarak markalar modelini özelleştirme](customize-brands-model-with-website.md)
