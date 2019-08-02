---
title: Bir Azure SQL veritabanını yedekten geri yükleme | Microsoft Docs
description: Bir Azure SQL veritabanını zaman içinde önceki bir noktaya (35 güne kadar) geri almanıza olanak sağlayan bir noktadan noktaya geri yükleme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 04/30/2019
ms.openlocfilehash: 55d60ec332515fcfa3deb565a4a770027681537a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566972"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Otomatik veritabanı yedeklemeleri kullanarak bir Azure SQL veritabanını kurtarma

Varsayılan olarak, SQL veritabanı yedeklemeleri coğrafi olarak çoğaltılan BLOB depolama alanında (RA-GRS) depolanır. [Otomatik veritabanı yedeklemeleri](sql-database-automated-backups.md)kullanılarak veritabanı kurtarma için aşağıdaki seçenekler kullanılabilir:

- Aynı SQL veritabanı sunucusunda, bekletme dönemi içinde belirli bir zaman noktasına kurtarılan yeni bir veritabanı oluşturun.
- Silinen bir veritabanının silme zamanına kurtarılan aynı SQL veritabanı sunucusunda bir veritabanı oluşturun.
- En son yedeklemelerin bulunduğu noktaya kurtarılan aynı bölgedeki herhangi bir SQL veritabanı sunucusunda yeni bir veritabanı oluşturun.
- En son çoğaltılan yedeklemelerin noktasına kurtarılan başka bir bölgedeki herhangi bir SQL veritabanı sunucusunda yeni bir veritabanı oluşturun.

[Yedekleme uzun süreli saklama](sql-database-long-term-retention.md)yapılandırdıysanız, HERHANGI bir SQL veritabanı sunucusundaki HERHANGI bir LTR yedeklemesinden yeni bir veritabanı da oluşturabilirsiniz.

> [!IMPORTANT]
> Geri yükleme sırasında var olan bir veritabanının üzerine yazamaz.

Standart veya Premium hizmet katmanını kullanırken, geri yüklenen bir veritabanı aşağıdaki koşullarda ek bir depolama maliyeti doğurur:

- Veritabanının en büyük boyutu 500 GB 'den büyükse, P11 – P15 to S4-S12 veya P1 – P6 geri yükleyin.
- Veritabanının en büyük boyutu 250 GB 'den büyükse P1 – P6 to S4-S12 ' i geri yükleyin.

Geri yüklenen veritabanının en büyük boyutu hedef veritabanının hizmet katmanına ve performans düzeyine dahil edilen depolama miktarından daha büyükse, ek maliyet ICursor olur. Dahil edilen miktarın üzerinde sağlanan ek depolama alanı ekstra olarak ücretlendirilir. Ek depolamanın fiyatlandırma ayrıntıları için bkz. [SQL Veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/). Kullanılan alanın gerçek miktarı, dahil edilen depolama miktarından daha azsa, en fazla veritabanı boyutunu dahil edilen miktara ayarlayarak bu ekstra maliyetten kaçınabilirsiniz.

> [!NOTE]
> [Otomatik veritabanı yedeklemeleri](sql-database-automated-backups.md) , bir [veritabanı kopyası](sql-database-copy.md)oluştururken kullanılır.

## <a name="recovery-time"></a>Kurtarma zamanı

Otomatik veritabanı yedeklemeleri kullanarak bir veritabanının geri yüklenmesi için kurtarma süresi, birkaç faktörden etkilenir:

- Veritabanı boyutu
- Veritabanının işlem boyutu
- Dahil edilen işlem günlüğü sayısı
- Geri yükleme noktasına kurtarmak için yeniden yürütülmesi gereken etkinliğin miktarı
- Geri yükleme farklı bir bölgeye ise ağ bant genişliği
- Hedef bölgede işlenmekte olan eşzamanlı geri yükleme isteklerinin sayısı

Büyük ve/veya çok etkin bir veritabanı için geri yükleme birkaç saat sürebilir. Bir bölgede uzun bir kesinti varsa, diğer bölgeler tarafından işlenen çok sayıda coğrafi geri yükleme isteği olması mümkündür. Çok sayıda istek olduğunda, bu bölgedeki veritabanları için kurtarma süresi artırılabilir. Çoğu veritabanı geri yükleme işleminin 12 saatten az olması tamamlanır.

Tek bir abonelik için, eş zamanlı geri yükleme istekleri sayısında sınırlamalar vardır.  Bu sınırlamalar, zaman içinde herhangi bir noktaya geri yükleme, coğrafi geri yükleme ve geri yükleme uzun süreli saklama yedeklemesinden yapılan herhangi bir bileşim için geçerlidir:

| | **İşlenmekte olan eşzamanlı istek sayısı üst sınırı** | **Gönderilen en fazla eşzamanlı istek sayısı** |
| :--- | --: | --: |
|Tek veritabanı (abonelik başına)|10|60|
|Elastik havuz (havuz başına)|4|200|
||||

Şu anda sunucunun tamamını geri yüklemek için yerleşik bir yöntem yok. [Azure SQL veritabanı: Tam sunucu kurtarma](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) betiği, bu görevi nasıl gerçekleştirebileceğiniz konusunda bir örnektir.

> [!IMPORTANT]
> Otomatik yedeklemeleri kullanarak kurtarmak için, abonelikte SQL Server katkıda bulunan rolünün bir üyesi olmanız veya abonelik sahibi olmanız gerekir-bkz [. RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md). Verileri Azure portalı, PowerShell veya REST API kullanarak kurtarabilirsiniz. Transact-SQL kullanamazsınız.

## <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

Tek başına, havuza alınmış veya örnek veritabanını Azure portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase)veya [REST API](https://docs.microsoft.com/rest/api/sql/databases)kullanarak daha önceki bir zaman noktasına geri yükleyebilirsiniz. İstek, geri yüklenen veritabanı için herhangi bir hizmet katmanını veya işlem boyutunu belirtebilir. Veritabanını geri yüklediğiniz sunucuda yeterli kaynak bulunduğundan emin olun. Bu işlem tamamlandıktan sonra, özgün veritabanıyla aynı sunucuda yeni bir veritabanı oluşturulur. Geri yüklenen veritabanı, hizmet katmanına ve işlem boyutuna bağlı olarak normal ücretler üzerinden ücretlendirilir. Veritabanı geri yüklemesi tamamlanana kadar ücretlendirilmeyin.

Genellikle kurtarma amacıyla bir veritabanını önceki bir noktaya geri yüklemeniz gerekir. Geri yüklenen veritabanını özgün veritabanının yerini alacak şekilde kabul edebilir veya özgün veritabanını güncelleştirmek için kaynak veri olarak kullanabilirsiniz.

- **Veritabanı değiştirme**

  Geri yüklenen veritabanı özgün veritabanının yerini alacak şekilde tasarlanıyorsa, özgün veritabanının işlem boyutunu ve hizmet katmanını belirtmeniz gerekir. Ardından özgün veritabanını yeniden adlandırabilir ve geri yüklenen veritabanına, T-SQL ' de [alter database](/sql/t-sql/statements/alter-database-azure-sql-database) komutunu kullanarak özgün adı verebilirsiniz.

- **Veri kurtarma**

  Bir kullanıcı veya uygulama hatasından kurtarmak için geri yüklenen veritabanından veri almayı planlıyorsanız, geri yüklenen veritabanından veri çıkaran ve özgün veritabanına uygulanan bir veri kurtarma betiği yazmanız ve yürütmeniz gerekir. Geri yükleme işleminin tamamlanması uzun zaman alabilir, ancak geri yükleme veritabanı geri yükleme işlemi boyunca veritabanı listesinde görünür. Veritabanını geri yükleme sırasında silerseniz geri yükleme işlemi iptal edilir ve geri yüklemeyi tamamlamamış olan veritabanı için ücretlendirilmeyecektir.

Tek bir havuza alınmış veya örnek veritabanını Azure portal kullanarak bir zaman noktasına kurtarmak için, veritabanınızın sayfasını açın ve araç çubuğunda **geri yükle** ' ye tıklayın.

![zaman içinde bir noktaya geri yükleme](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

> [!IMPORTANT]
> Bir veritabanını bir yedekten programlı bir şekilde geri yüklemek için bkz. [Otomatik yedeklemeleri kullanarak programlı bir şekilde kurtarma gerçekleştirme](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="deleted-database-restore"></a>Veritabanı geri yükleme silindi

Silinen bir veritabanını, Azure portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase)veya [Rest (createmode = restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)kullanarak aynı SQL veritabanı sunucusundaki silme zamanına veya önceki bir zaman noktasına geri yükleyebilirsiniz. [Yönetilen örnekteki silinen veritabanını PowerShell kullanarak geri](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance)yükleyebilirsiniz. 

> [!TIP]
> Silinen bir veritabanının nasıl geri yükleneceğini gösteren örnek bir PowerShell betiği için bkz. [PowerShell kullanarak BIR SQL veritabanını geri yükleme](scripts/sql-database-restore-database-powershell.md).
> [!IMPORTANT]
> Bir Azure SQL veritabanı sunucu örneğini silerseniz, tüm veritabanları da silinir ve kurtarılamaz. Silinen sunucuları geri yükleme işlemi şu an için desteklenmemektedir.

### <a name="deleted-database-restore-using-the-azure-portal"></a>Azure portal kullanarak veritabanı geri yükleme silindi

Silinen bir veritabanını Azure portal kullanarak kurtarmak için, sunucunuza yönelik sayfayı açın ve Işlemler alanında, **silinen veritabanları**' na tıklayın.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)

![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

> [!IMPORTANT]
> Silinen bir veritabanını program aracılığıyla geri yüklemek için bkz. [Otomatik yedeklemeleri kullanarak programlı bir şekilde kurtarma gerçekleştirme](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="geo-restore"></a>Coğrafi Geri Yükleme

Her bir Azure bölgesindeki sunucuda bir SQL veritabanını, en son coğrafi çoğaltılan yedeklerden geri yükleyebilirsiniz. Coğrafi geri yükleme, kaynak olarak coğrafi olarak çoğaltılan bir yedeklemeyi kullanır. Veritabanı veya veri merkezi bir kesinti nedeniyle erişilemez olsa bile istenebilir.

Coğrafi geri yükleme, veritabanınız barındırma bölgesindeki bir olay nedeniyle kullanılamadığında varsayılan kurtarma seçeneğidir. Veritabanını başka bir bölgedeki sunucuya geri yükleyebilirsiniz. Bir yedeklemenin ne zaman alındığı ve farklı bir bölgedeki Azure blobuna coğrafi olarak çoğaltılma arasında bir gecikme vardır. Sonuç olarak, geri yüklenen veritabanı orignal veritabanının arkasında bir saate kadar sürebilir. Aşağıdaki çizimde veritabanının başka bir bölgedeki son kullanılabilir yedekten geri yüklenmesi gösterilmektedir.

![coğrafi geri yükleme](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Coğrafi geri yükleme yapmayı gösteren örnek bir PowerShell betiği için bkz. [PowerShell kullanarak BIR SQL veritabanını geri yükleme](scripts/sql-database-restore-database-powershell.md).

Bir coğrafi-ikincil üzerinde noktadan noktaya geri yükleme şu anda desteklenmiyor. Zaman içinde geri yükleme yalnızca birincil veritabanında yapılabilir. Bir kesinti durumundan kurtulmak için coğrafi geri yükleme kullanma hakkında ayrıntılı bilgi için bkz. [bir kesinti Ile kurtarma](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Coğrafi geri yükleme, SQL veritabanında bulunan en temel olağanüstü durum kurtarma çözümüdür. RPO = 1 saat ve tahmini kurtarma süresi olan en fazla 12 saate kadar otomatik olarak oluşturulan coğrafi olarak çoğaltılan yedeklemeleri kullanır. Hedef bölgenin, bir bölge ourage sonra veritabanınızı geri yükleme kapasitesine sahip olacağını garanti etmez, çünkü bu da keskin bir istek artışı olacaktır. Görece küçük veritabanları kullanan iş dışı kritik uygulamalar için, coğrafi geri yükleme uygun bir olağanüstü durum kurtarma çözümüdür. Büyük veritabanları kullanan ve iş sürekliliği sağlamak için iş açısından kritik uygulamalar için [otomatik yük devretme grupları](sql-database-auto-failover-group.md)kullanmanız gerekir. Daha düşük bir RPO ve RTO sağlar ve kapasite her zaman garanti edilir. İş sürekliliği seçenekleri hakkında daha fazla bilgi için bkz. [iş sürekliliği konusuna genel bakış](sql-database-business-continuity.md).

### <a name="geo-restore-using-the-azure-portal"></a>Azure portal kullanarak coğrafi geri yükleme

Bir veritabanını [DTU tabanlı model tutma süresi](sql-database-service-tiers-dtu.md) sırasında veya [sanal çekirdek tabanlı model tutma süresi](sql-database-service-tiers-vcore.md) Azure Portal kullanarak coğrafi olarak geri yüklemek için SQL veritabanları sayfasını açın ve ardından **Ekle**' ye tıklayın. **Kaynak Seç** metin kutusunda **Yedekle**' yi seçin. Kurtarmada ve seçtiğiniz sunucuda kurtarmanın gerçekleştirileceği yedeği belirtin.

> [!Note]
> Azure portal kullanılarak coğrafi geri yükleme yönetilen örnekte kullanılamaz. Lütfen bunun yerine PowerShell kullanın.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Otomatik yedeklemeleri kullanarak programlı bir şekilde kurtarma gerçekleştirme

Daha önce anlatıldığı gibi, Azure portal ek olarak, veritabanı kurtarma, Azure PowerShell veya REST API kullanılarak programlı bir şekilde gerçekleştirilebilir. Aşağıdaki tablolarda kullanılabilen komut kümesi açıklanır.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

- Tek başına veya havuza alınmış bir veritabanını geri yüklemek için bkz. [restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase).

  | Cmdlet | Açıklama |
  | --- | --- |
  | [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) |Bir veya daha fazla veritabanını alır. |
  | [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Geri yükleyebileceğiniz, silinmiş bir veritabanını alır. |
  | [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) |Bir veritabanının coğrafi olarak yedekli bir yedeklemesini alır. |
  | [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) |SQL veritabanını geri yükler. |

  > [!TIP]
  > Bir veritabanının bir noktadan noktaya geri yüklemesini nasıl gerçekleştirekullanacağınızı gösteren örnek bir PowerShell betiği için bkz. [PowerShell kullanarak BIR SQL veritabanını geri yükleme](scripts/sql-database-restore-database-powershell.md).

- Yönetilen örnek veritabanını geri yüklemek için bkz. [restore-Azsqlınstancedatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase).

  | Cmdlet | Açıklama |
  | --- | --- |
  | [Get-Azsqlınstance](/powershell/module/az.sql/get-azsqlinstance) |Bir veya daha fazla yönetilen örneği alır. |
  | [Get-Azsqlınstancedatabase](/powershell/module/az.sql/get-azsqlinstancedatabase) | Örnek veritabanlarını alır. |
  | [Restore-Azsqlınstancedatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase) |Örnek veritabanını geri yükler. |

### <a name="rest-api"></a>REST API

REST API kullanarak tek veya havuza alınmış bir veritabanını geri yüklemek için:

| API | Açıklama |
| --- | --- |
| [REST (createMode = kurtarma)](https://docs.microsoft.com/rest/api/sql/databases) |Bir veritabanını geri yükler |
| [Veritabanı oluşturma veya güncelleştirme durumunu al](https://docs.microsoft.com/rest/api/sql/operations) |Geri yükleme işlemi sırasında durumu döndürür |

### <a name="azure-cli"></a>Azure CLI

- Azure CLı kullanarak tek veya havuza alınmış bir veritabanını geri yüklemek için, bkz. [az SQL DB restore](/cli/azure/sql/db#az-sql-db-restore).
- Azure CLı kullanarak yönetilen bir örneği geri yüklemek için, bkz. [az SQL mıdb restore](/cli/azure/sql/midb#az-sql-midb-restore)

## <a name="summary"></a>Özet

Otomatik yedeklemeler, veritabanlarınızı Kullanıcı ve uygulama hatalarından, yanlışlıkla veritabanı silmeye ve uzun kesintilere karşı korur. Bu yerleşik yetenek tüm hizmet katmanları ve işlem boyutları için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- İş sürekliliği için genel bakış ve senaryolar için bkz. [iş sürekliliği genel bakış](sql-database-business-continuity.md).
- Azure SQL veritabanı otomatik yedeklemeleri hakkında bilgi edinmek için bkz. [SQL veritabanı otomatik yedeklemeleri](sql-database-automated-backups.md).
- Uzun süreli saklama hakkında bilgi edinmek için bkz. [uzun süreli saklama](sql-database-long-term-retention.md).
- Daha hızlı kurtarma seçenekleri hakkında daha fazla bilgi edinmek için bkz. [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) veya [otomatik yük devretme grupları](sql-database-auto-failover-group.md).
