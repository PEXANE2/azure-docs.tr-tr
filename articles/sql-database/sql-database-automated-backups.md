---
title: Otomatik, coğrafi olarak yedekli yedeklemeler
description: SQL veritabanı, her birkaç dakikada bir yerel veritabanı yedeklemesi oluşturur ve coğrafi yedeklilik için Azure Okuma Erişimli Coğrafi olarak yedekli depolama kullanır.
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
ms.openlocfilehash: 6b880696b4922c68c73ce4ff59f72a62ce5a5a30
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348957"
---
# <a name="automated-backups"></a>Otomatik yedeklemeler

SQL veritabanı, yapılandırılan saklama süresi boyunca tutulan veritabanı yedeklerini otomatik olarak oluşturur ve veri merkezi kullanılabilir durumda olsa bile korundıklarından emin olmak için Azure [Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS)](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) kullanır. Bu yedeklemeler otomatik olarak oluşturulur. Veritabanı yedeklemeleri, verilerinizin yanlışlıkla bozulma veya silme işlemlerini korumasından dolayı, herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır. Güvenlik kurallarınız, yedeklemelerinizin uzun bir süre (10 yıla kadar) kullanılabilmesini gerektiriyorsa, tek bir veritabanı ve elastik havuzlarda [uzun süreli bir bekletme](sql-database-long-term-retention.md) yapılandırabilirsiniz.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>SQL veritabanı yedeklemesi nedir?

SQL veritabanı her hafta [tam yedeklemeler](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server) oluşturmak için SQL Server teknolojisini kullanır, her 12 saatte bir [fark yedeklemeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) ve 5-10 dakikada bir [işlem günlüğü yedeklemesi](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) . Yedeklemeler, veri merkezi kesintisine karşı koruma için [eşleştirilmiş bir veri merkezine](../best-practices-availability-paired-regions.md) çoğaltılan [RA-GRS depolama Blobları](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) 'nda depolanır. Bir veritabanını geri yüklerken, hizmet hangi tam, fark ve işlem günlüğü yedeklemelerinin geri yüklenmesi gerektiğini belirler.

Bu yedeklemeleri kullanarak şunları yapabilirsiniz:

- Mevcut bir veritabanını, Azure portal, Azure PowerShell, Azure CLı veya REST API kullanarak saklama süresi içinde **geçmiş bir noktaya geri yükleyin** . Tek veritabanı ve elastik havuzlarda bu işlem, özgün veritabanıyla aynı sunucuda yeni bir veritabanı oluşturur. Yönetilen örnekte bu işlem, aynı abonelik kapsamında veritabanının veya aynı ya da farklı yönetilen örneğin bir kopyasını oluşturabilir.
- **Silinen bir veritabanını silindiği zamana** veya saklama dönemi içinde herhangi bir zamanda geri yükleyin. Silinen veritabanı yalnızca özgün veritabanının oluşturulduğu mantıksal sunucuya veya yönetilen örneğe geri yüklenebilir.
- **Veritabanını başka bir coğrafi bölgeye geri yükleyin**. Coğrafi geri yükleme, sunucunuza ve veritabanınıza erişene zaman coğrafi bir olağanüstü durumdan kurtulmanızı sağlar. Dünyanın her yerindeki var olan herhangi bir sunucuda yeni bir veritabanı oluşturur.
- Veritabanı uzun süreli bir bekletme ilkesiyle (LTR) yapılandırılmışsa, Tek Veritabanı veya Elastik Havuz **belirli bir uzun süreli yedekten bir veritabanını geri yükleyin** . LTR, bir uyumluluk isteğini karşılamak veya uygulamanın eski bir sürümünü çalıştırmak için [Azure Portal](sql-database-long-term-backup-retention-configure.md#using-azure-portal) veya [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#using-powershell) kullanarak veritabanının eski bir sürümünü geri yüklemenize olanak tanır. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).
- Geri yükleme gerçekleştirmek için bkz. [veritabanlarını yedeklerden geri yükleme](sql-database-recovery-using-backups.md).

> [!NOTE]
> Azure depolama 'da, *çoğaltma* terimi dosyaları bir konumdan diğerine kopyalamayı belirtir. SQL *veritabanının veritabanı çoğaltması* , birden çok ikincil veritabanının birincil veritabanıyla eşitlenmiş kalmasını sağlar.

Aşağıdaki örnekleri kullanarak bu işlemlerden bazılarını deneyebilirsiniz:

| | Azure portal | Azure PowerShell |
|---|---|---|
| Yedekleme bekletmesini değiştirme | [Tek Veritabanı](sql-database-automated-backups.md?tabs=managed-instance#change-pitr-backup-retention-period-using-azure-portal) <br/> [Yönetilen örnek](sql-database-automated-backups.md?tabs=managed-instance#change-pitr-backup-retention-period-using-azure-portal) | [Tek Veritabanı](sql-database-automated-backups.md#change-pitr-backup-retention-period-using-powershell) <br/>[Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasebackupshorttermretentionpolicy) |
| Uzun süreli yedekleme bekletmesini değiştirme | [Tek veritabanı](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies)<br/>Yönetilen örnek-yok  | [Tek Veritabanı](sql-database-long-term-backup-retention-configure.md)<br/>Yönetilen örnek-yok  |
| Veritabanını zaman noktasından geri yükle | [Tek veritabanı](sql-database-recovery-using-backups.md#point-in-time-restore) | [Tek veritabanı](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) <br/> [Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase) |
| Silinen veritabanını geri yükleme | [Tek veritabanı](sql-database-recovery-using-backups.md) | [Tek veritabanı](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) <br/> [Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeletedinstancedatabasebackup)|
| Veritabanını Azure Blob depolamadan geri yükleme | Tek veritabanı-yok <br/>Yönetilen örnek-yok  | Tek veritabanı-yok <br/>[Yönetilen örnek](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) |


## <a name="backup-frequency"></a>Yedekleme sıklığı

### <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

SQL veritabanı, tam yedekleme, değişiklik yedeklemeleri ve işlem günlüğü yedeklemeleri otomatik olarak oluşturarak, zaman içinde nokta geri yükleme (ıNR) için self servis hizmetini destekler. Tam veritabanı yedeklemeleri haftalık olarak oluşturulur, fark veritabanı yedeklemeleri genellikle 12 saatte bir oluşturulur ve işlem günlüğü yedeklemeleri genellikle işlem boyutu ve veritabanı etkinliğinin miktarına göre sıklık ile her 5-10 dakikada bir oluşturulur. İlk tam yedekleme, bir veritabanı oluşturulduktan hemen sonra zamanlanır. Genellikle 30 dakika içinde tamamlanır, ancak veritabanı önemli boyutta olduğunda daha uzun sürebilir. Örneğin, ilk yedekleme geri yüklenen bir veritabanında veya bir veritabanı kopyasında daha uzun sürebilir. İlk tam yedeklemeden sonra diğer tüm yedeklemeler otomatik olarak zamanlanır ve arka planda sessizce yönetilir. Tüm veritabanı yedeklemelerinin tam zamanlaması, genel sistem iş yükünü dengelediği için SQL Veritabanı hizmeti tarafından belirlenir. Yedekleme işlerini değiştiremez veya devre dışı bırakamazsınız. 

INR yedeklemeleri coğrafi olarak yedekli ve [Azure Storage çapraz bölgesel çoğaltma](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) tarafından korunuyor

Daha fazla bilgi için bkz. [Noktadan noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="long-term-retention"></a>Uzun süreli saklama

Tek ve havuza alınmış veritabanları, Azure Blob depolamada en fazla 10 yıla kadar uzun süreli saklama (LTR) tam yedeklemeler yapılandırma seçeneği sunar. LTR ilkesi etkinse, haftalık tam yedeklemeler otomatik olarak farklı bir RA-GRS depolama kapsayıcısına kopyalanır. Farklı uyumluluk gereksinimini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı saklama süreleri seçebilirsiniz. Depolama alanı tüketimi, seçilen yedekleme sıklığına ve bekletme dönemine bağlıdır. LTR depolama maliyetini tahmin etmek için [LTR Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/?service=sql-database) ' yı kullanabilirsiniz.

Invr gibi, LTR yedeklemeler coğrafi olarak yedekli ve [Azure Storage çapraz bölgesel çoğaltma](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)tarafından korunur.

Daha fazla bilgi için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md).

## <a name="backup-storage-consumption"></a>Yedekleme depolama alanı tüketimi 

Tek veritabanları için toplam yedekleme depolama alanı kullanımı aşağıdaki şekilde hesaplanır:   
`Total backup storage size = (size of full backups + size of differential backups + size of log backups) – database size`.

Elastik havuzlar için toplam yedekleme depolama boyutu havuz düzeyinde toplanır ve şu şekilde hesaplanır:   
`Total backup storage size = (total size of all full backups + total size of all differential backups + total size of all log backups) - allocated pool data storage`. 

Saklama süresinden daha eski yedeklemeler, zaman damgalarına göre otomatik olarak temizlenir. Değişiklik yedeklemeleri ve günlük yedeklemeleri, daha önce bir tam yedeklemenin yararlı olmasını gerektirdiğinden, bunlar haftalık öbeklerde birlikte temizlenir. 

Azure SQL veritabanı, toplam bekletme yedekleme depolama alanınızı birikimli bir değer olarak hesaplacaktır. Bu değer, her ay sonunda tükeminizi hesaplamak üzere bu saatlik kullanımı sağlamaktan sorumlu olan Azure Faturalandırma ardışık düzenine bildirilir. Veritabanı bırakıldıktan sonra, tüketim yedekleme yaşı olarak azalır. Yedeklemeler, bekletme süresinden daha eski olduktan sonra faturalandırılır. 


### <a name="monitoring-consumption"></a>Tüketim izleme

Her yedekleme türü (tam, fark ve günlük), veritabanı izleme dikey penceresinde ayrı bir ölçüm olarak raporlanır. Aşağıdaki diyagramda, yedeklemelerin depolama tüketiminin nasıl izleneceği gösterilmektedir.  

![Azure portal veritabanı izleme dikey penceresinde veritabanı yedekleme tüketimini izleme](media/sql-database-automated-backup/backup-metrics.png)

### <a name="fine-tune-the-backup-storage-consumption"></a>Yedekleme depolama tüketimine ince ayar yapma

Fazla yedekleme depolama alanı tüketimi, bireysel veritabanlarının iş yüküne ve boyutuna bağlıdır. Yedekleme depolama tüketiminizi daha fazla azaltmak için aşağıdaki ayarlama tekniklerinden bazılarını uygulamayı deneyebilirsiniz:

* [Yedekleme saklama süresini](#change-pitr-backup-retention-period-using-azure-portal) gereksinimleriniz için mümkün olan en düşük süreye düşürün.
* Gerektiğinde, dizin yeniden oluştur gibi büyük yazma işlemlerini gerekenden daha sık gerçekleştirmekten kaçının.
* Büyük veri yükleme işlemleri için, [kümelenmiş columnstore dizinleri](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes)kullanmayı düşünün, kümelenmemiş dizinlerin sayısını azaltır ve ayrıca 1.000.000 etrafında satır sayısıyla toplu yükleme işlemlerini de göz önünde bulundurun.
* Genel Amaçlı hizmet katmanında, sağlanan veri depolama alanı, sürekli olarak yüksek düzeyde yedekleme depolama maliyetlerine sahip olan müşterilerimiz, veri depolama alanının, yedekleme depolama alanı.
* Kalıcı tablolar yerine geçici sonuçları depolamak için, ETL mantığınızdaki TempDB kullanın (yalnızca yönetilen örnek için geçerlidir).
* Hassas veriler içermeyen (örneğin, geliştirme veya test veritabanları) veritabanları için TDE şifrelemeyi kapatmayı düşünün. Şifrelenmemiş veritabanları için yedeklemeler genellikle daha yüksek bir sıkıştırma oranıyla sıkıştırılır.

> [!IMPORTANT]
> Analitik, veri Reyonları \ veri ambarı iş yükleri için, [kümelenmiş columnstore dizinlerinin](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes)kullanılması önemle önerilir, kümelenmemiş dizinlerin sayısını azaltır ve ayrıca fazla yedekleme depolama kullanımını azaltmak için 1.000.000 etrafında satır sayısıyla toplu yükleme işlemlerini göz önünde bulundurun.


## <a name="storage-costs"></a>Depolama maliyetleri


### <a name="dtu-model"></a>DTU modeli

DTU modelini kullanan veritabanları ve elastik havuzlar için yedekleme depolaması için ek ücret alınmaz. 

### <a name="vcore-model"></a>Sanal çekirdek modeli

Tek veritabanları için, veritabanı boyutunun %100 ' ına eşit olan en düşük yedekleme depolama miktarı ek ücret ödemeden sunulmaktadır. Elastik havuzlar ve yönetilen örnekler için, havuz veya örnek boyutu için ayrılan veri depolamanın %100 ' ına eşit olan en düşük yedekleme depolama miktarı, ek ücret ödemeden sunulmaktadır. Ek yedekleme alanı kullanımı, GB/ay üzerinden ücretlendirilir. Bu ek tüketim, bireysel veritabanlarının iş yüküne ve boyutuna bağlı olarak değişir.

Azure SQL DB toplam bekletme yedekleme depolama alanınızı birikimli bir değer olarak hesaplacaktır. Bu değer, her ayın sonunda, bu saatlik kullanımı sağlamaktan sorumlu olan Azure Faturalandırma işlem hattına bildirilir. Veritabanı bırakıldıktan sonra, yedek yaş olarak tüketimi azaldık. Saklama süresinden daha eski olmaya başladıktan sonra faturalandırma işlemi duraklar. Tüm günlük yedeklemeleri ve değişiklik yedeklemeleri tam saklama süresi boyunca korunduğu için, yoğun olarak değiştirilen veritabanlarının yedekleme ücretleri daha yüksektir. 

Veritabanında 744 GB yedekleme depolama alanı olduğunu ve bu miktarın tamamı bir ay boyunca sabit kalır. Bu toplu depolama tüketimini saatlik kullanım için dönüştürmek üzere, bunu 744,0 (ayda 31 gün * günde 24 saat) böleceğiz. Bu nedenle, SQL DB veritabanını her saat 1 GB 'lık yedekleme kullanacak şekilde bildirir. Azure Faturalandırma bunu toplar ve bölgenizdeki $/GB/ay hızına bağlı olarak tüm ayın 744 GB kullanımını ve maliyeti gösterir. 

Şimdi daha karmaşık bir örnek. Veritabanının, bekletmenin Ayın ortasında 14 güne yükseldiğini ve bu (hypothetically) Toplam yedekleme depolama alanının 1488 GB 'ye kattığı hakkında bir sonuç olduğunu varsayalım. SQL DB, 1-372 saatleri için 1 GB kullanım raporlar ve sonra da kullanımı saat 373-744 için 2 GB olarak bildirir. Bu, son 1116 GB/ay olan bir fatura olarak toplanır. 

Azure abonelik maliyeti analizini, yedekleme depolamada geçerli harcamalarınızı öğrenmek için kullanabilirsiniz.

![Yedekleme depolama maliyeti Analizi](./media/sql-database-automated-backup/check-backup-storage-cost-sql-mi.png)

Örneğin, yönetilen örnek için yedekleme depolama maliyetlerini anlamak için lütfen Azure portal ' de aboneliğinize gidin ve maliyet analizi dikey penceresini açın. Geçerli yedekleme maliyetinizi ve ücretlendirmesini görmek için alt kategori **mı \ yedekleme depolama alanı** ' nı seçin. Ayrıca, yedekleme depolama maliyetini diğer maliyet kategorilerine göre karşılaştırmak için, **yönetilen örnek genel amaçlı depolama alanı** veya **yönetilen örnek genel amaçlı işlem 5. nesil** gibi diğer ölçüm alt kategorilerini da dahil edebilirsiniz.

## <a name="backup-retention"></a>Yedekleri bekletme

Tüm Azure SQL veritabanlarının (tek, havuza alınmış ve yönetilen örnek veritabanlarının) **yedi** günlük bir varsayılan yedekleme saklama süresi vardır. [Yedekleme saklama süresini 35 güne kadar değiştirebilirsiniz](#change-pitr-backup-retention-period).

Bir veritabanını silerseniz, SQL veritabanı yedeklemeleri çevrimiçi bir veritabanı ile aynı şekilde tutacaktır. Örneğin, bir saklama süresi yedi güne sahip olan temel bir veritabanını silerseniz, dört günden eski bir yedekleme üç gün boyunca kaydedilir.

Yedeklemeleri maksimum saklama süresinden daha uzun süre tutmanız gerekiyorsa, veritabanınıza bir veya daha fazla uzun süreli bekletme dönemi eklemek için yedekleme özelliklerini değiştirebilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

> [!IMPORTANT]
> SQL veritabanlarını barındıran Azure SQL Server 'ı silerseniz, sunucuya ait olan tüm elastik havuzlar ve veritabanları da silinir ve kurtarılamaz. Silinen bir sunucuyu geri yükleyemezsiniz. Ancak uzun süreli saklama yapılandırdıysanız, LTR içeren veritabanlarının yedeklemeleri silinmez ve bu veritabanları geri yüklenebilir.

## <a name="encrypted-backups"></a>Şifrelenmiş yedeklemeler

Veritabanınız TDE ile şifrelenirse, yedeklemeler, LTR yedeklemeler de dahil olmak üzere Rest 'de otomatik olarak şifrelenir. TDE bir Azure SQL veritabanı için etkinleştirildiğinde yedeklemeler de şifrelenir. Tüm yeni Azure SQL veritabanları, varsayılan olarak TDE etkin ile yapılandırılır. TDE hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı ile saydam veri şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="backup-integrity"></a>Yedekleme bütünlüğü

Azure SQL veritabanı Mühendisliği ekibi, sürekli olarak mantıksal sunuculara ve elastik havuzlara yerleştirilmiş veritabanlarının otomatik veritabanı yedeklerinin geri yüklenmesini otomatik olarak sınar (yönetilen örnekte kullanılamaz). Bir noktadan sonra geri yükleme sonrasında veritabanları DBCC CHECKDB kullanarak bütünlük denetimleri de alır.

Yönetilen örnek, geçiş tamamlandıktan sonra yerel `RESTORE` komutu veya veri geçiş hizmeti kullanılarak geri yüklenen veritabanlarının `CHECKSUM` otomatik olarak ilk yedeklemesini alır.

Bütünlük denetimi sırasında bulunan tüm sorunlar, mühendislik ekibine bir uyarıya neden olur. Azure SQL veritabanı 'nda veri bütünlüğü hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı 'Nda veri bütünlüğü](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/).

## <a name="compliance"></a>Uyumluluk

Veritabanınızı DTU tabanlı bir hizmet katmanından sanal çekirdek tabanlı bir hizmet katmanına geçirdiğinizde, uygulamanızın veri kurtarma ilkesinin güvenliğinin aşılmadığından emin olmak için, sür saklama korunur. Varsayılan saklama, uyumluluk gereksinimlerinizi karşılamıyorsa, PowerShell veya REST API kullanarak elde tutma süresini değiştirebilirsiniz. Daha fazla bilgi için bkz. [yedekleme saklama süresini değiştirme](#change-pitr-backup-retention-period).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="change-pitr-backup-retention-period"></a>Yedek saklama süresini değiştirme

Varsayılan yedek saklama süresini Azure portal, PowerShell veya REST API kullanarak değiştirebilirsiniz. Aşağıdaki örneklerde, fr bekletmenin 28 güne nasıl değiştirileceği gösterilmektedir.

> [!WARNING]
> Geçerli saklama süresini azaldıysanız, yeni saklama süresinden eski olan tüm mevcut yedeklemeler artık kullanılamaz. Geçerli saklama süresini artırırsanız, daha uzun süreli saklama süresine ulaşılana kadar SQL veritabanı mevcut yedekleri tutacaktır.

> [!NOTE]
> Bu API 'Ler yalnızca sür saklama süresini etkiler. Veritabanınız için LTR yapılandırdıysanız, bu, etkilenmeyecektir. LTR bekletme süresinin nasıl değiştirileceği hakkında daha fazla bilgi için bkz. [uzun süreli saklama](sql-database-long-term-retention.md).

### <a name="change-pitr-backup-retention-period-using-azure-portal"></a>Azure portal kullanarak yedek saklama süresini değiştirme

Azure portal kullanarak yedek saklama süresini değiştirmek için, saklama süresini portalda değiştirmek istediğiniz sunucu nesnesine gidin ve ardından değiştirmekte olduğunuz sunucu nesnesine göre uygun seçeneği belirleyin.

#### <a name="single-database--elastic-poolstabsingle-database"></a>[Tek veritabanı & elastik havuzlar](#tab/single-database)

Tek Azure SQL veritabanı için yedek saklama bekletme değişikliği, sunucu düzeyinde gerçekleştirilir. Sunucu düzeyinde yapılan değişiklik, bu sunucudaki veritabanları için geçerlidir. Azure SQL veritabanı sunucusu için veri Azure portal ' dan değiştirmek için, sunucuya genel bakış dikey penceresine gidin, gezinti menüsünde Yedeklemeleri Yönet ' e tıklayın ve ardından Gezinti çubuğunda bekletme yapılandırması ' na tıklayın.

![Azure portal Değiştir](./media/sql-database-automated-backup/configure-backup-retention-sqldb.png)

#### <a name="managed-instancetabmanaged-instance"></a>[Yönetilen örnek](#tab/managed-instance)

SQL veritabanı yönetilen örneği için ara yedek saklama 'nın değiştirilmesi, tek bir veritabanı düzeyinde gerçekleştirilir. Azure portal bir örnek veritabanının ara veritabanı yedekleme bekletmesini değiştirmek için, tek tek veritabanına genel bakış dikey penceresine gidin ve ardından Gezinti çubuğunda yedekleme bekletmesini yapılandır seçeneğine tıklayın.

![Azure portal Değiştir](./media/sql-database-automated-backup/configure-backup-retention-sqlmi.png)

---

### <a name="change-pitr-backup-retention-period-using-powershell"></a>PowerShell kullanarak yedek saklama süresini değiştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

```powershell
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

### <a name="change-pitr-retention-period-using-rest-api"></a>REST API kullanarak elde tutma süresini değiştirme

#### <a name="sample-request"></a>Örnek İsteği

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```

#### <a name="request-body"></a>İstek Gövdesi

```json
{
  "properties":{
    "retentionDays":28
  }
}
```

#### <a name="sample-response"></a>Örnek Yanıtı

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

Daha fazla bilgi için bkz. [yedekleme bekletme REST API](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies).

## <a name="next-steps"></a>Sonraki adımlar

- Veritabanı yedeklemeleri, verilerinizin yanlışlıkla bozulma veya silme işlemlerini korumasından dolayı, herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır. Diğer Azure SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için bkz. [iş sürekliliği genel bakış](sql-database-business-continuity.md).
- Azure portal kullanarak bir noktaya geri yüklemek için, bkz. [Azure Portal kullanarak veritabanını zaman noktasına geri yükleme](sql-database-recovery-using-backups.md).
- PowerShell 'i kullanarak zaman noktasına geri yüklemek için bkz. [PowerShell kullanarak veritabanını zaman noktasına geri yükleme](scripts/sql-database-restore-database-powershell.md).
- Azure Blob depolama alanında Azure portal kullanarak otomatik yedeklemelerin uzun süreli bekletmesini yapılandırmak, yönetmek ve geri yüklemek için, bkz. [Azure Portal kullanarak uzun süreli yedek saklama 'Yı yönetme](sql-database-long-term-backup-retention-configure.md).
- PowerShell kullanarak Azure Blob Storage 'da otomatik yedeklemelerin uzun süreli bekletmesini yapılandırmak, yönetmek ve geri yüklemek için, bkz. [PowerShell kullanarak uzun süreli yedekleme bekletmeyi yönetme](sql-database-long-term-backup-retention-configure.md).
