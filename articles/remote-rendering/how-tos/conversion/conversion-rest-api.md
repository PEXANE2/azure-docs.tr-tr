---
title: Varlık dönüştürme REST API
description: REST API aracılığıyla bir varlığın nasıl dönüştürülür şekilde dönüştürüldüğünü açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: 064a7b8adaec6f47a153743b58f4e1caf50db723
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681512"
---
# <a name="use-the-model-conversion-rest-api"></a>Model dönüştürme REST API'yi kullanma

[Model dönüştürme](model-conversion.md) [hizmeti, REST API](https://en.wikipedia.org/wiki/Representational_state_transfer)üzerinden denetlenir. Bu makalede, dönüşüm hizmeti API ayrıntıları açıklanmaktadır.

## <a name="regions"></a>Bölgeler

İstekleri göndermek için temel URL'ler için [kullanılabilir bölgelerin listesine](../../reference/regions.md) bakın.

## <a name="common-headers"></a>Ortak üstbilgi

### <a name="common-request-headers"></a>Ortak istek üstbilgi

Bu üstbilgi tüm istekler için belirtilmelidir:

- **Yetkilendirme** üstbilgisi ,*[TOKEN*] bir [hizmet erişim belirteci](../tokens.md)olduğu "Taşıyıcı [*TOKEN*]" değerine sahip olmalıdır.

### <a name="common-response-headers"></a>Ortak yanıt üstbilgi

Tüm yanıtlar şu üstbilgileri içerir:

- **MS-CV** üstbilgisi, hizmet içindeki çağrıyı izlemek için kullanılabilecek benzersiz bir dize içerir.

## <a name="endpoints"></a>Uç Noktalar

Dönüştürme hizmeti üç REST API uç noktası sağlar:

- Azure Uzaktan İşleme hesabınızla bağlantılı bir depolama hesabı kullanarak model dönüştürmeye başlayın. 
- sağlanan *Paylaşılan Erişim İmzaları (SAS)* kullanarak model dönüştürmebaşlatın.
- dönüşüm durumunu sorgula

### <a name="start-conversion-using-a-linked-storage-account"></a>Bağlantılı bir depolama hesabı kullanarak dönüşüm başlatma
Azure Uzaktan İşlem Emki Hesabınızın, depolama hesaplarını nasıl bağlayacaklarına ilişkin adımları izleyerek sağlanan depolama [hesabına](../create-an-account.md#link-storage-accounts)erişmesi gerekir.

| Uç Nokta | Yöntem |
|-----------|:-----------|
| /v1/accounts/**accountID**/conversion/create | POST |

JSON belgesine sarılmış devam eden dönüşümün kimliğini verir. Alan adı "conversionId"dir.

#### <a name="request-body"></a>İstek gövdesi


```json
{
    "input":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container containing your input asset data>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the input blob container>",
        "inputAssetPath" : "<path to the model in the input blob container relative to the folderPath (or container root if no folderPath is specified)>"
    },
    "output":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container where the converted asset will be copied to>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the output blob container. Will contain the asset and log files>",
        "outputAssetFileName": "<optional: can be omitted or empty. The filename of the converted asset. If provided the filename needs to end in .arrAsset>"
    }
}
```
### <a name="start-conversion-using-provided-shared-access-signatures"></a>Sağlanan paylaşılan erişim imzalarını kullanarak dönüştürmeyi başlatma
ARR hesabınız depolama hesabınıza bağlı değilse, bu REST arabirimi *Paylaşılan Erişim İmzalarını (SAS)* kullanarak erişim sağlamanıza olanak tanır.

| Uç Nokta | Yöntem |
|-----------|:-----------|
| /v1/accounts/**accountID**/conversion/createWithSharedAccessSignature | POST |

JSON belgesine sarılmış devam eden dönüşümün kimliğini verir. Alan adı "conversionId"dir.

#### <a name="request-body"></a>İstek gövdesi

İstek gövdesi yukarıdaki REST araması oluşturma ile aynıdır, ancak giriş ve çıktı *Paylaşılan Erişim İmzaları (SAS) belirteçleri*içerir. Bu belirteçler, girişin okunması ve dönüşüm sonucunun yazılması için depolama hesabına erişim sağlar.

> [!NOTE]
> Bu SAS URI belirteçleri tam URI değil, sorgu dizeleri vardır. 


```json
{
    "input":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container containing your input asset data>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the input blob container>",
        "inputAssetPath" : "<path to the model in the input blob container relative to the folderPath (or container root if no folderPath is specified)>",
        "containerReadListSas" : "<a container SAS token which gives read and list access to the given input blob container>"
    },
    "output":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container where the converted asset will be copied to>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the output blob container. Will contain the asset and log files>",
        "outputAssetFileName": "<optional: can be omitted or empty. The filename of the converted asset. If provided the filename needs to end in .arrAsset>",
        "containerWriteSas" : "<a container SAS token which gives write access to the given output blob container>"
    }
}
```

### <a name="poll-conversion-status"></a>Anket dönüştürme durumu
Yukarıdaki REST çağrılarından biriyle başlayan devam eden bir dönüştürmenin durumu aşağıdaki arabirimi kullanarak sorgulanabilir:


| Uç Nokta | Yöntem |
|-----------|:-----------|
| /v1/accounts/**accountID**/conversions/**conversionId** | GET |

Aşağıdaki değerlere sahip bir "durum" alanına sahip bir JSON belgesi döndürür:

- "Koşmak"
- "Başarı"
- "Başarısızlık"

Durum "Hata" ise, hata bilgilerini içeren bir "ileti" alt alanı içeren ek bir "hata" alanı olacaktır. Ek günlükler çıkış kabınıza yüklenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Model dönüştürme için Azure Blob Depolama'yı kullanma](blob-storage.md)
- [Model dönüştürme](model-conversion.md)
