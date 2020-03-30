---
title: Yedeklemeden veritabanı nı geri yükleme
description: Azure SQL veritabanını 35 güne kadar geri almanızı sağlayan zamanında geri yükleme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab, danil
ms.date: 09/26/2019
ms.openlocfilehash: b98331a9cdb359aeefac5db1546f3a15b54010ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268749"
---
# <a name="recover-an-azure-sql-database-by-using-automated-database-backups"></a>Otomatik veritabanı yedeklemelerini kullanarak bir Azure SQL veritabanını kurtarma

Varsayılan olarak, Azure SQL Veritabanı yedeklemeleri coğrafi olarak çoğaltılmış blob depolamaalanında (RA-GRS depolama türü) depolanır. Aşağıdaki [seçenekler, otomatik veritabanı yedeklemeleri](sql-database-automated-backups.md)kullanılarak veritabanı kurtarma için kullanılabilir. Şunları yapabilirsiniz:

- Bekletme süresi içinde belirli bir noktaya kurtarılan, aynı SQL Veritabanı sunucusunda yeni bir veritabanı oluşturun.
- Silinen bir veritabanı için silme süresine kadar kurtarılan, aynı SQL Veritabanı sunucusunda bir veritabanı oluşturun.
- Aynı bölgedeki herhangi bir SQL Veritabanı sunucusunda, en son yedeklemeler noktasına kadar kurtarılan yeni bir veritabanı oluşturun.
- Başka bir bölgedeki herhangi bir SQL Veritabanı sunucusunda, en son çoğaltılan yedeklemeler noktasına kadar kurtarılmış yeni bir veritabanı oluşturun.

[Yedeklemeuzun uzun süreli bekletme](sql-database-long-term-retention.md)yapılandırılmışsa, herhangi bir SQL Veritabanı sunucusunda uzun vadeli bekletme yedeklemesinden yeni bir veritabanı da oluşturabilirsiniz.

> [!IMPORTANT]
> Geri yükleme sırasında varolan bir veritabanının üzerine yazamazsınız.

Standart veya Premium hizmet katmanlarını kullanırken, veritabanı geri yüklemeniz ek bir depolama maliyetine neden olabilir. Geri yüklenen veritabanının maksimum boyutu, hedef veritabanının hizmet katmanı ve performans düzeyiyle birlikte verilen depolama miktarından büyük olduğunda ek maliyet tahakkuk eder. Ek depolama nın fiyatlandırma ayrıntıları için [SQL Veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/)bakın. Kullanılan alanın gerçek miktarı dahil edilen depolama miktarından daha azsa, en yüksek veritabanı boyutunu dahil edilen tutara ayarlayarak bu ek maliyeti önleyebilirsiniz.

## <a name="recovery-time"></a>Kurtarma süresi

Otomatik veritabanı yedeklemeleri kullanarak bir veritabanını geri yüklemek için kurtarma süresi çeşitli etkenlerden etkilenir:

- Veritabanının boyutu.
- Veritabanının işlem boyutu.
- İlgili işlem günlüklerinin sayısı.
- Geri yükleme noktasına kurtarmak için yeniden çalınması gereken etkinlik miktarı.
- Geri yükleme farklı bir bölgeye yse ağ bant genişliği.
- Hedef bölgede işlenen eşzamanlı geri yükleme isteklerinin sayısı.

Büyük veya çok etkin bir veritabanı için geri yükleme birkaç saat sürebilir. Bir bölgede uzun süreli bir kesinti varsa, olağanüstü durum kurtarma için çok sayıda coğrafi geri yükleme isteği başlatılması mümkündür. Çok sayıda istek olduğunda, tek tek veritabanları için kurtarma süresi artabilir. Çoğu veritabanı 12 saatten kısa sürede tamamlar.

Tek bir abonelik için, eşzamanlı geri yükleme isteklerinin sayısında sınırlamalar vardır. Bu sınırlamalar, zaman içinde geri yüklemelerin, coğrafi geri yüklemelerin ve uzun süreli bekletme yedeklemesinin herhangi bir birleşimi için geçerlidir.

| | **Eşzamanlı isteklerin Max # işleniyor** | **Gönderilen eşzamanlı isteklerin Max #** |
| :--- | --: | --: |
|Tek veritabanı (abonelik başına)|10|60|
|Elastik havuz (havuz başına)|4|200|
||||

Tüm sunucuyu geri yüklemek için yerleşik bir yöntem yoktur. Bu görevi nasıl başaracağına yürütülecek bir çağrıda, [Azure SQL Database: Full Server Recovery](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666)çalıştı.

> [!IMPORTANT]
> Otomatik yedeklemeler kullanarak kurtarmak için, abonelikteki SQL Server katılımcısı rolünün bir üyesi veya abonelik sahibi olmalısınız. Daha fazla bilgi için Bkz. [RBAC: Yerleşik roller.](../role-based-access-control/built-in-roles.md) Azure portalını, PowerShell'i veya REST API'sini kullanarak kurtarabilirsiniz. Transact-SQL kullanamazsınız.

## <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

Azure portalını, [PowerShell'i](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase)veya [REST API'yi](https://docs.microsoft.com/rest/api/sql/databases)kullanarak bağımsız, havuzlu veya örnek veritabanını daha önceki bir noktaya geri yükleyebilirsiniz. İstek, geri yüklenen veritabanı için herhangi bir hizmet katmanı veya bilgi işlem boyutu belirtebilir. Veritabanını geri aldığınız sunucuda yeterli kaynağa sahip olduğundan emin olun. Tamamlandığında, geri yükleme özgün veritabanı yla aynı sunucuda yeni bir veritabanı oluşturur. Geri yüklenen veritabanı, hizmet katmanına ve işlem boyutuna bağlı olarak normal ücretlerle ücretlendirilir. Veritabanı geri yüklemesi tamamlanana kadar ücretödemezsiniz.

Genellikle kurtarma amacıyla daha önceki bir noktaya bir veritabanı geri yükleyin. Geri yüklenen veritabanını özgün veritabanının yerine işleyebilir veya özgün veritabanını güncelleştirmek için veri kaynağı olarak kullanabilirsiniz.

- **Veritabanı değiştirme**

  Geri yüklenen veritabanının özgün veritabanının yerine olmasını istiyorsanız, özgün veritabanının bilgi işlem boyutunu ve hizmet katmanını belirtmeniz gerekir. Daha sonra özgün veritabanını yeniden adlandırabilir ve T-SQL'deki [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) komutunu kullanarak geri yüklenen veritabanına orijinal adı verebilirsiniz.

- **Veri kurtarma**

  Bir kullanıcı veya uygulama hatasıkurtarmak için geri yüklenen veritabanından veri almayı planlıyorsanız, geri yüklenen veritabanından veri ayıklayan ve özgün veritabanına uygulanan bir veri kurtarma komut dosyası yazmanız ve yürütmeniz gerekir. Geri yükleme işleminin tamamlanması uzun zaman alsa da, geri yükleme işlemi boyunca veritabanı listesinde görünür. Geri yükleme sırasında veritabanını silerseniz, geri yükleme işlemi iptal edilir ve geri yüklemeyi tamamlamayan veritabanı için ücretlendirilmezsiniz.
  
### <a name="point-in-time-restore-by-using-azure-portal"></a>Azure portalLarını kullanarak zamanında geri yükleme

Azure portalında geri yüklemek istediğiniz veritabanının genel bakış bıçağından tek bir SQL veritabanını veya örnek veritabanını zamanında kurtarabilirsiniz.

#### <a name="single-azure-sql-database"></a>Tek Azure SQL veritabanı

Azure portalını kullanarak tek veya havuza alınan bir veritabanını zamanında kurtarmak için veritabanına genel bakış sayfasını açın ve araç çubuğunda **Geri Yükle'yi** seçin. Yedekleme kaynağını seçin ve yeni bir veritabanının oluşturulacağı zaman içinde yedekleme noktasını seçin. 

  ![Veritabanı geri yükleme seçeneklerinin ekran görüntüsü](./media/sql-database-recovery-using-backups/pitr-backup-sql-database-annotated.png)

#### <a name="managed-instance-database"></a>Yönetilen örnek veritabanı

Azure portalını kullanarak yönetilen örnek veritabanını zamanında kurtarmak için veritabanına genel bakış sayfasını açın ve araç çubuğunda **Geri Yükle'yi** seçin. Yeni bir veritabanının oluşturulacağı zaman içinde yedekleme noktasını seçin. 

  ![Veritabanı geri yükleme seçeneklerinin ekran görüntüsü](./media/sql-database-recovery-using-backups/pitr-backup-managed-instance-annotated.png)

> [!TIP]
> Bir yedeklemeden bir veritabanını programlı olarak geri yüklemek için, [otomatik yedeklemeleri kullanarak programmatically kurtarma gerçekleştirme'ye](sql-database-recovery-using-backups.md)bakın.

## <a name="deleted-database-restore"></a>Silinen veritabanı geri yükleme

Silinmiş bir veritabanını silme süresine veya aynı SQL Veritabanı sunucusunda veya aynı yönetilen örnekte zaman içinde daha önceki bir noktaya geri yükleyebilirsiniz. Bunu Azure portalı, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase)veya REST [(createMode=Restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)aracılığıyla gerçekleştirebilirsiniz. Yedeklemeden yeni bir veritabanı oluşturarak silinmiş bir veritabanını geri yükleyebilirsiniz.

> [!IMPORTANT]
> Bir Azure SQL Veritabanı sunucusunu veya yönetilen örneği silerseniz, tüm veritabanları da silinir ve kurtarılamaz. Silinen bir sunucuveya yönetilen örneği geri yükleyemezsiniz.

### <a name="deleted-database-restore-by-using-the-azure-portal"></a>Azure portalını kullanarak silinen veritabanı geri yüklemesi

Azure portalından silinen veritabanlarını sunucu ve örnek kaynağından geri yüklersiniz.

#### <a name="single-azure-sql-database"></a>Tek Azure SQL veritabanı

Azure portalını kullanarak tek veya havuza girilen silinmiş bir veritabanını silme süresine kurtarmak için sunucuya genel bakış sayfasını açın ve **Silinmiş veritabanlarını**seçin. Geri yüklemek istediğiniz silinmiş bir veritabanı seçin ve yedeklemeden geri yüklenen verilerle oluşturulacak yeni veritabanının adını yazın.

  ![Silinen Azure SQL veritabanını geri yükleme ekran görüntüsü](./media/sql-database-recovery-using-backups/restore-deleted-sql-database-annotated.png)

#### <a name="managed-instance-database"></a>Yönetilen örnek veritabanı

Azure portalını kullanarak yönetilen bir veritabanını kurtarmak için yönetilen örneğe genel bakış sayfasını açın ve **Silinmiş veritabanlarını**seçin. Geri yüklemek istediğiniz silinmiş bir veritabanı seçin ve yedeklemeden geri yüklenen verilerle oluşturulacak yeni veritabanının adını yazın.

  ![Silinen Azure SQL örnek veritabanını geri yükleme ekran görüntüsü](./media/sql-database-recovery-using-backups/restore-deleted-sql-managed-instance-annotated.png)

### <a name="deleted-database-restore-by-using-powershell"></a>PowerShell kullanarak silinen veritabanı geri yükleme

Azure SQL Veritabanı için silinmiş bir veritabanını ve PowerShell'i kullanarak yönetilen bir örneği geri yüklemek için aşağıdaki örnek komut dosyalarını kullanın.

#### <a name="single-azure-sql-database"></a>Tek Azure SQL veritabanı

Silinmiş bir Azure SQL veritabanını nasıl geri yükleyin ilerler örnek bir PowerShell komut dosyası için [PowerShell kullanarak bir SQL veritabanını geri yükleme'ye](scripts/sql-database-restore-database-powershell.md)bakın.

#### <a name="managed-instance-database"></a>Yönetilen örnek veritabanı

Silinmiş örnek veritabanını nasıl geri yükleyin ilerler örneklem için [PowerShell'i kullanarak yönetilen örnekte silinmiş veritabanını geri yükleme'ye](sql-database-managed-instance-point-in-time-restore.md#restore-a-deleted-database) bakın

> [!TIP]
> Silinen bir veritabanını programlı olarak geri yüklemek için, [otomatik yedeklemeleri kullanarak programmatically kurtarma gerçekleştirme'ye](sql-database-recovery-using-backups.md)bakın.

## <a name="geo-restore"></a>Coğrafi Geri Yükleme

Herhangi bir Azure bölgesindeki herhangi bir sunucudaki SQL veritabanını en son coğrafi olarak çoğaltılan yedeklemelerden geri yükleyebilirsiniz. Coğrafi geri yükleme kaynağı olarak coğrafi olarak çoğaltılmış bir yedekleme kullanır. Bir kesinti nedeniyle veritabanına veya veri merkezine erişilemiyor olsa bile coğrafi geri yükleme isteyebilirsiniz.

Coğrafi geri yükleme, barındırma bölgesindeki bir olay nedeniyle veritabanınız kullanılamadığında varsayılan kurtarma seçeneğidir. Veritabanını başka herhangi bir bölgedeki sunucuya geri yükleyebilirsiniz. Bir yedeklemenin alınmasıyla farklı bir bölgedeki bir Azure blob'una coğrafi olarak çoğaltılması arasında bir gecikme vardır. Sonuç olarak, geri yüklenen veritabanı özgün veritabanının bir saat gerisinde olabilir. Aşağıdaki resimde, başka bir bölgedeki son kullanılabilir yedeklemeden bir veritabanı geri yüklemesi gösterilmektedir.

![Coğrafi geri yükleme grafiği](./media/sql-database-geo-restore/geo-restore-2.png)

### <a name="geo-restore-by-using-the-azure-portal"></a>Azure portalını kullanarak coğrafi geri yükleme

Azure portalından, yeni bir tek veya yönetilen örnek veritabanı oluşturur ve kullanılabilir bir coğrafi geri yükleme yedeklemesi seçersiniz. Yeni oluşturulan veritabanı, coğrafi olarak geri yüklenen yedekleme verilerini içerir.

#### <a name="single-azure-sql-database"></a>Tek Azure SQL veritabanı

Bölgenizdeki ve seçtiğiniz sunucudaki Azure portalından tek bir SQL veritabanını coğrafi olarak geri yüklemek için aşağıdaki adımları izleyin:

1. **Pano'dan****EKLE SQL Veritabanı'nı** **seçin.** >  Temel **Bilgiler** sekmesinde, gerekli bilgileri girin.
2. **Ek ayarları**seçin.
3. **Varolan verileri kullanmak**için **Yedekleme'yi**seçin.
4. **Yedekleme**için, kullanılabilir coğrafi geri yükleme yedekleri listesinden bir yedekleme seçin.

    ![SQL Veritabanı Oluştur seçeneklerinin ekran görüntüsü](./media/sql-database-recovery-using-backups/geo-restore-azure-sql-database-list-annotated.png)

Yedeklemeden yeni bir veritabanı oluşturma işlemini tamamlayın. Tek bir Azure SQL veritabanı oluşturduğunuzda, geri yüklenen coğrafi geri yükleme yedeklemesini içerir.

#### <a name="managed-instance-database"></a>Yönetilen örnek veritabanı

Yönetilen bir örnek veritabanını Azure portalından seçtiğiniz bir bölgedeki varolan yönetilen bir örneğe coğrafi olarak geri yüklemek için, veritabanının geri yüklenmesini istediğiniz yönetilen bir örneği seçin. Şu adımları uygulayın:

1. **Yeni veritabanını**seçin.
2. İstenilen bir veritabanı adı yazın.
3. **Varolan verileri kullan**altında **Yedekleme'yi**seçin.
4. Kullanılabilir coğrafi geri yükleme yedekleri listesinden bir yedekleme seçin.

    ![Yeni veritabanı seçeneklerinin ekran görüntüsü](./media/sql-database-recovery-using-backups/geo-restore-sql-managed-instance-list-annotated.png)

Yeni bir veritabanı oluşturma işlemini tamamlayın. Örnek veritabanını oluşturduğunuzda, geri yüklenen coğrafi geri yükleme yedeklemesini içerir.

### <a name="geo-restore-by-using-powershell"></a>PowerShell kullanarak coğrafi geri yükleme

#### <a name="single-azure-sql-database"></a>Tek Azure SQL veritabanı

Tek bir SQL veritabanı için coğrafi geri yüklemenin nasıl yapılacağını gösteren bir PowerShell komut dosyası için, [Azure SQL tek veritabanını daha önceki bir noktaya geri yüklemek için PowerShell'i kullan'a](scripts/sql-database-restore-database-powershell.md)bakın.

#### <a name="managed-instance-database"></a>Yönetilen örnek veritabanı

Yönetilen bir örnek veritabanı için coğrafi geri yüklemenin nasıl yapılacağını gösteren bir PowerShell komut dosyası [için, yönetilen örnek veritabanını başka bir coğrafi bölgeye geri yüklemek için PowerShell'i kullan'a](scripts/sql-managed-instance-restore-geo-backup.md)bakın.

### <a name="geo-restore-considerations"></a>Coğrafi geri yükleme hususları

Jeo-ikincil veritabanında zaman içinde geri yükleme gerçekleştiremezsiniz. Bunu yalnızca birincil veritabanında yapabilirsiniz. Bir kesintiden kurtarmak için coğrafi geri yükleme yi kullanma hakkında ayrıntılı bilgi için [bkz.](sql-database-disaster-recovery.md)

> [!IMPORTANT]
> Coğrafi geri yükleme, SQL Veritabanı'nda bulunan en temel olağanüstü durum kurtarma çözümüdür. Otomatik olarak oluşturulan coğrafi olarak çoğaltılan yedeklemelere, kurtarma noktası hedefine (RPO) 1 saate eşit ve tahmini kurtarma süresi12 saate kadar dayanır. Bu, bölgesel bir kesintiden sonra hedef bölgenin veritabanlarınızı geri yükleme kapasitesine sahip olacağını garanti etmez, çünkü talepte ani bir artış olasılığı yüksektir. Uygulamanız nispeten küçük veritabanları kullanıyorsa ve işletme için kritik değilse, coğrafi geri yükleme uygun bir olağanüstü durum kurtarma çözümüdür. Büyük veritabanları gerektiren ve iş sürekliliğini sağlaması gereken iş açısından kritik uygulamalar için [Otomatik hata grupları](sql-database-auto-failover-group.md)kullanın. Bu çok daha düşük RPO ve kurtarma süresi hedefi sunuyor ve kapasite her zaman garantilidir. İş sürekliliği seçenekleri hakkında daha fazla bilgi için iş [sürekliliğine genel bakış](sql-database-business-continuity.md)bölümüne bakın.

## <a name="programmatically-performing-recovery-by-using-automated-backups"></a>Otomatik yedeklemeler kullanarak programlanabilir şekilde kurtarma gerçekleştirme

Kurtarma için Azure PowerShell veya REST API'yi de kullanabilirsiniz. Aşağıdaki tablolarda kullanılabilir komutkümesi açıklayınız.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri büyük ölçüde aynıdır.

#### <a name="single-azure-sql-database"></a>Tek Azure SQL veritabanı

Bağımsız veya havuza edilmiş bir veritabanını geri yüklemek için [Geri Yükleme-AzSqlDatabase'e](/powershell/module/az.sql/restore-azsqldatabase)bakın.

  | Cmdlet | Açıklama |
  | --- | --- |
  | [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) |Bir veya daha fazla veritabanını alır. |
  | [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Geri yükleyebileceğiniz, silinmiş bir veritabanını alır. |
  | [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) |Bir veritabanının coğrafi olarak yedekli bir yedeklemesini alır. |
  | [Geri Yükleme-AzSqlVeritabanı](/powershell/module/az.sql/restore-azsqldatabase) |SQL veritabanını geri yükler. |

  > [!TIP]
  > Veritabanının zamanında geri yüklemesinin nasıl yapılacağını gösteren örnek bir PowerShell komut dosyası için [PowerShell kullanarak bir SQL veritabanını geri yükleme](scripts/sql-database-restore-database-powershell.md)bölümüne bakın.

#### <a name="managed-instance-database"></a>Yönetilen örnek veritabanı

Yönetilen örnek veritabanını geri yüklemek için geri [yükleme-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase)'a bakın.

  | Cmdlet | Açıklama |
  | --- | --- |
  | [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance) |Bir veya daha fazla yönetilen örneği alır. |
  | [Get-AzSqlInstanceDatabase](/powershell/module/az.sql/get-azsqlinstancedatabase) | Örnek veritabanı alır. |
  | [Geri Yükleme-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase) |Örnek veritabanını geri yükler. |

### <a name="rest-api"></a>REST API

REST API'yi kullanarak tek veya havuza edilmiş bir veritabanını geri yüklemek için:

| API | Açıklama |
| --- | --- |
| [REST (createMode=Recovery)](https://docs.microsoft.com/rest/api/sql/databases) |Veritabanını geri yükler. |
| [Veritabanı Durumu Oluştur veya Güncelleştir](https://docs.microsoft.com/rest/api/sql/operations) |Geri yükleme işlemi sırasında durumu döndürür. |

### <a name="azure-cli"></a>Azure CLI

#### <a name="single-azure-sql-database"></a>Tek Azure SQL veritabanı

Azure CLI'yi kullanarak tek veya havuza tıkılmış bir veritabanını geri yüklemek için [az sql db geri yükleme'ye](/cli/azure/sql/db#az-sql-db-restore)bakın.

#### <a name="managed-instance-database"></a>Yönetilen örnek veritabanı

Azure CLI'yi kullanarak yönetilen örnek veritabanını geri yüklemek için [az sql midb geri yükleme'ye](/cli/azure/sql/midb#az-sql-midb-restore)bakın.

## <a name="summary"></a>Özet

Otomatik yedeklemeler veritabanlarınızı kullanıcı ve uygulama hatalarından, yanlışlıkla veritabanı silme işlemine ve uzun süreli kesintilere karşı korur. Bu yerleşik özellik tüm hizmet katmanları ve işlem boyutları için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [İş sürekliliğine genel bakış](sql-database-business-continuity.md)
- [SQL Veritabanı otomatik yedeklemeleri](sql-database-automated-backups.md)
- [Uzun vadeli bekletme](sql-database-long-term-retention.md)
- Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) veya Otomatik başarısız [gruplarına](sql-database-auto-failover-group.md)bakın.
