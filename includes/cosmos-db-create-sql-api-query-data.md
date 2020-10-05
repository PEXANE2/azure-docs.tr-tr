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
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "85115225"
---
Verilerinizi almak ve filtrelemek için Veri Gezgini sorguları kullanabilirsiniz.

1. Veri Gezgini **öğeleri** sekmesinin en üstünde, varsayılan sorguyu gözden geçirin `SELECT * FROM c` . Bu sorgu, KIMLIĞE göre sıralanmış kapsayıcıdan tüm belgeleri alır ve görüntüler. 
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png" alt-text="Veri Gezgini 'de varsayılan sorgu SELECT * FROM c":::
   
1. Sorguyu değiştirmek için **Filtreyi Düzenle**' yi seçin, varsayılan sorguyu ile değiştirin `ORDER BY c._ts DESC` ve sonra **Filtre Uygula**' yı seçin.
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png" alt-text="Veri Gezgini 'de varsayılan sorgu SELECT * FROM c":::

   Değiştirilen sorgu, belgeleri zaman damgalarına göre azalan sırada görüntüler, bu nedenle artık ikinci belgeniz önce listelenir. 
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png" alt-text="Veri Gezgini 'de varsayılan sorgu SELECT * FROM c":::

SQL söz dizimini biliyorsanız, sorgu koşulu kutusuna desteklenen herhangi bir [SQL](../articles/cosmos-db/sql-api-sql-query.md) sorgusunu girebilirsiniz. Ayrıca, sunucu tarafı iş mantığı için saklı yordamlar, UDF 'ler ve Tetikleyiciler oluşturmak üzere Veri Gezgini de kullanabilirsiniz. 

Veri Gezgini, API 'lerde kullanılabilen tüm yerleşik programlı veri erişim özelliklerine kolay Azure portal erişim sağlar. Ayrıca, iş üretimini ölçeklendirmek, anahtar ve bağlantı dizelerini almak ve Azure Cosmos DB hesabınız için ölçümleri ve SLA 'Ları gözden geçirmek üzere portalını de kullanabilirsiniz. 

