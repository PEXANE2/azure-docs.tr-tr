---
title: Varlıklar
titleSuffix: Azure Media Services
description: Varlıkların ne olduğunu ve Azure Media Services tarafından nasıl kullanıldığını öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/29/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ab4eebf56abd2d328ccf86929a043d4354ca157c
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74186323"
---
# <a name="assets-in-azure-media-services"></a>Azure Media Services varlıklar

Azure Media Services bir [varlık](https://docs.microsoft.com/rest/api/media/assets) , Azure depolama 'da depolanan dijital dosyalarla ilgili bilgiler içerir (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı altyazı dosyaları dahil).

Bir varlık, [Azure depolama hesabındaki](storage-account-concept.md) bir blob kapsayıcısına eşlenir ve varlık içindeki dosyalar bu kapsayıcıda blok Bloblar olarak depolanır. Media Services, hesap genel amaçlı v2 (GPv2) depolaması kullandığında blob katmanlarını destekler. GPv2 ile dosyaları seyrek erişimli [veya arşiv depolamaya](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)taşıyabilirsiniz. **Arşiv** depolama, artık gerekli olmadığında (örneğin, kodlandıktan sonra) kaynak dosyalarını arşivlemek için uygundur.

**Arşiv** depolama katmanı yalnızca önceden kodlanmış çok büyük kaynak dosyaları için önerilir ve kodlama iş çıktısı bir çıkış blobu kapsayıcısına konur. Bir varlıkla ilişkilendirmek istediğiniz çıkış kapsayıcısındaki Bloblar, **sık** erişimli veya seyrek erişimli **bir depolama katmanında** bulunmalıdır.

### <a name="naming-blobs"></a>Blob 'ları adlandırma

Bir varlık içindeki dosyaların/Blobların adları, hem [BLOB adı gereksinimlerini](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) hem de [NTFS ad gereksinimlerini](https://docs.microsoft.com/windows/win32/fileio/naming-a-file)izlemelidir. Bu gereksinimlerin nedeni, dosyaların işlenmek üzere blob depolamadan yerel bir NTFS diskine kopyalanmasını sağlayabilir.

## <a name="upload-digital-files-into-assets"></a>Dijital dosyaları varlıklara yükleme

Dijital dosyalar depolama alanına yüklendikten ve bir varlıkla ilişkilendirildikten sonra, Media Services kodlama, akış ve içerik iş akışlarını çözümleme için kullanılabilirler. Ortak Media Services iş akışlarından biri bir dosyayı karşıya yüklemek, kodlamak ve akışa almak. Bu bölümde genel adımlar özetlenmektedir.

> [!TIP]
> Geliştirmeye başlamadan önce, [Media Services v3 API 'leri Ile geliştirmeyi](media-services-apis-overview.md) Inceleyin (API 'lere erişme, adlandırma kuralları vb.).

1. Media Services v3 API'sini kullanarak yeni bir "input" Varlığı oluşturun. Bu işlem, Media Services hesabınızla ilişkilendirilen depolama hesabında bir kapsayıcı oluşturur. API, kapsayıcı adını döndürür (örneğin, `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).

    Bir varlıkla ilişkilendirmek istediğiniz bir blob Kapsayıcınız zaten varsa, varlığı oluştururken kapsayıcı adını belirtebilirsiniz. Media Services şu anda blobları yalnızca kapsayıcı kökünde destekler ve dosya adında yol kullanılmasını desteklemez. Bu nedenle "input.mp4" gibi bir dosya adına sahip kapsayıcıları kullanabilirsiniz. Bununla birlikte, "Videolar/girişler/Input. mp4" dosya adına sahip bir kapsayıcı çalışmaz.

    Azure CLI'yi kullanarak aboneliğinizde gerekli izinlere sahip olduğunuz tüm depolama hesaplarına ve kapsayıcılara içerik yükleyebilirsiniz.

    Kapsayıcı adının benzersiz ve depolama adlandırma kurallarına uygun olması gerekir. Ad için Media Services Varlık kapsayıcısı adı (Varlık-GUID) biçimlendirmesinin kullanılması şart değildir.

    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Dijital dosyaları Varlık kapsayıcısına yüklemek için kullanmak üzere okuma-yazma izinlerine sahip bir SAS URL'si alın. Media Services API'sini kullanarak [varlık kapsayıcısı URL'lerini listeleyebilirsiniz](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Dosyaları varlık kapsayıcısına yüklemek için Azure depolama API 'Lerini veya SDK 'Larını (örneğin, [depolama REST API](../../storage/common/storage-rest-api-auth.md) veya [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) kullanın.
4. Media Services v3 API'lerini kullanarak "input" Varlığınızı işlemek üzere bir Dönüşüm ve bir İş oluşturun. Daha fazla bilgi için [Dönüşümler ve İşler](transform-concept.md) konusuna bakın.
5. "Çıkış" kıymetinin içeriğini akışla.

Varlığın nasıl oluşturulduğunu, depolama alanındaki varlığın kapsayıcısına yazılabilir SAS URL 'sini alma ve SAS URL 'sini kullanarak dosyayı depolama alanındaki kapsayıcıya yükleme hakkında tam bir .NET örneği için bkz. [yerel bir dosyadan iş girişi oluşturma](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Yeni varlık oluştur

> [!NOTE]
> Bir varlığın tarih saat türünün özellikleri her zaman UTC biçimindedir.

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

REST örneği için bkz. [rest Ile varlık oluşturma](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) örneği.

Örnek, açıklama, kapsayıcı adı, depolama hesabı ve diğer yararlı bilgileri belirtebileceğiniz **Istek gövdesinin** nasıl oluşturulacağını gösterir.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

Tam bir örnek için bkz. [yerel bir dosyadan iş girişi oluşturma](job-input-from-local-file-how-to.md). Media Services v3 'de, bir işin girişi HTTPS URL 'Lerinden de oluşturulabilir (bkz. bir [https URL 'sinden iş girişi oluşturma](job-input-from-http-how-to.md)).

## <a name="map-v3-asset-properties-to-v2"></a>V3 varlık özelliklerini v2 'ye eşleyin

Aşağıdaki tabloda, [varlıkların](https://docs.microsoft.com/rest/api/media/assets/createorupdate#asset)sürüm 2 ' deki varlık özelliklerinin v2 'de nasıl eşlenme şekli gösterilmektedir.

|V3 Özellikleri|v2 özellikleri|
|---|---|
|`id`-(benzersiz) tam Azure Resource Manager yolu, bkz. [varlık](https://docs.microsoft.com/rest/api/media/assets/createorupdate) içindeki örnekler||
|`name`-(benzersiz) bkz. [adlandırma kuralları](media-services-apis-overview.md#naming-conventions) ||
|`alternateId`|`AlternateId`|
|`assetId`|`Id`-(unique) değeri `nb:cid:UUID:` ön ekiyle başlar.|
|`created`|`Created`|
|`description`|`Name`|
|`lastModified`|`LastModified`|
|`storageAccountName`|`StorageAccountName`|
|`storageEncryptionFormat`| `Options` (oluşturma seçenekleri)|
|`type`||

## <a name="storage-side-encryption"></a>Depolama tarafında şifreleme

Bekleyen veri varlıklarınızı korumanın varlıklar tarafından depolama tarafı şifrelemesi şifrelenmelidir. Aşağıdaki tabloda, depolama tarafı şifrelemesi Media Services'de nasıl çalıştığı gösterilmektedir:

|Şifreleme seçeneği|Açıklama|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services'ı depolama şifrelemesi|AES-256 şifrelemesi, anahtar Media Services tarafından yönetiliyor.|Desteklenen<sup>(1)</sup>|Desteklenmiyor<sup>(2)</sup>|
|[Bekleyen veriler için Depolama Hizmeti Şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Azure depolama tarafından sunulan ve Azure tarafından yönetilen veya müşteri tarafından yönetilen sunucu tarafı şifrelemesi.|Desteklenir|Desteklenir|
|[Depolama Istemci tarafı şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama tarafından sunulan ve Key Vault ' de müşteri tarafından yönetilen anahtar olan istemci tarafı şifreleme.|Desteklenmez|Desteklenmez|

<sup>1</sup> Media Services hiçbir şifreleme formu olmadan açık/olmayan içerik işlemeyi desteklese de, bunu yapmanız önerilmez.

<sup>2</sup> Media Services v3 'de, depolama ŞIFRELEMESI (AES-256 şifrelemesi) yalnızca varlıklarınız Media Services V2 ile oluşturulduysa geriye dönük uyumluluk için desteklenir. V3, mevcut depolama şifrelenmiş varlıklarla birlikte çalışarak, yenilerini oluşturulmasına izin vermez.

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Bkz. [Media Services varlıkların filtrelenmesi, sıralanması, sayfalama](entities-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
* [Bulut DVR kullanma](live-event-cloud-dvr.md)
* [Media Services V2 ve v3 arasındaki farklar](migrate-from-v2-to-v3.md)
