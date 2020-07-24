---
title: Mongo DB için Azure Cosmos DB API 'sindeki yaygın hataların sorunlarını giderme
description: Bu belgede, MongoDB için Azure Cosmos DB API 'sinde karşılaşılan yaygın sorunları gidermeye yönelik yollar ele alınmaktadır.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 07/15/2020
ms.author: lbosq
ms.openlocfilehash: f75374fc88923a0f131d513bebf0ffe1feeca359
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87076769"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'sindeki yaygın sorunları giderme

Aşağıdaki makalede MongoDB için Azure Cosmos DB API kullanan veritabanları için yaygın hatalar ve çözümler açıklanmaktadır.

>[!Note]
> Azure Cosmos DB MongoDB altyapısını barındırmıyor. MongoDB [hat Protokolü sürüm 3,6](mongodb-feature-support-36.md) ve eski [hat Protokolü sürüm 3,2](mongodb-feature-support.md)' nin bir uygulamasını sağlar, bu nedenle bu hatalardan bazıları yalnızca mongodb için Azure Cosmos DB API 'sinde bulunur. 

## <a name="common-errors-and-solutions"></a>Yaygın hatalar ve çözümler

| Hata               | Kod  | Açıklama  | Çözüm  |
|---------------------|-------|--------------|-----------|
| ExceededTimeLimit   | 50 | İstek, 60 saniyelik yürütmenin zaman aşımını aştı. | Bu hatanın pek çok nedeni olabilir. Nedenlerimizden biri, geçerli ayrılmış istek birimi kapasitesinin isteği tamamlayacak kadar yeterli olmadığı durumlarda oluşur. Bu, bu koleksiyonun veya veritabanının istek birimleri arttırılarak çözülebilir. Diğer durumlarda bu hata, büyük bir isteği daha küçük olanlara bölerek de çalıştırılabilir. |
| TooManyRequests     | 16500 | Tüketilen istek birimlerinin toplam sayısı, koleksiyonun sağlanan istek birimi hızından daha fazla ve kısıtlanmış. | Azure portal bir kapsayıcıya veya bir kapsayıcı kümesine atanmış üretilen işi ölçeklendirebilir veya işlemi yeniden deneyebilirsiniz. |
| ExceededMemoryLimit | 16501 | Çok kiracılı bir hizmet olarak, işlem istemcinin bellek işlem biriminden çıktı. | Daha kısıtlayıcı sorgu ölçütleri aracılığıyla işlemin kapsamını azaltın veya [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)destek ekibiyle iletişime geçin. Örnek: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| Belirtilen order by öğesine karşılık gelen dizin yolu hariç tutulur/order by sorgusunda, kendisine sunulabilecek bir bileşik dizin yoktur. | 2 | Sorgu, dizini oluşturulmamış bir alana sıralama isteğinde bulunur. | Denenen sıralama sorgusu için eşleşen bir dizin (veya bileşik dizin) oluşturun. |
| MongoDB kablo sürümü sorunları | - | MongoDB sürücülerinin eski sürümleri, bağlantı dizelerindeki Azure Cosmos hesabının adını algılayamaz. | The MongoDB bağlantı dizesi için Cosmos DB API 'sinin sonunda *appname = @**AccountName** @ * ekleyin; burada ***AccountName*** , Cosmos DB hesabınızın adıdır. |

## <a name="next-steps"></a>Sonraki adımlar

- [Studio 3T](mongodb-mongochef.md) 'ı Azure Cosmos DB MongoDB IÇIN API 'si ile nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB API 'SI ile [Robo 3T kullanmayı](mongodb-robomongo.md) öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.

