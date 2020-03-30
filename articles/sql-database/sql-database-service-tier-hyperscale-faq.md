---
title: Azure SQL Veritabanı Hiper ölçekli SSS
description: Müşterilerin Hyperscale hizmet katmanındaki bir Azure SQL veritabanı hakkında sorduğu sık sorulan soruların yanıtları -genellikle Hyperscale veritabanı olarak adlandırılır).
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dimitri-furman
ms.author: dfurman
ms.reviewer: ''
ms.date: 03/03/2020
ms.openlocfilehash: 9f518df02b1923513fd014be53646a9a1be8465e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268632"
---
# <a name="azure-sql-database-hyperscale-faq"></a>Azure SQL Veritabanı Hiper ölçekli SSS

Bu makalede, bu SSS'nin geri kalanında yalnızca Hyperscale olarak adlandırılan Azure SQL Veritabanı Hiper ölçek hizmet katmanında veritabanı nı düşünen müşteriler için sık sorulan soruların yanıtları verilmektedir. Bu makalede, Hyperscale'in desteklediği senaryolar ve Hyperscale ile uyumlu özellikler açıklanmaktadır.

- Bu SSS, Hiperölçekli hizmet katmanı hakkında kısa bir anlayışa sahip olan ve özel sorularının ve endişelerinin yanıtlanmasını isteyen okuyucular için tasarlanmıştır.
- Bu SSS bir rehber kitap veya Hyperscale veritabanı nasıl kullanılacağı ile ilgili soruları yanıtlamak için dir. Hyperscale'e giriş için Azure SQL [Veritabanı Hiper ölçek](sql-database-service-tier-hyperscale.md) belgelerine başvurmanızı öneririz.

## <a name="general-questions"></a>Genel Sorular

### <a name="what-is-a-hyperscale-database"></a>Hiper ölçekli veritabanı nedir

Hyperscale veritabanı, Hyperscale hizmet katmanında bulunan ve Hyperscale ölçeklendirilmiş depolama teknolojisi tarafından desteklenen bir Azure SQL veritabanıdır. Hyperscale veritabanı 100 TB'a kadar veriyi destekler ve yüksek iş verisi ve performansının yanı sıra iş yükü gereksinimlerine uyum sağlamak için hızlı ölçeklendirme sağlar. Ölçekleme uygulama için saydamdır – bağlantı, sorgu işleme, vb. diğer Azure SQL veritabanı gibi çalışır.

### <a name="what-resource-types-and-purchasing-models-support-hyperscale"></a>Hangi kaynak türleri ve satın alma modelleri Hyperscale'i destekler

Hyperscale hizmet katmanı, Azure SQL Veritabanı'ndaki vCore tabanlı satın alma modelini kullanan tek veritabanları için kullanılabilir.  

### <a name="how-does-the-hyperscale-service-tier-differ-from-the-general-purpose-and-business-critical-service-tiers"></a>Hyperscale hizmet katmanı nın Genel Amaç ve İş Açısından Kritik hizmet katmanlarından farkı nedir?

vCore tabanlı hizmet katmanları, aşağıdaki tabloda açıklandığı gibi veritabanı kullanılabilirliği ve depolama türüne, performansına ve maksimum boyutuna göre farklılaştırılır.

| | Kaynak türü | Genel Amaçlı |  Hiper Ölçek | İş Kritik |
|:---:|:---:|:---:|:---:|:---:|
| **En iyisi** |Tümü|Bütçe odaklı dengeli işlem ve depolama seçenekleri sunar.|Çoğu iş yükü. Depolama boyutunu 100 TB'a kadar otomatikleştirin, hızlı dikey ve yatay işlem ölçekleme, hızlı veritabanı geri yükleme.|Yüksek işlem hızı ve düşük IO gecikme süresi ile OLTP uygulamaları. Birden çok eşzamanlı olarak güncellenen yinelemeler kullanarak hatalara ve hızlı hatalara karşı en yüksek esneklik sağlar.|
|  **Kaynak türü** ||Tek veritabanı / elastik havuz / yönetilen örnek | Tek veritabanı | Tek veritabanı / elastik havuz / yönetilen örnek |
| **İşlem boyutu**|Tek veritabanı / elastik havuz * | 1 - 80 vCores | 1 ile 80 vCores* | 1 - 80 vCores |
| |Yönetilen örnek | 8, 16, 24, 32, 40, 64, 80 vCores | Yok | 8, 16, 24, 32, 40, 64, 80 vCores |
| **Depolama türü** | Tümü |Premium uzak depolama (örnek başına) | Yerel SSD önbelleğiyle birleştirilmiş depolama alanı (örneğinbaşına) | Süper hızlı yerel SSD depolama (örnek başına) |
| **Depolama boyutu** | Tek veritabanı / elastik havuz *| 5 GB – 4 TB | 100 TB'a kadar | 5 GB – 4 TB |
| | Yönetilen örnek  | 32 GB – 8 TB | Yok | 32 GB – 4 TB |
| **IOPS** | Tek veritabanı | 7000 maksimum IOPS ile vCore başına 500 IOPS | Hiperölçek, birden çok düzeyde önbelleğe alınmış çok katmanlı bir mimaridir. Etkili IOPS iş yüküne bağlıdır. | 200.000 maksimum IOPS ile 5000 IOPS|
| | Yönetilen örnek | Dosya boyutuna bağlı | Yok | 1375 IOPS/vCore |
|**Kullanılabilir -lik**|Tümü|1 çoğaltma, hiçbir Okuma Ölçeği-out, hiçbir yerel önbellek | Birden çok yineleme, en fazla 4 Okuma Ölçeği, kısmi yerel önbellek | 3 çoğaltma, 1 Okuma Ölçeklendirme, bölge yedekli HA, tam yerel depolama |
|**Yedeklemeler**|Tümü|RA-GRS, 7-35 gün bekletme (varsayılan olarak 7 gün)| RA-GRS, 7 günlük tutma, sabit zaman noktası zamanlı kurtarma (PITR) | RA-GRS, 7-35 gün bekletme (varsayılan olarak 7 gün) |

\*Elastik havuzlar Hyperscale servis katmanında desteklenmez

### <a name="who-should-use-the-hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanını kimler kullanmalı?

Hyperscale hizmet katmanı, büyük şirket içi SQL Server veritabanlarına sahip olan ve buluta taşınarak uygulamalarını modernize etmek isteyen veya Azure SQL Veritabanı'nı kullanmakta olan ve veritabanı büyüme potansiyeli. Hiperölçek aynı zamanda hem yüksek performans hem de yüksek ölçeklenebilirlik arayan müşteriler için tasarlanmıştır. Hyperscale ile şunları elde eeesiniz:

- 100 TB'a kadar veritabanı boyutu
- Veritabanı boyutundan bağımsız olarak hızlı veritabanı yedeklemeleri (yedeklemeler depolama anlık görüntülerine dayanır)
- Veritabanı boyutundan bağımsız olarak hızlı veritabanı geri yüklemeleri (geri yüklemeler depolama anlık görüntülerinden)
- Veritabanı boyutu ve vCoresayısı ne olursa olsun daha yüksek günlük girişi
- Okuma boşaltma ve sıcak bekleme olarak kullanılan bir veya daha fazla salt okunur yinelemeyi kullanarak Ölçeklendirme'yi okuyun.
- Ağır iş yükünü karşılamak için daha güçlü olmak ve daha sonra sabit bir süre içinde küçültmek için, sabit bir süre içinde, hesaplamanın hızlı bir şekilde ölçeklendirin. Bu, örneğin bir P6 ve P11 arasında yukarı ve aşağı ölçeklemeye benzer, ancak bu bir veri işlemi boyutu olmadığı için çok daha hızlıdır.

### <a name="what-regions-currently-support-hyperscale"></a>Şu anda Hiperölçek'i destekleyen bölgeler

Hyperscale hizmet katmanı şu anda [Azure SQL Veritabanı Hiper ölçekli genel bakış](sql-database-service-tier-hyperscale.md#regions)altında listelenen bölgelerde kullanılabilir.

### <a name="can-i-create-multiple-hyperscale-databases-per-logical-server"></a>Mantıksal sunucu başına birden çok Hiper ölçekli veritabanları oluşturabilir miyim

Evet. Mantıksal sunucu başına Hiperölçekli veritabanlarının sayısı hakkında daha fazla bilgi ve sınırlar için, [mantıksal bir sunucuda tek ve havuza alınan veritabanları için SQL Veritabanı kaynak sınırlarına](sql-database-resource-limits-logical-server.md)bakın.

### <a name="what-are-the-performance-characteristics-of-a-hyperscale-database"></a>Hiper ölçekli veritabanının performans özellikleri nelerdir

Hiperölçek mimarisi, büyük veritabanı boyutlarını desteklerken yüksek performans ve iş elde etme sağlar. 

### <a name="what-is-the-scalability-of-a-hyperscale-database"></a>Hiper ölçekleme veritabanının ölçeklenebilirliği nedir?

Hiperölçek, iş yükü talebinize bağlı olarak hızlı ölçeklenebilirlik sağlar.

- **Yukarı/Aşağı Ölçekleme**

  Hyperscale ile, birincil işlem boyutunu CPU ve bellek gibi kaynaklar açısından ölçeklendirebilir ve sabit bir süre içinde küçültebilirsiniz. Depolama paylaşıldığından, ölçekleme ve ölçekleme veri işlemi boyutu değildir.  
- **İç/Dış Ölçekleme**

  Hyperscale ile, okuma isteklerinize hizmet etmek için kullanabileceğiniz bir veya daha fazla ek işlem yinelemesini sağlama olanağı da elde elabilirsiniz. Bu, okundu iş yükünüzü birincil işlemden boşaltmak için bu ek bilgi işlem yinelemelerini salt okunur yinelemeler olarak kullanabileceğiniz anlamına gelir. Salt okunur alamete ek olarak, bu yinelemeler birincil den bir başarısızlık durumunda sıcak bekleme olarak da hizmet vermektedir.

  Bu ek bilgi işlem yinelemelerinin her birinin sağlanması sürekli olarak yapılabilir ve çevrimiçi bir işlemdir. Bağlantı dizenizdeki bağımsız değişkeni `ApplicationIntent` 'ye ayarlayarak bu ek salt `ReadOnly`okunur işlem yinelemelerine bağlanabilirsiniz. `ReadOnly` Uygulama amacıyla yapılan tüm bağlantılar otomatik olarak salt okunur işlem yinelemelerinden birine yönlendirilir.

## <a name="deep-dive-questions"></a>Derin Dalış Soruları

### <a name="can-i-mix-hyperscale-and-single-databases-in-a-single-logical-server"></a>Hiperölçek ve tek veritabanlarını tek bir mantıksal sunucuda karıştırabilir miyim?

Evet, bunu yapabilirsiniz.

### <a name="does-hyperscale-require-my-application-programming-model-to-change"></a>Hyperscale, uygulama programlama modelimin değişmesini gerektirir mi?

Hayır, uygulama programlama modeliniz olduğu gibi kalır. Bağlantı dizenizi her zamanki gibi ve Hiper ölçek veritabanınızla etkileşim kurmanın diğer normal yollarını kullanırsınız.

### <a name="what-transaction-isolation-level-is-the-default-in-a-hyperscale-database"></a>Bir Hiper ölçek veritabanında varsayılan işlem yalıtım düzeyi nedir

Birincil yinelemede, varsayılan işlem yalıtım düzeyi RCSI 'dir (Kararlı Anlık Görüntü Yalıtımını Oku). Okuma Ölçeği ikincil yinelemelerinde varsayılan yalıtım düzeyi Anlık Görüntü'dür.

### <a name="can-i-bring-my-on-premises-or-iaas-sql-server-license-to-hyperscale"></a>Şirket içi veya IaaS SQL Server lisansımı Hyperscale'e getirebilir miyim?

Evet, [Azure Karma Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) Hyperscale için kullanılabilir. Her SQL Server Standart çekirdek 1 Hyperscale vCores eşleyebilir. Her SQL Server Enterprise çekirdeği 4 Hyperscale vCore'a eşleyebilir. İkincil yinelemeler için SQL lisansına ihtiyacınız yoktur. Azure Karma Avantajı fiyatı, Okuma Ölçeği (ikincil) yinelemelerine otomatik olarak uygulanır.

### <a name="what-kind-of-workloads-is-hyperscale-designed-for"></a>Hyperscale ne tür iş yükleri için tasarlanmıştır

Hyperscale tüm SQL Server iş yüklerini destekler, ancak öncelikle OLTP için optimize edin. Hibrit (HTAP) ve Analitik (veri mart) iş yüklerini de getirebilirsiniz.

### <a name="how-can-i-choose-between-azure-sql-data-warehouse-and-azure-sql-database-hyperscale"></a>Azure SQL Veri Ambarı ve Azure SQL Veritabanı Hiper Ölçeği arasında nasıl seçim yapabilirim?

Şu anda veri ambarı olarak SQL Server'ı kullanarak etkileşimli analitik sorgular çalıştırıyorsanız, Hiperölçek harika bir seçenektir, çünkü küçük ve orta ölçekli veri ambarlarını (birkaç TB'den 100 TB'ye kadar) daha düşük bir maliyetle barındırabilirsiniz ve SQL Server verilerinizi aktarabilirsiniz en az T-SQL kodu değişiklikleriyle Hyperscale'e depo iş yükleri.

Karmaşık sorgularla ve 100 MB/s'den daha yüksek sürekli yutma oranlarıyla büyük ölçekte veri analitiği çalıştırıyorsanız veya Paralel Veri Ambarı (PDW), Teradata veya diğer Massively Parallel Processing (MPP) veri ambarlarını kullanıyorsanız, SQL Veri Ambarı en iyi seçim olabilir.
  
## <a name="hyperscale-compute-questions"></a>Hiper ölçekli bilgi işlem soruları

### <a name="can-i-pause-my-compute-at-any-time"></a>İşlemimi herhangi bir zamanda duraklatabilir miyim?

Şu anda değil, ancak en yoğun olmayan zamanlarda maliyeti azaltmak için işlem ve çoğaltma sayısını küçültebilirsiniz.

### <a name="can-i-provision-a-compute-replica-with-extra-ram-for-my-memory-intensive-workload"></a>Bellek yoğun iş yüküiçin ekstra RAM ile bir işlem çoğaltma sağlayabilir miyim

Hayır. Daha fazla RAM almak için daha yüksek bir işlem boyutuna yükseltmeniz gerekir. Daha fazla bilgi [için, Hyperscale depolama ve bilgi işlem boyutlarına](sql-database-vcore-resource-limits-single-databases.md#hyperscale---provisioned-compute---gen5)bakın.

### <a name="can-i-provision-multiple-compute-replicas-of-different-sizes"></a>Farklı boyutlarda birden fazla işlem yinelemesi sağlayabilir miyim

Hayır.

### <a name="how-many-read-scale-out-replicas-are-supported"></a>Kaç Okuma Ölçeği-out yinelemesi desteklenir

Hyperscale veritabanları varsayılan olarak bir Okuma Ölçeği-out yinelemesi (birincil dahil iki yineleme) ile oluşturulur. [Azure portalı](https://portal.azure.com) veya [REST API'yi](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)kullanarak salt okunur yineleme sayısını 0 ile 4 arasında ölçeklendirebilirsiniz.

### <a name="for-high-availability-do-i-need-to-provision-additional-compute-replicas"></a>Yüksek kullanılabilirlik için, ek bilgi işlem yinelemeleri sağlamam gerekiyor mu?

Hiper ölçekli veritabanlarında, veri esnekliği depolama düzeyinde sağlanır. Esneklik sağlamak için yalnızca bir kopyaya ihtiyacınız var. İşlem yinelemesi çöktüğinde, veri kaybı olmadan otomatik olarak yeni bir yineleme oluşturulur.

Ancak, yalnızca bir yineleme varsa, başarısız olduktan sonra yeni yinelemede yerel önbelleği oluşturmak biraz zaman alabilir. Önbellek yeniden oluşturma aşamasında, veritabanı verileri doğrudan sayfa sunucularından alarak daha yüksek depolama gecikmesi ve azalan sorgu performansıyla sonuçlanır.

En az arıza etkisiyle yüksek kullanılabilirlik gerektiren görev açısından kritik uygulamalar için, birincil işlem yinelemesi de dahil olmak üzere en az 2 işlem yinelemesi sağlamanız gerekir. Bu, varsayılan yapılandırmadır. Bu şekilde bir failover hedef olarak hizmet veren bir sıcak bekleme çoğaltma kullanılabilir.

## <a name="data-size-and-storage-questions"></a>Veri Boyutu ve Depolama Soruları

### <a name="what-is-the-maximum-database-size-supported-with-hyperscale"></a>Hyperscale ile desteklenen maksimum veritabanı boyutu nedir

100 TB.

### <a name="what-is-the-size-of-the-transaction-log-with-hyperscale"></a>Hyperscale ile işlem günlüğünün boyutu nedir

Hyperscale ile işlem günlüğü neredeyse sonsuzdur. Yüksek günlük elde etme kaynağı olan bir sistemde günlük alanının tükenme konusunda endişelenmenize gerek yoktur. Ancak, günlük oluşturma oranı sürekli agresif iş yükleri yazmak için daraltılmış olabilir. En yüksek sürekli günlük üretim hızı 100 MB/sn'dir.

### <a name="does-my-tempdb-scale-as-my-database-grows"></a>Veritabanım `tempdb` büyüdükçe ölçeğim ilerler mi

Veritabanınız `tempdb` yerel SSD depolama da bulunur ve sizin sağlama yaptığınız işlem boyutuyla orantılı olarak boyutlandırılır. Sizin `tempdb` maksimum performans avantajları sağlamak için optimize edin. `tempdb`boyutu yapılandırılabilir değildir ve sizin için yönetilir.

### <a name="does-my-database-size-automatically-grow-or-do-i-have-to-manage-the-size-of-data-files"></a>Veritabanı boyutum otomatik olarak büyür mü, yoksa veri dosyalarının boyutunu yönetmem mi gerekiyor

Daha fazla veri ekledikçe/yuttukça veritabanı boyutunuz otomatik olarak büyür.

### <a name="what-is-the-smallest-database-size-that-hyperscale-supports-or-starts-with"></a>Hyperscale'in desteklediği veya başladığı en küçük veritabanı boyutu nedir?

40 GB. Bir Hyperscale veritabanı 10 GB başlangıç boyutu ile oluşturulur. Daha sonra, 40 GB boyutuna ulaşana kadar, her 10 dakikada bir 10 GB büyümeye başlar. Bu 10 GB aynaların her biri, daha fazla IOPS ve daha yüksek G/Ç paralelliği sağlamak için farklı bir sayfa sunucusuna ayrılmıştır. Bu optimizasyon nedeniyle, ilk veritabanı boyutunu 40 GB'dan küçük seçseniz bile, veritabanı otomatik olarak en az 40 GB'a büyür.

### <a name="in-what-increments-does-my-database-size-grow"></a>Veritabanı boyutum hangi artışlarla büyür

Her veri dosyası 10 GB büyür. Aynı anda birden çok veri dosyası büyüyebilir.

### <a name="is-the-storage-in-hyperscale-local-or-remote"></a>Hyperscale'deki depolama alanı yerel mi yoksa uzak mı?

Hyperscale'de veri dosyaları Azure standart depolama alanında depolanır. Veriler, yerel SSD depolama da, bilgi işlem yinelemelerine yakın sayfa sunucularında tamamen önbelleğe alınır. Buna ek olarak, bilgi işlem yinelemeleri uzak sayfa sunucularından veri alma sıklığını azaltmak için yerel SSD ve bellekte veri önbellekleri vardır.

### <a name="can-i-manage-or-define-files-or-filegroups-with-hyperscale"></a>Hiperölçekli dosya veya dosya gruplarını yönetebilir veya tanımlayabilir miyim?

Hayır. Veri dosyaları otomatik olarak eklenir. Ek dosya grupları oluşturmanın yaygın nedenleri Hyperscale depolama mimarisinde geçerli değildir.

### <a name="can-i-provision-a-hard-cap-on-the-data-growth-for-my-database"></a>Veritabanım için veri büyümesi için sabit bir kapak sağlayabilir miyim

Hayır.

### <a name="how-are-data-files-laid-out-with-hyperscale"></a>Hyperscale ile veri dosyaları nasıl düzenlenir?

Veri dosyaları, veri dosyası başına bir sayfa sunucusu yla sayfa sunucuları tarafından denetlenir. Veri boyutu büyüdükçe, veri dosyaları ve ilişkili sayfa sunucuları eklenir.

### <a name="is-database-shrink-supported"></a>Veritabanı küçültme destekli mi

Hayır.

### <a name="is-data-compression-supported"></a>Veri sıkıştırma destekli mi

Evet, satır, sayfa ve sütun mağazası sıkıştırma dahil.

### <a name="if-i-have-a-huge-table-does-my-table-data-get-spread-out-across-multiple-data-files"></a>Büyük bir tablom varsa, tablo verilerim birden çok veri dosyasına yayılır mı?

Evet. Belirli bir tabloyla ilişkili veri sayfaları, tümü aynı dosya grubunun parçası olan birden çok veri dosyasına eklenebilir. SQL Server, verileri veri dosyaları üzerinden dağıtmak için [orantılı doldurma stratejisi](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups#file-and-filegroup-fill-strategy) kullanır.

## <a name="data-migration-questions"></a>Veri Geçişi Soruları

### <a name="can-i-move-my-existing-azure-sql-databases-to-the-hyperscale-service-tier"></a>Mevcut Azure SQL veritabanlarımı Hyperscale hizmet katmanına taşıyabilir miyim?

Evet. Varolan Azure SQL veritabanlarınızı Hyperscale'e taşıyabilirsiniz. Bu tek yönlü bir geçiş. Veritabanlarını Hyperscale'den başka bir hizmet katmanına taşıyamazsınız. Kavram kanıtları (POCs) için veritabanınızın bir kopyasını oluşturmanızı ve kopyayı Hyperscale'e geçirmenizi öneririz.
  
### <a name="can-i-move-my-hyperscale-databases-to-other-service-tiers"></a>Hiper ölçekli veritabanlarımı diğer hizmet katmanlarına taşıyabilir miyim?

Hayır. Şu anda, bir Hiper ölçekli veritabanını başka bir hizmet katmanına taşıyamazsınız.

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanına geçişten sonra herhangi bir işlevselliği veya yeteneği mi kaybediyorum?

Evet. Azure SQL Veritabanı özelliklerinin bazıları, uzun süreli yedekleme bekletme dahil ancak bunlarla sınırlı olmamak üzere henüz Hyperscale'de desteklenmez. Veritabanlarınızı Hyperscale'e geçirtmenizden sonra bu özellikler çalışmayı durdurur.  Bu sınırlamaların geçici olmasını bekliyoruz.

### <a name="can-i-move-my-on-premises-sql-server-database-or-my-sql-server-database-in-a-cloud-virtual-machine-to-hyperscale"></a>Şirket içi SQL Server veritabanımı veya SQL Server veritabanımı bir bulut sanal makinesinde Hyperscale'e taşıyabilir miyim?

Evet. İşlem çoğaltma ve diğer veri hareketi teknolojileri (Toplu Kopya, Azure Veri Fabrikası, Azure Veri Tuğlaları, SSIS) dahil olmak üzere, Hyperscale'e geçiş yapmak için varolan tüm geçiş teknolojilerini kullanabilirsiniz. Ayrıca, birçok geçiş senaryosunu destekleyen [Azure Veritabanı Geçiş Hizmeti'ne](../dms/dms-overview.md)de bakın.

### <a name="what-is-my-downtime-during-migration-from-an-on-premises-or-virtual-machine-environment-to-hyperscale-and-how-can-i-minimize-it"></a>Şirket içi veya sanal makine ortamından Hyperscale'e geçiş sırasında ki kapalı kalma sürem nedir ve bunu nasıl en aza indirebilirim

Hiper ölçek'e geçiş için kapalı kalma süresi, veritabanlarınızı diğer Azure SQL Veritabanı hizmet katmanlarına geçirdiğinizde ki kapalı kalma süresiyle aynıdır. Birkaç TB boyutuna kadar olan veritabanları için kapalı kalma süresini en aza indirmek için [işlem çoğaltmak](replication-to-sql-database.md#data-migration-scenario
) kullanabilirsiniz. Çok büyük veritabanları (10+ TB) için, ADF, Spark veya diğer veri hareketi teknolojilerini kullanarak veri geçişini düşünebilirsiniz.

### <a name="how-much-time-would-it-take-to-bring-in-x-amount-of-data-to-hyperscale"></a>Hyperscale'e X miktarda veri getirmek ne kadar zaman alır?

Hyperscale 100 MB/s yeni/değiştirilmiş veri tüketebilir, ancak verileri Azure SQL veritabanlarına taşımak için gereken süre de kullanılabilir ağ oluşturma, kaynak okuma hızı ve hedef veritabanı hizmet düzeyi hedefinden etkilenir.

### <a name="can-i-read-data-from-blob-storage-and-do-fast-load-like-polybase-in-sql-data-warehouse"></a>Blob depolamadan gelen verileri okuyabilir miyim ve hızlı yükleme yapabilir miyim (SQL Veri Ambarı'ndaki Polybase gibi)

Bir istemci uygulamasının Azure Depolama'daki verileri okumasını ve bir Hyperscale veritabanına veri yüklemesini sağlayabilirsiniz (tıpkı diğer Azure SQL veritabanında yaptığınız gibi). Polybase şu anda Azure SQL Veritabanı'nda desteklenmez. Hızlı yük sağlamak için alternatif olarak Azure [Veri Fabrikası'nı](https://docs.microsoft.com/azure/data-factory/)kullanabilir veya SQL için [Spark bağlayıcısıyla](sql-database-spark-connector.md) [Azure Databricks'te](https://docs.microsoft.com/azure/azure-databricks/) bir Kıvılcım işi kullanabilirsiniz. SQL'e spark konektörü toplu eklemeyi destekler.

Toplu INSERT veya OPENROWSET: [Azure Blob Depolama'da Verilere Toplu Erişim Örnekleri'ni](https://docs.microsoft.com/sql/relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage?view=sql-server-2017#accessing-data-in-a-csv-file-referencing-an-azure-blob-storage-location)kullanarak Azure Blob mağazasından toplu okuma verileri de mümkündür.

Basit kurtarma veya toplu günlük modeli Hyperscale desteklenmez. Tam kurtarma modeli yüksek kullanılabilirlik ve nokta-in-time kurtarma sağlamak için gereklidir. Ancak, Hyperscale günlük mimarisi, diğer Azure SQL Veritabanı hizmet katmanlarına kıyasla daha iyi veri alma oranı sağlar.

### <a name="does-hyperscale-allow-provisioning-multiple-nodes-for-parallel-ingesting-of-large-amounts-of-data"></a>Hiperölçek, büyük miktarda verinin paralel olarak sindirilmesi için birden çok düğüm sağlanmasına izin verir mi?

Hayır. Hiperölçek simetrik çok işleme (SMP) mimarisidir ve büyük ölçüde paralel bir işleme (MPP) veya çok ana mimari değildir. Salt okunur iş yüklerini ölçeklendirmek için yalnızca birden çok yineleme oluşturabilirsiniz.

### <a name="what-is-the-oldest-sql-server-version-supported-for-migration-to-hyperscale"></a>Hyperscale'e geçiş için desteklenen en eski SQL Server sürümü nedir

SQL Server 2005. Daha fazla bilgi için bkz: ["Geçir" tek bir veritabanına veya havuza alınan veritabanına.](sql-database-single-database-migrate.md#migrate-to-a-single-database-or-a-pooled-database) Uyumluluk sorunları için veritabanı [geçiş uyumluluk sorunlarını çözme konusuna](sql-database-single-database-migrate.md#resolving-database-migration-compatibility-issues)bakın.

### <a name="does-hyperscale-support-migration-from-other-data-sources-such-as-amazon-aurora-mysql-postgresql-oracle-db2-and-other-database-platforms"></a>Hyperscale, Amazon Aurora, MySQL, PostgreSQL, Oracle, DB2 ve diğer veritabanı platformları gibi diğer veri kaynaklarından geçişi destekliyor mu?

Evet. [Azure Veritabanı Geçiş Hizmeti](../dms/dms-overview.md) birçok geçiş senaryosudestekler.

## <a name="business-continuity-and-disaster-recovery-questions"></a>İş Sürekliliği ve Afet Kurtarma Soruları

### <a name="what-slas-are-provided-for-a-hyperscale-database"></a>Hiper ölçekli veritabanı için sağlanan SLA'lar

[Azure SQL Veritabanı için SLA'ya](https://azure.microsoft.com/support/legal/sla/sql-database/v1_4/)bakın. İki veya daha fazla ikincil işlem yinelemesine sahip bir veritabanı için %99,99'a kadar ek ikincil işlem yinelemeleri kullanılabilirliği artırır.

### <a name="are-the-database-backups-managed-for-me-by-the-azure-sql-database-service"></a>Veritabanı yedeklemeleri azure SQL Veritabanı hizmeti tarafından benim için yönetiliyor mu?

Evet.

### <a name="how-often-are-the-database-backups-taken"></a>Veritabanı yedeklemeleri ne sıklıkta alınır?

Hiperölçekli veritabanları için geleneksel tam, diferansiyel ve günlük yedeklemesi yoktur. Bunun yerine, veri dosyalarının düzenli depolama anlık görüntüleri vardır. Oluşturulan günlük, yapılandırılmış bekletme süresi için olduğu gibi korunur ve bekletme süresi içinde herhangi bir zaman anına geri yükleme sağlar.

### <a name="does-hyperscale-support-point-in-time-restore"></a>Zaman geri yüklemede Hyperscale destek noktası mı

Evet.

### <a name="what-is-the-recovery-point-objective-rporecovery-time-objective-rto-for-database-restore-in-hyperscale"></a>Hyperscale veritabanı geri yükleme için Kurtarma Noktası Hedefi (RPO)/Kurtarma Süresi Hedefi (RTO) nedir

RPO 0 dk. RTO hedefi, veritabanı boyutuna bakılmaksızın 10 dakikadan daha azdır. 

### <a name="does-database-backup-affect-compute-performance-on-my-primary-or-secondary-replicas"></a>Veritabanı yedeklemesi birincil veya ikincil yinelemelerimdeki işlem performansını etkiler mi?

Hayır. Yedeklemeler depolama alt sistemi tarafından yönetilir ve depolama anlık görüntülerinden yararlanır. Kullanıcı iş yüklerini etkilemez.

### <a name="can-i-perform-geo-restore-with-a-hyperscale-database"></a>Hiper ölçekli bir veritabanı ile coğrafi geri yükleme gerçekleştirebilir miyim

Evet. Coğrafi geri yükleme tamamen desteklenir. Zaman içinde geri yüklemenin aksine, coğrafi geri yükleme veri boyutu işlemi gerektirir. Veri dosyaları paralel olarak kopyalandığından, bu işlemin süresi toplam veritabanı boyutuyerine öncelikle veritabanındaki en büyük dosyanın boyutuna bağlıdır. Veritabanı, kaynak veritabanının bölgesiyle [eşleştirilmiş](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) Azure bölgesinde geri yüklenirse, coğrafi geri yükleme süresi önemli ölçüde kısa olacaktır.

### <a name="can-i-set-up-geo-replication-with-hyperscale-database"></a>Hyperscale veritabanı ile coğrafi çoğaltma ayarlayabilir miyim

Şu anda değil.

### <a name="can-i-take-a-hyperscale-database-backup-and-restore-it-to-my-on-premises-server-or-on-sql-server-in-a-vm"></a>Bir Hyperscale veritabanı yedeklemesi alıp şirket içi sunucuma veya VM'de SQL Server'a geri yükleyebilir miyim?

Hayır. Hyperscale veritabanlarının depolama biçimi SQL Server'ın yayımlanmış tüm sürümünden farklıdır ve yedeklemeleri denetlemez veya bunlara erişemezsiniz. Verilerinizi Bir Hiper Ölçek veritabanından çıkarmak için, Azure Veri Fabrikası, Azure Veri Tuğlaları, SSIS gibi veri hareketi teknolojilerini kullanarak veri ayıklayabilirsiniz.

## <a name="cross-feature-questions"></a>Çapraz Özellik Soruları

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanına geçişten sonra herhangi bir işlevselliği veya yeteneği mi kaybediyorum?

Evet. Azure SQL Veritabanı özelliklerinin bazıları, uzun süreli yedekleme bekletme dahil ancak bunlarla sınırlı olmamak üzere Hyperscale'de desteklenmez. Veritabanlarınızı Hyperscale'e geçirtmenizden sonra bu özellikler çalışmayı durdurur.

### <a name="will-polybase-work-with-hyperscale"></a>Polybase Hyperscale ile çalışacak mı

Hayır. Polybase Azure SQL Veritabanı'nda desteklenmez.

### <a name="does-hyperscale-have-support-for-r-and-python"></a>Hyperscale'in R ve Python desteği var mı?

Şu anda değil.

### <a name="are-compute-nodes-containerized"></a>İşlem düğümleri konteynerleştirilmiş mi

Hayır. Hiper ölçekli işlemler kapsayıcılarda değil, [Servis Kumaş](https://azure.microsoft.com/services/service-fabric/) düğümleri (VM) üzerinde çalıştırın.

## <a name="performance-questions"></a>Performans Soruları

### <a name="how-much-write-throughput-can-i-push-in-a-hyperscale-database"></a>Bir Hyperscale veritabanında ne kadar yazma iş bölümü yapabilirim

İşlem günlüğü işlem kapağı, herhangi bir Hiperölçek işlem boyutu için 100 MB/s olarak ayarlanır. Bu oranı elde etme yeteneği, iş yükü türü, istemci yapılandırması ve bu hızda günlük üretmek için birincil bilgi işlem yinelemesi üzerinde yeterli bilgi işlem kapasitesine sahip olmak üzere ancak bunlarla sınırlı olmamak üzere birden çok etkene bağlıdır.

### <a name="how-many-iops-do-i-get-on-the-largest-compute"></a>En büyük işlemde kaç IOPS alabilirim

IOPS ve IO gecikmesi iş yükü modellerine bağlı olarak değişir. Erişilen veriler bilgi işlem yinelemesi üzerinde önbelleğe alınmışsa, yerel SSD ile benzer IO performansı görürsünüz.

### <a name="does-my-throughput-get-affected-by-backups"></a>Elde iş tadımı yedeklemelerden etkileniyor mu?

Hayır. İşlem depolama katmanından ayrıldı. Bu, yedeklemenin performans etkisini ortadan kaldırır.

### <a name="does-my-throughput-get-affected-as-i-provision-additional-compute-replicas"></a>Ek bilgi işlem yinelemeleri temin ederken iş bilgim etkileniyor mu?

Depolama paylaşılan ve birincil ve ikincil işlem yinelemeleri arasında doğrudan fiziksel çoğaltma olmadığından, birincil yineleme üzerindeki iş ortası yinelemeler ekleyerek doğrudan etkilenmez. Ancak, ikincil yinelemelerde kötü okuma performansını önlemek için ikincil yinelemelere ve sayfa sunucularına günlük uygulamasına izin vermek için birincil de sürekli agresif bir şekilde iş yükünü azaltabiliriz.

### <a name="how-do-i-diagnose-and-troubleshoot-performance-problems-in-a-hyperscale-database"></a>Hiper ölçek veritabanındaki performans sorunlarını nasıl tanılar ve sorun giderme mina'ları

Çoğu performans sorunu için, özellikle depolama performansına dayanmayanlar için, yaygın SQL Server tanılama ve sorun giderme adımları uygulanır. Hiper ölçeklere özgü depolama tanılama ları için [SQL Hiper ölçekli performans sorun giderme tanılama](sql-database-hyperscale-performance-diagnostics.md)larına bakın.

## <a name="scalability-questions"></a>Ölçeklenebilirlik Soruları

### <a name="how-long-would-it-take-to-scale-up-and-down-a-compute-replica"></a>Bir işlem çoğaltmasını küçültmek ne kadar sürer?

Yukarı veya aşağı hesaplama ölçekleme veri boyutu ne olursa olsun 5-10 dakika sürer.

### <a name="is-my-database-offline-while-the-scaling-updown-operation-is-in-progress"></a>Ölçekleme/aşağı işlemi devam ederken veritabanım çevrimdışı mı

Hayır. Yukarı ve aşağı ölçekleme çevrimiçi olacak.

### <a name="should-i-expect-connection-drop-when-the-scaling-operations-are-in-progress"></a>Ölçekleme işlemleri devam ederken bağlantının düşmesini beklemeli miyim?

Ölçekleme işleminin sonunda bir hata olduğunda varolan bağlantıların düşmesine neden olur. İkincil yinelemeler eklemek bağlantı düşmelerine neden olmaz.

### <a name="is-the-scaling-up-and-down-of-compute-replicas-automatic-or-end-user-triggered-operation"></a>Bilgi işlem yinelemelerinin yukarı ve aşağı ölçekleme otomatik veya son kullanıcı tetiklenen işlemi mi

Son kullanıcı. Otomatik değil.  

### <a name="does-the-size-of-my-tempdb-database-also-grow-as-the-compute-is-scaled-up"></a>Veritabanımın `tempdb` boyutu da işlem ölçeklendirildikçe büyür mü?

Evet. İşlem `tempdb` büyüdükçe veritabanı otomatik olarak ölçeklendirilir.  

### <a name="can-i-provision-multiple-primary-compute-replicas-such-as-a-multi-master-system-where-multiple-primary-compute-heads-can-drive-a-higher-level-of-concurrency"></a>Birden çok birincil işlem kafası eşzamanlılık daha yüksek bir düzeyde sürücü bir çok ana sistem gibi birden çok birincil işlem yinelemeler, sağlayabilir miyim

Hayır. Yalnızca birincil işlem yinelemesi okuma/yazma isteklerini kabul eder. İkincil işlem yinelemeleri yalnızca salt okunur isteklerini kabul eder.

## <a name="read-scale-out-questions"></a>Ölçeklendirme Sorularını Oku

### <a name="how-many-secondary-compute-replicas-can-i-provision"></a>Kaç ikincil işlem yinelemesi sağlayabilirim

Varsayılan olarak Hyperscale veritabanları için ikincil bir yineleme oluştururuz. Yineleme sayısını ayarlamak istiyorsanız, [bunu Azure portalı](https://portal.azure.com) veya REST [API'yi](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)kullanarak yapabilirsiniz.

### <a name="how-do-i-connect-to-these-secondary-compute-replicas"></a>Bu ikincil işlem yinelemelerine nasıl bağlanıyorum

Bağlantı dizenizdeki bağımsız değişkeni `ApplicationIntent` 'ye ayarlayarak bu ek salt `ReadOnly`okunur işlem yinelemelerine bağlanabilirsiniz. İşaretlenen `ReadOnly` tüm bağlantılar otomatik olarak salt okunur işlem yinelemelerinden birine yönlendirilir.  

### <a name="how-do-i-validate-if-i-have-successfully-connected-to-secondary-compute-replica-using-ssms-or-other-client-tools"></a>SSMS veya diğer istemci araçlarını kullanarak ikincil işlem yinelemesine başarıyla bağlanıp bağlanmadığımdoğrula nasıl doğrulanırım?

Aşağıdaki T-SQL sorgusunu yürütebilirsiniz: `SELECT DATABASEPROPERTYEX ('<database_name>', 'Updateability')`.
Sonuç, `READ_ONLY` salt okunur ikincil yinelemeye bağlıysanız ve `READ_WRITE` birincil yinelemeye bağlıysanız. Veritabanı bağlamının `master` veritabanına değil, Hyperscale veritabanının adına ayarlanması gerektiğini unutmayın.

### <a name="can-i-create-a-dedicated-endpoint-for-a-read-scale-out-replica"></a>Okuma Ölçeği-out yinelemesi için özel bir bitiş noktası oluşturabilir miyim?

Hayır. Yalnızca Oku Ölçeklendirme yinelemelerine ' ' `ApplicationIntent=ReadOnly`yi belirterek bağlanabilirsiniz.

### <a name="does-the-system-do-intelligent-load-balancing-of-the-read-workload"></a>Sistem okunan iş yükünün akıllı yük dengelemesini yapar mı?

Hayır. Salt okunur amacla yeni bir bağlantı, rasgele bir Okuma Ölçeği-out yinelemesine yönlendirilir.

### <a name="can-i-scale-updown-the-secondary-compute-replicas-independently-of-the-primary-replica"></a>Birincil yinelemeden bağımsız olarak ikincil işlem yinelemelerini ölçeklendirebilir/küçültebilir miyim?

Hayır. İkincil işlem yinelemesi de yüksek kullanılabilirlik başarısız hedefleri olarak kullanılır, bu nedenle başarısız olduktan sonra beklenen performansı sağlamak için birincil ile aynı yapılandırmaya sahip olmaları gerekir.

### <a name="do-i-get-different-tempdb-sizing-for-my-primary-compute-and-my-additional-secondary-compute-replicas"></a>Birincil bilgi `tempdb` işlem ve ek ikincil işlem kopyalarım için farklı boyutlandırma alabilir miyim?

Hayır. Veritabanınız, `tempdb` bilgi işlem boyutu sağlama temel alınarak yapılandırılır, ikincil işlem yinelemeleriniz birincil işlemle aynı boyuttadır.

### <a name="can-i-add-indexes-and-views-on-my-secondary-compute-replicas"></a>İkincil işlem yinelemelerime dizin ler ve görünümler ekleyebilir miyim?

Hayır. Hiper ölçekli veritabanları depolamayı paylaştı, yani tüm işlem yinelemeleri aynı tabloları, dizinleri ve görünümleri görür. İkincil okumalar için optimize edilmiş ek dizinler istiyorsanız, bunları birincil ekine eklemeniz gerekir.

### <a name="how-much-delay-is-there-going-to-be-between-the-primary-and-secondary-compute-replicas"></a>Birincil ve ikincil işlem yinelemeleri arasında ne kadar gecikme olacak?

Bir işlemin birincil işlemde işlendiği andan ikincil bir işlemde görünür olduğu anda geçerli günlük oluşturma hızına bağlıdır. Tipik veri gecikmesi düşük milisaniye cinsindendir.

## <a name="next-steps"></a>Sonraki Adımlar

Hyperscale hizmet katmanı hakkında daha fazla bilgi için [Hyperscale hizmet katmanına](sql-database-service-tier-hyperscale.md)bakın.
