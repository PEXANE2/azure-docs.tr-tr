---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/05/2019
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 64c900e01496dad0d5f6a96d8d790ae0c2a8c95a
ms.sourcegitcommit: 9cbd5b790299f080a64bab332bb031543c2de160
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2020
ms.locfileid: "78927413"
---
Verilerinizi almak ve filtrelemek için Veri Gezgini sorguları kullanabilirsiniz.

1. Veri Gezgini **öğeleri** sekmesinin en üstünde, varsayılan sorgu `SELECT * FROM c`gözden geçirin. Bu sorgu, koleksiyondaki tüm belgeleri alır ve KIMLIK sırasına göre görüntüler. 
   
   ![Veri Gezgini’ndeki varsayılan sorgu: `SELECT * FROM c`](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)
   
1. Sorguyu değiştirmek için **Filtreyi Düzenle**' yi seçin, varsayılan sorguyu `ORDER BY c._ts DESC`değiştirin ve sonra **Filtre Uygula**' yı seçin.
   
   ![ORDER BY c._ts DESC ekleyerek ve Filtre Uygula’ya tıklayarak varsayılan sorguyu değiştirin](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

   Değiştirilen sorgu, belgeleri zaman damgalarına göre azalan sırada görüntüler, bu nedenle artık ikinci belgeniz önce listelenir. 
   
   ![Sorgu ORDER BY c olarak değiştirildi. _ts DESC ve Filtre Uygula 'ya tıklayın](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png)

SQL söz dizimini biliyorsanız, sorgu koşulu kutusuna desteklenen herhangi bir [SQL](../articles/cosmos-db/sql-api-sql-query.md) sorgusunu girebilirsiniz. Ayrıca, sunucu tarafı iş mantığı için saklı yordamlar, UDF 'ler ve Tetikleyiciler oluşturmak üzere Veri Gezgini de kullanabilirsiniz. 

Veri Gezgini, API 'lerde kullanılabilen tüm yerleşik programlı veri erişim özelliklerine kolay Azure portal erişim sağlar. Ayrıca, iş üretimini ölçeklendirmek, anahtar ve bağlantı dizelerini almak ve Azure Cosmos DB hesabınız için ölçümleri ve SLA 'Ları gözden geçirmek üzere portalını de kullanabilirsiniz. 

