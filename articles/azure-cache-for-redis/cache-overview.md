---
title: Redis için Azure Cache nedir?
description: Önbellek, içerik önbelleğe alma, Kullanıcı oturumu önbelleğe alma, iş ve İleti Kuyruklama ve dağıtılmış işlemlerin etkinleştirilmesi için Redsıs için Azure önbelleği hakkında bilgi edinin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: overview
ms.date: 05/12/2020
ms.openlocfilehash: 3751560125ea8ac6cc00ed63521bff30b751e688
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009605"
---
# <a name="azure-cache-for-redis"></a>Redis için Azure Önbelleği
Redsıs için Azure önbelleği, açık kaynaklı yazılım [redin](https://redis.io/)temel alınarak bellek içi veri deposu sağlar. Redsıs, arka uç veri depolarında yoğun bir şekilde kullanılan bir uygulamanın performansını ve ölçeklenebilirliğini geliştirir. Sık erişilen verileri hızlı bir şekilde yazılabilen ve hızla okunabilen sunucu belleğinde tutarak, büyük hacimde uygulama isteğini işleyebilir. Redin, modern uygulamalara kritik düşük gecikmeli ve yüksek performanslı veri depolama çözümü getirir.

Redsıs için Azure önbelleği, yönetilen bir hizmet olarak Redsıs sunmaktadır. Güvenli ve adanmış Redsıs sunucu örnekleri ve tam Redsıs API uyumluluğu sağlar. Hizmet, Azure 'da barındırılan ve Azure 'un içindeki veya dışındaki herhangi bir uygulamayla erişilebilen Microsoft tarafından çalıştırılır.

Redsıs için Azure önbelleği, dağıtılmış bir veri veya içerik önbelleği, bir oturum deposu, bir ileti Aracısı ve daha fazlası olarak kullanılabilir. Azure SQL veya Cosmos DB gibi diğer Azure veritabanı hizmetiyle tek başına veya yan yana dağıtılabilir.

## <a name="key-scenarios"></a>Önemli senaryolar
Redis için Azure önbelleği, genel uygulama mimarisi desenlerini destekleyerek uygulama performansını geliştirir. En yaygın olanlarından bazıları şunlardır:

| Desen      | Açıklama                                        |
| ------------ | -------------------------------------------------- |
| [Veri önbelleği](cache-web-app-cache-aside-leaderboard.md) | Veritabanları genellikle bir önbelleğe doğrudan yüklenemeyecek kadar büyük olur. Yalnızca gerektiğinde önbelleğe veri yüklemek için [önbelleğe alma](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) deseninin kullanılması yaygındır. Sistem verilerde değişiklik yaptığında sistem, daha sonra diğer istemcilere dağıtılan önbelleği de güncelleştirebilir. Ayrıca, sistem veri üzerinde bir süre sonu ayarlayabilir veya veri güncelleştirmelerini önbelleğe tetikleyebilmesi için bir çıkarma ilkesi kullanabilir.|
| [İçerik önbelleği](cache-aspnet-output-cache-provider.md) | Üst bilgiler, alt bilgiler ve başlık gibi statik içerik kullanan şablonlardan birçok Web sayfası oluşturulur. Bu statik öğeler sık değişmemelidir. Bellek içi önbellek kullanmak, arka uç veri depolarına kıyasla statik içeriğe hızlı erişim sağlar. Bu model, işlem süresini ve sunucu yükünü azaltarak Web sunucularının daha hızlı yanıt vermesine olanak tanır. Yükü işlemek için gereken sunucu sayısını azaltmanıza izin verebilir. Redsıs için Azure Cache, ASP.NET ile bu kalıbı desteklemek için Redsıs çıkış önbelleği sağlayıcısı sağlar.|
| [Oturum deposu](cache-aspnet-session-state-provider.md) | Bu model genellikle alışveriş sepetlerini ve bir Web uygulamasının Kullanıcı tanımlama bilgileriyle ilişkilendirmek isteyebileceğiniz diğer Kullanıcı Geçmişi verileri ile birlikte kullanılır. Bir tanımlama bilgisinde çok fazla bilgi depolamak, tanımlama bilgisi boyutu arttıkça ve tanımlama bilgisi her istek ile birlikte geçirilip doğrulandıkça performansı olumsuz yönde etkileyebilir. Tipik bir çözüm, bir veritabanındaki verileri sorgulamak için anahtar olarak tanımlama bilgisini kullanır. Redde için Azure önbelleği gibi bellek içi önbellek kullanmak, bilgilerin bir kullanıcıyla ilişkilendirilmesi, tam ilişkisel veritabanıyla etkileşime kıyasla çok daha hızlıdır. |
| İş ve ileti sıraya alma | Uygulamalar genellikle istekle ilişkili işlemler yürütme süresi geldiğinde bir kuyruğa görev ekler. Daha uzun çalışan işlemler, genellikle başka bir sunucu tarafından sırada işlenmek üzere sıraya alınır.  Bu iş erteleme yöntemine görevi sıraya alma adı verilir. Redsıs için Azure önbelleği, uygulamanızda bu düzenin etkinleştirilmesi için dağıtılmış bir sıra sağlar.|
| Dağıtılmış işlemler | Uygulamalar bazen bir arka uç veri deposunda tek bir atomik işlem olarak yürütülecek bir dizi komut gerektirir. Tüm komutlar başarılı olmalı veya tümü ilk durumuna geri döndürülmelidir. Redsıs için Azure Cache, tek bir [işlem](https://redis.io/topics/transactions)olarak toplu bir komut yürütmeyi destekler. |

## <a name="service-tiers"></a>Hizmet katmanları
Redo için Azure Cache aşağıdaki katmanlarda kullanılabilir:

| Katman | Açıklama |
|---|---|
Temel | Tek düğümlü bir önbellek. Bu katman birden çok bellek boyutunu (250 MB-53 GB) destekler ve geliştirme/test ve kritik olmayan iş yükleri için idealdir. Temel katmanda hizmet düzeyi anlaşması (SLA) yoktur. |
| Standart | Yüksek kullanılabilirlik [SLA 'sı](https://azure.microsoft.com/support/legal/sla/cache/v1_0/)ile Azure tarafından yönetilen iki düğümlü, birincil/çoğaltma, yapılandırma içindeki çoğaltılan bir önbellek. |
| Premium | Premium katman, kurumsal olmaya yönelik katmandır. Premium katmanı Önbellekler daha fazla özelliği destekler ve düşük gecikmeyle daha yüksek aktarım hızına sahiptir. Premium katmanındaki önbellekler, Temel veya Standart Katmanına kıyasla daha iyi performans sağlayan daha güçlü donanımlara dağıtılır. Bu avantaj, aynı boyuttaki bir önbellek için üretilen iş hızının Standart katmana kıyasla Premium 'da daha yüksek olacağı anlamına gelir. |

### <a name="feature-comparison"></a>Özellik karşılaştırması
[Redin fiyatlandırması Için Azure önbelleği](https://azure.microsoft.com/pricing/details/cache/) , her katman için ayrıntılı bir karşılaştırma sağlar. Aşağıdaki tablo, katmana göre desteklenen özelliklerden bazılarını açıklamaya yardımcı olur:

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

### <a name="choosing-the-right-tier"></a>Doğru katmanı seçme
Redsıs katmanı için bir Azure önbelleği seçerken aşağıdakileri göz önünde bulundurmanız gerekir.

* **Bellek**: temel ve standart katmanlar 250 MB – 53 GB sunar. Premium katman en fazla 1,2 TB (küme olarak) veya 120 GB (kümelenmemiş). Daha fazla bilgi için bkz. [Redsıs fiyatlandırması Için Azure önbelleği](https://azure.microsoft.com/pricing/details/cache/).
* **Ağ performansı**: yüksek aktarım hızı gerektiren bir iş yükünüz varsa, Premium katmanı standart veya temel ile karşılaştırıldığında daha fazla bant genişliği sunar. Ayrıca, her katmanda daha büyük boyutlu önbellekler, önbelleği barındıran temel VM nedeniyle daha fazla bant genişliğine sahiptir. Daha fazla bilgi için bkz. [redsıs performansı Için Azure önbelleği](cache-planning-faq.md#azure-cache-for-redis-performance).
* **Aktarım hızı**: Premium katmanı, kullanılabilir en fazla üretilen işi sunar. Önbellek sunucusu veya istemcisi bant genişliği sınırlarına ulaşırsa, istemci tarafında zaman aşımları alabilirsiniz. Daha fazla bilgi için aşağıdaki tabloya bakın.
* **Yüksek kullanılabilirlik**: redsıs Için Azure önbelleği, bir Standart/Premium önbelleğin [SLA](https://azure.microsoft.com/support/legal/sla/cache/v1_0/)'imize göre kullanılabilir olmasını güvence altına alır. SLA yalnızca önbellek uç noktalarına olan bağlantıyı içerir. SLA, veri kaybından korumayı kapsamaz. Veri kaybına karşı dayanıklılığı artırmak için Premium katmanda Redsıs veri kalıcılığı özelliğini kullanmanızı öneririz.
* **Redsıs veri kalıcılığı**: Premium katmanı, önbellek verilerini bir Azure depolama hesabında kalıcı hale bırakmanıza olanak tanır. Temel/standart önbellekte, tüm veriler yalnızca bellekte depolanır. Temel altyapı sorunları, olası veri kaybına neden olabilir. Veri kaybına karşı dayanıklılığı artırmak için Premium katmanda Redsıs veri kalıcılığı özelliğini kullanmanızı öneririz. Redsıs için Azure önbelleği, Redsıs kalıcılığı 'nda RDB ve AOF (Önizleme) seçeneklerini sunmaktadır. Daha fazla bilgi için bkz. [redsıs Için Premium Azure önbelleği için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md).
* **Redsıs kümesi**: 120 GB 'den daha büyük önbellekler oluşturmak veya birden çok redin düğümüne veri eklemek için Premium katmanda bulunan reddo kümeleme kullanabilirsiniz. Her düğüm, yüksek kullanılabilirlik için birincil/çoğaltma önbellek çiftinin oluşur. Daha fazla bilgi için bkz. [redsıs Için Premium Azure önbelleği için kümeleme yapılandırma](cache-how-to-premium-clustering.md).
* **Gelişmiş güvenlik ve ağ yalıtımı**: Azure sanal ağ (VNet) dağıtımı, Azure önbelleğiniz için gelişmiş güvenlik ve yalıtımın yanı sıra alt ağlar, erişim denetim ilkeleri ve diğer özellikler için erişimi daha da kısıtlamak sağlar. Daha fazla bilgi için bkz. [bir Premium Azure önbelleği için bkz. redsıs Için sanal ağ desteğini yapılandırma](cache-how-to-premium-vnet.md).
* **Redsıs 'Yi yapılandırma**: hem standart hem de Premium katmanlarda, Redsıs 'Yi keyspace bildirimleri için yapılandırabilirsiniz.
* **İstemci bağlantısı sayısı üst sınırı**: Premium katmanı, daha büyük boyutlu önbellekler için daha fazla bağlantı ile redo 'ya bağlanabilecek maksimum istemci sayısını sağlar. Kümeleme, kümelenmiş bir önbellek için kullanılabilen bağlantı sayısını artırmaz. Daha fazla bilgi için bkz. [redsıs fiyatlandırması Için Azure önbelleği](https://azure.microsoft.com/pricing/details/cache/).
* **Redsıs sunucusu Için adanmış çekirdek**: Premium katmanda, redsıs için özel bir çekirdeğe sahiptir. Temel/standart katmanlarında C1 boyutu ve üzeri, Redsıs sunucusu için adanmış bir çekirdeğe sahiptir.
* **Tek iş parçacıklı işleme**: redsıs, tasarıma göre, komut işleme için yalnızca bir iş parçacığı kullanır. Redde için Azure Cache, g/ç işleme için ek çekirdekler kullanır. Daha fazla çekirdeğe sahip olmak, üretilen iş performansını artırır ve bu da doğrusal ölçekleme üretmeyebilir. Ayrıca, daha büyük VM boyutları genellikle daha küçük olanlara göre daha yüksek bant genişliği limitleriyle gelir. Bu, uygulamanızda zaman aşımları oluşmasına neden olacak ağ doygunluğunun önlenmesine yardımcı olur.
* **Performans iyileştirmeleri**: Premium katmanda bulunan önbellekler, daha hızlı işlemciler içeren donanımda dağıtılır ve temel veya Standart katmana kıyasla daha iyi bir performans sağlar. Premium katman önbelleklerine daha yüksek aktarım hızı ve düşük gecikme süreleri vardır. Daha fazla bilgi için bkz. [redsıs performansı Için Azure önbelleği](cache-planning-faq.md#azure-cache-for-redis-performance)

Oluşturulduktan sonra önbelleğinizi daha yüksek bir katmana ölçeklendirebilirsiniz. Ölçeğin daha düşük bir katmana indirilmesi desteklenmez. Adım adım ölçeklendirme yönergeleri için bkz. [Azure önbelleğini redsıs Için ölçeklendirme](cache-how-to-scale.md) ve [bir ölçeklendirme işlemini otomatikleştirme](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Sonraki adımlar
* [ASP.NET Web uygulaması hızlı başlangıç](cache-web-app-howto.md) Redsıs için Azure önbelleği kullanan basit bir ASP.NET Web uygulaması oluşturun.
* [.Net hızlı başlangıç](cache-dotnet-how-to-use-azure-redis-cache.md) Redsıs için Azure önbelleği kullanan bir .NET uygulaması oluşturun.
* [.NET Core hızlı başlangıç](cache-dotnet-core-quickstart.md) Redsıs için Azure önbelleği kullanan bir .NET Core uygulaması oluşturun.
* [Hızlı başlangıçNode.js](cache-nodejs-get-started.md) Redsıs için Azure önbelleği kullanan basit bir Node.js uygulaması oluşturun.
* [Java hızlı başlangıç](cache-java-get-started.md) Redsıs için Azure önbelleği kullanan basit bir Java uygulaması oluşturun.
* [Python hızlı başlangıç](cache-python-get-started.md) Redsıs için Azure önbelleği kullanan bir Python uygulaması oluşturun.
