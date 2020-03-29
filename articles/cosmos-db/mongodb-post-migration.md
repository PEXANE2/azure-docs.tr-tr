---
title: Azure Cosmos DB'nin MongoDB için API'si ile geçiş sonrası optimizasyon adımları
description: Bu doküman, MongoDB'den Azure Cosmos DB'nin Mongo DB için APi'sine geçiş sonrası optimizasyon tekniklerini sağlar.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: lbosq
ms.openlocfilehash: ce33651aae64d0a90264dde6da64b4044c6ce132
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063602"
---
# <a name="post-migration-optimization-steps-when-using-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB'nin API'si kullanırken geçiş sonrası optimizasyon adımları

MongoDB veritabanında depolanan verileri Azure Cosmos DB'nin MongoDB api'sine geçirttikten sonra Azure Cosmos DB'ye bağlanabilir ve verileri yönetebilirsiniz. Bu kılavuz, geçişten sonra göz önünde bulundurmanız gereken adımları sağlar. Geçiş adımları için [MongoDB öğreticisi için Azure Cosmos DB'nin API'sine Geçir MongoDB'ye](../dms/tutorial-mongodb-cosmos-db.md) bakın.

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- [Uygulamanızı bağlayın](#connect-your-application)
- [Dizin oluşturma ilkesini optimize etme](#optimize-the-indexing-policy)
- [MongoDB için Azure Cosmos DB'nin API'si için küresel dağıtımı yapılandırma](#globally-distribute-your-data)
- [Tutarlılık düzeyini ayarlama](#set-consistency-level)

> [!NOTE]
> Uygulama düzeyinizdeki tek zorunlu geçiş sonrası adım, uygulamanızdaki bağlantı dizesini yeni Azure Cosmos DB hesabınıza işaret etmek üzere değiştirmektir. Diğer tüm geçiş adımları en iyi duruma düşükleri önerilir.
>

## <a name="connect-your-application"></a>Uygulamanızı bağlayın

1. [Azure portalında](https://www.portal.azure.com/) yeni bir pencere işareti
2. Azure [portalından,](https://www.portal.azure.com/)sol bölmede **Tüm kaynaklar** menüsünü açın ve verilerinizi aktardığınız Azure Cosmos DB hesabını bulun.
3. Bağlantı **String** bıçağını açın. Sağ bölme, hesabınıza başarıyla bağlanmak için gereken tüm bilgileri içerir.
4. Uygulamanızda Azure Cosmos DB'nin MongoDB bağlantısı için API'sini yansıtmak için uygulamanızın yapılandırmasındaki bağlantı bilgilerini (veya diğer ilgili yerlerde) kullanın.
![Bağlantı-String](./media/mongodb-post-migration/connection-string.png)

Daha fazla bilgi için lütfen [MongoDB uygulamasını Azure Cosmos DB sayfasına bağlayın.](connect-mongodb-account.md)

## <a name="optimize-the-indexing-policy"></a>Dizin oluşturma ilkesini optimize etme

Tüm veri alanları varsayılan olarak, verilerin Azure Cosmos DB'ye geçişi sırasında otomatik olarak dizine eklenir. Çoğu durumda, bu varsayılan dizin oluşturma ilkesi kabul edilebilir. Genel olarak, dizinlerin kaldırılması yazma isteklerini en iyi duruma getirerek varsayılan dizin oluşturma ilkesine (örneğin, otomatik dizin oluşturma) okuma isteklerini en iyi duruma getirerek en iyi duruma gelir.

Dizin oluşturma hakkında daha fazla bilgi için Azure [Cosmos DB'nin MongoDB api'sinde](mongodb-indexing.md) ve [Azure Cosmos DB makalelerinde Dizin oluşturma'ya](index-overview.md) bakın.

## <a name="globally-distribute-your-data"></a>Verilerinizi genel olarak dağıtın

Azure Cosmos DB, dünya çapındaki tüm [Azure bölgelerinde](https://azure.microsoft.com/regions/#services) kullanılabilir. Azure Cosmos DB hesabınız için varsayılan tutarlılık düzeyini seçtikten sonra, bir veya daha fazla Azure bölgesini ilişkilendirebilirsiniz (genel dağıtım gereksinimlerinize bağlı olarak). Yüksek kullanılabilirlik ve iş sürekliliği için her zaman en az 2 bölgede çalışmanızı öneririz. [Azure Cosmos DB'de çok bölgeli dağıtımların maliyetini optimize etme](optimize-cost-regions.md)ipuçlarını inceleyebilirsiniz.

Verilerinizi genel olarak dağıtmak için lütfen [Azure Cosmos DB'nin MongoDB için API'sindeki verileri genel olarak dağıt'a](tutorial-global-distribution-mongodb.md)bakın.

## <a name="set-consistency-level"></a>Tutarlılık düzeyini ayarlama

Azure Cosmos DB, iyi tanımlanmış 5 [tutarlılık düzeyi](consistency-levels.md)sunar. MongoDB ve Azure Cosmos DB tutarlılık düzeyleri arasındaki eşleme hakkında bilgi almak için [Tutarlılık düzeyleri ve Azure Cosmos DB API'lerini](consistency-levels-across-apis.md)okuyun. Varsayılan tutarlılık düzeyi oturum tutarlılık düzeyidir. Tutarlılık düzeyini değiştirmek isteğe bağlıdır ve uygulamanız için optimize edebilirsiniz. Azure portalını kullanarak tutarlılık düzeyini değiştirmek için:

1. Ayarlar altında **Varsayılan Tutarlılık** bıçağına gidin.
2. Tutarlılık [düzeyinizi](consistency-levels.md) seçin

Çoğu kullanıcı tutarlılık düzeyini varsayılan oturum tutarlılık ayarında bırakır. Ancak, [çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans dengeleri](consistency-levels-tradeoffs.md)vardır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB’ye MongoDB uygulaması bağlama](connect-mongodb-account.md)
* [Studio 3T'yi kullanarak Azure Cosmos DB hesabına bağlanın](mongodb-mongochef.md)
* [MongoDB için Azure Cosmos DB'nin API'sini kullanarak okumaları genel olarak dağıtma](mongodb-readpreference.md)
* [MongoDB için Azure Cosmos DB API'siyle verilerde süre sonu](mongodb-time-to-live.md)
* [Azure Cosmos DB'de Tutarlılık Düzeyleri](consistency-levels.md)
* [Azure Cosmos DB’de dizin oluşturma](index-overview.md)
* [Azure Cosmos DB'de İstek Birimleri](request-units.md)