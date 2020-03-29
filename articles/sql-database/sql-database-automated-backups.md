---
title: Otomatik, coğrafi yedekli yedeklemeler
description: SQL Veritabanı birkaç dakikada bir otomatik olarak yerel bir veritabanı yedeklemesi oluşturur ve coğrafi yedeklilik için Azure okuma-erişim coğrafi yedekli depolama kullanır.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab, danil
manager: craigg
ms.date: 12/13/2019
ms.openlocfilehash: 9ac6927df63d51830a58773e32ad0968920c0867
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061772"
---
# <a name="automated-backups"></a>Otomatik yedeklemeler

Azure SQL Veritabanı, yapılandırılan bekletme süresi boyunca tutulan veritabanı yedeklemelerini otomatik olarak oluşturur. Veri merkezi kullanılamamış olsa bile yedeklemelerin korunmasını sağlamak için Azure [okuma-erişim coğrafi yedekli depolama (RA-GRS)](../storage/common/storage-redundancy.md) kullanır.

Veritabanı yedeklemeleri, verilerinizi yanlışlıkla oluşan bozulmaveya silmeye karşı korudukları için herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır. Güvenlik kurallarınız yedeklemelerinizin uzun bir süre (10 yıla kadar) kullanılabilir olmasını gerektiriyorsa, singleton veritabanlarında ve esnek veritabanı havuzlarında [uzun süreli bekletme](sql-database-long-term-retention.md) yapılandırabilirsiniz.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>SQL Veritabanı yedeklemesi nedir?

SQL Veritabanı, her hafta [tam yedekleme,](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server) her 12 saatte bir [diferansiyel yedekleme ve](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) her 5 ila 10 dakikada bir işlem günlüğü [yedeklemeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) oluşturmak için SQL Server teknolojisini kullanır. Yedeklemeler, veri merkezi kesintisine karşı korunmak üzere eşleştirilmiş bir [veri merkezine](../best-practices-availability-paired-regions.md) çoğaltılan [RA-GRS depolama lekelerinde](../storage/common/storage-redundancy.md) depolanır. Bir veritabanını geri yüklediğinizde, hizmet hangi tam, diferansiyel ve işlem günlüğü yedeklemelerinin geri yüklenmesi gerektiğini belirler.

Bu yedeklemeleri kullanarak şunları yapabilirsiniz:

- Azure portalı, Azure PowerShell, Azure CLI veya REST API'sını **kullanarak, varolan bir veritabanını bekletme** süresi içinde geçmiş bir noktaya geri yükleyin. Tek veritabanları ve elastik veritabanı havuzları için bu işlem, özgün veritabanıyla aynı sunucuda yeni bir veritabanı oluşturur. Yönetilen durumda, bu işlem veritabanının bir kopyasını veya aynı abonelik altında aynı veya farklı yönetilen örneği oluşturabilir.
- **Silinen bir veritabanını silme zamanına** veya bekletme süresi içinde her zaman geri yükleyin. Silinen veritabanı yalnızca özgün veritabanının oluşturulduğu aynı mantıksal sunucuda veya yönetilen örnekte geri yüklenebilir.
- **Veritabanını başka bir coğrafi bölgeye geri yükleyin.** Coğrafi geri yükleme, sunucunuza ve veritabanınıza erişemediğinizde coğrafi bir felaketten kurtulmanızı sağlar. Dünyanın herhangi bir yerindeki mevcut sunucularda yeni bir veritabanı oluşturur.
- Veritabanı uzun vadeli bir bekletme ilkesi (LTR) ile yapılandırılırsa, tek bir veritabanı veya elastik veritabanı havuzunda belirli bir **uzun vadeli yedeklemeden veritabanını geri yükleyin.** LTR, bir uyumluluk isteğini karşılamak veya uygulamanın eski bir sürümünü çalıştırmak için [Azure portalını](sql-database-long-term-backup-retention-configure.md#using-azure-portal) veya [Azure PowerShell'i](sql-database-long-term-backup-retention-configure.md#using-powershell) kullanarak veritabanının eski bir sürümünü geri yüklemenize olanak tanır. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

Geri yükleme gerçekleştirmek için, [yedeklerden veritabanını geri yükleme'ye](sql-database-recovery-using-backups.md)bakın.

> [!NOTE]
> Azure Depolama'da *çoğaltma* terimi, dosyaları bir konumdan diğerine kopyalamayı ifade eder. SQL Server *veritabanı çoğaltma,* birden çok ikincil veritabanının birincil veritabanıyla eşitlenmiş tutulması anlamına gelir.

Aşağıdaki örnekleri kullanarak bu işlemlerden bazılarını deneyebilirsiniz:

| | Azure portal | Azure PowerShell |
|---|---|---|
| Yedekleme bekletme | [Tek veritabanı](sql-database-automated-backups.md?tabs=managed-instance#change-the-pitr-backup-retention-period-by-using-the-azure-portal) <br/> [Yönetilen örnek](sql-database-automated-backups.md?tabs=managed-instance#change-the-pitr-backup-retention-period-by-using-the-azure-portal) | [Tek veritabanı](sql-database-automated-backups.md#change-the-pitr-backup-retention-period-by-using-powershell) <br/>[Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasebackupshorttermretentionpolicy) |
| Uzun vadeli yedekleme tutma değiştirme | [Tek veritabanı](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies)<br/>Yönetilen örnek - N/A  | [Tek veritabanı](sql-database-long-term-backup-retention-configure.md)<br/>Yönetilen örnek - N/A  |
| Zaman içinde bir noktadan veritabanını geri yükleme | [Tek veritabanı](sql-database-recovery-using-backups.md#point-in-time-restore) | [Tek veritabanı](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) <br/> [Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase) |
| Silinen veritabanını geri yükleme | [Tek veritabanı](sql-database-recovery-using-backups.md) | [Tek veritabanı](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) <br/> [Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeletedinstancedatabasebackup)|
| Azure Blob depolamasından veritabanı geri yükleme | Tek veritabanı - N/A <br/>Yönetilen örnek - N/A  | Tek veritabanı - N/A <br/>[Yönetilen örnek](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) |


## <a name="backup-frequency"></a>Yedekleme sıklığı

### <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

SQL Veritabanı otomatik olarak tam yedeklemeler, diferansiyel yedeklemeler ve işlem günlüğü yedeklemeleri oluşturarak zaman içinde geri yükleme (PITR) için self servis destekler. Tam veritabanı yedeklemeleri haftalık olarak oluşturulur ve diferansiyel veritabanı yedekleri genellikle her 12 saatte bir oluşturulur. Hareket günlüğü yedeklemeleri genellikle her 5 ila 10 dakikada bir oluşturulur. İşlem günlüğü yedeklemelerinin sıklığı, işlem boyutuna ve veritabanı etkinliği miktarına bağlıdır. 

İlk tam yedekleme, veritabanı oluşturulduktan hemen sonra zamanlanır. Bu yedekleme genellikle 30 dakika içinde tamamlanır, ancak veritabanı büyük olduğunda daha uzun sürebilir. Örneğin, ilk yedekleme geri yüklenen bir veritabanında veya veritabanı kopyasında daha uzun sürebilir. İlk tam yedeklemeden sonra diğer tüm yedeklemeler otomatik olarak zamanlanır ve arka planda sessizce yönetilir. Tüm veritabanı yedeklemelerinin tam zamanlaması, genel sistem iş yükünü dengelediği için SQL Veritabanı hizmeti tarafından belirlenir. Yedekleme işlerini değiştiremez veya devre dışı bırakamazsınız.

PITR yedeklemeleri coğrafi yedekli depolama ile korunur. Daha fazla bilgi için bkz. [Azure Depolama yedekliliği](../storage/common/storage-redundancy.md).

PITR hakkında daha fazla bilgi için [bkz.](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="long-term-retention"></a>Uzun vadeli bekletme

Tek ve havuzlu veritabanlarıiçin, Azure Blob depolama alanında 10 yıla kadar uzun süreli yedeklemelerin uzun süreli bekletme (LTR) yapılandırılabilir. LTR ilkesini etkinleştiriseniz, haftalık tam yedeklemeler otomatik olarak farklı bir RA-GRS depolama kabına kopyalanır. Çeşitli uyumluluk gereksinimlerini karşılamak için, haftalık, aylık ve/veya yıllık yedeklemeler için farklı bekletme dönemleri seçebilirsiniz. Depolama tüketimi, yedeklerin seçili sıklığına ve bekletme süresine veya dönemlere bağlıdır. LTR depolama maliyetini tahmin etmek için [LTR fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/?service=sql-database) kullanabilirsiniz.

PITR yedeklemeleri gibi LTR yedeklemeleri de coğrafi yedekli depolama alanıyla korunur. Daha fazla bilgi için bkz. [Azure Depolama yedekliliği](../storage/common/storage-redundancy.md).

LTR hakkında daha fazla bilgi [için, uzun süreli yedekleme bekletme](sql-database-long-term-retention.md)bakın.

## <a name="backup-storage-consumption"></a>Yedekleme depolama tüketimi

Tek veritabanları için bu denklem, toplam yedekleme depolama kullanımını hesaplamak için kullanılır:

`Total backup storage size = (size of full backups + size of differential backups + size of log backups) – database size`

Esnek veritabanı havuzları için, toplam yedekleme depolama boyutu havuz düzeyinde toplanır ve aşağıdaki gibi hesaplanır:

`Total backup storage size = (total size of all full backups + total size of all differential backups + total size of all log backups) - allocated pool data storage`

Bekletme döneminden önce oluşan yedeklemeler, zaman damgasına göre otomatik olarak temizlenir. Diferansiyel yedeklemeler ve günlük yedeklemeleri yararlı olması için daha önce tam yedekleme gerektirdiğinden, haftalık parçalar halinde birlikte temizlenirler.

Azure SQL Veritabanı, toplam bekletme yedekleme depolamanızı kümülatif bir değer olarak hesaplanır. Her saat, bu değer, her ayın sonunda tüketiminizi hesaplamak için bu saatlik kullanımı toplamaktan sorumlu olan Azure faturalandırma ardışık birimine bildirilir. Veritabanı düştükten sonra, yedeklemeyaşlandıkça tüketim azalır. Yedeklemeler bekletme döneminden daha eski hale geldikten sonra faturalandırma durur.

   > [!IMPORTANT]
   > Veritabanı bırakılsa bile, veritabanının yedekleri belirtilen bekletme süresi boyunca tutulur. Bir veritabanını düşürmek ve yeniden oluşturmak depolama ve bilgi işlem maliyetlerinden sık sık tasarruf sağlarken, Microsoft her bırakılan veritabanı için belirtilen bekletme süresi için bir yedekleme sakladığından (en az 7 gün) yedekleme depolama maliyetlerini artırabilir zaman düştü.

### <a name="monitor-consumption"></a>Tüketimi izleyin

Her yedekleme türü (tam, diferansiyel ve günlük) veritabanı izleme bıçağında ayrı bir metrik olarak bildirilir. Aşağıdaki diyagram, tek bir veritabanı için yedekleme depolama tüketimini nasıl izleyebilirsiniz gösterir. Bu özellik şu anda yönetilen örnekler için kullanılamaz.

![Azure portalında veritabanı yedekleme tüketimini izleme](media/sql-database-automated-backup/backup-metrics.png)

### <a name="fine-tune-backup-storage-consumption"></a>Yedekleme depolama alanı tüketiminde ince ayar

Aşırı yedekleme depolama alanı tüketimi, iş yüküne ve tek tek veritabanlarının boyutuna bağlıdır. Yedekleme depolama tüketiminizi azaltmak için aşağıdaki aparatlama tekniklerinden bazılarını göz önünde bulundurun:

* Yedekleme [bekletme süresini](#change-the-pitr-backup-retention-period-by-using-the-azure-portal) ihtiyaçlarınız için mümkün olan en aza indirin.
* Dizin yeniden oluşturma gibi büyük yazma işlemleri yapmaktan kaçının, gereksinim duyduğunuzdan daha sık.
* Büyük veri yükü işlemleri [için, kümelenmiş sütun deposu dizinlerini](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes)kullanmayı düşünün, kümelenmeyen dizinlerin sayısını azaltın ve satır sayısı 1 milyon civarında olan toplu yük işlemlerini düşünün.
* Genel amaçlı hizmet katmanında, sağlanan veri depolama, fazla yedekleme depolama fiyatından daha ucuzdur. Sürekli olarak yüksek fazla yedekleme depolama maliyetleriniz varsa, yedekleme depolamadan tasarruf etmek için veri depolama alanını artırmayı düşünebilirsiniz.
* Geçici sonuçları depolamak için ETL mantığınızda kalıcı tablolar yerine TempDB'yi kullanın. (Yalnızca yönetilen örnek için geçerlidir.)
* Hassas veriler içermeyen veritabanları (örneğin geliştirme veya test veritabanları) için TDE şifrelemeyi kapatmayı düşünün. Şifrelenmemiş veritabanlarının yedekleri genellikle daha yüksek sıkıştırma oranıyla sıkıştırılır.

> [!IMPORTANT]
> Analitik veri mart \ veri ambarı iş yükleri için, [kümelenmiş sütun deposu dizinleri](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes)kullanmanızı, kümelenmeyen dizinlerin sayısını azaltmanızı ve fazla yedekleme depolama tüketimini azaltmak için satır sayısı yaklaşık 1 milyon olan toplu yük işlemlerini düşünmenizi öneririz.

## <a name="storage-costs"></a>Depolama maliyetleri

Depolama fiyatı, DTU modelini mi yoksa vCore modelini mi kullandığınıza bağlı olarak değişir.

### <a name="dtu-model"></a>DTU modeli

DTU modelini kullanıyorsanız, veritabanları ve elastik veritabanı havuzları için yedekleme depolama için ek ücret alınmaz.

### <a name="vcore-model"></a>Sanal çekirdek modeli

Tek veritabanları için, veritabanı boyutunun yüzde 100'üne eşit minimum yedekleme depolama miktarı ek ücret ödemeden sağlanır. Elastik veritabanı havuzları ve yönetilen örnekler için, havuz veya örnek boyutu için ayrılan veri depolamasının sırasıyla yüzde 100'üne eşit minimum yedekleme depolama tutarı ek ücret ödemeden sağlanır. Ek yedekleme alanı kullanımı aylık GB olarak ücretlendirilir. Bu ek tüketim, tek tek veritabanlarının iş yüküne ve boyutuna bağlıdır.

Azure SQL Veritabanı, toplam bekletme yedekleme depolamaalanınızı kümülatif bir değer olarak hesaplar. Her saat, bu değer, her ayın sonunda tüketiminizi almak için bu saatlik kullanımı toplamaktan sorumlu olan Azure faturalandırma ardışık birimine bildirilir. Veritabanı düştükten sonra, Microsoft yedekleme yaşlandıkça tüketimi azaltır. Yedeklemeler bekletme döneminden daha eski hale geldikten sonra faturalandırma durur. Tüm günlük yedeklemeleri ve diferansiyel yedeklemeler tam bekletme süresi boyunca tutulduğundan, ağır şekilde değiştirilen veritabanlarında daha yüksek yedekleme ücretleri olacaktır.

Bir veritabanının 744 GB yedekleme depolama alanı biriktirdiğini ve bu tutarın tüm ay boyunca sabit kaldığını varsayalım. Bu kümülatif depolama tüketimini saatlik kullanıma dönüştürmek için 744,0 (ayda 31 gün * günde 24 saat) bölün. Yani SQL Veritabanı veritabanı her saat 1 GB PITR yedekleme tüketilen rapor edecektir. Azure faturalandırması bu tüketimi toplar ve tüm ay boyunca 744 GB kullanım gösterir. Maliyet, bölgenizdeki $/GB/ay oranına göre olacaktır.

Şimdi, daha karmaşık bir örnek. Veritabanının bekletme sayısının ay ortasında 14 güne çıkarOlduğunu varsayalım. Bu artışın (varsayımsal olarak) toplam yedekleme depolamasının 1.488 GB'a katlanarak sonuçlanmasını varsayalım. SQL Veritabanı saat 1 ile 372 arasında 1 GB kullanım rapor edin. Bu saat 373 ile 744 için 2 GB olarak kullanımı rapor olur. Bu kullanım, 1.116 GB/ay'lık son faturaya toplanır.

### <a name="monitor-costs"></a>Maliyetleri izleme

Yedekleme depolama maliyetlerini anlamak için Azure portalında **Maliyet Yönetimi + Faturalandırma'ya** gidin, **Maliyet Yönetimi'ni**seçin ve ardından **Maliyet çözümlemesi'ni**seçin. **Kapsam**olarak istediğiniz aboneliği seçin ve ardından ilgilendiğiniz zaman dilimi ve hizmet için filtre uygulayın.

**Hizmet adı**için bir filtre ekleyin ve ardından açılan listede **sql veritabanını** seçin. Hizmetinizin fatura sayacını seçmek için **sayaç alt kategori** filtresini kullanın. Tek bir veritabanı veya elastik veritabanı havuzu için **tek/elastik havuz pitr yedekleme depolama seçin.** Yönetilen bir örnek için **mi pitr yedekleme depolama alanını**seçin. **Depolama** ve **bilgi işlem** alt kategorileri de ilginizi çekebilir, ancak bunlar yedekleme depolama maliyetleriyle ilişkili değildir.

![Yedekleme depolama maliyet analizi](./media/sql-database-automated-backup/check-backup-storage-cost-sql-mi.png)

## <a name="backup-retention"></a>Yedekleri bekletme

Tüm Azure SQL veritabanları (tek, havuzlu ve yönetilen örnek veritabanları) varsayılan yedekleme bekletme süresi 7 gün dür. Yedekleme [bekletme süresini](#change-the-pitr-backup-retention-period) 35 güne kadar değiştirebilirsiniz.

Bir veritabanını silerseniz, SQL Veritabanı yedeklemeleri çevrimiçi bir veritabanında olduğu gibi tutar. Örneğin, bekletme süresi yedi gün olan bir Temel veritabanını silerseniz, dört günlük bir yedekleme üç gün daha kaydedilir.

Yedeklemeleri maksimum bekletme süresinden daha uzun süre tutmanız gerekiyorsa, veritabanınıza bir veya daha fazla uzun süreli bekletme süreleri eklemek için yedekleme özelliklerini değiştirebilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

> [!IMPORTANT]
> SQL veritabanlarını barındıran Azure SQL sunucusunu silerseniz, sunucuya ait tüm esnek veritabanı havuzları ve veritabanları da silinir. Kurtarılamazlar. Silinen bir sunucunun geri yüklenmez. Ancak uzun süreli bekletme yapılandırırsanız, LTR'li veritabanlarının yedekleri silinmez ve bu veritabanları geri yüklenebilir.

## <a name="encrypted-backups"></a>Şifreli yedeklemeler

Veritabanınız TDE ile şifrelenmişse, yedeklemeler LTR yedeklemeleri de dahil olmak üzere istirahatte otomatik olarak şifrelenir. Azure SQL veritabanı için TDE etkinleştirildiğinde, yedeklemeler de şifrelenir. Tüm yeni Azure SQL veritabanları varsayılan olarak TDE etkinleştirilmiş olarak yapılandırılır. TDE hakkında daha fazla bilgi için [Azure SQL Veritabanı ile Saydam Veri Şifreleme](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)bölümüne bakın.

## <a name="backup-integrity"></a>Yedekleme bütünlüğü

Azure SQL Veritabanı mühendislik ekibi sürekli olarak, mantıksal sunuculara ve esnek veritabanı havuzlarına yerleştirilen veritabanlarının otomatik veritabanı yedeklemelerinin geri yüklenmesini otomatik olarak sınar. (Bu sınama yönetilen örnekte kullanılamaz.) Zaman içinde geri yükleme üzerine, veritabanları da DBCC CHECKDB bütünlük denetimleri alırsınız.

Yönetilen örnek, geçiş `CHECKSUM` tamamlandıktan sonra yerel `RESTORE` komutla geri yüklenen veritabanları veya Azure Veri Geçiş Hizmeti ile otomatik ilk yedeklemeyi alır.

Bütünlük denetimi sırasında bulunan tüm sorunlar mühendislik ekibine bir uyarı ile sonuçlanır. Daha fazla bilgi için [Azure SQL Veritabanı'nda Veri Bütünlüğü'ne](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/)bakın.

## <a name="compliance"></a>Uyumluluk

Veritabanınızı DTU tabanlı bir hizmet katmanından vCore tabanlı bir hizmet katmanına geçirdiğinizde, UYGULAMANIZIn veri kurtarma ilkesinin tehlikeye girmediğinden emin olmak için PITR bekletme korunur. Varsayılan bekletme uyumluluk gereksinimlerinizi karşılamazsa, PowerShell veya REST API'yi kullanarak PITR saklama süresini değiştirebilirsiniz. Daha fazla bilgi için [pitr yedekleme bekletme süresini değiştir'e](#change-the-pitr-backup-retention-period)bakın.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-pitr-backup-retention-period"></a>PITR yedekleme bekletme süresini değiştirme

Azure portalını, PowerShell'i veya REST API'sini kullanarak varsayılan PITR yedekleme bekletme süresini değiştirebilirsiniz. Aşağıdaki örnekler, PITR bekletme işleminin 28 güne nasıl değiştirilebildiğini göstermektedir.

> [!WARNING]
> Geçerli bekletme süresini azaltırsanız, yeni bekletme döneminden eski olan varolan tüm yedeklemeler artık kullanılamaz. Geçerli bekletme süresini artırırsanız, SQL Veritabanı varolan yedeklemeleri daha uzun bekletme süresinin sonuna kadar saklar.

> [!NOTE]
> Bu API'ler yalnızca PITR bekletme süresini etkiler. Veritabanınız için LTR'yi yapılandırırsanız, bu etkilenmez. LTR bekletme süreleri nasıl değiştirilen hakkında bilgi için [bkz.](sql-database-long-term-retention.md)

### <a name="change-the-pitr-backup-retention-period-by-using-the-azure-portal"></a>Azure portalını kullanarak PITR yedekleme bekletme süresini değiştirme

Azure portalını kullanarak PITR yedekleme bekletme süresini değiştirmek için, portalda saklama süresini değiştirmek istediğiniz sunucu nesnesine gidin. Ardından, değiştirdiğiniz sunucu nesnesini temel alan uygun seçeneği seçin.

#### <a name="single-database-and-elastic-database-pools"></a>[Tek veritabanı ve elastik veritabanı havuzları](#tab/single-database)

Tek Bir Azure SQL veritabanları için PITR yedekleme bekletme değişiklikleri sunucu düzeyinde yapılır. Sunucu düzeyinde yapılan değişiklikler sunucudaki veritabanları için geçerlidir. Azure portalından bir Azure SQL Veritabanı sunucusu için PITR bekletmesini değiştirmek için sunucuya genel bakış bıçağına gidin. Sol bölmede **Yedeklemeleri Yönet'i** seçin ve ardından ekranın üst kısmında **bekletme yapılandırma'yı** seçin:

![PITR tutma, sunucu düzeyini değiştirme](./media/sql-database-automated-backup/configure-backup-retention-sqldb.png)

#### <a name="managed-instance"></a>[Yönetilen örnek](#tab/managed-instance)

SQL Veritabanı yönetilen örnekleri için PITR yedekleme bekletme değişiklikleri tek bir veritabanı düzeyinde yapılır. Azure portalından bir örnek veritabanı için PITR yedekleme bekletmesini değiştirmek için tek tek veritabanına genel bakış bıçağına gidin. Ardından ekranın üst kısmında **ki yedekleme bekletmesini Yapılandır'ı** seçin:

![PITR tutma, yönetilen örnek değiştirme](./media/sql-database-automated-backup/configure-backup-retention-sqlmi.png)

---

### <a name="change-the-pitr-backup-retention-period-by-using-powershell"></a>PowerShell'i kullanarak PITR yedekleme saklama süresini değiştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell AzureRM modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirmeler Az.Sql modülü içindir. Daha fazla bilgi için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)'e bakın. Az modülündeki komutların bağımsız değişkenleri AzureRm modüllerinde bulunanlarla önemli ölçüde aynıdır.

```powershell
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

### <a name="change-the-pitr-backup-retention-period-by-using-the-rest-api"></a>REST API'sini kullanarak PITR yedekleme bekletme süresini değiştirme

#### <a name="sample-request"></a>Örnek istek

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```

#### <a name="request-body"></a>İstek gövdesi

```json
{
  "properties":{
    "retentionDays":28
  }
}
```

#### <a name="sample-response"></a>Örnek yanıt

Durum kodu: 200

```json
{
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/providers/Microsoft.Sql/resourceGroups/resourceGroup/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default",
  "name": "default",
  "type": "Microsoft.Sql/resourceGroups/servers/databases/backupShortTermRetentionPolicies",
  "properties": {
    "retentionDays": 28
  }
}
```

Daha fazla bilgi için Bkz. [Yedekleme Saklama DINLENME API.](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies)

## <a name="next-steps"></a>Sonraki adımlar

- Veritabanı yedeklemeleri, verilerinizi yanlışlıkla oluşan bozulmaveya silmeye karşı korudukları için herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır. Diğer Azure SQL Veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için [İş sürekliliğine genel bakış'a](sql-database-business-continuity.md)bakın.
- [Azure portalını kullanarak veritabanını bir noktaya](sql-database-recovery-using-backups.md)nasıl geri yükleyin hakkında daha fazla bilgi edinin.
- [PowerShell'i kullanarak veritabanını bir noktaya](scripts/sql-database-restore-database-powershell.md)nasıl geri yükleyin hakkında daha fazla bilgi edinin.
- Azure portalını kullanarak Azure Blob depolama alanında otomatik yedeklemelerin uzun süreli bekletmesinde nasıl yapılandırılabilen, yönetilip geri yükleyeceklerine ilişkin bilgi [için](sql-database-long-term-backup-retention-configure.md)bkz.
- PowerShell'i kullanarak Azure Blob depolamasında otomatik yedeklemelerin uzun süreli bekletmesinden nasıl yapılandırılabilen, yönetecek ve geri yükleyecekleri hakkında bilgi için [powershell'i kullanarak uzun süreli yedekleme tutmayı yönet'e](sql-database-long-term-backup-retention-configure.md)bakın.
