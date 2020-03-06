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
ms.openlocfilehash: ce32343faefbcf2484ec0b1b39f752654a2d8514
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303622"
---
# <a name="assets-in-azure-media-services"></a>Azure Media Services varlıklar

Azure Media Services, bir [varlık](https://docs.microsoft.com/rest/api/media/assets) temel kavramdır. Medyayı (örneğin, karşıya yükleme veya canlı alma aracılığıyla), çıkış medyasını (bir iş çıktısından) ve medya yayımlama (akış için). 

Bir varlık, [Azure depolama hesabındaki](storage-account-concept.md) bir blob kapsayıcısına eşlenir ve varlık içindeki dosyalar bu kapsayıcıda blok Bloblar olarak depolanır. Varlıklar, Azure depolama 'da depolanan dijital dosyalarla ilgili bilgiler içerir (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı altyazı dosyaları dahil).

Media Services, hesap genel amaçlı v2 (GPv2) depolaması kullandığında blob katmanlarını destekler. GPv2 ile dosyaları seyrek erişimli [veya arşiv depolamaya](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)taşıyabilirsiniz. **Arşiv** depolama, artık gerekli olmadığında (örneğin, kodlandıktan sonra) kaynak dosyalarını arşivlemek için uygundur.

**Arşiv** depolama katmanı yalnızca önceden kodlanmış çok büyük kaynak dosyaları için önerilir ve kodlama iş çıktısı bir çıkış blobu kapsayıcısına konur. Bir varlıkla ilişkilendirmek istediğiniz çıkış kapsayıcısındaki Bloblar, **sık** erişimli veya seyrek erişimli **bir depolama katmanında** bulunmalıdır.

## <a name="naming"></a>Adlandırma 

### <a name="assets"></a>Varlıklar

Varlığın adları benzersiz olmalıdır. Media Services v3 kaynak adları (örneğin, varlıklar, Işler, dönüşümler) Azure Resource Manager adlandırma kısıtlamalarına tabidir. Daha fazla bilgi için bkz. [adlandırma kuralları](media-services-apis-overview.md#naming-conventions).

### <a name="blobs"></a>Bloblar

Bir varlık içindeki dosyaların/Blobların adları, hem [BLOB adı gereksinimlerini](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) hem de [NTFS ad gereksinimlerini](https://docs.microsoft.com/windows/win32/fileio/naming-a-file)izlemelidir. Bu gereksinimlerin nedeni, dosyaların işlenmek üzere blob depolamadan yerel bir NTFS diskine kopyalanmasını sağlayabilir.

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
|[Bekleyen veriler için Depolama Hizmeti Şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Azure depolama tarafından sunulan ve Azure tarafından yönetilen veya müşteri tarafından yönetilen sunucu tarafı şifrelemesi.|Destekleniyor|Destekleniyor|
|[Depolama Istemci tarafı şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama tarafından sunulan ve Key Vault ' de müşteri tarafından yönetilen anahtar olan istemci tarafı şifreleme.|Desteklenmiyor|Desteklenmiyor|

<sup>1</sup> Media Services hiçbir şifreleme formu olmadan açık/olmayan içerik işlemeyi desteklese de, bunu yapmanız önerilmez.

<sup>2</sup> Media Services v3 'de, depolama ŞIFRELEMESI (AES-256 şifrelemesi) yalnızca varlıklarınız Media Services V2 ile oluşturulduysa geriye dönük uyumluluk için desteklenir. V3, mevcut depolama şifrelenmiş varlıklarla birlikte çalışarak, yenilerini oluşturulmasına izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

[Media Services varlıkları yönetme](manage-asset-concept.md)

## <a name="see-also"></a>Ayrıca bkz.

[Media Services V2 ve v3 arasındaki farklar](migrate-from-v2-to-v3.md)
