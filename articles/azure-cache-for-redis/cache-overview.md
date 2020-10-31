---
title: Redis için Azure Cache nedir?
description: Önbellek, içerik önbelleğe alma, Kullanıcı oturumu önbelleğe alma, iş ve İleti Kuyruklama ve dağıtılmış işlemlerin etkinleştirilmesi için Redsıs için Azure önbelleği hakkında bilgi edinin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: overview
ms.date: 05/12/2020
ms.openlocfilehash: 3bea474ae61ba4a0213d880934d9536d6ad71796
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93131641"
---
# <a name="azure-cache-for-redis"></a>Redis için Azure Önbelleği
Redsıs için Azure önbelleği, [redin](https://redis.io/) yazılımını temel alan bellek içi veri deposu sağlar. Redsıs, arka uç veri depolarında yoğun bir şekilde kullanılan bir uygulamanın performansını ve ölçeklenebilirliğini geliştirir. Sık erişilen verileri hızlı bir şekilde yazılabilen ve hızla okunabilen sunucu belleğinde tutarak, büyük hacimde uygulama isteğini işleyebilir. Redin, modern uygulamalara kritik düşük gecikmeli ve yüksek performanslı veri depolama çözümü getirir.

Redin için Azure Cache, redsıs açık kaynaklı ve bir ticari ürünün hem redsıs Labs hem de yönetilen bir hizmet olarak sunulur. Güvenli ve adanmış Redsıs sunucu örnekleri ve tam Redsıs API uyumluluğu sağlar. Hizmet, Azure 'da barındırılan ve Azure 'un içindeki veya dışındaki herhangi bir uygulamayla erişilebilen Microsoft tarafından çalıştırılır.

Redsıs için Azure önbelleği, dağıtılmış bir veri veya içerik önbelleği, bir oturum deposu, bir ileti Aracısı ve daha fazlası olarak kullanılabilir. Azure SQL veya Cosmos DB gibi diğer Azure veritabanı hizmetiyle tek başına veya yan yana dağıtılabilir.

## <a name="key-scenarios"></a>Önemli senaryolar
Redis için Azure önbelleği, genel uygulama mimarisi desenlerini destekleyerek uygulama performansını geliştirir. En yaygın olanlarından bazıları şunlardır:

| Desen      | Açıklama                                        |
| ------------ | -------------------------------------------------- |
| [Veri önbelleği](cache-web-app-cache-aside-leaderboard.md) | Veritabanları genellikle bir önbelleğe doğrudan yüklenemeyecek kadar büyük olur. Yalnızca gerektiğinde önbelleğe veri yüklemek için [önbelleğe alma](/azure/architecture/patterns/cache-aside) deseninin kullanılması yaygındır. Sistem verilerde değişiklik yaptığında sistem, daha sonra diğer istemcilere dağıtılan önbelleği de güncelleştirebilir. Ayrıca, sistem veri üzerinde bir süre sonu ayarlayabilir veya veri güncelleştirmelerini önbelleğe tetikleyebilmesi için bir çıkarma ilkesi kullanabilir.|
| [İçerik önbelleği](cache-aspnet-output-cache-provider.md) | Üst bilgiler, alt bilgiler ve başlık gibi statik içerik kullanan şablonlardan birçok Web sayfası oluşturulur. Bu statik öğeler sık değişmemelidir. Bellek içi önbellek kullanmak, arka uç veri depolarına kıyasla statik içeriğe hızlı erişim sağlar. Bu model, işlem süresini ve sunucu yükünü azaltarak Web sunucularının daha hızlı yanıt vermesine olanak tanır. Yükü işlemek için gereken sunucu sayısını azaltmanıza izin verebilir. Redsıs için Azure Cache, ASP.NET ile bu kalıbı desteklemek için Redsıs çıkış önbelleği sağlayıcısı sağlar.|
| [Oturum deposu](cache-aspnet-session-state-provider.md) | Bu model genellikle alışveriş sepetlerini ve bir Web uygulamasının Kullanıcı tanımlama bilgileriyle ilişkilendirmek isteyebileceğiniz diğer Kullanıcı Geçmişi verileri ile birlikte kullanılır. Bir tanımlama bilgisinde çok fazla bilgi depolamak, tanımlama bilgisi boyutu arttıkça ve tanımlama bilgisi her istek ile birlikte geçirilip doğrulandıkça performansı olumsuz yönde etkileyebilir. Tipik bir çözüm, bir veritabanındaki verileri sorgulamak için anahtar olarak tanımlama bilgisini kullanır. Redde için Azure önbelleği gibi bellek içi önbellek kullanmak, bilgilerin bir kullanıcıyla ilişkilendirilmesi, tam ilişkisel veritabanıyla etkileşime kıyasla çok daha hızlıdır. |
| İş ve ileti sıraya alma | Uygulamalar genellikle istekle ilişkili işlemler yürütme süresi geldiğinde bir kuyruğa görev ekler. Daha uzun çalışan işlemler, genellikle başka bir sunucu tarafından sırada işlenmek üzere sıraya alınır.  Bu iş erteleme yöntemine görevi sıraya alma adı verilir. Redsıs için Azure önbelleği, uygulamanızda bu düzenin etkinleştirilmesi için dağıtılmış bir sıra sağlar.|
| Dağıtılmış işlemler | Uygulamalar bazen bir arka uç veri deposunda tek bir atomik işlem olarak yürütülecek bir dizi komut gerektirir. Tüm komutlar başarılı olmalı veya tümü ilk durumuna geri döndürülmelidir. Redsıs için Azure Cache, tek bir [işlem](https://redis.io/topics/transactions)olarak toplu bir komut yürütmeyi destekler. |

## <a name="redis-versions"></a>Redsıs sürümleri

Redsıs için Azure Cache, OSS redin sürüm 4. x ve önizleme olarak 6,0 ' i destekler. En son sürümü size getirmek için redin 5,0 ' i atlama kararı yaptık. Daha önce, Reda için Azure önbelleği yalnızca tek bir Redsıs sürümü ile korunur. Daha yeni bir ana sürüm yükseltmesi ve ileride en az bir eski kararlı sürüm sağlayacak. Hangi sürümün uygulamanız için en uygun olanı [seçebileceğinizi seçebilirsiniz](cache-how-to-version.md) .

> [!NOTE]
> Redsıs 6,0 şu anda önizleme aşamasındadır. ilgilendiğiniz durumlarda [bizimle iletişim kurun](mailto:azurecache@microsoft.com) . Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="service-tiers"></a>Hizmet katmanları
Redo için Azure Cache aşağıdaki katmanlarda kullanılabilir:

| Katman | Açıklama |
|---|---|
| Temel | Tek bir VM üzerinde çalışan bir OSS Redsıs önbelleği. Bu katmanda hizmet düzeyi sözleşmesi (SLA) yoktur ve geliştirme/test ve kritik olmayan iş yükleri için idealdir. |
| Standart | Çoğaltılan bir yapılandırmadaki iki VM üzerinde çalışan bir OSS Redsıs önbelleği. |
| Premium | Yüksek performanslı OSS Redsıs önbellekler. Bu katman daha yüksek aktarım hızı, daha düşük gecikme süresi, daha iyi kullanılabilirlik ve daha fazla özellik sunar. Premium önbellekler, temel veya standart önbellekler ile karşılaştırıldığında daha güçlü sanal makinelere dağıtılır. |
| Kurumsal | Redsıs Labs ' Redsıs Enterprise Software tarafından desteklenen yüksek performanslı önbellekler. Bu katman RediSearch, RedisBloom ve RedisTimeSeries gibi Redsıs modüllerini destekler. Ayrıca, Premium katmandan daha yüksek kullanılabilirlik sağlar. |
| Enterprise Flash | Redsıs Labs ' Redsıs Enterprise Software tarafından desteklenen uygun maliyetli büyük önbellekler. Bu katman Redsıs veri depolamayı bir VM 'de DRAM 'dan farklı olan geçici olmayan belleğe genişletir. Toplam GB başına bellek maliyetini azaltır. |

### <a name="feature-comparison"></a>Özellik karşılaştırması
[Redin fiyatlandırması Için Azure önbelleği](https://azure.microsoft.com/pricing/details/cache/) , her katman için ayrıntılı bir karşılaştırma sağlar. Aşağıdaki tablo, katmana göre desteklenen özelliklerden bazılarını açıklamaya yardımcı olur:

| Özellik Açıklaması | Temel | Standart | Premium | Kurumsal | Enterprise Flash |
| ------------------- | :-----: | :------: | :---: | :---: | :---: |
| [Hizmet Düzeyi Sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |-|✔|✔|✔|✔|
| Veri şifrelemesi |✔|✔|✔|✔|✔|
| [Ağ yalıtımı](cache-how-to-premium-vnet.md) |✔|✔|✔|✔|✔|
| [Ölçeklendirme](cache-how-to-scale.md) |✔|✔|✔|✔|✔|
| [Bölge yedekliliği](cache-how-to-zone-redundancy.md) |-|-|✔|✔|✔|
| [Coğrafi çoğaltma](cache-how-to-geo-replication.md) |-|-|✔|-|-|
| [Veri kalıcılığı](cache-how-to-premium-persistence.md) |-|-|✔|-|-|
| [OSS kümesi](cache-how-to-premium-clustering.md) |-|-|✔|✔|✔|
| [Modül](https://redis.io/modules) |-|-|-|✔|-|
| [İçeri/Dışarı Aktarma](cache-how-to-import-export-data.md) |-|-|✔|✔|✔|
| [Zamanlanan güncelleştirmeler](cache-administration.md#schedule-updates) |✔|✔|✔|-|-|

### <a name="choosing-the-right-tier"></a>Doğru katmanı seçme
Redsıs katmanı için bir Azure önbelleği seçerken aşağıdakileri göz önünde bulundurmanız gerekir:

* **Bellek** : temel ve standart katmanlar 250 MB – 53 GB; Premium katman 6 GB-1,2 TB; Kurumsal katmanlar 12 GB-14 TB.  120 GB 'den büyük bir Premium katman önbelleği oluşturmak için Redsıs OSS kümeleme kullanabilirsiniz. Daha fazla bilgi için bkz. [Redsıs fiyatlandırması Için Azure önbelleği](https://azure.microsoft.com/pricing/details/cache/). Daha fazla bilgi için bkz. [redsıs Için Premium Azure önbelleği için kümeleme yapılandırma](cache-how-to-premium-clustering.md).
* **Ağ performansı** : yüksek aktarım hızı gerektiren bir iş yükünüz varsa, Premium veya Enterprise katmanı temel veya standart ile karşılaştırıldığında daha fazla bant genişliği sunar. Ayrıca, her katmanda daha büyük boyutlu önbellekler, önbelleği barındıran temel VM nedeniyle daha fazla bant genişliğine sahiptir. Daha fazla bilgi için bkz. [redsıs performansı Için Azure önbelleği](cache-planning-faq.md#azure-cache-for-redis-performance).
* **Aktarım hızı** : Premium katmanı, kullanılabilir en fazla üretilen işi sunar. Önbellek sunucusu veya istemcisi bant genişliği sınırlarına ulaşırsa, istemci tarafında zaman aşımları alabilirsiniz. Daha fazla bilgi için aşağıdaki tabloya bakın.
* **Yüksek kullanılabilirlik** : Reda Için Azure önbelleği birden çok [yüksek kullanılabilirlik](cache-high-availability.md) seçeneği sağlar. Standart, Premium veya kurumsal önbelleğin [SLA](https://azure.microsoft.com/support/legal/sla/cache/v1_0/)'imize göre kullanılabilir olmasını güvence altına alır. SLA yalnızca önbellek uç noktalarına olan bağlantıyı içerir. SLA, veri kaybından korumayı kapsamaz. Veri kaybına karşı dayanıklılığı artırmak için Premium katmanda Redsıs veri kalıcılığı özelliğini kullanmanızı öneririz.
* **Veri kalıcılığı** : Premium katmanı, önbellek verilerini bir Azure depolama hesabında kalıcı hale bırakmanıza olanak tanır. Diğer katmanlarda, veriler yalnızca bellekte depolanır. Temel altyapı sorunları, olası veri kaybına neden olabilir. Veri kaybına karşı dayanıklılığı artırmak için Premium katmanda Redsıs veri kalıcılığı özelliğini kullanmanızı öneririz. Redsıs için Azure önbelleği, Redsıs kalıcılığı 'nda RDB ve AOF (Önizleme) seçeneklerini sunmaktadır. Daha fazla bilgi için bkz. [redsıs Için Premium Azure önbelleği için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md).
* **Ağ yalıtımı** : Azure özel bağlantısı ve sanal ağ (VNet) dağıtımları, Reda Için Azure önbelleğiniz için gelişmiş güvenlik ve trafik yalıtımı sağlar. VNET, ağ erişim denetimi ilkeleriyle erişimi daha fazla kısıtlayabilmeniz için izin verir. Daha fazla bilgi için bkz. [Azure özel bağlantısı Ile redsıs Için Azure önbelleği](cache-private-link.md) ve [Redsıs Için Premium bir Azure önbelleği için sanal ağ desteğini yapılandırma](cache-how-to-premium-vnet.md).
* **İstemci bağlantısı sayısı üst sınırı** : Premium katmanı, daha büyük boyutlu önbellekler için daha fazla bağlantı ile redo 'ya bağlanabilecek maksimum istemci sayısını sağlar. Kümeleme, kümelenmiş bir önbellek için kullanılabilen bağlantı sayısını artırmaz. Daha fazla bilgi için bkz. [redsıs fiyatlandırması Için Azure önbelleği](https://azure.microsoft.com/pricing/details/cache/).
* **Redsıs Server Için adanmış çekirdek** : C0 dışında tüm ÖNBELLEKLER adanmış VM çekirdeğini çalıştırır.
* **Tek iş parçacıklı işleme** : redsıs, tasarıma göre, komut işleme için yalnızca bir iş parçacığı kullanır. Redde için Azure Cache, g/ç işleme için ek çekirdekler kullanır. Daha fazla çekirdeğe sahip olmak, üretilen iş performansını artırır ve bu da doğrusal ölçekleme üretmeyebilir. Ayrıca, daha büyük VM boyutları genellikle daha küçük olanlara göre daha yüksek bant genişliği limitleriyle gelir. Bu, uygulamanızda zaman aşımları oluşmasına neden olacak ağ doygunluğunun önlenmesine yardımcı olur.
* **Performans iyileştirmeleri** : Premium ve kurumsal katmanlardaki önbellekler, daha hızlı işlemciler içeren donanımda dağıtılır ve temel veya Standart katmana kıyasla daha iyi bir performans sağlar. Premium katman önbelleklerine daha yüksek aktarım hızı ve düşük gecikme süreleri vardır. Daha fazla bilgi için bkz. [redsıs performansı Için Azure önbelleği](cache-planning-faq.md#azure-cache-for-redis-performance).

Oluşturulduktan sonra, temel katmandaki önbelleğinizi Premium 'a ölçeklendirebilirsiniz. Ölçeğin daha düşük bir katmana indirilmesi desteklenmez. Adım adım ölçeklendirme yönergeleri için bkz. [Azure önbelleğini redsıs Için ölçeklendirme](cache-how-to-scale.md) ve [bir ölçeklendirme işlemini otomatikleştirme](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

### <a name="enterprise-tier-requirements"></a>Kurumsal katman gereksinimleri

Kurumsal katmanlar redin Labs 'in ticari bir sürümü olan redsıs Enterprise 'ı kullanır. Müşteriler bir Azure Market teklifi aracılığıyla bu yazılıma yönelik bir lisans edinecektir ve ödeyebilir. Redsıs için Azure önbelleği, lisans alımı kolaylaştırmaya yardımcı olur, böylece ayrı olarak bunu yapmanız gerekmez. Azure Marketi 'nde satın almak için aşağıdaki önkoşullara sahip olmanız gerekir:
* Azure aboneliğinizin geçerli bir ödeme aracı vardır. Azure kredileri veya ücretsiz MSDN Abonelikleri desteklenmez.
* Aboneliğin sahibi veya katılımcısı olursunuz.
* Kuruluşunuz [Azure Marketi satın alımlara](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-azure-marketplace#enabling-azure-marketplace-purchases)izin veriyor.
* Özel bir market kullanıyorsanız, redin Labs kurumsal teklifini içermesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* [Redsıs örneği için Azure önbelleği oluşturma](quickstart-create-redis.md)
* [Kurumsal katman önbelleği oluşturma](quickstart-create-redis-enterprise.md)
* [ASP.NET Web uygulamasında Redsıs için Azure önbelleğini kullanma](cache-web-app-howto.md)
* [.NET Core 'da Redsıs için Azure önbelleğini kullanma](cache-dotnet-core-quickstart.md)
* [Redsıs için Azure önbelleğini kullanın .NET Framework](cache-dotnet-how-to-use-azure-redis-cache.md)
* [Redsıs için Azure önbelleğini kullanın Node.js](cache-nodejs-get-started.md)
* [Java 'da Redsıs için Azure önbelleğini kullanma](cache-java-get-started.md)
* [Python 'da Redsıs için Azure önbelleğini kullanma](cache-python-get-started.md)
