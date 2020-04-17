---
title: Okuma kopyaları - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'nda okundu yineleme özelliği açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/23/2020
ms.openlocfilehash: 545d04bdede76a6ce25c9e4665f39c01ff6caa73
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81531992"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı'nda yinelemeleri okuyun - Tek Sunucu

Okuma çoğaltma özelliği, PostgreSQL sunucusu için Azure Veritabanı'ndaki verileri salt okunur sunucuya çoğaltmanızı sağlar. Ana sunucudan en fazla beş çoğaltmaya çoğaltabilirsiniz. Yinelemeler, PostgreSQL engine yerel çoğaltma teknolojisi yle eşzamanlı olarak güncelleştirilir.

Yinelemeler, PostgreSQL sunucuları için normal Azure Veritabanı'na benzer şekilde yönettiğiniz yeni sunuculardır. Her okuma yinelemesi için, vCores ve GB / ay depolama sağlanan hesaplama için fatura konum.

Yinelemeleri nasıl [oluşturup yöneteceklerini](howto-read-replicas-portal.md)öğrenin.

## <a name="when-to-use-a-read-replica"></a>Okuma yinelemesi ne zaman kullanılır?
Okuma yineleme özelliği, okuma yoğun iş yüklerinin performansını ve ölçeğini geliştirmeye yardımcı olur. Okuma iş yükleri yinelemelere yalıtılabilirken, yazma iş yükleri ana yönelik olabilir.

Yaygın bir senaryo, BI ve analitik iş yüklerinin raporlama için veri kaynağı olarak okundu yinelemesini kullanmasını sağlamaktır.

Yinelemeler salt okunur olduğundan, ana kurumüzerindeki yazma kapasitesi yüklerini doğrudan azaltmaz. Bu özellik yazma yoğun iş yüklerini hedeflemez.

Okunan çoğaltma özelliği PostgreSQL asynchronous çoğaltma kullanır. Özellik eşzamanlı çoğaltma senaryoları için değildir. Ana ve çoğaltma arasında ölçülebilir bir gecikme olacak. Yinelemedeki veriler sonunda ana daki verilerle tutarlı hale gelir. Bu gecikmeyi karşılayabilecek iş yükleri için bu özelliği kullanın.

## <a name="cross-region-replication"></a>Bölgeler arası çoğaltma
Ana sunucunuzdan farklı bir bölgede okuma yinelemesi oluşturabilirsiniz. Bölgeler arası çoğaltma, olağanüstü durum kurtarma planlaması veya verileri kullanıcılarınıza yakınlaştırma gibi senaryolar için yararlı olabilir.

[PostgreSQL bölgesi için](https://azure.microsoft.com/global-infrastructure/services/?products=postgresql)herhangi bir Azure Veritabanında ana sunucunuz olabilir. Ana sunucu, eşleştirilmiş bölgesinde veya evrensel yineleme bölgelerinde bir yinelemeye sahip olabilir. Aşağıdaki resimde, ana bölgenize bağlı olarak hangi yineleme bölgelerinin kullanılabilir olduğu gösterilmektedir.

[![Yineleme bölgelerini okuma](media/concepts-read-replica/read-replica-regions.png)](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>Evrensel çoğaltma bölgeleri
Ana sunucunuzun nerede bulunduğuna bakılmaksızın, aşağıdaki bölgelerden herhangi birinde her zaman bir okuma yinelemesi oluşturabilirsiniz. Bunlar evrensel çoğaltma bölgeleridir:

Avustralya Doğu, Avustralya Güneydoğu, Orta ABD, Doğu Asya, Doğu ABD, Doğu ABD 2, Japonya Doğu, Japonya Batı, Kore Merkez, Kore Güney, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, İngiltere Güney, İngiltere Batı, Batı Avrupa, Batı ABD.

*West US 2 geçici olarak bölgeler arası yineleme konumu olarak kullanılamaz.


### <a name="paired-regions"></a>Eşleştirilmiş bölgeler
Evrensel yineleme bölgelerine ek olarak, ana sunucunuzun Azure eşleştirilmiş bölgesinde bir okuma yinelemesi oluşturabilirsiniz. Bölgenizin çiftini bilmiyorsanız, [Azure Eşli Bölgeler makalesinden](../best-practices-availability-paired-regions.md)daha fazla bilgi edinebilirsiniz.

Olağanüstü durum kurtarma planlaması için bölgeler arası yinelemeler kullanıyorsanız, yinelemeyi diğer bölgelerden biri yerine eşleştirilmiş bölgede oluşturmanızı öneririz. Eşli bölgeler eşzamanlı güncelleştirmeleri önler ve fiziksel yalıtım ve veri ihtisasını önceliklendirir.  

Göz önünde bulundurulması gereken sınırlamalar vardır: 

* Bölgesel kullanılabilirlik: PostgreSQL için Azure Veritabanı Batı ABD 2, France Central, BAE North ve Germany Central'da kullanılabilir. Ancak, eşleştirilmiş bölgeleri kullanılamıyor.
    
* Tek yönlü çiftler: Bazı Azure bölgeleri yalnızca tek yönde eşlenir. Bu bölgeler Batı Hindistan, Brezilya Güney içerir. 
   Bu, Batı Hindistan'daki bir ana sunucunun Güney Hindistan'da bir kopyasını oluşturabileceği anlamına gelir. Ancak, Güney Hindistan'da bir ana sunucu Batı Hindistan'da bir kopyasını oluşturamazsınız. Bunun nedeni Batı Hindistan'ın ikincil bölgesinin Güney Hindistan olması, ancak Güney Hindistan'ın ikincil bölgesinin Batı Hindistan olmamasıdır.


## <a name="create-a-replica"></a>Çoğaltma oluşturma
Çoğaltma oluşturma iş akışını başlattığınızda, PostgreSQL sunucusu için boş bir Azure Veritabanı oluşturulur. Yeni sunucu ana sunucudaki verilerle doldurulur. Oluşturma süresi, ana daki veri miktarına ve son haftalık tam yedeklemeden bu yana geçen süreye bağlıdır. Zaman birkaç dakika ile birkaç saat arasında değişebilir.

Depolama [otomatik büyümek](concepts-pricing-tiers.md#storage-auto-grow)için her yineleme etkinleştirilir. Otomatik büyütme özelliği, yinelemenin çoğaltılan verileri takip etmesini ve depolama hatalarının dışında oluşan çoğaltma kırılmasını önlemesini sağlar.

Okunan çoğaltma özelliği, mantıksal çoğaltma değil, PostgreSQL fiziksel çoğaltma kullanır. Çoğaltma yuvalarını kullanarak çoğaltma akışı varsayılan işlem modudur. Gerektiğinde, günlük nakliye yetişmek için kullanılır.

[Azure portalında okuma yinelemesini](howto-read-replicas-portal.md)nasıl oluşturabilirsiniz öğrenin.

## <a name="connect-to-a-replica"></a>Yinelemeye bağlanma
Bir yineleme oluşturduğunuzda, ana sunucunun güvenlik duvarı kurallarını veya VNet hizmet bitiş noktasını devralmaz. Bu kurallar çoğaltma için bağımsız olarak ayarlanmalıdır.

Yineleme, yönetici hesabını ana sunucudan devralır. Ana sunucudaki tüm kullanıcı hesapları okundu yinelemelere kopyalanır. Yalnızca ana sunucuda bulunan kullanıcı hesaplarını kullanarak okuma yinelemesine bağlanabilirsiniz.

PostgreSQL sunucusu için normal bir Azure Veritabanında olduğu gibi, ana bilgisayar adını ve geçerli bir kullanıcı hesabını kullanarak yinelemeye bağlanabilirsiniz. Admin kullanıcı adı **myadmin**ile **benim çoğaltma** adlı bir sunucu için, psql kullanarak çoğaltma bağlayabilirsiniz:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

İsteyerek, kullanıcı hesabının parolasını girin.

## <a name="monitor-replication"></a>Çoğaltmayı izleme
PostgreSQL için Azure Veritabanı çoğaltma izleme için iki ölçüm sağlar. İki ölçüm **Max Lag Çoğaltmalar** ve **Replica Lag**across vardır. Bu ölçümleri nasıl görüntüleyin, **nasıl** [görüntülenecek makalenin yineleme](howto-read-replicas-portal.md)bölümüne bakın.

**Çoğaltmalar Arasında Max Lag** ölçümü, ana ve en geri kalan yineleme arasındaki baytgecikmesini gösterir. Bu metrik yalnızca ana sunucuda kullanılabilir.

**Yineleme Gecikmesi** ölçümü, son yeniden oynan işlemden bu yana geçen süreyi gösterir. Ana sunucunuzda herhangi bir işlem meydana gelmezse, metrik bu süre gecikmesini yansıtır. Bu metrik yalnızca yineleme sunucuları için kullanılabilir. Çoğaltma Gecikmesi `pg_stat_wal_receiver` görünümden hesaplanır:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

Yineleme gecikmesi iş yükünüz için kabul edilemez bir değere ulaştığında sizi bilgilendirmek için bir uyarı ayarlayın. 

Ek bir kavrayış için, tüm yinelemelerde çoğaltma gecikmesi almak için ana sunucuyu doğrudan sorgula.

PostgreSQL sürüm 10'da:

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

PostgreSQL sürüm 9.6 ve önceki sürümde:

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Bir ana sunucu veya okuma yinelemeyeniden başlatılırsa, yeniden başlatma ve yakalama süresi Çoğaltma Gecikmesi ölçüsüne yansıtılır.

## <a name="stop-replication"></a>Çoğaltmayı durdurma
Bir ana ve bir yineleme arasındaki çoğaltmayı durdurabilirsiniz. Durdurma eylemi yinelemenin yeniden başlatılmasına ve çoğaltma ayarlarını kaldırmasına neden olur. Çoğaltma ana sunucu ve okuma yinelemesi arasında durdurulduktan sonra, çoğaltma bağımsız bir sunucu olur. Bağımsız sunucudaki veriler, çoğaltmayı durdurma komutu başlatıldığında yinelemede bulunan verilerdir. Bağımsız sunucu ana sunucuya yetişmiyor.

> [!IMPORTANT]
> Bağımsız sunucu yeniden çoğaltma yapılamaz.
> Okuma yinelemesi üzerinde çoğaltmayı durdurmadan önce, yinelemenin gereksinim duyduğunuz tüm verilere sahip olduğundan emin olun.

Çoğaltmayı durdurduğunuzda, yineleme önceki ana ve diğer yinelemelere tüm bağlantıları kaybeder.

Çoğaltmayı nasıl [durdurup durdurabildiğini](howto-read-replicas-portal.md)öğrenin.

## <a name="failover"></a>Yük devretme
Ana ve çoğaltma sunucuları arasında otomatik bir hata yoktur. 

Çoğaltma asynchronous olduğundan, ana ve çoğaltma arasında gecikme vardır. Gecikme miktarı, ana sunucuda çalışan iş yükünün ne kadar ağır olduğu ve veri merkezleri arasındaki gecikme sayısı gibi bir dizi faktörden etkilenebilir. Çoğu durumda, çoğaltma gecikmesi birkaç saniye ile birkaç dakika arasında değişir. Her çoğaltma için kullanılabilir metrik *Çoğaltma Gecikmesi*kullanarak gerçek çoğaltma gecikmenizi izleyebilirsiniz. Bu metrik, son yeniden oynan işlemden sonraki zamanı gösterir. Bir süre boyunca çoğaltma gecikmenizi gözlemleyerek ortalama gecikmenizin ne olduğunu belirlemenizi öneririz. Yineleme gecikmesi üzerinde bir uyarı ayarlayabilirsiniz, böylece beklenen aralığınızın dışına çıkarsa, harekete geçebilirsiniz.

> [!Tip]
> Yinelemeye geçemezseniz, yinelemeyi ana bilgisayardan ayırdığınızdaki gecikme, ne kadar veri kaybolduğunu gösterir.

Bir çoğaltma için başarısız olmak istediğinize karar verdikten sonra, 

1. Yinelemeye çoğaltmayı durdurma<br/>
   Bu adım, yineleme sunucusunun yazıları kabul edebilmesi için gereklidir. Bu işlemin bir parçası olarak, çoğaltma sunucusu yeniden başlatılacak ve ana kaynaktan bağlantısı zıddı. Durdurma çoğaltmasını başlattıktan sonra, arka uç işleminin tamamlanması genellikle yaklaşık 2 dakika sürer. Bu eylemin sonuçlarını anlamak için bu makalenin [çoğaltmayı durdurma](#stop-replication) bölümüne bakın.
    
2. Uygulamanızı (eski) yinelemeye yönlendirin<br/>
   Her sunucunun benzersiz bir bağlantı dizesi vardır. Uygulamanızı ana kopya yerine (eski) yinelemeye işaret etmek için güncelleştirin.
    
Uygulamanız okuma ve yazma işlemlerini başarıyla tamamladıktan sonra, başarısızlığı tamamladınız. Uygulama deneyimlerinizin çalışmam süresi, bir sorunu algılayıp yukarıdaki 1 ve 2 adımlarını tamamlanıncaya bağlıdır.


## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu bölümde, okundu yineleme özelliği yle ilgili hususlar özetlenmiştir.

### <a name="prerequisites"></a>Ön koşullar
Bir okuma yinelemesi `azure.replication_support` oluşturmadan önce, parametre ana sunucuda **ÇOĞALTMA** olarak ayarlanmalıdır. Bu parametre değiştirildiğinde, değişikliğin etkili olması için sunucu yeniden başlatması gerekir. `azure.replication_support` Parametre yalnızca Genel Amaç ve Bellek Optimize Edilmiş katmanlar için geçerlidir.

### <a name="new-replicas"></a>Yeni yinelemeler
PostgreSQL sunucusu için yeni bir Azure Veritabanı olarak okundu yinelemesi oluşturulur. Varolan bir sunucu çoğaltma haline getirilebilir. Başka bir okuma yinelemesinin kopyasını oluşturamazsınız.

### <a name="replica-configuration"></a>Yineleme yapılandırması
Yineleme, ana bilgisayarla aynı bilgi işlem ve depolama ayarları kullanılarak oluşturulur. Yineleme oluşturulduktan sonra, ana sunucudan bağımsız olarak birkaç ayar değiştirilebilir: bilgi işlem oluşturma, vCores, depolama ve yedekleme bekletme süresi. Fiyatlandırma katmanı, Temel katman alabilen veya temel katmandan bağımsız olarak değiştirilebilir.

> [!IMPORTANT]
> Ana ayar yeni bir değere güncelleştirilmeden önce, yineleme yapılandırmasını eşit veya daha büyük bir değerle güncelleştirin. Bu eylem, çoğaltmanın ana kopya üzerinde yapılan değişiklikleri yansıtmasını sağlar.

PostgreSQL, okundu yinelemesindeki `max_connections` parametrenin değerinin ana değerden büyük veya eşit olmasını gerektirir; aksi takdirde, yineleme başlatılamayacak. PostgreSQL için Azure Veritabanı'nda `max_connections` parametre değeri SKU'yu temel almaktadır. Daha fazla bilgi için [PostgreSQL için Azure Veritabanı'ndaki Sınırlar'a](concepts-limits.md)bakın. 

Yukarıda açıklanan sunucu değerlerini güncelleştirmeye çalışır, ancak sınırlara uymazsanız, bir hata alırsınız.

Güvenlik duvarı kuralları, sanal ağ kuralları ve parametre ayarları, yineleme oluşturulduğunda veya sonrasında ana sunucudan yinelemeye devredilmez.

### <a name="max_prepared_transactions"></a>max_prepared_transactions
[PostgreSQL,](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAX-PREPARED-TRANSACTIONS) okundu yinelemesindeki `max_prepared_transactions` parametrenin değerinin ana değerden büyük veya eşit olmasını gerektirir; aksi takdirde, yineleme başlatılamayacak. Ana üzerinde değiştirmek `max_prepared_transactions` istiyorsanız, önce kopyaları değiştirin.

### <a name="stopped-replicas"></a>Durdurulan yinelemeler
Ana sunucu ile okuma yinelemesi arasındaki çoğaltmayı durdurursanız, yineleme değişikliği uygulamak için yeniden başlatılır. Durdurulan yineleme, hem okumaları hem de yazmaları kabul eden bağımsız bir sunucu haline gelir. Bağımsız sunucu yeniden çoğaltma yapılamaz.

### <a name="deleted-master-and-standalone-servers"></a>Silinmiş ana ve bağımsız sunucular
Ana sunucu silindiğinde, okunan tüm yinelemeler bağımsız sunuculara dönüşür. Yinelemeler bu değişikliği yansıtacak şekilde yeniden başlatılır.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure portalında okuma yinelemelerini](howto-read-replicas-portal.md)nasıl oluşturup yöneteceklerini öğrenin.
* [Azure CLI ve REST API'sında okuma yinelemelerini nasıl oluşturup yöneteceğimize](howto-read-replicas-cli.md)öğrenin.
