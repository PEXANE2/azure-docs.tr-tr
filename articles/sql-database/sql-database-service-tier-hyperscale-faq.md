---
title: Azure SQL veritabanı hiper ölçek SSS | Microsoft Docs
description: Yaygın soruların cevapları müşteriler, hiper ölçekli hizmet katmanında bir Azure SQL veritabanı (genellikle hiper ölçek veritabanı denir) hakkında bilgi ister.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 05/06/2019
ms.openlocfilehash: 8c35877c7de2fa89a8fe7a94c11787814183df9e
ms.sourcegitcommit: a7a9d7f366adab2cfca13c8d9cbcf5b40d57e63a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71162250"
---
# <a name="faq-about-azure-sql-hyperscale-databases"></a>Azure SQL hiper ölçek veritabanları hakkında SSS

Bu makalede, genellikle hiper ölçekli veritabanı olarak adlandırılan Azure SQL veritabanı hiper ölçek hizmeti katmanındaki bir veritabanını ele almak için sık sorulan soruların yanıtları sunulmaktadır. Bu makalede, Hyperscale 'nin desteklediği senaryolar ve platformlar arası hizmetler, genel olarak SQL veritabanı Hyperscale ile uyumludur.

- Bu SSS, hiper ölçek hizmet katmanını kısa bir şekilde anlayabilen ve belirli soruların ve kaygılarının cevaplanmasını isteyen okuyucular için hazırlanmıştır.
- Bu SSS, bir kılavuz kitabı olması veya bir SQL veritabanı hiper ölçek veritabanının nasıl kullanılacağına ilişkin soruların yanıtlanmasına yöneliktir. Bunun için [Azure SQL veritabanı hiper ölçek](sql-database-service-tier-hyperscale.md) belgelerine başvurmanız önerilir.

## <a name="general-questions"></a>Genel sorular

### <a name="what-is-a-hyperscale-database"></a>Hiper ölçekli veritabanı nedir?

Hiper ölçekli bir veritabanı, hiper ölçek genişleme depolama teknolojisi tarafından desteklenen hiper ölçek hizmet katmanındaki bir Azure SQL veritabanıdır. Bir Hyperscale veritabanı en fazla 100 TB veri destekler ve yüksek aktarım hızı ve performans sağlar ve iş yükü gereksinimlerine uyum sağlamak için hızlı ölçeklendirmeye olanak tanır. Ölçeklendirme, uygulama-bağlantı, sorgu işleme vb. için saydam, diğer SQL veritabanı gibi çalışır.

### <a name="what-resource-types-and-purchasing-models-support-hyperscale"></a>Hiper ölçeklendirmeyi destekleyen kaynak türleri ve satın alma modelleri

Hyperscale hizmet katmanı yalnızca Azure SQL veritabanı 'ndaki sanal çekirdek tabanlı satın alma modeli kullanılarak tek veritabanlarında kullanılabilir.  

### <a name="how-does-the-hyperscale-service-tier-differ-from-the-general-purpose-and-business-critical-service-tiers"></a>Hiper ölçek hizmet katmanının Genel Amaçlı ve İş Açısından Kritik hizmet katmanlarından farkı nedir?

Sanal çekirdek tabanlı hizmet katmanları, birincil olarak kullanılabilirlik, depolama türü ve IOPS temel alınarak farklılaştırılır.

- Genel Amaçlı hizmet katmanı, iş yüklerinin çoğu için uygun olduğundan, GÇ gecikme süresinin veya yük devretme sürelerinin öncelik olmadığı dengeli bir dizi işlem ve depolama seçeneği sunar.
- Hiper ölçek hizmeti katmanı çok büyük veritabanı iş yükleri için iyileştirilmiştir.
- İş Açısından Kritik hizmet katmanı, GÇ gecikmesi önem derecesi olan iş yükleri için uygundur.

| | Kaynak türü | Genel Amaçlı |  Hiper ölçek | İş Açısından Kritik |
|:---:|:---:|:---:|:---:|:---:|
| **En iyisi** |Tümü|  Birçok iş yükü. Bütçe odaklı dengeli işlem ve depolama seçenekleri sunar. | Büyük veri kapasitesi gereksinimlerine sahip veri uygulamaları ve depolamayı otomatik olarak ölçeklendirme ve işlem akıcı bir şekilde ölçeklendirme özelliği. | Yüksek işlem hızına sahip OLTP uygulamaları ve en düşük gecikmeli GÇ. Birkaç, yalıtılmış çoğaltma kullanarak hatalara en yüksek esnekliği sağlar.|
|  **Kaynak türü** ||Tek veritabanı/elastik havuz/yönetilen örnek | Tek veritabanı | Tek veritabanı/elastik havuz/yönetilen örnek |
| **İşlem boyutu**|Tek veritabanı/elastik havuz * | 1-80 sanal çekirdek | 1-80 sanal çekirdek * | 1-80 sanal çekirdek |
| |Yönetilen örnek | 8, 16, 24, 32, 40, 64, 80 Vçekirdekler | Yok | 8, 16, 24, 32, 40, 64, 80 Vçekirdekler |
| **Depolama türü** | Tümü |Premium uzak depolama (örnek başına) | Yerel SSD Önbelleği (örnek başına) ile birlikte bağlanmış depolama | Süper hızlı yerel SSD depolaması (örnek başına) |
| **Depolama boyutu** | Tek veritabanı/elastik havuz | 5 GB – 4 TB | 100 TB 'a kadar | 5 GB – 4 TB |
| | Yönetilen örnek  | 32 GB – 8 TB | Yok | 32 GB – 4 TB |
| **GÇ işleme** | Tek veritabanı * * | maksimum 7000 ıOPS ile vCore başına 500 ıOPS | Hiper ölçek, birden çok düzeyde önbelleğe alma özelliği olan çok katmanlı bir mimaridir. Etkin IOPS iş yüküne bağlı olacaktır. | 5000 ıOPS, en fazla 200.000 ıOPS|
| | Yönetilen örnek | Dosyanın boyutuna bağlıdır | Yok | Yönetilen örnek: Dosyanın boyutuna bağlıdır|
|**Kullanılabilirlik**|Tümü|1 çoğaltma, okuma ölçeği yok, yerel önbellek yok | Birden çok çoğaltma, 4 okuma ölçeğinde, kısmi yerel önbellek | 3 çoğaltma, 1 okuma ölçeği, bölge yedekli HA, tam yerel önbellek |
|**Lerine**|Tümü|RA-GRS, 7-35 gün (varsayılan olarak 7 gün)| RA-GRS, 7 gün, sabit zaman zaman aşımı kurtarma (sür) | RA-GRS, 7-35 gün (varsayılan olarak 7 gün) |

\*Esnek havuzlar hiper ölçek hizmeti katmanında desteklenmiyor

### <a name="who-should-use-the-hyperscale-service-tier"></a>Hiper ölçek hizmet katmanını kimler kullanmalıdır?

Hyperscale hizmet katmanı öncelikle büyük şirket içi SQL Server veritabanlarına sahip olan müşterilere yöneliktir ve buluta veya zaten Azure SQL veritabanı kullanan müşteriler ve önemli ölçüde genişletmek için uygulamalarını modernleştirin etmek ister veritabanı büyümesi için olası. Hiper ölçek, hem yüksek performans hem de yüksek ölçeklenebilirliğe sahip müşteriler için de tasarlanmıştır. Hiperscale ile şunları alırsınız:

- 100 TB 'a kadar veritabanı boyutu desteği
- Veritabanı boyutundan bağımsız olarak hızlı veritabanı yedeklemeleri (yedeklemeler dosya anlık görüntülerini temel alır)
- Veritabanı boyutundan bağımsız olarak hızlı veritabanı geri yüklenir (geri yüklemeler dosya anlık görüntülerinden alınır)
- Daha yüksek günlük verimlilik, veritabanı boyutundan bağımsız olarak hızlı işlem işleme süreleriyle sonuçlanır
- Okuma iş yükünüzü boşaltmak için bir veya daha fazla salt okunurdur.
- Sabit zamanlı olarak hızlı bir şekilde işleme, ağır iş yüküne uyum sağlamak ve daha sonra ölçeği sabit bir zamanda genişletmek daha güçlü olacak şekilde. Bu, örneğin, bir P6 ile P11 arasında ölçeği artırma ve azaltma ile benzerdir, ancak bu, veri işlemi boyutunun bir boyutu olmadığından çok daha hızlıdır.

### <a name="what-regions-currently-support-hyperscale"></a>Şu anda hiper ölçeklendirmeyi destekleyen bölgeler

Azure SQL veritabanı hiper ölçek katmanı Şu anda [Azure SQL veritabanı hiper ölçeğe genel bakış](sql-database-service-tier-hyperscale.md#regions)altında listelenen bölgelerde mevcuttur.

### <a name="can-i-create-multiple-hyperscale-databases-per-logical-server"></a>Mantıksal sunucu başına birden çok hiper ölçekli veritabanı oluşturabilir miyim

Evet. Mantıksal sunucu başına hiper ölçek veritabanlarının sayısı hakkında daha fazla bilgi ve sınırlamalar için bkz. [mantıksal bir sunucuda, tek ve havuza alınmış veritabanları Için SQL veritabanı kaynak sınırları](sql-database-resource-limits-logical-server.md).

### <a name="what-are-the-performance-characteristics-of-a-hyperscale-database"></a>Hiper ölçek veritabanının performans özellikleri nelerdir

SQL veritabanı hiper ölçek mimarisi, büyük veritabanı boyutlarını desteklerken yüksek performans ve verimlilik sağlar. 

### <a name="what-is-the-scalability-of-a-hyperscale-database"></a>Hiper ölçek veritabanının ölçeklenebilirliği nedir?

SQL veritabanı hiper ölçek, iş yükü talebe göre hızlı ölçeklenebilirlik sağlar.

- **Ölçeği artırma/azaltma**

  Hyperscale ile, birincil işlem boyutunu CPU, bellek gibi kaynaklar bakımından ölçeklendirebilir ve sonra ölçeği sabit bir süre içinde azaltabilirsiniz. Depolama paylaşıldığından ölçeği büyütme ve küçültme, veri işleme bir boyut değildir.  
- **Ölçeği genişletme/genişletme**

  Hiperscale ile, okuma isteklerinizi karşılamak için kullanabileceğiniz bir veya daha fazla ek işlem düğümü sağlama olanağı da alırsınız. Diğer bir deyişle, okuma iş yükünüzü birincil işlem üzerinden boşaltmak için bu ek işlem düğümlerini salt okuma düğümleri olarak kullanabilirsiniz. Salt okuma özelliklerine ek olarak, bu düğümler birincil sunucudan yük devretme durumunda etkin bekleme işlevi de sunar.

  Bu ek işlem düğümlerinin her birini sağlamak, sabit zamanlı olarak yapılabilir ve çevrimiçi bir işlemdir. Bağlantı dizinizdeki `ApplicationIntent` bağımsız değişkeni olarak `readonly`ayarlayarak, bu ek salt okuma işlem düğümlerine bağlanabilirsiniz. İle `readonly` işaretlenen tüm bağlantılar otomatik olarak ek salt okuma işlem düğümlerinden birine yönlendirilir.

## <a name="deep-dive-questions"></a>Derinlemesine bakış soruları

### <a name="can-i-mix-hyperscale-and-single-databases-in-a-single-logical-server"></a>Tek bir mantıksal sunucuda hiper ölçek ve tek veritabanlarını karıştırabilirim

Evet, uygulayabilirsiniz.

### <a name="does-hyperscale-require-my-application-programming-model-to-change"></a>Hiper ölçek, uygulama programlama modelinizin değiştirilmesini gerektiriyor

Hayır, uygulama programlama modeliniz olduğu gibi kalır. Azure SQL veritabanı ile etkileşim kurmak için Bağlantı dizenizi her zamanki gibi ve diğer normal modlar olarak kullanırsınız.

### <a name="what-transaction-isolation-levels-are-going-to-be-default-on-sql-database-hyperscale-database"></a>SQL veritabanı hiper ölçek veritabanında hangi işlem yalıtım düzeylerinin varsayılan olacak

Birincil düğümde, işlem yalıtım düzeyi RCSı 'dir (okuma kaydedilmiş anlık görüntü yalıtımı). Okuma ölçeği ikincil düğümlerinde, yalıtım düzeyi anlık görüntüdür.

### <a name="can-i-bring-my-on-premises-or-iaas-sql-server-license-to-sql-database-hyperscale"></a>Şirket içi veya IaaS SQL Server lisansımı SQL veritabanı Hyperscale 'ye getirebilir miyim?

Evet, [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) hiper ölçek için kullanılabilir. Her SQL Server Standard çekirdek, 1 hiper ölçekli sanal çekirdekler ile eşlenir. Her SQL Server Enterprise çekirdek, 4 hiper ölçekli sanal çekirdekler ile eşlenir. İkincil çoğaltmalar için SQL lisansına ihtiyacınız yoktur. Azure Hibrit Avantajı fiyatı, okuma ölçeğinde (ikincil) çoğaltmalar için otomatik olarak uygulanır.

### <a name="what-kind-of-workloads-is-sql-database-hyperscale-designed-for"></a>İçin tasarlanan SQL veritabanı hiper ölçeği ne tür iş yükleri

SQL veritabanı hiper ölçek tüm SQL Server iş yüklerini destekler, ancak öncelikle OLTP için iyileştirilmiştir. Karma (HTAP) ve analitik (veri reyonu) iş yüklerini da getirebilirsiniz.

### <a name="how-can-i-choose-between-azure-sql-data-warehouse-and-sql-database-hyperscale"></a>Azure SQL veri ambarı ve SQL veritabanı Hyperscale arasından nasıl seçim yapabilirim

Şu anda bir veri ambarı olarak SQL Server kullanarak etkileşimli analiz sorguları çalıştırıyorsanız, daha düşük bir maliyette görece küçük veri ambarlarını (10 GB 'ye kadar birkaç TB 'ye kadar) barındırabilmeniz için SQL veritabanı hiper ölçek harika bir seçenektir ve verilerinizi geçirebilirsiniz. T-SQL kodu değişiklikleri olmadan SQL veritabanı Hyperscale 'e yönelik arehouse iş yükü.

Karmaşık sorgularla büyük ölçekte veri analizi çalıştırıyorsanız ve paralel veri ambarı (PDW), Teradata veya diğer yoğun paralel Işlemci (MPP)) veri ambarları kullanıyorsanız SQL veri ambarı en iyi seçim olabilir.
  
## <a name="sql-database-hyperscale-compute-questions"></a>SQL veritabanı hiper ölçek işlem soruları

### <a name="can-i-pause-my-compute-at-any-time"></a>İşlem için istediğiniz zaman duraklatıp

Bu sırada değil, yoğun olmayan saatlerde maliyeti azaltmak için işlem ve yineleme sayısını ölçeklendirebilirsiniz.

### <a name="can-i-provision-a-compute-with-extra-ram-for-my-memory-intensive-workload"></a>Yoğun bellek yoğunluğu olan iş yüküm için ek RAM ile bir işlem sağlayabilir miyim

Hayır. Daha fazla RAM almak için, daha yüksek bir işlem boyutuna yükseltmeniz gerekir. Daha fazla bilgi için bkz. [hiper ölçek depolama ve işlem boyutları](sql-database-vcore-resource-limits-single-databases.md#hyperscale-service-tier-for-provisioned-compute).

### <a name="can-i-provision-multiple-compute-nodes-of-different-sizes"></a>Farklı boyutlarda birden çok işlem düğümü sağlayabilir miyim

Hayır.

### <a name="how-many-read-scale-replicas-are-supported"></a>Kaç tane okuma ölçeği çoğaltması desteklenir

Hiper ölçek veritabanları, varsayılan olarak bir okuma ölçeğinde çoğaltma (toplamda iki çoğaltma) ile oluşturulur. [Azure Portal](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current), [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)kullanarak 0 ile 4 arasındaki salt okuma çoğaltması sayısını ölçeklendirebilirsiniz.

### <a name="for-high-availability-do-i-need-to-provision-additional-compute-nodes"></a>Yüksek kullanılabilirlik için ek işlem düğümleri sağlamam gerekir

Hiper ölçekli veritabanlarında esneklik, depolama düzeyinde sağlanır. Dayanıklılık sağlamak için yalnızca bir çoğaltmaya ihtiyacınız vardır. İşlem çoğaltması kapatıldığında, veri kaybı olmadan otomatik olarak yeni bir çoğaltma oluşturulur.

Ancak, yalnızca bir çoğaltma varsa, yük devretmeden sonra yeni çoğaltmada yerel önbelleğin oluşturulması biraz zaman alabilir. Önbellek yeniden oluşturma aşamasında, veritabanı doğrudan sayfa sunucularından verileri getirir, bu da düşürülmüş ıOPS ve sorgu performansına yol açar.

Yüksek kullanılabilirlik gerektiren görev açısından kritik uygulamalar için, birincil işlem düğümü (varsayılan) dahil olmak üzere en az 2 işlem düğümü sağlamanız gerekir. Bu şekilde, yük devretme durumunda kullanılabilir bir etkin bekleme vardır.

## <a name="data-size-and-storage-questions"></a>Veri boyutu ve depolama soruları

### <a name="what-is-the-max-db-size-supported-with-sql-database-hyperscale"></a>SQL veritabanı Hyperscale ile desteklenen en fazla veritabanı boyutu nedir?

100 TB

### <a name="what-is-the-size-of-the-transaction-log-with-hyperscale"></a>Hiper ölçeğe sahip işlem günlüğü boyutu nedir?

Hiperscale ile işlem günlüğü, pratik olarak sonsuzdur. Yüksek günlük üretimi olan bir sistemde günlük alanının tükenmek için endişelenmeniz gerekmez. Ancak, sürekli agresif iş yükleri için günlük oluşturma hızı kısıtlanabilir. En yoğun sürekli günlük oluşturma oranı yaklaşık 100 MB/sn 'dir.

### <a name="does-my-temp-db-scale-as-my-database-grows"></a>Veritabanım arttıkça geçici db ölçeklendirim

`tempdb` Veritabanınız yerel SSD depolamada bulunur ve sağladığınız işlem boyutuna göre yapılandırılır. `tempdb` En iyi performans avantajları sağlamak için en iyi duruma getirilmiştir ve düzenlenir. `tempdb` Boyut yapılandırılabilir değildir ve depolama alt sistemi tarafından sizin için yönetilir.

### <a name="does-my-database-size-automatically-grow-or-do-i-have-to-manage-the-size-of-the-data-files"></a>Veritabanı boyutumun otomatik olarak büyümesi veya veri dosyalarının boyutunu yönetmesi gerekir

Veritabanı boyutunuz, daha fazla veri eklerken/alarak otomatik olarak büyür.

### <a name="what-is-the-smallest-database-size-that-sql-database-hyperscale-supports-or-starts-with"></a>SQL veritabanı hiper ölçek tarafından desteklenen veya başlayan en küçük veritabanı boyutu nedir?

10 GB

### <a name="in-what-increments-does-my-database-size-grow"></a>Veritabanı boyutumun büyümesi ne kadar artar

1 GB

### <a name="is-the-storage-in-sql-database-hyperscale-local-or-remote"></a>Depolama, SQL veritabanı hiper ölçekte yerel veya uzak

Hiper ölçekte, veri dosyaları Azure Standart depolama 'da depolanır. Veriler yoğun bir şekilde yerel SSD depolamada önbelleğe alınarak işlem düğümlerine yakın makinelerde yapılır. Ayrıca, uzak düğümlerden veri getirme sıklığını azaltmak için, işlem düğümlerinin yerel SSD ve bellek içi (arabellek havuzu vb.) önbelleğinde bir önbelleği vardır.

### <a name="can-i-manage-or-define-files-or-filegroups-with-hyperscale"></a>Hyperscale ile dosya veya dosya gruplarını yönetebilir veya tanımlayabilir miyim?

Hayır
  
### <a name="can-i-provision-a-hard-cap-on-the-data-growth-for-my-database"></a>Veritabanım için veri büyümesi üzerinde sabit bir büyük harf sağlayabilirim

Hayır

### <a name="how-are-data-files-laid-out-with-sql-database-hyperscale"></a>Veri dosyaları SQL veritabanı Hyperscale ile nasıl düzenlenir

Veri dosyaları sayfa sunucularıyla denetlenir. Veri boyutu büyüdükçe, veri dosyaları ve ilişkili sayfa sunucusu düğümleri eklenir.

### <a name="is-database-shrink-supported"></a>Veritabanı daraltımı destekleniyor

Hayır

### <a name="is-database-compression-supported"></a>Veritabanı sıkıştırması destekleniyor

Evet

### <a name="if-i-have-a-huge-table-does-my-table-data-get-spread-out-across-multiple-data-files"></a>Çok büyük bir tablonuz varsa, tablomın verileri birden fazla veri dosyasına yayılmaktadır

Evet. Belirli bir tabloyla ilişkili veri sayfaları, aynı dosya grubunun bir parçası olan birden çok veri dosyasında bitebilirler. SQL Server, verileri veri dosyaları üzerinden dağıtmak için [orantılı bir Fill stratejisi](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups#file-and-filegroup-fill-strategy) kullanır.

## <a name="data-migration-questions"></a>Veri geçişi soruları

### <a name="can-i-move-my-existing-azure-sql-databases-to-the-hyperscale-service-tier"></a>Mevcut Azure SQL veritabanlarınızı hiper ölçek hizmeti katmanına taşıyabilir miyim?

Evet. Mevcut Azure SQL veritabanlarınızı hiper ölçeğe taşıyabilirsiniz. Bu tek yönlü geçişdir. Veritabanlarını Hyperscale 'den başka bir hizmet katmanına taşıyamazsınız. Üretim veritabanlarınızın bir kopyasını oluşturmanızı ve kavram kanıtı (POCs) için Hiperscale 'e geçirmeniz önerilir.
  
### <a name="can-i-move-my-hyperscale-databases-to-other-editions"></a>Hiperscale veritabanlarınızı diğer sürümlere taşıyabilir miyim?

Hayır. Şu anda hiper ölçekli bir veritabanını başka bir hizmet katmanına taşıyamazsınız.

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Hiper ölçek hizmet katmanına geçişten sonra herhangi bir işlevi veya özelliği kaybetmem

Evet. Azure SQL veritabanı özelliklerinden bazıları, henüz uzun süreli saklama yedeklemesi dahil, ancak sınırlı olmamak üzere hiper ölçekte desteklenmez. Veritabanlarınızı Hyperscale 'e geçirdikten sonra, bu özellikler çalışmayı durdurur.  Bu sınırlamaların geçici olması beklenir.

### <a name="can-i-move-my--on-premises-sql-server-database-or-my-sql-server-virtual-machine-database-to-hyperscale"></a>Şirket içi SQL Server veritabanımı veya SQL Server sanal makine veritabanımı hiper ölçeğe taşıyabilir miyim?

Evet. BACPAC, işlemsel çoğaltma, mantıksal veri yükleme dahil olmak üzere hiper ölçeğe geçiş yapmak için var olan tüm geçiş teknolojilerini kullanabilirsiniz. Ayrıca bkz. [Azure veritabanı geçiş hizmeti](../dms/dms-overview.md).

### <a name="what-is-my-downtime-during-migration-from-an-on-premises-or-virtual-machine-environment-to-hyperscale-and-how-can-i-minimize-it"></a>Şirket içi veya sanal makine ortamından hiper ölçeğe geçiş sırasında kapalı kalma süresi nedir ve bunu nasıl en aza indirebilir

Etkin kalma süresi, veritabanlarınızı Azure SQL veritabanı 'nda tek bir veritabanına geçirirken kapalı kalma süresiyle aynıdır. Az sayıda TB ['a kadar](replication-to-sql-database.md#data-migration-scenario
) olan veritabanlarının kapalı kalma süresini en aza indirmek için işlemsel çoğaltmayı kullanabilirsiniz. Çok büyük veritabanı (10 + TB) için ADF, Spark veya diğer veri taşıma teknolojilerini kullanarak verileri geçirmeyi düşünebilirsiniz.

### <a name="how-much-time-would-it-take-to-bring-in-x-amount-of-data-to-sql-database-hyperscale"></a>X veri miktarını SQL veritabanı hiper ölçeğe getirmek için ne kadar sürer?

Hiper ölçek, 100 MB/sn yeni/değiştirilmiş veri tüketilebilir.

### <a name="can-i-read-data-from-blob-storage-and-do-fast-load-like-polybase-and-sql-data-warehouse"></a>Blob depolamadaki verileri okuyabilir ve hızlı yükleme yapabilir (PolyBase ve SQL veri ambarı gibi)

Azure depolama 'dan verileri okuyabilir ve veri yükünü bir hiper ölçekli veritabanına yükleyebilirsiniz (tıpkı normal tek bir veritabanıyla yapabilecekleriniz gibi). PolyBase Şu anda Azure SQL veritabanı 'nda desteklenmiyor. [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) kullanarak PolyBase 'i GERÇEKLEŞTIREBILIR veya [SQL için spark Bağlayıcısı](sql-database-spark-connector.md)ile [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/) bir Spark işi çalıştırabilirsiniz. SQL 'e Spark Bağlayıcısı toplu ekleme 'yi destekler.

Hiper ölçekte basit kurtarma veya toplu günlük modeli desteklenmez. Yüksek kullanılabilirlik sağlamak için tam kurtarma modeli gereklidir. Ancak, Hyperscale, yeni günlük mimarisi nedeniyle tek bir Azure SQL veritabanı ile karşılaştırıldığında daha iyi bir veri alma oranı sağlar.

### <a name="does-sql-database-hyperscale-allow-provisioning-multiple-nodes-for-ingesting-large-amounts-of-data"></a>SQL veritabanı hiper ölçek, büyük miktarlarda veriyi almak için birden çok düğüm sağlanmasına izin veriyor

Hayır. SQL veritabanı hiper ölçek bir SMP mimarisidir ve asimetrik çok işlem veya çok yöneticili mimari değildir. Yalnızca salt okuma iş yüklerini ölçeklendirmek için birden çok çoğaltma oluşturabilirsiniz.

### <a name="what-is-the-oldest-sql-server-version-will-sql-database-hyperscale-support-migration-from"></a>En eski SQL Server sürümü olan SQL veritabanı hiper ölçek desteği geçişi

SQL Server 2005. Daha fazla bilgi için bkz. [tek bir veritabanına veya havuza alınmış bir veritabanına geçirme](sql-database-single-database-migrate.md#migrate-to-a-single-database-or-a-pooled-database). Uyumluluk sorunları için bkz. [Veritabanı geçişi uyumluluk sorunlarını çözme](sql-database-single-database-migrate.md#resolving-database-migration-compatibility-issues).

### <a name="does-sql-database-hyperscale-support-migration-from-other-data-sources-such-as-aurora-mysql-oracle-db2-and-other-database-platforms"></a>SQL veritabanı hiper ölçek, Aurora, MySQL, Oracle, DB2 ve diğer veritabanı platformları gibi diğer veri kaynaklarından geçişi destekler

Evet. SQL Server dışındaki farklı veri kaynaklarından gelen bir mantıksal geçiş gerektirir. [Azure veritabanı geçiş hizmeti](../dms/dms-overview.md) 'ni mantıksal geçiş için kullanabilirsiniz.

## <a name="business-continuity-and-disaster-recovery-questions"></a>İş sürekliliği ve olağanüstü durum kurtarma soruları

### <a name="what-slas-are-provided-for-a-hyperscale-database"></a>Hiper ölçek veritabanı için hangi SLA 'lar sağlanır

Varsayılan birincil ve 1 okunabilir ikincil ile SLA% 99,95 kullanılabilir.  Daha fazla çoğaltma ile SLA% 99,99 ' e kadar gider.  

### <a name="are-the-database-backups-managed-for-me-by-the-azure-sql-database-service"></a>Veritabanı yedeklemeleri, Azure SQL veritabanı hizmeti tarafından benim için yönetiliyor

Evet

### <a name="how-often-are-the-database-backups-taken"></a>Veritabanı yedeklemeleri ne sıklıkta alınır

SQL veritabanı hiper ölçek veritabanları için geleneksel bir tam, değişiklik ve günlük yedeklemesi yoktur. Bunun yerine, veri dosyalarının düzenli anlık görüntüleri ve oluşturulan günlük, yapılandırılan veya sizin için kullanılabilen bekletme dönemi için olduğu gibi korunur.

### <a name="does-sql-database-hyperscale-support-point-in-time-restore"></a>SQL veritabanı hiper ölçek destek noktası zaman geri yükleme sırasında yapılır

Evet

### <a name="what-is-the-recovery-point-objective-rporecovery-time-objective-rto-with-backuprestore-in-sql-database-hyperscale"></a>SQL veritabanı hiper ölçeğinde yedekleme/geri yükleme ile kurtarma noktası hedefi (RPO)/kurtarma süresi hedefi (RTO) nedir?

RPO 0 dk. RTO hedefi, veritabanı boyutundan bağımsız olarak 10 dakikadan azdır. 

### <a name="do-backups-of-large-databases-affect-compute-performance-on-my-primary"></a>Büyük veritabanlarının yedeklemeleri, birincil My işlem performansını etkiler

Hayır. Yedeklemeler, depolama alt sistemi tarafından yönetilir ve dosya anlık görüntülerinden yararlanır. Birincil üzerinde Kullanıcı iş yükünü etkilemez.

### <a name="can-i-perform-geo-restore-with-a-sql-database-hyperscale-database"></a>Bir SQL veritabanı hiper ölçek veritabanıyla coğrafi geri yükleme yapabilir miyim

Evet.  Coğrafi geri yükleme tam olarak desteklenmektedir.

### <a name="can-i-setup-geo-replication-with-sql-database-hyperscale-database"></a>SQL veritabanı hiper ölçek veritabanı ile coğrafi çoğaltma ayarı yapabilir miyim

Şu anda değil.

### <a name="do-my-secondary-compute-nodes-get-geo-replicated-with-sql-database-hyperscale"></a>İkincil işlem düğümlerimi, SQL veritabanı Hyperscale ile coğrafi olarak çoğaltılmış olarak al

Şu anda değil.

### <a name="can-i-take-a-sql-database-hyperscale-database-backup-and-restore-it-to-my-on-premises-server-or-sql-server-in-vm"></a>Bir SQL veritabanı hiper ölçek veritabanı yedeklemesini alabilir ve şirket içi sunucuma veya VM 'de SQL Server geri yüklemenize olanak verebilir

Hayır. Hiper ölçekli veritabanlarının depolama biçimi geleneksel SQL Server farklıdır ve yedeklemeleri denetlemenize veya bunlara erişim sahibi kalmazsınız. Verilerinizi bir SQL veritabanı hiper ölçek veritabanından almak için, dışa aktarma hizmetini kullanın veya betik ve BCP kullanın.

## <a name="cross-feature-questions"></a>Çapraz Özellik soruları

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Hiper ölçek hizmet katmanına geçişten sonra herhangi bir işlevi veya özelliği kaybetmem

Evet. Azure SQL veritabanı özelliklerinden bazıları, uzun süreli saklama yedeklemesi dahil, ancak sınırlı olmamak üzere hiper ölçekte desteklenmez. Veritabanlarınızı Hyperscale 'e geçirdikten sonra, bu özellikler çalışmayı durdurur.

### <a name="will-polybase-work-with-sql-database-hyperscale"></a>PolyBase, SQL veritabanı hiper Ölçeklendirimiyle çalışır

Hayır. PolyBase, Azure SQL veritabanı 'nda desteklenmiyor.

### <a name="does-the-compute-have-support-for-r-and-python"></a>İşlem için R ve Python desteği vardır

Hayır. Azure SQL veritabanı 'nda R ve Python desteklenmez.

### <a name="are-the-compute-nodes-containerized"></a>İşlem düğümleri Kapsayıcılı

Hayır. Veritabanınız bir kapsayıcı değil işlem VM 'sinde bulunur.

## <a name="performance-questions"></a>Performans soruları

### <a name="how-much-throughput-can-i-push-on-the-largest-sql-database-hyperscale-compute"></a>En büyük SQL veritabanı hiper ölçek işlem üzerine ne kadar aktarım gönderebilirim?

Tutarlı bir 100 MB/sn değişiklik verisi gördük (işlem günlüğü veri oluşturma)

### <a name="how-many-iops-do-i-get-on-the-largest-sql-database-hyperscale-compute"></a>En büyük SQL veritabanı hiper ölçek işlem üzerinde kaç ıOPS edinebilirim?

IOPS ve GÇ gecikmesi, iş yükü desenlerine bağlı olarak değişir.  Erişilmesi gereken veriler, işlem önbelleğinde yereliyorsa, yerel SSD ile aynı GÇ desenleri olur.   

### <a name="does-my-throughput-get-affected-by-backups"></a>İş aktarım alanım yedeklerden etkilendi

Hayır. İşlem, işlem üzerindeki etkileri önlemek için depolama katmanından ayrılır.

### <a name="does-my-throughput-get-affected-as-i-provision-additional-compute-nodes"></a>Ek işlem düğümleri sağladığımda aktarım hızı etkilendi

Depolama paylaşıldığından ve birincil ve ikincil işlem düğümleri arasında doğrudan fiziksel çoğaltma olmadığı için, teknik olarak birincil düğümdeki aktarım hızı, okuma ölçeği düğümleri eklenerek etkilenmeyecektir. Ancak, ikincil düğümlerde ve sayfa sunucularında günlüğe kaydedilecek, ikincil düğümlerde hatalı okuma performansından kaçınmak için sürekli agresif iş yükünü kısıtlayamaz.

## <a name="scalability-questions"></a>Ölçeklenebilirlik soruları

### <a name="how-long-would-it-take-to-scale-up-and-down-a-compute-node"></a>İşlem düğümünün ölçeğini artırma ve azaltma işlemi ne kadar sürer?

İşlem ölçeği artırma veya azaltma, veri boyutundan bağımsız olarak 5-10 dakika sürer.

### <a name="is-my-database-offline-while-the-scaling-updown-operation-is-in-progress"></a>Ölçeklendirme artırma/azaltma işlemi devam ederken Veritabanım çevrimdışı

Hayır. Ölçeği artırma ve azaltma çevrimiçi olacak.

### <a name="should-i-expect-connection-drop-when-the-scaling-operations-are-in-progress"></a>Ölçeklendirme işlemleri sürerken bağlantı bırakması beklenmeli

Yük devretme işlemi, hedef boyutu ile işlem düğümü olduğunda, mevcut bağlantıların ölçeğini artırma veya azaltma. Okuma çoğaltmaları eklemek, bağlantı düşmelere neden olmaz.

### <a name="is-the-scaling-up-and-down-of-compute-nodes-automatic-or-end-user-triggered-operation"></a>İşlem düğümlerinin otomatik veya son kullanıcı tarafından tetiklenen bir şekilde ölçeğini artırma ve azaltma

Son Kullanıcı. Otomatik değil.  

### <a name="does-my-tempb-also-grow-as-the-compute-is-scaled-up"></a>İşlem büyüdükçe `tempb` da büyür

Evet. İşlem büyüdükçe geçici db otomatik olarak artar.  

### <a name="can-i-provision-multiple-primary-compute-nodes-such-as-a-multi-master-system-where-multiple-primary-compute-heads-can-drive-a-higher-level-of-concurrency"></a>Birden çok birincil işlem yöneticisinin daha yüksek bir eşzamanlılık düzeyini sağlayabildiği çok yöneticili bir sistem gibi birden çok birincil işlem düğümü sağlayabilir miyim

Hayır. Yalnızca birincil işlem düğümü okuma/yazma isteklerini kabul eder. İkincil işlem düğümleri yalnızca salt okuma isteklerini kabul eder.

## <a name="read-scale-questions"></a>Okuma Ölçeği soruları

### <a name="how-many-secondary-compute-nodes-can-i-provision"></a>Kaç tane ikincil işlem düğümü sağlayabilirim?

Varsayılan olarak hiper ölçekli veritabanları için 2 çoğaltma oluşturacağız. Kopyaların sayısını ayarlamak istiyorsanız [Azure Portal](https://portal.azure.com)kullanarak bunu yapabilirsiniz.

### <a name="how-do-i-connect-to-these-secondary-compute-nodes"></a>Nasıl yaparım? bu ikincil işlem düğümlerine bağlanın

Bağlantı dizinizdeki `ApplicationIntent` bağımsız değişkeni olarak `readonly`ayarlayarak, bu ek salt okuma işlem düğümlerine bağlanabilirsiniz. İle `readonly` işaretlenen tüm bağlantılar otomatik olarak ek salt okuma işlem düğümlerinden birine yönlendirilir.  

### <a name="how-do-i-validate-if-i-have-successfully-connected-to-secondary-compute-node-using-ssms--other-client-tools"></a>SSMS/diğer istemci araçlarını kullanarak ikincil işlem düğümüne başarılı bir şekilde bağlanıp bağlanmadığı Nasıl yaparım? doğrulayın.

SSMS/diğer istemci araçlarını kullanarak aşağıdaki T-SQL sorgusunu çalıştırabilirsiniz: `SELECT DATABASEPROPERTYEX ( '<database_name>' , 'updateability' )`.
Bunun sonucu `READ_ONLY` , bağlantınızın salt okunurdur ikincil düğümü işaret ediyor ya da `READ_WRITE` bağlantınız birincil düğümü işaret ediyor.

### <a name="can-i-create-a-dedicated-endpoint-for-the-read-scale-replica"></a>Okuma ölçeği çoğaltması için adanmış bir uç nokta oluşturabilir miyim

Hayır. Yalnızca öğesini belirterek `ApplicationIntent=ReadOnly`okuma ölçeğinde çoğaltmaya bağlanabilirsiniz.

### <a name="does-the-system-do-intelligent-load-balancing-of-the-read-workload"></a>Sistem, okuma iş yükünün akıllı yük dengelemesini yapar

Hayır. Salt okuma iş yükü rastgele bir okuma ölçeği çoğaltmasına yönlendirilir.

### <a name="can-i-scale-updown-the-secondary-compute-nodes-independently-of-the-primary-compute"></a>İkincil işlem düğümlerinin birincil işlem dışında bir şekilde ölçeğini artırma/azaltma yapabilir miyim?

Hayır. İkincil işlem düğümleri aynı zamanda HA için de kullanılır, bu nedenle yük devretme durumunda birincil ile aynı yapılandırma olmaları gerekir.

### <a name="do-i-get-different-temp-db-sizing-for-my-primary-compute-and-my-additional-secondary-compute-nodes"></a>Birincil işlem ve ek ikincil işlem düğümlerim için farklı geçici db Boyutlandırımı alıyorum

Hayır. `tempdb` İşlem boyutu sağlama temel alınarak yapılandırılır, ikincil işlem düğümleriniz birincil işlem ile aynı boyutlardır.

### <a name="can-i-add-indexes-and-views-on-my-secondary-compute-nodes"></a>İkincil işlem düğümlerimde dizin ve görünüm ekleyebilir miyim?

Hayır. Hiper ölçek veritabanlarının paylaşılan depolama alanı vardır ve tüm işlem düğümlerinin aynı tabloları, dizinleri ve görünümleri görmeyeceği anlamına gelir. İkincil üzerinde okuma için iyileştirilmiş ek dizinler istiyorsanız, bunları birinciye eklemeniz gerekir.

### <a name="how-much-delay-is-there-going-to-be-between-the-primary-and-secondary-compute-node"></a>Birincil ve ikincil işlem düğümü arasında ne kadar gecikme olur?

Günlük oluşturma hızına bağlı olarak, bir işlemin birincil üzerinde yürütüldüğü zamandan itibaren, anlık veya düşük milisaniyelik olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Hyperscale hizmet katmanı hakkında daha fazla bilgi için bkz. [Hyperscale hizmet katmanı](sql-database-service-tier-hyperscale.md).
