---
title: Kaynak ve nesne sınırlarını Azure Analysis Services | Microsoft Docs
description: Bu makalede bir Azure Analysis Services sunucusu için kaynak ve nesne sınırları açıklanmaktadır.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c2caab4e449f4299d00fff14b697887ec00f35e6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83697501"
---
# <a name="analysis-services-resource-and-object-limits"></a>Kaynak ve nesne sınırlarını Analysis Services

Bu makalede kaynak ve model nesne sınırları açıklanmaktadır.

## <a name="tier-limits"></a>Katman sınırları

Geliştirici, temel ve standart katmanlara yönelik QPU ve bellek sınırları için [Azure Analysis Services fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/analysis-services/)bakın.

## <a name="object-limits"></a>Nesne sınırları

Bu sınırlar teorik olarak ayarlanır. Performans, daha düşük sayılarda azalmalıdır.

|Nesne|En büyük boyut/sayı|  
|------------|----------------------------|  
|Bir örnekteki veritabanları|16.000|  
|Bir veritabanında birleştirilmiş tablo ve sütun sayısı|16.000|  
|Tablodaki satırlar|Sınırsız<br /><br /> **Uyarı:** Tablodaki tek bir sütunun 1.999.999.997 ' den fazla farklı değere sahip olmadığı kısıtlamadır.|  
|Bir tablodaki hiyerarşiler|15.999|  
|Hiyerarşideki düzeyler|15.999|  
|İlişkiler|8,000|  
|Tüm tablodaki anahtar sütunları|15.999|  
|Tablolardaki ölçüler|2 ^ 31-1 = 2.147.483.647|  
|Sorgu tarafından döndürülen hücreler|2 ^ 31-1 = 2.147.483.647|  
|Kaynak sorgusunun kayıt boyutu|64 K|  
|Nesne adlarının uzunluğu|512 karakter|  


