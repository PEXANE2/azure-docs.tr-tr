---
title: Kaynak ve nesne sınırlarını Azure Analysis Services | Microsoft Docs
description: Kaynak ve nesne sınırlarını Azure Analysis Services açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 08/23/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 40a5b68a12724f2574af19bb10c276c54c5afba0
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69997598"
---
# <a name="analysis-services-resource-and-object-limits"></a>Kaynak ve nesne sınırlarını Analysis Services

Bu makalede kaynak ve model nesne sınırları açıklanmaktadır.

## <a name="tier-limits"></a>Katman sınırları

Geliştirici, temel ve standart katmanlara yönelik QPU ve bellek sınırları için [Azure Analysis Services fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/analysis-services/)bakın.

## <a name="object-limits"></a>Nesne sınırları

Bu sınırlar teorik olarak ayarlanır. Performans, daha düşük sayılarda azalmalıdır.

|Object|En büyük boyut/sayı|  
|------------|----------------------------|  
|Bir örnekteki veritabanları|16,000|  
|Bir veritabanında birleştirilmiş tablo ve sütun sayısı|16,000|  
|Tablodaki satırlar|Sınırsız<br /><br /> **Warning** Tablodaki tek bir sütunun 1.999.999.997 ' den fazla farklı değere sahip olmadığı kısıtlamadır.|  
|Bir tablodaki hiyerarşiler|15.999|  
|Hiyerarşideki düzeyler|15.999|  
|İlişkiler|8,000|  
|Tüm tablodaki anahtar sütunları|15.999|  
|Tablolardaki ölçüler|2 ^ 31-1 = 2.147.483.647|  
|Sorgu tarafından döndürülen hücreler|2 ^ 31-1 = 2.147.483.647|  
|Kaynak sorgusunun kayıt boyutu|64 K|  
|Nesne adlarının uzunluğu|512 karakter|  


