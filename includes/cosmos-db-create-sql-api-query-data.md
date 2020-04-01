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
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80502381"
---
Verilerinizi almak ve filtrelemek için Veri Gezgini'ndeki sorguları kullanabilirsiniz.

1. Veri Gezgini'ndeki **Öğeler** sekmesinin üst kısmında `SELECT * FROM c`varsayılan sorguyu gözden geçirin. Bu sorgu, kimlikle sipariş edilen kapsayıcıdaki tüm belgeleri alır ve görüntüler. 
   
   ![Veri Gezgini’ndeki varsayılan sorgu: `SELECT * FROM c`](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)
   
1. Sorguyu değiştirmek için **Filtreyi Edit'i** `ORDER BY c._ts DESC`seçin, varsayılan sorguyu değiştirin ve ardından **Filtre Uygula'yı**seçin.
   
   ![ORDER BY c._ts DESC ekleyerek ve Filtre Uygula’ya tıklayarak varsayılan sorguyu değiştirin](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

   Değiştirilen sorgu, belgeleri zaman damgasına göre azalan sırada görüntüler, bu nedenle şimdi ikinci belgeniz önce listelenir. 
   
   ![Sorguc._ts DESC ve Uygula Filtresi'ni tıklatarak ORDER olarak değiştirildi](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png)

SQL sözdizimini biliyorsanız, sorgu yüklemi kutusuna desteklenen [SQL sorgularını](../articles/cosmos-db/sql-api-sql-query.md) girebilirsiniz. Depolanan yordamlar, UDF'ler ve tetikleyiciler oluşturmak için sunucu tarafı iş mantığı için Veri Gezgini'ni de kullanabilirsiniz. 

Data Explorer, API'lerde bulunan tüm yerleşik programlı veri erişim özelliklerine kolay Azure portalı erişimi sağlar. Ayrıca, azure Cosmos DB hesabınız için iş elde etme, anahtar ve bağlantı dizelerini almak ve ölçümleri ve SLA'ları gözden geçirmek için portalı da kullanırsınız. 

