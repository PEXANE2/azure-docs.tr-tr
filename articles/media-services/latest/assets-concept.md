---
title: Varlıklar
titleSuffix: Azure Media Services
description: Azure Medya Hizmetleri tarafından hangi varlıkların olduğu ve nasıl kullanıldığı hakkında bilgi edinin.
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
ms.openlocfilehash: 9b04941a5799955097fbd54ad9bdf50eccb87541
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79087915"
---
# <a name="assets-in-azure-media-services-v3"></a>Azure Medya Hizmetleri v3'teki varlıklar

Azure Medya Hizmetleri'nde [Varlık](https://docs.microsoft.com/rest/api/media/assets) temel bir kavramdır. Medya girişi yaptığınız yerdir (örneğin, yükleme veya canlı yutma yoluyla), çıktı ortamını (bir iş çıkışından) ve (akış için) medya yayımlama. 

Bir [Varlık, Azure Depolama hesabındaki](storage-account-concept.md) bir blob kapsayıcısına eşlenir ve Varlık'taki dosyalar bu kapsayıcıda blok blob'ları olarak depolanır. Varlıklar, Azure Depolama'da depolanan dijital dosyalar (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı altyazı dosyaları dahil) hakkında bilgiler içerir.

Ortam Hizmetleri, hesap Genel amaçlı v2 (GPv2) depolama alanını kullandığında Blob katmanlarını destekler. GPv2 ile dosyaları Cool [veya Arşiv depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)alanına taşıyabilirsiniz. **Arşiv** depolama, kaynak dosyaları artık gerekmediğinde (örneğin, kodlandıktan sonra) arşivlemek için uygundur.

**Arşiv** depolama katmanı yalnızca zaten kodlanmış çok büyük kaynak dosyaları için önerilir ve kodlama İş çıktısı bir çıktı blob kapsayıcısına konulmuştur. Bir Varlıkla ilişkilendirmek ve içeriğinizi akış veya analiz etmek için kullanmak istediğiniz çıkış kabındaki **lekeler, Sıcak** veya **Serin** depolama katmanında bulunmalıdır.

## <a name="naming"></a>Adlandırma 

### <a name="assets"></a>Varlıklar

Varlık'ın adları benzersiz olmalı. Medya Hizmetleri v3 kaynak adları (örneğin, Varlıklar, İşler, Dönüşümler) Azure Kaynak Yöneticisi adlandırma kısıtlamalarına tabidir. Daha fazla bilgi [için, Adlandırma kurallarına](media-services-apis-overview.md#naming-conventions)bakın.

### <a name="blobs"></a>Bloblar

Bir varlık içindeki dosyaların/blobs adlarının hem [blob adı gereksinimlerini](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) hem de [NTFS ad gereksinimlerini](https://docs.microsoft.com/windows/win32/fileio/naming-a-file)izlemesi gerekir. Bu gereksinimlerin nedeni, dosyaların blob depolamadan yerel bir NTFS diskine kopyalanabiliyor olmasıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Medya Hizmetleri'ndeki varlıkları yönetme](manage-asset-concept.md)

## <a name="see-also"></a>Ayrıca bkz.

[Medya Hizmetleri v2 ve v3 arasındaki farklar](migrate-from-v2-to-v3.md)
