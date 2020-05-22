---
title: Otomatik, coğrafi olarak yedekli yedeklemeler
description: SQL veritabanı, her birkaç dakikada bir yerel veritabanı yedeklemesi oluşturur ve coğrafi yedeklilik için Azure Okuma Erişimli Coğrafi olarak yedekli depolama kullanır.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab, danil
manager: craigg
ms.date: 12/13/2019
ms.openlocfilehash: 7858d626282b3090e6c95af9c06b3a0a7e603462
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773591"
---
# <a name="automated-backups"></a>Otomatik yedeklemeler

Azure SQL veritabanı, yapılandırılan Bekletme dönemi boyunca tutulan veritabanı yedeklerini otomatik olarak oluşturur. Veri merkezi kullanılamasa bile yedeklemelerin korunacağından emin olmak için Azure [Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS)](../storage/common/storage-redundancy.md) kullanır.

Veritabanı yedeklemeleri, verilerinizin yanlışlıkla bozulma veya silme işlemlerini korumasından dolayı, herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır. Güvenlik kurallarınız, yedeklemelerinizin genişletilmiş bir süre (en fazla 10 yıla kadar) kullanılmasını gerektiriyorsa, tek veritabanları ve elastik veritabanı havuzlarında [uzun süreli saklama](sql-database-long-term-retention.md) yapılandırabilirsiniz.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>SQL veritabanı yedeklemesi nedir?

SQL veritabanı her hafta [tam yedeklemeler](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server) oluşturmak için SQL Server teknolojisini kullanır, her 12 saatte bir [fark yedeklemeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) ve [işlem günlüğü yedeklemeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) 5 ila 10 dakika sürer. Yedeklemeler, veri merkezi kesintisine karşı koruma için [eşleştirilmiş bir veri merkezine](../best-practices-availability-paired-regions.md) çoğaltılan [RA-GRS depolama Blobları](../storage/common/storage-redundancy.md) 'nda depolanır. Bir veritabanını geri yüklediğinizde, hizmet hangi tam, fark ve işlem günlüğü yedeklemelerinin geri yüklenmesi gerektiğini belirler.

Bu yedeklemeleri kullanarak şunları yapabilirsiniz:

- Mevcut bir veritabanını, Azure portal, Azure PowerShell, Azure CLı veya REST API kullanarak saklama süresi içinde **geçmiş bir noktaya geri yükleyin** . Tek veritabanları ve elastik veritabanı havuzları için bu işlem, özgün veritabanıyla aynı sunucuda yeni bir veritabanı oluşturur. Yönetilen örnekte bu işlem, aynı abonelik kapsamında veritabanının bir kopyasını veya aynı ya da farklı bir yönetilen örneği oluşturabilir.
- **Silinen bir veritabanını silme zamanına** veya Bekletme dönemi içinde herhangi bir zamanda geri yükleyin. Silinen veritabanı yalnızca özgün veritabanının oluşturulduğu mantıksal sunucuya veya yönetilen örneğe geri yüklenebilir.
- **Veritabanını başka bir coğrafi bölgeye geri yükleyin**. Coğrafi geri yükleme, sunucunuza ve veritabanınıza erişene zaman coğrafi bir olağanüstü durumdan kurtulmanızı sağlar. Bu, dünyanın herhangi bir yerindeki mevcut herhangi bir sunucuda yeni bir veritabanı oluşturur.
- Veritabanı uzun süreli bir bekletme ilkesiyle yapılandırılmışsa (LTR), tek bir veritabanı veya elastik veritabanı havuzundaki **belirli bir uzun süreli yedekten bir veritabanını geri yükleyin** . LTR, bir uyumluluk isteğini karşılamak veya uygulamanın eski bir sürümünü çalıştırmak için [Azure Portal](sql-database-long-term-backup-retention-configure.md#using-azure-portal) veya [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#using-powershell) kullanarak veritabanının eski bir sürümünü geri yüklemenize olanak tanır. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

Geri yükleme gerçekleştirmek için bkz. [veritabanlarını yedeklerden geri yükleme](sql-database-recovery-using-backups.md).

> [!NOTE]
> Azure depolama 'da, *çoğaltma* terimi dosyaları bir konumdan diğerine kopyalamayı belirtir. SQL Server *veritabanı çoğaltması* , birden çok ikincil veritabanının birincil veritabanıyla eşitlenmiş kalmasını sağlar.

Aşağıdaki örnekleri kullanarak bu işlemlerden bazılarını deneyebilirsiniz:

| | Azure portal | Azure PowerShell |
|---|---|---|
| Yedekleme bekletmesini değiştirme | [Tek veritabanı](sql-database-automated-backups.md?tabs=managed-instance#change-the-pitr-backup-retention-period-by-using-the-azure-portal) <br/> [Yönetilen örnek](sql-database-automated-backups.md?tabs=managed-instance#change-the-pitr-backup-retention-period-by-using-the-azure-portal) | [Tek veritabanı](sql-database-automated-backups.md#change-the-pitr-backup-retention-period-by-using-powershell) <br/>[Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasebackupshorttermretentionpolicy) |
| Uzun süreli yedekleme bekletmesini değiştirme | [Tek veritabanı](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies)<br/>Yönetilen örnek-yok  | [Tek veritabanı](sql-database-long-term-backup-retention-configure.md)<br/>Yönetilen örnek-yok  |
| Bir veritabanından bir zaman noktasından geri yükleme | [Tek veritabanı](sql-database-recovery-using-backups.md#point-in-time-restore) | [Tek veritabanı](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) <br/> [Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase) |
| Silinen veritabanını geri yükleme | [Tek veritabanı](sql-database-recovery-using-backups.md) | [Tek veritabanı](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) <br/> [Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeletedinstancedatabasebackup)|
| Azure Blob depolamadan bir veritabanını geri yükleme | Tek veritabanı-yok <br/>Yönetilen örnek-yok  | Tek veritabanı-yok <br/>[Yönetilen örnek](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) |


## <a name="backup-frequency"></a>Yedekleme sıklığı

### <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

SQL veritabanı, otomatik olarak tam yedeklemeler, fark yedeklemeleri ve işlem günlüğü yedeklemeleri oluşturarak, zaman içinde nokta geri yükleme (ıNR) için self servis hizmetini destekler. Tam veritabanı yedeklemeleri haftalık olarak oluşturulur ve fark veritabanı yedeklemeleri genellikle her 12 saatte bir oluşturulur. İşlem günlüğü yedeklemeleri genellikle her 5 ila 10 dakika oluşturulur. İşlem günlüğü yedeklemelerinin sıklığı, işlem boyutuna ve veritabanı etkinliğinin miktarına göre belirlenir. 

İlk tam yedekleme, bir veritabanı oluşturulduktan hemen sonra zamanlanır. Bu yedekleme genellikle 30 dakika içinde tamamlanır, ancak veritabanı büyükse daha uzun sürebilir. Örneğin, ilk yedekleme geri yüklenen bir veritabanında veya bir veritabanı kopyasında daha uzun sürebilir. İlk tam yedeklemeden sonra diğer tüm yedeklemeler otomatik olarak zamanlanır ve arka planda sessizce yönetilir. Tüm veritabanı yedeklemelerinin tam zamanlaması, genel sistem iş yükünü dengelediği için SQL Veritabanı hizmeti tarafından belirlenir. Yedekleme işlerini değiştiremez veya devre dışı bırakamazsınız.

### <a name="default-backup-retention-period"></a>Varsayılan yedekleme saklama süresi

INR yedeklemeleri, coğrafi olarak yedekli depolama ile korunur. Daha fazla bilgi için bkz. [Azure Depolama yedekliliği](../storage/common/storage-redundancy.md).

SÜR hakkında daha fazla bilgi için bkz. [Noktadan noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore).

### <a name="long-term-retention"></a>Uzun vadeli bekletme

Tek ve havuza alınmış veritabanları için, Azure Blob depolamada 10 yıla kadar tam yedeklemelerin uzun süreli bekletmesini (LTR) yapılandırabilirsiniz. LTR ilkesini etkinleştirirseniz haftalık tam yedeklemeler otomatik olarak farklı bir RA-GRS depolama kapsayıcısına kopyalanır. Çeşitli uyumluluk gereksinimlerini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı saklama süreleri seçebilirsiniz. Depolama tüketimi, seçilen yedekleme sıklığına ve Bekletme dönemi ya da dönemler temelinde değişir. LTR depolama maliyetini tahmin etmek için [LTR Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/?service=sql-database) ' yı kullanabilirsiniz.

Invr yedeklemeleri gibi, LTR yedeklemeler, coğrafi olarak yedekli depolama ile korunur. Daha fazla bilgi için bkz. [Azure Depolama yedekliliği](../storage/common/storage-redundancy.md).

LTR hakkında daha fazla bilgi için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md).

## <a name="backup-storage-consumption"></a>Yedekleme depolama alanı tüketimi

Tek veritabanları için bu denklem toplam yedekleme depolama kullanımını hesaplamak için kullanılır:

`Total backup storage size = (size of full backups + size of differential backups + size of log backups) – database size`

Esnek veritabanı havuzları için toplam yedekleme depolama boyutu havuz düzeyinde toplanır ve şu şekilde hesaplanır:

`Total backup storage size = (total size of all full backups + total size of all differential backups + total size of all log backups) - allocated pool data storage`

Saklama süresinden önce gerçekleşen yedeklemeler, zaman damgalarına göre otomatik olarak temizlenir. Fark yedeklemeleri ve günlük yedeklemeleri, daha önce bir tam yedeklemenin yararlı olmasını gerektirdiğinden, bunlar haftalık öbeklerde birlikte temizlenir.

Azure SQL veritabanı toplam bekletme yedekleme depolama alanınızı birikimli bir değer olarak hesaplar. Her saat, bu değer Azure Faturalandırma işlem hattında raporlanır ve bu saatlik kullanımı, her ayın sonunda tüketiminizi hesaplamak için sağlamaktan sorumludur. Veritabanı bırakıldıktan sonra, tüketim yedekleme yaşı olarak azalır. Yedeklemeler, bekletme süresinden daha eski olduktan sonra faturalandırılır.

   > [!IMPORTANT]
   > Veritabanı bırakılmış olsa bile, bir veritabanının yedeklemeleri belirtilen bekletme süresi boyunca tutulur. Bir veritabanını bırakma ve yeniden oluşturma işlemi genellikle depolama ve işlem maliyetlerinde tasarruf edilirken, Microsoft, her bırakıldığında bırakılan her veritabanı için belirtilen saklama süresi için bir yedek sakladığı için yedekleme depolama maliyetlerini artırabilir.

### <a name="monitor-consumption"></a>Tüketimi izleme

Her yedekleme türü (tam, değişiklik ve günlük), veritabanı izleme dikey penceresinde ayrı bir ölçüm olarak raporlanır. Aşağıdaki diyagramda tek bir veritabanı için yedekleme depolama tüketiminin nasıl izleneceği gösterilmektedir. Bu özellik şu anda yönetilen örnekler için kullanılamıyor.

![Azure portal veritabanı yedeklemesi kullanımını izleme](media/sql-database-automated-backup/backup-metrics.png)

### <a name="fine-tune-backup-storage-consumption"></a>Yedekleme depolama tüketimine ince ayar yapma

Aşırı yedekleme depolama alanı tüketimi, iş yüküne ve ayrı veritabanlarının boyutuna bağlı olarak değişir. Yedekleme depolama tüketiminizi azaltmak için aşağıdaki ayarlama tekniklerinden bazılarını göz önünde bulundurun:

* [Yedekleme saklama süresini](#change-the-pitr-backup-retention-period-by-using-the-azure-portal) gereksinimleriniz için mümkün olan en düşük süreye düşürün.
* Dizin yeniden oluşturmanız gibi büyük yazma işlemlerini yapmaktan kaçının, ancak gerekenden daha sık.
* Büyük veri yükleme işlemleri için, [kümelenmiş columnstore dizinleri](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes)kullanmayı düşünün, kümelenmemiş dizinlerin sayısını azaltır ve 1.000.000 etrafında satır sayısıyla toplu yükleme işlemlerini göz önünde bulundurun.
* Genel amaçlı hizmet katmanında, sağlanan veri depolama alanı fazla yedekleme depolama fiyatından daha ucuz. Sürekli yedekleme depolama maliyetleriniz varsa, yedekleme depolama alanı üzerinde kaydedilecek veri depolama alanını artırmayı düşünebilirsiniz.
* Geçici sonuçları depolamak için ETL mantığınızdaki kalıcı tablolar yerine TempDB kullanın. (Yalnızca yönetilen örnek için geçerlidir.)
* Gizli veriler (örneğin, geliştirme veya test veritabanları) içermeyen veritabanları için TDE şifrelemeyi kapatmayı göz önünde bulundurun. Şifrelenmemiş veritabanları için yedeklemeler genellikle daha yüksek bir sıkıştırma oranıyla sıkıştırılır.

> [!IMPORTANT]
> Analitik veri reyonu \ veri ambarı iş yükleri için, [kümelenmiş columnstore dizinleri](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes)kullanmanızı, kümelenmemiş dizinlerin sayısını azaltmanızı ve 1.000.000 etrafında satır sayısıyla toplu yükleme işlemlerini göz önünde bulundurmanız önerilir.

## <a name="storage-costs"></a>Depolama maliyetleri

Depolama fiyatı, DTU modelini mi yoksa vCore modelini mi kullandığınıza bağlı olarak farklılık gösterir.

### <a name="dtu-model"></a>DTU modeli

DTU modelini kullanıyorsanız veritabanları ve elastik veritabanı havuzları için yedekleme depolaması için ek ücret alınmaz.

### <a name="vcore-model"></a>Sanal çekirdek modeli

Tek veritabanları için, veritabanı boyutunun yüzde 100 ' una eşit olan minimum yedekleme depolama miktarı ek bir ücret ödemeden sağlanır. Elastik veritabanı havuzları ve yönetilen örnekler için, havuz veya örnek boyutu için ayrılan veri depolamanın yüzde 100 ' una eşit bir yedekleme depolama miktarı, ek ücret ödemeden sunulmaktadır. Ek yedekleme alanı kullanımı aylık GB olarak ücretlendirilir. Bu ek tüketim, bireysel veritabanlarının iş yüküne ve boyutuna bağlı olarak değişir.

Azure SQL veritabanı, toplam bekletme yedekleme depolama alanınızı birikimli bir değer olarak hesaplacaktır. Bu değer, her saat için bu saatlik kullanımı sağlamaktan sorumlu olan Azure Faturalandırma işlem hattına bildirilir. Veritabanı bırakıldıktan sonra, Microsoft bu tüketimi yedeklemeler yaşı olarak düşürür. Yedeklemeler, bekletme süresinden daha eski olduktan sonra faturalandırılır. Tüm günlük yedeklemeleri ve değişiklik yedeklemeleri tam saklama süresi boyunca korunduğundan, yoğun olarak değiştirilen veritabanları daha yüksek yedekleme ücretlerine sahip olur.

Bir veritabanının 744 GB 'lık yedekleme depolama alanı olduğunu ve bu miktarın tüm bir ay boyunca sabit kalmasını varsayalım. Bu toplu depolama tüketimini saatlik kullanıma dönüştürmek için, 744,0 (ayda 31 gün * günde 24 saat) ayırın. Bu nedenle SQL veritabanı, veritabanının her saat 1 GB 'lık yedekleme tükettiğini rapor edecektir. Azure Faturalandırma, bu tüketimi toplar ve tüm ay için 744 GB kullanımını gösterir. Maliyet, bölgenizdeki $/GB/aya oranı temel alır.

Şimdi daha karmaşık bir örnek. Veritabanının bekletmenin, Ayın ortasında 14 güne kadar arttığını varsayalım. Bu artışın (hypothetically) Toplam yedekleme depolama alanının 1.488 GB 'a katdığını varsayın. SQL veritabanı 1 ila 372 saat boyunca 1 GB kullanım rapor verebilir. Kullanım, 373 ile 744 arasındaki saatler için 2 GB olarak rapor edecektir. Bu kullanım, son 1.116 GB/ay faturalandırılmakta toplanacak.

### <a name="monitor-costs"></a>Maliyetleri izleme

Yedekleme depolama maliyetlerini anlamak için Azure portal **maliyet yönetimi + faturalandırma** ' e gidin, **maliyet yönetimi**' ni seçin ve ardından **Maliyet Analizi**' ni seçin. **Kapsam**olarak istenen aboneliği seçin ve ilgilendiğiniz zaman aralığı ve hizmet için filtre uygulayın.

**Hizmet adı**için bir filtre ekleyin ve ardından açılan listede **SQL veritabanı** ' nı seçin. Hizmetiniz için faturalandırma sayacını seçmek üzere **ölçüm alt kategori** filtresini kullanın. Tek bir veritabanı veya elastik veritabanı havuzu için, **tek/elastik havuz yedek depolama**' yı seçin. Yönetilen bir örnek için mı. **yedekleme depolama alanı**' nı seçin. **Depolama** ve **işlem** alt kategorileri sizi ilgilendirir, ancak yedekleme depolama maliyetleriyle ilişkili değildir.

![Yedekleme depolama maliyeti Analizi](./media/sql-database-automated-backup/check-backup-storage-cost-sql-mi.png)

## <a name="backup-retention"></a>Yedekleri bekletme

Tüm Azure SQL veritabanlarının (tek, havuza alınmış ve yönetilen örnek veritabanlarının) varsayılan yedekleme saklama süresi 7 gündür. [Yedekleme saklama süresini](#change-the-pitr-backup-retention-period) 35 gün olarak değiştirebilirsiniz.

Bir veritabanını silerseniz, SQL veritabanı yedeklemeleri çevrimiçi bir veritabanı ile aynı şekilde tutacaktır. Örneğin, bir saklama süresi yedi güne sahip olan temel bir veritabanını silerseniz, dört gün öncesine ait bir yedek üç gün boyunca kaydedilir.

Yedeklemeleri maksimum saklama süresinden daha uzun süre tutmanız gerekiyorsa, veritabanınıza bir veya daha fazla uzun süreli bekletme dönemi eklemek için yedekleme özelliklerini değiştirebilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

> [!IMPORTANT]
> SQL veritabanlarını barındıran Azure SQL Server 'ı silerseniz, sunucuya ait olan tüm elastik veritabanı havuzları ve veritabanları da silinir. Bunlar kurtarılamaz. Silinen bir sunucuyu geri alamazsınız. Ancak uzun süreli saklama yapılandırdıysanız, LTR ile veritabanlarının yedeklemeleri silinmez ve bu veritabanları geri yüklenebilir.

> [!NOTE]
> Azure portal aracılığıyla tek, havuza alınmış veritabanları ve yönetilen esnetler için yapılandırılabilen minimum yedek yedekleme saklama süresi 7 gündür. En az 1 günlük yedek saklama, yönetilen örnek için yalnızca as. SQL Module v 2.6.0 veya üzeri ile PowerShell kullanılarak yapılandırılabilir.

## <a name="encrypted-backups"></a>Şifrelenmiş yedeklemeler

Veritabanınız TDE ile şifrelenirse, yedeklemeler, LTR yedeklemeler de dahil olmak üzere Rest 'de otomatik olarak şifrelenir. TDE bir Azure SQL veritabanı için etkinleştirildiğinde yedeklemeler de şifrelenir. Tüm yeni Azure SQL veritabanları, varsayılan olarak TDE etkin ile yapılandırılır. TDE hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı ile saydam veri şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="backup-integrity"></a>Yedekleme bütünlüğü

Azure SQL veritabanı Mühendisliği ekibi, sürekli olarak mantıksal sunuculara ve esnek veritabanı havuzlarına yerleştirilmiş veritabanlarının otomatik veritabanı yedeklerinin geri yüklenmesini otomatik olarak sınar. (Bu test yönetilen örnekte kullanılamaz.) Bir noktadan sonra geri yükleme sonrasında veritabanları DBCC CHECKDB bütünlük denetimleri de alır.

Yönetilen örnek, `CHECKSUM` yerel `RESTORE` komutla veya geçiş tamamlandıktan sonra Azure Data Migration hizmeti ile geri yüklenen veritabanlarıyla otomatik olarak ilk yedeklemeyi alır.

Bütünlük denetimi sırasında bulunan tüm sorunlar, mühendislik ekibine bir uyarıya neden olur. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'Nda veri bütünlüğü](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/).

## <a name="compliance"></a>Uyumluluk

Veritabanınızı DTU tabanlı bir hizmet katmanından sanal çekirdek tabanlı bir hizmet katmanına geçirdiğinizde, uygulamanızın veri kurtarma ilkesinin tehlikeye atılmasını sağlamak için, sür saklama korunur. Varsayılan saklama, uyumluluk gereksinimlerinizi karşılamıyorsa, PowerShell veya REST API kullanarak, elde tutma süresini değiştirebilirsiniz. Daha fazla bilgi için bkz. [yedek saklama süresini değiştirme](#change-the-pitr-backup-retention-period).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-pitr-backup-retention-period"></a>Yedek yedekleme saklama süresini değiştirme

Varsayılan yedek saklama süresini Azure portal, PowerShell veya REST API kullanarak değiştirebilirsiniz. Aşağıdaki örneklerde, fr bekletmenin 28 güne nasıl değiştirileceği gösterilmektedir.

> [!WARNING]
> Geçerli saklama süresini azaldıysanız, yeni saklama süresinden daha eski olan tüm mevcut yedeklemeler artık kullanılamaz. Geçerli saklama süresini artırırsanız, daha uzun saklama süresine ulaşılana kadar SQL veritabanı mevcut yedekleri tutar.

> [!NOTE]
> Bu API 'Ler yalnızca sür saklama süresini etkiler. Veritabanınız için LTR yapılandırdıysanız, bu etkilenmez. LTR bekletme dönemlerini değiştirme hakkında daha fazla bilgi için bkz. [uzun süreli saklama](sql-database-long-term-retention.md).

### <a name="change-the-pitr-backup-retention-period-by-using-the-azure-portal"></a>Azure portal kullanarak, yedek yedekleme saklama süresini değiştirin

Azure portal kullanarak yedek saklama süresini değiştirmek için, saklama süresini portalda değiştirmek istediğiniz sunucu nesnesine gidin. Ardından, değiştirmekte olduğunuz sunucu nesnesine göre uygun seçeneği belirleyin.

#### <a name="single-database-and-elastic-database-pools"></a>[Tek veritabanı ve elastik veritabanı havuzları](#tab/single-database)

Tek bir Azure SQL veritabanı için yedek saklama durumunda yapılan değişiklikler sunucu düzeyinde yapılır. Sunucu düzeyinde yapılan değişiklikler sunucudaki veritabanları için geçerlidir. Azure portal bir Azure SQL veritabanı sunucusu için veri saklama bekletmeyi değiştirmek üzere sunucuya genel bakış dikey penceresine gidin. Sol bölmedeki **Yedeklemeleri Yönet** ' i seçin ve ardından ekranın en üstünde **bekletme Yapılandır** ' ı seçin:

![GIZLI tutma bekletme, sunucu düzeyi](./media/sql-database-automated-backup/configure-backup-retention-sqldb.png)

#### <a name="managed-instance"></a>[Yönetilen örnek](#tab/managed-instance)

SQL veritabanı yönetilen örnekleri için yedek saklama değişiklikleri, tek bir veritabanı düzeyinde yapılır. Azure portal bir örnek veritabanının ara veritabanı yedekleme bekletmesini değiştirmek için, tek tek veritabanına genel bakış dikey penceresine gidin. Ardından ekranın üst kısmındaki **yedekleme bekletmesini Yapılandır** ' ı seçin:

![INR saklama, yönetilen örneği Değiştir](./media/sql-database-automated-backup/configure-backup-retention-sqlmi.png)

---

### <a name="change-the-pitr-backup-retention-period-by-using-powershell"></a>PowerShell kullanarak yedek saklama süresini değiştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azurerd modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Daha fazla bilgi için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module içindeki komutlar için bağımsız değişkenler Azurerd modüllerindekilerle oldukça benzerdir.

```powershell
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

### <a name="change-the-pitr-backup-retention-period-by-using-the-rest-api"></a>REST API kullanarak, yedek yedekleme saklama süresini değiştirin

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

Daha fazla bilgi için bkz. [yedekleme bekletme REST API](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies).

## <a name="next-steps"></a>Sonraki adımlar

- Veritabanı yedeklemeleri, verilerinizin yanlışlıkla bozulma veya silme işlemlerini korumasından dolayı, herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır. Diğer Azure SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için bkz. [iş sürekliliği genel bakış](sql-database-business-continuity.md).
- [Azure Portal kullanarak bir veritabanını zaman noktasına geri yükleme](sql-database-recovery-using-backups.md)hakkında daha fazla bilgi alın.
- [PowerShell 'i kullanarak bir veritabanını zaman içinde bir noktaya geri yükleme](scripts/sql-database-restore-database-powershell.md)hakkında daha fazla bilgi alın.
- Azure Blob depolamada Azure portal kullanarak otomatik yedeklemelerin uzun süreli bekletmesini yapılandırma, yönetme ve geri yükleme hakkında daha fazla bilgi için, bkz. [Azure Portal kullanarak uzun süreli yedek saklama Işlemlerini yönetme](sql-database-long-term-backup-retention-configure.md).
- PowerShell kullanarak Azure Blob depolamada otomatik yedeklemelerin uzun süreli bekletmesini yapılandırma, yönetme ve geri yükleme hakkında daha fazla bilgi için bkz. [PowerShell kullanarak uzun süreli yedek saklama 'Yı yönetme](sql-database-long-term-backup-retention-configure.md).
