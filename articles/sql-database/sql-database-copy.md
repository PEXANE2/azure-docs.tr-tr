---
title: Veritabanı kopyalama
description: Aynı sunucuda veya farklı bir sunucuda varolan bir Azure SQL veritabanının işlem açısından tutarlı bir kopyasını oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sashan
ms.reviewer: carlrab
ms.date: 02/24/2020
ms.openlocfilehash: 7e4744627cfd08874e07bb126df048ea3e644f39
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473753"
---
# <a name="copy-a-transactionally-consistent-copy-of-an-azure-sql-database"></a>Azure SQL veritabanının işlem açısından tutarlı bir kopyasını kopyalama

Azure SQL Veritabanı, aynı sunucuda veya farklı bir sunucuda varolan bir Azure SQL veritabanının[(tek veritabanı)](sql-database-single-database.md)işlem açısından tutarlı bir kopyasını oluşturmak için çeşitli yöntemler sağlar. Azure portalını, PowerShell'i veya T-SQL'i kullanarak bir SQL veritabanıkopyalayabilirsiniz.

## <a name="overview"></a>Genel Bakış

Veritabanı kopyası, kopya isteğinin zaman itibariyle kaynak veritabanının anlık görüntüsüdür. Aynı sunucuyu veya farklı bir sunucuyu seçebilirsiniz. Ayrıca, hizmet katmanını ve işlem boyutunu korumayı seçebilir veya aynı hizmet katmanı (sürüm) içinde farklı bir işlem boyutu kullanabilirsiniz. Kopya tamamlandıktan sonra, tamamen işlevsel, bağımsız bir veritabanı haline gelir. Bu noktada, herhangi bir baskıya yükseltebilir veya düşürebilirsiniz. Oturum açmalar, kullanıcılar ve izinler bağımsız olarak yönetilebilir. Kopya, coğrafi çoğaltma teknolojisi kullanılarak oluşturulur ve tohumlama tamamlandıktan sonra coğrafi çoğaltma bağlantısı otomatik olarak sonlandırılır. Coğrafi çoğaltma kullanmak için tüm gereksinimler veritabanı kopyalama işlemi için geçerlidir. Ayrıntılar için [Etkin coğrafi çoğaltma genel görünümüne](sql-database-active-geo-replication.md) bakın.

> [!NOTE]
> Veritabanı kopyası oluşturduğunuzda [otomatik veritabanı yedeklemeleri](sql-database-automated-backups.md) kullanılır.

## <a name="logins-in-the-database-copy"></a>Veritabanı kopyasında oturum açma

Bir veritabanını aynı SQL Veritabanı sunucusuna kopyaladığınızda, her iki veritabanında da aynı oturum açmalar kullanılabilir. Veritabanını kopyalamak için kullandığınız güvenlik ilkesi, yeni veritabanında veritabanı sahibi olur. 

Bir veritabanını farklı bir SQL Veritabanı sunucusuna kopyaladiğinizde, hedef sunucudaki kopyalama işlemini başlatan güvenlik ilkesi yeni veritabanının sahibi olur. 

Hedef sunucudan bağımsız olarak, tüm veritabanı kullanıcıları, izinleri ve güvenlik tanımlayıcıları (SIT'ler) veritabanı kopyasına kopyalanır. Veri erişimi için [içerdiği veritabanı kullanıcılarının](sql-database-manage-logins.md) kullanılması, kopyalanan veritabanının aynı kullanıcı kimlik bilgilerine sahip olmasını sağlar, böylece kopya tamamlandıktan sonra hemen aynı kimlik bilgileriyle erişebilirsiniz.

Veri erişimi için sunucu düzeyinde ki girişleri kullanır ve veritabanını farklı bir sunucuya kopyalarsanız, oturum açma tabanlı erişim çalışmayabilir. Girişler hedef sunucuda bulunmadığından veya parolaları ve güvenlik tanımlayıcıları (SID'ler) farklı olduğundan bu durum olabilir. Bir veritabanını farklı bir SQL Veritabanı sunucusuna kopyalarken girişleri yönetme hakkında bilgi edinmek [için, olağanüstü durum kurtarma sonrasında Azure SQL veritabanı güvenliğini nasıl yönetisiniz](sql-database-geo-replication-security-config.md)ebakını görün. Farklı bir sunucuya kopyalama işlemi başarılı olduktan sonra ve diğer kullanıcılar yeniden eşlenmeden önce, yalnızca veritabanı sahibiyle ilişkili oturum açma veya sunucu yöneticisi kopyalanan veritabanında oturum açabilir. Oturum açma işlemlerinin çözümü ve kopyalama işlemi tamamlandıktan sonra veri erişimi kurmak için [bkz.](#resolve-logins)

## <a name="copy-a-database-by-using-the-azure-portal"></a>Azure portalını kullanarak veritabanını kopyalama

Azure portalını kullanarak bir veritabanını kopyalamak için veritabanınızın sayfasını açın ve ardından **Kopyala'yı**tıklatın.

   ![Veritabanı kopyası](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell-or-azure-cli"></a>PowerShell veya Azure CLI kullanarak veritabanını kopyalama

Veritabanını kopyalamak için aşağıdaki örnekleri kullanın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell [için, Yeni-AzSqlDatabaseCopy](/powershell/module/az.sql/new-azsqldatabasecopy) cmdlet kullanın.

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi (RM) modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirmeler Az.Sql modülü içindir. AzureRM modülü en az Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecektir.  Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumlulukları hakkında daha fazla bilgi için [yeni Azure PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

```powershell
New-AzSqlDatabaseCopy -ResourceGroupName "<resourceGroup>" -ServerName $sourceserver -DatabaseName "<databaseName>" `
    -CopyResourceGroupName "myResourceGroup" -CopyServerName $targetserver -CopyDatabaseName "CopyOfMySampleDatabase"
```

Veritabanı kopyası bir eşzamanlı işlemdir, ancak hedef veritabanı istek kabul edildikten hemen sonra oluşturulur. Devam ederken kopyalama işlemini iptal etmeniz gerekiyorsa, [Remove-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) cmdlet'i kullanarak hedef veritabanını bırakın.

Tam bir örnek PowerShell komut dosyası [için](scripts/sql-database-copy-database-to-new-server-powershell.md)bkz.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az sql db copy --dest-name "CopyOfMySampleDatabase" --dest-resource-group "myResourceGroup" --dest-server $targetserver `
    --name "<databaseName>" --resource-group "<resourceGroup>" --server $sourceserver
```

Veritabanı kopyası bir eşzamanlı işlemdir, ancak hedef veritabanı istek kabul edildikten hemen sonra oluşturulur. Devam ederken kopyalama işlemini iptal etmeniz gerekiyorsa, [az sql db delete](/cli/azure/sql/db#az-sql-db-delete) komutunu kullanarak hedef veritabanını bırakın.

* * *

## <a name="rbac-roles-to-manage-database-copy"></a>Veritabanı kopyasını yönetmek için RBAC rolleri

Veritabanı kopyasını oluşturmak için aşağıdaki rollerde olmanız gerekir

- Abonelik Sahibi veya
- SQL Server Katılımcısıfatı rolü veya
- Aşağıdaki izinle kaynak ve hedef veritabanlarında özel rol:

   Microsoft.Sql/servers/databases/read Microsoft.Sql/servers/databases/write

Bir veritabanı kopyasını iptal etmek için aşağıdaki rollerde olmanız gerekir

- Abonelik Sahibi veya
- SQL Server Katılımcısıfatı rolü veya
- Aşağıdaki izinle kaynak ve hedef veritabanlarında özel rol:

   Microsoft.Sql/servers/databases/read Microsoft.Sql/servers/databases/write

Azure portalını kullanarak veritabanı kopyasını yönetmek için aşağıdaki izinlere de ihtiyacınız olacaktır:

   Microsoft.Resources/subscriptions/resources/read Microsoft.Resources/subscriptions/resources/write Microsoft.Resources/deployments/read Microsoft.Resources/deployments/write Microsoft.Resources/deployments/operationstatuses/read

Portaldaki kaynak grubunda dağıtımaltındaki işlemleri, SQL işlemleri de dahil olmak üzere birden çok kaynak sağlayıcısıarasında işlemleri görmek istiyorsanız, şu ek RBAC rollerine ihtiyacınız olacaktır:

   Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read

## <a name="copy-a-database-by-using-transact-sql"></a>Transact-SQL kullanarak veritabanını kopyalama

Sunucu yöneticisi girişi veya kopyalamak istediğiniz veritabanını oluşturan oturum açma ile ana veritabanına giriş yapın. Veritabanı kopyasının başarılı olabilmesi için, sunucu yöneticisi olmayan oturum `dbmanager` açmaların rolün üyeleri olması gerekir. Oturum açma lar ve sunucuya bağlanma hakkında daha fazla bilgi için [bkz.](sql-database-manage-logins.md)

CREATE DATABASE ile kaynak veritabanıkopyalamaya başlayın [... İfadeNIN KOPYASI](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current#copy-a-database) OLARAK. T-SQL deyimi veritabanı kopyalama işlemi tamamlanana kadar çalışmaya devam ediyor.

> [!NOTE]
> T-SQL deyimini sonlandırmak veritabanı kopyalama işlemini sonlandırmaz. İşlemi sonlandırmak için hedef veritabanını bırakın.
>

### <a name="copy-a-sql-database-to-the-same-server"></a>SQL veritabanını aynı sunucuya kopyalama

Sunucu yöneticisi girişi veya kopyalamak istediğiniz veritabanını oluşturan oturum açma ile ana veritabanına giriş yapın. Veritabanı kopyalamanın başarılı olması için, sunucu yöneticisi olmayan oturum açmaların rolün `dbmanager` üyeleri olması gerekir.

Bu komut, Database1'i aynı sunucuda Database2 adlı yeni bir veritabanına kopyalar. Veritabanınızın boyutuna bağlı olarak, kopyalama işleminin tamamlanması biraz zaman alabilir.

   ```sql
   -- execute on the master database to start copying
   CREATE DATABASE Database2 AS COPY OF Database1;
   ```

### <a name="copy-a-sql-database-to-a-different-server"></a>SQL veritabanını farklı bir sunucuya kopyalama

Yeni veritabanının oluşturulacağı hedef sunucunun ana veritabanına giriş yapın. Kaynak sunucudaki kaynak veritabanının veritabanı sahibiyle aynı ad ve parolaya sahip bir giriş kullanın. Hedef sunucudaki oturum açma da `dbmanager` rolün bir üyesi veya sunucu yöneticisi girişi olmalıdır.

Bu komut, server1'deki Database1'i server2'de Database2 adlı yeni bir veritabanına kopyalar. Veritabanınızın boyutuna bağlı olarak, kopyalama işleminin tamamlanması biraz zaman alabilir.

```sql
-- Execute on the master database of the target server (server2) to start copying from Server1 to Server2
CREATE DATABASE Database2 AS COPY OF server1.Database1;
```

> [!IMPORTANT]
> Her iki sunucunun güvenlik duvarları, T-SQL CREATE DATABASE'i veren istemcinin IP'sinden gelen bağlantıya izin verecek şekilde yapılandırılmalıdır... AS KOPYA KOMUTU.

### <a name="copy-a-sql-database-to-a-different-subscription"></a>SQL veritabanını farklı bir aboneye kopyalama

Veritabanınızı T-SQL kullanarak farklı bir abonelikteki bir SQL Veritabanı sunucusuna kopyalamak için [SQL veritabanını farklı bir sunucubölümüne kopyala](#copy-a-sql-database-to-a-different-server) adımlarını kullanabilirsiniz. Kaynak veritabanının veritabanı sahibiyle aynı ad ve parolaya sahip bir giriş kullandığınızdan emin olun. Ayrıca, giriş rolün `dbmanager` bir üyesi veya hem kaynak hem de hedef sunucularda bir sunucu yöneticisi olmalıdır.

> [!NOTE]
> [Azure portalı](https://portal.azure.com), PowerShell ve Azure CLI veritabanı kopyasını farklı bir aboneye desteklemez.

### <a name="monitor-the-progress-of-the-copying-operation"></a>Kopyalama işleminin ilerlemesini izleme

[sys.veritabanları, sys.dm_database_copies](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-databases-transact-sql)ve [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) görünümlerini sorgulayarak kopyalama işlemini izleyin. [sys.dm_database_copies](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database) Kopyalama devam ederken, yeni veritabanı için sys.databases görünümü **state_desc** sütun **KOPYALAMA**olarak ayarlanır.

* Kopyalama başarısız olursa, yeni veritabanı için sys.databases görünümü **state_desc** **sütunu SUSPECT**olarak ayarlanır. Yeni veritabanında DROP deyimini çalıştırın ve daha sonra yeniden deneyin.
* Kopyalama başarılı olursa, yeni veritabanı için sys.databases görünümü **state_desc** **sütunu ONLINE**olarak ayarlanır. Kopyalama tamamlandı ve yeni veritabanı kaynak veritabanından bağımsız olarak değiştirilebilen düzenli bir veritabanıdır.

> [!NOTE]
> Devam ederken kopyalamayı iptal etmeye karar verirseniz, yeni veritabanında [DROP DATABASE](https://docs.microsoft.com/sql/t-sql/statements/drop-database-transact-sql) deyimini çalıştırın.

> [!IMPORTANT]
> Kaynaktan çok daha küçük bir hizmet hedefine sahip bir kopya oluşturmanız gerekiyorsa, hedef veritabanı tohumlama işlemini tamamlamak için yeterli kaynağa sahip olmayabilir ve kopya operaion başarısız olmasına neden olabilir. Bu senaryoda, farklı bir sunucuda ve/veya farklı bir bölgede kopya oluşturmak için coğrafi geri yükleme isteği kullanın. Bkz. Daha fazla informaion için [veritabanı yedeklemelerini kullanarak bir Azure SQL veritabanını kurtarın.](sql-database-recovery-using-backups.md#geo-restore)

## <a name="resolve-logins"></a>Oturum açmaları çözümle

Yeni veritabanı hedef sunucuda çevrimiçi olduktan sonra ALTER [USER](https://docs.microsoft.com/sql/t-sql/statements/alter-user-transact-sql?view=azuresqldb-current) deyimini kullanarak yeni veritabanındaki kullanıcıları hedef sunucudaki oturum açmalara yeniden eşleyin. Yetim kullanıcıları çözümlemek için, [Sorun Giderme Kullanıcıları](https://docs.microsoft.com/sql/sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server)gör [Ayrıca](sql-database-geo-replication-security-config.md)bkz.

Yeni veritabanındaki tüm kullanıcılar kaynak veritabanında sahip oldukları izinleri korur. Veritabanı kopyasını başlatan kullanıcı, yeni veritabanının veritabanı sahibi olur. Kopyalama başarılı olduktan ve diğer kullanıcılar yeniden ele alındıktan sonra, yeni veritabanında yalnızca veritabanı sahibi oturum açabilir.

Bir veritabanını farklı bir SQL Veritabanı sunucusuna kopyalarken kullanıcıları ve oturum açmaları yönetme hakkında bilgi edinmek [için, olağanüstü durum kurtarma sonrasında Azure SQL veritabanı güvenliğini nasıl yönetisiniz](sql-database-geo-replication-security-config.md)e bakın.

## <a name="database-copy-errors"></a>Veritabanı kopyalama hataları

Azure SQL Veritabanı'nda bir veritabanı kopyalanırken aşağıdaki hatalarla karşılaşılabilir. Daha fazla bilgi için bkz. [Azure SQL Veritabanını kopyalama](sql-database-copy.md).

| Hata kodu | Severity | Açıklama |
| ---:| ---:|:--- |
| 40635 |16 |IP adresi '%.&#x2a;ls' olan istemci geçici olarak devre dışı bırakılır. |
| 40637 |16 |Veritabanı kopyasını oluştur şu anda devre dışı bırakılmıştır. |
| 40561 |16 |Veritabanı kopyası başarısız oldu. Kaynak veya hedef veritabanı yok. |
| 40562 |16 |Veritabanı kopyası başarısız oldu. Kaynak veritabanı kaldırıldı. |
| 40563 |16 |Veritabanı kopyası başarısız oldu. Hedef veritabanı düştü. |
| 40564 |16 |Veritabanı kopyası bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40565 |16 |Veritabanı kopyası başarısız oldu. Aynı kaynaktan en fazla 1 eşzamanlı veritabanı kopyasına izin verilir. Lütfen hedef veritabanını bırakın ve daha sonra tekrar deneyin. |
| 40566 |16 |Veritabanı kopyası bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40567 |16 |Veritabanı kopyası bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40568 |16 |Veritabanı kopyası başarısız oldu. Kaynak veritabanı kullanılamaz hale geldi. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40569 |16 |Veritabanı kopyası başarısız oldu. Hedef veritabanı kullanılamaz hale geldi. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40570 |16 |Veritabanı kopyası bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve daha sonra tekrar deneyin. |
| 40571 |16 |Veritabanı kopyası bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve daha sonra tekrar deneyin. |

## <a name="next-steps"></a>Sonraki adımlar

- Oturum açmalar hakkında daha fazla bilgi için, genel durum kurtarma sonrasında [girişleri yönet](sql-database-manage-logins.md) ve Azure SQL veritabanı güvenliğini [nasıl yönetebilirsiniz'e](sql-database-geo-replication-security-config.md)bakın.
- Veritabanını dışa aktarmak [için veritabanını BACPAC'a dışa aktarma'ya](sql-database-export.md)bakın.
