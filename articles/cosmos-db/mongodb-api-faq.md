---
title: Azure Cosmos DB MongoDB için API 'SI hakkında sık sorulan sorular
description: Azure Cosmos DB MongoDB API 'SI hakkında sık sorulan soruların yanıtlarını alın
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: sngun
ms.openlocfilehash: de75ea1bc0a1cf63c74be3f7d9e486e1fe38db6f
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82614570"
---
# <a name="frequently-asked-questions-about-the-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB MongoDB için API 'SI hakkında sık sorulan sorular

Azure Cosmos DB MongoDB için API 'SI, uygulamaların mevcut, topluluk tarafından desteklenen SDK 'Ları ve MongoDB için sürücüleri kullanarak yerel Azure Cosmos veritabanı altyapısına kolayca ve şeffaf bir şekilde iletişim kurmasına olanak tanıyan bir tel protokol uyumluluk katmanıdır. Geliştiriciler artık Azure Cosmos DB faydalanan uygulamalar oluşturmak için mevcut MongoDB araç zincirlerini ve yeteneklerini kullanabilir. Geliştiriciler, çok yöneticili çoğaltma, Otomatik Dizin oluşturma, yedekleme bakımı, mali olarak desteklenen hizmet düzeyi sözleşmeleri (SLA 'Lar) vb. genel dağıtımı içeren Azure Cosmos DB benzersiz yetilerinden yararlanır.

## <a name="how-do-i-connect-to-my-database"></a>Nasıl yaparım? veritabanıma bağlansın mı?

MongoDB için Azure Cosmos DB API 'SI ile Cosmos veritabanına bağlanmak için en hızlı yol, [Azure Portal](https://portal.azure.com). Hesabınıza gidin ve ardından sol gezinti menüsünde **hızlı başlangıç**' ye tıklayın. Hızlı başlangıç, veritabanınıza bağlanmak için kod parçacıkları almanın en iyi yoludur.

Azure Cosmos DB katı güvenlik gereksinimleri ve standartları uygular. Azure Cosmos DB hesapların TLS aracılığıyla kimlik doğrulaması ve güvenli iletişim olması gerekir, bu nedenle TLSv 1.2 kullandığınızdan emin olun.

Daha fazla bilgi için bkz. [MongoDB için Azure Cosmos DB API 'Siyle Cosmos veritabanınıza bağlanma](connect-mongodb-account.md).

## <a name="error-codes-while-using-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'SI kullanılırken hata kodları var mı?

Ortak MongoDB hata kodlarıyla birlikte Azure Cosmos DB MongoDB için API 'sinin kendi özel hata kodları vardır:

| Hata               | Kod  | Açıklama  | Çözüm  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Tüketilen istek birimlerinin toplam sayısı, kapsayıcının sağlanan istek birimi oranından daha fazla ve kısıtlanmış. | Azure portal bir kapsayıcıya veya bir kapsayıcı kümesine atanmış üretilen işi ölçeklendirmeye veya yeniden denemeye dikkat edin. |
| ExceededMemoryLimit | 16501 | Çok kiracılı bir hizmet olarak, işlem istemcinin bellek işlem biriminden çıktı. | Daha kısıtlayıcı sorgu ölçütleri aracılığıyla işlemin kapsamını azaltın veya [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)destek ekibiyle iletişime geçin. <br><br> Örnek: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |

## <a name="supported-drivers"></a>Desteklenen sürücüler

### <a name="is-the-simba-driver-for-mongodb-supported-for-use-with-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Simba sürücüsü Azure Cosmos DB MongoDB için API 'SI ile kullanılmak üzere destekleniyor mu?

Evet, MongoDB için Azure Cosmos DB API 'SI ile SIBA 'nın Mongo ODBC sürücüsünü kullanabilirsiniz

## <a name="next-steps"></a>Sonraki adımlar

* [MongoDB için Azure Cosmos DB API 'sini kullanarak .NET Web uygulaması oluşturma](create-mongodb-dotnet.md)
* [Azure Cosmos DB 'de Java ve MongoDB API 'SI ile bir konsol uygulaması oluşturma](create-mongodb-java.md)
