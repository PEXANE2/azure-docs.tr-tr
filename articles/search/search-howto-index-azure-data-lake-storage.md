---
title: Azure Veri Gölü Depolama Gen2 üzerinden arama (önizleme)
titleSuffix: Azure Cognitive Search
description: Azure Veri Gölü Depolama Gen2'de içeriği ve meta verileri nasıl dizine ekleyip dizinleri nizi dizine ekinler gösterin. Bu özellik şu anda genel önizlemede
manager: nitinme
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 4b725c8a1bf0649a640c02a9a1828ec9014d36d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905650"
---
# <a name="indexing-documents-in-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'de belgeleri dizine alma

> [!IMPORTANT] 
> Azure Veri Gölü Depolama Gen2 desteği şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [Bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)doldurarak önizlemelere erişim isteğinde bulunabilirsiniz. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.


Bir Azure depolama hesabı [ayarlarken, hiyerarşik ad alanını](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-namespace)etkinleştirme seçeneğiniz vardır. Bu, bir hesaptaki içerik koleksiyonunun dizinler ve iç içe geçen alt dizinler hiyerarşisi içinde düzenlenmesine olanak tanır. Hiyerarşik ad alanını etkinleştirerek [Azure Veri Gölü Depolama Gen2'yi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)etkinleştirebilirsiniz.

Bu makalede, Azure Veri Gölü Depolama Gen2'de bulunan dizin oluşturma belgeleriyle nasıl başlanırsınız.

## <a name="set-up-azure-data-lake-storage-gen2-indexer"></a>Azure Veri Gölü Depolama Gen2 dizinleyicisi ayarlama

Veri Gölü Depolama Gen2'deki içeriği dizine dizine almak için tamamlamanız gereken birkaç adım vardır.

### <a name="step-1-sign-up-for-the-preview"></a>Adım 1: Önizleme için kaydolun

[Bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)doldurarak Veri Gölü Depolama Gen2 dizinleyici önizlemesine kaydolun. Önizlemeye kabul edildikten sonra bir onay e-postası alacaksınız.

### <a name="step-2-follow-the-azure-blob-storage-indexing-setup-steps"></a>Adım 2: Azure Blob depolama dizin kurulumu adımlarını izleyin

Önizleme kaydenizin başarılı olduğuna dair onay aldıktan sonra, dizin oluşturma ardışık hattını oluşturmaya hazırsınız.

[REST API sürümü 2019-05-06-Preview'u](search-api-preview.md)kullanarak Veri Gölü Depolama Gen2'deki içeriği ve meta verileri dizine ekebilirsiniz. Şu anda portal veya .NET SDK desteği yok.

Veri Gölü Depolama Gen2'deki içeriği dizine alma, Azure Blob depolamasındaki içeriği dizine eksine yle aynıdır. Veri Gölü Depolama Gen2 veri kaynağının, dizininin ve dizinleyicinin nasıl ayarlayacağıhakkında bilgi almak için Azure [Bilişsel Arama ile Azure Blob Depolama'daki belgeleri nasıl dizine ekleyip](search-howto-indexing-azure-blob-storage.md)dizine aktarın. Blob depolama makalesi ayrıca hangi belge biçimlerinin desteklendirilip desteklendirildigi, hangi blob meta veri özelliklerinin ayıklandırılan, artımlı dizin oluşturma ve daha fazlası hakkında bilgi sağlar. Bu bilgiler Data Lake Storage Gen2 için de geçerli olacaktır.

## <a name="access-control"></a>Erişim denetimi

Azure Veri Gölü Depolama Gen2, hem Azure rol tabanlı erişim denetimini (RBAC) hem de POSIX benzeri erişim denetim listelerini (AçS) destekleyen bir [erişim denetimi modeli](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control) uygular. Azure Bilişsel Arama, Veri Gölü Depolama Gen2'den içeriği dizine ekstre ederken, rbac ve ACL bilgilerini içerikten çıkarmaz. Sonuç olarak, bu bilgiler Azure Bilişsel Arama dizininize dahil edilmez.

Dizindeki her belgede erişim denetiminin sürdürülmesi önemliyse, [güvenlik kırpma](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search)uygulaması uygulama geliştiricisine bağlıdır.

## <a name="change-detection"></a>Algılamayı Değiştir

Veri Gölü Depolama Gen2 dizinleyicisi değişiklik algılamasını destekler. Bu, dizinleyici çalıştırdığında yalnızca blob `LastModified` zaman damgası tarafından belirlenen değiştirilen lekeleri yeniden dizineleri anlamına gelir.

> [!NOTE] 
> Data Lake Storage Gen2 dizinlerin yeniden adlandırılmasını sağlar. Bir dizin, o dizindeki lekelerin zaman damgaları yeniden adlandırıldığında güncelleştirilmez. Sonuç olarak, dizinleyici bu lekeleri yeniden dizine almaz. Artık yeni URL'leri olduğundan dizin yeniden adlandırıldıktan sonra dizin yeniden dizine alınması için dizindeki lekelerin yeniden dizine alınması gerekiyorsa, dizinleyicinin gelecekteki bir çalışma sırasında bunları yeniden dizine ekleyebilir şekilde dizindeki tüm lekelerin `LastModified` zaman damgasını güncelleştirmeniz gerekir.
