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
ms.date: 03/09/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 6c9f69a39f725b082771b66959a219581c281ed5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87043513"
---
# <a name="assets-in-azure-media-services-v3"></a>Azure Media Services v3 içindeki varlıklar

Azure Media Services, bir [varlık](/rest/api/media/assets) temel kavramdır. Medyayı (örneğin, karşıya yükleme veya canlı alma aracılığıyla), çıkış medyasını (bir iş çıktısından) ve medya yayımlama (akış için). 

Bir varlık, [Azure depolama hesabındaki](storage-account-concept.md) bir blob kapsayıcısına eşlenir ve varlık içindeki dosyalar bu kapsayıcıda blok Bloblar olarak depolanır. Varlıklar, Azure depolama 'da depolanan dijital dosyalarla ilgili bilgiler içerir (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı altyazı dosyaları dahil).

Media Services, hesap genel amaçlı v2 (GPv2) depolaması kullandığında blob katmanlarını destekler. GPv2 ile dosyaları seyrek erişimli [veya arşiv depolamaya](../../storage/blobs/storage-blob-storage-tiers.md)taşıyabilirsiniz. **Arşiv** depolama, artık gerekli olmadığında (örneğin, kodlandıktan sonra) kaynak dosyalarını arşivlemek için uygundur.

**Arşiv** depolama katmanı yalnızca önceden kodlanmış çok büyük kaynak dosyaları için önerilir ve kodlama iş çıktısı bir çıkış blobu kapsayıcısına konur. Bir varlıkla ilişkilendirmek istediğiniz çıkış kapsayıcısındaki Bloblar, **sık** erişimli veya seyrek erişimli **bir depolama katmanında** bulunmalıdır.

## <a name="naming"></a>Adlandırma 

### <a name="assets"></a>Varlıklar

Varlığın adları benzersiz olmalıdır. Media Services v3 kaynak adları (örneğin, varlıklar, Işler, dönüşümler) Azure Resource Manager adlandırma kısıtlamalarına tabidir. Daha fazla bilgi için bkz. [adlandırma kuralları](media-services-apis-overview.md#naming-conventions).

### <a name="blobs"></a>Bloblar

Bir varlık içindeki dosyaların/Blobların adları, hem [BLOB adı gereksinimlerini](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) hem de [NTFS ad gereksinimlerini](/windows/win32/fileio/naming-a-file)izlemelidir. Bu gereksinimlerin nedeni, dosyaların işlenmek üzere blob depolamadan yerel bir NTFS diskine kopyalanmasını sağlayabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Media Services varlıkları yönetme](manage-asset-concept.md)

## <a name="see-also"></a>Ayrıca bkz.

[Media Services V2 ve v3 arasındaki farklar](migrate-from-v2-to-v3.md)
