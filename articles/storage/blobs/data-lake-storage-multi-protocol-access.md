---
title: Azure Data Lake Storage çoklu protokol erişimi (Önizleme) | Microsoft Docs
description: Blob API 'Leri ve Azure Data Lake Storage 2. blob API 'Leri kullanan uygulamaları kullanın.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 6e3b368d1f5615c34a49b155d288d3c37c28bc26
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72933130"
---
# <a name="multi-protocol-access-on-azure-data-lake-storage-preview"></a>Azure Data Lake Storage çoklu protokol erişimi (Önizleme)

Blob API 'Leri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu, tüm bir araç, uygulama ve hizmet ekosisteminin yanı sıra hiyerarşik bir ad alanına sahip hesaplara tüm blob depolama özelliklerini de kaldırır.

Yakın zamanda, nesne depolama ve analiz depolaması için ayrı depolama çözümlerini korumanız gerekiyordu. Çünkü Azure Data Lake Storage 2., ekosistem desteğinin sınırlı olması gerekiyordu. Ayrıca, tanılama günlüğü gibi BLOB hizmeti özelliklerine sınırlı erişimi vardır. Çeşitli senaryolar gerçekleştirmek için verileri hesaplar arasında taşımanız gerektiğinden parçalanmış bir depolama çözümü devam etmek zordur. Artık bunu yapmanız gerekmez.

> [!NOTE]
> Data Lake Storage çoklu protokol erişimi genel önizlemede ve tüm bölgelerde kullanılabilir. Hiyerarşik bir ad alanına sahip tüm hesaplar tarafından otomatik olarak kullanılabildiği için genel önizlemeye kaydolmak zorunda değilsiniz. Sınırlamaları gözden geçirmek için, [bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın.

## <a name="use-the-entire-ecosystem-of-applications-tools-and-services"></a>Uygulamaların, araçların ve hizmetlerin tüm ekosistemini kullanın

Data Lake Storage çoklu protokol erişimi sayesinde, araçların, uygulamaların ve hizmetlerin tüm ekosistemini kullanarak tüm verileriniz ile çalışabilirsiniz. Bu, [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction), [ıot hub](https://docs.microsoft.com/azure/iot-hub/), [Power BI](https://docs.microsoft.com/power-bi/desktop-data-sources)ve diğer birçok farklı Azure hizmetlerini içerir. Tüm liste için bkz. [Azure hizmetleriyle Azure Data Lake Storage tümleştirme](data-lake-store-integrate-with-azure-services.md).

Bu ayrıca üçüncü taraf araçları ve uygulamaları da içerir. Bunları değiştirmek zorunda kalmadan hiyerarşik bir ad alanına sahip hesaplara işaret edebilirsiniz. Blob API 'Leri artık hiyerarşik bir ad alanına sahip hesaplarda veri üzerinde çalışabildiklerinden, bu uygulamalar blob API 'Lerini çağırsalar *bile çalışır* .

> [!NOTE]
> Sınırlamaları gözden geçirmek için, [bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın.

## <a name="use-all-blob-storage-features"></a>Tüm blob depolama özelliklerini kullan

[Tanılama günlüğü](../common/storage-analytics-logging.md), [erişim katmanları](storage-blob-storage-tiers.md)ve [BLOB depolama yaşam döngüsü yönetim ilkeleri](storage-lifecycle-management-concepts.md) gibi BLOB depolama özellikleri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu nedenle, bu önemli özelliklere erişimi kaybetmeksizin BLOB depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz. 

> [!NOTE]
> Sınırlamaları gözden geçirmek için, [bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın.

## <a name="how-multi-protocol-access-on-data-lake-storage-works"></a>Data Lake Storage 'da çoklu protokol erişimi nasıl kullanılır?

Blob API 'Leri ve Data Lake Storage 2. API 'Leri, hiyerarşik bir ad alanına sahip depolama hesaplarında aynı verilerde çalışabilir. Data Lake Storage 2., birinci sınıf dizin işlemlerinin ve POSIX ile uyumlu erişim denetim listelerinin (ACL 'Ler) avantajlarından yararlanmak için, blob API 'Lerini hiyerarşik ad alanı aracılığıyla yönlendirir. 

![Kavramsal Data Lake Storage çoklu protokol erişimi](./media/data-lake-storage-interop/interop-concept.png) 

Blob API kullanan mevcut araçlar ve uygulamalar bu avantajları otomatik olarak elde edebilir. Geliştiricilerin onları değiştirmesi gerekmez. Data Lake Storage 2., Araçlar ve uygulamaların verilere erişmek için kullandığı Protokolden bağımsız olarak dizin ve dosya düzeyi ACL 'Leri tutarlı bir şekilde uygular. 

## <a name="next-steps"></a>Sonraki adımlar

[Bilinen sorunlara](data-lake-storage-known-issues.md) bakın




