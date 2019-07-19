---
title: Azure Data Lake Storage çoklu protokol erişimi | Microsoft Docs
description: Blob API 'Leri ve Azure Data Lake Storage 2. blob API 'Leri kullanan uygulamaları kullanın.
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: normesta
ms.openlocfilehash: f384fb738fe719b8e622e8d61502e6acba2bbf31
ms.sourcegitcommit: da0a8676b3c5283fddcd94cdd9044c3b99815046
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314368"
---
# <a name="multi-protocol-access-on-azure-data-lake-storage"></a>Azure Data Lake Storage çoklu protokol erişimi

Blob API 'Leri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu, tüm bir araç, uygulama ve hizmet ekosisteminin yanı sıra hiyerarşik bir ad alanına sahip hesaplara tüm blob depolama özelliklerini de kaldırır.

Yakın zamanda, nesne depolama ve analiz depolaması için ayrı depolama çözümlerini korumanız gerekiyordu. Çünkü Azure Data Lake Storage 2., ekosistem desteğinin sınırlı olması gerekiyordu. Ayrıca, tanılama günlüğü gibi BLOB hizmeti özelliklerine sınırlı erişimi vardır. Çeşitli senaryolar gerçekleştirmek için verileri hesaplar arasında taşımanız gerektiğinden parçalanmış bir depolama çözümü devam etmek zordur. Artık bunu yapmanız gerekmez.

> [!NOTE]
> Data Lake Storage çoklu protokol erişimi genel önizlemede ve yalnızca **Batı ABD 2** ve **Orta Batı ABD** bölgelerinde kullanılabilir. Sınırlamaları gözden geçirmek için, [bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın. Önizlemeye kaydolmak için [Bu sayfaya](https://aka.ms/blobinteropsignup)bakın.

## <a name="use-the-entire-ecosystem-of-applications-tools-and-services"></a>Uygulamaların, araçların ve hizmetlerin tüm ekosistemini kullanın

Data Lake Storage çoklu protokol erişiminin önizlemesine kaydoldıysanız, araçların, uygulamaların ve hizmetlerin tüm ekosistemini kullanarak tüm verileriniz ile çalışabilirsiniz. Bu, [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction), [ıot hub](https://docs.microsoft.com/azure/iot-hub/), [Power BI](https://docs.microsoft.com/power-bi/desktop-data-sources)ve diğer birçok farklı Azure hizmetlerini içerir. 

Bu ayrıca üçüncü taraf araçları ve uygulamaları da içerir. Bunları değiştirmek zorunda kalmadan hiyerarşik bir ad alanına sahip hesaplara işaret edebilirsiniz. Blob API 'Leri artık hiyerarşik bir ad alanına sahip hesaplarda veri üzerinde çalışabildiklerinden, bu uygulamalar blob API 'Lerini çağırsalar *bile çalışır* .

> [!NOTE]
> Sınırlamaları gözden geçirmek için, [bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın.

## <a name="use-all-blob-storage-features"></a>Tüm blob depolama özelliklerini kullan

[Tanılama günlüğü](../common/storage-analytics-logging.md), [erişim katmanları](storage-blob-storage-tiers.md)ve [BLOB depolama yaşam döngüsü yönetim ilkeleri](storage-lifecycle-management-concepts.md) gibi BLOB depolama özellikleri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu nedenle, bu önemli özelliklere erişimi kaybetmeden BLOB depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz. 

> [!NOTE]
> Sınırlamaları gözden geçirmek için, [bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın.

## <a name="how-multi-protocol-access-on-data-lake-storage-works"></a>Data Lake Storage 'da çoklu protokol erişimi nasıl kullanılır?

Blob API 'Leri ve Data Lake Storage 2. API 'Leri, hiyerarşik bir ad alanına sahip depolama hesaplarında aynı verilerde çalışabilir. Data Lake Storage 2., birinci sınıf dizin işlemlerinin ve POSIX ile uyumlu erişim denetim listelerinin (ACL 'Ler) avantajlarından yararlanmak için, blob API 'Lerini hiyerarşik ad alanı aracılığıyla yönlendirir. 

![Kavramsal Data Lake Storage çoklu protokol erişimi](./media/data-lake-storage-interop/interop-concept.png) 

Blob API kullanan mevcut araçlar ve uygulamalar bu avantajları otomatik olarak elde edebilir. Geliştiricilerin onları değiştirmesi gerekmez. Data Lake Storage 2., Araçlar ve uygulamaların verilere erişmek için kullandığı Protokolden bağımsız olarak dizin ve dosya düzeyi ACL 'Leri tutarlı bir şekilde uygular.   

## <a name="next-steps"></a>Sonraki adımlar

[Bilinen sorunlara](data-lake-storage-known-issues.md) bakın




