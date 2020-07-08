---
title: Azure SQL veritabanı hiper ölçek SSS
description: Yaygın soruların yanıtları-genellikle hiper ölçekli bir veritabanı olarak adlandırılan hiper ölçekli hizmet katmanındaki SQL veritabanında bir veritabanı hakkında bilgi ister.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: dimitri-furman
ms.author: dfurman
ms.reviewer: ''
ms.date: 03/03/2020
ms.openlocfilehash: d5faa08b0b493ac9b7fc03cd15c4747c3e602f02
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84049921"
---
# <a name="azure-sql-database-hyperscale-faq"></a>Azure SQL veritabanı hiper ölçek SSS
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu makalede, bu SSS 'nin geri kalanında yalnızca Hiperscale olarak adlandırılan Azure SQL veritabanı hiper ölçek hizmeti katmanında bir veritabanını ele almak için sık sorulan soruların yanıtları sunulmaktadır. Bu makalede, Hyperscale 'nin desteklediği senaryolar ve Hyperscale ile uyumlu özellikler açıklanmaktadır.

- Bu SSS, hiper ölçek hizmet katmanını kısa bir şekilde anlayabilen ve belirli soruların ve kaygılarının cevaplanmasını isteyen okuyucular için hazırlanmıştır.
- Bu SSS, bir kılavuz kitabı olması veya bir hiper ölçekli veritabanının nasıl kullanılacağına ilişkin sorulara yanıt vermesi anlamına gelir. Hiper ölçeğe giriş için [Azure SQL veritabanı hiper ölçek](service-tier-hyperscale.md) belgelerine başvurmanız önerilir.

## <a name="general-questions"></a>Genel sorular

### <a name="what-is-a-hyperscale-database"></a>Hiper ölçekli veritabanı nedir?

Hiper ölçekli veritabanı, hiper ölçek genişleme depolama teknolojisi tarafından desteklenen hiper ölçek hizmet katmanındaki SQL veritabanı 'ndaki bir veritabanıdır. Bir Hyperscale veritabanı en fazla 100 TB veri destekler ve yüksek aktarım hızı ve performans sağlar ve iş yükü gereksinimlerine uyum sağlamak için hızlı ölçeklendirmeye olanak tanır. Ölçeklendirme, uygulama bağlantısı, sorgu işleme vb. için, Azure SQL veritabanı 'ndaki diğer veritabanları gibi çalışır.

### <a name="what-resource-types-and-purchasing-models-support-hyperscale"></a>Hiper ölçeklendirmeyi destekleyen kaynak türleri ve satın alma modelleri

Hyperscale hizmet katmanı yalnızca Azure SQL veritabanı 'ndaki sanal çekirdek tabanlı satın alma modeli kullanılarak tek veritabanlarında kullanılabilir.  

### <a name="how-does-the-hyperscale-service-tier-differ-from-the-general-purpose-and-business-critical-service-tiers"></a>Hiper ölçek hizmet katmanının Genel Amaçlı ve İş Açısından Kritik hizmet katmanlarından farkı nedir?

Sanal çekirdek tabanlı hizmet katmanları, aşağıdaki tabloda açıklandığı gibi veritabanı kullanılabilirliğine ve depolama türüne, performansa ve en büyük boyuta göre farklılaştırılır.

| | Kaynak türü | Genel Amaçlı |  Hiper Ölçek | İş Açısından Kritik |
|:---:|:---:|:---:|:---:|:---:|
| **En iyisi** |Tümü|Bütçe odaklı dengeli işlem ve depolama seçenekleri sunar.|Birçok iş yükü. Otomatik ölçeklendirme depolama boyutu 100 TB 'ye kadar, hızlı dikey ve yatay işlem ölçekleme, hızlı veritabanı geri yükleme.|Yüksek işlem hızına sahip OLTP uygulamaları ve düşük GÇ gecikme süresi. Birden çok zaman uyumlu olarak güncellenen çoğaltmaları kullanarak hatalara en yüksek esnekliği ve hızlı yük devretme olanağı sunar.|
|  **Kaynak türü** ||Tek veritabanı/elastik havuz/yönetilen örnek | Tek veritabanı | Tek veritabanı/elastik havuz/yönetilen örnek |
| **İşlem boyutu**|Tek veritabanı/elastik havuz * | 1-80 sanal çekirdek | 1-80 sanal çekirdek * | 1-80 sanal çekirdek |
| |SQL Yönetilen Örnek | 8, 16, 24, 32, 40, 64, 80 Vçekirdekler | YOK | 8, 16, 24, 32, 40, 64, 80 Vçekirdekler |
| **Depolama türü** | Tümü |Premium uzak depolama (örnek başına) | Yerel SSD Önbelleği (örnek başına) ile birlikte bağlanmış depolama | Süper hızlı yerel SSD depolaması (örnek başına) |
| **Depolama boyutu** | Tek veritabanı/elastik havuz *| 5 GB – 4 TB | 100 TB 'a kadar | 5 GB – 4 TB |
| | SQL Yönetilen Örnek  | 32 GB – 8 TB | YOK | 32 GB – 4 TB |
| **IOPS** | Tek veritabanı | maksimum 7000 ıOPS ile vCore başına 500 ıOPS | Hiper ölçek, birden çok düzeyde önbelleğe alma özelliği olan çok katmanlı bir mimaridir. Etkin ıOPS iş yüküne bağlı olacaktır. | 5000 ıOPS, en fazla 200.000 ıOPS|
| | SQL Yönetilen Örnek | Dosya boyutuna bağlıdır | YOK | 1375 ıOPS/sanal çekirdek |
|**Kullanılabilirlik**|Tümü|1 çoğaltma, okuma ölçeği yok, yerel önbellek yok | Birden çok çoğaltma, en fazla 4 okuma ölçeği genişletme, kısmi yerel önbellek | 3 çoğaltma, 1 okuma ölçeği genişletme, bölge yedekli HA, tam yerel depolama |
|**Yedeklemeler**|Tümü|RA-GRS, 7-35 gün bekletme (varsayılan olarak 7 gün)| RA-GRS, 7 günlük bekletme, sabit zaman zaman aşımı kurtarma (sür) | RA-GRS, 7-35 gün bekletme (varsayılan olarak 7 gün) |

\*Esnek havuzlar hiper ölçek hizmeti katmanında desteklenmez

### <a name="who-should-use-the-hyperscale-service-tier"></a>Hiper ölçek hizmet katmanını kimler kullanmalıdır?

Hyperscale hizmet katmanı, büyük şirket içi SQL Server veritabanlarına sahip olan ve buluta geçerek uygulamalarına modernleştirin veya zaten Azure SQL veritabanı kullanan müşteriler ve veritabanı büyümesi için potansiyelini önemli ölçüde genişletmek isteyen müşterilere yöneliktir. Hiper ölçek, hem yüksek performans hem de yüksek ölçeklenebilirliğe sahip müşteriler için de tasarlanmıştır. Hiperscale ile şunları alırsınız:

- Veritabanı boyutu 100 TB 'a kadar
- Veritabanı boyutundan bağımsız olarak hızlı veritabanı yedeklemeleri (yedeklemeler depolama anlık görüntülerine dayalıdır)
- Veritabanı boyutundan bağımsız olarak hızlı veritabanı geri yüklenir (geri yüklemeler depolama anlık görüntülerinden alınır)
- Veritabanı boyutundan ve sanal çekirdek sayısından bağımsız olarak daha yüksek günlük aktarım hızı
- Okuma yükü boşaltma ve sık erişimli olarak kullanılan bir veya daha fazla salt okuma çoğaltması kullanarak ölçeği okuyun.
- Sabit zamanlı olarak hızlı bir şekilde işleme, ağır iş yüküne uyum sağlamak ve daha sonra ölçeği sabit bir zamanda genişletmek daha güçlü olacak şekilde. Bu, örneğin bir P6 ve P11 arasında ölçeği artırma ve azaltma ile benzerdir, ancak bu, veri işleme boyutu olmadığından çok daha hızlıdır.

### <a name="what-regions-currently-support-hyperscale"></a>Şu anda hiper ölçeklendirmeyi destekleyen bölgeler

Hiper ölçek hizmeti katmanı Şu anda [Azure SQL veritabanı hiper ölçek](service-tier-hyperscale.md#regions)altında listelenen bölgelerde kullanılabilir.

### <a name="can-i-create-multiple-hyperscale-databases-per-server"></a>Sunucu başına birden çok hiper ölçekli veritabanı oluşturabilir miyim

Evet. Sunucu başına hiper ölçek veritabanlarının sayısı hakkında daha fazla bilgi ve sınırlamalar için bkz. [bir sunucudaki tek ve havuza alınmış veritabanları Için SQL veritabanı kaynak sınırları](resource-limits-logical-server.md).

### <a name="what-are-the-performance-characteristics-of-a-hyperscale-database"></a>Hiper ölçek veritabanının performans özellikleri nelerdir

Hiper ölçek mimarisi, büyük veritabanı boyutlarını desteklerken yüksek performans ve verimlilik sağlar.

### <a name="what-is-the-scalability-of-a-hyperscale-database"></a>Hiper ölçek veritabanının ölçeklenebilirliği nedir?

Hiper ölçek, iş yükü talebe göre hızlı ölçeklenebilirlik sağlar.

- **Ölçeği artırma/azaltma**

  Hyperscale ile, birincil işlem boyutunu CPU ve bellek gibi kaynaklar açısından ölçeklendirebilir ve sonra ölçeği sabit bir süre içinde azaltabilirsiniz. Depolama paylaşıldığından ölçeği büyütme ve küçültme, veri işleme bir boyut değildir.  
- **Ölçeği genişletme/genişletme**

  Hiperscale ile, okuma isteklerinizi karşılamak için kullanabileceğiniz bir veya daha fazla ek işlem çoğaltması sağlama olanağı da alırsınız. Yani, bu ek bilgi işlem çoğaltmalarını, birincil işlem üzerinden okuma iş yükünüzü boşaltmak için salt okuma çoğaltmaları olarak kullanabilirsiniz. Salt okuma özelliklerine ek olarak, bu çoğaltmalar Ayrıca birincil sunucudan yük devretme durumunda sık erişimli olarak da çalışır.

  Bu ek bilgi işlem çoğaltmalarının sağlanması, sabit zamanlı olarak yapılabilir ve çevrimiçi bir işlemdir. `ApplicationIntent`Bağlantı dizinizdeki bağımsız değişkeni olarak ayarlayarak, bu ek salt okuma işlem çoğaltmalarına bağlanabilirsiniz `ReadOnly` . Uygulama amacına sahip tüm bağlantılar `ReadOnly` otomatik olarak ek salt okuma işlem Çoğaltmalarından birine yönlendirilir.

## <a name="deep-dive-questions"></a>Derinlemesine bakış soruları

### <a name="can-i-mix-hyperscale-and-single-databases-in-a-single-server"></a>Tek bir sunucuda hiper ölçek ve tek veritabanlarını karıştırabilirim

Evet, bunu yapabilirsiniz.

### <a name="does-hyperscale-require-my-application-programming-model-to-change"></a>Hiper ölçek, uygulama programlama modelinizin değiştirilmesini gerektiriyor

Hayır, uygulama programlama modeliniz olduğu gibi kalır. Bağlantı dizenizi her zamanki gibi, hiper ölçekli veritabanlarınızla etkileşimde bulunmak için de normal yöntemlerle kullanırsınız.

### <a name="what-transaction-isolation-level-is-the-default-in-a-hyperscale-database"></a>Hiper ölçek veritabanında varsayılan değer olan işlem yalıtım düzeyi

Birincil çoğaltmada, varsayılan işlem yalıtım düzeyi RCSı 'dir (okuma kaydedilmiş anlık görüntü yalıtımı). Okuma ölçeği genişletme ikincil çoğaltmalarda varsayılan yalıtım düzeyi anlık görüntüdür.

### <a name="can-i-bring-my-on-premises-or-iaas-sql-server-license-to-hyperscale"></a>Şirket içi veya IaaS SQL Server lisansımı hiper ölçekli olarak getirebilir miyim

Evet, [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) hiper ölçek için kullanılabilir. Her SQL Server Standard çekirdek, 1 hiper ölçekli sanal çekirdekler ile eşlenir. Her SQL Server Enterprise çekirdek, 4 hiper ölçekli sanal çekirdekler ile eşlenir. İkincil çoğaltmalar için SQL lisansına ihtiyacınız yoktur. Azure Hibrit Avantajı fiyatı, okuma ölçeği genişletme (ikincil) çoğaltmaları için otomatik olarak uygulanır.

### <a name="what-kind-of-workloads-is-hyperscale-designed-for"></a>İçin bir tür iş yükü için tasarlanan hiper ölçek

Hiper ölçek tüm SQL Server iş yüklerini destekler, ancak öncelikle OLTP için iyileştirilmiştir. Karma (HTAP) ve analitik (veri reyonu) iş yüklerini da getirebilirsiniz.

### <a name="how-can-i-choose-between-azure-sql-data-warehouse-and-azure-sql-database-hyperscale"></a>Azure SQL veri ambarı ve Azure SQL veritabanı hiper ölçeği arasında nasıl seçim yapabilirim?

Şu anda bir veri ambarı olarak SQL Server kullanarak etkileşimli analiz sorguları çalıştırıyorsanız, daha düşük bir maliyetle küçük ve orta ölçekli veri ambarlarını (100 TB 'a kadar birkaç TB) barındırabileceğiniz için Hiperscale harika bir seçenektir ve SQL Server veri ambarı iş yüklerinizi en az T-SQL kod değişiklikleriyle hiper ölçeğe geçirebilirsiniz.

Veri analizlerini, karmaşık sorgular ve sürekli olarak 100 MB/sn 'den büyük bir ölçekte çalıştırıyorsanız ya da paralel veri ambarı (PDW), Teradata veya diğer yoğun paralel Işleme (MPP) veri ambarlarından yararlanarak SQL veri ambarı en iyi seçim olabilir.
  
## <a name="hyperscale-compute-questions"></a>Hiper ölçekli işlem soruları

### <a name="can-i-pause-my-compute-at-any-time"></a>İşlem için istediğiniz zaman duraklatıp

Bu sırada değil, yoğun olmayan saatlerde maliyeti azaltmak için işlem ve yineleme sayısını ölçeklendirebilirsiniz.

### <a name="can-i-provision-a-compute-replica-with-extra-ram-for-my-memory-intensive-workload"></a>Bellek yoğun iş yüküm için ek RAM ile bir işlem çoğaltması sağlayabilir miyim

Hayır. Daha fazla RAM almak için, daha yüksek bir işlem boyutuna yükseltmeniz gerekir. Daha fazla bilgi için bkz. [hiper ölçek depolama ve işlem boyutları](resource-limits-vcore-single-databases.md#hyperscale---provisioned-compute---gen5).

### <a name="can-i-provision-multiple-compute-replicas-of-different-sizes"></a>Farklı boyutlarda birden çok işlem çoğaltması sağlayabilir miyim

Hayır.

### <a name="how-many-read-scale-out-replicas-are-supported"></a>Kaç okuma ölçeği genişletme çoğaltması desteklenir

Hiper ölçek veritabanları, varsayılan olarak bir okuma ölçeği genişletme çoğaltması (birincil dahil olmak üzere iki çoğaltma) ile oluşturulur. [Azure Portal](https://portal.azure.com) veya [REST API](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)kullanarak 0 ile 4 arasında salt okuma çoğaltması sayısını ölçeklendirebilirsiniz.

### <a name="for-high-availability-do-i-need-to-provision-additional-compute-replicas"></a>Yüksek kullanılabilirlik için ek işlem çoğaltmaları sağlanması gerekir

Hiper ölçekli veritabanlarında, veri dayanıklılığı depolama düzeyinde sağlanır. Dayanıklılık sağlamak için yalnızca bir çoğaltmaya ihtiyacınız vardır. İşlem çoğaltması kapatıldığında, veri kaybı olmadan otomatik olarak yeni bir çoğaltma oluşturulur.

Ancak, yalnızca bir çoğaltma varsa, yük devretmeden sonra yeni çoğaltmada yerel önbelleğin oluşturulması biraz zaman alabilir. Önbellek yeniden oluşturma aşamasında, veritabanı doğrudan sayfa sunucularından verileri getirir, daha yüksek depolama gecikmesini ve düşürülmüş sorgu performansına yol açar.

En düşük yük devretme etkisi ile yüksek kullanılabilirlik gerektiren görev açısından kritik uygulamalar için, birincil işlem çoğaltması dahil olmak üzere en az 2 işlem yinelemesi sağlamanız gerekir. Bu, varsayılan yapılandırmadır. Bu şekilde, yük devretme hedefi olarak hizmet veren bir etkin bekleme çoğaltması vardır.

## <a name="data-size-and-storage-questions"></a>Veri boyutu ve depolama soruları

### <a name="what-is-the-maximum-database-size-supported-with-hyperscale"></a>Hiperscale ile desteklenen en büyük veritabanı boyutu nedir?

100 TB.

### <a name="what-is-the-size-of-the-transaction-log-with-hyperscale"></a>Hiper ölçeğe sahip işlem günlüğü boyutu nedir?

Hiperscale ile işlem günlüğü, pratik olarak sonsuzdur. Yüksek günlük üretimi olan bir sistemde günlük alanının tükenmek için endişelenmeniz gerekmez. Ancak, günlük oluşturma hızı sürekli kararlılık yazma iş yükleri için kısıtlanabilir. En yoğun sürekli günlük oluşturma hızı 100 MB/sn 'dir.

### <a name="does-my-tempdb-scale-as-my-database-grows"></a>`tempdb`My Veritabanım olarak ölçeklendirim büyüyor

`tempdb`Veritabanınız yerel SSD depolamada bulunur ve sağladığınız işlem boyutuna orantılı olarak boyutlandırılır. `tempdb`En yüksek performans avantajları sağlamak için en iyi duruma getirilmiştir. `tempdb`Boyut yapılandırılamaz ve sizin için yönetiliyor.

### <a name="does-my-database-size-automatically-grow-or-do-i-have-to-manage-the-size-of-data-files"></a>Veritabanı boyutumun otomatik olarak büyümesi veya veri dosyalarının boyutunu yönetmesi gerekir

Veritabanı boyutunuz, daha fazla veri eklerken/alarak otomatik olarak büyür.

### <a name="what-is-the-smallest-database-size-that-hyperscale-supports-or-starts-with"></a>Hiperscale tarafından desteklenen veya başlayan en küçük veritabanı boyutu nedir?

40 GB. Hiper ölçek veritabanı, 10 GB başlangıç boyutuyla oluşturulur. Daha sonra, 40 GB boyutuna ulaşana kadar 10 dakikada bir, 10 GB büyümeye başlar. Bu 10 GB chucks 'in her biri, daha fazla ıOPS ve daha yüksek g/ç paralelliğini sağlamak için farklı bir sayfa sunucusuna ayrılır. Bu iyileştirme nedeniyle, ilk veritabanı boyutunu 40 GB 'tan daha küçük bir tercih etseniz bile, veritabanı otomatik olarak en az 40 GB 'a genişlecektir.

### <a name="in-what-increments-does-my-database-size-grow"></a>Veritabanı boyutumun büyümesi ne kadar artar

Her veri dosyası 10 GB ile artar. Birden çok veri dosyası aynı anda büyümeyebilir.

### <a name="is-the-storage-in-hyperscale-local-or-remote"></a>Depolama alanı, yerel veya uzak için hiper ölçekte

Hiper ölçekte, veri dosyaları Azure Standart depolama 'da depolanır. Veriler, işlem çoğaltmalarına yakın olan sayfa sunucularında yerel SSD depolamada tamamen önbelleğe alınır. Ayrıca, uzak sayfa sunucularından veri getirme sıklığını azaltmak için, işlem çoğaltmalarının yerel SSD 'de ve bellekte veri önbellekleri vardır.

### <a name="can-i-manage-or-define-files-or-filegroups-with-hyperscale"></a>Hyperscale ile dosya veya dosya gruplarını yönetebilir veya tanımlayabilir miyim?

Hayır. Veri dosyaları otomatik olarak eklenir. Ek dosya grupları oluşturmanın yaygın nedenleri hiper ölçekli depolama mimarisinde uygulanmaz.

### <a name="can-i-provision-a-hard-cap-on-the-data-growth-for-my-database"></a>Veritabanım için veri büyümesi üzerinde sabit bir büyük harf sağlayabilirim

Hayır.

### <a name="how-are-data-files-laid-out-with-hyperscale"></a>Veri dosyaları Hyperscale ile nasıl düzenlenir

Veri dosyaları, her veri dosyası için tek sayfalı sunucu ile sayfa sunucularıyla denetlenir. Veri boyutu büyüdükçe, veri dosyaları ve ilişkili sayfa sunucuları eklenir.

### <a name="is-database-shrink-supported"></a>Veritabanı daraltımı destekleniyor

Hayır.

### <a name="is-data-compression-supported"></a>Veri sıkıştırması destekleniyor

Satır, sayfa ve columnstore sıkıştırması dahil olmak üzere Evet.

### <a name="if-i-have-a-huge-table-does-my-table-data-get-spread-out-across-multiple-data-files"></a>Çok büyük bir tablonuz varsa, tablomın verileri birden fazla veri dosyasına yayılmaktadır

Evet. Belirli bir tabloyla ilişkili veri sayfaları, aynı dosya grubunun bir parçası olan birden çok veri dosyasında bitebilirler. SQL Server, verileri veri dosyaları üzerinden dağıtmak için [orantılı bir Fill stratejisi](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups#file-and-filegroup-fill-strategy) kullanır.

## <a name="data-migration-questions"></a>Veri geçişi soruları

### <a name="can-i-move-my-existing-databases-in-azure-sql-database-to-the-hyperscale-service-tier"></a>Azure SQL veritabanında var olan veritabanlarınızı hiper ölçekli hizmet katmanına taşıyabilir miyim?

Evet. Mevcut veritabanlarınızı Azure SQL veritabanı 'nda hiper ölçeğe taşıyabilirsiniz. Bu tek yönlü geçişdir. Veritabanlarını Hyperscale 'den başka bir hizmet katmanına taşıyamazsınız. Kavram provaları (POCs) için veritabanınızın bir kopyasını oluşturmanızı ve kopyayı hiper ölçeğe geçirmeniz önerilir.
  
### <a name="can-i-move-my-hyperscale-databases-to-other-service-tiers"></a>Hiperscale veritabanlarınızı diğer hizmet katmanlarına taşıyabilir miyim?

Hayır. Şu anda hiper ölçekli bir veritabanını başka bir hizmet katmanına taşıyamazsınız.

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Hiper ölçek hizmet katmanına geçişten sonra herhangi bir işlevi veya özelliği kaybetmem

Evet. Bazı Azure SQL veritabanı özellikleri, uzun süreli yedekleme saklama ile sınırlı olmamak üzere henüz hiper ölçekte desteklenmez. Veritabanlarınızı Hyperscale 'e geçirdikten sonra, bu özellikler çalışmayı durdurur.  Bu sınırlamaların geçici olması beklenir.

### <a name="can-i-move-my-on-premises-sql-server-database-or-my-sql-server-database-in-a-cloud-virtual-machine-to-hyperscale"></a>Şirket içi SQL Server veritabanını veya bir bulut sanal makinesindeki SQL Server veritabanı 'nı hiper ölçeğe taşıyabilir miyim?

Evet. İşlem çoğaltması ve diğer veri taşıma teknolojileri (toplu kopyalama, Azure Data Factory, Azure Databricks, SSIS) dahil olmak üzere hiper ölçeğe geçiş yapmak için var olan tüm geçiş teknolojilerini kullanabilirsiniz. Ayrıca, birçok geçiş senaryosunu destekleyen [Azure veritabanı geçiş hizmeti](../../dms/dms-overview.md)'ne bakın.

### <a name="what-is-my-downtime-during-migration-from-an-on-premises-or-virtual-machine-environment-to-hyperscale-and-how-can-i-minimize-it"></a>Şirket içi veya sanal makine ortamından hiper ölçeğe geçiş sırasında kapalı kalma süresi nedir ve bunu nasıl en aza indirebilir

Hiper ölçeğe geçiş için kapalı kalma süresi, veritabanlarınızı diğer Azure SQL veritabanı hizmet katmanlarına geçirdiğinizde kapalı kalma süresiyle aynıdır. Az sayıda TB 'a kadar olan veritabanlarının kapalı kalma süresini en aza indirmek için [İşlemsel çoğaltmayı](replication-to-sql-database.md#data-migration-scenario
) kullanabilirsiniz. Çok büyük veritabanları (10 + TB) için ADF, Spark veya diğer veri taşıma teknolojilerini kullanarak verileri geçirmeyi düşünebilirsiniz.

### <a name="how-much-time-would-it-take-to-bring-in-x-amount-of-data-to-hyperscale"></a>X veri miktarını hiper ölçeğe getirmek için ne kadar sürer?

Hiper ölçek, yeni/değiştirilmiş verileri 100 MB/s olarak kullanabilir, ancak verileri Azure SQL veritabanı 'nda veritabanlarına taşımak için gereken süre, kullanılabilir ağ aktarım hızı, kaynak okuma hızı ve hedef veritabanı hizmet düzeyi hedefi tarafından da etkilenir.

### <a name="can-i-read-data-from-blob-storage-and-do-fast-load-like-polybase-in-sql-data-warehouse"></a>Blob depolamadaki verileri okuyabilir ve hızlı yükleme yapabilir (SQL veri ambarı 'nda PolyBase gibi)

Bir istemci uygulamanızın Azure Storage 'dan veri okuyabilmesini ve veri yükünü bir hiper ölçekli veritabanına yüklemesini (tıpkı Azure SQL veritabanı 'ndaki diğer veritabanları gibi) sağlayabilirsiniz. PolyBase Şu anda Azure SQL veritabanı 'nda desteklenmiyor. Hızlı yük sağlamaya alternatif olarak, [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/)KULLANABILIR veya [SQL için spark Bağlayıcısı](spark-connector.md)ile [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/) bir Spark işi kullanabilirsiniz. SQL 'e Spark Bağlayıcısı toplu ekleme 'yi destekler.

BULK INSERT veya OPENROWSET kullanarak Azure Blob mağazasından verileri toplu olarak okumak de mümkündür: [Azure Blob depolama alanındaki verilere toplu erişim örnekleri](https://docs.microsoft.com/sql/relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage?view=sql-server-2017#accessing-data-in-a-csv-file-referencing-an-azure-blob-storage-location).

Hiper ölçekte basit kurtarma veya toplu günlük modeli desteklenmez. Yüksek kullanılabilirlik ve zaman içinde kurtarma sağlamak için tam kurtarma modeli gereklidir. Ancak, hiper ölçek günlüğü mimarisi diğer Azure SQL veritabanı hizmet katmanlarına kıyasla daha iyi veri alma oranı sağlar.

### <a name="does-hyperscale-allow-provisioning-multiple-nodes-for-parallel-ingesting-of-large-amounts-of-data"></a>Hiper ölçek, büyük miktarlarda verilerin paralel bir şekilde sağlanması için birden çok düğüm sağlanmasını sağlar

Hayır. Hiper ölçek, simetrik bir çoklu işlem (SMP) mimarisidir ve yüksek düzeyde paralel işleme (MPP) veya çok yöneticili bir mimari değildir. Yalnızca salt okuma iş yüklerini ölçeklendirmek için birden çok çoğaltma oluşturabilirsiniz.

### <a name="what-is-the-oldest-sql-server-version-supported-for-migration-to-hyperscale"></a>Hiper ölçeğe geçiş için desteklenen en eski SQL Server sürümü nedir?

SQL Server 2005. Daha fazla bilgi için bkz. [tek bir veritabanına veya havuza alınmış bir veritabanına geçirme](migrate-to-database-from-sql-server.md#migrate-to-a-single-database-or-a-pooled-database). Uyumluluk sorunları için bkz. [Veritabanı geçişi uyumluluk sorunlarını çözme](migrate-to-database-from-sql-server.md#resolving-database-migration-compatibility-issues).

### <a name="does-hyperscale-support-migration-from-other-data-sources-such-as-amazon-aurora-mysql-postgresql-oracle-db2-and-other-database-platforms"></a>Hiper ölçek, Amazon Aurora, MySQL, PostgreSQL, Oracle, DB2 ve diğer veritabanı platformları gibi diğer veri kaynaklarından geçişi destekler

Evet. [Azure veritabanı geçiş hizmeti](../../dms/dms-overview.md) birçok geçiş senaryosunu destekler.

## <a name="business-continuity-and-disaster-recovery-questions"></a>İş sürekliliği ve olağanüstü durum kurtarma soruları

### <a name="what-slas-are-provided-for-a-hyperscale-database"></a>Hiper ölçek veritabanı için hangi SLA 'lar sağlanır

Bkz. [Azure SQL veritabanı Için SLA](https://azure.microsoft.com/support/legal/sla/sql-database/v1_4/). Ek ikincil işlem çoğaltmaları 99,99, iki veya daha fazla ikincil işlem çoğaltması olan bir veritabanı için kullanılabilirliği artırır.

### <a name="are-the-database-backups-managed-for-me-by-azure-sql-database"></a>Veritabanı yedeklemeleri, benim için Azure SQL veritabanı tarafından yönetiliyor

Evet.

### <a name="how-often-are-the-database-backups-taken"></a>Veritabanı yedeklemeleri ne sıklıkta alınır

Hiper ölçekli veritabanları için geleneksel bir tam, değişiklik ve günlük yedeklemesi yoktur. Bunun yerine, veri dosyalarının normal depolama anlık görüntüleri vardır. Oluşturulan günlük, yapılandırılan Bekletme dönemi için olduğu gibi tutulur ve Bekletme dönemi içinde herhangi bir zaman noktasına geri yüklemeye olanak tanır.

### <a name="does-hyperscale-support-point-in-time-restore"></a>Hiper ölçek desteği, zaman içinde geri yükleme

Evet.

### <a name="what-is-the-recovery-point-objective-rporecovery-time-objective-rto-for-database-restore-in-hyperscale"></a>Hiper ölçekte veritabanı geri yüklemesi için kurtarma noktası hedefi (RPO)/kurtarma süresi hedefi (RTO) nedir?

RPO 0 dk. RTO hedefi, veritabanı boyutundan bağımsız olarak 10 dakikadan azdır.

### <a name="does-database-backup-affect-compute-performance-on-my-primary-or-secondary-replicas"></a>Veritabanı yedeklemesi, birincil veya ikincil çoğaltmalarda işlem performansını etkiler

Hayır. Yedeklemeler, depolama alt sistemi tarafından yönetilir ve depolama anlık görüntülerinden yararlanır. Kullanıcı iş yüklerini etkilemez.

### <a name="can-i-perform-geo-restore-with-a-hyperscale-database"></a>Hiper ölçek veritabanı ile coğrafi geri yükleme yapabilir miyim

Evet. Coğrafi geri yükleme tam olarak desteklenmektedir. Bir noktadan noktaya geri yükleme işleminin aksine, coğrafi geri yükleme bir veri boyutu işlemi gerektirir. Veri dosyaları paralel olarak kopyalanır, bu nedenle bu işlemin süresi, birincil olarak veritabanındaki en büyük dosyanın boyutuna (Toplam veritabanı boyutu yerine) bağlıdır. Veritabanı, kaynak veritabanının bölgesiyle [eşleştirilmiş](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) olan Azure bölgesine geri yüklenirse, coğrafi geri yükleme süresi önemli ölçüde daha kısa olur.

### <a name="can-i-set-up-geo-replication-with-hyperscale-database"></a>Hiper ölçekli veritabanı ile Coğrafi çoğaltmayı ayarlayabilir miyim

Şu anda değil.

### <a name="can-i-take-a-hyperscale-database-backup-and-restore-it-to-my-on-premises-server-or-on-sql-server-in-a-vm"></a>Hiper ölçekli bir veritabanı yedeklemesini alabilir ve şirket içi sunucuma veya bir VM 'deki SQL Server geri yüklemenize olanak verebilir

Hayır. Hiper ölçekli veritabanları için depolama biçimi SQL Server yayınlanan herhangi bir sürümden farklıdır ve yedeklemeleri denetlemenize veya bunlara erişim sahibi kalmazsınız. Verilerinizi bir hiper ölçekli veritabanının dışına çıkarmak için veri taşıma teknolojilerini (örn. Azure Data Factory, Azure Databricks, SSIS vb.) kullanarak verileri ayıklayabilirsiniz.

## <a name="cross-feature-questions"></a>Çapraz Özellik soruları

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Hiper ölçek hizmet katmanına geçişten sonra herhangi bir işlevi veya özelliği kaybetmem

Evet. Bazı Azure SQL veritabanı özellikleri, uzun süreli yedek saklama dahil, ancak bunlarla sınırlı olmamak üzere hiper ölçekte desteklenmez. Veritabanlarınızı Hyperscale 'e geçirdikten sonra, bu özellikler çalışmayı durdurur.

### <a name="will-polybase-work-with-hyperscale"></a>PolyBase, hiper ölçeğe göre çalışır

Hayır. PolyBase, Azure SQL veritabanı 'nda desteklenmiyor.

### <a name="does-hyperscale-have-support-for-r-and-python"></a>Hiper ölçekte R ve Python desteği vardır

Şu anda değil.

### <a name="are-compute-nodes-containerized"></a>İşlem düğümleri Kapsayıcılı

Hayır. Hiper ölçekli süreçler, kapsayıcılar içinde değil [Service Fabric](https://azure.microsoft.com/services/service-fabric/) düğümlerinde (VM) çalışır.

## <a name="performance-questions"></a>Performans soruları

### <a name="how-much-write-throughput-can-i-push-in-a-hyperscale-database"></a>Hiper ölçek veritabanına ne kadar yazma aktarım hızı gönderebilirim?

İşlem günlüğü verimlilik sınırı, herhangi bir hiper ölçek işlem boyutu için 100 MB/sn olarak ayarlanır. Bu oranı elde etme yeteneği, iş yükü türü, istemci yapılandırması dahil, ancak bunlarla sınırlı olmamak üzere çok sayıda etkene bağlıdır ve bu hızda günlük oluşturmak için birincil işlem çoğaltmasında yeterli işlem kapasitesi vardır.

### <a name="how-many-iops-do-i-get-on-the-largest-compute"></a>En büyük işlem üzerinde kaç ıOPS alabilirim?

IOPS ve GÇ gecikmesi, iş yükü desenlerine bağlı olarak değişir. Erişilmekte olan veriler işlem çoğaltmasında önbelleğe alınmışsa, yerel SSD ile olduğu gibi benzer GÇ performansı görürsünüz.

### <a name="does-my-throughput-get-affected-by-backups"></a>İş aktarım alanım yedeklerden etkilendi

Hayır. İşlem, depolama katmanından ayrılır. Bu, yedeklemenin performans etkisini ortadan kaldırır.

### <a name="does-my-throughput-get-affected-as-i-provision-additional-compute-replicas"></a>Ek işlem çoğaltmaları sağlamam sırasında aktarım hızı etkilendi

Depolama paylaşıldığından ve birincil ve ikincil işlem çoğaltmaları arasında doğrudan fiziksel çoğaltma olmadığından, birincil çoğaltmadaki aktarım hızı ikincil çoğaltmalar eklenerek doğrudan etkilenmez. Bununla birlikte, ikincil çoğaltmalarda önemli okuma performansını önlemek için ikincil çoğaltmalarda ve sayfa sunucularında günlük uygulanmasına izin vermek üzere birincil üzerinde sürekli kararlılık yazma iş yükünü azaltamazuz.

### <a name="how-do-i-diagnose-and-troubleshoot-performance-problems-in-a-hyperscale-database"></a>Nasıl yaparım? hiper ölçek veritabanında performans sorunlarını tanılayın ve sorun giderin

Çoğu performans sorunu için, özellikle depolama performansında belirtilmemiş olanlar, ortak SQL tanılama ve sorun giderme adımları geçerlidir. Hiper ölçeğe özgü depolama tanılaması için bkz. [SQL hiper ölçek performansı sorun giderme tanılaması](hyperscale-performance-diagnostics.md).

## <a name="scalability-questions"></a>Ölçeklenebilirlik ile ilgili sorular

### <a name="how-long-would-it-take-to-scale-up-and-down-a-compute-replica"></a>Bir işlem çoğaltmasının ölçeğini artırma ve azaltma işlemi ne kadar sürer?

İşlem ölçeği artırma veya azaltma, veri boyutundan bağımsız olarak 5-10 dakika sürer.

### <a name="is-my-database-offline-while-the-scaling-updown-operation-is-in-progress"></a>Ölçeklendirme artırma/azaltma işlemi devam ederken Veritabanım çevrimdışı

Hayır. Ölçeği artırma ve azaltma çevrimiçi olacak.

### <a name="should-i-expect-connection-drop-when-the-scaling-operations-are-in-progress"></a>Ölçeklendirme işlemleri sürerken bağlantı bırakması beklenmeli

Ölçeklendirme işleminin sonunda bir yük devretme gerçekleştiğinde, varolan bağlantıların ölçeğini artırma veya azaltma. İkincil çoğaltmaların eklenmesi bağlantı düşmesinin sonucu vermez.

### <a name="is-the-scaling-up-and-down-of-compute-replicas-automatic-or-end-user-triggered-operation"></a>İşlem çoğaltmalarının ölçeğini artırma ve azaltma otomatik veya son kullanıcı tarafından tetiklenen işlem

Son Kullanıcı. Otomatik değil.  

### <a name="does-the-size-of-my-tempdb-database-also-grow-as-the-compute-is-scaled-up"></a>`tempdb`İşlem büyüdükçe veritabanı boyutu da büyür

Evet. `tempdb`İşlem büyüdükçe veritabanı otomatik olarak ölçeklendirilir.  

### <a name="can-i-provision-multiple-primary-compute-replicas-such-as-a-multi-master-system-where-multiple-primary-compute-heads-can-drive-a-higher-level-of-concurrency"></a>Birden fazla birincil bilgi işlem yöneticisinin daha yüksek bir eşzamanlılık düzeyini sağlayabildiği çok yöneticili bir sistem gibi birden çok birincil işlem çoğaltması sağlayabilir miyim

Hayır. Yalnızca birincil işlem çoğaltması okuma/yazma isteklerini kabul eder. İkincil işlem çoğaltmaları yalnızca salt okuma isteklerini kabul eder.

## <a name="read-scale-out-questions"></a>Genişleme sorularını okuyun

### <a name="how-many-secondary-compute-replicas-can-i-provision"></a>Kaç tane ikincil işlem çoğaltması sağlayabilirim?

Hiper ölçek veritabanları için varsayılan olarak bir ikincil çoğaltma oluşturacağız. Kopyaların sayısını ayarlamak istiyorsanız [Azure Portal](https://portal.azure.com) veya [REST API](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)kullanarak bunu yapabilirsiniz.

### <a name="how-do-i-connect-to-these-secondary-compute-replicas"></a>Nasıl yaparım? bu ikincil işlem çoğaltmalarına bağlanın

`ApplicationIntent`Bağlantı dizinizdeki bağımsız değişkeni olarak ayarlayarak, bu ek salt okuma işlem çoğaltmalarına bağlanabilirsiniz `ReadOnly` . İle işaretlenen tüm bağlantılar `ReadOnly` otomatik olarak ek salt okuma işlem Çoğaltmalarından birine yönlendirilir.  

### <a name="how-do-i-validate-if-i-have-successfully-connected-to-secondary-compute-replica-using-ssms-or-other-client-tools"></a>SSMS veya diğer istemci araçlarını kullanarak ikincil işlem çoğaltmasına başarıyla bağlanıp bağlanmadım Nasıl yaparım? doğrulayın.

Aşağıdaki T-SQL sorgusunu çalıştırabilirsiniz: `SELECT DATABASEPROPERTYEX ('<database_name>', 'Updateability')` .
Sonuç, `READ_ONLY` salt okunurdur bir ikincil çoğaltmaya bağlıysanız ve `READ_WRITE` birincil çoğaltmaya bağlıysanız olur. Veritabanı bağlamının veritabanına değil hiper ölçek veritabanının adı olarak ayarlanması gerektiğini unutmayın `master` .

### <a name="can-i-create-a-dedicated-endpoint-for-a-read-scale-out-replica"></a>Okuma ölçeği genişletme çoğaltması için adanmış bir uç nokta oluşturabilir miyim

Hayır. Yalnızca belirterek, okuma ölçeğini genişletme çoğaltmalarına bağlanabilirsiniz `ApplicationIntent=ReadOnly` .

### <a name="does-the-system-do-intelligent-load-balancing-of-the-read-workload"></a>Sistem, okuma iş yükünün akıllı yük dengelemesini yapar

Hayır. Salt okuma amacına sahip yeni bir bağlantı, rastgele bir okuma ölçeği genişletme çoğaltmasına yeniden yönlendirilir.

### <a name="can-i-scale-updown-the-secondary-compute-replicas-independently-of-the-primary-replica"></a>İkincil işlem çoğaltmalarının birincil çoğaltmadan bağımsız olarak ölçeğini artırma/azaltma yapabilir miyim?

Hayır. İkincil işlem çoğaltması aynı zamanda yüksek kullanılabilirlik yük devretme hedefleri olarak da kullanılır, bu nedenle yük devretme sonrasında beklenen performansı sağlamak için birincil ile aynı yapılandırmaya sahip olmaları gerekir.

### <a name="do-i-get-different-tempdb-sizing-for-my-primary-compute-and-my-additional-secondary-compute-replicas"></a>`tempdb`Birincil işlem ve ek ikincil işlem çoğaltmalarım için farklı bir boyutlandırma alıyorum

Hayır. `tempdb`Veritabanınız, işlem boyutu sağlama temel alınarak yapılandırılır, ikincil işlem çoğaltmalarınız birincil işlem ile aynı boyutlardır.

### <a name="can-i-add-indexes-and-views-on-my-secondary-compute-replicas"></a>İkincil işlem çoğaltmalarıma dizinler ve görünümler ekleyebilir miyim?

Hayır. Hiper ölçekli veritabanlarında paylaşılan depolama vardır ve tüm bilgi işlem çoğaltmalarının aynı tabloları, dizinleri ve görünümleri görmeyeceği anlamına gelir. İkincil üzerinde okuma için iyileştirilmiş ek dizinler istiyorsanız, bunları birinciye eklemeniz gerekir.

### <a name="how-much-delay-is-there-going-to-be-between-the-primary-and-secondary-compute-replicas"></a>Birincil ve ikincil işlem çoğaltmaları arasında ne kadar gecikme olur?

Bir işlemin birincili üzerinde yürütüldüğü zamandan itibaren, bir ikincil üzerinde görünür durumda olan veri gecikmesi, geçerli günlük oluşturma hızına bağlıdır. Tipik veri gecikmesi düşük milisaniyedir.

## <a name="next-steps"></a>Sonraki adımlar

Hyperscale hizmet katmanı hakkında daha fazla bilgi için bkz. [Hyperscale hizmet katmanı](service-tier-hyperscale.md).
