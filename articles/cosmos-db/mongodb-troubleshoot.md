---
title: Mongo DB için Azure Cosmos DB API 'sindeki yaygın hataların sorunlarını giderme
description: Bu belgede, MongoDB için Azure Cosmos DB API 'sinde karşılaşılan yaygın sorunları gidermeye yönelik yollar ele alınmaktadır.
author: christopheranderson
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 07/15/2020
ms.author: chrande
ms.openlocfilehash: 9d76c3d9943300f88a146e82b862624d491cf546
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93360216"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'sindeki yaygın sorunları giderme
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Aşağıdaki makalede MongoDB için Azure Cosmos DB API kullanan veritabanları için yaygın hatalar ve çözümler açıklanmaktadır.

>[!Note]
> Azure Cosmos DB MongoDB altyapısını barındırmıyor. MongoDB [hat Protokolü sürüm 3,6](mongodb-feature-support-36.md) ve eski [hat Protokolü sürüm 3,2](mongodb-feature-support.md)' nin bir uygulamasını sağlar, bu nedenle bu hatalardan bazıları yalnızca mongodb için Azure Cosmos DB API 'sinde bulunur. 

## <a name="common-errors-and-solutions"></a>Genel hatalar ve çözümleri

| Hata               | Kod  | Açıklama  | Çözüm  |
|---------------------|-------|--------------|-----------|
| ExceededTimeLimit   | 50 | İstek 60 saniyelik yürütme zaman aşımı süresini aştı. | Bu hatanın pek çok nedeni olabilir. Nedenlerinden biri geçerli ayrılmış istek birimi kapasitesinin isteğe tamamlamak için yeterli olmamasıdır. Bu durum söz konusu koleksiyonun veya veritabanının istek birimlerini artırarak çözülebilir. Diğer durumlarda bu hata, büyük bir isteği daha küçük olanlara bölerek de çalıştırılabilir. |
| TooManyRequests     | 16500 | Kullanılan toplam istek birimi sayısı, koleksiyon için sağlanan istek birimi hızından fazla ve kısıtlanmış. | Azure portaldan bir kapsayıcıya veya kapsayıcı kümesine atanan aktarım hızını ölçeklendirmeyi göz önünde bulundurabilir veya işlemi yeniden deneyebilirsiniz. |
| ExceededMemoryLimit | 16501 | Çok kiracılı bir hizmet olarak, işlem istemcinin bellek işlem biriminden çıktı. | Daha kısıtlayıcı sorgu ölçütleri aracılığıyla işlemin kapsamını azaltın veya [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)destek ekibiyle iletişime geçin. Örnek: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| Belirtilen öğeye göre sipariş değerine karşılık gelen dizin yolu dışlandı / Sıralama ölçütü sorgusunun hizmet verebilecek ilgili bileşik dizini yok. | 2 | Sorgu dizine alınmamış bir alana göre sıralama istiyor. | Denenen sıralama sorgusu için eşleşen bir dizin (veya bileşik dizin) oluşturun. |
| MongoDB kablo sürümü sorunları | - | MongoDB sürücülerinin eski sürümleri, bağlantı dizelerindeki Azure Cosmos hesabının adını algılayamaz. | The MongoDB bağlantı dizesi için Cosmos DB API 'sinin sonunda *appname = @ **AccountName** @* ekleyin; burada ***AccountName*** , Cosmos DB hesabınızın adıdır. |

## <a name="next-steps"></a>Sonraki adımlar

- [Studio 3T](mongodb-mongochef.md) 'ı Azure Cosmos DB MongoDB IÇIN API 'si ile nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB API 'SI ile [Robo 3T kullanmayı](mongodb-robomongo.md) öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.

