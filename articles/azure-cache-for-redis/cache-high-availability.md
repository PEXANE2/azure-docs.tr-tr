---
title: Redsıs için Azure önbelleği için yüksek kullanılabilirlik
description: Redsıs yüksek kullanılabilirlik özellikleri ve seçenekleri için Azure önbelleği hakkında bilgi edinin
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 02/08/2021
ms.author: yegu
ms.openlocfilehash: d9c8f5dd8b2647756087ce6f36ff3a25b2aaaadc
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100387980"
---
# <a name="high-availability-for-azure-cache-for-redis"></a>Redsıs için Azure önbelleği için yüksek kullanılabilirlik

Redin için Azure önbelleğinde yerleşik yüksek kullanılabilirlik vardır. Yüksek kullanılabilirlik mimarisinin hedefi, temel alınan sanal makineler (VM 'Ler) planlı veya plansız kesintilerden etkilendiğinde bile, yönetilen Redis örneğinizin çalışır durumda olduğundan emin olmaktır. Tek bir VM 'de redin barındırılmasına göre daha fazla yüzde oranı sunar.

Redin için Azure Cache, bir önbellek için *düğüm* olarak adlandırılan birden çok VM kullanarak yüksek kullanılabilirlik uygular. Bu düğümleri, veri çoğaltma ve yük devretme ile eşgüdümlü bir şekilde gerçekleşecek şekilde yapılandırır. Redsıs yazılım düzeltme eki uygulama gibi bakım işlemlerini de düzenler. Standart, Premium ve Enterprise katmanlarında çeşitli yüksek kullanılabilirlik seçenekleri mevcuttur:

| Seçenek | Açıklama | Kullanılabilirlik | Standart | Premium | Kurumsal |
| ------------------- | ------- | ------- | :------: | :---: | :---: |
| [Standart çoğaltma](#standard-replication)| Otomatik yük devretme ile tek bir veri merkezinde çift düğümlü çoğaltılan yapılandırma | %99,9 |✔|✔|-|
| [Bölge yedekliliği](#zone-redundancy) | Otomatik yük devretme ile AZs genelinde çok düğümlü çoğaltılan yapılandırma | % 99,95 (Premium katman),% 99,99 (kurumsal Katmanlar) |-|Önizleme|Önizleme|
| [Coğrafi çoğaltma](#geo-replication) | Kullanıcı denetimli yük devretmeyle, iki bölgedeki bağlantılı önbellek örnekleri | % 99,9 (Premium katmanı, tek bölge) |-|✔|-|

## <a name="standard-replication"></a>Standart çoğaltma

Standart veya Premium katmanda redin için bir Azure önbelleği, varsayılan olarak bir dizi Redsıs sunucusu üzerinde çalışır. İki sunucu adanmış VM 'lerde barındırılır. Açık kaynaklı redo, veri yazma isteklerini yalnızca bir sunucunun işlemesini sağlar. Bu sunucu, diğer *çoğaltma* sırasında *birincil* düğümdür. Sunucu düğümlerini sağlamasını gerçekleştirdikten sonra Redsıs için Azure önbelleği, bunlara birincil ve çoğaltma rolleri atar. Birincil düğüm, genellikle yazma hizmetinin yanı sıra Redsıs istemcilerinden gelen okuma isteklerini de sorumludur. Bir yazma işleminde, iç belleğine yeni bir anahtar ve bir anahtar güncelleştirmesi kaydeder ve istemciye anında yanıt verir. İşlemi zaman uyumsuz olarak çoğaltmaya iletir.

:::image type="content" source="media/cache-high-availability/replication.png" alt-text="Veri çoğaltma Kurulumu":::
   
>[!NOTE]
>Normalde, redin istemcisi tüm okuma ve yazma istekleri için bir Redsıs önbelleğinde birincil düğümle iletişim kurar. Belirli Reddir istemcileri, çoğaltma düğümünden okunacak şekilde yapılandırılabilir.
>
>

Redsıs önbelleğinde birincil düğüm kullanılamıyorsa, çoğaltma kendisini otomatik olarak yeni birincil olacak şekilde yükseltir. Bu işleme *Yük devretme* adı verilir. Çoğaltma, birincil düğümün hızlı bir şekilde kurtarmasının önüne alınmadan önce yeterince uzun süre bekleyecektir. Yük devretme Işlemi gerçekleştiğinde, redin için Azure önbelleği yeni bir VM sağlar ve bunu çoğaltma düğümü olarak önbelleğe birleştirir. Çoğaltma, birincil ile tam veri eşitlemesi gerçekleştirerek önbellek verilerinin başka bir kopyasına sahip olur.

Bir birincil düğüm, Redsıs yazılımı veya işletim sistemi güncelleştirmesi gibi planlı bir bakım etkinliğinin bir parçası olarak hizmetten geçebilir. Ayrıca, temeldeki donanımda, yazılımda veya ağda oluşan hatalarda oluşan planlanmamış olaylar nedeniyle çalışmayı durdurabilir. [Redin Için Azure önbelleği Için yük devretme ve düzeltme eki uygulama](cache-failover.md) , Redsıs yük devretmeleri türleri hakkında ayrıntılı bir açıklama sağlar. Redo için bir Azure önbelleği, ömrü boyunca birçok yük devretmenize devam edecektir. Yüksek kullanılabilirlik mimarisi, bu değişiklikleri bir önbellekte mümkün olduğunca saydam hale getirmek için tasarlanmıştır.

>[!NOTE]
>Aşağıdaki önizleme olarak kullanılabilir.
>
>

Ayrıca, redde için Azure önbelleği Premium katmanda ek çoğaltma düğümlerine izin verir. [Çoklu çoğaltma önbelleği](cache-how-to-multi-replicas.md) , en çok üç çoğaltma düğümü ile yapılandırılabilir. Daha fazla çoğaltmanın olması, birincil olarak yedeklenen ek düğümler nedeniyle genellikle dayanıklılığı geliştirir. Daha fazla çoğaltmalarla birlikte, Redsıs örneği için Azure önbelleği yine de veri merkezi veya AZ geniş bir kesinti tarafından ciddi bir şekilde etkilenebilir. Birden çok kopyayı, [bölge artıklarıyla](#zone-redundancy)birlikte kullanarak önbellek kullanılabilirliğini artırabilirsiniz.

## <a name="zone-redundancy"></a>Bölge yedekliliği

Redsıs için Azure önbelleği, Premium ve Enterprise katmanlarında bölgesel olarak yedekli yapılandırmaların kullanılmasını destekler. Bölgesel olarak [yedekli önbellek](cache-how-to-zone-redundancy.md) , düğümlerini aynı bölgedeki farklı [Azure kullanılabilirlik alanları](../availability-zones/az-overview.md) arasında yerleştirebilir. Tek bir hata noktası olarak veri merkezini veya AZ kesintiyi ortadan kaldırır ve önbelleğinizin genel kullanılabilirliğini artırır.

### <a name="premium-tier"></a>Premium katmanı

>[!NOTE]
>Bu, önizleme olarak kullanılabilir.
>
>

Aşağıdaki diyagramda Premium katmanının bölgesel olarak yedekli yapılandırması gösterilmektedir:

:::image type="content" source="media/cache-high-availability/zone-redundancy.png" alt-text="Bölge artıklığı kurulumu":::
   
Reds için Azure Cache, bir bölgenin yedekli önbelleğindeki düğümleri seçtiğiniz Azlara göre hepsini bir kez kez dağıtır. Başlangıçta birincil olarak hangi düğümün çalışacağını de belirler.

Bölge yedekli önbelleği otomatik yük devretme sağlar. Geçerli birincil düğüm kullanılamadığında, çoğaltmalardan biri üzerinden alınır. Yeni birincil düğüm farklı bir AZ ise uygulamanız daha yüksek önbellek yanıt süresi ile karşılaşabilir. Azlar coğrafi olarak ayrılır. Bir AZ diğerine geçiş yapmak, uygulamanızın ve önbelleğinin barındırıldığı konum arasındaki fiziksel mesafeyi değiştirir. Bu değişiklik, uygulamadan önbelleğe gidiş dönüş ağ gecikmelerinin etkilerini etkiler. Ek gecikme süresinin çoğu uygulama için kabul edilebilir bir Aralık içinde olması beklenir. Uygulamanızı, bölgesel olarak yedekli bir önbellekte iyi gerçekleştirebileceği şekilde test etmenizi öneririz.

### <a name="enterprise-and-enterprise-flash-tiers"></a>Enterprise ve Enterprise Flash katmanları

Bir açık kurumsal katmandaki bir önbellek Redsıs kurumsal kümesi üzerinde çalışır. Her zaman çekirdek oluşturmak için tek sayıda sunucu düğümü gerektirir. Varsayılan olarak, her biri ayrılmış bir VM 'de barındırılan üç düğümden oluşur. Kurumsal önbellekte iki adet aynı boyutlu *veri düğümü* ve bir küçük *çekirdek düğümü* vardır. Enterprise Flash önbelleğinde üç boyutlu veri düğümü vardır. Kurumsal küme Redsıs verilerini dahili olarak bölümlere böler. Her bölümde *birincil* ve en az bir *çoğaltma* bulunur. Her veri düğümü bir veya daha fazla bölüm barındırır. Kurumsal küme, herhangi bir bölümün birincil ve çoğaltmasının aynı veri düğümüne hiçbir şekilde birlikte olmamasını sağlar. Bölümler, verileri zaman uyumsuz iken ilgili çoğaltmalara göre çoğaltır.

Bir veri düğümü kullanılamaz hale geldiğinde veya ağ bölünmesi gerçekleştiğinde, [Standart çoğaltmada](#standard-replication) açıklanacak olan bir yük devretme gerçekleşecektir. Kurumsal küme, hangi kalan düğümlerin yeni bir çekirdeğe katılacağı tespit etmek için çekirdek tabanlı bir model kullanır. Ayrıca, gerektiğinde bu düğümlerde bulunan çoğaltma bölümlerini de yükseltir.

## <a name="geo-replication"></a>Coğrafi çoğaltma

[Coğrafi çoğaltma](cache-how-to-geo-replication.md) , genellikle iki Azure bölgesini kapsayan redsıs örnekleri Için Iki Azure önbelleğinin bağlanmasına yönelik bir mekanizmadır. Birincil bağlı önbellek ve diğeri ikincil bağlantılı önbellek olarak bir önbellek seçilir. Yalnızca birincil bağlantılı önbellek okuma ve yazma isteklerini kabul eder. Birincil önbelleğe yazılan veriler ikincil bağlantılı önbelleğe çoğaltılır. İkincil bağlantılı önbellek okuma isteklerine yönelik olarak kullanılabilir. Birincil ve ikincil önbellek örnekleri arasındaki veri aktarımı TLS ile korunmaktadır.

Coğrafi çoğaltma, genellikle olağanüstü durum kurtarma için tasarlanmıştır. Bu, önbellek verilerinizi farklı bir bölgeye yedekleme olanağı sağlar. Varsayılan olarak, uygulamanız birincil bölgeye yazar ve okur. Bu, isteğe bağlı olarak ikincil bölgeden okunacak şekilde yapılandırılabilir. Coğrafi çoğaltma, uygulamanızın geri kalanı birincil bölgede kalırsa bölgeler arasında eklenen ağ gecikmesinden kaynaklanan sorunlar nedeniyle otomatik yük devretme sağlamaz. İkincil önbelleğin bağlantısını kaldırarak yük devretmeyi yönetmeniz ve başlatmanız gerekir. Bu, bunu yeni birincil örnek olarak yükseltir.

## <a name="next-steps"></a>Sonraki adımlar

Redsıs yüksek kullanılabilirlik seçenekleri için Azure önbelleğini yapılandırma hakkında daha fazla bilgi edinin.

* [Redsıs Premium hizmet katmanları için Azure önbelleği](cache-overview.md#service-tiers)
* [Redsıs için Azure önbelleğine çoğaltmalar ekleme](cache-how-to-multi-replicas.md)
* [Redsıs için Azure önbelleği için bölge yedekliliği etkinleştirin](cache-how-to-zone-redundancy.md)
* [Redsıs için Azure önbelleği için Coğrafi çoğaltmayı ayarlama](cache-how-to-geo-replication.md)