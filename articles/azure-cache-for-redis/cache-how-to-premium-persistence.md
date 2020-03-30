---
title: Veri kalıcılığını yapılandırma - Redis için Premium Azure Önbelleği
description: Redis örnekleri için Premium katman Azure Önbelleğinizi nasıl yapılandıracağınızı ve verileri nasıl yöneteceğinizi öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 08/24/2017
ms.openlocfilehash: 84a5b4784a36fb22ae50a7a1ec4fcb7e5ef5b7c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80245285"
---
# <a name="how-to-configure-data-persistence-for-a-premium-azure-cache-for-redis"></a>Redis için Premium Azure Önbelleği için veri kalıcılığı nasıl yapılandırılır?
Redis için Azure Önbellek, kümeleme, kalıcılık ve sanal ağ desteği gibi Premium katman özellikleri de dahil olmak üzere önbellek boyutu ve özellikleri seçiminde esneklik sağlayan farklı önbellek tekliflerine sahiptir. Bu makalede, Redis örneğinde premium Azure Önbelleğinde kalıcılığın nasıl yapılandırılabildiği açıklanmaktadır.

Diğer premium önbellek özellikleri hakkında daha fazla bilgi için [redis premium katmanı için Azure Önbelleğine Giriş'e](cache-premium-tier-intro.md)bakın.

## <a name="what-is-data-persistence"></a>Veri kalıcılığı nedir?
[Redis kalıcılığı,](https://redis.io/topics/persistence) Redis'te depolanan verileri devam ettiremenizi sağlar. Ayrıca anlık görüntüleri alabilir ve bir donanım hatası durumunda yükleyebilirsiniz verileri yedeklemek. Bu, tüm verilerin bellekte depolandığı ve Önbellek düğümlerinin düştüğü bir hata durumunda olası veri kaybı olabileceği Temel veya Standart katmana göre büyük bir avantajdır. 

Redis için Azure Önbellek, aşağıdaki modelleri kullanarak Redis kalıcılığı sunar:

* **RDB kalıcılığı** - RDB (Redis veritabanı) kalıcılığı yapılandırıldığında, Redis için Azure Önbelleği, Redis için Azure Önbelleğinin bir anlık görüntüsünü, yapılandırılabilir yedekleme frekansına dayalı olarak diske Redis ikili biçiminde saklar. Hem birincil hem de yineleme önbelleğini devre dışı eden bir felaket oluşursa, önbellek en son anlık görüntü kullanılarak yeniden oluşturulur. RDB kalıcılığının [avantajları](https://redis.io/topics/persistence#rdb-advantages) ve [dezavantajları](https://redis.io/topics/persistence#rdb-disadvantages) hakkında daha fazla bilgi edinin.
* **AOF kalıcılığı** - AOF (Yalnızca dosyayı ek) kalıcılığı yapılandırıldığında, Redis için Azure Önbellek her yazma işlemini saniyede en az bir kez kaydedilmiş bir günlük lüğe kaydeder. Hem birincil hem de yineleme önbelleğini devre dışı eden bir felaket oluşursa, önbellek depolanan yazma işlemleri kullanılarak yeniden oluşturulur. AOF kalıcılığının [avantajları](https://redis.io/topics/persistence#aof-advantages) ve [dezavantajları](https://redis.io/topics/persistence#aof-disadvantages) hakkında daha fazla bilgi edinin.

Persistence, Redis verilerini sahip olduğunuz ve yönettiğiniz bir Azure Depolama hesabına yazar. Önbellek oluşturma sırasında **Redis için Yeni Azure Önbelleğinden** ve varolan premium önbellekler için **Kaynak menüsünden** yapılandırabilirsiniz.

> [!NOTE]
> 
> Azure Depolama, kalıcı olduğunda verileri otomatik olarak şifreler. Şifreleme için kendi anahtarlarınızı kullanabilirsiniz. Daha fazla bilgi için [Azure Key Vault ile Müşteri tarafından yönetilen anahtarlara](/azure/storage/common/storage-service-encryption)bakın.
> 
> 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Bir premium fiyatlandırma katmanı seçildikten sonra **Redis kalıcılığını**tıklatın.

![Redis sebat][redis-cache-persistence]

Sonraki bölümdeki adımlar, Redis kalıcılığının yeni premium önbelleğinizde nasıl yapılandırılabildiğini açıklar. Redis kalıcılığı yapılandırıldıktan sonra, Redis kalıcılığıyla yeni premium önbelleğinizi oluşturmak için **Oluştur'u** tıklatın.

## <a name="enable-redis-persistence"></a>Redis'in kalıcılığını etkinleştirme

Redis **kalıcılığı, RDB** veya **AOF** kalıcılığı seçilerek **Veri kalıcılığı** kılıcında etkinleştirilir. Yeni önbellekler için, önceki bölümde açıklandığı gibi önbellek oluşturma işlemi sırasında bu bıçak erişilir. Varolan önbellekler için, Önbelleğiniz için **Kaynak menüsünden** **Veri kalıcılığı** bıçağına erişilir.

![Redis ayarları][redis-cache-settings]


## <a name="configure-rdb-persistence"></a>RDB kalıcılığını yapılandırma

RDB kalıcılığını etkinleştirmek için **RDB'yi**tıklatın. RdB kalıcılığını daha önce etkinleştirilmiş bir premium önbellekte devre dışı kalmak için **Devre Dışı'** yı tıklatın.

![Redis RDB kalıcılığı][redis-cache-rdb-persistence]

Yedekleme aralığını yapılandırmak için açılır listeden **bir Yedekleme Frekansı** seçin. Seçenekler **15 Dakika,** **30 dakika,** **60 dakika,** **6 saat,** **12 saat**ve **24 saat**içerir. Bu aralık, önceki yedekleme işlemi başarıyla tamamlandıktan ve yeni bir yedekleme başlatıldıktan sonra geri saymaya başlar.

Kullanılacak depolama hesabını seçmek için **Depolama Hesabı'nı** tıklatın ve **Depolama Anahtarı** açılır açılır tarafından kullanılacak **Birincil anahtar** veya **İkincil anahtarı** seçin. Önbellekle aynı bölgede bir depolama hesabı seçmeniz gerekir ve premium depolama alanı daha yüksek iş girdisi olduğundan **Premium Depolama** hesabı önerilir. 

> [!IMPORTANT]
> Kalıcılık hesabınızın depolama anahtarı yeniden oluşturulursa, **Depolama Anahtarı** açılır tesliminden istenen anahtarı yeniden yapılandırmanız gerekir.
> 
> 

Kalıcılık yapılandırmasını kaydetmek için **Tamam'ı** tıklatın.

Yedekleme frekans aralığı geçtikten sonra bir sonraki yedekleme (veya yeni önbellekler için ilk yedekleme) başlatılır.

## <a name="configure-aof-persistence"></a>AOF kalıcılığını yapılandırma

AOF kalıcılığını etkinleştirmek için **AOF'yi**tıklatın. AOF kalıcılığını daha önce etkinleştirilmiş bir premium önbellekte devre dışı kalmak için **Devre Dışı'** yı tıklatın.

![Redis AOF kalıcılığı][redis-cache-aof-persistence]

AOF kalıcılığını yapılandırmak için bir **İlk Depolama Hesabı**belirtin. Bu depolama hesabı önbellekle aynı bölgede olmalıdır ve premium depolama nın daha yüksek iş girdisi olduğundan **Premium Depolama** hesabı önerilir. İsteğe bağlı olarak İkinci Depolama **Hesabı**adlı ek bir depolama hesabı yapılandırabilirsiniz. İkinci bir depolama hesabı yapılandırılırsa, yineleme önbelleğine yazılanlar bu ikinci depolama hesabına yazılır. Yapılandırılan her depolama hesabı için **Depolama Anahtarı** açılır tesliminden kullanılacak **Birincil anahtar** veya **İkincil anahtarı** seçin. 

> [!IMPORTANT]
> Kalıcılık hesabınızın depolama anahtarı yeniden oluşturulursa, **Depolama Anahtarı** açılır tesliminden istenen anahtarı yeniden yapılandırmanız gerekir.
> 
> 

AOF kalıcılığı etkinleştirildiğinde, önbelleğe yazma işlemleri belirlenen depolama hesabına (veya ikinci bir depolama hesabı yapılandırmışsanız hesaplara) kaydedilir. Hem birincil hem de çoğaltma önbelleğini kapatan bir felaket hatası durumunda, depolanan AOF günlüğü önbelleği yeniden oluşturmak için kullanılır.

## <a name="persistence-faq"></a>Kalıcılık SSS
Aşağıdaki liste, Redis kalıcılığı için Azure Önbelleği hakkında sık sorulan soruların yanıtlarını içerir.

* [Daha önce oluşturulmuş bir önbellekte kalıcılığı etkinleştirebilir miyim?](#can-i-enable-persistence-on-a-previously-created-cache)
* [AOF ve RDB kalıcılığını aynı anda etkinleştirebilir miyim?](#can-i-enable-aof-and-rdb-persistence-at-the-same-time)
* [Hangi kalıcımodeli seçmeliyim?](#which-persistence-model-should-i-choose)
* [Ölçeklendirme işleminden önce yapılan bir yedekleme farklı boyutlara ölçeklenirse ve yedekleme geri yüklenirse ne olur?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)


### <a name="rdb-persistence"></a>RDB kalıcılığı
* [Önbelleği oluşturduktan sonra RDB yedekleme sıklığını değiştirebilir miyim?](#can-i-change-the-rdb-backup-frequency-after-i-create-the-cache)
* [RdB yedekleme frekansım 60 dakikaysa yedeklemeler arasında 60 dakikadan fazla neden olur?](#why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
* [Yeni bir yedekleme yapıldığında eski RDB yedeklemelerine ne olur?](#what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made)

### <a name="aof-persistence"></a>AOF kalıcılığı
* [İkinci bir depolama hesabı ne zaman kullanmalıyım?](#when-should-i-use-a-second-storage-account)
* [AOF kalıcılığı, gecikme veya önbelleğin performansını etkiler mi?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)
* [İkinci depolama hesabını nasıl kaldırabilirim?](#how-can-i-remove-the-second-storage-account)
* [Yeniden yazma nedir ve önbelleğimi nasıl etkiler?](#what-is-a-rewrite-and-how-does-it-affect-my-cache)
* [AOF etkinken önbelleği ölçeklerken ne beklemem gerekir?](#what-should-i-expect-when-scaling-a-cache-with-aof-enabled)
* [AOF verilerim depolamada nasıl düzenlenir?](#how-is-my-aof-data-organized-in-storage)


### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Daha önce oluşturulmuş bir önbellekte kalıcılığı etkinleştirebilir miyim?
Evet, Redis kalıcılığı hem önbellek oluşturma da hem de varolan premium önbelleklerde yapılandırılabilir.

### <a name="can-i-enable-aof-and-rdb-persistence-at-the-same-time"></a>AOF ve RDB kalıcılığını aynı anda etkinleştirebilir miyim?

Hayır, yalnızca RDB veya AOF'yi etkinleştirebilirsiniz, ancak her ikisini de aynı anda etkinleştiremezsiniz.

### <a name="which-persistence-model-should-i-choose"></a>Hangi kalıcımodeli seçmeliyim?

AOF kalıcılığı, her yazıyı, yapılandırılan yedekleme aralığına göre yedekleri en az performans etkisiyle kaydeden RDB kalıcılığıyla karşılaştırıldığında, iş çıktısı üzerinde bazı etkileri olan bir günlük için kaydeder. Birincil hedefiniz veri kaybını en aza indirmekse AOF kalıcılığını seçin ve önbelleğiniz için iş hacminde bir düşüş işleyebilirsiniz. Önbelleğinizde en iyi iş verisini korumak, ancak yine de veri kurtarma için bir mekanizma istiyorsanız RDB kalıcılığını seçin.

* RDB kalıcılığının [avantajları](https://redis.io/topics/persistence#rdb-advantages) ve [dezavantajları](https://redis.io/topics/persistence#rdb-disadvantages) hakkında daha fazla bilgi edinin.
* AOF kalıcılığının [avantajları](https://redis.io/topics/persistence#aof-advantages) ve [dezavantajları](https://redis.io/topics/persistence#aof-disadvantages) hakkında daha fazla bilgi edinin.

AOF kalıcılığı kullanırken performans hakkında daha fazla bilgi için [bkz.](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Ölçeklendirme işleminden önce yapılan bir yedekleme farklı boyutlara ölçeklenirse ve yedekleme geri yüklenirse ne olur?

Hem RDB hem de AOF kalıcılığı için:

* Daha büyük bir boyuta ölçeklendirildiyseniz, etkisi yoktur.
* Daha küçük bir boyuta ölçeklendiyseniz ve yeni boyutunuz için [veritabanları sınırından](cache-configure.md#databases) daha büyük özel bir [veritabanları](cache-configure.md#databases) ayarınız varsa, bu veritabanlarındaki veriler geri yüklenmez. Daha fazla bilgi için bkz: [Özel veritabanlarım ölçekleme sırasında etkileniyor mu?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
* Daha küçük bir boyuta ölçeklendiyseniz ve son yedeklemedeki tüm verileri tutmak için daha küçük boyutta yeterli alan yoksa, anahtarlar genellikle [allkeys-lru](https://redis.io/topics/lru-cache) çıkarma ilkesi kullanılarak geri yükleme işlemi sırasında tahliye edilecektir.

### <a name="can-i-change-the-rdb-backup-frequency-after-i-create-the-cache"></a>Önbelleği oluşturduktan sonra RDB yedekleme sıklığını değiştirebilir miyim?
Evet, **Veri kalıcılığı** bıçağındaki RDB kalıcılığı için yedekleme sıklığını değiştirebilirsiniz. Yönergeler için redis kalıcılığını yapılandır' a bakın.

### <a name="why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>RdB yedekleme frekansım 60 dakikaysa yedeklemeler arasında 60 dakikadan fazla neden olur?
RDB kalıcılık yedekleme frekans aralığı, önceki yedekleme işlemi başarıyla tamamlanana kadar başlamaz. Yedekleme sıklığı 60 dakikaysa ve yedekleme işleminin başarıyla tamamlanması 15 dakika sürüyorsa, bir sonraki yedekleme önceki yedeklemenin başlangıç saatinden 75 dakika sonrasına kadar başlamaz.

### <a name="what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made"></a>Yeni bir yedekleme yapıldığında eski RDB yedeklemelerine ne olur?
En son yedekleme hariç tüm RDB kalıcılık yedekleri otomatik olarak silinir. Bu silme işlemi hemen gerçekleşmeyebilir, ancak eski yedeklemeler süresiz olarak kalıcı değildir.


### <a name="when-should-i-use-a-second-storage-account"></a>İkinci bir depolama hesabı ne zaman kullanmalıyım?

Önbellekte beklenenden daha yüksek ayarlanmış işlemlere sahip olduğunuzu düşünüyorsanız, AOF kalıcılığı için ikinci bir depolama hesabı kullanmalısınız.  İkincil depolama hesabının ayarlanması, önbelleğinizin depolama bant genişliği sınırlarına ulaşmamasını sağlamaya yardımcı olur.

### <a name="does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache"></a>AOF kalıcılığı, gecikme veya önbelleğin performansını etkiler mi?

AOF kalıcılığı, önbellek maksimum yükün altında olduğunda (CPU ve Sunucu Yükü %90'ın altında) yaklaşık %15 – %20 oranında verimi etkiler. Önbellek bu sınırlar içindeyken gecikme sorunları olmamalıdır. Ancak, önbellek AOF etkin ile bu sınırlara daha erken ulaşacaktır.

### <a name="how-can-i-remove-the-second-storage-account"></a>İkinci depolama hesabını nasıl kaldırabilirim?

İkinci depolama hesabını ilk depolama hesabıyla aynı olacak şekilde ayarlayarak AOF kalıcılık ikincil depolama hesabını kaldırabilirsiniz. Yönergeler için Bkz. [AOF kalıcılığını yapılandırın.](#configure-aof-persistence)

### <a name="what-is-a-rewrite-and-how-does-it-affect-my-cache"></a>Yeniden yazma nedir ve önbelleğimi nasıl etkiler?

AOF dosyası yeterince büyük olduğunda, önbellekte otomatik olarak yeniden yazma sıralanır. Yeniden yazma, AOF dosyasını geçerli veri kümesini oluşturmak için gereken en az işlem kümesiyle yeniden boyutlandırıyor. Yeniden yazma lar sırasında, özellikle büyük veri kümeleriyle uğraşırken performans sınırlarına daha erken ulaşmayı bekleyin. AOF dosyası büyüdükçe yeniden yazmalar daha az sıklıkta gerçekleşir, ancak gerçekleştiğinde önemli miktarda zaman alır.

### <a name="what-should-i-expect-when-scaling-a-cache-with-aof-enabled"></a>AOF etkinken önbelleği ölçeklerken ne beklemem gerekir?

Ölçeklendirme sırasındaki AOF dosyası önemli ölçüde büyükse, ölçeklendirme bittikten sonra dosyayı yeniden yükleyeceği için ölçeklendirme işleminin beklenenden daha uzun sürmesini bekleyin.

Ölçekleme hakkında daha fazla bilgi için bkz: [Farklı bir boyuta ölçeklendirildim ve ölçeklendirme işleminden önce yapılan bir yedekleme geri yüklenirse ne olur?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="how-is-my-aof-data-organized-in-storage"></a>AOF verilerim depolamada nasıl düzenlenir?

AOF dosyalarında depolanan veriler, verileri depolamaya kaydetme performansını artırmak için düğüm başına birden çok sayfa blob'una bölünür. Aşağıdaki tablo, her fiyatlandırma katmanı için kaç sayfa blob'unun kullanıldığını görüntüler:

| Premium katman | Bloblar |
|--------------|-------|
| P1           | Parça başına 4    |
| P2           | Parça başına 8    |
| P3           | Parça başına 16   |
| P4           | Parça başına 20   |

Kümeleme etkinleştirildiğinde, önbellekteki her parçanın önceki tabloda belirtildiği gibi kendi sayfa lekeleri kümesi vardır. Örneğin, üç parçadan oluşan bir P2 önbelleği AOF dosyasını 24 sayfa blobs (parça başına 8 blobs, 3 kırıkile) dağıtır.

Yeniden yazdıktan sonra, depolama alanında iki AOF dosyası kümesi bulunur. Yeniden yazmalar arka planda oluşur ve ilk dosya kümesine eklenirken, ikinci kümeye yeniden yazma ekinde önbelleğe gönderilen işlemleri ayarlayın. Bir yedekleme, hata durumunda yeniden yazma sırasında geçici olarak depolanır, ancak yeniden yazma bittikten sonra hemen silinir.


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliğini nasıl kullanacağınızı öğrenin.

* [Redis Premium katmanı için Azure Önbelleğine Giriş](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-rdb-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-rdb-persistence.png

[redis-cache-aof-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-aof-persistence.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
