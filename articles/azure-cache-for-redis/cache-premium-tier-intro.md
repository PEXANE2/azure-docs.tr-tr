---
title: Redis Premium katmanı için Azure Önbelleğine Giriş
description: Redis örnekleri için Premium katman Azure Önbelleği için Redis Kalıcılığı, Redis kümeleme ve VNET desteğini nasıl oluşturup yöneteceğinizi öğrenin
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 07/05/2017
ms.author: yegu
ms.openlocfilehash: aadcc13d2397f10ea40f06d1259c86b9a179c38b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74121660"
---
# <a name="introduction-to-the-azure-cache-for-redis-premium-tier"></a>Redis Premium katmanı için Azure Önbelleğine Giriş
Redis için Azure Önbelleği, verilerinize süper hızlı erişim sağlayarak yüksek ölçeklenebilir ve yanıt veren uygulamalar oluşturmanıza yardımcı olan dağıtılmış, yönetilen bir önbellektir. 

Yeni Premium katmanı, daha iyi performans, daha büyük iş yükleri, olağanüstü durum kurtarma, alma/dışa aktarma ve gelişmiş güvenlik gibi tüm Standart katman özelliklerini ve daha fazlasını içeren bir Enterprise hazır katmanıdır. Premium önbellek katmanının ek özellikleri hakkında daha fazla bilgi edinmek için okumaya devam edin.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Standart veya Temel katmana göre daha iyi performans
**Standart veya Temel katmana göre daha iyi performans.** Premium katmandaki önbellekler, daha hızlı işlemcilere sahip olan ve Temel veya Standart Katman'a göre daha iyi performans sağlayan donanımda dağıtılır. Premium katman önbellekleri daha yüksek iş sahibi ve daha düşük gecikme süreleri vardır. 

**Aynı boyuttaki Önbellek için üretim, Premium'da Standart katmana göre daha yüksektir.** Örneğin, 53 GB P4 (Premium) önbelleğinin iş kısmı, C6 için 150K (Standart) ile karşılaştırıldığında saniyede 250K istektir.

Boyut, iş ortası ve birinci sınıf önbelleklerle bant genişliği hakkında daha fazla bilgi için [Redis SSS için Azure Önbelleği'ne](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use) bakın

## <a name="redis-data-persistence"></a>Redis veri kalıcılığı
Premium katmanı, önbellek verilerini bir Azure Depolama hesabında devam etmenizi sağlar. Temel/Standart önbelleğinde tüm veriler yalnızca bellekte depolanır. Altta yatan altyapı sorunları söz konusu olduğunda potansiyel veri kaybı olabilir. Veri kaybına karşı dayanıklılığı artırmak için Premium katmanındaki Redis veri kalıcılığı özelliğini kullanmanızı öneririz. Redis için Azure Önbellek RdB ve AOF (yakında) Seçenekleri [Redis kalıcılık](https://redis.io/topics/persistence)sunuyor. 

Kalıcılığı yapılandırma yönergeleri [için, Redis için Premium Azure Önbelleği için kalıcılığı nasıl yapılandıracağınız](cache-how-to-premium-persistence.md)abakın.

## <a name="redis-cluster"></a>Redis kümesi
53 GB'dan büyük önbellekler oluşturmak veya birden çok Redis düğümünde veri parçalamak istiyorsanız, Premium katmanında bulunan Redis kümelemasını kullanabilirsiniz. Her düğüm, yüksek kullanılabilirlik için Azure tarafından yönetilen bir birincil/yineleme önbellek çiftinden oluşur. 

**Redis kümeleme maksimum ölçek ve verim verir.** Kümedeki parça (düğüm) sayısını artırdıkça üretim doğrusal olarak artar. Örn. 10 parçadan oluşan bir P4 kümesi oluşturursanız, kullanılabilir iş parçacığı saniyede 250K *10 = 2,5 Milyon istek olur. Boyut, iş ortası ve bant genişliği hakkında daha fazla bilgi için lütfen [Redis SSS için Azure](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use) Önbelleği'ne bakın.

Kümeleme ile başlamak [için Redis için Premium Azure Önbelleği için kümeleme yapılandırmayı nasıl yapılandıracağınız](cache-how-to-premium-clustering.md)ı görün.

## <a name="enhanced-security-and-isolation"></a>Gelişmiş güvenlik ve yalıtım
Temel veya Standart katmanda oluşturulan önbellekler herkese açık internet üzerinden erişilebilir. Önbellek erişimi erişim anahtarına bağlı olarak kısıtlanır. Premium katmanı ile önbelleğe yalnızca belirli bir ağdaki istemcilerin Önbelleğe erişebilmesini sağlayabilirsiniz. Redis için Azure Önbelleğini bir [Azure Sanal Ağı'nda (VNet)](https://azure.microsoft.com/services/virtual-network/)dağıtabilirsiniz. Redis’e erişimi daha da fazla kısıtlamak için alt ağlar, erişim denetimi, ilkeler gibi VNet’in tüm özelliklerini ve diğer özellikleri kullanabilirsiniz.

Daha fazla bilgi için [Redis için Premium Azure Önbelleği için Sanal Ağ desteğini nasıl yapılandıracağınız](cache-how-to-premium-vnet.md)abakın.

## <a name="importexport"></a>İçeri/Dışarı Aktarma
Alma/Dışa Aktarma, Redis için Verileri Azure Önbelleğine aktarmanıza veya Redis için Azure Önbelleğinden veri dışa aktarmanıza olanak tanıyan bir Azure Önbelleğidir, bir Azure Depolama Hesabı'ndaki bir sayfa kalıbından Redis Veritabanı için Bir Azure Önbelleği (RDB) anlık görüntüsünü içe aktarıp dışa aktarabilirsiniz. Bu, Redis örnekleri için farklı Azure Önbelleği arasında geçiş yapmanızı veya kullanmadan önce önbelleği verilerle doldurmanızı sağlar.

Alma, Linux, Windows veya Amazon Web Hizmetleri ve diğerleri gibi herhangi bir bulut sağlayıcısında çalışan Redis de dahil olmak üzere herhangi bir bulut veya ortamda çalışan herhangi bir Redis sunucusundan Redis uyumlu RDB dosya(ları) getirmek için kullanılabilir. Veri alma, önceden doldurulmuş verilerle bir önbellek oluşturmanın kolay bir yoludur. Alma işlemi sırasında, Redis için Azure Önbelleği RDB dosyalarını Azure depolama alanından belleğe yükler ve anahtarları önbelleğe ekler.

Dışa aktarma, Redis için Azure Önbelleğinde depolanan verileri Redis uyumlu RDB dosyasına(lar) dışa aktarmanızı sağlar. Bu özelliği, Redis örneği için bir Azure Önbelleğinden diğerine veya başka bir Redis sunucusuna taşımak için kullanabilirsiniz. Dışa aktarma işlemi sırasında, Redis sunucusu örneği için Azure Önbelleğini barındıran VM'de geçici bir dosya oluşturulur ve dosya belirlenen depolama hesabına yüklenir. Dışa aktarma işlemi başarı veya hata durumuyla tamamlandığında, geçici dosya silinir.

Daha fazla bilgi için, [Redis için Azure Önbelleğinden verileri nasıl içe aktarAcağınız ve dışa aktarmayı](cache-how-to-import-export-data.md)öğrenin.

## <a name="reboot"></a>Yeniden başlatma
Premium katman, önbelleğinizin isteğe bağlı bir veya daha fazla düğümlerini yeniden başlatmanızı sağlar. Bu, bir hata durumunda uygulamanızı esneklik açısından test etmenizi sağlar. Aşağıdaki düğümleri yeniden başlatabilirsiniz.

* Önbelleğinizin ana düğümü
* Önbelleğinizin ikincil düğümü
* Önbelleğinizin hem birincil hem de ikincil düğümleri
* Kümeleme ile premium önbellek kullanırken, önbellekteki tek tek kırıklar için birincil, ikincil veya her iki düğümü yeniden başlatabilirsiniz

Daha fazla bilgi için [Reboot FAQ](cache-administration.md#reboot-faq) [bkz.](cache-administration.md#reboot)

>[!NOTE]
>Redis katmanları için tüm Azure Önbelleği işlevleri artık etkinleştirildi.
>
>

## <a name="schedule-updates"></a>Güncelleştirmeleri zamanlama
Zamanlanmış güncelleştirmeler özelliği önbelleğiniz için bir bakım penceresi belirlemenize olanak tanır. Bakım penceresi belirtildiğinde, bu pencere sırasında redis sunucu güncelleştirmeleri yapılır. Bakım penceresi belirlemek için istediğiniz günleri seçin ve her gün için bakım penceresi başlangıç saatini belirtin. Bakım penceresi süresinin UTC'de olduğunu unutmayın. 

Daha fazla bilgi için zamanlama [güncelleştirmeleri](cache-administration.md#schedule-updates) ve [Zamanlama güncelleştirmeleri SSS'ye](cache-administration.md#schedule-updates-faq)bakın.

> [!NOTE]
> Yalnızca Redis sunucu güncelleştirmeleri zamanlanan bakım penceresi sırasında yapılır. Bakım penceresi Azure güncelleştirmeleri veya VM işletim sistemindeki güncelleştirmeler için geçerli değildir.
> 
> 

## <a name="geo-replication"></a>Coğrafi çoğaltma

**Coğrafi çoğaltma,** Redis örnekleri için iki Premium katmanAzure Önbelleği bağlamak için bir mekanizma sağlar. Bir önbellek birincil bağlı önbellek olarak, diğeri ise ikincil bağlantılı önbellek olarak belirlenir. İkincil bağlantılı önbellek salt okunur olur ve birincil önbelleğe yazılan veriler ikincil bağlantılı önbelleğe kopyalanır. Bu işlev, Azure bölgelerinde bir önbelleği çoğaltmak için kullanılabilir.

Daha fazla bilgi için [Redis için Azure Önbelleği için Coğrafi çoğaltma yı nasıl yapılandıracağınız](cache-how-to-geo-replication.md)abakın.


## <a name="to-scale-to-the-premium-tier"></a>Premium katmana ölçeklendirmek için
Premium katmana ölçeklendirmek için, **Değiştir fiyatlandırma katmanı** bıçağındaki premium katmanlardan birini seçmeniz yeterlidir. Önbelleğinizi PowerShell ve CLI'yi kullanarak premium katmana ölçeklendirebilirsiniz. Adım adım yönergeler [için, Redis için Azure Önbelleği Nasıl Ölçeklendirilir](cache-how-to-scale.md) ve [ölçeklendirme işlemini otomatikleştirin.](cache-how-to-scale.md#how-to-automate-a-scaling-operation)

## <a name="next-steps"></a>Sonraki adımlar
Bir önbellek oluşturun ve yeni premium katman özelliklerini keşfedin.

* [Redis için Premium Azure Önbelleği için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md)
* [Redis için Premium Azure Önbelleği için Sanal Ağ desteği nasıl yapılandırılır?](cache-how-to-premium-vnet.md)
* [Redis için Premium Azure Önbelleği için kümeleme yapılandırma](cache-how-to-premium-clustering.md)
* [Redis için Azure Önbelleğinden veri alma ve dışa aktarma](cache-how-to-import-export-data.md)
* [Redis için Azure Önbelleği nasıl uygulanır?](cache-administration.md)

