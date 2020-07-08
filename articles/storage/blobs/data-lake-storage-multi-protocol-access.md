---
title: Azure Data Lake Storage çoklu protokol erişimi | Microsoft Docs
description: Blob API 'Leri ve Azure Data Lake Storage 2. blob API 'Leri kullanan uygulamaları kullanın.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: e3997fc215637175165402a926bffc6ac8d02771
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77914867"
---
# <a name="multi-protocol-access-on-azure-data-lake-storage"></a>Azure Data Lake Storage çoklu protokol erişimi

Blob API 'Leri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu, araçların, uygulamaların ve hizmetlerin ekosisteminin yanı sıra hiyerarşik bir ad alanına sahip hesaplara birçok BLOB depolama özelliği de sunar.

Yakın zamanda, nesne depolama ve analiz depolaması için ayrı depolama çözümlerini korumanız gerekiyordu. Çünkü Azure Data Lake Storage 2., ekosistem desteğinin sınırlı olması gerekiyordu. Ayrıca, tanılama günlüğü gibi BLOB hizmeti özelliklerine sınırlı erişimi vardır. Çeşitli senaryolar gerçekleştirmek için verileri hesaplar arasında taşımanız gerektiğinden parçalanmış bir depolama çözümü devam etmek zordur. Artık bunu yapmanız gerekmez.

Data Lake Storage çoklu protokol erişimi sayesinde, Araçlar, uygulamalar ve hizmetler ekosistemini kullanarak verilerinize çalışabilirsiniz. Bu ayrıca üçüncü taraf araçları ve uygulamaları da içerir. Bunları değiştirmek zorunda kalmadan hiyerarşik bir ad alanına sahip hesaplara işaret edebilirsiniz. Blob API 'Leri artık hiyerarşik bir ad alanına sahip hesaplarda veri üzerinde çalışabildiklerinden, bu uygulamalar blob API 'Lerini çağırsalar *bile çalışır* .

[Tanılama günlüğü](../common/storage-analytics-logging.md), [erişim katmanları](storage-blob-storage-tiers.md)ve [BLOB depolama yaşam döngüsü yönetim ilkeleri](storage-lifecycle-management-concepts.md) gibi BLOB depolama özellikleri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu nedenle, bu önemli özelliklere erişimi kaybetmeksizin BLOB depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz. 

> [!NOTE]
> Data Lake Storage çoklu protokol erişimi genel kullanıma sunulmuştur ve tüm bölgelerde kullanılabilir. Çoklu protokol erişimi tarafından etkinleştirilen bazı Azure hizmetleri veya blob depolama özellikleri önizlemede kalır.  Bu makaleler, BLOB depolama özellikleri ve Azure hizmet tümleştirmeleri için geçerli desteği özetler. 
>
> [Azure Data Lake Storage 2. bulunan BLOB depolama özellikleri](data-lake-storage-supported-blob-storage-features.md)
>
>[Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)

## <a name="how-multi-protocol-access-on-data-lake-storage-works"></a>Data Lake Storage 'da çoklu protokol erişimi nasıl kullanılır?

Blob API 'Leri ve Data Lake Storage 2. API 'Leri, hiyerarşik bir ad alanına sahip depolama hesaplarında aynı verilerde çalışabilir. Data Lake Storage 2., birinci sınıf dizin işlemlerinin ve POSIX ile uyumlu erişim denetim listelerinin (ACL 'Ler) avantajlarından yararlanmak için, blob API 'Lerini hiyerarşik ad alanı aracılığıyla yönlendirir. 

![Kavramsal Data Lake Storage çoklu protokol erişimi](./media/data-lake-storage-interop/interop-concept.png) 

Blob API kullanan mevcut araçlar ve uygulamalar bu avantajları otomatik olarak elde edebilir. Geliştiricilerin onları değiştirmesi gerekmez. Data Lake Storage 2., Araçlar ve uygulamaların verilere erişmek için kullandığı Protokolden bağımsız olarak dizin ve dosya düzeyi ACL 'Leri tutarlı bir şekilde uygular. 

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. bulunan BLOB depolama özellikleri](data-lake-storage-supported-blob-storage-features.md)
- [Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)
- [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](data-lake-storage-known-issues.md)




