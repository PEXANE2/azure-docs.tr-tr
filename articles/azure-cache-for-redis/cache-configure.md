---
title: Redis için Azure Önbelleği nasıl yapılandırılır?
description: Redis için Azure Önbelleği için varsayılan Redis yapılandırmasını anlayın ve Redis örnekleri için Azure Önbelleğinizi nasıl yapılandıracağınızı öğrenin
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 08/22/2017
ms.author: yegu
ms.openlocfilehash: 3f0de52782694e6cbc8fdb6b55d545191dbbb350
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010316"
---
# <a name="how-to-configure-azure-cache-for-redis"></a>Redis için Azure Önbelleği nasıl yapılandırılır?
Bu konu, Redis örnekleri için Azure Önbelleğiniz için kullanılabilen yapılandırmaları açıklar. Bu konu, Redis örnekleri için Azure Önbelleği için varsayılan Redis sunucu yapılandırmasını da kapsar.

> [!NOTE]
> Premium önbellek özelliklerini yapılandırma ve kullanma hakkında daha fazla bilgi için [kalıcılık yapılandırma,](cache-how-to-premium-persistence.md) [kümeleme nasıl yapılandırılabildiği](cache-how-to-premium-clustering.md)ve [Sanal Ağ desteğinin nasıl yapılandırılabildiği](cache-how-to-premium-vnet.md)hakkında bilgi sahibi okadardır.
>
>

## <a name="configure-azure-cache-for-redis-settings"></a>Redis ayarları için Azure Önbelleğini yapılandırma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Redis için Azure Önbelleği **ayarları, Kaynak Menüsü**kullanılarak **Redis için Azure Önbelleği'nde** görüntülenir ve yapılandırılır.

![Redis Ayarları için Azure Önbelleği](./media/cache-configure/redis-cache-settings.png)

**Kaynak Menüsünü**kullanarak aşağıdaki ayarları görüntüleyebilir ve yapılandırabilirsiniz.

* [Genel bakış](#overview)
* [Etkinlik günlüğü](#activity-log)
* [Erişim denetimi (IAM)](#access-control-iam)
* [Etiketler](#tags)
* [Sorunları tanılama ve çözme](#diagnose-and-solve-problems)
* [Ayarlar](#settings)
    * [Erişim tuşları](#access-keys)
    * [Gelişmiş ayarlar](#advanced-settings)
    * [Redis Danışmanı için Azure Önbelleği](#azure-cache-for-redis-advisor)
    * [Ölçek](#scale)
    * [Küme boyutu](#cluster-size)
    * [Veri kalıcılığı](#redis-data-persistence)
    * [Güncelleştirmeleri zamanlama](#schedule-updates)
    * [Coğrafi çoğaltma](#geo-replication)
    * [Sanal Ağ](#virtual-network)
    * [Güvenlik duvarı](#firewall)
    * [Özellikler](#properties)
    * [Kilitler](#locks)
    * [Otomasyon betiği](#automation-script)
* Yönetim
    * [Veri içeri aktarma](#importexport)
    * [Verileri dışarı aktarma](#importexport)
    * [Yeniden başlatma](#reboot)
* [İzleme](#monitoring)
    * [Redis ölçümleri](#redis-metrics)
    * [Uyarı kuralları](#alert-rules)
    * [Tanılama](#diagnostics)
* Sorun giderme ayarlarını & destek
    * [Kaynak durumu](#resource-health)
    * [Yeni destek isteği](#new-support-request)


## <a name="overview"></a>Genel Bakış

**Genel bakış,** önbelleğiniz hakkında ad, bağlantı noktaları, fiyatlandırma katmanı ve seçili önbellek ölçümleri gibi temel bilgileri sağlar.

### <a name="activity-log"></a>Etkinlik günlüğü

Önbelleğinizde gerçekleştirilen eylemleri görüntülemek için **Etkinlik günlüğüne** tıklayın. Bu görünümü diğer kaynakları da içerecek şekilde genişletmek için filtreleme de kullanabilirsiniz. Denetim günlükleriyle çalışma hakkında daha fazla bilgi için [Kaynak Yöneticisi ile Denetim işlemlerine](../azure-resource-manager/management/view-activity-logs.md)bakın. Redis olayları için Azure Önbelleğini izleme hakkında daha fazla bilgi için [İşlemler ve uyarılara](cache-how-to-monitor.md#operations-and-alerts)bakın.

### <a name="access-control-iam"></a>Erişim denetimi (IAM)

**Erişim denetimi (IAM)** bölümü, Azure portalındaki rol tabanlı erişim denetimi (RBAC) desteği sağlar. Bu yapılandırma, kuruluşların erişim yönetimi gereksinimlerini basit ve kesin bir şekilde karşılamalarına yardımcı olur. Daha fazla bilgi için [Azure portalında Rol tabanlı erişim denetimine](../role-based-access-control/role-assignments-portal.md)bakın.

### <a name="tags"></a>Etiketler

**Etiketler** bölümü kaynaklarınızı düzenlemenize yardımcı olur. Daha fazla bilgi için bkz. [Etiketleri kullanarak Azure kaynaklarınızı düzenleme](../azure-resource-manager/management/tag-resources.md).


### <a name="diagnose-and-solve-problems"></a>Sorunları tanılayın ve çözün

**Sorunları tanıla'yı tıklatın ve** bunları çözmek için ortak sorunlar ve stratejilerle sağlanacak sorunları çözün.



## <a name="settings"></a>Ayarlar
**Ayarlar** bölümü önbelleğiniz için aşağıdaki ayarlara erişmenizi ve yapılandırmanızı sağlar.

* [Erişim tuşları](#access-keys)
* [Gelişmiş ayarlar](#advanced-settings)
* [Redis Danışmanı için Azure Önbelleği](#azure-cache-for-redis-advisor)
* [Ölçek](#scale)
* [Küme boyutu](#cluster-size)
* [Veri kalıcılığı](#redis-data-persistence)
* [Güncelleştirmeleri zamanlama](#schedule-updates)
* [Coğrafi çoğaltma](#geo-replication)
* [Sanal Ağ](#virtual-network)
* [Güvenlik duvarı](#firewall)
* [Özellikler](#properties)
* [Kilitler](#locks)
* [Otomasyon betiği](#automation-script)



### <a name="access-keys"></a>Erişim tuşları
Önbelleğinizin erişim anahtarlarını görüntülemek veya yeniden oluşturmak için **Access anahtarlarını** tıklatın. Bu anahtarlar önbelleğinize bağlanan istemciler tarafından kullanılır.

![Redis Erişim Anahtarları için Azure Önbelleği](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a>Gelişmiş ayarlar
Aşağıdaki ayarlar **Gelişmiş ayarlar** bıçak üzerinde yapılandırılır.

* [Erişim Bağlantı Noktaları](#access-ports)
* [Bellek ilkeleri](#memory-policies)
* [Anahtar alanı bildirimleri (gelişmiş ayarlar)](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a>Erişim Bağlantı Noktaları
Varsayılan olarak, TLS/SSL olmayan erişim yeni önbellekler için devre dışı bırakılır. TLS olmayan bağlantı noktasını etkinleştirmek **için, Yalnızca** **Gelişmiş ayarlar** bıçağında **SSL üzerinden erişime izin ver'e** hayır'ı tıklatın ve ardından **Kaydet'i**tıklatın.

> [!NOTE]
> TLS erişimi Redis için Azure Önbelleği'ne erişim şu anda TLS 1.0, 1.1 ve 1.2'yi destekler, ancak 1.0 ve 1.1 sürümleri yakında kullanımdan kaldırılmaktadır.  Daha fazla bilgi için [lütfen TLS 1.0 ve 1.1 sayfamızı](cache-remove-tls-10-11.md) okuyunuz.

![Redis Access Bağlantı Noktaları için Azure Önbelleği](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a>Bellek ilkeleri
**Maxmemory ilkesi**, **maxmemory ayrılmış**ve gelişmiş **ayarlar** bıçak **üzerinde maxfragmentationmemory ayrılmış** ayarları önbellek için bellek ilkeleri yapılandırır.

![Redis Maxmemory İlkesi için Azure Önbelleği](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory ilkesi** önbellek için çıkarma ilkesini yapılandırır ve aşağıdaki çıkarma ilkeleri arasından seçim yapmanızı sağlar:

* `volatile-lru`- Bu varsayılan tahliye ilkesidir.
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

İlkeler hakkında `maxmemory` daha fazla bilgi için [Tahliye ilkeleri'ne](https://redis.io/topics/lru-cache#eviction-policies)bakın.

**Maksimum bellek ayrılmış** ayar, başarısızlık sırasında çoğaltma gibi önbellek dışı işlemler için ayrılmış olan MB bellek miktarını yapılandırır. Bu değeri ayarlamak, yükünüz değiştiğinde daha tutarlı bir Redis sunucu deneyimi yaşamanızı sağlar. Bu değer, ağır yazma iş yükleri için daha yüksek ayarlanmalıdır. Bellek bu tür işlemler için ayrılmışsa, önbelleğe alınmış verilerin depolanması için kullanılamaz.

**Maxfragmentationmemory ayrılmış** ayar bellek parçalanması için karşılamak için ayrılmış MB bellek miktarını yapılandırır. Bu değeri ayarlamak, önbellek tam veya yakın olduğunda ve parçalanma oranı yüksek olduğunda daha tutarlı bir Redis sunucu deneyimi yaşamanızı sağlar. Bellek bu tür işlemler için ayrılmışsa, önbelleğe alınmış verilerin depolanması için kullanılamaz.

Yeni bir bellek rezervasyon değeri seçerken göz önünde bulundurulması gereken bir nokta **(maxmemory-ayrılmış** veya **maxfragmentationmemory ayrılmış)** bu değişikliğin zaten büyük miktarda veri ile çalışan bir önbellek etkileyebilir nasıl. Örneğin, 49 GB veri içeren 53 GB önbelleğiniz varsa, rezervasyon değerini 8 GB olarak değiştirirseniz, bu değişiklik sistem için kullanılabilir maksimum belleği 45 GB'a düşürür. Geçerli `used_memory` veya değerleriniz `used_memory_rss` yeni 45 GB sınırından yüksekse, sistem her ikisi `used_memory` de `used_memory_rss` ve 45 GB'ın altında olana kadar verileri tahliye etmek zorunda kalacaktır. Çıkarma sunucu yükünü ve bellek parçalanmasını artırabilir. Önbellek ölçümleri hakkında `used_memory` daha fazla `used_memory_rss`bilgi için kullanılabilir ölçümler [ve raporlama aralıkları](cache-how-to-monitor.md#available-metrics-and-reporting-intervals)gibi bkz.

> [!IMPORTANT]
> **Maksimum bellek ayrılmış** ve **maxfragmentationmemory ayrılmış** ayarları yalnızca Standart ve Premium önbellekler için kullanılabilir.
>
>

#### <a name="keyspace-notifications-advanced-settings"></a>Anahtar alanı bildirimleri (gelişmiş ayarlar)
Redis anahtar alanı bildirimleri **Gelişmiş ayarlar** bıçak üzerinde yapılandırılır. Anahtar alanı bildirimleri, belirli olaylar oluştuğunda istemcilerin bildirim almasına olanak sağlar.

![Redis Gelişmiş Ayarlar için Azure Önbelleği](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> Keyspace bildirimleri ve **bildirim-anahtar alanı olayları** ayarı yalnızca Standart ve Premium önbellekler için kullanılabilir.
>
>

Daha fazla bilgi için [Redis Keyspace Bildirimleri'ne](https://redis.io/topics/notifications)bakın. Örnek kod için [Merhaba dünyası](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örneğindeki [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) dosyasına bakın.


<a name="recommendations"></a>
## <a name="azure-cache-for-redis-advisor"></a>Redis Danışmanı için Azure Önbelleği
Redis Advisor için **Azure Önbelleği,** önbelleğiniz için öneriler görüntüler. Normal işlemler sırasında hiçbir öneri görüntülenmez.

![Öneriler](./media/cache-configure/redis-cache-no-recommendations.png)

Önbelleğinizin işlemleri sırasında yüksek bellek kullanımı, ağ bant genişliği veya sunucu yükü gibi herhangi bir koşul oluşursa, Redis için **Azure Önbelleğinde** bir uyarı görüntülenir.

![Öneriler](./media/cache-configure/redis-cache-recommendations-alert.png)

Daha fazla bilgiyi **Öneriler** bıçağında bulabilirsiniz.

![Öneriler](./media/cache-configure/redis-cache-recommendations.png)

Bu ölçümleri Redis için **Azure Önbelleği'nin** [İzleme grafikleri](cache-how-to-monitor.md#monitoring-charts) ve [Kullanım grafikleri](cache-how-to-monitor.md#usage-charts) bölümlerinde izleyebilirsiniz.

Her fiyatlandırma katmanı istemci bağlantıları, bellek ve bant genişliği için farklı sınırlara sahiptir. Önbelleğiniz bu ölçümler için uzun bir süre boyunca maksimum kapasiteye yaklaşırsa, bir öneri oluşturulur. **Öneriler** aracı tarafından gözden alınan ölçümler ve sınırlar hakkında daha fazla bilgi için aşağıdaki tabloya bakın:

| Redis ölçümü için Azure Önbelleği | Daha fazla bilgi |
| --- | --- |
| Ağ bant genişliği kullanımı |[Önbellek performansı - kullanılabilir bant genişliği](cache-faq.md#cache-performance) |
| Bağlı istemciler |[Varsayılan Redis sunucu yapılandırması - maxclients](#maxclients) |
| Sunucu yükü |[Kullanım grafikleri - Redis Server Load](cache-how-to-monitor.md#usage-charts) |
| Bellek kullanımı |[Önbellek performansı - boyut](cache-faq.md#cache-performance) |

Önbelleğinizi yükseltmek için, fiyatlandırma katmanını değiştirmek ve önbelleğinizi [ölçeklendirmek](#scale) için **şimdi Yükselt'i** tıklatın. Bir fiyatlandırma katmanı seçme hakkında daha fazla bilgi için, [Redis için hangi Azure Önbelleğini ve boyutunu kullanmam gerektiğini](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use) görün?


### <a name="scale"></a>Ölçek
Önbelleğinizin fiyatlandırma katmanını görüntülemek veya değiştirmek için **Ölçek'i** tıklatın. Ölçeklendirme hakkında daha fazla bilgi için [Redis için Azure Önbelleğini niçin ölçeklendireceğini](cache-how-to-scale.md)öğrenin.

![Redis fiyatlandırma katmanı için Azure Önbelleği](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a>Redis Küme Boyutu
**Kümeleme** etkin leştirilmiş çalışan premium önbellek için küme boyutunu değiştirmek için Küme Boyutu'nu tıklatın.

![Küme boyutu](./media/cache-configure/redis-cache-redis-cluster-size.png)

Küme boyutunu değiştirmek için kaydırıcıyı kullanın veya **Shard sayısı** metin kutusuna 1 ile 10 arasında bir sayı yazın ve kaydetmek için **Tamam'ı** tıklatın.

> [!IMPORTANT]
> Redis kümeleme yalnızca Premium önbellekler için kullanılabilir. Daha fazla bilgi için [Redis için Premium Azure Önbelleği için kümeleme yapılandırmanın nasıl yapılacağını](cache-how-to-premium-clustering.md)öğrenin.
>
>


### <a name="redis-data-persistence"></a>Redis veri kalıcılığı
Premium önbelleğiniz için veri kalıcılığını etkinleştirmek, devre dışı kakmak veya yapılandırmak için **Veri kalıcılığını** tıklatın. Redis için Azure Önbelleği, [RDB kalıcılığı](cache-how-to-premium-persistence.md#configure-rdb-persistence) veya [AOF kalıcılığını](cache-how-to-premium-persistence.md#configure-aof-persistence)kullanarak Redis kalıcılığı sunar.

Daha fazla bilgi için [Redis için Premium Azure Önbelleği için kalıcılığı nasıl yapılandıracağınız](cache-how-to-premium-persistence.md)abakın.


> [!IMPORTANT]
> Redis veri kalıcılığı yalnızca Premium önbellekler için kullanılabilir.
>
>

### <a name="schedule-updates"></a>Güncelleştirmeleri zamanlama
**Zamanlama güncelleştirmeleri** bıçağı, önbelleğiniz için Redis sunucu güncelleştirmeleri için bir bakım penceresi belirlemenize olanak tanır.

> [!IMPORTANT]
> Bakım penceresi yalnızca Redis sunucu güncelleştirmeleri için geçerlidir ve önbelleği barındıran VM'lerin işletim sistemindeki Azure güncelleştirmeleri veya güncelleştirmeleri için geçerli değildir.
>
>

![Güncelleştirmeleri zamanlama](./media/cache-configure/redis-schedule-updates.png)

Bakım penceresi belirtmek için, istenilen günleri kontrol edin ve her gün için bakım penceresi başlangıç saatini belirtin ve **Tamam'ı**tıklatın. Bakım penceresi süresi UTC'dedir.

> [!IMPORTANT]
> **Zamanlama güncelleştirmeleri** işlevi yalnızca Premium katman önbellekleri için kullanılabilir. Daha fazla bilgi ve yönergeler [için Redis yönetimi için Azure Önbelleği - Zamanlama güncelleştirmeleri bölümüne](cache-administration.md#schedule-updates)bakın.
>
>

### <a name="geo-replication"></a>Coğrafi çoğaltma

**Coğrafi çoğaltma** bıçağı, Redis örnekleri için iki Premium katmanAzure Önbelleği bağlamak için bir mekanizma sağlar. Bir önbellek birincil bağlı önbellek olarak, diğeri ise ikincil bağlantılı önbellek olarak belirlenir. İkincil bağlantılı önbellek salt okunur olur ve birincil önbelleğe yazılan veriler ikincil bağlantılı önbelleğe kopyalanır. Bu işlev, Azure bölgelerinde bir önbelleği çoğaltmak için kullanılabilir.

> [!IMPORTANT]
> **Coğrafi çoğaltma** yalnızca Premium katman önbellekleri için kullanılabilir. Daha fazla bilgi ve yönergeler [için, Redis için Azure Önbelleği için Coğrafi çoğaltma yı nasıl yapılandıracağınız](cache-how-to-geo-replication.md)abakalım.
>
>

### <a name="virtual-network"></a>Sanal Ağ
**Sanal Ağ** bölümü önbelleğiniz için sanal ağ ayarlarını yapılandırmanızı sağlar. VNET desteği yle premium önbellek oluşturma ve ayarlarını güncelleme hakkında bilgi için [Redis için Premium Azure Önbelleği için Sanal Ağ Desteği'ni nasıl yapılandıracağınız](cache-how-to-premium-vnet.md)abakın.

> [!IMPORTANT]
> Sanal ağ ayarları yalnızca önbellek oluşturma sırasında VNET desteği ile yapılandırılan premium önbellekler için kullanılabilir.
>
>

### <a name="firewall"></a>Güvenlik duvarı

Güvenlik duvarı kuralları yapılandırması, Redis katmanları için tüm Azure Önbelleği için kullanılabilir.

Önbellek için güvenlik duvarı kurallarını görüntülemek ve yapılandırmak için **Güvenlik Duvarı'nı** tıklatın.

![Güvenlik duvarı](./media/cache-configure/redis-firewall-rules.png)

Ip adresi aralığını başlat ve sonla ile güvenlik duvarı kuralları belirtebilirsiniz. Güvenlik duvarı kuralları yapılandırıldığında, yalnızca belirtilen IP adresi aralıklarından istemci bağlantıları önbelleğe bağlanabilir. Bir güvenlik duvarı kuralı kaydedildiğinde, kuralın etkili olması için kısa bir gecikme vardır. Bu gecikme genellikle bir dakikadan azdır.

> [!IMPORTANT]
> Güvenlik duvarı kuralları yapılandırılsa bile, Redis izleme sistemleri için Azure Önbelleğinden bağlantılara her zaman izin verilir.
>
>

### <a name="properties"></a>Özellikler
Önbellek bitiş noktası ve bağlantı noktaları da dahil olmak üzere önbelleğiniz hakkındaki bilgileri görüntülemek için **Özellikler'i** tıklatın.

![Redis Özellikleri için Azure Önbelleği](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a>Kilitler
**Kilitler** bölümü, kuruluşunuzdaki diğer kullanıcıların kritik kaynakları yanlışlıkla siler veya değiştirmesini önlemek için bir aboneliği, kaynak grubunu veya kaynağı kilitlemenize olanak tanır. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/management/lock-resources.md).

### <a name="automation-script"></a>Otomasyon betiği

Gelecekteki dağıtımlar için dağıtılan kaynakların şablonunu oluşturmak ve dışa aktarmak için **Otomasyon komut dosyasını** tıklatın. Şablonlarla çalışma hakkında daha fazla bilgi için Azure [Kaynak Yöneticisi şablonlarıyla kaynakları dağıt'a](../azure-resource-manager/templates/deploy-powershell.md)bakın.

## <a name="administration-settings"></a>Yönetim ayarları
**Yönetim** bölümündeki ayarlar, önbelleğiniz için aşağıdaki yönetim görevlerini gerçekleştirmenize olanak tanır.

![Yönetim](./media/cache-configure/redis-cache-administration.png)

* [Veri içeri aktarma](#importexport)
* [Verileri dışarı aktarma](#importexport)
* [Yeniden başlatma](#reboot)


### <a name="importexport"></a>İçeri/Dışarı Aktarma
Alma/Dışa Aktarma, Redis Veritabanı için bir Azure Önbelleği (RDB) anlık görüntüsünü bir Azure Depolama Hesabı'ndaki bir sayfa örneğine aktarıp dışa aktararak önbellekteki verileri içe aktarmanıza ve dışa aktarmanıza olanak tanıyan bir Azure Önbelleğidir. Alma/Dışa Aktarma, Redis örnekleri için farklı Azure Önbelleği arasında geçiş yapmanızı veya kullanmadan önce önbelleği verilerle doldurmanızı sağlar.

Alma, Linux, Windows veya Amazon Web Hizmetleri ve diğerleri gibi herhangi bir bulut sağlayıcısında çalışan Redis de dahil olmak üzere herhangi bir bulut veya ortamda çalışan herhangi bir Redis sunucusundan Redis uyumlu RDB dosyalarını getirmek için kullanılabilir. Veri alma, önceden doldurulmuş verilerle bir önbellek oluşturmanın kolay bir yoludur. Alma işlemi sırasında, Redis için Azure Önbelleği RDB dosyalarını Azure depolama alanından belleğe yükler ve anahtarları önbelleğe ekler.

Dışa aktarma, Redis için Azure Önbelleğinde depolanan verileri Redis uyumlu RDB dosyalarına dışa aktarmanızı sağlar. Bu özelliği, Redis örneği için bir Azure Önbelleğinden diğerine veya başka bir Redis sunucusuna taşımak için kullanabilirsiniz. Dışa aktarma işlemi sırasında, Redis sunucusu örneği için Azure Önbelleğini barındıran VM'de geçici bir dosya oluşturulur ve dosya belirlenen depolama hesabına yüklenir. Dışa aktarma işlemi başarı veya hata durumuyla tamamlandığında, geçici dosya silinir.

> [!IMPORTANT]
> Alma/Dışa Aktarma yalnızca Premium katman önbellekleri için kullanılabilir. Daha fazla bilgi ve yönergeler [için Redis için Azure Önbelleğindeİçve Dışa Aktar verilerine](cache-how-to-import-export-data.md)bakın.
>
>

### <a name="reboot"></a>Yeniden başlatma
**Yeniden Başlatma** bıçağı önbelleğinizin düğümlerini yeniden başlatmanızı sağlar. Bu yeniden başlatma özelliği, önbellek düğümünde bir hata varsa uygulamanızı esneklik açısından sınamamanızı sağlar.

![Yeniden başlatma](./media/cache-configure/redis-cache-reboot.png)

Kümeleme etkinleştirilmiş bir premium önbelleğiniz varsa, önbelleğin hangi kırıklarının yeniden başlatılacağını seçebilirsiniz.

![Yeniden başlatma](./media/cache-configure/redis-cache-reboot-cluster.png)

Önbelleğinizin bir veya daha fazla düğümünü yeniden başlatmak için, istenen düğümleri seçin ve **Yeniden Başlat'ı**tıklatın. Kümeleme etkin leştirilmiş bir premium önbelleğiniz varsa, yeniden başlatmak için shard(lar)'ı seçin ve ardından **Yeniden Başlat'ı**tıklatın. Birkaç dakika sonra, seçili düğüm yeniden başlatın ve birkaç dakika sonra tekrar çevrimiçi olur.

> [!IMPORTANT]
> Yeniden başlatma artık tüm fiyatlandırma katmanları için kullanılabilir. Daha fazla bilgi ve yönergeler [için Redis yönetimi için Azure Önbelleği - Yeniden Başlat'a](cache-administration.md#reboot)bakın.
>
>


## <a name="monitoring"></a>İzleme

**İzleme** bölümü, Redis için Azure Önbelleğiniz için tanılama ve izleme işlemlerini yapılandırmanıza olanak tanır.
Redis izleme ve tanılama için Azure Önbelleği hakkında daha fazla bilgi için, [Redis için Azure Önbelleğini niçin izleyebilirsiniz.](cache-how-to-monitor.md)

![Tanılama](./media/cache-configure/redis-cache-diagnostics.png)

* [Redis ölçümleri](#redis-metrics)
* [Uyarı kuralları](#alert-rules)
* [Tanılama](#diagnostics)

### <a name="redis-metrics"></a>Redis ölçümleri
Önbelleğinizin [ölçümlerini görüntülemek](cache-how-to-monitor.md#view-cache-metrics) için **Redis ölçümlerini** tıklatın.

### <a name="alert-rules"></a>Uyarı kuralları

Redis ölçümleri için Azure Önbelleği'ni temel alan uyarıları yapılandırmak için **Uyarı kurallarını** tıklatın. Daha fazla bilgi için [Uyarılar'a](cache-how-to-monitor.md#alerts)bakın.

### <a name="diagnostics"></a>Tanılama

Varsayılan olarak, Azure Monitor'daki önbellek ölçümleri [30 gün boyunca saklanır](../azure-monitor/platform/data-platform-metrics.md) ve ardından silinir. Önbellek ölçümlerinizi 30 günden uzun süre devam etmek için, önbellek tanılamalarını depolamak için kullanılan [depolama hesabını yapılandırmak](cache-how-to-monitor.md#export-cache-metrics) için **Tanılama'yı** tıklatın.

>[!NOTE]
>Önbellek ölçümlerinizi depolamaalanına arşivlemenin yanı sıra, [bunları bir Etkinlik hub'ına](../azure-monitor/platform/stream-monitoring-data-event-hubs.md)aktarabilir veya Azure Monitor günlüklerine gönderebilirsiniz.
>
>

## <a name="support--troubleshooting-settings"></a>Sorun giderme ayarlarını & destek
Destek + **sorun giderme** bölümündeki ayarlar, önbelleğinizle ilgili sorunları çözme seçenekleri sunar.

![Destek ve sorun giderme](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [Kaynak durumu](#resource-health)
* [Yeni destek isteği](#new-support-request)

### <a name="resource-health"></a>Kaynak durumu
**Kaynak durumu** kaynağınızı izler ve beklendiği gibi çalışır mı size bildirir. Azure Kaynak sistem durumu hizmeti hakkında daha fazla bilgi için Azure [Kaynak sistem durumu genel bakışı'na](../resource-health/resource-health-overview.md)bakın.

> [!NOTE]
> Kaynak durumu şu anda sanal ağda barındırılan Redis örnekleri için Azure Önbelleğinin durumu hakkında rapor veremiyor. Daha fazla bilgi için [bkz.](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)
>
>

### <a name="new-support-request"></a>Yeni destek isteği
Önbelleğiniz için bir destek isteği açmak için **Yeni destek isteğini** tıklatın.





## <a name="default-redis-server-configuration"></a>Varsayılan Redis sunucu yapılandırması
Redis örnekleri için yeni Azure Önbelleği aşağıdaki varsayılan Redis yapılandırma değerleriyle yapılandırılır:

> [!NOTE]
> Bu bölümdeki ayarlar `StackExchange.Redis.IServer.ConfigSet` yöntem kullanılarak değiştirilemez. Bu yöntem bu bölümdeki komutlardan biriyle çağrılırsa, aşağıdaki örneğe benzer bir özel durum atılır:  
>
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>
> **Maksimum bellek ilkesi**gibi yapılandırılabilir olan tüm değerler, Azure portalı veya Azure CLI veya PowerShell gibi komut satırı yönetim araçları aracılığıyla yapılandırılabilir.
>
>

| Ayar | Varsayılan değer | Açıklama |
| --- | --- | --- |
| `databases` |16 |Varsayılan veritabanları sayısı 16'dır, ancak fiyatlandırma katmanına göre farklı bir sayı yapılandırabilirsiniz. <sup>1</sup> Varsayılan veritabanı DB 0, arasında bir `connection.GetDatabase(dbid)` sayı ve `dbid` bir numara `0` kullanarak bağlantı `databases - 1`başına farklı bir seçebilirsiniz . |
| `maxclients` |Fiyatlandırma katmanı<sup>2</sup> bağlıdır |Bu değer, aynı anda izin verilen en fazla bağlı istemci sayısıdır. Sınıra ulaşıldıktan sonra Redis tüm yeni bağlantıları kapatır ve 'ulaşılan maksimum istemci sayısı' hatasını döndürür. |
| `maxmemory-policy` |`volatile-lru` |Maxmemory ilkesi, Redis'in ne leri kaldıracağını nasıl seçeceğini `maxmemory` (önbelleği oluşturduğunuzda seçtiğiniz önbelleğin boyutu) ayardır. Redis için Azure Önbelleği `volatile-lru`ile varsayılan ayar, lru algoritması kullanarak bir son kullanma kümesi yle anahtarları kaldıran varsayılan ayardır. Bu ayar Azure portalında yapılandırılabilir. Daha fazla bilgi için [Bellek ilkelerine](#memory-policies)bakın. |
| `maxmemory-samples` |3 |Bellek kaydetmek için, LRU ve minimal TTL algoritmaları hassas algoritmalar yerine yaklaşık algoritmalar vardır. Varsayılan olarak Redis üç anahtarı denetler ve son zamanlarda daha az kullanılan ı seçer. |
| `lua-time-limit` |5.000 |Milisaniye cinsinden bir Lua komut dosyasının maksimum yürütme süresi. En yüksek yürütme süresine ulaşılırsa, Redis komut dosyasının izin verilen en yüksek süreden sonra yürütmede olduğunu kaydeder ve sorguları bir hatayla yanıtlamaya başlar. |
| `lua-event-limit` |500 |Komut dosyası olay sırasının maksimum boyutu. |
| `client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub` |0 0 032mb 8mb 60 |İstemci çıktı arabelleği sınırları, sunucudan yeterince hızlı veri okumayan istemcilerin bağlantısını bozmaya zorlamak için kullanılabilir (yaygın bir neden, bir Pub/Alt istemcinin iletileri yayımcının üretebileceği kadar hızlı tüketememesidir). Daha fazla bilgi [https://redis.io/topics/clients](https://redis.io/topics/clients)için bkz. |

<a name="databases"></a>
<sup>1.1.2</sup> Redis `databases` fiyatlandırma katmanı için her Azure Önbelleği için sınır farklıdır ve önbellek oluşturmada ayarlanabilir. Önbellek `databases` oluşturma sırasında ayar belirtilmemişse, varsayılan değer 16'dır.

* Temel ve Standart önbellekler
  * C0 (250 MB) önbellek - en fazla 16 veritabanları
  * C1 (1 GB) önbellek - en fazla 16 veritabanları
  * C2 (2,5 GB) önbellek - en fazla 16 veritabanları
  * C3 (6 GB) önbellek - en fazla 16 veritabanları
  * C4 (13 GB) önbellek - en fazla 32 veritabanları
  * C5 (26 GB) önbellek - en fazla 48 veritabanları
  * C6 (53 GB) önbellek - 64 veritabanına kadar
* Premium önbellekler
  * P1 (6 GB - 60 GB) - en fazla 16 veritabanları
  * P2 (13 GB - 130 GB) - en fazla 32 veritabanları
  * P3 (26 GB - 260 GB) - en fazla 48 veritabanları
  * P4 (53 GB - 530 GB) - 64 veritabanına kadar
  * Redis kümesi etkin olan tüm premium önbellekler - Redis `databases` kümesi yalnızca veritabanı 0 kullanımını destekler, böylece Redis cluster etkinleştirilmiş herhangi bir premium önbellek sınırı etkin bir şekilde 1 olur ve [Select](https://redis.io/commands/select) komutuna izin verilmez. Daha fazla bilgi için bkz. Kümeleme kullanmak [için istemci uygulamamda herhangi bir değişiklik yapmam gerekiyor mu?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)

Veritabanları hakkında daha fazla bilgi için [Redis veritabanları nelerdir?](cache-faq.md#what-are-redis-databases)

> [!NOTE]
> Ayar `databases` yalnızca önbellek oluşturma sırasında ve yalnızca PowerShell, CLI veya diğer yönetim istemcileri kullanılarak yapılandırılabilir. PowerShell kullanarak önbellek oluşturma `databases` sırasında yapılandırma örneği [için](cache-how-to-manage-redis-cache-powershell.md#databases)bkz.
>
>

<a name="maxclients"></a>
<sup>Redis</sup> `maxclients` fiyatlandırma katmanı için her Azure Önbelleği için 2 farklıdır.

* Temel ve Standart önbellekler
  * C0 (250 MB) önbellek - 256 bağlantıya kadar
  * C1 (1 GB) önbellek - 1.000'e kadar bağlantı
  * C2 (2,5 GB) önbellek - 2.000'e kadar bağlantı
  * C3 (6 GB) önbellek - 5.000'e kadar bağlantı
  * C4 (13 GB) önbellek - 10.000'e kadar bağlantı
  * C5 (26 GB) önbellek - 15.000'e kadar bağlantı
  * C6 (53 GB) önbellek - 20.000'e kadar bağlantı
* Premium önbellekler
  * P1 (6 GB - 60 GB) - 7.500 bağlantıya kadar
  * P2 (13 GB - 130 GB) - 15.000 bağlantıya kadar
  * P3 (26 GB - 260 GB) - 30.000 bağlantıya kadar
  * P4 (53 GB - 530 GB) - 40.000 bağlantıya kadar

> [!NOTE]
> Önbelleğin her boyutu belirli sayıda *bağlantıya* izin verirken, Redis'e yapılan her bağlantının yükü yle ilişkilidir. Bu tür ek yükün bir örneği, TLS/SSL şifrelemesi sonucunda CPU ve bellek kullanımı olacaktır. Belirli bir önbellek boyutu için maksimum bağlantı sınırı, hafif yüklü bir önbellek varsayar. Bağlantı yükünden gelen yük *artı* istemci işlemlerinden gelen yük sistem kapasitesini aşıyorsa, geçerli önbellek boyutu için bağlantı sınırını aşmamış olsanız bile önbellek kapasite sorunlarıyla karşılaşabilir.
>
>



## <a name="redis-commands-not-supported-in-azure-cache-for-redis"></a>Redis komutları Redis için Azure Önbelleğinde desteklenmiyor
> [!IMPORTANT]
> Redis örnekleri için Azure Önbelleği yapılandırması ve yönetimi Microsoft tarafından yönetildiği için, aşağıdaki komutlar devre dışı bırakılır. Bunları çağırmaya çalışırsanız, `"(error) ERR unknown command"`''ye benzer bir hata iletisi alırsınız.
>
> * BGREWRITEAOF
> * BGSAVE
> * Config
> * HATA AYIKLAMA
> * Geçirme
> * KAYDET
> * Kapatma
> * KÖLE
> * CLUSTER - Cluster yazma komutları devre dışı bırakılır, ancak salt okunur Cluster komutlarına izin verilir.
>
>

Redis komutları hakkında daha [https://redis.io/commands](https://redis.io/commands)fazla bilgi için bkz.

## <a name="redis-console"></a>Redis konsolu
Tüm önbellek katmanları için Azure portalında bulunan **Redis Konsolu'nu**kullanarak Redis örnekleri için Azure Önbelleği'nize güvenli komutlar verebilirsiniz.

> [!IMPORTANT]
> - Redis [KonsolVNET](cache-how-to-premium-vnet.md)ile çalışmıyor. Önbelleğiniz Bir VNET'in parçası olduğunda, önbelleğe yalnızca VNET'teki istemciler erişebilir. Redis Console, VNET'in dışındaki yerel tarayıcınızda çalıştığından önbelleğinize bağlanamıyor.
> - Tüm Redis komutları Redis için Azure Önbelleğinde desteklenmez. Redis için Azure Önbelleği için devre dışı bırakılan Redis komutlarının listesi [için, Redis için Azure Önbelleği bölümünde desteklenmeyen önceki Redis komutlarına](#redis-commands-not-supported-in-azure-cache-for-redis) bakın. Redis komutları hakkında daha [https://redis.io/commands](https://redis.io/commands)fazla bilgi için bkz.
>
>

Redis Konsoluna erişmek için **Redis için Azure Önbelleğinden** **Konsol'u** tıklatın.

![Redis konsolu](./media/cache-configure/redis-console-menu.png)

Önbellek örneğinize karşı komutlar vermek için konsola istediğiniz komutu yazın.

![Redis konsolu](./media/cache-configure/redis-console.png)


### <a name="using-the-redis-console-with-a-premium-clustered-cache"></a>Redis Konsolu'nu birinci sınıf kümelenmiş önbellekle kullanma

Redis Console'u birinci sınıf kümelenmiş önbellekle kullanırken, önbelleğin tek bir parçasına komutlar verebilirsiniz. Belirli bir parçaya komut vermek için, önce parça toplayıcıya tıklayarak istediğiniz parçaya bağlanın.

![Redis konsolu](./media/cache-configure/redis-console-premium-cluster.png)

Bağlı parçadan farklı bir parçada depolanan bir anahtara erişmeye çalışırsanız, aşağıdaki iletiye benzer bir hata iletisi alırsınız:

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

Önceki örnekte, shard 1 seçili parçadır, ancak `myKey` hata iletisinin `(shard 0)` bölümüyle gösterildiği gibi parça 0'da bulunur. Bu örnekte, `myKey`erişmek için, shard picker kullanarak shard 0 seçin ve sonra istenen komutu sorun.


## <a name="move-your-cache-to-a-new-subscription"></a>Önbelleğinizi yeni bir aboneye taşıma
Önbelleğinizi **Taşı'yı**tıklatarak yeni bir aboneye taşıyabilirsiniz.

![Redis için Azure Önbelleğini Taşı](./media/cache-configure/redis-cache-move.png)

Kaynakları bir kaynak grubundan diğerine ve bir abonelikten diğerine taşıma hakkında bilgi [için](../azure-resource-manager/management/move-resource-group-and-subscription.md)bkz.

## <a name="next-steps"></a>Sonraki adımlar
* Redis komutlarıyla çalışma hakkında daha fazla bilgi için [bkz.](cache-faq.md#how-can-i-run-redis-commands)
