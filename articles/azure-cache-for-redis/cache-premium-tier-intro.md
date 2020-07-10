---
title: Redsıs Premium katmanı için Azure önbelleğine giriş
description: Redsıs kalıcılığı oluşturma ve yönetme hakkında bilgi edinin ve redsıs örnekleri için Premium katmanınız için Azure önbelleğiniz için sanal ağ desteği
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 07/05/2017
ms.author: yegu
ms.openlocfilehash: 15b4764d32c536698246bddfcca50ffa6ce9b3b5
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86184693"
---
# <a name="introduction-to-the-azure-cache-for-redis-premium-tier"></a>Redsıs Premium katmanı için Azure önbelleğine giriş
Redsıs için Azure önbelleği, verilerinize Çok hızlı erişme olanağı sunarak yüksek düzeyde ölçeklenebilir ve hızlı uygulamalar oluşturmanıza yardımcı olan dağıtılmış, yönetilen bir önbelleğidir. 

Yeni Premium katmanı, daha iyi performans, daha büyük iş yükleri, olağanüstü durum kurtarma, içeri/dışarı aktarma ve gelişmiş güvenlik gibi tüm standart katman özelliklerini ve daha fazlasını içeren bir kurumsal özellikli katmandır. Premium önbellek katmanının ek özellikleri hakkında daha fazla bilgi edinmek için okumaya devam edin.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Standart veya temel katmana kıyasla daha iyi performans
**Standart veya temel katman üzerinde daha iyi performans.** Premium katmandaki önbellekler, daha hızlı işlemciler içeren ve temel veya Standart katmana kıyasla daha iyi performans sağlayan donanımlar üzerinde dağıtılır. Premium katman önbelleklerine daha yüksek aktarım hızı ve düşük gecikme süreleri vardır. 

**Aynı boyutlu önbellek için üretilen iş, Standart katmana kıyasla Premium 'da daha yüksektir.** Örneğin, 53 GB P4 (Premium) önbelleğinin üretilen iş hacmi, C6 (Standart) için 150K ile karşılaştırıldığında saniyede 250 KB istektir.

Premium önbellekler ile boyut, aktarım hızı ve bant genişliği hakkında daha fazla bilgi için bkz. [redsıs Için Azure ÖNBELLEĞI SSS](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis veri kalıcılığı
Premium katmanı, önbellek verilerini bir Azure depolama hesabında kalıcı hale bırakmanıza olanak tanır. Temel veya standart önbellek tüm verileri yalnızca bellekte depolar. Temel altyapı sorunları söz konusu olduğunda, olası veri kaybı olabilir. Veri kaybına karşı dayanıklılığı artırmak için Premium katmanda Redsıs veri kalıcılığı özelliğini kullanmanızı öneririz. Redsıs için Azure önbelleği, [redsıs kalıcılığı](https://redis.io/topics/persistence)içindeki RDB ve AOF (çok yakında) seçenekleri sunar. 

Kalıcılığı yapılandırma hakkında yönergeler için bkz. [redsıs Için Premium Azure önbelleği için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md).

## <a name="redis-cluster"></a>Redis kümesi
53 GB 'den büyük önbellekler oluşturmak veya birden çok redin düğümüne veri eklemek istiyorsanız Premium katmanda bulunan reddo kümeleme kullanabilirsiniz. Her düğüm, yüksek kullanılabilirlik için Azure tarafından yönetilen birincil/çoğaltma önbellek çiftinin oluşur. 

**Redsıs Kümelemesi, size maksimum ölçek ve aktarım hızı sağlar.** Küme içindeki parça sayısını (düğümler) artırdıkça üretilen iş miktarı artar. Örneğin, 10 parçalı bir P4 kümesi oluşturursanız, kullanılabilir üretilen iş miktarı 250K * 10 = 2.500.000 istek/saniye olur. Premium önbellekler ile boyut, aktarım hızı ve bant genişliği hakkında daha fazla ayrıntı için bkz. [redsıs Için Azure önbelleği](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use) .

Kümeleme ile çalışmaya başlamak için bkz. [redsıs Için Premium Azure önbelleği için kümeleme yapılandırma](cache-how-to-premium-clustering.md).

## <a name="enhanced-security-and-isolation"></a>Gelişmiş güvenlik ve yalıtım
Temel veya Standart katmanda oluşturulan önbelleklere genel İnternet üzerinden erişilebilir. Önbelleğe erişim, erişim anahtarına göre kısıtlıdır. Premium katmanıyla birlikte, yalnızca belirtilen bir ağ içindeki istemcilerin önbelleğe erişebildiğinden emin olabilirsiniz. Redsıs için Azure önbellek 'yi bir [Azure sanal ağı 'nda (VNet)](https://azure.microsoft.com/services/virtual-network/)dağıtabilirsiniz. Redis’e erişimi daha da fazla kısıtlamak için alt ağlar, erişim denetimi, ilkeler gibi VNet’in tüm özelliklerini ve diğer özellikleri kullanabilirsiniz.

Daha fazla bilgi için bkz. [bir Premium Azure önbelleği için bkz. redsıs Için sanal ağ desteğini yapılandırma](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>İçeri/Dışarı Aktarma
İçeri/dışarı aktarma, redsıs veritabanı (RDB) anlık görüntüsüne yönelik bir Azure önbelleğini bir Azure Storage hesabındaki bir sayfa blobuna içeri aktarıp dışarı aktararak redsıs için Azure Cache 'e veri aktarmanıza veya redsıs için Azure önbelleğinden veri aktarmaya olanak tanıyan Redsıs veri yönetimi işlemi için bir Azure önbelleğidir. Bu, Redsıs örnekleri için farklı Azure önbelleği arasında geçiş yapmanızı veya kullanılmadan önce verileri kullanarak önbelleğin doldurulmasını sağlar.

İçeri aktarma, Linux, Windows veya Amazon Web Services ve diğer bulut sağlayıcıları üzerinde çalışan bir bulut veya ortamda çalışan redsıs uyumlu RDB dosyalarını getirmek için kullanılabilir. Verilerin içeri aktarılması, önceden doldurulmuş verilerle bir önbellek oluşturmanın kolay bir yoludur. İçeri aktarma işlemi sırasında Redsıs için Azure önbelleği, RDB dosyalarını Azure depolama alanından belleğe yükler ve ardından anahtarları önbelleğe ekler.

Dışarı aktarma, redin için Azure önbelleğinde depolanan verileri Redsıs uyumlu RDB dosyaları 'na dışarı aktarıp verbırakmanıza olanak tanır. Bu özelliği, Redsıs örneği için bir Azure önbelleğinden diğerine veya başka bir Redsıs sunucusuna veri taşımak için kullanabilirsiniz. Dışarı aktarma işlemi sırasında, Redsıs sunucu örneği için Azure önbelleğini barındıran VM 'de geçici bir dosya oluşturulur ve dosya belirtilen depolama hesabına yüklenir. Dışarı aktarma işlemi başarı veya başarısızlık durumu ile tamamlandığında geçici dosya silinir.

Daha fazla bilgi için bkz. [redin Için Azure önbelleğinden veri aktarma ve verileri dışarı aktarma](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Yeniden başlatma
Premium katmanı, önbelleğinizin bir veya daha fazla düğümünü isteğe bağlı olarak yeniden başlatmanızı sağlar. Bu, hata durumunda uygulamanızı dayanıklılık açısından test etmenizi sağlar. Aşağıdaki düğümleri yeniden başlatabilirsiniz.

* Önbelleğinizin birincil düğümü
* Önbelleğinizin çoğaltma düğümü
* Önbelleğinizin birincil ve çoğaltma düğümleri
* Kümeleme ile Premium önbellek kullanırken, önbellekteki bireysel parçalar için birincil, çoğaltma veya her iki düğümü yeniden başlatabilirsiniz

Daha fazla bilgi için bkz. [yeniden başlatma](cache-administration.md#reboot) ve [yeniden başlatma SSS](cache-administration.md#reboot-faq).

>[!NOTE]
>Önyükleme işlevselliği artık Redsıs katmanları için tüm Azure önbelleğinde etkinleştirilmiştir.
>
>

## <a name="schedule-updates"></a>Güncelleştirmeleri zamanlama
Zamanlanan Güncelleştirmeler özelliği önbelleğiniz için bir bakım penceresi atamanıza olanak tanır. Bakım penceresi belirtildiğinde, bu pencere sırasında tüm Red, Server güncelleştirmeleri yapılır. Bir bakım penceresi belirlemek için, istenen günleri seçin ve her gün için bakım penceresi başlangıç saatini belirtin. Bakım penceresi saati UTC 'dir. 

Daha fazla bilgi için bkz. [güncelleştirmeleri zamanlama](cache-administration.md#schedule-updates) ve [güncelleştirmeleri zamanlama SSS](cache-administration.md#schedule-updates-faq).

> [!NOTE]
> Zamanlanan bakım penceresi sırasında yalnızca Redsıs sunucu güncelleştirmeleri yapılır. Bakım penceresi, Azure güncelleştirmeleri veya VM işletim sistemi güncelleştirmeleri için geçerlidir.
> 
> 

## <a name="geo-replication"></a>Coğrafi çoğaltma

**Coğrafi çoğaltma** , redsıs örnekleri Için iki Premium katman Azure önbelleğinin bağlanmasına yönelik bir mekanizma sağlar. Bir önbellek, birincil bağlı önbellek ve diğeri ikincil bağlantılı önbellek olarak atanır. İkincil bağlantılı önbellek salt okunurdur ve birincil önbelleğe yazılan veriler ikincil bağlantılı önbelleğe çoğaltılır. Bu işlev, Azure bölgeleri arasında bir önbelleği çoğaltmak için kullanılabilir.

Daha fazla bilgi için bkz. [redsıs Için Azure önbelleği Için Coğrafi çoğaltmayı yapılandırma](cache-how-to-geo-replication.md).


## <a name="to-scale-to-the-premium-tier"></a>Premium katmana ölçeklendirmek için
Premium katmana ölçeklemek için, **fiyatlandırma katmanını Değiştir** dikey penceresinde Premium katmanlardan birini seçin. Ayrıca, PowerShell ve CLı kullanarak önbelleğinizi Premium katmana ölçeklendirebilirsiniz. Adım adım yönergeler için bkz. [Azure önbelleğini redne ölçeklendirme](cache-how-to-scale.md) ve [bir ölçeklendirme işlemini otomatikleştirme](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Sonraki adımlar
Bir önbellek oluşturun ve yeni Premium katman özelliklerini araştırın.

* [Redsıs için Premium Azure önbelleği için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md)
* [Redsıs için Premium Azure önbelleği için sanal ağ desteğini yapılandırma](cache-how-to-premium-vnet.md)
* [Redsıs için Premium Azure önbelleği için kümeleme yapılandırma](cache-how-to-premium-clustering.md)
* [Redsıs için Azure önbelleğinden veri aktarma ve verileri dışarı aktarma](cache-how-to-import-export-data.md)
* [Redsıs için Azure önbelleğini yönetme](cache-administration.md)

