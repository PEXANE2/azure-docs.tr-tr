---
title: Redis için Azure Cache nedir?
description: Önbellek, içerik önbelleğe alma, Kullanıcı oturumu önbelleğe alma, iş ve İleti Kuyruklama ve dağıtılmış işlemlerin etkinleştirilmesi için Redsıs için Azure önbelleği hakkında bilgi edinin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: overview
ms.date: 03/11/2020
ms.openlocfilehash: cd4e7c8e2693c25f3fc092fb53874a97cfd62434
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82113213"
---
# <a name="azure-cache-for-redis-description"></a>Redis için Azure Cache açıklaması

Redsıs için Azure önbelleği, açık kaynaklı yazılım [redin](https://redis.io/)temel alınarak bellek içi veri deposu sağlar. Reddir, önbellek olarak kullanıldığında, yoğun arka uç veri depolarında yoğun olan sistemlerin performansını ve ölçeklenebilirliğini geliştirir. Sık erişilen veriler uygulamaya yakın olan hızlı depolamaya kopyalanarak performans artırıldı. Redin için Azure Cache ile, bu hızlı depolama, diskten bir veritabanı tarafından yüklenmesi yerine bellek içinde bulunur.

Redo için Azure Cache, bellek içi veri yapısı deposu, dağıtılmış ilişkisel olmayan bir veritabanı ve bir ileti Aracısı olarak kullanılabilir. Redis altyapısının düşük gecikmeli, yüksek aktarım hızına sahip performansından yararlanılarak uygulama performansı iyileştirilir.

Redsıs için Azure önbelleği, güvenli ve adanmış bir Redsıs önbelleğine erişim sağlar. Redsıs için Azure önbelleği, Azure 'da barındırılan ve Azure içindeki veya dışındaki tüm uygulamalar tarafından erişilebilen Microsoft tarafından yönetilir. Ayrıca, Azure Redsıs for Cache, daha az çoğaltma stratejisini kullanır ve bu da ödeme kartı sektöriyle uyumluluğu geliştirir.

## <a name="using-azure-cache-for-redis"></a>Redsıs için Azure önbelleğini kullanma

Redis için Azure önbelleği, genel uygulama mimarisi desenlerini destekleyerek uygulama performansını geliştirir. En yaygın olanlarından bazıları şunlardır:

| Desen      | Açıklama                                        |
| ------------ | -------------------------------------------------- |
| [Edilgen Önbellek](cache-web-app-cache-aside-leaderboard.md) | Veritabanları genellikle bir önbelleğe doğrudan yüklenemeyecek kadar büyük olur. Yalnızca gerektiğinde önbelleğe veri yüklemek için [önbelleğe alma](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) deseninin kullanılması yaygındır. Sistem verilerde değişiklik yaptığında sistem, daha sonra diğer istemcilere dağıtılan önbelleği de güncelleştirebilir. Ayrıca, sistem veri üzerinde bir süre sonu ayarlayabilir veya veri güncelleştirmelerini önbelleğe tetikleyebilmesi için bir çıkarma ilkesi kullanabilir.|
| [İçeriği Önbelleğe Alma](cache-aspnet-output-cache-provider.md) | Üst bilgiler, alt bilgiler ve başlık gibi statik içerik kullanan şablonlardan birçok Web sayfası oluşturulur. Bu statik öğeler sık değişmemelidir. Bellek içi önbellek kullanmak, arka uç veri depolarına kıyasla statik içeriğe hızlı erişim sağlar. Bu model, işlem süresini ve sunucu yükünü azaltarak Web sunucularının daha hızlı yanıt vermesine olanak tanır. Yükü işlemek için gereken sunucu sayısını azaltmanıza izin verebilir. Redsıs için Azure Cache, ASP.NET ile bu kalıbı desteklemek için Redsıs çıkış önbelleği sağlayıcısı sağlar.|
| [Kullanıcı oturumunu önbelleğe alma](cache-aspnet-session-state-provider.md) | Bu model genellikle alışveriş sepetlerini ve bir Web uygulamasının Kullanıcı tanımlama bilgileriyle ilişkilendirmek isteyebileceğiniz diğer Kullanıcı Geçmişi verileri ile birlikte kullanılır. Bir tanımlama bilgisinde çok fazla bilgi depolamak, tanımlama bilgisi boyutu arttıkça ve tanımlama bilgisi her istek ile birlikte geçirilip doğrulandıkça performansı olumsuz yönde etkileyebilir. Tipik bir çözüm, bir veritabanındaki verileri sorgulamak için anahtar olarak tanımlama bilgisini kullanır. Redde için Azure önbelleği gibi bellek içi önbellek kullanmak, bilgilerin bir kullanıcıyla ilişkilendirilmesi, tam ilişkisel veritabanıyla etkileşime kıyasla çok daha hızlıdır. |
| İş ve ileti sıraya alma | Uygulamalar genellikle istekle ilişkili işlemler yürütme süresi geldiğinde bir kuyruğa görev ekler. Daha uzun çalışan işlemler, genellikle başka bir sunucu tarafından sırada işlenmek üzere sıraya alınır.  Bu iş erteleme yöntemine görevi sıraya alma adı verilir. Redsıs için Azure önbelleği, uygulamanızda bu düzenin etkinleştirilmesi için dağıtılmış bir sıra sağlar.|
| Dağıtılmış işlemler | Uygulamalar bazen bir arka uç veri deposunda tek bir atomik işlem olarak yürütülecek bir dizi komut gerektirir. Tüm komutlar başarılı olmalı veya tümü ilk durumuna geri döndürülmelidir. Redsıs için Azure Cache, tek bir [işlem](https://redis.io/topics/transactions)olarak toplu bir komut yürütmeyi destekler. |

## <a name="azure-cache-for-redis-offerings"></a>Redsıs teklifleri için Azure önbelleği

Redo için Azure Cache aşağıdaki katmanlarda kullanılabilir:

| Katman | Açıklama |
|---|---|
Temel | Tek düğümlü bir önbellek. Bu katman birden çok bellek boyutunu (250 MB-53 GB) destekler ve geliştirme/test ve kritik olmayan iş yükleri için idealdir. Temel katmanda hizmet düzeyi sözleşmesi (SLA) yoktur |
| Standart | Azure tarafından yüksek kullanılabilirliğe sahip bir SLA ile yönetilen, birincil/ikincil ve yapılandırma için çoğaltılan bir önbellek (% 99,9) |
| Premium | Premium katman, kurumsal olmaya yönelik katmandır. Premium katmanı Önbellekler daha fazla özelliği destekler ve düşük gecikmeyle daha yüksek aktarım hızına sahiptir. Premium katmanındaki önbellekler, Temel veya Standart Katmanına kıyasla daha iyi performans sağlayan daha güçlü donanımlara dağıtılır. Bu avantaj, aynı boyuttaki bir önbellek için üretilen iş hızının Standart katmana kıyasla Premium 'da daha yüksek olacağı anlamına gelir. |

> [!TIP]
> Premium önbellekler ile boyut, aktarım hızı ve bant genişliği hakkında daha fazla bilgi için bkz. [REDSıS hakkında Azure önbelleği](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use).
>

Oluşturulduktan sonra önbelleğinizi daha yüksek bir katmana ölçeklendirebilirsiniz. Ölçeğin daha düşük bir katmana indirilmesi desteklenmez. Adım adım ölçeklendirme yönergeleri için bkz. [Azure önbelleğini redsıs Için ölçeklendirme](cache-how-to-scale.md) ve [bir ölçeklendirme işlemini otomatikleştirme](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

### <a name="feature-comparison"></a>Özellik karşılaştırması

[Redsıs fiyatlandırma sayfası Için Azure önbelleği](https://azure.microsoft.com/pricing/details/cache/) , her katman için ayrıntılı bir karşılaştırma sağlar. Aşağıdaki tablo, katmana göre desteklenen özelliklerden bazılarını açıklamaya yardımcı olur:

| Özellik Açıklaması | Premium | Standart | Temel |
| ------------------- | :-----: | :------: | :---: |
| [Hizmet Düzeyi Sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Redis veri kalıcılığı](cache-how-to-premium-persistence.md) |✔|-|-|
| [Redis kümesi](cache-how-to-premium-clustering.md) |✔|-|-|
| [Güvenlik duvarı kuralları ile güvenlik](cache-configure.md#firewall) |✔|✔|✔|
| Aktarım sırasında şifreleme |✔|✔|✔|
| [VNet ile geliştirilmiş güvenlik ve yalıtım](cache-how-to-premium-vnet.md) |✔|-|-|
| [İçeri/Dışarı Aktarma](cache-how-to-import-export-data.md) |✔|-|-|
| [Zamanlanan güncelleştirmeler](cache-administration.md#schedule-updates) |✔|✔|✔|
| [Coğrafi çoğaltma](cache-how-to-geo-replication.md) |✔|-|-|
| [Yeniden başlatma](cache-administration.md#reboot) |✔|✔|✔|

## <a name="next-steps"></a>Sonraki adımlar

* [ASP.NET Web uygulaması hızlı başlangıç](cache-web-app-howto.md) Redsıs için Azure önbelleği kullanan basit bir ASP.NET Web uygulaması oluşturun.
* [.Net hızlı başlangıç](cache-dotnet-how-to-use-azure-redis-cache.md) Redsıs için Azure önbelleği kullanan bir .NET uygulaması oluşturun.
* [.NET Core hızlı başlangıç](cache-dotnet-core-quickstart.md) Redsıs için Azure önbelleği kullanan bir .NET Core uygulaması oluşturun.
* [Node. js hızlı başlangıç](cache-nodejs-get-started.md) Redsıs için Azure önbelleği kullanan basit bir Node. js uygulaması oluşturun.
* [Java hızlı başlangıç](cache-java-get-started.md) Redsıs için Azure önbelleği kullanan basit bir Java uygulaması oluşturun.
* [Python hızlı başlangıç](cache-python-get-started.md) Redsıs için Azure önbelleği kullanan bir Python uygulaması oluşturun.
