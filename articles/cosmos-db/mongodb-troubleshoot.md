---
title: Azure Cosmos DB'nin Mongo DB için API'sindeki sık karşılaşılan hataları giderme
description: Bu doküman, Azure Cosmos DB'nin MongoDB için API'sinde karşılaşılan sık karşılaşılan sorunları gidermenin yollarını tartışır.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 06/05/2019
ms.author: lbosq
ms.openlocfilehash: d9a4e336f582e866fd057f6c281f892ce07b34fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75941838"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB'nin MongoDB için API'sinde sık karşılaşılan sorunları giderme

Azure Cosmos DB, MongoDB de dahil olmak üzere yaygın NoSQL veritabanlarının tel protokollerini uygular. Tel protokolü uygulaması sayesinde, NoSQL veritabanlarıyla çalışan varolan istemci SDK'ları, sürücüleri ve araçları kullanarak Azure Cosmos DB ile saydam olarak etkileşimkurabilirsiniz. Azure Cosmos DB, NoSQL veritabanlarının herhangi biri için kablo uyumlu API'ler sağlamak için veritabanlarının kaynak kodunu kullanmaz. Tel protokolü sürümlerini anlayan herhangi bir MongoDB istemci sürücüsü Azure Cosmos DB'ye bağlanabilir.

Azure Cosmos DB'nin MongoDB api'si MongoDB'nin tel protokolünün 3,2 sürümüyle uyumlu olsa da (sürüm 3.4'e eklenen sorgu operatörleri ve özellikler şu anda önizleme olarak kullanılabilir), Azure Cosmos DB'ye karşılık gelen bazı özel hata kodları vardır belirli hatalar. Bu makalede, farklı hatalar, hata kodları ve bu hataları gidermek için adımlar açıklanmaktadır.

## <a name="common-errors-and-solutions"></a>Sık karşılaşılan hatalar ve çözümler

| Hata               | Kod  | Açıklama  | Çözüm  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Tüketilen toplam istek birimi sayısı, tahsilat için sağlanan istek birimi oranından fazladır ve azaltılmıştır. | Azure portalından bir kapsayıcıya veya kapsayıcı kümesine atanan iş kısmını ölçeklemeyi düşünün veya işlemi yeniden deneyebilirsiniz. |
| ExceededMemoryLimit | 16501 | Çok kiracılı bir hizmet olarak, işlem istemcinin bellek alanı üzerinden gitti. | Daha kısıtlayıcı sorgu ölçütleri veya [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)başvuru desteği yle işlemin kapsamını küçültün. Örnek: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| Belirtilen sipariş ekibe karşılık gelen dizin yolu hariç tutulur / Sorgu tarafından sıralanan sipariş, hizmet verebileceği karşılık gelen bir bileşik dizine sahip değildir. | 2 | Sorgu, dizine eklenmemiş bir alanda bir sıralama ister. | Denenen tür sorgusu için eşleşen bir dizin (veya bileşik dizin) oluşturun. |
| MongoDB kablo sürümü sorunları | - | MongoDB sürücülerinin eski sürümleri bağlantı dizelerinde Azure Cosmos hesabının adını algılayamaz. | Append *appName=@**hesapName** @ * MongoDB bağlantı dizesi için Cosmos DB's API sonunda, ***hesap Adı*** Cosmos DB hesap adıdır. |


## <a name="next-steps"></a>Sonraki adımlar

- Azure Cosmos DB'nin MongoDB için API'si ile [Studio 3T'yi](mongodb-mongochef.md) nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB'nin API'si ile [Robo 3T'yi](mongodb-robomongo.md) nasıl kullanacağınızı öğrenin.
- Azure Cosmos DB'nin MongoDB için API'si ile MongoDB [örneklerini](mongodb-samples.md) keşfedin.

