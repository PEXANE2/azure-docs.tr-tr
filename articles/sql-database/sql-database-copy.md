---
title: Azure SQL veritabanını kopyalama | Microsoft Docs
description: Aynı sunucuda veya farklı bir sunucuda var olan bir Azure SQL veritabanının işlemsel olarak tutarlı bir kopyasını oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 06/03/2019
ms.openlocfilehash: e9cc5aaaf11a799b17cc87b40113e166fcd93afb
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569006"
---
# <a name="copy-a-transactionally-consistent-copy-of-an-azure-sql-database"></a>Azure SQL veritabanı 'nın işlem temelli tutarlı bir kopyasını kopyalama

Azure SQL veritabanı, aynı sunucuda veya farklı bir sunucuda var olan bir Azure SQL veritabanının ([tek veritabanı](sql-database-single-database.md)) işlemsel olarak tutarlı bir kopyasını oluşturmak için çeşitli yöntemler sağlar. Azure portal, PowerShell veya T-SQL ' i kullanarak bir SQL veritabanını kopyalayabilirsiniz. 

## <a name="overview"></a>Genel Bakış

Veritabanı kopyası, kopyalama isteği sırasında kaynak veritabanının anlık görüntüsüdür. Aynı sunucuyu veya farklı bir sunucuyu seçebilirsiniz. Ayrıca, hizmet katmanını ve işlem boyutunu tutmayı seçebilir veya aynı hizmet katmanı (Edition) içinde farklı bir işlem boyutu kullanabilirsiniz. Kopyalama işlemi tamamlandıktan sonra, tamamen işlevsel, bağımsız bir veritabanı haline gelir. Bu noktada, dilediğiniz sürüme yükseltebilir veya onu indirgeyede olursunuz. Oturum açmalar, kullanıcılar ve izinler bağımsız olarak yönetilebilir.  

> [!NOTE]
> [Otomatik veritabanı yedeklemeleri](sql-database-automated-backups.md) , bir veritabanı kopyası oluştururken kullanılır.

## <a name="logins-in-the-database-copy"></a>Veritabanı kopyasında oturum açma işlemleri

Aynı SQL veritabanı sunucusuna bir veritabanını kopyaladığınızda, her iki veritabanında da aynı oturum açma işlemleri kullanılabilir. Veritabanını kopyalamak için kullandığınız güvenlik sorumlusu yeni veritabanında veritabanı sahibi olur. Tüm veritabanı kullanıcıları, izinleri ve güvenlik tanımlayıcıları (SID 'Ler) veritabanı kopyasına kopyalanır.  

Bir veritabanını farklı bir SQL veritabanı sunucusuna kopyaladığınızda, yeni sunucudaki güvenlik sorumlusu yeni veritabanında veritabanı sahibi olur. Veri erişimi için [Kapsanan Veritabanı kullanıcılarını](sql-database-manage-logins.md) kullanıyorsanız, hem birincil hem de ikincil veritabanlarının her zaman aynı kullanıcı kimlik bilgilerine sahip olduğundan emin olun, böylece kopyalama tamamlandıktan sonra aynı kimlik bilgileriyle hemen erişebilirsiniz. 

[Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)kullanıyorsanız, kopyada kimlik bilgilerini yönetme gereksinimini tamamen ortadan kaldırabilirsiniz. Ancak, veritabanını yeni bir sunucuya kopyaladığınızda oturum açma tabanlı erişim çalışmayabilir, çünkü oturum açmalar yeni sunucuda mevcut değildir. Farklı bir SQL veritabanı sunucusuna bir veritabanını kopyaladığınızda, oturum açma işlemlerini yönetme hakkında bilgi edinmek için bkz. [olağanüstü durum kurtarma sonrasında Azure SQL veritabanı güvenliğini yönetme](sql-database-geo-replication-security-config.md). 

Kopyalama başarılı olduktan ve diğer kullanıcılar yeniden eşlendikten sonra, yalnızca kopyalamayı Başlatan oturum açma, veritabanı sahibi yeni veritabanında oturum açabilir. Kopyalama işlemi tamamlandıktan sonra oturum açma işlemlerini çözümlemek için bkz. [oturum açma bilgilerini çözümleme](#resolve-logins).

## <a name="copy-a-database-by-using-the-azure-portal"></a>Azure portal kullanarak bir veritabanını kopyalama

Azure portal kullanarak bir veritabanını kopyalamak için veritabanınızın sayfasını açın ve ardından **Kopyala**' ya tıklayın. 

   ![Veritabanı kopyalama](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>PowerShell kullanarak bir veritabanını kopyalama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell kullanarak bir veritabanını kopyalamak için [New-AzSqlDatabaseCopy](/powershell/module/az.sql/new-azsqldatabasecopy) cmdlet 'ini kullanın. 

```powershell
New-AzSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

Tüm örnek betik için bkz. [veritabanını yeni bir sunucuya kopyalama](scripts/sql-database-copy-database-to-new-server-powershell.md).

## <a name="copy-a-database-by-using-transact-sql"></a>Transact-SQL kullanarak bir veritabanını kopyalama

Ana veritabanında sunucu düzeyi sorumlu oturum açma veya kopyalamak istediğiniz veritabanını oluşturan oturum açma bilgileriyle oturum açın. Veritabanı kopyalama işleminin başarılı olması için, sunucu düzeyi sorumlu olmayan oturum açma işlemleri dbmanager rolünün üyesi olmalıdır. Oturumlar ve sunucuya bağlanma hakkında daha fazla bilgi için bkz. [oturum açma bilgilerini yönetme](sql-database-manage-logins.md).

[Veritabanı oluştur](https://msdn.microsoft.com/library/ms176061.aspx) ifadesiyle kaynak veritabanını kopyalamaya başlayın. Bu bildirimin yürütülmesi, veritabanı kopyalama işlemini başlatır. Bir veritabanının kopyalanması zaman uyumsuz bir işlem olduğundan, CREATE DATABASE deyimleri, veritabanı kopyalama işlemi tamamlanmadan önce döndürülür.

### <a name="copy-a-sql-database-to-the-same-server"></a>Bir SQL veritabanını aynı sunucuya kopyalama

Ana veritabanında sunucu düzeyi sorumlu oturum açma veya kopyalamak istediğiniz veritabanını oluşturan oturum açma bilgileriyle oturum açın. Veritabanı kopyalama işleminin başarılı olması için, sunucu düzeyi sorumlu olmayan oturum açma işlemleri dbmanager rolünün üyesi olmalıdır.

Bu komut, Veritabanı1 adlı yeni bir veritabanına aynı sunucuda Veritabanı2 kopyalar. Veritabanınızın boyutuna bağlı olarak kopyalama işleminin tamamlanması biraz zaman alabilir.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database2 AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>SQL veritabanını farklı bir sunucuya kopyalama

Yeni veritabanının oluşturulacağı SQL veritabanı sunucusu hedef sunucunun ana veritabanında oturum açın. Kaynak SQL veritabanı sunucusundaki kaynak veritabanının veritabanı sahibiyle aynı ada ve parolaya sahip bir oturum açma kullanın. Hedef sunucudaki oturum açma, dbmanager rolünün bir üyesi olmalıdır veya sunucu düzeyinde asıl oturum açma olmalıdır.

Bu komut Sunucu1 üzerindeki Veritabanı1 öğesini Sunucu2 üzerinde Veritabanı2 adlı yeni bir veritabanına kopyalar. Veritabanınızın boyutuna bağlı olarak kopyalama işleminin tamamlanması biraz zaman alabilir.

    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database2 AS COPY OF server1.Database1;
    
> [!IMPORTANT]
> Her iki sunucu da güvenlik duvarı, T-SQL COPY komutunu veren istemcinin IP 'sinden gelen bağlantıya izin verecek şekilde yapılandırılmalıdır.

### <a name="copy-a-sql-database-to-a-different-subscription"></a>SQL veritabanını farklı bir aboneliğe kopyalama

Veritabanınızı farklı bir abonelikte SQL veritabanı sunucusuna kopyalamak için önceki bölümde açıklanan adımları kullanabilirsiniz. Kaynak veritabanının veritabanı sahibiyle aynı ada ve parolaya sahip bir oturum açma ve dbmanager rolünün bir üyesi veya sunucu düzeyinde asıl oturum açma olduğundan emin olun. 

> [!NOTE]
> [Azure Portal](https://portal.azure.com) , Portal ARM API 'sini çağırdığından ve coğrafi çoğaltma işleminde yer alan her iki sunucuya erişmek için abonelik sertifikalarını kullandığından, farklı bir aboneliğe kopyalama desteklenmez.  

### <a name="monitor-the-progress-of-the-copying-operation"></a>Kopyalama işleminin ilerlemesini izleme

Sys. databases ve sys. DM _database_kopyalar görünümlerini sorgulayarak kopyalama işlemini izleyin. Kopyalama devam ederken, yeni veritabanı için sys. databases görünümünün **state_desc** sütunu **kopyalama**olarak ayarlanır.

* Kopyalama başarısız olursa, yeni veritabanı için sys. databases görünümünün **state_desc** sütunu **şüpheli**olarak ayarlanır. DROP ifadesini yeni veritabanında yürütün ve daha sonra yeniden deneyin.
* Kopyalama başarılı olursa, yeni veritabanı için sys. databases görünümünün **state_desc** sütunu **çevrimiçi**olarak ayarlanır. Kopyalama tamamlanmıştır ve yeni veritabanı, kaynak veritabanından bağımsız olarak değiştirilebilen normal bir veritabanıdır.

> [!NOTE]
> Kopyalama işlemi devam ederken iptal etmeyi seçerseniz, yeni veritabanında [drop database](https://msdn.microsoft.com/library/ms178613.aspx) ifadesini yürütün. Alternatif olarak, kaynak veritabanında DROP DATABASE ifadesinin yürütülmesi kopyalama işlemini de iptal eder.

## <a name="resolve-logins"></a>Oturum açma işlemlerini çözümle

Yeni veritabanı hedef sunucuda çevrimiçi olduktan sonra, kullanıcıları yeni veritabanından hedef sunucudaki oturum açmayla yeniden eşlemek için [alter User](https://msdn.microsoft.com/library/ms176060.aspx) deyimini kullanın. Yalnız bırakılmış kullanıcıları çözümlemek için bkz. [yalnız bırakılmış kullanıcılarda sorun giderme](https://msdn.microsoft.com/library/ms175475.aspx). Ayrıca bkz. [olağanüstü durum kurtarma sonrasında Azure SQL veritabanı güvenliğini yönetme](sql-database-geo-replication-security-config.md).

Yeni veritabanındaki tüm kullanıcılar, kaynak veritabanında sahip oldukları izinleri korurlar. Veritabanı kopyasını Başlatan Kullanıcı, yeni veritabanının veritabanı sahibi olur ve yeni bir güvenlik tanımlayıcısı (SID) atanır. Kopyalama başarılı olduktan ve diğer kullanıcılar yeniden eşlendikten sonra, yalnızca kopyalamayı Başlatan oturum açma, veritabanı sahibi yeni veritabanında oturum açabilir.

Bir veritabanını farklı bir SQL veritabanı sunucusuna kopyaladığınızda kullanıcıları ve oturum açma işlemlerini yönetme hakkında bilgi edinmek için bkz. [Azure SQL veritabanı güvenliğini olağanüstü durum kurtarma sonrasında yönetme](sql-database-geo-replication-security-config.md).

## <a name="next-steps"></a>Sonraki adımlar

* Oturumlar hakkında daha fazla bilgi için bkz. [oturum açma bilgilerini yönetme](sql-database-manage-logins.md) ve [olağanüstü durum kurtarma sonrasında Azure SQL veritabanı güvenliğini yönetme](sql-database-geo-replication-security-config.md).
* Bir veritabanını dışarı aktarmak için bkz. [veritabanını BACPAC 'e aktarma](sql-database-export.md).
