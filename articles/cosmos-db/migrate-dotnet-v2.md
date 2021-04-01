---
title: Uygulamanızı .NET SDK 2,0 (Microsoft. Azure. Cosmos) Azure Cosmos DB kullanmak için geçirin
description: Mevcut .NET uygulamanızı v1 SDK 'dan .NET SDK v2 for Core (SQL) API 'sine nasıl yükselteceğinizi öğrenin.
author: stefArroyo
ms.author: esarroyo
ms.service: cosmos-db
ms.topic: how-to
ms.date: 10/15/2020
ms.openlocfilehash: 88c40452da72ed4ab43d2d7613636136a5b78cfe
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94550110"
---
# <a name="migrate-your-application-to-use-the-azure-cosmos-db-net-sdk-v2"></a>Azure Cosmos DB .NET SDK v2 'yi kullanmak için uygulamanızı geçirin

> [!IMPORTANT]
> .NET SDK 'sının v3 'in Şu anda kullanılabilir olduğunu ve v2 'den v3 'e geçiş planının [burada](migrate-dotnet-v3.md)bulunduğunu unutmayın. Azure Cosmos DB .NET SDK v2 hakkında bilgi edinmek için [sürüm notlarına](sql-api-sdk-dotnet.md), [.net GitHub DEPOSUNA](https://github.com/Azure/azure-cosmos-dotnet-v2), .NET SDK v2 [performans ipuçlarına](performance-tips.md)ve [sorun giderme kılavuzuna](troubleshoot-dot-net-sdk.md)bakın.
>

Bu makalede, mevcut v1 .NET uygulamanızı çekirdek (SQL) API 'SI için .NET SDK v2 Azure Cosmos DB 'ye yükseltme hakkında bazı konular vurgulanmaktadır. Azure Cosmos DB .NET SDK v2, `Microsoft.Azure.DocumentDB` ad alanına karşılık gelir. Uygulamanızı, v2 SDK 'sını kullanmak için aşağıdaki Azure Cosmos DB .NET platformlarından herhangi birinden geçiriyorsanız, bu belgede belirtilen bilgileri kullanabilirsiniz `Microsoft.Azure.Cosmos` :

* SQL API için Azure Cosmos DB .NET Framework v1 SDK
* SQL API için Azure Cosmos DB .NET Core SDK v1

## <a name="whats-available-in-the-net-v2-sdk"></a>.NET v2 SDK 'sında kullanılabilen özellikler

V2 SDK, aşağıdakiler dahil olmak üzere birçok kullanılabilirlik ve performans geliştirmesi içerir:

* Windows olmayan istemciler için TCP doğrudan modu desteği
* Çok bölgeli yazma desteği
* Sorgu performansı geliştirmeleri
* Jeo-uzamsal/geometri koleksiyonları ve dizin oluşturma desteği
* Doğrudan/TCP Aktarım tanısı için daha fazla geliştirmeler
* Kurulan bağlantı sayısını azaltmak için doğrudan TCP Aktarım yığınında güncelleştirmeler
* RequestTimeout 'de gecikme süresi azalmasıyla iyileştirmeler

Yeniden deneme mantığı ve SDK 'sının alt seviyelerinin çoğu büyük ölçüde değişmeden kalır.

## <a name="why-migrate-to-the-net-v2-sdk"></a>Neden .NET v2 SDK 'sına geçirilir?

Çok sayıda performans iyileştirmelerine ek olarak, en son SDK 'da yapılan yeni özellik yatırımları eski sürümlere geri eklenmeyecektir.

Ayrıca, eski SDK 'lar yeni sürümlere göre değiştirilerek v1 SDK [bakım moduna](sql-api-sdk-dotnet.md)alınacaktır. En iyi geliştirme deneyimi için, uygulamanızı SDK 'nın sonraki bir sürümüne geçirmeyi öneririz.

## <a name="major-changes-from-v1-sdk-to-v2-sdk"></a>V1 SDK 'dan v2 SDK 'ya büyük değişiklikler

### <a name="direct-mode--tcp"></a>Doğrudan mod + TCP

.NET v2 SDK artık hem doğrudan hem de ağ geçidi modunu desteklemektedir. Doğrudan mod, TCP protokolü aracılığıyla bağlanırlığı destekler ve daha az ağ atlaması ile arka uç çoğaltmalarına doğrudan bağlandığından daha iyi performans sunar.

Daha fazla ayrıntı için [Azure Cosmos db SQL SDK bağlantı modları Kılavuzu](sql-sdk-connection-modes.md)' nu okuyun.

### <a name="session-token-formatting"></a>Oturum belirteci biçimlendirmesi

V2 SDK artık v1 'de kullanılan *basit* oturum belirteci biçimini kullanmaz, bunun yerine SDK *vektör* biçimlendirmesi kullanır. Biçimlerin, farklı sürümlerle istemci uygulamasına geçirilmesi sırasında dönüştürülmesi gerekir, çünkü biçimler birlikte değiştirilebilir değildir.

Daha fazla bilgi için bkz. [.NET SDK 'da oturum belirteci biçimlerini dönüştürme](how-to-convert-session-token.md).

### <a name="using-the-net-change-feed-processor-sdk"></a>.NET değişiklik akışı işlemcisi SDK 'sını kullanma

.NET değişiklik akışı işlemci kitaplığı 2.1. x, `Microsoft.Azure.DocumentDB` 2,0 veya üzeri bir sürüm gerektirir.

2.1. x kitaplığı aşağıdaki önemli değişikliklere sahiptir:

* Kararlılık ve tanılama geliştirmeleri
* Hataların ve özel durumların geliştirilmiş işlenmesi
* Bölümlenmiş kira koleksiyonları için ek destek
* `ChangeFeedDocument`Ek hata işleme ve izleme için arabirimi ve sınıfı uygulamak üzere gelişmiş uzantılar
* Bölüm başına devamlılık belirteçlerini kalıcı hale getirmek için özel bir depo kullanma desteği eklendi

Daha fazla bilgi için bkz. değişiklik akışı işlemci Kitaplığı [sürüm notları](sql-api-sdk-dotnet-changefeed.md).

### <a name="using-the-bulk-executor-library"></a>Toplu yürütücü kitaplığı 'nı kullanma

V2 toplu yürütücü kitaplığı şu anda Azure Cosmos DB .NET SDK 2.5.1 veya üzeri bir bağımlılığa sahiptir.

Daha fazla bilgi için Azure Cosmos DB bkz. [toplu yürütücü kitaplığına genel bakış](bulk-executor-overview.md) ve .net toplu yürütücü Kitaplığı [sürüm notları](sql-api-sdk-bulk-executor-dot-net.md).

## <a name="next-steps"></a>Sonraki adımlar

* En yüksek performansa ulaşmak için uygulamanızı en iyi duruma getirmek üzere SQL API v2 için Azure Cosmos DB kullanarak [ek performans ipuçları](sql-api-get-started.md) hakkında bilgi edinin
* [V2 SDK ile](sql-api-dotnet-samples.md) yapabilecekleriniz hakkında daha fazla bilgi edinin