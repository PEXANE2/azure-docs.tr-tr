---
title: dosya dahil etme
description: dosya dahil etme
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/05/2019
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 609b62312329b3a8f9f16d15458a0a47f5eb377f
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2020
ms.locfileid: "85115225"
---
Verilerinizi almak ve filtrelemek için Veri Gezgini sorguları kullanabilirsiniz.

1. Veri Gezgini **öğeleri** sekmesinin en üstünde, varsayılan sorguyu gözden geçirin `SELECT * FROM c` . Bu sorgu, KIMLIĞE göre sıralanmış kapsayıcıdan tüm belgeleri alır ve görüntüler. 
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png" alt-text="Veri Gezgini 'de varsayılan sorgu SELECT * FROM c":::
   
1. Sorguyu değiştirmek için **Filtreyi Düzenle**' yi seçin, varsayılan sorguyu ile değiştirin `ORDER BY c._ts DESC` ve sonra **Filtre Uygula**' yı seçin.
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png" alt-text="ORDER BY c._ts DESC ekleyerek ve Filtre Uygula’ya tıklayarak varsayılan sorguyu değiştirin":::

   Değiştirilen sorgu, belgeleri zaman damgalarına göre azalan sırada görüntüler, bu nedenle artık ikinci belgeniz önce listelenir. 
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png" alt-text="Sorgu ORDER BY c olarak değiştirildi. _ts DESC ve Filtre Uygula 'ya tıklayın":::

SQL söz dizimini biliyorsanız, sorgu koşulu kutusuna desteklenen herhangi bir [SQL](../articles/cosmos-db/sql-api-sql-query.md) sorgusunu girebilirsiniz. Ayrıca, sunucu tarafı iş mantığı için saklı yordamlar, UDF 'ler ve Tetikleyiciler oluşturmak üzere Veri Gezgini de kullanabilirsiniz. 

Veri Gezgini, API 'lerde kullanılabilen tüm yerleşik programlı veri erişim özelliklerine kolay Azure portal erişim sağlar. Ayrıca, iş üretimini ölçeklendirmek, anahtar ve bağlantı dizelerini almak ve Azure Cosmos DB hesabınız için ölçümleri ve SLA 'Ları gözden geçirmek üzere portalını de kullanabilirsiniz. 

