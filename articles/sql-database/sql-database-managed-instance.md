---
title: SQL yönetilen örnek genel bakış
description: Bu makalede, Azure SQL Veritabanı yönetilen örnek açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab, vanto
ms.date: 01/21/2020
ms.openlocfilehash: b9fdd1b25e53e1cdc8aa76564304a61adaa8d804
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268788"
---
# <a name="what-is-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı yönetilen örneği nedir?

Yönetilen örnek, en son SQL Server şirket içi (Enterprise Edition) Veritabanı Motoru ile %100'e yakın uyumluluk sağlayan ve ortak güvenlik endişelerini gideren yerel bir [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md) uygulaması ve şirket içi SQL Server müşterileri için uygun bir [iş modeli](https://azure.microsoft.com/pricing/details/sql-database/) sağlayan Azure SQL Veritabanı'nın yeni bir dağıtım seçeneğidir. Yönetilen örnek dağıtım modeli, varolan SQL Server müşterilerinin en az uygulama ve veritabanı değişiklikleriyle şirket içi uygulamalarını kaldırmasına ve buluta kaydırmasına olanak tanır. Aynı zamanda, yönetilen örnek dağıtım seçeneği, yönetim ek yükü ve TCO'yu önemli ölçüde azaltan tüm PaaS özelliklerini (otomatik düzeltme ve sürüm [güncelleştirmeleri, otomatik yedeklemeler,](sql-database-automated-backups.md) [yüksek kullanılabilirlik)](sql-database-high-availability.md) korur.

> [!IMPORTANT]
> Yönetilen örnek dağıtım seçeneğinin şu anda kullanılabildiği bölgelerin listesi için [desteklenen bölgelere](sql-database-managed-instance-resource-limits.md#supported-regions)bakın.

Aşağıdaki diyagram, yönetilen örneklerin temel özelliklerini özetler:

![temel özellikler](./media/sql-database-managed-instance/key-features.png)

Yönetilen örnek dağıtım modeli, şirket içi veya IaaS, kendi kendine inşa edilmiş veya ISV tarafından sağlanan ortamdan mümkün olduğunca düşük geçiş çabasıyla tam olarak yönetilen PaaS bulut ortamına çok sayıda uygulama geçirmek isteyen müşteriler için tasarlanmıştır. Azure'da tam otomatik [leştirilmiş Veri Geçiş Hizmeti 'ni (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) kullanan müşteriler, şirket içi SQL Server'larını şirket içi SQL Server ile uyumluluk ve müşteri örneklerinin yerel VNet desteğiyle tam yalıtım ı sunan yönetilen bir örneğe kaldırabilir ve kaydırabilir.  Yazılım Güvencesi ile, [SQL Server için Azure Karma Avantajı'nı](https://azure.microsoft.com/pricing/hybrid-benefit/)kullanarak yönetilen bir örnekte mevcut lisanslarınızı indirimli fiyatlarla değiştirebilirsiniz.  Yönetilen örnek, yüksek güvenlik ve zengin bir programlanabilirlik yüzeyi gerektiren SQL Server örnekleri için buluttaki en iyi geçiş hedefidir.

Yönetilen örnek dağıtım seçeneği, aşamalı bir sürüm planı aracılığıyla en son şirket içi SQL Server sürümüyle %100'e yakın yüzey alanı uyumluluğu sağlar.

Azure SQL Veritabanı dağıtım seçenekleri arasında karar vermek için: tek veritabanı, havuzlu veritabanı ve yönetilen örnek ve sanal makinede barındırılan SQL [Server, Azure'da SQL Server'ın doğru sürümünü nasıl seçeceğinizi](sql-database-paas-vs-sql-server-iaas.md)görün.

## <a name="key-features-and-capabilities"></a>Temel özellikler ve özellikler

Yönetilen örnek, hem Azure SQL Veritabanı'nda hem de SQL Server Veritabanı Altyapısında kullanılabilen en iyi özellikleri birleştirir.

> [!IMPORTANT]
> Yönetilen bir örnek, çevrimiçi işlemler, otomatik plan düzeltmeleri ve diğer kurumsal performans geliştirmeleri de dahil olmak üzere SQL Server'ın en son sürümünün tüm özellikleriyle çalışır. Kullanılabilir özelliklerin karşılaştırılması [Özellik karşılaştırmasında açıklanmıştır: Azure SQL Veritabanı ile SQL Server.](sql-database-features.md)

| **PaaS avantajları** | **İş sürekliliği** |
| --- | --- |
|Donanım satın alma ve yönetimi yok <br>Altta yatan altyapıyı yönetmek için yönetim yükü yok <br>Hızlı sağlama ve hizmet ölçekleme <br>Otomatik düzeltme ve sürüm yükseltme <br>Diğer PaaS veri hizmetleriyle entegrasyon |99.99% çalışma süresi SLA  <br>Yüksek [kullanılabilirlik](sql-database-high-availability.md) içinde yerleşik <br>[Otomatik yedeklemelerle](sql-database-automated-backups.md) korunan veriler <br>Müşteri yapılandırılabilir yedekleme bekletme süresi <br>Kullanıcı tarafından başlatılan [yedeklemeler](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql?view=azuresqldb-mi-current) <br>[Zaman veritabanı geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore) özelliği noktası |
|**Güvenlik ve uyumluluk** | **Yönetim**|
|Yalıtılmış ortam ([VNet entegrasyonu,](sql-database-managed-instance-connectivity-architecture.md)tek kiracı hizmeti, özel işlem ve depolama) <br>[Saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)<br>[Azure AD kimlik doğrulaması](sql-database-aad-authentication.md), tek oturum açma desteği <br> <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">Azure AD sunucu ilkeleri (oturum açmalar)</a>  <br>Azure SQL veritabanıyla aynı uyumluluk standartlarına uyar <br>[SQL denetimi](sql-database-managed-instance-auditing.md) <br>[Gelişmiş Tehdit Koruması](sql-database-managed-instance-threat-detection.md) |Hizmet sağlama ve ölçekleme otomatikleştirmek için Azure Kaynak Yöneticisi API <br>Manuel hizmet sağlama ve ölçekleme için Azure portalı işlevselliği <br>Veri Geçiş Hizmeti

> [!IMPORTANT]
> Azure SQL Veritabanı (tüm dağıtım seçenekleri), bir dizi uyumluluk standardına göre onaylanmıştır. Daha fazla bilgi için, SQL Veritabanı uyumluluk sertifikalarının en güncel listesini bulabileceğiniz [Microsoft Azure Güven Merkezi'ne](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) bakın.

Yönetilen örneklerin temel özellikleri aşağıdaki tabloda gösterilmiştir:

|Özellik | Açıklama|
|---|---|
| SQL Server sürümü / build | SQL Server Database Engine (en son kararlı) |
| Yönetilen otomatik yedeklemeler | Evet |
| Yerleşik örnek ve veritabanı izleme ve ölçümleri | Evet |
| Otomatik yazılım düzeltme | Evet |
| En son Veritabanı Altyapısı özellikleri | Evet |
| Veritabanı başına veri dosyası (ROWS) sayısı | Birden çok |
| Veritabanı başına günlük dosyası sayısı (LOG) | 1 |
| VNet - Azure Kaynak Yöneticisi dağıtımı | Evet |
| VNet - Klasik dağıtım modeli | Hayır |
| Portal desteği | Evet|
| Dahili Entegrasyon Hizmeti (SSIS) | Hayır - SSIS, [Azure Veri Fabrikası PaaS'ın](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) bir parçasıdır |
| Dahili Analiz Hizmeti (SSAS) | Hayır - SSAS ayrı [PaaS](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) olduğunu |
| Dahili Raporlama Hizmeti (SSRS) | Hayır - Power BI veya SSRS IaaS kullanın |
|||

## <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

Yönetilen örnekler için [vCore tabanlı satın alma modeli,](sql-database-service-tiers-vcore.md) size esneklik, denetim, şeffaflık ve şirket içi iş yükü gereksinimlerini buluta çevirmenin basit bir yolunu sunar. Bu model, iş yükü gereksinimlerinize bağlı olarak işlem, bellek ve depolama yı değiştirmenize olanak tanır. vCore modeli, SQL Server için Azure Karma [Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)ile yüzde 55'e kadar tasarruf için de uygundur.

vCore modelinde, nesillerden nesile donanım seçebilirsiniz.

- **Gen4** Mantıksal CPU'lar Intel E5-2673 v3 (Haswell) 2.4-GHz işlemciler, ekli SSD, fiziksel çekirdekler, çekirdek başına 7 GB RAM ve 8 ve 24 vCores arasında hesaplama boyutları dayanmaktadır.
- **Gen5** Mantıksal CPU'lar Intel E5-2673 v4 (Broadwell) 2.3-GHz ve Intel SP-8160 (Skylake) işlemciler, hızlı NVMe SSD, hiper dişli mantıksal çekirdek ve 4 ve 80 çekirdek arasındaki işlem boyutları dayanmaktadır.

[Yönetilen örnek kaynak sınırlarında](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics)donanım nesilleri arasındaki fark hakkında daha fazla bilgi edinin.

## <a name="managed-instance-service-tiers"></a>Yönetilen örnek hizmet katmanları

Yönetilen örnek iki hizmet katmanında kullanılabilir:

- **Genel amaç**: Tipik performans ve IO gecikme şartlarına sahip uygulamalar için tasarlanmıştır.
- **İş açısından kritik**: Düşük IO gecikme süresi gereksinimlerine ve temel bakım işlemlerinin iş yükü üzerindeki minimum etkisine sahip uygulamalar için tasarlanmıştır.

Her iki hizmet katmanı da %99,99 kullanılabilirliği garanti eder ve depolama boyutunu ve işlem kapasitesini bağımsız olarak seçmenize olanak tanır. Azure SQL Veritabanı'nın yüksek kullanılabilirlik mimarisi hakkında daha fazla bilgi için yüksek [kullanılabilirlik ve Azure SQL Veritabanı'na](sql-database-high-availability.md)bakın.

### <a name="general-purpose-service-tier"></a>Genel amaçlı hizmet katmanı

Aşağıdaki liste, Genel Amaçlı hizmet katmanının temel özelliğini açıklar:

- Tipik performans gereksinimlerine sahip iş uygulamalarının çoğu için tasarım
- Yüksek performanslı Azure Blob depolama (8 TB)
- Güvenilir Azure Blob depolama ve [Azure Hizmet Kumaşı'na](../service-fabric/service-fabric-overview.md) dayalı yerleşik yüksek [kullanılabilirlik](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability)

Daha fazla bilgi için, [genel amaçlı katmanve](https://medium.com/azure-sqldb-managed-instance/file-layout-in-general-purpose-azure-sql-managed-instance-cf21fff9c76c) [depolama performansı en iyi uygulamaları ve yönetilen örnekleri (genel amaçlı) için hususlar](https://blogs.msdn.microsoft.com/sqlcat/2018/07/20/storage-performance-best-practices-and-considerations-for-azure-sql-db-managed-instance-general-purpose/)depolama katmanı bakın.

[Yönetilen örnek kaynak sınırlarındaki](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)hizmet katmanları arasındaki fark hakkında daha fazla bilgi edinin.

### <a name="business-critical-service-tier"></a>İş Kritik hizmet katmanı

Business Critical hizmet katmanı, yüksek IO gereksinimleri olan uygulamalar için oluşturulmuştür. Birkaç yalıtılmış çoğaltma kullanarak hatalara karşı en yüksek esneklik sağlar.

Aşağıdaki liste, İş Açısından Kritik hizmet katmanının temel özelliklerini özetleyilmiştir:

- En yüksek performans ve HA gereksinimlerine sahip iş uygulamaları için tasarlanmıştır
- Süper hızlı yerel SSD depolama (Gen4'te 1 TB'a kadar ve Gen5'te 4 TB'a kadar)
- [Her Zaman Kullanılabilirlik Gruplarına](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) ve [Azure Hizmet Kumaşına](../service-fabric/service-fabric-overview.md)dayalı yerleşik [yüksek kullanılabilirlik.](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability)
- Raporlama ve diğer salt okunur iş yükleri için kullanılabilecek yerleşik ek [salt okunur veritabanı yinelemesi](sql-database-read-scale-out.md)
- Yüksek performans gereksinimleri ile iş yükü için kullanılabilen [Bellek Içi OLTP](sql-database-in-memory.md)  

[Yönetilen örnek kaynak sınırlarındaki](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)hizmet katmanları arasındaki fark hakkında daha fazla bilgi edinin.


## <a name="managed-instance-management-operations"></a>Yönetilen örnek yönetim işlemleri

Azure SQL Veritabanı yeni yönetilen örnekleri otomatik olarak dağıtmak, örnek özelliklerini güncelleştirmek ve artık gerekmediğinde örnekleri silmek için kullanabileceğiniz yönetim işlemleri sağlar. Bu bölümde yönetim işlemleri ve bunların tipik süreleri hakkında bilgi verilmektedir.

Azure [Sanal Ağlar (VNets) içindeki dağıtımları](../virtual-network/virtual-network-for-azure-services.md#deploy-azure-services-into-virtual-networks) desteklemek ve müşteriler için yalıtım ve güvenlik sağlamak için yönetilen örnek, müşterinin sanal ağ alt ağında dağıtılan özel bir yalıtılmış sanal makine kümesini temsil eden [sanal kümelere](sql-database-managed-instance-connectivity-architecture.md#high-level-connectivity-architecture)dayanır. Esasen, boş bir alt ağda yönetilen her örnek dağıtımı yeni bir sanal küme oluşturmayla sonuçlanır.

Dağıtılan yönetilen örneklerdeki sonraki işlemlerin altta yatan sanal küme üzerinde de etkileri olabilir. Bu, yeni dağıtımlar veya varolan yönetilen örneklerde güncelleştirmeler planlarken dikkate alınması gereken bir ek yükü yle birlikte ek sanal makineler intikal ettiğinden, yönetim işlemlerinin süresini etkiler.

Tüm yönetim işlemleri aşağıdaki gibi kategorilere ayrılabilir:

- Örnek dağıtım (yeni örnek oluşturma). 
- Örnek güncelleştirme (vCores veya ayrılmış depolama gibi örnek özelliklerini değiştirme.
- Örnek silme.

Genellikle, sanal kümeler üzerindeki işlemler en uzun sürer. Sanal kümeler üzerindeki işlemlerin süresi değişir – aşağıdaki değerler, mevcut hizmet telemetrisi verilerine göre genellikle bekleyebilirsiniz:

- Sanal küme oluşturma. Bu, örnek yönetim işlemlerinde eşzamanlı bir adımdır. **Operasyonların %90'ı 4 saatte tamamlar.**
- Sanal küme yeniden boyutlandırma (genişletme veya küçültme). Genişletme eşzamanlı bir adımdır, ancak küçülme eşzamanlı olarak gerçekleştirilir (örnek yönetim işlemlerinin süresi üzerinde etkisi olmadan). **Küme genişletmelerinin %90'ı 2,5 saatten kısa sürede tamamlar.**
- Sanal küme silme. Silme işlemi eşzamanlı bir adımdır, ancak boş bir sanal kümede [el ile başlatılabilir](sql-database-managed-instance-delete-virtual-cluster.md) ve bu durumda eşzamanlı olarak yürütülür. **Sanal küme silmelerin %90'ı 1,5 saatte tamamlar.**

Ayrıca, örneklerin yönetimi, barındırılan veritabanlarındaki işlemlerden birini de içerebilir ve bu da daha uzun sürelerle sonuçlanır:

- Azure Depolama'dan veritabanı dosyaları ekleme. Bu, genel amaçlı hizmet katmanında yukarı veya aşağı ölçekleme hesaplama (vCore) veya depolama alanı gibi eşzamanlı bir adımdır. **Bu operasyonların %90'ı 5 dakikada tamamlar.**
- Her zaman kullanılabilirlik grup tohumlama üzerinde. Bu, işlem (vCore) veya İş Açısından Kritik hizmet katmanında depolama ölçeklendirmesi gibi ve hizmet katmanını Genel Amaç'tan İş Açısından Kritik 'e (veya tam tersi) değiştirme gibi eşzamanlı bir adımdır. Bu işlemin süresi, toplam veritabanı boyutunun yanı sıra geçerli veritabanı etkinliği (etkin hareket sayısı) ile orantılıdır. Bir örneği güncellerken veritabanı etkinliği toplam süreye önemli fark lar getirebilir. **Bu işlemlerin %90'ı 220 GB / saat veya daha yüksek hızda yürütülür.**

Aşağıdaki tabloişlemleri ve tipik genel süreleri özetler:

|Kategori  |İşlem  |Uzun soluklu segment  |Tahmini süre  |
|---------|---------|---------|---------|
|**Dağıtım** |Boş bir alt ağda ilk örnek|Sanal küme oluşturma|Operasyonların %90'ı 4 saatte bitiyor|
|Dağıtım |Boş olmayan bir alt ağdaki başka bir donanım oluşturma nın ilk örneği (örneğin, Gen 4 örnekleriolan bir alt ağdaki ilk Gen 5 örneği)|Sanal küme oluşturma*|Operasyonların %90'ı 4 saatte bitiyor|
|Dağıtım |Boş veya boş olmayan bir alt ağda 4 vCore'un ilk örneği oluşturulması|Sanal küme oluşturma**|Operasyonların %90'ı 4 saatte bitiyor|
|Dağıtım |Boş olmayan alt ağ içinde sonraki örnek oluşturma (2., 3. vb. örnek)|Sanal küme yeniden boyutlandırma|Operasyonların %90'ı 2,5 saatte bitiyor|
|**Güncelleştirme** |Örnek özellik değişikliği (yönetici parolası, AAD girişi, Azure Karma Avantaj bayrağı)|Yok|1 dakikaya kadar|
|Güncelleştirme |Örnek depolama ölçekleme yukarı/aşağı (Genel Amaçlı hizmet katmanı)|Veritabanı dosyalarını ekleme|Operasyonların %90'ı 5 dakikada bitiyor|
|Güncelleştirme |Örnek depolama ölçeklendirme yukarı/aşağı (İş Kritik hizmet katmanı)|- Sanal küme yeniden boyutlandırma<br>- Her zaman kullanılabilirlik grup tohumlama|İşlemlerin %90'ı 2,5 saat içinde sona erdirin + tüm veritabanlarını tohumlamak için zaman (220 GB / saat)|
|Güncelleştirme |Örnek hesaplama (vCores) yukarı ve aşağı ölçekleme (Genel Amaç)|- Sanal küme yeniden boyutlandırma<br>- Veritabanı dosyalarını ekleme|Operasyonların %90'ı 2,5 saatte bitiyor|
|Güncelleştirme |Örnek hesaplama (vCores) yukarı ve aşağı ölçekleme (İş Kritik)|- Sanal küme yeniden boyutlandırma<br>- Her zaman kullanılabilirlik grup tohumlama|İşlemlerin %90'ı 2,5 saat içinde sona erdirin + tüm veritabanlarını tohumlamak için zaman (220 GB / saat)|
|Güncelleştirme |Örnek ölçek 4 vCores (Genel Amaç)|- Sanal küme yeniden boyutlandırma (ilk kez yapılırsa, sanal küme oluşturma**)<br>- Veritabanı dosyalarını ekleme|Operasyonların %90'ı 4 saat 5 dk**|
|Güncelleştirme |Örnek ölçek 4 vCores (İş Kritik) aşağı|- Sanal küme yeniden boyutlandırma (ilk kez yapılırsa, sanal küme oluşturma**)<br>- Her zaman kullanılabilirlik grup tohumlama|İşlemlerin %90'ı 4 saat içinde sona erdirin + tüm veritabanlarını tohumlamak için zaman (220 GB / saat)|
|Güncelleştirme |Örnek hizmet katmanı değişikliği (İş Açısından Genel Amaç ve tam tersi)|- Sanal küme yeniden boyutlandırma<br>- Her zaman kullanılabilirlik grup tohumlama|İşlemlerin %90'ı 2,5 saat içinde sona erdirin + tüm veritabanlarını tohumlamak için zaman (220 GB / saat)|
|**Silme**|Örnek silme|Tüm veritabanları için günlük kuyruk yedekleme|%90 operasyonlar 1 dakikaya kadar biter.<br>Not: alt ağdaki son örnek silinirse, bu işlem 12 saat sonra sanal küme silme zamanlayacaktır***|
|Silme|Sanal küme silme (kullanıcı tarafından başlatılan işlem olarak)|Sanal küme silme|Operasyonların %90'ı 1,5 saate kadar tamamlar|

\*Sanal küme donanım üretimine göre oluşturulur.

\*\*4 vCores dağıtım seçeneği Haziran 2019'da piyasaya sürüldü ve yeni bir sanal küme sürümü gerektirir. Hedef alt nette 12 Haziran'dan önce oluşturulmuş örnekleri varsa, 4 vCore örneğini barındırmak için otomatik olarak yeni bir sanal küme dağıtılır.

\*\*\*12 saat geçerli yapılandırma dır, ancak bu gelecekte değişebilir, bu nedenle ona sıkı bir bağımlılık almayın. Sanal kümeyi daha önce silmeniz gerekiyorsa (örneğin alt ağı serbest bırakmak için), [bkz.](sql-database-managed-instance-delete-virtual-cluster.md)

### <a name="instance-availability-during-management"></a>Yönetim sırasında örnek kullanılabilirliği

Yönetilen örnekler, dağıtım ve silme işlemleri sırasında istemci uygulamaları için kullanılamaz.

Yönetilen örnekler güncelleştirme işlemleri sırasında kullanılabilir, ancak genellikle 10 saniyeye kadar süren güncelleştirmelerin sonunda gerçekleşen başarısızlık nedeniyle kısa bir kapalı kalma süresi vardır. Bunun istisnası, Genel Amaçlı hizmet katmanında ayrılmış depolama alanının güncelleştirildiği ve bunun başarısız olması veya örnek kullanılabilirliğini etkilememesidir.

> [!IMPORTANT]
> Bir arıza [süresi, uzun kurtarma süresi](sql-database-accelerated-database-recovery.md#the-current-database-recovery-process)nedeniyle veritabanlarında meydana gelen uzun süreli işlemler durumunda önemli ölçüde değişebilir. Bu nedenle, Azure SQL Veritabanı yönetilen örneğinin bilgi işlem veya depolamasını ölçeklendirmek veya uzun süren işlemlerle (veri alma, veri işleme işleri, dizin yeniden oluşturma vb.) hizmet katmanını aynı anda değiştirmek önerilmez. İşlem sonunda gerçekleştirilecek veritabanı başarısızlığı, devam eden hareketleri iptal eder ve uzun kurtarma süresine neden olur.

> [!TIP]
> Genel Amaçlı hizmet katmanında ayrılmış depolama alanının güncelleştirimi, başarısızlık veya örnek kullanılabilirliğini etkilemez.

[Hızlandırılmış veritabanı kurtarma](sql-database-accelerated-database-recovery.md) şu anda Azure SQL Veritabanı yönetilen örnekleri için kullanılamaz. Etkinleştirildiğinde, bu özellik, uzun süren işlemlerde bile zaman içinde arıza değişkenliğini önemli ölçüde azaltır.

### <a name="canceling-management-operations"></a>Yönetim işlemlerini iptal etme

Aşağıdaki tablo, belirli yönetim işlemlerini ve tipik genel süreleri iptal etme yeteneğini özetlemektedir:

Kategori  |İşlem  |Cancelable  |Tahmini iptal süresi  |
|---------|---------|---------|---------|
|Dağıtım |Örnek oluşturma |Hayır |  |
|Güncelleştirme |Örnek depolama ölçekleme yukarı/aşağı (Genel Amaçlı) |Hayır |  |
|Güncelleştirme |Örnek depolama ölçeklendirme yukarı/aşağı (İş Kritik) |Evet |Operasyonların %90'ı 5 dakikada bitiyor |
|Güncelleştirme |Örnek hesaplama (vCores) yukarı ve aşağı ölçekleme (Genel Amaç) |Evet |Operasyonların %90'ı 5 dakikada bitiyor |
|Güncelleştirme |Örnek hesaplama (vCores) yukarı ve aşağı ölçekleme (İş Kritik) |Evet |Operasyonların %90'ı 5 dakikada bitiyor |
|Güncelleştirme |Örnek hizmet katmanı değişikliği (İş Açısından Genel Amaç ve tam tersi) |Evet |Operasyonların %90'ı 5 dakikada bitiyor |
|Sil |Örnek silme |Hayır |  |
|Sil |Sanal küme silme (kullanıcı tarafından başlatılan işlem olarak) |Hayır |  |

Yönetim işlemini iptal etmek için genel bakış bıçağına gidin ve devam eden işlemin bildirim kutusuna tıklayın. Sağ taraftan, devam eden işlemi olan bir ekran görüntülenir ve işlemi iptal etmek için düğme görüntülenir. İlk tıklattıktan sonra, yeniden tıklatmanız ve işlemi iptal etmek istediğinizi onaylamanız istenir.

[![](./media/sql-database-managed-instance/canceling-operation.png)](./media/sql-database-managed-instance/canceling-operation.png#lightbox)

İptal isteği gönderildikten ve işlendikten sonra, iptal gönderimi başarılı olup olmadığını bildiren bildirim alırsınız. 

İptal işlemi nin iptal edilmesi durumunda, yönetim işlemi birkaç dakika içinde iptal edilir ve bu da bir başarısızlıkla sonuçlanır.

![işlem sonucunu iptal etme](./media/sql-database-managed-instance/canceling-operation-result.png)

İptal isteği başarısız olursa veya iptal düğmesi etkin değilse, bu yönetim işleminin iptal edilemeyeceğini ve birkaç dakika içinde biteceği anlamına gelir. Yönetim işlemi tamamlanana kadar yürütmeye devam edecektir.

> [!IMPORTANT]
> İptal işlemi şu anda yalnızca Portal'da desteklenir.

## <a name="advanced-security-and-compliance"></a>Gelişmiş koruma ve uyumluluk

Yönetilen örnek dağıtım seçeneği, Azure bulutu ve SQL Server Veritabanı Altyapısı tarafından sağlanan gelişmiş güvenlik özelliklerini birleştirir.

### <a name="managed-instance-security-isolation"></a>Yönetilen örnek güvenlik yalıtımı

Yönetilen bir örnek, Azure bulutundaki diğer kiracılardan ek güvenlik yalıtımı sağlar. Güvenlik yalıtımı şunları içerir:

- Azure Express Route veya VPN Ağ Geçidi'ni kullanarak [yerel sanal ağ uygulaması](sql-database-managed-instance-connectivity-architecture.md) ve şirket ortamınıza bağlantı.
- Varsayılan dağıtımda, SQL uç noktası yalnızca özel bir IP adresi aracılığıyla açıklanır ve özel Azure veya karma ağlardan güvenli bağlantı sağlar.
- Özel temel altyapı (işlem, depolama) ile tek kiracı.

Aşağıdaki diyagram, uygulamalarınız için çeşitli bağlantı seçeneklerini özetleyişverilmiştir:

![yüksek kullanılabilirlik](./media/sql-database-managed-instance/application-deployment-topologies.png)  

Alt net düzeyinde VNet tümleştirmesi ve ağ ilkesi zorlama hakkında daha fazla bilgi edinmek [için yönetilen örnekler için VNet mimarisine](sql-database-managed-instance-connectivity-architecture.md) bakın ve [uygulamanızı yönetilen bir örne bağlayın.](sql-database-managed-instance-connect-app.md)

> [!IMPORTANT]
> Güvenlik gereksinimleriniz tarafından izin verilen her yerde, size ek avantajlar getireceğinden, aynı alt ağa birden çok yönetilen örnek yerleştirin. Uzun sağlama süresi, bir alt ağda ilk yönetilen örneği dağıtma maliyetiyle ilişkili olduğundan, aynı alt ağdaki örnekleri harmanlama önemli ölçüde ağ altyapısı bakımını basitleştirir ve örnek sağlama süresini azaltır.

### <a name="azure-sql-database-security-features"></a>Azure SQL Veritabanı Güvenlik Özellikleri

Azure SQL Veritabanı, verilerinizi korumak için kullanılabilecek bir dizi gelişmiş güvenlik özelliği sağlar.

- [Yönetilen örnek denetimi](sql-database-managed-instance-auditing.md) veritabanı olaylarını izler ve bunları Azure depolama hesabınıza yerleştirilen bir denetim günlüğü dosyasına yazar. Denetim, mevzuata uygunluğun korunmasına, veritabanı etkinliğini anlamanıza ve iş kaygılarına veya şüpheli güvenlik ihlallerini gösterebilecek tutarsızlıklar ve anormallikler hakkında bilgi edinmeye yardımcı olabilir.
- Hareket halindeki veri şifreleme - yönetilen bir örnek, Aktarım Katmanı Güvenliği'ni kullanarak hareket halindeki veriler için şifreleme sağlayarak verilerinizi güvence altına alar. Aktarım katmanı güvenliğine ek olarak, yönetilen örnek dağıtım seçeneği, [her zaman şifrelenmiş](/sql/relational-databases/security/encryption/always-encrypted-database-engine)olan uçuş, istirahat ve sorgu işleme sırasında hassas verilerin korunmasını sağlar. Always Encrypted, kritik öneme sahip verilerin çalınması gibi güvenlik ihlallerine karşı sektörde benzersiz koruma sağlayan bir özelliktir. Örneğin, Her Zaman Şifrelenmiş ile, kredi kartı numaraları her zaman veritabanında şifrelenir, hatta sorgu işleme sırasında bile, yetkili personel veya bu verileri işlemek için gereken uygulamalar tarafından kullanım noktasında şifre çözme sağlar.
- [Gelişmiş Tehdit Koruması,](sql-database-managed-instance-threat-detection.md) hizmette yerleşik olarak veritabanlarına erişmek veya veritabanlarından yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılayan ek bir güvenlik zekası katmanı sağlayarak [denetimi](sql-database-managed-instance-auditing.md) tamamlar. Şüpheli etkinlikler, olası güvenlik açıkları ve SQL enjeksiyon saldırılarının yanı sıra anormal veritabanı erişim desenleri hakkında uyarılırsınız. Gelişmiş Tehdit Koruması uyarıları Azure [Güvenlik Merkezi'nden](https://azure.microsoft.com/services/security-center/) görüntülenebilir ve şüpheli etkinliklerin ayrıntılarını sağlayabilir ve tehdidin nasıl araştırılabildiğini ve azaltılabildiğini nasıl değerlendireceğiniz konusunda eylem önerebilir.  
- [Dinamik veri maskeleme,](/sql/relational-databases/security/dynamic-data-masking) ayrıcalıklı olmayan kullanıcılara maskeleyerek hassas veri maruziyetini sınırlar. Dinamik veri maskeleme, hassas verilerin ne kadarının uygulama katmanı üzerinde en az etkiyle ortaya çıkaracağını belirlemenize olanak sağlayarak hassas verilere yetkisiz erişimi önlemeye yardımcı olur. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir.
- [Satır düzeyinde güvenlik,](/sql/relational-databases/security/row-level-security) bir sorguyu yürüten kullanıcının özelliklerine (grup üyeliği veya yürütme bağlamı gibi) dayalı olarak veritabanı tablosundaki satırlara erişimi denetlemenize olanak tanır. Satır düzeyi güvenlik (RLS), uygulamanızın güvenlik tasarımını ve kodlama aşamasını kolaylaştırır. RLS, veri satırı erişiminde kısıtlama uygulamanızı sağlar. Örneğin, çalışanların yalnızca departmanlarıyla ilgili veri satırlarına erişebilmelerini sağlamak veya bir veri erişimini yalnızca ilgili verilerle sınırlamak.
- [Saydam veri şifreleme (TDE),](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) yönetilen örnek veri dosyalarını şifreler, veri leri istirahatte şifreleme olarak bilinir. TDE gerçek zamanlı G/Ç şifrelemeve veri ve günlük dosyalarışifresini gerçekleştirir. Şifreleme, kurtarma sırasında kullanılabilirlik için veritabanı önyükleme kaydında depolanan bir veritabanı şifreleme anahtarı (DEK) kullanır. Tüm veritabanlarınızı yönetilen bir durumda saydam veri şifrelemeile koruyabilirsiniz. TDE, SQL Server'ın depolama ortamının çalınmasına karşı korumak için birçok uyumluluk standardı tarafından gerekli olan kanıtlanmış şifreleme teknolojisidir.

Şifrelenmiş bir veritabanının yönetilen bir örne geçişi, Azure Veritabanı Geçiş Hizmeti (DMS) veya yerel geri yükleme yoluyla desteklenir. Yerel geri yükleme yi kullanarak şifreli bir veritabanını taşımayı planlıyorsanız, varolan TDE sertifikasının SANAL Makine'deki SQL Server'dan veya SQL Server'dan yönetilen bir örne geçişi gerekli bir adımdır. Geçiş seçenekleri hakkında daha fazla bilgi için, [yönetilen örneğe SQL Server örnek geçişi ne](sql-database-managed-instance-migrate.md)bakın.

## <a name="azure-active-directory-integration"></a>Azure Etkin Dizin Tümleştirmesi

Yönetilen örnek dağıtım seçeneği, Azure Active Directory (AAD) ile tümleşik geleneksel SQL server Database engine logins ve girişleri destekler. Azure AD sunucu ilkeleri (oturum açmalar)**(genel önizleme),** şirket içi ortamınızda kullandığınız şirket içi veritabanı girişlerinin Azure bulut sürümüdür. Azure AD sunucu ilkeleri (oturum açmalar), Azure Active Directory kiracınızdaki kullanıcıları ve grupları, aynı yönetilen veritabanı sorguları da dahil olmak üzere, örnek düzeyinde herhangi bir işlemi gerçekleştirebilen gerçek örnek kapsamlı ilkeler olarak belirtmenize olanak tanır Örnek.

**External Provider'dan**Azure AD sunucu ilkeleri (oturum açmalar) oluşturmak için yeni bir sözdizimi tanıtıldı. Sözdizimi hakkında daha fazla bilgi için <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN'e</a>bakın ve [yönetilen örnek makaleniz için Azure Etkin Dizin yöneticisi hükmünü gözden geçirin.](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance)

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory tümleştirmesi ve çok faktörlü kimlik doğrulaması

Yönetilen örnek dağıtım seçeneği, [Azure Active Directory tümleştirmesi](sql-database-aad-authentication.md)ile veritabanı kullanıcısının ve diğer Microsoft hizmetlerinin kimliklerini merkezi olarak yönetmenize olanak tanır. Bu özellik, izin yönetimini kolaylaştırırken güvenliği artırır. Azure Active Directory, veri ve uygulama güvenliğini artırmak için [çok faktörlü kimlik doğrulamasını](sql-database-ssms-mfa-authentication-configure.md) (MFA) ve çoklu oturum açma işlemini destekler.

### <a name="authentication"></a>Kimlik doğrulaması

Yönetilen örnek kimlik doğrulaması, kullanıcıların veritabanına bağlanırken kimliklerini nasıl kanıtladığını ifade eder. SQL Veritabanı iki kimlik doğrulaması türünü destekler:  

- **SQL Kimlik Doğrulama**:

  Bu kimlik doğrulama yöntemi bir kullanıcı adı ve parola kullanır.
- **Azure Etkin Dizin Kimlik Doğrulaması**:

  Bu kimlik doğrulama yöntemi, Azure Active Directory tarafından yönetilen kimlikleri kullanır ve yönetilen ve tümleşik etki alanları için desteklenir. [Mümkün olduğunda](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode)Etkin Dizin kimlik doğrulaması (tümleşik güvenlik) kullanın.

### <a name="authorization"></a>Yetkilendirme

Yetkilendirme, bir kullanıcının Azure SQL Veritabanı içinde neler yapabileceğini ifade eder ve kullanıcı hesabınızın veritabanı rol üyelikleri ve nesne düzeyindeizinler tarafından denetlenir. Yönetilen bir örnek, SQL Server 2017 ile aynı yetkilendirme özelliklerine sahiptir.

## <a name="database-migration"></a>Veritabanı geçişi

Yönetilen örnek dağıtım seçeneği, şirket içi veya IaaS veritabanı uygulamalarından toplu veritabanı geçişi olan kullanıcı senaryolarını hedefler. Yönetilen örnek birkaç veritabanı geçiş seçeneklerini destekler:

### <a name="back-up-and-restore"></a>Yedekleme ve geri yükleme  

Geçiş yaklaşımı, SQL yedeklemelerinden Azure Blob depolamasına yararlanır. Azure depolama blob'unda depolanan yedeklemeler, [T-SQL RESTORE komutu](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql?view=azuresqldb-mi-current)kullanılarak yönetilen bir örneğe doğrudan geri yüklenebilir.

- Geniş Dünya İthalatçıları - Standart veritabanı yedekleme dosyasının nasıl geri yüklenir olduğunu gösteren hızlı bir başlangıç için, [bkz.](sql-database-managed-instance-get-started-restore.md) Bu hızlı başlangıç, bir yedekleme dosyasını Azure blob depolama alanına yüklemeniz ve Paylaşılan erişim imzası (SAS) anahtarını kullanarak güvenli hale getirmek zorunda olduğunuzu gösterir.
- URL'den geri yükleme hakkında daha fazla bilgi [için, URL'den Yerel GERI YÜKLEME'ye](sql-database-managed-instance-migrate.md#native-restore-from-url)bakın.

> [!IMPORTANT]
> Yönetilen bir örneğin yedekleri yalnızca yönetilen başka bir örneğe geri yüklenebilir. Bunlar şirket içi BIR SQL Server'a veya tek bir veritabanı/elastik havuza geri yüklenemez.

### <a name="data-migration-service"></a>Veri Geçiş Hizmeti

Azure Veritabanı Geçiş Hizmeti, birden çok veritabanı kaynağından Azure Veri platformlarına en az kapalı kalma süresiyle sorunsuz geçişler sağlamak üzere tasarlanmış tam olarak yönetilen bir hizmettir. Bu hizmet, varolan üçüncü taraf ve SQL Server veritabanlarını Azure SQL Veritabanına (tek veritabanları, elastik havuzlarda havuzlu veritabanları ve yönetilen bir örnekte örnek veritabanları) ve Azure VM'deki SQL Server'a taşımak için gereken görevleri kolaylaştırır. [DMS kullanarak şirket içi veritabanınızı yönetilen örneğe nasıl geçirebilirsiniz.](https://aka.ms/migratetoMIusingDMS)

## <a name="sql-features-supported"></a>Desteklenen SQL özellikleri

Yönetilen örnek dağıtım seçeneği, hizmet genel kullanılabilirliğine kadar aşamalı olarak gelen şirket içi SQL Server ile %100'e yakın yüzey alanı uyumluluğu sağlamayı amaçlamaktadır. Bir özellik ve karşılaştırma listesi için, [SQL Veritabanı özellik karşılaştırması](sql-database-features.md)ve yönetilen örneklerdeki T-SQL farklılıklarının bir listesi için SQL [Server'dan yönetilen örnek T-SQL farklılıklarına](sql-database-managed-instance-transact-sql-information.md)bakın.

Yönetilen örnek dağıtım seçeneği, SQL 2008 veritabanlarına geriye dönük uyumluluğu destekler. SQL 2005 veritabanı sunucularından doğrudan geçiş desteklenir, geçirilen SQL 2005 veritabanları için uyumluluk düzeyi SQL 2008'e güncelleştirilir.
  
Aşağıdaki diyagram, yönetilen örnekte yüzey alanı uyumluluğunu özetler:  

![Geçiş](./media/sql-database-managed-instance/migration.png)

### <a name="key-differences-between-sql-server-on-premises-and-in-a-managed-instance"></a>SQL Server şirket içi ve yönetilen bir örnekte arasındaki temel farklar

Yönetilen örnek dağıtım seçeneği bulutta her zaman güncel olmaktan yararlanır, bu da şirket içi SQL Server'daki bazı özelliklerin eski, kullanımdan kaldırılmış veya alternatifleri olabileceği anlamına gelir. Araçların belirli bir özelliğin biraz farklı bir şekilde çalıştığını veya hizmetin tam olarak kontrol etmediğiniz bir ortamda çalıştığını tanıması gereken özel durumlar vardır:

- Yüksek kullanılabilirlik, [Always On Availability Groups'a](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server)benzer bir teknoloji kullanılarak yerleşik ve önceden yapılandırılmıştır.
- Otomatik yedeklemeler ve nokta zaman geri yükleme. Müşteri, otomatik `copy-only` yedekleme zincirini engellemeyen yedeklemeler başlatabilir.
- Yönetilen örnek, tüm ilgili senaryoların farklı şekilde desteklenmesi gerektiği için tam fiziksel yolların belirtilmesine izin vermez: RESTORE DB MOVE ile desteklemez, CREATE DB fiziksel yollara izin vermez, TOPLU INSERT yalnızca Azure Blobs ile çalışır, vb.
- Yönetilen örnek, Windows kimlik doğrulamasına bulut alternatifi olarak [Azure AD kimlik doğrulamasını](sql-database-aad-authentication.md) destekler.
- Yönetilen örnek, Bellek İçi OLTP nesneleri içeren veritabanları için XTP dosya grubunu ve dosyalarını otomatik olarak yönetir
- Yönetilen örnek SQL Server Integration Services 'ı (SSIS) destekler ve SSIS paketlerini depolayan SSIS kataloğunu (SSISDB) barındırabilir, ancak bunlar Azure Veri Fabrikası'nda (ADF) yönetilen bir Azure-SSIS Tümleştirme Çalışma Süresi 'nde (IR) yürütülür, [bkz.](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime) SQL Veritabanı'ndaki SSIS özelliklerini karşılaştırmak için [bkz.](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance)

### <a name="managed-instance-administration-features"></a>Yönetilen örnek yönetim özellikleri

Yönetilen örnek dağıtım seçeneği, SQL Veritabanı hizmeti bunları sizin için gerçekleştirdiğinden veya bu görevleri büyük ölçüde basitleştirdiği için sistem yöneticisinin yönetim görevlerine daha az zaman harcamasını sağlar. Örneğin, [OS / RDBMS yükleme ve yama,](sql-database-high-availability.md) [dinamik örnek yeniden boyutlandırma ve yapılandırma,](sql-database-single-database-scale.md) [yedekleme,](sql-database-automated-backups.md) [veritabanı çoğaltma](replication-with-sql-database-managed-instance.md) (sistem veritabanları dahil), [yüksek kullanılabilirlik yapılandırmave](sql-database-high-availability.md)sistem ve [performans izleme](../azure-monitor/insights/azure-sql.md) veri akışları yapılandırma.

> [!IMPORTANT]
> Desteklenen, kısmen desteklenen ve desteklenmeyen özelliklerin listesi için [SQL Veritabanı özelliklerine](sql-database-features.md)bakın. Yönetilen örneklerdeki T-SQL farklılıklarının bir listesi için SQL [Server'dan yönetilen örnek T-SQL farklılıklarına](sql-database-managed-instance-transact-sql-information.md) bakın

### <a name="how-to-programmatically-identify-a-managed-instance"></a>Yönetilen bir örneği programlı olarak tanımlama

Aşağıdaki tablo, uygulamanızın yönetilen örnekle çalıştığını algılamak ve önemli özellikleri almak için kullanabileceğiniz Transact SQL aracılığıyla erişilebilen çeşitli özellikleri gösterir.

|Özellik|Değer|Açıklama|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Telif Hakkı (C) 2018 Microsoft Corporation.|Bu değer SQL Veritabanı'ndakiyle aynıdır. Bu, SQL engine sürüm **12'yi** göstermez (SQL Server 2014). Yönetilen örnek her zaman SQL Server'ın en son kullanılabilir RTM sürümüne eşit veya daha yüksek olan en son kararlı SQL motor sürümünü çalıştırılır.  |
|`SERVERPROPERTY ('Edition')`|SQL Azure|Bu değer SQL Veritabanı'ndakiyle aynıdır.|
|`SERVERPROPERTY('EngineEdition')`|8|Bu değer, yönetilen bir örneği benzersiz olarak tanımlar.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Aşağıdaki biçimde tam örnek DNS adı:`<instanceName>`. `<dnsPrefix>`.database.windows.net, `<instanceName>` müşteri tarafından sağlanan adın `<dnsPrefix>` bulunduğu yer, adın global DNS adının benzersizliğini garanti eden otomatik olarak oluşturulmuş bir parçası iken (örneğin wcus17662feb9ce98))|Örnek: my-managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen ilk örneğini nasıl oluşturabilirsiniz öğrenmek için [Quickstart kılavuzuna](sql-database-managed-instance-get-started.md)bakın.
- Özellikler ve karşılaştırma listesi için [SQL ortak özelliklerine](sql-database-features.md)bakın.
- VNet yapılandırması hakkında daha fazla bilgi için [yönetilen örnek VNet](sql-database-managed-instance-connectivity-architecture.md)yapılandırması'na bakın.
- Yönetilen bir örnek oluşturan ve bir yedekleme dosyasından veritabanını geri yükleyen hızlı [bir](sql-database-managed-instance-get-started.md)başlangıç için bkz.
- Geçiş için Azure Veritabanı Geçiş Hizmeti'ni (DMS) kullanan bir öğretici için, [Yönetilen Örnek Geçişi'ne bakın.](../dms/tutorial-sql-server-to-managed-instance.md)
- Yerleşik sorun giderme zekasıyla yönetilen örnek veritabanı performansının gelişmiş izlenmesi için Azure [SQL Analytics'i kullanarak Azure SQL Veritabanını Izleyin](../azure-monitor/insights/azure-sql.md)bölümüne bakın.
- Fiyatlandırma bilgileri için, [SQL Veritabanı yönetilen örnek fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/managed/)bakın.
