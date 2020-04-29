---
title: Veritabanı kopyalama
description: Aynı sunucuda veya farklı bir sunucuda var olan bir Azure SQL veritabanının işlemsel olarak tutarlı bir kopyasını oluşturun.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79473753"
---
# <a name="copy-a-transactionally-consistent-copy-of-an-azure-sql-database"></a>Azure SQL veritabanı 'nın işlem temelli tutarlı bir kopyasını kopyalama

Azure SQL veritabanı, aynı sunucuda veya farklı bir sunucuda var olan bir Azure SQL veritabanının ([tek veritabanı](sql-database-single-database.md)) işlemsel olarak tutarlı bir kopyasını oluşturmak için çeşitli yöntemler sağlar. Azure portal, PowerShell veya T-SQL ' i kullanarak bir SQL veritabanını kopyalayabilirsiniz.

## <a name="overview"></a>Genel Bakış

Veritabanı kopyası, kopyalama isteği sırasında kaynak veritabanının anlık görüntüsüdür. Aynı sunucuyu veya farklı bir sunucuyu seçebilirsiniz. Ayrıca, hizmet katmanını ve işlem boyutunu tutmayı seçebilir veya aynı hizmet katmanı (Edition) içinde farklı bir işlem boyutu kullanabilirsiniz. Kopyalama işlemi tamamlandıktan sonra, tamamen işlevsel, bağımsız bir veritabanı haline gelir. Bu noktada, dilediğiniz sürüme yükseltebilir veya onu indirgeyede olursunuz. Oturum açmalar, kullanıcılar ve izinler bağımsız olarak yönetilebilir. Kopya, coğrafi çoğaltma teknolojisi kullanılarak oluşturulur ve dengeli dağıtım tamamlandığında, coğrafi çoğaltma bağlantısı otomatik olarak sonlandırılır. Coğrafi çoğaltmanın kullanılmasıyla ilgili tüm gereksinimler veritabanı kopyalama işlemi için geçerlidir. Ayrıntılar için bkz. [etkin coğrafi Çoğaltmaya genel bakış](sql-database-active-geo-replication.md) .

> [!NOTE]
> [Otomatik veritabanı yedeklemeleri](sql-database-automated-backups.md) , bir veritabanı kopyası oluştururken kullanılır.

## <a name="logins-in-the-database-copy"></a>Veritabanı kopyasında oturum açma işlemleri

Aynı SQL veritabanı sunucusuna bir veritabanını kopyaladığınızda, her iki veritabanında da aynı oturum açma işlemleri kullanılabilir. Veritabanını kopyalamak için kullandığınız güvenlik sorumlusu yeni veritabanında veritabanı sahibi olur. 

Bir veritabanını farklı bir SQL veritabanı sunucusuna kopyaladığınızda, hedef sunucuda kopyalama işlemini başlatan güvenlik sorumlusu yeni veritabanının sahibi olur. 

Hedef sunucudan bağımsız olarak, tüm veritabanı kullanıcıları, izinleri ve güvenlik tanımlayıcıları (SID 'Ler) veritabanı kopyasına kopyalanır. Veri erişimi için [Kapsanan Veritabanı kullanıcılarının](sql-database-manage-logins.md) kullanılması, kopyalanmış veritabanının aynı kullanıcı kimlik bilgilerine sahip olmasını sağlar, böylece kopyalama tamamlandıktan sonra aynı kimlik bilgileriyle hemen erişebilirsiniz.

Veri erişimi için sunucu düzeyinde oturum açma işlemleri kullanırsanız ve veritabanını farklı bir sunucuya kopyalarsanız, oturum açma tabanlı erişim çalışmayabilir. Bu durum, oturum açma işlemleri hedef sunucuda bulunmadığı veya parolaları ve güvenlik tanımlayıcıları (SID 'Ler) farklı olduğu için meydana gelebilir. Farklı bir SQL veritabanı sunucusuna bir veritabanını kopyaladığınızda, oturum açma işlemlerini yönetme hakkında bilgi edinmek için bkz. [olağanüstü durum kurtarma sonrasında Azure SQL veritabanı güvenliğini yönetme](sql-database-geo-replication-security-config.md). Farklı bir sunucuya kopyalama işlemi başarılı olduktan sonra ve diğer kullanıcılar yeniden eşlenmeden önce, yalnızca veritabanı sahibiyle ilişkili oturum açma veya Sunucu Yöneticisi kopyalanmış veritabanında oturum açabilir. Kopyalama işlemi tamamlandıktan sonra oturum açma işlemlerini çözümlemek ve veri erişimi oluşturmak için bkz. [oturum açma bilgilerini çözümleme](#resolve-logins).

## <a name="copy-a-database-by-using-the-azure-portal"></a>Azure portal kullanarak bir veritabanını kopyalama

Azure portal kullanarak bir veritabanını kopyalamak için veritabanınızın sayfasını açın ve ardından **Kopyala**' ya tıklayın.

   ![Veritabanı kopyalama](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell-or-azure-cli"></a>PowerShell veya Azure CLı kullanarak bir veritabanını kopyalama

Bir veritabanını kopyalamak için aşağıdaki örnekleri kullanın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell için, [New-AzSqlDatabaseCopy](/powershell/module/az.sql/new-azsqldatabasecopy) cmdlet 'ini kullanın.

> [!IMPORTANT]
> PowerShell Azure Resource Manager (RM) modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. AzureRM modülü, en az Aralık 2020 ' e kadar hata düzeltmeleri almaya devam edecektir.  Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumluluklarını hakkında daha fazla bilgi için bkz. [new Azure PowerShell konusuna giriş az Module](/powershell/azure/new-azureps-module-az).

```powershell
New-AzSqlDatabaseCopy -ResourceGroupName "<resourceGroup>" -ServerName $sourceserver -DatabaseName "<databaseName>" `
    -CopyResourceGroupName "myResourceGroup" -CopyServerName $targetserver -CopyDatabaseName "CopyOfMySampleDatabase"
```

Veritabanı kopyası zaman uyumsuz bir işlemdir ancak hedef veritabanı, istek kabul edildikten hemen sonra oluşturulur. Hala devam ederken kopyalama işlemini iptal etmeniz gerekiyorsa, [Remove-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) cmdlet 'ini kullanarak hedef veritabanını bırakın.

Tüm örnek bir PowerShell betiği için bkz. [veritabanını yeni bir sunucuya kopyalama](scripts/sql-database-copy-database-to-new-server-powershell.md).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az sql db copy --dest-name "CopyOfMySampleDatabase" --dest-resource-group "myResourceGroup" --dest-server $targetserver `
    --name "<databaseName>" --resource-group "<resourceGroup>" --server $sourceserver
```

Veritabanı kopyası zaman uyumsuz bir işlemdir ancak hedef veritabanı, istek kabul edildikten hemen sonra oluşturulur. Hala devam ederken kopyalama işlemini iptal etmeniz gerekirse, [az SQL DB Delete](/cli/azure/sql/db#az-sql-db-delete) komutunu kullanarak hedef veritabanını bırakın.

* * *

## <a name="rbac-roles-to-manage-database-copy"></a>Veritabanı kopyasını yönetmek için RBAC rolleri

Bir veritabanı kopyası oluşturmak için aşağıdaki rollerde olması gerekir

- Abonelik sahibi veya
- SQL Server katkıda bulunan rolü veya
- Kaynak ve hedef veritabanlarında aşağıdaki izinle özel rol:

   Microsoft. SQL/Servers/veritabanları/Microsoft. SQL/Servers/veritabanlarını/Write 'i okuyun

Bir veritabanı kopyasını iptal etmek için, aşağıdaki rollerde olması gerekir

- Abonelik sahibi veya
- SQL Server katkıda bulunan rolü veya
- Kaynak ve hedef veritabanlarında aşağıdaki izinle özel rol:

   Microsoft. SQL/Servers/veritabanları/Microsoft. SQL/Servers/veritabanlarını/Write 'i okuyun

Veritabanı kopyasını Azure portal kullanarak yönetmek için aşağıdaki izinlere de ihtiyacınız olacaktır:

   Microsoft. resources/abonelikleri/kaynakları/Microsoft. resources/abonelikleri/kaynakları/yaz Microsoft. resources/dağıtımlar/okuma Microsoft. resources/dağıtımlar/Write Microsoft. resources/dağıtımlar/operationdurumlar/Read

Portalda kaynak grubunda bulunan ve SQL işlemleri de dahil olmak üzere birden çok kaynak sağlayıcılarındaki işlemler altında gerçekleştirilen işlemleri görmek isterseniz, bu ek RBAC rollerinin olması gerekir:

   Microsoft. resources/abonelikler/ResourceGroups/dağıtımlar/işlemler/okuma Microsoft. resources/abonelikler/ResourceGroups/dağıtımlar/operationdurumlarının/Read

## <a name="copy-a-database-by-using-transact-sql"></a>Transact-SQL kullanarak bir veritabanını kopyalama

Sunucu Yöneticisi oturum açma veya kopyalamak istediğiniz veritabanını oluşturan oturum açma ile ana veritabanında oturum açın. Veritabanı kopyalama işleminin başarılı olması için, sunucu yöneticisi olmayan oturum açma `dbmanager` rollerinin üyesi olmalıdır. Oturumlar ve sunucuya bağlanma hakkında daha fazla bilgi için bkz. [oturum açma bilgilerini yönetme](sql-database-manage-logins.md).

Kaynak veritabanını CREATE DATABASE ile kopyalamaya başla [... Deyimin KOPYASı olarak](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current#copy-a-database) . T-SQL deyimleri, veritabanı kopyalama işlemi tamamlanana kadar çalışmaya devam eder.

> [!NOTE]
> T-SQL ifadesini sonlandırmak, veritabanı kopyalama işlemini sonlandırmaz. İşlemi sonlandırmak için hedef veritabanını bırakın.
>

### <a name="copy-a-sql-database-to-the-same-server"></a>Bir SQL veritabanını aynı sunucuya kopyalama

Sunucu Yöneticisi oturum açma veya kopyalamak istediğiniz veritabanını oluşturan oturum açma ile ana veritabanında oturum açın. Veritabanı kopyalama işleminin başarılı olması için, sunucu yöneticisi olmayan oturum açma `dbmanager` rollerinin üyesi olmalıdır.

Bu komut, Veritabanı1 adlı yeni bir veritabanına aynı sunucuda Veritabanı2 kopyalar. Veritabanınızın boyutuna bağlı olarak kopyalama işleminin tamamlanması biraz zaman alabilir.

   ```sql
   -- execute on the master database to start copying
   CREATE DATABASE Database2 AS COPY OF Database1;
   ```

### <a name="copy-a-sql-database-to-a-different-server"></a>SQL veritabanını farklı bir sunucuya kopyalama

Yeni veritabanının oluşturulacağı hedef sunucunun ana veritabanında oturum açın. Kaynak sunucudaki kaynak veritabanının veritabanı sahibiyle aynı ada ve parolaya sahip bir oturum açma kullanın. Hedef sunucudaki oturum açma `dbmanager` rolünün de rolün bir üyesi olması veya Sunucu Yöneticisi oturum açması gerekir.

Bu komut Sunucu1 üzerindeki Veritabanı1 öğesini Sunucu2 üzerinde Veritabanı2 adlı yeni bir veritabanına kopyalar. Veritabanınızın boyutuna bağlı olarak kopyalama işleminin tamamlanması biraz zaman alabilir.

```sql
-- Execute on the master database of the target server (server2) to start copying from Server1 to Server2
CREATE DATABASE Database2 AS COPY OF server1.Database1;
```

> [!IMPORTANT]
> Her iki sunucu güvenlik duvarı da T-SQL CREATE VERITABANıNı veren istemcinin IP 'sinden gelen bağlantıya izin verecek şekilde yapılandırılmalıdır... Komutun KOPYASı olarak.

### <a name="copy-a-sql-database-to-a-different-subscription"></a>SQL veritabanını farklı bir aboneliğe kopyalama

Veritabanınızı, T-SQL ' i kullanarak farklı bir abonelikte SQL veritabanı sunucusuna kopyalamak için [SQL veritabanını farklı bir sunucuya kopyalama](#copy-a-sql-database-to-a-different-server) bölümündeki adımları kullanabilirsiniz. Kaynak veritabanının veritabanı sahibiyle aynı ada ve parolaya sahip bir oturum açma kullandığınızdan emin olun. Ayrıca, oturum açma, hem kaynak hem de hedef `dbmanager` sunucularda rolün bir üyesi veya bir Sunucu Yöneticisi olmalıdır.

> [!NOTE]
> [Azure Portal](https://portal.azure.com), PowerShell ve Azure CLI, farklı bir aboneliğe veritabanı kopyalamayı desteklemez.

### <a name="monitor-the-progress-of-the-copying-operation"></a>Kopyalama işleminin ilerlemesini izleme

[Sys. databases](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-databases-transact-sql), [sys. dm_database_copies](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database)ve [sys. dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) görünümlerini sorgulayarak kopyalama işlemini izleyin. Kopyalama işlemi devam ederken, yeni veritabanı için sys. databases görünümünün **state_desc** sütunu **kopyalama**olarak ayarlanır.

* Kopyalama başarısız olursa, yeni veritabanı için sys. databases görünümünün **state_desc** sütunu **şüpheli**olarak ayarlanır. DROP ifadesini yeni veritabanında yürütün ve daha sonra yeniden deneyin.
* Kopyalama başarılı olursa, yeni veritabanı için sys. databases görünümünün **state_desc** sütunu **çevrimiçi**olarak ayarlanır. Kopyalama tamamlanmıştır ve yeni veritabanı, kaynak veritabanından bağımsız olarak değiştirilebilen normal bir veritabanıdır.

> [!NOTE]
> Kopyalama işlemi devam ederken iptal etmeyi seçerseniz, yeni veritabanında [drop database](https://docs.microsoft.com/sql/t-sql/statements/drop-database-transact-sql) ifadesini yürütün.

> [!IMPORTANT]
> Kaynaktan önemli ölçüde daha küçük bir hizmet hedefine sahip bir kopya oluşturmanız gerekiyorsa, hedef veritabanı dengeli işlem işlemini tamamlayacak yeterli kaynağa sahip olmayabilir ve kopyalama işlemi başarısız olmasına neden olabilir. Bu senaryoda, farklı bir sunucuda ve/veya farklı bir bölgede bir kopya oluşturmak için bir coğrafi geri yükleme isteği kullanın. Daha fazla bilgi için bkz. [veritabanı yedeklerini kullanarak bir Azure SQL veritabanını kurtarma](sql-database-recovery-using-backups.md#geo-restore) .

## <a name="resolve-logins"></a>Oturum açma işlemlerini çözümle

Yeni veritabanı hedef sunucuda çevrimiçi olduktan sonra, kullanıcıları yeni veritabanından hedef sunucudaki oturum açmayla yeniden eşlemek için [alter User](https://docs.microsoft.com/sql/t-sql/statements/alter-user-transact-sql?view=azuresqldb-current) deyimini kullanın. Yalnız bırakılmış kullanıcıları çözümlemek için bkz. [yalnız bırakılmış kullanıcılarda sorun giderme](https://docs.microsoft.com/sql/sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server). Ayrıca bkz. [olağanüstü durum kurtarma sonrasında Azure SQL veritabanı güvenliğini yönetme](sql-database-geo-replication-security-config.md).

Yeni veritabanındaki tüm kullanıcılar, kaynak veritabanında sahip oldukları izinleri korurlar. Veritabanı kopyasını Başlatan Kullanıcı, yeni veritabanının veritabanı sahibi olur. Kopyalama başarılı olduktan ve diğer kullanıcılar yeniden eşlendikten sonra, yalnızca veritabanı sahibi yeni veritabanında oturum açabilir.

Bir veritabanını farklı bir SQL veritabanı sunucusuna kopyaladığınızda kullanıcıları ve oturum açma işlemlerini yönetme hakkında bilgi edinmek için bkz. [Azure SQL veritabanı güvenliğini olağanüstü durum kurtarma sonrasında yönetme](sql-database-geo-replication-security-config.md).

## <a name="database-copy-errors"></a>Veritabanı kopyalama hataları

Azure SQL veritabanı 'nda bir veritabanı kopyalanırken aşağıdaki hatalarla karşılaşılabilir. Daha fazla bilgi için bkz. [Azure SQL Veritabanını kopyalama](sql-database-copy.md).

| Hata kodu | Severity | Açıklama |
| ---:| ---:|:--- |
| 40635 |16 |IP adresi '%. &#x2a;ls ' olan istemci geçici olarak devre dışı. |
| 40637 |16 |Veritabanı kopyası oluşturma şu anda devre dışı. |
| 40561 |16 |Veritabanı kopyalama başarısız oldu. Kaynak ya da hedef veritabanı yok. |
| 40562 |16 |Veritabanı kopyalama başarısız oldu. Kaynak veritabanı bırakılmış. |
| 40563 |16 |Veritabanı kopyalama başarısız oldu. Hedef veritabanı bırakılmış. |
| 40564 |16 |Veritabanı kopyalama bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40565 |16 |Veritabanı kopyalama başarısız oldu. Aynı kaynaktan 1 ' den fazla eşzamanlı veritabanı kopyası yapılmasına izin verilmez. Lütfen hedef veritabanını bırakın ve daha sonra yeniden deneyin. |
| 40566 |16 |Veritabanı kopyalama bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40567 |16 |Veritabanı kopyalama bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40568 |16 |Veritabanı kopyalama başarısız oldu. Kaynak veritabanı kullanılamaz duruma geldi. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40569 |16 |Veritabanı kopyalama başarısız oldu. Hedef veritabanı kullanılamaz duruma geldi. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40570 |16 |Veritabanı kopyalama bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve daha sonra yeniden deneyin. |
| 40571 |16 |Veritabanı kopyalama bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve daha sonra yeniden deneyin. |

## <a name="next-steps"></a>Sonraki adımlar

- Oturumlar hakkında daha fazla bilgi için bkz. [oturum açma bilgilerini yönetme](sql-database-manage-logins.md) ve [olağanüstü durum kurtarma sonrasında Azure SQL veritabanı güvenliğini yönetme](sql-database-geo-replication-security-config.md).
- Bir veritabanını dışarı aktarmak için bkz. [veritabanını BACPAC 'e aktarma](sql-database-export.md).
