---
title: Azure Veri Kataloğu'nda ilgili veri varlıklarını görüntüleme
description: Bu makalede, Azure Veri Kataloğu'nda seçili bir veri varlığının ilgili veri varlıklarının nasıl görüntüleneneaçıklanmaktadır.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 212ba647e6eb44e800a589928620f56fba65107c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68737026"
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Azure Veri Kataloğu'nda ilgili veri varlıkları nasıl görüntülenebilir?
Azure Veri Kataloğu, seçili bir veri varlığıyla ilgili veri varlıklarını görüntülemenize ve aralarındaki ilişkileri görüntülemenize olanak tanır. 

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları 
Aşağıdaki veri kaynaklarından veri varlıklarını kaydettiğinizde, Azure Veri Kataloğu seçili veri varlıkları arasındaki birleştirme ilişkileri yle ilgili meta verileri otomatik olarak kaydeder. 

- SQL Server
- Azure SQL Veritabanı
- MySQL
- Oracle

> [!NOTE]
> Veri Kataloğu'nun iki veri varlığı arasındaki ilişkiyi içe aktarabilmesi için her iki varlığı da aynı anda kaydetmeniz gerekir. Bunlardan birini ayrı olarak eklediyseniz, aralarındaki ilişkiyi almak için yeniden ve diğer veri kıymetini ekleyin.

## <a name="view-related-data-assets"></a>İlgili veri varlıklarını görüntüleme
Seçili bir veri kümesiyle ilişkili veri varlıklarını görüntülemek için, aşağıdaki resimde gösterildiği gibi **İlişkiler** sekmesini kullanın: 

![Azure Veri Kataloğu - İlgili veri varlıklarını görüntüleme](media/data-catalog-how-to-view-related-data-assets/relationships-tab.png)

Bu örnekte, seçili **ProductSubcategory** veri varlığı için iki ilişki vardır: 

- Ürün tablosunun ProductSubcategoryID sütunu, seçili ProductSubcategory tablosunun ProductSubcategoryID sütunu ile yabancı anahtar ilişkisine sahiptir. 
- ProductCategoryID tablosunun ProductCategoryID sütunu, seçili ProductCategory tablosunun ProductCategoryID sütunu ile yabancı anahtar ilişkisine sahiptir.

> [!NOTE]
> İlişkiler ağacı görünümünde okun yönüne dikkat edin.  

Sütunun tam nitelikli adı gibi daha fazla ayrıntı görmek için fareyi aşağı taşıyın ve aşağıdaki resme benzer bir açılır pencere görürsünüz: 

![Azure Veri Kataloğu - İlişki açılır](media/data-catalog-how-to-view-related-data-assets/relationship-popup.png)

Daha önce kaydedilmiş varlıklar arasındaki ilişkileri eklemek için, bu kıymetleri yeniden kaydedin.

## <a name="next-steps"></a>Sonraki adımlar
- [Veri varlıklarını yönetme](data-catalog-how-to-manage.md)