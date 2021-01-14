---
title: Çoğaltmaları oku-MySQL için Azure veritabanı
description: "MySQL için Azure veritabanı 'nda çoğaltmaları okuma hakkında bilgi edinin: bölge seçme, çoğaltmalar oluşturma, çoğaltmalara bağlanma, çoğaltmayı izleme ve çoğaltmayı durdurma."
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2021
ms.custom: references_regions
ms.openlocfilehash: f4a97f5534e4fd3847bf1cce6874de0f006cce38
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201017"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı’nda okuma amaçlı çoğaltmalar

Okuma amaçlı çoğaltma özelliği, MySQL için Azure Veritabanı’ndan salt okunur bir sunucuya verileri çoğaltmanıza olanak sağlar. Kaynak sunucudan en fazla beş çoğaltmaya çoğaltabilirsiniz. Çoğaltmalar, MySQL altyapısının yerel ikili günlük (binlog) dosya konumu tabanlı çoğaltma teknolojisini kullanılarak zaman uyumsuz olarak güncelleştirilir. Binlog çoğaltma hakkında daha fazla bilgi edinmek için [MySQL binlog çoğaltmasına genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html)bölümüne bakın.

Çoğaltmalar, yönettiğiniz yeni sunuculardan MySQL için normal Azure veritabanı sunucularına benzer. Her okuma çoğaltması için, sanal çekirdekler ve depolama biriminde GB/ay içinde sağlanan işlem için faturalandırılırsınız.

MySQL çoğaltma özellikleri ve sorunları hakkında daha fazla bilgi edinmek için [MySQL Çoğaltma belgelerine](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html)bakın.

> [!NOTE]
> Sapma ücretsiz iletişim
>
> Microsoft, farklı ve üçlü ortamları destekler. Bu makale _ana_ ve _bağımlı_ sözcüklere başvurular içerir. Kullanım açısından [ücretsiz iletişim Için Microsoft Stil Kılavuzu](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) , bu sözcükleri exclusionary kelimeleri olarak tanır. Bu makalede, şu anda yazılımda görüntülenen sözcükler olduklarından, bu makale tutarlılık için kullanılır. Yazılım, kelimeleri kaldırmak üzere güncelleniyorsa, bu makale hizalamayla olacak şekilde güncelleştirilir.
>

## <a name="when-to-use-a-read-replica"></a>Okuma çoğaltması ne zaman kullanılır?

Okuma çoğaltması özelliği, okuma yoğunluklu iş yüklerinin performansını ve ölçeğini artırmaya yardımcı olur. Okuma iş yükleri çoğaltmalar için yalıtılabilir, yazma iş yükleri kaynağa yönlendirilebilir.

Yaygın bir senaryo, bı ve analitik iş yüklerinin raporlama için veri kaynağı olarak okuma çoğaltmasını kullanmasını sağlar.

Çoğaltmalar salt okunurdur, kaynak üzerindeki yazma kapasitesini doğrudan azaltmazlar. Bu özellik, yazma açısından yoğun iş yükleri için uygun değildir.

Okuma çoğaltması özelliği MySQL zaman uyumsuz çoğaltma kullanır. Özelliği, zaman uyumlu çoğaltma senaryolarına yönelik değildir. Kaynak ve çoğaltma arasında ölçülebilir bir gecikme olacaktır. Çoğaltılan veriler, sonunda kaynaktaki verilerle tutarlı hale gelir. Bu gecikmeyi barındırabilecek iş yükleri için bu özelliği kullanın.

## <a name="cross-region-replication"></a>Bölgeler arası çoğaltma

Kaynak sunucunuzdaki farklı bir bölgede bir okuma çoğaltması oluşturabilirsiniz. Çapraz bölge çoğaltma, olağanüstü durum kurtarma planlaması veya kullanıcılarınıza daha yakın veri getirme gibi senaryolar için yararlı olabilir.

[MySQL Için Azure veritabanı bölgesinde](https://azure.microsoft.com/global-infrastructure/services/?products=mysql)bir kaynak sunucusuna sahip olabilirsiniz.  Kaynak sunucu, eşleştirilmiş bölgede veya evrensel çoğaltma bölgelerinde bir çoğaltmaya sahip olabilir. Aşağıdaki resimde, kaynak bölgenize göre hangi çoğaltma bölgelerinin kullanılabildiği gösterilmektedir.

[:::image type="content" source="media/concepts-read-replica/read-replica-regions.png" alt-text="Çoğaltma bölgelerini oku":::](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>Evrensel çoğaltma bölgeleri

Kaynak sunucunuzun bulunduğu yere bakılmaksızın, aşağıdaki bölgelerin herhangi birinde bir okuma çoğaltması oluşturabilirsiniz. Desteklenen evrensel çoğaltma bölgeleri şunları içerir:

Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta ABD, Doğu Asya, Doğu ABD, Doğu ABD 2, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Orta Kuzey ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, UK Güney, UK Batı, Batı Avrupa, Batı ABD, Batı ABD 2, Orta Batı ABD.

### <a name="paired-regions"></a>Eşleştirilmiş bölgeler

Evrensel çoğaltma bölgelerine ek olarak, kaynak sunucunuzun Azure eşlenmiş bölgesinde bir okuma çoğaltması oluşturabilirsiniz. Bölgenizin çiftini bilmiyorsanız [Azure eşlenmiş bölgeler makalesinden](../best-practices-availability-paired-regions.md)daha fazla bilgi edinebilirsiniz.

Olağanüstü durum kurtarma planlaması için çapraz bölge çoğaltmaları kullanıyorsanız, çoğaltmayı diğer bölgelerden biri yerine eşleştirilmiş bölgede oluşturmanızı öneririz. Eşleştirilmiş bölgeler eşzamanlı güncelleştirmeleri önleyin ve fiziksel yalıtımın ve veri fazlalığını önceliklendirir.  

Ancak göz önünde bulundurulması gereken sınırlamalar vardır: 

* Bölgesel kullanılabilirlik: MySQL için Azure veritabanı Fransa Orta, BAE Kuzey ve Almanya Orta sunulmaktadır. Ancak, eşleştirilmiş bölgeleri kullanılamaz.

* Tek yönlü çiftler: bazı Azure bölgeleri yalnızca bir yönde eşleştirilmelidir. Bu bölgeler Batı Hindistan, Brezilya Güney ve US Gov Virginia içerir.
   Bu, Batı Hindistan içindeki bir kaynak sunucunun Güney Hindistan bir çoğaltma oluşturmasıdır. Ancak, Güney Hindistan bir kaynak sunucu Batı Hindistan bir çoğaltma oluşturamaz. Bunun nedeni, Batı Hindistan ikincil bölgesinin Güney Hindistan, ancak Güney Hindistan ikincil bölgesi Batı Hindistan değildir.

## <a name="create-a-replica"></a>Çoğaltma oluşturma

> [!IMPORTANT]
> Çoğaltma oku özelliği yalnızca Genel Amaçlı veya bellek için Iyileştirilmiş fiyatlandırma katmanlarında MySQL için Azure veritabanı sunucuları için kullanılabilir. Kaynak sunucunun bu fiyatlandırma katmanlarından birinde olduğundan emin olun.

Kaynak sunucuda var olan bir çoğaltma sunucusu yoksa, kaynak ilk olarak çoğaltma için hazırlamak üzere yeniden başlatılır.

Çoğaltma oluşturma iş akışı 'nı başlattığınızda, MySQL için boş bir Azure veritabanı sunucusu oluşturulur. Yeni sunucu, kaynak sunucuda bulunan verilerle doldurulur. Oluşturma süresi, kaynaktaki veri miktarına ve son haftalık tam yedeklemeden bu yana geçen zamana bağlıdır. Süre, birkaç dakika ile birkaç saat arasında değişebilir. Çoğaltma sunucusu her zaman aynı kaynak grubunda ve kaynak sunucuyla aynı abonelikte oluşturulur. Farklı bir kaynak grubuna veya farklı bir aboneliğe çoğaltma sunucusu oluşturmak istiyorsanız, oluşturulduktan sonra [Çoğaltma sunucusunu taşıyabilirsiniz](../azure-resource-manager/management/move-resource-group-and-subscription.md) .

Her çoğaltma, depolama [otomatik büyüme](concepts-pricing-tiers.md#storage-auto-grow)için etkinleştirilir. Otomatik büyüme özelliği, çoğaltmanın kendisine çoğaltılan verilerle kalmasına izin verir ve çoğaltma dışı hatalardan kaynaklanan bir kesintiye uğramasını önler.

[Azure Portal bir okuma çoğaltması oluşturmayı](howto-read-replicas-portal.md)öğrenin.

## <a name="connect-to-a-replica"></a>Bir çoğaltmaya bağlanma

Oluşturma sırasında bir çoğaltma, kaynak sunucunun güvenlik duvarı kurallarını devralır. Daha sonra, bu kurallar kaynak sunucudan bağımsızdır.

Çoğaltma, yönetim hesabını kaynak sunucudan devralır. Kaynak sunucudaki tüm Kullanıcı hesapları, okuma çoğaltmalarına çoğaltılır. Bir okuma çoğaltmasına yalnızca kaynak sunucuda bulunan Kullanıcı hesaplarını kullanarak bağlanabilirsiniz.

Bir MySQL için Azure veritabanı sunucusunda yaptığınız gibi, ana bilgisayar adını ve geçerli bir kullanıcı hesabını kullanarak çoğaltmaya bağlanabilirsiniz. Yönetici Kullanıcı adı **myadmin** olan **myreplica** adlı BIR sunucu için MySQL CLI kullanarak çoğaltmaya bağlanabilirsiniz:

```bash
mysql -h myreplica.mysql.database.azure.com -u myadmin@myreplica -p
```

İstemde, Kullanıcı hesabının parolasını girin.

## <a name="monitor-replication"></a>Çoğaltmayı izleme

MySQL için Azure veritabanı, Azure Izleyici 'de **saniye cinsinden yineleme gecikmesi** sağlar. Bu ölçüm yalnızca çoğaltmalar için kullanılabilir. Bu ölçüm, `seconds_behind_master` MySQL 'in komutunda kullanılabilir olan ölçüm kullanılarak hesaplanır `SHOW SLAVE STATUS` . Çoğaltma gecikmesi iş yükünüz için kabul edilebilir bir değere ulaştığında sizi bilgilendirmek için bir uyarı ayarlayın.

Daha fazla çoğaltma gecikmesi görürseniz, sorunun giderilmesi ve olası nedenleri anlamak için [sorun giderme çoğaltma gecikmesini](howto-troubleshoot-replication-latency.md) inceleyin.

## <a name="stop-replication"></a>Çoğaltmayı durdurma

Kaynak ve çoğaltma arasında çoğaltmayı durdurabilirsiniz. Kaynak sunucu ve okuma çoğaltması arasında çoğaltma durdurulduktan sonra, çoğaltma tek başına bir sunucu haline gelir. Tek başına sunucusundaki veriler, çoğaltma durdurma komutunun başlatıldığı zamanda çoğaltma üzerinde kullanılabilir olan veri. Tek başına sunucu, kaynak sunucuyla ilgili değildir.

Çoğaltma çoğaltmasını durdurmayı seçtiğinizde, önceki kaynak ve diğer çoğaltmaların tüm bağlantılarını kaybeder. Kaynak ve çoğaltma arasında otomatik yük devretme yoktur.

> [!IMPORTANT]
> Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.
> Bir okuma çoğaltmasında çoğaltmayı durdurmadan önce, çoğaltmanın gerekli tüm verilere sahip olduğundan emin olun.

[Bir çoğaltmaya çoğaltmayı durdurmayı](howto-read-replicas-portal.md)öğrenin.

## <a name="failover"></a>Yük devretme

Kaynak ve çoğaltma sunucuları arasında otomatik yük devretme yoktur.

Çoğaltma zaman uyumsuz olduğundan, kaynak ve çoğaltma arasında bir gecikme olur. Gecikme miktarı, kaynak sunucuda çalışan iş yükünün ne kadar ağır ve veri merkezleri arasındaki gecikme süresi gibi birçok faktörden etkilenebilir. Çoğu durumda, çoğaltma gecikmesi birkaç saniyeyle birkaç dakika arasında değişir. Her bir çoğaltma için kullanılabilen ölçüm *çoğaltması* gecikmesini kullanarak gerçek çoğaltma gecikmelerinizi izleyebilirsiniz. Bu ölçüm, son yeniden yürütülmüş işlemden bu yana geçen süreyi gösterir. Yineleme gecikmesini bir süre içinde gözlemleyerek ortalama gecikmenizin ne olduğunu tanımlamanızı öneririz. Çoğaltma gecikmesi üzerinde bir uyarı ayarlayabilirsiniz, böylece beklenen aralığın dışında olursa işlem yapabilirsiniz.

> [!Tip]
> Çoğaltmaya yük devretmek, çoğaltmayı kaynaktan geri bağladığınızda oluşan gecikme süresi, ne kadar veri kaybı olduğunu gösterir.

Bir çoğaltmaya yük devretmeye karar verdikten sonra:

1. Çoğaltmaya çoğaltmayı durdur<br/>
   Bu adım, çoğaltma sunucusunun yazmaları kabul edebilmesini sağlamak için gereklidir. Bu işlemin bir parçası olarak, çoğaltma sunucusu kaynaktan kaldırılacak. Çoğaltmayı durdur işlemini başlattıktan sonra, arka uç işleminin genellikle yaklaşık 2 dakika sürer. Bu eylemin etkilerini anlamak için bu makalenin [çoğaltmayı durdur](#stop-replication) bölümüne bakın.

2. Uygulamanızı (eski) çoğaltmaya işaret edin<br/>
   Her sunucunun benzersiz bir bağlantı dizesi vardır. Uygulamanızı kaynak yerine (eski) çoğaltmaya işaret etmek üzere güncelleştirin.

Uygulamanız okuma ve yazma işlemlerini başarıyla tamamladıktan sonra, yük devretmeyi tamamladınız. Bir sorunu saptadığınızda ve daha önce listelenen 1. ve 2. adımları tamamladıktan sonra, uygulama deneyimlerinizin ne kadar süre içinde olduğuna bağlı olacaktır.

## <a name="global-transaction-identifier-gtid"></a>Genel işlem tanımlayıcısı (GTıD)

Genel işlem tanımlayıcısı (GTıD), bir kaynak sunucuda yürütülen her işlemle oluşturulmuş benzersiz bir tanımlayıcıdır ve MySQL için Azure veritabanı 'nda varsayılan olarak KAPALıDıR. GTıD, 5,7 ve 8,0 sürümlerinde ve yalnızca 16 TB 'a kadar depolamayı destekleyen sunucularda desteklenir. GTıD hakkında daha fazla bilgi edinmek ve nasıl kullanılacağı hakkında daha fazla bilgi edinmek için, MySQL 'in [gtıd belgeleriyle çoğaltma](https://dev.mysql.com/doc/refman/5.7/en/replication-gtids.html) bölümüne bakın.

MySQL iki tür işlemi destekler: GTıD işlemleri (GTıD ile tanımlanır) ve anonim işlemler (ayrılmış bir GTıD 'ye sahip değil)

Aşağıdaki sunucu parametreleri GTıD 'yi yapılandırmak için kullanılabilir: 

|**Sunucu parametresi**|**Açıklama**|**Varsayılan değer**|**Değerler**|
|--|--|--|--|
|`gtid_mode`|İşlemleri tanımlamak için Gtıds 'nin kullanıldığını gösterir. Modlar arasındaki değişiklikler, her seferinde artan düzende (örn. bir adım yapılabilir) gerçekleştirilebilir. `OFF` -> `OFF_PERMISSIVE` -> `ON_PERMISSIVE` -> `ON`)|`OFF`|`OFF`: Hem yeni hem de çoğaltma işlemleri anonim olmalıdır <br> `OFF_PERMISSIVE`: Yeni işlemler anonimdir. Çoğaltılan işlemler anonim ya da GTıD işlemleri olabilir. <br> `ON_PERMISSIVE`: Yeni işlemler GTıD işlemlerdir. Çoğaltılan işlemler anonim ya da GTıD işlemleri olabilir. <br> `ON`: Hem yeni hem de çoğaltılan işlemler GTıD işlemleri olmalıdır.|
|`enforce_gtid_consistency`|Yalnızca işlemsel olarak güvenli bir şekilde oturum açılabilen deyimlerin yürütülmesine izin vererek GTıD tutarlılığını zorlar. Bu değer, `ON` gtıd çoğaltmasını etkinleştirmeden önce olarak ayarlanmalıdır. |`OFF`|`OFF`: Tüm işlemlerin GTıD tutarlılığını ihlal edebileceği şekilde izin verilir.  <br> `ON`: Hiçbir işlemin GTıD tutarlılığını ihlal etme izni yok. <br> `WARN`: Tüm işlemlere GTıD tutarlılığı ihlal etmek için izin verilir, ancak bir uyarı oluşturulur. | 

> [!NOTE]
> GTıD etkinleştirildikten sonra, yeniden kapatamaz. GTıD 'yi kapatmanız gerekiyorsa lütfen desteğe başvurun. 

GTıD 'yi etkinleştirmek ve tutarlılık davranışını yapılandırmak için `gtid_mode` `enforce_gtid_consistency` [Azure Portal](howto-server-parameters.md), [Azure CLI](howto-configure-server-parameters-using-cli.md)veya [PowerShell](howto-configure-server-parameters-using-powershell.md)'i kullanarak ve sunucu parametrelerini güncelleştirin.

Bir kaynak sunucuda (= on) GTıD etkinse `gtid_mode` , yeni oluşturulan çoğaltmalarda de gtıd etkinleştirilir ve gtıd çoğaltmasını kullanılır. Çoğaltmanın tutarlılığını sağlamak için `gtid_mode` kaynak veya Çoğaltma sunucusunda güncelleştiremezsiniz.

## <a name="considerations-and-limitations"></a>Önemli noktalar ve sınırlamalar

### <a name="pricing-tiers"></a>Fiyatlandırma katmanları

Okuma çoğaltmaları Şu anda yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş fiyatlandırma katmanlarında kullanılabilir.

> [!NOTE]
> Çoğaltma sunucusunu çalıştırmanın maliyeti, çoğaltma sunucusunun çalıştığı bölgeyi temel alır.

### <a name="source-server-restart"></a>Kaynak sunucu yeniden başlatması

Mevcut çoğaltmaları olmayan bir kaynak için çoğaltma oluşturduğunuzda, kaynak ilk olarak çoğaltma için hazırlamak üzere yeniden başlatılır. Bunu dikkate alın ve yoğun bir süre boyunca bu işlemleri gerçekleştirin.

### <a name="new-replicas"></a>Yeni çoğaltmalar

Bir okuma çoğaltması, MySQL için yeni bir Azure veritabanı sunucusu olarak oluşturulur. Var olan bir sunucu bir çoğaltmaya yapılamaz. Başka bir okuma çoğaltmasının çoğaltmasını oluşturamazsınız.

### <a name="replica-configuration"></a>Çoğaltma yapılandırması

Bir çoğaltma, kaynakla aynı sunucu yapılandırması kullanılarak oluşturulur. Bir çoğaltma oluşturulduktan sonra, kaynak sunucudan bağımsız olarak çeşitli ayarlar değiştirilebilir: işlem oluşturma, sanal çekirdek, depolama ve yedekleme saklama süresi. Fiyatlandırma Katmanı, temel katmandan veya dışında bağımsız olarak da değiştirilebilir.

> [!IMPORTANT]
> Kaynak sunucu yapılandırması yeni değerlere güncelleştirilmeden önce, çoğaltma yapılandırmasını eşit veya daha büyük değerlerle güncelleştirin. Bu eylem, çoğaltmanın kaynak kopya üzerinde yapılan değişiklikleri yansıtmasını sağlar.

Çoğaltma oluşturulduğunda, güvenlik duvarı kuralları ve parametre ayarları kaynak sunucudan çoğaltmaya devralınır. Daha sonra, çoğaltmanın kuralları bağımsızdır.

### <a name="stopped-replicas"></a>Durdurulan çoğaltmalar

Kaynak sunucuyla bir okuma çoğaltması arasında çoğaltmayı durdurursanız, durdurulan çoğaltma hem okuma hem de yazma işlemlerini kabul eden tek başına bir sunucu haline gelir. Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.

### <a name="deleted-source-and-standalone-servers"></a>Kaynak ve tek başına sunucular silindi

Bir kaynak sunucu silindiğinde, çoğaltma tüm okuma çoğaltmalarına durdurulur. Bu çoğaltmalar otomatik olarak tek başına sunucular olur ve hem okuma hem de yazma işlemlerini kabul edebilir. Kaynak sunucunun kendisi silinir.

### <a name="user-accounts"></a>Kullanıcı hesapları

Kaynak sunucudaki kullanıcılar okuma çoğaltmalarına çoğaltılır. Bir okuma çoğaltmasına yalnızca kaynak sunucuda bulunan Kullanıcı hesaplarını kullanarak bağlanabilirsiniz.

### <a name="server-parameters"></a>Sunucu parametreleri

Verilerin eşitlenmemiş duruma gelmesini ve olası veri kaybını önlemek için, okuma amaçlı çoğaltmaları kullanırken bazı sunucu parametreleri güncelleştirmeye karşı kilitlenir.

Aşağıdaki sunucu parametreleri hem kaynak hem de çoğaltma sunucularında kilitlidir:

* [`innodb_file_per_table`](https://dev.mysql.com/doc/refman/8.0/en/innodb-file-per-table-tablespaces.html) 
* [`log_bin_trust_function_creators`](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html#sysvar_log_bin_trust_function_creators)

[`event_scheduler`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_event_scheduler)Parametresi, çoğaltma sunucularında kilitlidir.

Kaynak sunucuda yukarıdaki parametrelerden birini güncelleştirmek için, çoğaltma sunucularını silin, kaynaktaki parametre değerini güncelleştirin ve çoğaltmaları yeniden oluşturun.

### <a name="gtid"></a>GTıD

GTıD desteklenir:

* MySQL sürüm 5,7 ve 8,0.
* 16 TB 'a kadar depolamayı destekleyen sunucular. 16 TB depolamayı destekleyen bölgelerin tam listesi için [fiyatlandırma katmanı](concepts-pricing-tiers.md#storage) makalesine başvurun.

GTıD varsayılan olarak KAPALıDıR. GTıD etkinleştirildikten sonra, yeniden açamazsınız. GTıD 'yi kapatmanız gerekirse desteğe başvurun.

Bir kaynak sunucuda GTıD etkinse, yeni oluşturulan çoğaltmalarda de GTıD etkinleştirilir ve GTıD çoğaltmasını kullanacaktır. Çoğaltmanın tutarlılığını sağlamak için `gtid_mode` kaynak veya Çoğaltma sunucusunda güncelleştiremezsiniz.

### <a name="other"></a>Diğer

* Bir çoğaltmanın çoğaltmasını oluşturma desteklenmez.
* Bellek içi tablolar çoğaltmaların eşitlenmemiş hale gelmesine neden olabilir. Bu, MySQL Çoğaltma teknolojisinin bir sınırlamasıdır. Daha fazla bilgi için [MySQL Reference belgelerindeki](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html) daha fazla bilgi edinin.
* Kaynak sunucu tablolarının birincil anahtarlara sahip olduğundan emin olun. Birincil anahtarların olmaması, kaynak ve çoğaltmalar arasında çoğaltma gecikmesine neden olabilir.
* [MySQL belgelerindeki](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html) MySQL Çoğaltma sınırlamalarının tam listesini gözden geçirin

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Portal kullanarak okuma çoğaltmaları oluşturmayı ve yönetmeyi](howto-read-replicas-portal.md) öğrenin
* [Azure CLI ve REST API kullanarak okuma çoğaltmaları oluşturma ve yönetme](howto-read-replicas-cli.md) hakkında bilgi edinin