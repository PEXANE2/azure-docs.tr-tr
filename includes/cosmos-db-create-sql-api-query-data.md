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
ms.openlocfilehash: 408fb2c40e645d9a8b10f1e04d282e134c5489a1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80502381"
---
Verilerinizi almak ve filtrelemek için Veri Gezgini sorguları kullanabilirsiniz.

1. Veri Gezgini **öğeleri** sekmesinin en üstünde, varsayılan sorguyu `SELECT * FROM c`gözden geçirin. Bu sorgu, KIMLIĞE göre sıralanmış kapsayıcıdan tüm belgeleri alır ve görüntüler. 
   
   ![Veri Gezgini’ndeki varsayılan sorgu: `SELECT * FROM c`](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)
   
1. Sorguyu değiştirmek için **Filtreyi Düzenle**' yi seçin, varsayılan sorguyu ile `ORDER BY c._ts DESC`değiştirin ve sonra **Filtre Uygula**' yı seçin.
   
   ![ORDER BY c._ts DESC ekleyerek ve Filtre Uygula’ya tıklayarak varsayılan sorguyu değiştirin](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

   Değiştirilen sorgu, belgeleri zaman damgalarına göre azalan sırada görüntüler, bu nedenle artık ikinci belgeniz önce listelenir. 
   
   ![Sorgu ORDER BY c olarak değiştirildi. _ts DESC ve Filtre Uygula 'ya tıklayın](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png)

SQL söz dizimini biliyorsanız, sorgu koşulu kutusuna desteklenen herhangi bir [SQL](../articles/cosmos-db/sql-api-sql-query.md) sorgusunu girebilirsiniz. Ayrıca, sunucu tarafı iş mantığı için saklı yordamlar, UDF 'ler ve Tetikleyiciler oluşturmak üzere Veri Gezgini de kullanabilirsiniz. 

Veri Gezgini, API 'lerde kullanılabilen tüm yerleşik programlı veri erişim özelliklerine kolay Azure portal erişim sağlar. Ayrıca, iş üretimini ölçeklendirmek, anahtar ve bağlantı dizelerini almak ve Azure Cosmos DB hesabınız için ölçümleri ve SLA 'Ları gözden geçirmek üzere portalını de kullanabilirsiniz. 

