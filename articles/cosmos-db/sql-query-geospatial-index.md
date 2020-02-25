---
title: Azure Cosmos DB Jeo uzamsal verileri dizinle
description: Uzamsal verileri Azure Cosmos DB ile dizinle
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: tisande
ms.openlocfilehash: 2cf682a404154b9c1bb94680b3adb673892c1c72
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77566380"
---
# <a name="index-geospatial-data-with-azure-cosmos-db"></a>Azure Cosmos DB Jeo uzamsal verileri dizinle

Azure Cosmos DB veritabanı altyapısını gerçekten şema belirsiz olacak şekilde tasarlıyoruz ve JSON için birinci sınıf desteğini sunduk. Azure Cosmos DB için en iyi duruma getirilmiş yazma veritabanı altyapısı, GeoJSON standardında temsil edilen uzamsal verileri yerel olarak anlamıştır.

Bir Nutshell 'de geometri, coğrafi olmayan koordinatlardan 2B düzlemin üzerine yansıtıldıysa, daha sonra **quadtree**kullanılarak hücrelere aşamalı olarak bölünür. Bu hücreler, noktaların yerini koruyan bir **Tepbert alanı doldurma eğrisi**içindeki hücrenin konumuna göre 1G ile eşleştirilir. Ayrıca, konum verileri dizinlendiğinde, **mozaik döşeme**olarak bilinen bir işlemden geçer, diğer bir deyişle, bir konumdan kesişen tüm hücreler Azure Cosmos DB dizininde anahtar olarak tanımlanır ve saklanır. Sorgu zamanında noktaları ve Çokgenleri gibi bağımsız değişkenleri ayrıca ilgili hücre kimliği aralıkları ayıklamak için grubun Mozaik ve dizinden veri almak için kullanılır.

/* (Tüm yollar) için uzamsal dizin içeren bir dizin oluşturma ilkesi belirtirseniz, kapsayıcıda bulunan tüm veriler etkili uzamsal sorgular için dizinlenir.

> [!NOTE]
> Azure Cosmos DB noktaların, LineStrings, çokgenler ve MultiPolygon dizinlemesini destekler
>
>

## <a name="geography-data-indexing-examples"></a>Coğrafya veri dizin oluşturma örnekleri

Aşağıdaki JSON kod parçacığında **Coğrafya** veri türü için etkin bir dizin oluşturma ilkesi gösterilmiştir. Coğrafya veri türü ile uzamsal veriler için geçerlidir ve uzamsal sorgulama için belgeler içinde bulunan coğrafi JSON noktası, Çokgen, MultiPolygon veya LineString dizimiyle dizin oluşturulur. Azure portal kullanarak dizin oluşturma ilkesini değiştiriyorsanız, kapsayıcıda uzamsal dizin oluşturmayı etkinleştirmek üzere dizin oluşturma ilkesi için aşağıdaki JSON 'u belirtebilirsiniz:

**Coğrafya uzamsal dizin oluşturma ile kapsayıcı dizin oluşturma ilkesi JSON**

```json
    {
       "automatic":true,
       "indexingMode":"Consistent",
        "includedPaths": [
        {
            "path": "/*"
        }
        ],
        "spatialIndexes": [
        {
            "path": "/*",
            "types": [
                "Point",
                "Polygon",
                "MultiPolygon",
                "LineString"
            ]
        }
    ],
       "excludedPaths":[]
    }
```

> [!NOTE]
> ' % S'konum GeoJSON değeri belge içinde hatalı biçimlendirilmiş veya geçersiz ise, ardından uzamsal sorgulama için dizine değil. Konum değerleri ST_ISVALID ve ST_ISVALIDDETAILED kullanarak doğrulayabilirsiniz.
>
>
>

[Dizin oluşturma Ilkesini](how-to-manage-indexing-policy.md) Azure CLI, PowerShell veya HERHANGI bir SDK kullanarak da değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de Jeo-uzamsal destek kullanmaya başlamak öğrendiniz, sonra şunları yapabilirsiniz:

* [Azure Cosmos db sorgu](sql-query-getting-started.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB uzamsal verileri sorgulama](sql-query-geospatial-query.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB Jeo-uzamsal ve coğrafi JSON konumu verileri](sql-query-geospatial-intro.md) hakkında daha fazla bilgi edinin