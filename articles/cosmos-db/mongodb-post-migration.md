---
title: MongoDB için Azure Cosmos DB API 'SI ile geçiş sonrası en iyi duruma getirme adımları
description: Bu belge MongoDB 'den Mongo DB için Azure Cosmos DB API 'sine yönelik geçiş sonrası iyileştirme teknikleri sağlar.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: lbosq
ms.openlocfilehash: ce33651aae64d0a90264dde6da64b4044c6ce132
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80063602"
---
# <a name="post-migration-optimization-steps-when-using-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'SI kullanılırken geçiş sonrası en iyi duruma getirme adımları

MongoDB veritabanında depolanan verileri MongoDB için Azure Cosmos DB API 'sine geçirdikten sonra, Azure Cosmos DB bağlanabilir ve verileri yönetebilirsiniz. Bu kılavuz, geçişten sonra dikkate almanız gereken adımları sağlar. Geçiş adımları için [MongoDB 'yi Azure Cosmos DB MongoDB IÇIN API 'Sine geçirme öğreticisine](../dms/tutorial-mongodb-cosmos-db.md) bakın.

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- [Uygulamanızı bağlama](#connect-your-application)
- [Dizin oluşturma ilkesini iyileştirme](#optimize-the-indexing-policy)
- [MongoDB için Azure Cosmos DB API 'SI için genel dağıtımı yapılandırma](#globally-distribute-your-data)
- [Tutarlılık düzeyini ayarla](#set-consistency-level)

> [!NOTE]
> Uygulama düzeyinizdeki tek zorunlu geçiş sonrası adımı, uygulamanızdaki bağlantı dizesini yeni Azure Cosmos DB hesabınıza işaret etmek üzere değiştiriyor. Diğer tüm geçiş adımları önerilen iyileştirmelerdir.
>

## <a name="connect-your-application"></a>Uygulamanızı bağlama

1. Yeni bir pencerede [Azure Portal](https://www.portal.azure.com/) oturum açın
2. [Azure Portal](https://www.portal.azure.com/)sol bölmede **tüm kaynaklar** menüsünü açın ve verilerinizi geçirdiğiniz Azure Cosmos DB hesabı bulun.
3. **Bağlantı dizesi** dikey penceresini açın. Sağ bölme, hesabınıza başarıyla bağlanmak için gereken tüm bilgileri içerir.
4. Uygulamanızda MongoDB bağlantısı için Azure Cosmos DB API 'sini yansıtmak için uygulamanızın yapılandırmasındaki (veya diğer ilgili yerlerdeki) bağlantı bilgilerini kullanın.
![Bağlantı dizesi](./media/mongodb-post-migration/connection-string.png)

Daha fazla ayrıntı için lütfen [MongoDB uygulamasını Azure Cosmos DB sayfasına bağlama](connect-mongodb-account.md) sayfasına bakın.

## <a name="optimize-the-indexing-policy"></a>Dizin oluşturma ilkesini iyileştirme

Verilerin Azure Cosmos DB geçişi sırasında varsayılan olarak tüm veri alanları otomatik olarak dizinlenir. Çoğu durumda, bu varsayılan dizin oluşturma ilkesi kabul edilebilir. Genel olarak, dizinleri kaldırmak yazma isteklerini iyileştirir ve varsayılan dizin oluşturma ilkesinin (yani, Otomatik Dizin oluşturma) okuma isteklerini iyileştirir.

Dizin oluşturma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB](index-overview.md) makalelerinde [mongodb IÇIN Azure Cosmos DB API 'sindeki veri dizini oluşturma](mongodb-indexing.md) .

## <a name="globally-distribute-your-data"></a>Verilerinizi Global olarak dağıtın

Azure Cosmos DB tüm dünyada [Azure bölgelerinde](https://azure.microsoft.com/regions/#services) kullanılabilir. Azure Cosmos DB hesabınız için varsayılan tutarlılık düzeyini seçtikten sonra, bir veya daha fazla Azure bölgesini ilişkilendirebilirsiniz (küresel dağıtım gereksinimlerinize bağlı olarak). Yüksek kullanılabilirlik ve iş sürekliliği için, her zaman en az 2 bölgede çalışmayı öneririz. [Azure Cosmos db içinde çok bölgeli dağıtımların maliyetini en iyi duruma getirme](optimize-cost-regions.md)ipuçlarını inceleyebilirsiniz.

Verilerinizi Global olarak dağıtmak için lütfen [MongoDB için Azure Cosmos DB API 'sindeki verileri küresel olarak dağıtma](tutorial-global-distribution-mongodb.md)bölümüne bakın.

## <a name="set-consistency-level"></a>Tutarlılık düzeyini ayarla

Azure Cosmos DB, 5 iyi tanımlanmış [tutarlılık düzeyi](consistency-levels.md)sunmaktadır. MongoDB ve Azure Cosmos DB tutarlılık düzeyleri arasındaki eşleme hakkında bilgi edinmek için, [tutarlılık düzeylerini ve Azure Cosmos DB API 'leri](consistency-levels-across-apis.md)okuyun. Varsayılan tutarlılık düzeyi, oturum tutarlılığı düzeyidir. Tutarlılık düzeyinin değiştirilmesi isteğe bağlıdır ve bunu uygulamanız için iyileştirebilirsiniz. Azure portal kullanarak tutarlılık düzeyini değiştirmek için:

1. Ayarlar altında **varsayılan tutarlılık** dikey penceresine gidin.
2. [Tutarlılık düzeyinizi](consistency-levels.md) seçin

Çoğu Kullanıcı, varsayılan oturum tutarlılığı ayarında tutarlılık düzeyini bırakır. Ancak, [çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans avantajları](consistency-levels-tradeoffs.md)vardır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB’ye MongoDB uygulaması bağlama](connect-mongodb-account.md)
* [Studio 3T kullanarak Azure Cosmos DB hesabına bağlanma](mongodb-mongochef.md)
* [Azure Cosmos DB MongoDB için API 'sini kullanarak genel olarak okuma dağıtımı](mongodb-readpreference.md)
* [MongoDB için Azure Cosmos DB API'siyle verilerde süre sonu](mongodb-time-to-live.md)
* [Azure Cosmos DB 'deki tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos DB’de dizin oluşturma](index-overview.md)
* [Azure Cosmos DB istek birimleri](request-units.md)