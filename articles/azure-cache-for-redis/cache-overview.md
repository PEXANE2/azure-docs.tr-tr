---
title: Redis için Azure Cache nedir?
description: Önbellek kenara, içerik önbelleğe alma, kullanıcı oturumu önbelleğe alma, iş ve ileti kuyruk ve dağıtılmış işlemleri etkinleştirmek için Redis için Azure Önbelleği hakkında bilgi edinin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: overview
ms.date: 03/11/2020
ms.openlocfilehash: 38936000e426d560237295105b5456429d9ae16d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79126374"
---
# <a name="azure-cache-for-redis-description"></a>Redis için Azure Cache açıklaması

Redis için Azure Önbelleği, açık kaynak kodlu yazılım [Redis'i](https://redis.io/)temel alan bir bellek içi veri deposu sağlar. Önbellek olarak kullanıldığında Redis, arka uç veri depolarına büyük ölçüde dayanan sistemlerin performansını ve ölçeklenebilirliğini artırır. Sık erişilen verilerin uygulamaya yakın bulunan hızlı depolama alanına kopyalanmasıyla performans artırılır. Redis için Azure Önbelleği ile bu hızlı depolama alanı, diskten bir veritabanı tarafından yüklenmek yerine bellekte bulunur.

Redis için Azure Önbelleği bellek içi veri yapısı deposu, dağıtılmış ilişkisel olmayan veritabanı ve ileti aracısı olarak kullanılabilir. Redis altyapısının düşük gecikmeli, yüksek aktarım hızına sahip performansından yararlanılarak uygulama performansı iyileştirilir.

Redis için Azure Önbelleği, güvenli ve özel bir Redis önbelleğine erişim sağlar. Redis için Azure Önbelleği Microsoft tarafından yönetilir, Azure içinde barındırılır ve Azure içindeki veya dışındaki tüm uygulamalar tarafından erişilebilir.

## <a name="using-azure-cache-for-redis"></a>Redis için Azure Önbelleğini Kullanma

Redis için Azure Önbellek, ortak uygulama mimarisi desenlerini destekleyerek uygulama performansını artırır. En yaygın olanlarından bazıları şunlardır:

| Desen      | Açıklama                                        |
| ------------ | -------------------------------------------------- |
| [Edilgen Önbellek](cache-web-app-cache-aside-leaderboard.md) | Veritabanları genellikle doğrudan önbelleğe yüklenemeyecek kadar büyüktür. Verileri yalnızca gerektiğinde önbelleğe yüklemek için [önbellek kenara](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) deseni kullanmak yaygındır. Sistem verilerde değişiklik yaptığında, sistem önbelleği de güncelleştirebilir ve bu önbelleği güncelleştirebilir ve bu da diğer istemcilere dağıtılır. Ayrıca, sistem veriler üzerinde bir son kullanma süresi ayarlayabilir veya önbelleğe veri güncelleştirmelerini tetiklemek için bir çıkarma ilkesi kullanabilir.|
| [İçeriği Önbelleğe Alma](cache-aspnet-output-cache-provider.md) | Birçok web sayfası, üstbilgiler, altbilgiler, başlıklar gibi statik içerik kullanan şablonlardan oluşturulur. Bu statik öğeler sık sık değişmemelidir. Bellek içi önbellek kullanmak, arka uç veri depolarına kıyasla statik içeriğe hızlı erişim sağlar. Bu desen, işlem süresini ve sunucu yükünü azaltarak web sunucuların daha duyarlı olmasını sağlar. Bu, yükleri işlemek için gereken sunucu sayısını azaltmanıza olanak sağlar. Redis için Azure Önbelleği, Redis Çıktı Önbelleği Sağlayıcısı'nın bu deseni ASP.NET ile desteklemesini sağlar.|
| [Kullanıcı oturumunu önbelleğe alma](cache-aspnet-session-state-provider.md) | Bu desen genellikle alışveriş sepetleri ve bir web uygulamasının kullanıcı çerezleri ile ilişkilendirmek isteyebileceği diğer kullanıcı geçmişi verileriyle kullanılır. Bir tanımlama bilgisinde çok fazla bilgi depolamak, tanımlama bilgisi boyutu arttıkça ve tanımlama bilgisi her istek ile birlikte geçirilip doğrulandıkça performansı olumsuz yönde etkileyebilir. Tipik bir çözüm, veritabanındaki verileri sorgulamak için çerezi anahtar olarak kullanır. Bilgileri bir kullanıcıyla ilişkilendirmek için Redis için Azure Önbelleği gibi bir bellek içi önbellek kullanmak, tam bir ilişkisel veritabanıyla etkileşimde kalmaktan çok daha hızlıdır. |
| İş ve ileti sıraya alma | İsteğe ilişkin işlemlerin yürütülmesi zaman aldığında, uygulamalar genellikle sıraya görevler ekler. Daha uzun çalışan işlemler genellikle başka bir sunucu tarafından sırayla işlenecek şekilde sıralanır.  Bu iş erteleme yöntemine görevi sıraya alma adı verilir. Redis için Azure Önbelleği, uygulamanızda bu deseni etkinleştirmek için dağıtılmış bir sıra sağlar.|
| Dağıtılmış işlemler | Uygulamalar bazen tek bir atomik işlem olarak yürütülmesi için arka uç veri deposuna karşı bir dizi komut gerektirir. Tüm komutlar başarılı olmalı veya tümü ilk durumuna geri döndürülmelidir. Redis için Azure Önbelleği, tek bir [işlem](https://redis.io/topics/transactions)olarak bir komut toplu yürütmeyi destekler. |

## <a name="azure-cache-for-redis-offerings"></a>Redis teklifleri için Azure Önbelleği

Redis için Azure Önbelleği aşağıdaki katmanlarda kullanılabilir:

| Katman | Açıklama |
|---|---|
Temel | Tek düğümlü bir önbellek. Bu katman birden çok bellek boyutunu (250 MB - 53 GB) destekler ve geliştirme/test ve kritik olmayan iş yükleri için idealdir. Temel katmanda hizmet düzeyi sözleşmesi (SLA) yoktur |
| Standart | Yüksek kullanılabilirlik sla (%99,9) ile Azure tarafından yönetilen iki düğümlü, birincil/ikincil yapılandırmada yinelenen önbellek |
| Premium | Premium katman, Enterprise'a hazır katmandır. Premium katmanı Önbellekler daha fazla özelliği destekler ve düşük gecikmeyle daha yüksek aktarım hızına sahiptir. Premium katmanındaki önbellekler, Temel veya Standart Katmanına kıyasla daha iyi performans sağlayan daha güçlü donanımlara dağıtılır. Bu avantaj, aynı boyuttaki bir önbellek için üretim inin Premium'da Standart katmana göre daha yüksek olacağı anlamına gelir. |

> [!TIP]
> Boyut, iş ortası ve premium önbelleklerle bant genişliği hakkında daha fazla bilgi [için Redis SSS için Azure Önbelleği'ne](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)bakın.
>

Önbelleğinizi oluşturulduktan sonra daha yüksek bir katmana ölçeklendirebilirsiniz. Ölçeğin daha düşük bir katmana indirilmesi desteklenmez. Adım adım ölçeklendirme yönergeleri [için Redis için Azure Önbelleği Nasıl Ölçeklendirilir](cache-how-to-scale.md) ve [ölçeklendirme işlemini otomatikleştirin.](cache-how-to-scale.md#how-to-automate-a-scaling-operation)

### <a name="feature-comparison"></a>Özellik karşılaştırması

[Redis Fiyatlandırması için Azure Önbelleği](https://azure.microsoft.com/pricing/details/cache/) sayfası, her bir katmanın ayrıntılı bir karşılaştırmasını sağlar. Aşağıdaki tablo, katmana göre desteklenen özelliklerden bazılarını açıklamaya yardımcı olur:

| Özellik Açıklaması | Premium | Standart | Temel |
| ------------------- | :-----: | :------: | :---: |
| [Hizmet Düzeyi Sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Redis veri kalıcılığı](cache-how-to-premium-persistence.md) |✔|-|-|
| [Redis kümesi](cache-how-to-premium-clustering.md) |✔|-|-|
| [Güvenlik duvarı kuralları ile güvenlik](cache-configure.md#firewall) |✔|✔|✔|
| Aktarım sırasında şifreleme |✔|✔|✔|
| [VNet ile geliştirilmiş güvenlik ve yalıtım](cache-how-to-premium-vnet.md) |✔|-|-|
| [İçeri/Dışarı Aktarma](cache-how-to-import-export-data.md) |✔|-|-|
| [Zamanlanmış güncelleştirmeler](cache-administration.md#schedule-updates) |✔|✔|✔|
| [Coğrafi çoğaltma](cache-how-to-geo-replication.md) |✔|-|-|
| [Yeniden başlatma](cache-administration.md#reboot) |✔|✔|✔|

## <a name="next-steps"></a>Sonraki adımlar

* [ASP.NET Web Uygulaması Quickstart](cache-web-app-howto.md) Redis için Azure Önbelleği kullanan basit bir ASP.NET web uygulaması oluşturun.
* [.NET Hızlı Başlat](cache-dotnet-how-to-use-azure-redis-cache.md) Redis için Azure Önbelleği kullanan bir .NET uygulaması oluşturun.
* [.NET Çekirdek Hızlı Başlat](cache-dotnet-core-quickstart.md) Redis için Azure Önbelleği kullanan bir .NET Core uygulaması oluşturun.
* [Düğüm.js Quickstart](cache-nodejs-get-started.md) Redis için Azure Önbelleği kullanan basit bir Düğüm.js uygulaması oluşturun.
* [Java Quickstart](cache-java-get-started.md) Redis için Azure Önbelleği kullanan basit bir Java uygulaması oluşturun.
* [Python Hızlı Başlat](cache-python-get-started.md) Redis için Azure Önbelleği kullanan bir Python uygulaması oluşturun.
