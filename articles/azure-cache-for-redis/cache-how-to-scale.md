---
title: Redis için Azure Önbelleği Nasıl Ölçeklendirilir?
description: Azure portalını ve Azure PowerShell ve Azure CLI gibi araçları kullanarak Azure Önbelleğinizi Redis için nasıl ölçeklendireceğinizi öğrenin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 04/11/2017
ms.openlocfilehash: 68c668561123aee943f54e6fdcbad7c6450957f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278005"
---
# <a name="how-to-scale-azure-cache-for-redis"></a>Redis için Azure Önbelleği Nasıl Ölçeklendirilir?
Redis için Azure Önbelleği, önbellek boyutu ve özellikleri seçiminde esneklik sağlayan farklı önbellek tekliflerine sahiptir. Önbellek oluşturulduktan sonra, uygulamanızın gereksinimleri değişirse önbelleğin boyutunu ve fiyatlandırma katmanını ölçeklendirebilirsiniz. Bu makalede, Azure portalını ve Azure PowerShell ve Azure CLI gibi araçları kullanarak önbelleğinizi nasıl ölçeklendireceğiniz gösterilmektedir.

## <a name="when-to-scale"></a>Ne zaman ölçeklendirme yapılmalıdır?
Önbelleğinizin durumunu ve performansını izlemek ve önbelleğin ne zaman ölçeklendireceğini belirlemeye yardımcı olmak için Redis için Azure Önbelleği'nin [izleme](cache-how-to-monitor.md) özelliklerini kullanabilirsiniz. 

Ölçeklendirmeniz gerekip gerekmeden gerek iplenmediğinizi belirlemeye yardımcı olmak için aşağıdaki ölçümleri izleyebilirsiniz.

* Redis Sunucu Yükü
* Bellek Kullanımı
* Ağ Bant Genişliği
* CPU Kullanımı

Önbelleğinizin artık uygulamanızın gereksinimlerini karşıladığını belirlerseniz, uygulamanız için doğru olan daha büyük veya daha küçük bir önbellek fiyatlandırma katmanına ölçeklendirebilirsiniz. Hangi önbellek fiyatlandırma katmanının kullanılacağını belirleme hakkında daha fazla bilgi için, [Redis için hangi Azure Önbelleğini ve boyutunu kullanmam gerektiğini](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)görün.

## <a name="scale-a-cache"></a>Önbelleği ölçeklendir
Önbelleğinizi ölçeklendirmek için [Azure portalındaki](https://portal.azure.com) [önbelleğe göz atın](cache-configure.md#configure-azure-cache-for-redis-settings) ve Kaynak **menüsünden** **Ölçekle'yi** tıklatın.

![Ölçek](./media/cache-how-to-scale/redis-cache-scale-menu.png)

**Select fiyatlandırma katmanı** bıçağından istenilen fiyatlandırma katmanını seçin ve **Seç'i**tıklatın.

![Fiyatlandırma katmanı][redis-cache-pricing-tier-blade]


Aşağıdaki kısıtlamalarla farklı bir fiyatlandırma katmanına ölçeklendirebilirsiniz:

* Daha yüksek bir fiyatlandırma katmanından daha düşük bir fiyatlandırma katmanına ölçeklendiremezsiniz.
  * **Premium** önbellekten **Standarda** veya **Temel** önbelleğe ölçeklendiremezsiniz.
  * **Standart** önbellekten **Temel** önbelleğe ölçeklendiremezsiniz.
* **Temel** önbellekten **Standart** önbelleğe ölçeklendirebilirsiniz, ancak boyutu aynı anda değiştiremezsiniz. Farklı bir boyuta ihtiyacınız varsa, sonraki ölçekleme işlemini istediğiniz boyutta yapabilirsiniz.
* **Temel** önbellekten doğrudan **Premium** önbelleğine ölçeklendiremezsiniz. İlk olarak, bir ölçeklendirme işleminde **Temel'den** **Standard'a,** ardından bir ölçeklendirme işleminde **Standart'tan** **Premium'a** ölçeklendirin.
* Daha büyük bir boyuttan **C0 (250 MB)** boyutuna kadar ölçeklendiremezsiniz.
 
Önbellek yeni fiyatlandırma katmanına ölçeklenirken, Redis kılıcı **için Azure Önbelleğinde** **ölçeklendirme** durumu görüntülenir.

![Ölçeklendirme][redis-cache-scaling]

Ölçekleme tamamlandığında, durum **Ölçekleme'den** **Çalıştırma'ya**dönüşür.

## <a name="how-to-automate-a-scaling-operation"></a>Ölçekleme işlemi otomatikleştirin
Önbellek örneklerinizi Azure portalında ölçeklendirmenin yanı sıra PowerShell cmdlets, Azure CLI ve Microsoft Azure Yönetim Kitaplıkları 'nı (MAML) kullanarak ölçeklendirebilirsiniz. 

* [PowerShell kullanarak ölçeklendirin](#scale-using-powershell)
* [Azure CLI'yi kullanarak ölçeklendirme](#scale-using-azure-cli)
* [MAML kullanarak ölçeklendirin](#scale-using-maml)

### <a name="scale-using-powershell"></a>PowerShell kullanarak ölçeklendirin

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

The [Set-AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/set-azrediscache) cmdlet'i kullanarak, `Size`"veya `Sku` `ShardCount` özellikler değiştirildiğinde, Redis örnekleri için Azure Önbelleğinizi PowerShell ile ölçeklendirebilirsiniz. Aşağıdaki örnek, 2,5 GB `myCache` önbelleğe adlandırılmış bir önbelleğe nasıl ölçeklendirilir gösterilmektedir. 

    Set-AzRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

PowerShell ile ölçeklendirme hakkında daha fazla bilgi için bkz. [Powershell kullanarak Redis için Bir Azure Önbelleğini ölçeklendirmek için](cache-how-to-manage-redis-cache-powershell.md#scale)bkz.

### <a name="scale-using-azure-cli"></a>Azure CLI'yi kullanarak ölçeklendirme
Azure CLI kullanarak Redis örnekleri için Azure Önbelleğinizi ölçeklendirmek için, istenen ölçeklendirme işlemine bağlı olarak yeni bir boyut, sku veya küme boyutu içeren istenen yapılandırma değişikliklerinde `azure rediscache set` komutu ve geçişi arayın.

Azure CLI ile ölçekleme hakkında daha fazla bilgi için, [Redis için varolan bir Azure Önbelleğinin ayarlarını değiştir'e](cache-manage-cli.md#scale)bakın.

### <a name="scale-using-maml"></a>MAML kullanarak ölçeklendirin
[Microsoft Azure Yönetim Kitaplıkları 'nı (MAML)](https://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/)kullanarak Redis örnekleri `IRedisOperations.CreateOrUpdate` için Azure Önbelleğinizi `RedisProperties.SKU.Capacity`ölçeklendirmek için, yöntemi arayın ve yeni boyutta geçirin.

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

Daha fazla bilgi için MAML örneğini [kullanarak Redis için Azure Önbelleğini Yönet'e](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) bakın.

## <a name="scaling-faq"></a>Ölçekleme SSS
Aşağıdaki liste, Redis ölçekleme için Azure Önbelleği hakkında sık sorulan soruların yanıtlarını içerir.

* [Premium önbelleğe ölçeklendirebilir miyim, oradan veya premium önbellek içinde mi?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Ölçekledikten sonra önbellek adımı veya erişim anahtarlarımı değiştirmem gerekiyor mu?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Ölçekleme nasıl çalışır?](#how-does-scaling-work)
* [Ölçekleme sırasında önbelleğimdeki verileri kaybeder miyim?](#will-i-lose-data-from-my-cache-during-scaling)
* [Özel veritabanlarım ölçekleme sırasında etkileniyor mu?](#is-my-custom-databases-setting-affected-during-scaling)
* [Önbelleğe alma sırasında kullanılabilir mi?](#will-my-cache-be-available-during-scaling)
* Coğrafi çoğaltma yapılandırıldığında, önbelleğimi ölçeklendirememez veya kümedeki kırıkları değiştiremem?
* [Desteklenmeyen işlemler](#operations-that-are-not-supported)
* [Ölçekleme ne kadar sürer?](#how-long-does-scaling-take)
* [Ölçeklemenin ne zaman tamamolduğunu nasıl anlayabilirim?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>Premium önbelleğe ölçeklendirebilir miyim, oradan veya premium önbellek içinde mi?
* **Premium** önbellekten **Temel** veya **Standart** fiyatlandırma katmanına ölçeklendiremezsiniz.
* Bir **Premium** önbellek fiyatlandırma katmanından diğerine ölçeklendirebilirsiniz.
* **Temel** önbellekten doğrudan **Premium** önbelleğine ölçeklendiremezsiniz. İlk olarak, bir ölçeklendirme işleminde **Temel'den** **Standard'a,** ardından bir ölçeklendirme işleminde **Standart'tan** **Premium'a** ölçeklendirin.
* **Premium** önbelleğinizi oluşturduğunuzda kümeleme özelliğini etkinleştirdiyseniz, [küme boyutunu değiştirebilirsiniz.](cache-how-to-premium-clustering.md#cluster-size) Önbelleğiniz kümeleme etkin olmadan oluşturulduysa, kümeleme yi daha sonra yapılandırabilirsiniz.
  
  Daha fazla bilgi için [Redis için Premium Azure Önbelleği için kümeleme yapılandırmanın nasıl yapılacağını](cache-how-to-premium-clustering.md)öğrenin.

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a>Ölçekledikten sonra önbellek adımı veya erişim anahtarlarımı değiştirmem gerekiyor mu?
Hayır, ölçekleme işlemi sırasında önbellek adınız ve anahtarlarınız değişmez.

### <a name="how-does-scaling-work"></a>Ölçekleme nasıl çalışır?
* **Temel** önbellek farklı bir boyuta ölçeklendirildiğinde, kapatılır ve yeni boyut kullanılarak yeni bir önbellek sağlanır. Bu süre zarfında önbellek kullanılamaz ve önbellekteki tüm veriler kaybolur.
* **Temel** önbellek **Standart** önbelleğe ölçeklendirildiğinde, yineleme önbelleği karşılanır ve veriler birincil önbellekten yineleme önbelleğine kopyalanır. Önbellek ölçekleme işlemi sırasında kullanılabilir kalır.
* **Standart** önbellek farklı bir boyuta veya **Premium** önbelleğe ölçeklendirildiğinde, yinelemelerden biri kapatılır ve yeni boyuta yeniden verilir ve veriler aktarılır ve diğer yineleme yeniden sağatılmadan önce, önbellek düğümlerinden birinin başarısızlığı sırasında gerçekleşen işleme benzer şekilde bir hata gerçekleştirir.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>Ölçekleme sırasında önbelleğimdeki verileri kaybeder miyim?
* **Temel** önbellek yeni bir boyuta ölçeklendirildiğinde, tüm veriler kaybolur ve ölçeklendirme işlemi sırasında önbellek kullanılamaz.
* **Temel** önbellek **Standart** önbelleğe ölçeklendirildiğinde, önbellekteki veriler genellikle korunur.
* **Standart** önbellek daha büyük bir boyuta veya katmana ölçeklendirildiğinde veya **Premium** önbellek daha büyük bir boyuta ölçeklendirildiğinde, tüm veriler genellikle korunur. **Bir Standart** veya **Premium** önbelleği daha küçük bir boyuta ölçeklendirirken, ölçeklendirildiğinde yeni boyutla ilgili önbellekte ne kadar veri olduğuna bağlı olarak veriler kaybolabilir. Ölçekleme yaparken veriler kaybolursa, [anahtarlar allkeys-lru](https://redis.io/topics/lru-cache) tahliye ilkesi kullanılarak tahliye edilir. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>Özel veritabanlarım ölçekleme sırasında etkileniyor mu?
Önbellek oluşturma sırasında `databases` ayar için özel bir değer yapılandırıldıysanız, bazı fiyatlandırma katmanlarının farklı veritabanları [sınırları](cache-configure.md#databases)olduğunu unutmayın. Bu senaryoda ölçekleme yaparken göz önünde bulundurulması gereken bazı noktalar şunlardır:

* Geçerli katmandan daha düşük `databases` bir sınıra sahip bir fiyatlandırma katmanına ölçekleme yaparken:
  * Tüm fiyatlandırma katmanları için `databases`16 olan varsayılan sayıyı kullanıyorsanız, veri kaybolmaz.
  * Ölçeklediğiniz katman için `databases` sınırlara denk gelen özel bir sayı kullanıyorsanız, `databases` bu ayar korunur ve hiçbir veri kaybolmaz.
  * Yeni katman sınırlarını aşan `databases` özel bir sayı kullanıyorsanız, `databases` ayar yeni katmanın sınırlarına düşürülir ve kaldırılan veritabanlarındaki tüm veriler kaybolur.
* Geçerli katmandan aynı veya daha yüksek `databases` sınıra sahip bir `databases` fiyatlandırma katmanına ölçeklenirken, ayarınız korunur ve hiçbir veri kaybolmaz.

Standart ve Premium önbellekler kullanılabilirlik için %99,9 SLA'ya sahip olsa da, veri kaybı için SLA yoktur.

### <a name="will-my-cache-be-available-during-scaling"></a>Önbelleğe alma sırasında kullanılabilir mi?
* **Ölçekleme** işlemi sırasında standart ve **Premium** önbellekler kullanılabilir durumda kalır. Ancak, Standart ve Premium önbellekleri ölçekleme sırasında ve temelden Standart önbelleklere ölçekleme yaparken bağlantı blipsoluşabilir. Bu bağlantı blips küçük olması bekleniyor ve redis istemcileri anında kendi bağlantı yeniden kurmak gerekir.
* **Temel** önbellekler, ölçekleme işlemleri sırasında farklı bir boyuta çevrimdışı dır. Temel önbellekler **Temel'den** **Standard'a** ölçekleme yaparken kullanılabilir kalır, ancak küçük bir bağlantı blip'i yaşayabilir. Bir bağlantı blip oluşursa, redis istemcileri anında bağlantılarını yeniden kurabilmelidir.


### <a name="scaling-limitations-with-geo-replication"></a>Coğrafi çoğaltma ile ölçekleme sınırlamaları

İki önbellek arasına bir Coğrafi çoğaltma bağlantısı ekledikten sonra, artık bir ölçekleme işlemi başlatamaz veya kümedeki parça sayısını değiştiremezsiniz. Bu komutları vermek için önbelleği çözmeniz gerekir. Daha fazla bilgi için [bkz.](cache-how-to-geo-replication.md)


### <a name="operations-that-are-not-supported"></a>Desteklenmeyen işlemler
* Daha yüksek bir fiyatlandırma katmanından daha düşük bir fiyatlandırma katmanına ölçeklendiremezsiniz.
  * **Premium** önbellekten **Standarda** veya **Temel** önbelleğe ölçeklendiremezsiniz.
  * **Standart** önbellekten **Temel** önbelleğe ölçeklendiremezsiniz.
* **Temel** önbellekten **Standart** önbelleğe ölçeklendirebilirsiniz, ancak boyutu aynı anda değiştiremezsiniz. Farklı bir boyuta ihtiyacınız varsa, sonraki ölçekleme işlemini istediğiniz boyutta yapabilirsiniz.
* **Temel** önbellekten doğrudan **Premium** önbelleğine ölçeklendiremezsiniz. İlk olarak bir ölçeklendirme işleminde **Temel'den** **Standard'a** ölçeklendirin, ardından bir sonraki operasyonda **Standart'tan** **Premium'a** ölçeklendirin.
* Daha büyük bir boyuttan **C0 (250 MB)** boyutuna kadar ölçeklendiremezsiniz.

Ölçekleme işlemi başarısız olursa, hizmet işlemi geri almaya çalışır ve önbellek özgün boyuta geri döner.


### <a name="how-long-does-scaling-take"></a>Ölçekleme ne kadar sürer?
Ölçekleme süresi önbellekte ne kadar veri olduğuna bağlıdır ve daha büyük miktarda verinin tamamlanması daha uzun sürer. Ölçekleme yaklaşık 20 dakika sürer. Kümelenmiş önbellekler için ölçekleme parça başına yaklaşık 20 dakika sürer.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Ölçeklemenin ne zaman tamamolduğunu nasıl anlayabilirim?
Azure portalında ölçekleme işleminin devam ettiğini görebilirsiniz. Ölçekleme tamamlandığında önbelleğin durumu **Çalıştır'a**dönüşür.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png
