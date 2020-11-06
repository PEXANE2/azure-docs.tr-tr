---
title: Çoğaltmaları oku-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede PostgreSQL için Azure veritabanı 'nda çoğaltmayı oku özelliği (tek sunucu) açıklanmaktadır.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/05/2020
ms.openlocfilehash: 8fabf8169270c3162604b6535a6cf2fb07cd9a9d
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422153"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda çoğaltmaları okuma-tek sunucu

Çoğaltma oku özelliği, bir PostgreSQL için Azure veritabanı sunucusunun bir salt okunurdur sunucusuna veri çoğaltmanıza olanak sağlar. Çoğaltmalar, PostgreSQL altyapısı yerel fiziksel çoğaltma teknolojisiyle **zaman uyumsuz** olarak güncelleştirilir. Birincil sunucudan en fazla beş çoğaltmaya çoğaltabilirsiniz.

Çoğaltmalar, normal PostgreSQL için Azure Veritabanı sunucularına benzer şekilde yönetilen yeni sunuculardır. Her okuma çoğaltması için, sanal çekirdekler ve depolama biriminde GB/ay içinde sağlanan işlem için faturalandırılırsınız.

[Çoğaltmaları oluşturma ve yönetme](howto-read-replicas-portal.md)hakkında bilgi edinin.

## <a name="when-to-use-a-read-replica"></a>Okuma çoğaltması ne zaman kullanılır?
Okuma çoğaltması özelliği, okuma yoğunluklu iş yüklerinin performansını ve ölçeğini artırmaya yardımcı olur. Okuma iş yükleri çoğaltmalar için yalıtılabilir, ancak yazma iş yükleri birincilde yönlendirilebilir. Okuma çoğaltmaları aynı zamanda farklı bir bölgeye dağıtılabilir ve olağanüstü durum kurtarma durumunda okuma/yazma sunucusu olarak yükseltilebilir.

Yaygın bir senaryo, bı ve analitik iş yüklerinin raporlama için veri kaynağı olarak okuma çoğaltmasını kullanmasını sağlar.

Çoğaltmalar salt okunurdur, birincil üzerindeki yazma kapasitesini doğrudan azaltmazlar.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Özelliği, öteleme kabul edilebilir olduğu ve sorguları boşaltma için amaçlanan senaryolar için tasarlanmıştır. Çoğaltma verilerinin güncel olması beklenen zaman uyumlu çoğaltma senaryolarına yönelik değildir. Birincil ve çoğaltma arasında ölçülebilir bir gecikme olacaktır. Bu, iş yüküne ve birincil ve çoğaltma arasındaki gecikme süresine bağlı olarak dakikalar veya hatta saat içinde olabilir. Çoğaltılan veriler, sonunda birincil verilerle tutarlı hale gelir. Bu gecikmeyi barındırabilecek iş yükleri için bu özelliği kullanın. 

> [!NOTE]
> Çoğu iş yükleri için okuma çoğaltmaları, birincil sunucudan neredeyse gerçek zamanlı güncelleştirmeler sunar. Ancak, kalıcı ağır yazma yoğunluklu birincil iş yükleri sayesinde çoğaltma gecikmesi büyümeye devam edebilir ve birincil ile hiçbir şekilde yakalayamayabilir. Bu durum, çoğaltma sırasında alınana kadar WAL dosyaları silinmediği için birincil sırada depolama kullanımını da artırabilir. Bu durum devam ederse, yazma yoğunluklu iş yükleri tamamlandıktan sonra okuma çoğaltmasını silip yeniden oluşturmak, bir çoğaltmayı gecikmeyle ilgili olarak iyi bir duruma getirme seçeneğidir.
> Zaman uyumsuz okuma çoğaltmaları, bu tür ağır yazma iş yükleri için uygun değildir. Uygulamanız için okuma çoğaltmalarını değerlendirirken, olası gecikme süresi ve iş yükü döngüsünün çeşitli noktalarında beklenen RTO/RPO 'ya erişmek için tam bir uygulama iş yükü döngüsünün çoğaltma üzerindeki gecikmesini izleyin.
> 
## <a name="cross-region-replication"></a>Bölgeler arası çoğaltma
Birincil sunucunuzdaki farklı bir bölgede bir okuma çoğaltması oluşturabilirsiniz. Çapraz bölge çoğaltma, olağanüstü durum kurtarma planlaması veya kullanıcılarınıza daha yakın veri getirme gibi senaryolar için yararlı olabilir.

>[!NOTE]
> Temel katman sunucuları yalnızca aynı bölge çoğaltmasını destekler.

[PostgreSQL Için Azure veritabanı bölgesinde](https://azure.microsoft.com/global-infrastructure/services/?products=postgresql)birincil bir sunucunuz olabilir. Birincil bir sunucu, eşleştirilmiş bölge veya evrensel çoğaltma bölgelerinde bir çoğaltmaya sahip olabilir. Aşağıdaki resimde, birincil bölgenize göre hangi çoğaltma bölgelerinin kullanılabildiği gösterilmektedir.

[:::image type="content" source="media/concepts-read-replica/read-replica-regions.png" alt-text="Çoğaltma bölgelerini oku":::](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>Evrensel çoğaltma bölgeleri
Birincil sunucunuzun bulunduğu yere bakılmaksızın, aşağıdaki bölgelerin herhangi birinde her zaman bir okuma çoğaltması oluşturabilirsiniz. Evrensel çoğaltma bölgeleri şunlardır:

Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta ABD, Doğu Asya, Doğu ABD, Doğu ABD 2, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Orta Kuzey ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, UK Güney, UK Batı, Batı Avrupa, Batı ABD, Batı ABD 2, Orta Batı ABD.

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

Bir PostgreSQL için normal Azure veritabanı sunucusunda yaptığınız gibi, konak adını ve geçerli bir kullanıcı hesabını kullanarak çoğaltmaya bağlanabilirsiniz. Yönetici Kullanıcı adı **myadmin** olan **çoğaltmam** adlı bir sunucu için, psql kullanarak çoğaltmaya bağlanabilirsiniz:

```bash
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

İstemde, Kullanıcı hesabının parolasını girin.

## <a name="monitor-replication"></a>Çoğaltmayı izleme
PostgreSQL için Azure veritabanı, çoğaltmayı izlemek için iki ölçüm sağlar. İki ölçüm çoğaltmalar ve **çoğaltma gecikmesi** **genelinde en fazla gecikme** . Bu ölçümleri görüntülemeyi öğrenmek için, [okuma çoğaltması nasıl yapılır makalesinin](howto-read-replicas-portal.md) **çoğaltma izleme** bölümüne bakın.

**Çoğaltmalar genelinde en fazla gecikme** ölçümü, birincil ve en fazla çoğaltma çoğaltması arasındaki gecikme sayısını bayt cinsinden gösterir. Bu ölçüm yalnızca birincil sunucuda geçerlidir ve kullanılabilir ve yalnızca, okuma çoğaltmasından en az biri birincil ağa bağlıysa ve birincil akış çoğaltma modundaysa kullanılabilir olur. Çoğaltma bilgileri, çoğaltma, bir dosya aktarma çoğaltma modundaki birincil öğesinin arşivlenmiş günlüklerini kullanarak birincil ile yakalama süreciyle ilgili ayrıntıları göstermez.

**Çoğaltma gecikmesi** ölçümü, son yeniden yürütülmüş işlemden bu yana geçen süreyi gösterir. Birincil sunucunuzda gerçekleşen işlem yoksa, ölçüm bu zaman gecikmesini yansıtır. Bu ölçüm yalnızca çoğaltma sunucuları için geçerlidir ve kullanılabilir. Çoğaltma gecikmesi `pg_stat_wal_receiver` görünümden hesaplanır:

```SQL
SELECT EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

Çoğaltma gecikmesi iş yükünüz için kabul edilebilir bir değere ulaştığında sizi bilgilendirmek için bir uyarı ayarlayın. 

Diğer Öngörüler için, çoğaltma gecikmesini tüm çoğaltmalarda bayt olarak almak üzere birincil sunucuyu doğrudan sorgulayın.

> [!NOTE]
> Birincil sunucu veya okuma çoğaltması yeniden başlatılırsa, yeniden başlatma ve yakalama için geçen süre çoğaltma gecikmesi ölçüsüne yansıtılır.

## <a name="stop-replication--promote-replica"></a>Çoğaltmayı durdur/çoğaltmayı yükselt
Birincil ve çoğaltma arasında çoğaltmayı dilediğiniz zaman durdurabilirsiniz. Durdurma eylemi çoğaltmanın yeniden başlatılmasına neden olur ve çoğaltmayı bağımsız, tek başına okunan yazılabilir bir sunucu olacak şekilde yükseltir. Tek başına sunucusundaki veriler, çoğaltma durdurulduğunda Çoğaltma sunucusunda bulunan veriler olur. Birincil olan sonraki güncelleştirmeler çoğaltmaya yayılmaz. Ancak, çoğaltma sunucusu henüz uygulanmamış birikmiş günlüklere sahip olabilir. Yeniden başlatma işleminin bir parçası olarak çoğaltma, istemci bağlantılarını kabul etmeden önce tüm bekleyen günlükleri uygular.  

### <a name="considerations"></a>Dikkat edilmesi gerekenler
- Bir okuma çoğaltmasında çoğaltmayı durdurmadan önce, çoğaltmanın gerekli tüm verilere sahip olduğundan emin olmak için çoğaltma gecikmesini denetleyin. 
- Okuma çoğaltması, tek başına bir sunucu oluşturulmadan önce tüm bekleyen günlükleri uygulamak zorunda olduğu için, çoğaltmada önemli bir gecikme olması nedeniyle çoğaltma durdurma gerçekleştiğinde, RTO yazma ağır iş yükleri için daha yüksek olabilir. Lütfen bir çoğaltmayı yükseltmeyi planlarken bu duruma dikkat edin.
- Yükseltilen çoğaltma sunucusu tekrar bir çoğaltmaya yapılamaz.
- Bir çoğaltmayı birincil sunucu olacak şekilde yükseltirseniz, eski birincil sunucuya çoğaltmayı geri yükleyemezsiniz. Eski birincil bölgeye geri dönmek istiyorsanız yeni bir adla yeni bir çoğaltma sunucusu kurabilir (veya) eski birincil adı kullanarak eski birincil adı silebilir ve bir çoğaltma oluşturabilirsiniz.
- Birden çok okuma çoğaltmanıza sahipseniz ve bunlardan birini birincil sunucunuz olarak yükseltirseniz, diğer çoğaltma sunucuları hala eski birincil ağa bağlanır. Yeni, yükseltilen sunucudan çoğaltmaları yeniden oluşturmanız gerekebilir.

Çoğaltmayı durdurduğunuzda, çoğaltma önceki birincil ve diğer çoğaltmalara ait tüm bağlantıları kaybeder.

[Bir çoğaltmaya çoğaltmayı durdurmayı](howto-read-replicas-portal.md)öğrenin.

## <a name="failover-to-replica"></a>Çoğaltmaya yük devretme

Birincil sunucu arızası durumunda, otomatik olarak okuma çoğaltması için yük **devredilmez** . 

Çoğaltma zaman uyumsuz olduğundan, birincil ve çoğaltma arasında önemli bir gecikme olabilir. Gecikme miktarı, birincil sunucuda çalışan iş yükü türü ve birincil ve çoğaltma sunucusu arasındaki gecikme süresi gibi bir dizi faktörden etkilenir. Nominal yazma iş yüküne sahip tipik durumlarda, çoğaltma gecikmesi birkaç saniye ile birkaç dakika arasında beklenmektedir. Ancak, birincil olarak çok ağır yazma yoğunluklu iş yükünün çalıştığı ve çoğaltmanın yeterince hızlı bir şekilde olmadığı durumlarda, öteleme çok daha yüksek olabilir. Ölçüm *çoğaltması* gecikmesini kullanarak her bir çoğaltma için çoğaltma gecikmesini izleyebilirsiniz. Bu ölçüm, çoğaltmada son yeniden yürütülen işlemden bu yana geçen süreyi gösterir. Çoğaltma gecikmesini bir süre boyunca gözlemleyerek ortalama gecikme sayısını tanımlamanızı öneririz. Çoğaltma gecikmesi üzerinde bir uyarı ayarlayabilirsiniz, böylece beklenen aralığın dışında olursa işlem yapmanız istenir.

> [!Tip]
> Çoğaltmaya yük devretmek, çoğaltmanın birincili bağlantısını kaldırma sırasındaki gecikme süresi, ne kadar veri kaybedildiğine işaret eder.

Bir çoğaltmaya yük devretmek istediğinizde, 

1. Çoğaltmaya çoğaltmayı durdur<br/>
   Bu adım, çoğaltma sunucusunun tek başına bir sunucu olmasını ve yazmaları kabul edebilmesini sağlamak için gereklidir. Bu işlemin bir parçası olarak, çoğaltma sunucusu yeniden başlatılır ve birincil bilgisayardan kaldırılır. Çoğaltmayı Durdur ' u başlattığınızda, arka uç işlemi genellikle henüz uygulanmamış olan kalan günlüklerin uygulanması ve veritabanını bir salt okuma yazılabilir sunucu olarak açmak birkaç dakika sürer. Bu eylemin etkilerini anlamak için bu makalenin [çoğaltmayı durdur](#stop-replication--promote-replica) bölümüne bakın.
    
2. Uygulamanızı (eski) çoğaltmaya işaret edin<br/>
   Her sunucunun benzersiz bir bağlantı dizesi vardır. Uygulama Bağlantı dizenizi birincil yerine (eski) çoğaltmaya işaret etmek üzere güncelleştirin.
    
Uygulamanız okuma ve yazma işlemlerini başarıyla tamamladıktan sonra, yük devretmeyi tamamladınız. Bir sorunu saptadığınızda ve yukarıdaki 1. ve 2. adımları tamamladıktan sonra uygulama deneyimlerinizin ne kadar süre açık olacağını gösterir.

### <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

Kullanılabilirlik bölge düzeyi veya bölgesel hata gibi önemli bir olağanüstü durum olayı olduğunda, okuma çoğaltmalarınızı yükselterek olağanüstü durum kurtarma işlemi gerçekleştirebilirsiniz. Kullanıcı arabirimi portalından, okuma çoğaltması sunucusuna gidebilirsiniz. Ardından çoğaltma sekmesine tıklayın ve çoğaltmayı bağımsız bir sunucu olarak yükseltmek için durdurabilirsiniz. Alternatif olarak, çoğaltma sunucusunu durdurmak ve yükseltmek için [Azure CLI](/cli/azure/postgres/server/replica#az_postgres_server_replica_stop) 'yi de kullanabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu bölümde çoğaltma oku özelliği hakkında dikkat edilecek noktalar özetlenmektedir.

### <a name="prerequisites"></a>Ön koşullar
Okuma çoğaltmaları ve [mantıksal kod çözme](concepts-logical.md) , bilgi Için doğrudan Postgres yazma günlüğüne (Wal) bağlıdır. Bu iki özellik, Postgres 'den farklı günlük düzeylerine sahip olmalıdır. Mantıksal kod çözme, okuma Çoğaltmalarından daha yüksek bir günlüğe kaydetme düzeyine sahip olmalıdır.

Doğru günlük kaydını yapılandırmak için Azure çoğaltma desteği parametresini kullanın. Azure çoğaltma desteğinin üç ayar seçeneği vardır:

* **Kapalı** -en az bilgiyi Wal 'e yerleştirir. Bu ayar, çoğu PostgreSQL için Azure veritabanı sunucuları üzerinde kullanılamaz.  
* **Çoğaltma** -daha ayrıntılı bir **şekilde.** Bu, [okuma çoğaltmalarının](concepts-read-replicas.md) çalışması için gereken en düşük günlüğe kaydetme düzeyidir. Bu ayar, çoğu sunucuda varsayılandır.
* **Çoğaltmadan** daha ayrıntılı **mantıksal** . Bu, mantıksal kod çözmenin çalışması için en düşük günlük kayıt düzeyidir. Okuma çoğaltmaları bu ayarda de çalışır.


### <a name="new-replicas"></a>Yeni çoğaltmalar
Bir okuma çoğaltması, PostgreSQL için yeni bir Azure veritabanı sunucusu olarak oluşturulur. Var olan bir sunucu bir çoğaltmaya yapılamaz. Başka bir okuma çoğaltmasının çoğaltmasını oluşturamazsınız.

### <a name="replica-configuration"></a>Çoğaltma yapılandırması
Çoğaltma, birincil ile aynı işlem ve depolama ayarları kullanılarak oluşturulur. Bir çoğaltma oluşturulduktan sonra, depolama ve yedekleme saklama süresi dahil olmak üzere çeşitli ayarlar değiştirilebilir.

Çoğaltma oluşturulduğunda veya daha sonra güvenlik duvarı kuralları, sanal ağ kuralları ve parametre ayarları birincil sunucudan çoğaltmaya devralınmaz.

### <a name="scaling"></a>Ölçeklendirme
Sanal çekirdekleri ölçeklendirin veya Genel Amaçlı ile bellek için Iyileştirilmiş:
* PostgreSQL, `max_connections` ikincil bir sunucudaki ayarın [birincil üzerindeki ayardan büyük veya ona eşit](https://www.postgresql.org/docs/current/hot-standby.html)olmasını gerektirir, aksi takdirde ikincil başlatılmaz.
* PostgreSQL için Azure veritabanı 'nda, bağlantılar bellek kullandığından, her bir sunucu için izin verilen en yüksek bağlantı, işlem SKU 'suna sabitlenmiştir. [Max_connections ve işlem SKU 'ları arasındaki eşleme](concepts-limits.md)hakkında daha fazla bilgi edinebilirsiniz.
* Ölçeği **artırma** : ilk olarak bir çoğaltmanın işlem ölçeğini ölçeklendirin, sonra birincili ölçeği ölçeklendirin. Bu sipariş, hataların gereksinimi ihlal etmelerini engeller `max_connections` .
* Ölçeği **azaltma** : ilk olarak birincil işlemin işlem ölçeğini azaltın, sonra çoğaltmanın ölçeğini azaltın. Çoğaltmayı Birincilden daha düşük ölçeklendirmeye çalışırsanız, bu, gereksinimi ihlal ettiğinden bir hata olur `max_connections` .

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