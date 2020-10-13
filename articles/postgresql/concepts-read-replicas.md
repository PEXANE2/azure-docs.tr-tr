---
title: Çoğaltmaları oku-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede PostgreSQL için Azure veritabanı 'nda çoğaltmayı oku özelliği (tek sunucu) açıklanmaktadır.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/10/2020
ms.openlocfilehash: 124034fc6c999c37c6e79547b062508c957d1bac
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2020
ms.locfileid: "91939843"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda çoğaltmaları okuma-tek sunucu

Çoğaltma oku özelliği, bir PostgreSQL için Azure veritabanı sunucusunun bir salt okunurdur sunucusuna veri çoğaltmanıza olanak sağlar. Birincil sunucudan en fazla beş çoğaltmaya çoğaltabilirsiniz. Çoğaltmalar, PostgreSQL altyapısı yerel çoğaltma teknolojisiyle zaman uyumsuz olarak güncelleştirilir.

Çoğaltmalar, normal PostgreSQL için Azure Veritabanı sunucularına benzer şekilde yönetilen yeni sunuculardır. Her okuma çoğaltması için, sanal çekirdekler ve depolama biriminde GB/ay içinde sağlanan işlem için faturalandırılırsınız.

[Çoğaltmaları oluşturma ve yönetme](howto-read-replicas-portal.md)hakkında bilgi edinin.

## <a name="when-to-use-a-read-replica"></a>Okuma çoğaltması ne zaman kullanılır?
Okuma çoğaltması özelliği, okuma yoğunluklu iş yüklerinin performansını ve ölçeğini artırmaya yardımcı olur. Okuma iş yükleri çoğaltmalar için yalıtılabilir, ancak yazma iş yükleri birincilde yönlendirilebilir.

Yaygın bir senaryo, bı ve analitik iş yüklerinin raporlama için veri kaynağı olarak okuma çoğaltmasını kullanmasını sağlar.

Çoğaltmalar salt okunurdur, birincil üzerindeki yazma kapasitesini doğrudan azaltmazlar. Bu özellik, yazma açısından yoğun iş yükleri için uygun değildir.

Okuma çoğaltması özelliği PostgreSQL zaman uyumsuz çoğaltmasını kullanır. Özelliği, zaman uyumlu çoğaltma senaryolarına yönelik değildir. Birincil ve çoğaltma arasında ölçülebilir bir gecikme olacaktır. Çoğaltılan veriler, sonunda birincil verilerle tutarlı hale gelir. Bu gecikmeyi barındırabilecek iş yükleri için bu özelliği kullanın.

## <a name="cross-region-replication"></a>Bölgeler arası çoğaltma
Birincil sunucunuzdaki farklı bir bölgede bir okuma çoğaltması oluşturabilirsiniz. Çapraz bölge çoğaltma, olağanüstü durum kurtarma planlaması veya kullanıcılarınıza daha yakın veri getirme gibi senaryolar için yararlı olabilir.

>[!NOTE]
> Temel katman sunucuları yalnızca aynı bölge çoğaltmasını destekler.

[PostgreSQL Için Azure veritabanı bölgesinde](https://azure.microsoft.com/global-infrastructure/services/?products=postgresql)birincil bir sunucunuz olabilir. Birincil bir sunucu, eşleştirilmiş bölge veya evrensel çoğaltma bölgelerinde bir çoğaltmaya sahip olabilir. Aşağıdaki resimde, birincil bölgenize göre hangi çoğaltma bölgelerinin kullanılabildiği gösterilmektedir.

[:::image type="content" source="media/concepts-read-replica/read-replica-regions.png" alt-text="Çoğaltma bölgelerini oku":::](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>Evrensel çoğaltma bölgeleri
Birincil sunucunuzun bulunduğu yere bakılmaksızın, aşağıdaki bölgelerin herhangi birinde her zaman bir okuma çoğaltması oluşturabilirsiniz. Evrensel çoğaltma bölgeleri şunlardır:

Avustralya Doğu, Avustralya Güneydoğu, Orta ABD, Doğu Asya, Doğu ABD, Doğu ABD 2, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Orta Kuzey ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, UK Güney, UK Batı, Batı Avrupa, Batı ABD, Batı ABD 2, Orta Batı ABD.

### <a name="paired-regions"></a>Eşleştirilmiş bölgeler
Evrensel çoğaltma bölgelerine ek olarak, birincil sunucunuzun Azure eşlenmiş bölgesinde bir okuma çoğaltması oluşturabilirsiniz. Bölgenizin çiftini bilmiyorsanız [Azure eşlenmiş bölgeler makalesinden](../best-practices-availability-paired-regions.md)daha fazla bilgi edinebilirsiniz.

Olağanüstü durum kurtarma planlaması için çapraz bölge çoğaltmaları kullanıyorsanız, çoğaltmayı diğer bölgelerden biri yerine eşleştirilmiş bölgede oluşturmanızı öneririz. Eşleştirilmiş bölgeler eşzamanlı güncelleştirmeleri önleyin ve fiziksel yalıtımın ve veri fazlalığını önceliklendirir.  

Göz önünde bulundurulması gereken sınırlamalar vardır: 

* Bölgesel kullanılabilirlik: PostgreSQL için Azure veritabanı Fransa Orta, BAE Kuzey ve Almanya Orta bulunabilir. Ancak, eşleştirilmiş bölgeleri kullanılamaz.
    
* Tek yönlü çiftler: bazı Azure bölgeleri yalnızca bir yönde eşleştirilmelidir. Bu bölgeler Batı Hindistan Brezilya Güney içerir. 
   Bu, Batı Hindistan 'deki birincil sunucunun Güney Hindistan bir çoğaltma oluşturmasıdır. Ancak, Güney Hindistan bir birincil sunucu Batı Hindistan bir çoğaltma oluşturamaz. Bunun nedeni, Batı Hindistan ikincil bölgesinin Güney Hindistan, ancak Güney Hindistan ikincil bölgesi Batı Hindistan değildir.


## <a name="create-a-replica"></a>Çoğaltma oluşturma
Çoğaltma oluşturma iş akışını başlattığınızda, PostgreSQL için boş bir Azure veritabanı sunucusu oluşturulur. Yeni sunucu, birincil sunucuda bulunan verilerle doldurulur. Oluşturulma Zamanı, birincil ve son haftalık tam yedeklemeden bu yana geçen süre miktarına bağlıdır. Süre, birkaç dakika ile birkaç saat arasında değişebilir.

Her çoğaltma, depolama [otomatik büyüme](concepts-pricing-tiers.md#storage-auto-grow)için etkinleştirilir. Otomatik büyüme özelliği, çoğaltmanın kendisine çoğaltılan verilerle kalmasına izin verir ve çoğaltma sırasında depolama hatalarından kaynaklanan bir kesmeyi önler.

Okuma çoğaltması özelliği, mantıksal çoğaltma değil PostgreSQL fiziksel çoğaltmasını kullanır. Çoğaltma yuvaları kullanılarak akış çoğaltma varsayılan işlem modudur. Gerektiğinde, günlük aktarma işlemi yakalamak için kullanılır.

[Azure Portal bir okuma çoğaltması oluşturmayı](howto-read-replicas-portal.md)öğrenin.

## <a name="connect-to-a-replica"></a>Bir çoğaltmaya bağlanma
Bir çoğaltma oluşturduğunuzda, birincil sunucunun güvenlik duvarı kuralları veya VNet hizmeti uç noktası aktarılmaz. Bu kuralların çoğaltma için bağımsız olarak ayarlanması gerekir.

Çoğaltma, birincil sunucudan yönetici hesabını devralır. Birincil sunucudaki tüm Kullanıcı hesapları, okuma çoğaltmalarına çoğaltılır. Bir okuma çoğaltmasına yalnızca birincil sunucuda bulunan Kullanıcı hesaplarını kullanarak bağlanabilirsiniz.

Bir PostgreSQL için normal Azure veritabanı sunucusunda yaptığınız gibi, konak adını ve geçerli bir kullanıcı hesabını kullanarak çoğaltmaya bağlanabilirsiniz. Yönetici Kullanıcı adı **myadmin**olan **çoğaltmam** adlı bir sunucu için, psql kullanarak çoğaltmaya bağlanabilirsiniz:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

İstemde, Kullanıcı hesabının parolasını girin.

## <a name="monitor-replication"></a>Çoğaltmayı izleme
PostgreSQL için Azure veritabanı, çoğaltmayı izlemek için iki ölçüm sağlar. İki ölçüm çoğaltmalar ve **çoğaltma gecikmesi** **genelinde en fazla gecikme** . Bu ölçümleri görüntülemeyi öğrenmek için, [okuma çoğaltması nasıl yapılır makalesinin](howto-read-replicas-portal.md) **çoğaltma izleme** bölümüne bakın.

**Çoğaltmalar genelinde en fazla gecikme** ölçümü, birincil ve en fazla çoğaltma çoğaltması arasındaki gecikme sayısını bayt cinsinden gösterir. Bu ölçüm yalnızca birincil sunucuda bulunur ve yalnızca, okuma çoğaltmasından en az biri birincil ağa bağlıysa kullanılabilir olacaktır.

**Çoğaltma gecikmesi** ölçümü, son yeniden yürütülmüş işlemden bu yana geçen süreyi gösterir. Birincil sunucunuzda gerçekleşen işlem yoksa, ölçüm bu zaman gecikmesini yansıtır. Bu ölçüm yalnızca çoğaltma sunucuları için kullanılabilir. Çoğaltma gecikmesi `pg_stat_wal_receiver` görünümden hesaplanır:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

Çoğaltma gecikmesi iş yükünüz için kabul edilebilir bir değere ulaştığında sizi bilgilendirmek için bir uyarı ayarlayın. 

Diğer Öngörüler için, çoğaltma gecikmesini tüm çoğaltmalarda bayt olarak almak üzere birincil sunucuyu doğrudan sorgulayın.

PostgreSQL sürüm 10 ' da:

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

PostgreSQL sürüm 9,6 ve önceki sürümlerde:

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Birincil sunucu veya okuma çoğaltması yeniden başlatılırsa, yeniden başlatma ve yakalama için geçen süre çoğaltma gecikmesi ölçüsüne yansıtılır.

## <a name="stop-replication"></a>Çoğaltmayı durdurma
Birincil ve çoğaltma arasında çoğaltmayı durdurabilirsiniz. Durdur eylemi, çoğaltmanın yeniden başlatılmasına ve çoğaltma ayarlarını kaldırmasına neden olur. Birincil sunucu ve bir okuma çoğaltması arasında çoğaltma durdurulduktan sonra, çoğaltma tek başına bir sunucu haline gelir. Tek başına sunucusundaki veriler, çoğaltma durdurma komutunun başlatıldığı zamanda çoğaltma üzerinde kullanılabilir olan veri. Tek başına sunucu, birincil sunucu ile birlikte yakalamaz.

> [!IMPORTANT]
> Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.
> Bir okuma çoğaltmasında çoğaltmayı durdurmadan önce, çoğaltmanın gerekli tüm verilere sahip olduğundan emin olun.

Çoğaltmayı durdurduğunuzda, çoğaltma önceki birincil ve diğer çoğaltmalara ait tüm bağlantıları kaybeder.

[Bir çoğaltmaya çoğaltmayı durdurmayı](howto-read-replicas-portal.md)öğrenin.

## <a name="failover"></a>Yük devretme
Birincil ve çoğaltma sunucuları arasında otomatik yük devretme yoktur. 

Çoğaltma zaman uyumsuz olduğundan, birincil ve çoğaltma arasında bir gecikme vardır. Gecikme miktarı, birincil sunucuda çalışan iş yükünün ne kadar ağır ve veri merkezleri arasındaki gecikme süresi gibi bir dizi faktörden etkilenebilir. Tipik durumlarda, çoğaltma gecikme süresi birkaç saniye ile birkaç dakika arasında değişir. Ancak, birincil olarak çok ağır iş yüklerinin çalıştığı ve çoğaltmanın yeterince hızlı bir şekilde olmadığı durumlarda, öteleme daha yüksek olabilir. Her bir çoğaltma için kullanılabilen ölçüm *çoğaltması*gecikmesini kullanarak gerçek çoğaltma gecikmelerinizi izleyebilirsiniz. Bu ölçüm, son yeniden yürütülmüş işlemden bu yana geçen süreyi gösterir. Yineleme gecikmesini bir süre içinde gözlemleyerek ortalama gecikmenizin ne olduğunu tanımlamanızı öneririz. Çoğaltma gecikmesi üzerinde bir uyarı ayarlayabilirsiniz, böylece beklenen aralığın dışında olursa işlem yapabilirsiniz.

> [!Tip]
> Çoğaltmaya yük devretmek, çoğaltmanın birincili bağlantısını kaldırma sırasındaki gecikme süresi, ne kadar veri kaybedildiğine işaret eder.

Bir çoğaltmaya yük devretmek istediğinizde, 

1. Çoğaltmaya çoğaltmayı durdur<br/>
   Bu adım, çoğaltma sunucusunun yazmaları kabul edebilmesini sağlamak için gereklidir. Bu işlemin bir parçası olarak, çoğaltma sunucusu yeniden başlatılır ve birincil bilgisayardan kaldırılır. Çoğaltmayı Durdur ' u başlattığınızda, arka uç işleminin genellikle yaklaşık 2 dakika sürer. Bu eylemin etkilerini anlamak için bu makalenin [çoğaltmayı durdur](#stop-replication) bölümüne bakın.
    
2. Uygulamanızı (eski) çoğaltmaya işaret edin<br/>
   Her sunucunun benzersiz bir bağlantı dizesi vardır. Uygulamanızı birincil yerine (eski) çoğaltmaya işaret etmek üzere güncelleştirin.
    
Uygulamanız okuma ve yazma işlemlerini başarıyla tamamladıktan sonra, yük devretmeyi tamamladınız. Bir sorunu saptadığınızda ve yukarıdaki 1. ve 2. adımları tamamladıktan sonra uygulama deneyimlerinizin ne kadar süre açık olacağını gösterir.

### <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

Kullanılabilirlik bölge düzeyi veya bölgesel hata gibi önemli bir olağanüstü durum olayı olduğunda, okuma çoğaltmalarınızı yükselterek olağanüstü durum kurtarma işlemi gerçekleştirebilirsiniz. Kullanıcı arabirimi portalından, okuma çoğaltması sunucusuna gidebilirsiniz. Ardından çoğaltma sekmesine tıklayın ve çoğaltmayı bağımsız bir sunucu olarak yükseltmek için durdurabilirsiniz. Alternatif olarak, çoğaltma sunucusunu durdurmak ve yükseltmek için [Azure CLI](https://docs.microsoft.com/cli/azure/postgres/server/replica?view=azure-cli-latest#az_postgres_server_replica_stop) 'yi de kullanabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu bölümde çoğaltma oku özelliği hakkında dikkat edilecek noktalar özetlenmektedir.

### <a name="prerequisites"></a>Ön koşullar
Okuma çoğaltmaları ve [mantıksal kod çözme](concepts-logical.md) , bilgi Için doğrudan Postgres yazma günlüğüne (Wal) bağlıdır. Bu iki özellik, Postgres 'den farklı günlük düzeylerine sahip olmalıdır. Mantıksal kod çözme, okuma Çoğaltmalarından daha yüksek bir günlüğe kaydetme düzeyine sahip olmalıdır.

Doğru günlük kaydını yapılandırmak için Azure çoğaltma desteği parametresini kullanın. Azure çoğaltma desteğinin üç ayar seçeneği vardır:

* **Kapalı** -en az bilgiyi Wal 'e yerleştirir. Bu ayar, çoğu PostgreSQL için Azure veritabanı sunucuları üzerinde kullanılamaz.  
* **Çoğaltma** -daha ayrıntılı bir **şekilde.** Bu, [okuma çoğaltmalarının](concepts-read-replicas.md) çalışması için gereken en düşük günlüğe kaydetme düzeyidir. Bu ayar, çoğu sunucuda varsayılandır.
* **Çoğaltmadan**daha ayrıntılı **mantıksal** . Bu, mantıksal kod çözmenin çalışması için en düşük günlük kayıt düzeyidir. Okuma çoğaltmaları bu ayarda de çalışır.

Bu parametrenin bir değişikliğinden sonra sunucunun yeniden başlatılması gerekiyor. Dahili olarak, bu parametre Postgres parametrelerini, `wal_level` `max_replication_slots` ve ' ı ayarlar `max_wal_senders` .

### <a name="new-replicas"></a>Yeni çoğaltmalar
Bir okuma çoğaltması, PostgreSQL için yeni bir Azure veritabanı sunucusu olarak oluşturulur. Var olan bir sunucu bir çoğaltmaya yapılamaz. Başka bir okuma çoğaltmasının çoğaltmasını oluşturamazsınız.

### <a name="replica-configuration"></a>Çoğaltma yapılandırması
Çoğaltma, birincil ile aynı işlem ve depolama ayarları kullanılarak oluşturulur. Bir çoğaltma oluşturulduktan sonra, depolama ve yedekleme saklama süresi dahil olmak üzere çeşitli ayarlar değiştirilebilir.

Çoğaltma oluşturulduğunda veya daha sonra güvenlik duvarı kuralları, sanal ağ kuralları ve parametre ayarları birincil sunucudan çoğaltmaya devralınmaz.

### <a name="scaling"></a>Ölçeklendirme
Sanal çekirdekleri ölçeklendirin veya Genel Amaçlı ile bellek için Iyileştirilmiş:
* PostgreSQL, `max_connections` ikincil bir sunucudaki ayarın [birincil üzerindeki ayardan büyük veya ona eşit](https://www.postgresql.org/docs/current/hot-standby.html)olmasını gerektirir, aksi takdirde ikincil başlatılmaz.
* PostgreSQL için Azure veritabanı 'nda, bağlantılar bellek kullandığından, her bir sunucu için izin verilen en yüksek bağlantı, işlem SKU 'suna sabitlenmiştir. [Max_connections ve işlem SKU 'ları arasındaki eşleme](concepts-limits.md)hakkında daha fazla bilgi edinebilirsiniz.
* Ölçeği **artırma**: ilk olarak bir çoğaltmanın işlem ölçeğini ölçeklendirin, sonra birincili ölçeği ölçeklendirin. Bu sipariş, hataların gereksinimi ihlal etmelerini engeller `max_connections` .
* Ölçeği **azaltma**: ilk olarak birincil işlemin işlem ölçeğini azaltın, sonra çoğaltmanın ölçeğini azaltın. Çoğaltmayı Birincilden daha düşük ölçeklendirmeye çalışırsanız, bu, gereksinimi ihlal ettiğinden bir hata olur `max_connections` .

Depolama alanı ölçeklendiriliyor:
* Depolama-tam çoğaltmasından çoğaltma sorunlarını engellemek için tüm çoğaltmaların depolama otomatik büyümesi etkinleştirilmiş olarak etkinleştirilir. Bu ayar devre dışı bırakılamaz.
* Ayrıca, diğer tüm sunucularda yaptığınız gibi, depolamayı el ile de ölçeklendirdirebilirsiniz


### <a name="basic-tier"></a>Temel katman
Temel katman sunucuları yalnızca aynı bölge çoğaltmasını destekler.

### <a name="max_prepared_transactions"></a>max_prepared_transactions
[PostgreSQL](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAX-PREPARED-TRANSACTIONS) , `max_prepared_transactions` okuma çoğaltmasındaki parametrenin değerinin birincil değere eşit veya ondan büyük olmasını gerektirir; Aksi takdirde, çoğaltma başlatılmaz. Birincil üzerinde değişiklik yapmak istiyorsanız `max_prepared_transactions` , önce çoğaltmalarda değiştirin.

### <a name="stopped-replicas"></a>Durdurulan çoğaltmalar
Birincil sunucu ve bir okuma çoğaltması arasında çoğaltmayı durdurursanız, çoğaltma değişikliği uygulamak için yeniden başlatılır. Durdurulan çoğaltma, hem okuma hem de yazma işlemlerini kabul eden tek başına bir sunucu haline gelir. Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.

### <a name="deleted-primary-and-standalone-servers"></a>Birincil ve tek başına sunucular silindi
Birincil sunucu silindiğinde, tüm okuma çoğaltmaları tek başına sunucular haline gelir. Çoğaltmalar bu değişikliği yansıtacak şekilde yeniden başlatılır.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Portal okuma çoğaltmaları oluşturmayı ve yönetmeyi](howto-read-replicas-portal.md)öğrenin.
* [Azure CLI ve REST API okuma çoğaltmaları oluşturma ve yönetme](howto-read-replicas-cli.md)hakkında bilgi edinin.
