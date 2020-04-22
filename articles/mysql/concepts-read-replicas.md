---
title: Yinelemeleri okuyun - MySQL için Azure Veritabanı.
description: "MySQL için Azure Veritabanı'nda okuma yinelemeleri hakkında bilgi edinin: bölgeleri seçme, yineleme oluşturma, yinelemelere bağlanma, çoğaltmayı izleme ve çoğaltmayı durdurma."
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: 47f686f810f62fe03a9b0217677c436f3b91782b
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81767878"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı’nda okuma amaçlı çoğaltmalar

Okuma amaçlı çoğaltma özelliği, MySQL için Azure Veritabanı’ndan salt okunur bir sunucuya verileri çoğaltmanıza olanak sağlar. Ana sunucudan en fazla beş çoğaltmaya çoğaltabilirsiniz. Çoğaltmalar, MySQL altyapısının yerel ikili günlük (binlog) dosya konumu tabanlı çoğaltma teknolojisini kullanılarak zaman uyumsuz olarak güncelleştirilir. Binlog çoğaltma hakkında daha fazla bilgi edinmek için [MySQL binlog çoğaltma genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html)bakın.

Yinelemeler, MySQL sunucuları için normal Azure Veritabanı'na benzer şekilde yönettiğiniz yeni sunuculardır. Her okuma yinelemesi için, vCores ve GB / ay depolama sağlanan hesaplama için fatura konum.

MySQL çoğaltma özellikleri ve sorunları hakkında daha fazla bilgi edinmek için [MySQL çoğaltma belgelerine](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html)bakın.

## <a name="when-to-use-a-read-replica"></a>Okuma yinelemesi ne zaman kullanılır?

Okuma yineleme özelliği, okuma yoğun iş yüklerinin performansını ve ölçeğini geliştirmeye yardımcı olur. Okuma iş yükleri yinelemelere yalıtılabilirken, yazma iş yükleri ana yönelik olabilir.

Yaygın bir senaryo, BI ve analitik iş yüklerinin raporlama için veri kaynağı olarak okundu yinelemesini kullanmasını sağlamaktır.

Yinelemeler salt okunur olduğundan, ana kurumüzerindeki yazma kapasitesi yüklerini doğrudan azaltmaz. Bu özellik yazma yoğun iş yüklerini hedeflemez.

Okuma çoğaltma özelliği MySQL asynchronous çoğaltma kullanır. Özellik eşzamanlı çoğaltma senaryoları için değildir. Ana ve çoğaltma arasında ölçülebilir bir gecikme olacak. Yinelemedeki veriler sonunda ana daki verilerle tutarlı hale gelir. Bu gecikmeyi karşılayabilecek iş yükleri için bu özelliği kullanın.

## <a name="cross-region-replication"></a>Bölgeler arası çoğaltma
Ana sunucunuzdan farklı bir bölgede okuma yinelemesi oluşturabilirsiniz. Bölgeler arası çoğaltma, olağanüstü durum kurtarma planlaması veya verileri kullanıcılarınıza yakınlaştırma gibi senaryolar için yararlı olabilir.

[MySQL bölgesi için](https://azure.microsoft.com/global-infrastructure/services/?products=mysql)herhangi bir Azure Veritabanında ana sunucunuz olabilir.  Ana sunucu, eşleştirilmiş bölgesinde veya evrensel yineleme bölgelerinde bir yinelemeye sahip olabilir. Aşağıdaki resimde, ana bölgenize bağlı olarak hangi yineleme bölgelerinin kullanılabilir olduğu gösterilmektedir.

[![Yineleme bölgelerini okuma](media/concepts-read-replica/read-replica-regions.png)](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>Evrensel çoğaltma bölgeleri
Ana sunucunuzun nerede bulunduğuna bakılmaksızın, aşağıdaki bölgelerden herhangi birinde okuma yinelemesi oluşturabilirsiniz. Desteklenen evrensel çoğaltma bölgeleri şunlardır:

Avustralya Doğu, Avustralya Güneydoğu, Orta ABD, Doğu Asya, Doğu ABD, Doğu ABD 2, Japonya Doğu, Japonya Batı, Kore Merkez, Kore Güney, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, İngiltere Güney, İngiltere Batı, Batı Avrupa, Batı ABD.

*West US 2 geçici olarak bölgeler arası yineleme konumu olarak kullanılamaz.

### <a name="paired-regions"></a>Eşleştirilmiş bölgeler
Evrensel yineleme bölgelerine ek olarak, ana sunucunuzun Azure eşleştirilmiş bölgesinde bir okuma yinelemesi oluşturabilirsiniz. Bölgenizin çiftini bilmiyorsanız, [Azure Eşli Bölgeler makalesinden](../best-practices-availability-paired-regions.md)daha fazla bilgi edinebilirsiniz.

Olağanüstü durum kurtarma planlaması için bölgeler arası yinelemeler kullanıyorsanız, yinelemeyi diğer bölgelerden biri yerine eşleştirilmiş bölgede oluşturmanızı öneririz. Eşli bölgeler eşzamanlı güncelleştirmeleri önler ve fiziksel yalıtım ve veri ihtisasını önceliklendirir.  

Ancak, göz önünde bulundurulması gereken sınırlamalar vardır: 

* Bölgesel kullanılabilirlik: MySQL için Azure Veritabanı Batı ABD 2, France Central, BAE North ve Germany Central'da kullanılabilir. Ancak, eşleştirilmiş bölgeleri kullanılamıyor.
    
* Tek yönlü çiftler: Bazı Azure bölgeleri yalnızca tek yönde eşlenir. Bu bölgeler Batı Hindistan, Brezilya Güney ve ABD Gov Virginia içerir. 
   Bu, Batı Hindistan'daki bir ana sunucunun Güney Hindistan'da bir kopyasını oluşturabileceği anlamına gelir. Ancak, Güney Hindistan'da bir ana sunucu Batı Hindistan'da bir kopyasını oluşturamazsınız. Bunun nedeni Batı Hindistan'ın ikincil bölgesinin Güney Hindistan olması, ancak Güney Hindistan'ın ikincil bölgesinin Batı Hindistan olmamasıdır.


## <a name="create-a-replica"></a>Çoğaltma oluşturma

Ana sunucuda varolan çoğaltma sunucuları yoksa, ana sunucu önce kendisini çoğaltma için hazırlamak için yeniden başlatılacaktır.

Çoğaltma oluşturma iş akışını başlattığınızda, MySQL sunucusu için boş bir Azure Veritabanı oluşturulur. Yeni sunucu ana sunucudaki verilerle doldurulur. Oluşturma süresi, ana daki veri miktarına ve son haftalık tam yedeklemeden bu yana geçen süreye bağlıdır. Zaman birkaç dakika ile birkaç saat arasında değişebilir.

Depolama [otomatik büyümek](concepts-pricing-tiers.md#storage-auto-grow)için her yineleme etkinleştirilir. Otomatik büyütme özelliği, yinelemenin çoğaltılan verileri takip etmesini ve depolama dışı hatalardan kaynaklanan çoğaltma kesintisini önlemesini sağlar.

[Azure portalında okuma yinelemesini](howto-read-replicas-portal.md)nasıl oluşturabilirsiniz öğrenin.

## <a name="connect-to-a-replica"></a>Yinelemeye bağlanma

Oluşturmada, yineleme ana sunucunun güvenlik duvarı kurallarını devralır. Daha sonra, bu kurallar ana sunucudan bağımsızdır.

Yineleme, yönetici hesabını ana sunucudan devralır. Ana sunucudaki tüm kullanıcı hesapları okundu yinelemelere kopyalanır. Yalnızca ana sunucuda bulunan kullanıcı hesaplarını kullanarak okuma yinelemesine bağlanabilirsiniz.

MySQL sunucusu için normal bir Azure Veritabanında olduğu gibi, ana bilgisayar adını ve geçerli bir kullanıcı hesabını kullanarak yinelemeye bağlanabilirsiniz. Admin kullanıcı adı **myadmin**ile **myreplica** adlı bir sunucu için, mysql CLI kullanarak çoğaltma bağlayabilirsiniz:

```bash
mysql -h myreplica.mysql.database.azure.com -u myadmin@myreplica -p
```

İsteyerek, kullanıcı hesabının parolasını girin.

## <a name="monitor-replication"></a>Çoğaltmayı izleme

MySQL için Azure Veritabanı, Azure **Monitor'da saniye cinsinden çoğaltma gecikmesi** sağlar. Bu metrik yalnızca yinelemeler için kullanılabilir.

Bu metrik, MySQL'in `seconds_behind_master` `SHOW SLAVE STATUS` komutundaki metrik kullanılarak hesaplanır.

Çoğaltma gecikmesi iş yükünüz için kabul edilemez bir değere ulaştığında sizi bilgilendirmek için bir uyarı ayarlayın.

## <a name="stop-replication"></a>Çoğaltmayı durdurma

Bir ana ve bir yineleme arasındaki çoğaltmayı durdurabilirsiniz. Çoğaltma ana sunucu ve okuma yinelemesi arasında durdurulduktan sonra, çoğaltma bağımsız bir sunucu olur. Bağımsız sunucudaki veriler, çoğaltmayı durdurma komutu başlatıldığında yinelemede bulunan verilerdir. Bağımsız sunucu ana sunucuya yetişmiyor.

Bir yinelemeye çoğaltmayı durdurmayı seçtiğinizde, önceki ana ve diğer yinelemelere olan tüm bağlantıları kaybeder. Bir ana ve onun çoğaltma arasında otomatik bir hata yoktur.

> [!IMPORTANT]
> Bağımsız sunucu yeniden çoğaltma yapılamaz.
> Okuma yinelemesi üzerinde çoğaltmayı durdurmadan önce, yinelemenin gereksinim duyduğunuz tüm verilere sahip olduğundan emin olun.

Çoğaltmayı nasıl [durdurup durdurabildiğini](howto-read-replicas-portal.md)öğrenin.

## <a name="considerations-and-limitations"></a>Önemli noktalar ve sınırlamalar

### <a name="pricing-tiers"></a>Fiyatlandırma katmanları

Okuma yinelemeleri şu anda yalnızca Genel Amaç ve Bellek Optimize Edilmiş fiyatlandırma katmanlarında kullanılabilir.

### <a name="master-server-restart"></a>Ana sunucu yeniden başlatma

Varolan yinelemeleri olmayan bir ana için bir yineleme oluşturduğunuzda, asıl önce kendisini çoğaltma için hazırlamak için yeniden başlatılacaktır. Bunu göz önünde bulundurun ve yoğun olmayan bir dönemde bu işlemleri gerçekleştirin.

### <a name="new-replicas"></a>Yeni yinelemeler

Okuma yinelemesi MySQL sunucusu için yeni bir Azure Veritabanı olarak oluşturulur. Varolan bir sunucu çoğaltma haline getirilebilir. Başka bir okuma yinelemesinin kopyasını oluşturamazsınız.

### <a name="replica-configuration"></a>Yineleme yapılandırması

Yineleme, ana bilgisayarla aynı sunucu yapılandırması kullanılarak oluşturulur. Yineleme oluşturulduktan sonra, ana sunucudan bağımsız olarak birkaç ayar değiştirilebilir: bilgi işlem oluşturma, vCores, depolama ve yedekleme bekletme süresi. Fiyatlandırma katmanı, Temel katman alabilen veya temel katmandan bağımsız olarak değiştirilebilir.

> [!IMPORTANT]
> Ana sunucu yapılandırması yeni değerlere güncelleştirilmeden önce, çoğaltma yapılandırmasını eşit veya daha büyük bir değere güncelleştirin. Bu eylem, çoğaltmanın ana kopya üzerinde yapılan değişiklikleri yansıtmasını sağlar.

Güvenlik duvarı kuralları ve parametre ayarları, yineleme oluşturulduğunda ana sunucudan yinelemeye devralınır. Daha sonra, yinelemenin kuralları bağımsızdır.

### <a name="stopped-replicas"></a>Durdurulan yinelemeler

Ana sunucu ile okuma yinelemesi arasındaki çoğaltmayı durdurursanız, durdurulan yineleme hem okumaları hem de yazmaları kabul eden bağımsız bir sunucu olur. Bağımsız sunucu yeniden çoğaltma yapılamaz.

### <a name="deleted-master-and-standalone-servers"></a>Silinmiş ana ve bağımsız sunucular

Ana sunucu silindiğinde, çoğaltma tüm okunan yinelemeler için durdurulur. Bu yinelemeler otomatik olarak bağımsız sunucular haline gelir ve hem okumaları hem de yazmaları kabul edebilir. Ana sunucunun kendisi silinir.

### <a name="user-accounts"></a>Kullanıcı hesapları

Ana sunucudaki kullanıcılar, okunan yinelemelere kopyalanır. Yalnızca ana sunucuda bulunan kullanıcı hesaplarını kullanarak okuma yinelemesine bağlanabilirsiniz.

### <a name="server-parameters"></a>Sunucu parametreleri

Verilerin eşitlenmemiş duruma gelmesini ve olası veri kaybını önlemek için, okuma amaçlı çoğaltmaları kullanırken bazı sunucu parametreleri güncelleştirmeye karşı kilitlenir.

Aşağıdaki sunucu parametreleri hem ana hem de yineleme sunucularında kilitlenir:
- [`innodb_file_per_table`](https://dev.mysql.com/doc/refman/5.7/en/innodb-multiple-tablespaces.html) 
- [`log_bin_trust_function_creators`](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html#sysvar_log_bin_trust_function_creators)

Parametre [`event_scheduler`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_event_scheduler) çoğaltma sunucularında kilitli. 

### <a name="other"></a>Diğer

- Genel işlem tanımlayıcıları (GTID) desteklenmez.
- Yinelemenin bir kopyasını oluşturma desteklenmez.
- Bellek içi tablolar yinelemelerin eşitlenmemiş olmasına neden olabilir. Bu MySQL çoğaltma teknolojisinin bir sınırlamasıdır. Daha fazla bilgi için [MySQL başvuru belgelerinde](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html) daha fazla bilgi edinin.
- Ana sunucu tablolarının birincil anahtarlara sahip olduğundan emin olun. Birincil anahtarların eksikliği, ana ve yinelemeler arasında çoğaltma gecikmesi ile sonuçlanabilir.
- [MySQL belgelerindeki](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html) MySQL çoğaltma sınırlamalarının tam listesini gözden geçirin

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını kullanarak okuma yinelemelerini](howto-read-replicas-portal.md) nasıl oluşturup yöneteceklerini öğrenin
- [Azure CLI ve REST API'sini kullanarak okuma yinelemelerini nasıl oluşturup yöneteceğimiz](howto-read-replicas-cli.md) öğrenin
