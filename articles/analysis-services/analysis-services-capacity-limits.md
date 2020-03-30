---
title: Azure Analiz Hizmetleri kaynak ve nesne sınırları | Microsoft Dokümanlar
description: Bu makalede, bir Azure Çözümleme Hizmetleri sunucusu için kaynak ve nesne sınırları açıklanmaktadır.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f309c9863eb2f3065251537380a2977839f990d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73573208"
---
# <a name="analysis-services-resource-and-object-limits"></a>Analiz Hizmetleri kaynak ve nesne sınırları

Bu makalede kaynak ve model nesne sýnýnýrlarý açıklanmaktadır.

## <a name="tier-limits"></a>Katman sınırları

Geliştirici, temel ve standart katmanlar için QPU ve Bellek sınırları için [Azure Analiz Hizmetleri fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/analysis-services/)bakın.

## <a name="object-limits"></a>Nesne sınırları

Bu sınırlar teoriktir. Performans daha düşük sayılarda azalacaktır.

|Nesne|Maksimum boyutlar/sayılar|  
|------------|----------------------------|  
|Bir örnekteki veritabanları|16.000|  
|Veritabanındaki birleştirilmiş tablo ve sütun sayısı|16.000|  
|Tablodaki satırlar|Sınırsız<br /><br /> **Uyarı:** Bu kısıtlama ile tabloda tek bir sütun fazla 1.999.999.997 farklı değerlere sahip olabilir.|  
|Tablodaki hiyerarşiler|15,999|  
|Hiyerarşideki düzeyler|15,999|  
|İlişkiler|8,000|  
|Tüm tablodaki Anahtar Sütunlar|15,999|  
|Tablolardaki ölçüler|2^31-1 = 2.147.483.647|  
|Sorgu tarafından döndürülen hücreler|2^31-1 = 2.147.483.647|  
|Kaynak sorgusunun kayıt boyutu|64 K|  
|Nesne adlarının uzunluğu|512 karakter|  


