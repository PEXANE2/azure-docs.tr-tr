---
title: Azure Veri Gölü Depolama'da çoklu protokol erişimi | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen2 ile Blob API'lerini ve Blob API'lerini kullanan uygulamaları kullanın.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: e3997fc215637175165402a926bffc6ac8d02771
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77914867"
---
# <a name="multi-protocol-access-on-azure-data-lake-storage"></a>Azure Veri Gölü Depolama'da çoklu protokol erişimi

Blob API'leri artık hiyerarşik ad alanına sahip hesaplarla çalışır. Bu, hiyerarşik ad alanına sahip hesaplara çeşitli Blob depolama özelliklerinin yanı sıra araçlar, uygulamalar ve hizmetler ekosisteminin kilidini açar.

Yakın zamana kadar, nesne depolama ve analiz depolama için ayrı depolama çözümleri korumak zorunda kalmış olabilirsiniz. Bunun nedeni, Azure Veri Gölü Depolama Gen2'nin sınırlı ekosistem desteğine sahip olmasıdır. Ayrıca tanılama günlüğü gibi Blob hizmet özelliklerine sınırlı erişimi vardı. Çeşitli senaryoları gerçekleştirmek için verileri hesaplar arasında taşımanız olduğundan, parçalanmış bir depolama çözümlerini korumak zordur. Artık bunu yapmak zorunda değil.

Veri Gölü Depolama'daki çoklu protokol erişimi yle, araçlar, uygulamalar ve hizmetler ekosistemini kullanarak verilerinizle birlikte çalışabilirsiniz. Bu, üçüncü taraf araçları ve uygulamaları da içerir. Bunları değiştirmek zorunda kalmadan hiyerarşik ad alanına sahip hesaplara yönlendirebilirsiniz. Blob API'leri artık hiyerarşik ad alanına sahip hesaplardaki veriler üzerinde çalışabildiği için, bu uygulamalar Blob API'leri olarak adlandırsalar bile *olduğu gibi* çalışır.

[Tanılama günlüğü,](../common/storage-analytics-logging.md) [erişim katmanları](storage-blob-storage-tiers.md)ve Blob depolama yaşam döngüsü yönetim ilkeleri gibi [Blob](storage-lifecycle-management-concepts.md) depolama özellikleri artık hiyerarşik ad alanına sahip hesaplarla çalışır. Bu nedenle, bu önemli özelliklere erişimi kaybetmeden blob Depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz. 

> [!NOTE]
> Veri Gölü Depolama'da çoklu protokol erişimi genellikle kullanılabilir ve tüm bölgelerde kullanılabilir. Çoklu protokol erişimi tarafından etkinleştirilen bazı Azure hizmetleri veya Blob depolama özellikleri önizlemede kalır.  Bu makaleler, Blob depolama özellikleri ve Azure hizmet tümleştirmeleri için geçerli desteği özetler. 
>
> [Blob Depolama özellikleri Azure Veri Gölü Depolama Gen2'de kullanılabilir](data-lake-storage-supported-blob-storage-features.md)
>
>[Azure Veri Gölü Depolama Gen2'yi destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)

## <a name="how-multi-protocol-access-on-data-lake-storage-works"></a>Veri gölü depolamasına çoklu protokol erişimi nasıl çalışır?

Blob API'leri ve Veri Gölü Depolama Gen2 API'leri hiyerarşik ad alanına sahip depolama hesaplarında aynı veriler üzerinde çalışabilir. Data Lake Storage Gen2, blob API'lerini hiyerarşik ad alanı üzerinden yönlendirir, böylece birinci sınıf dizin işlemleri ve POSIX uyumlu erişim denetim listelerinden (AçS) yararlanabilirsiniz. 

![Veri Gölü Depolama kavramsal ında çoklu protokol erişimi](./media/data-lake-storage-interop/interop-concept.png) 

Blob API'yi kullanan varolan araçlar ve uygulamalar bu avantajlardan otomatik olarak yararlanır. Geliştiricilerin bunları değiştirmesi gerekmeyecek. Data Lake Storage Gen2, araçların ve uygulamaların verilere erişmek için kullandığı protokolden bağımsız olarak, sürekli olarak dizin ve dosya düzeyinde ACD uygular. 

## <a name="see-also"></a>Ayrıca bkz.

- [Blob Depolama özellikleri Azure Veri Gölü Depolama Gen2'de kullanılabilir](data-lake-storage-supported-blob-storage-features.md)
- [Azure Veri Gölü Depolama Gen2'yi destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)
- [Azure Veri Gölü Depolama Gen2'yi destekleyen açık kaynak platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Veri Gölü Depolama Gen2 ile bilinen sorunlar](data-lake-storage-known-issues.md)




