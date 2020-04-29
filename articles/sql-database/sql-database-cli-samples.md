---
title: Azure CLı betik örnekleri
description: Azure SQL Veritabanı sunucuları, elastik havuzlar, veritabanları ve güvenlik duvarları oluşturmak ve yönetmek için Azure CLI betik örnekleri.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 02/03/2019
ms.openlocfilehash: 459a5ea69e11a8614c572f68fce039b6cabbb1ed
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80061709"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>Azure SQL Veritabanı için Azure CLI örnekleri

Azure SQL veritabanı, <a href="/cli/azure">Azure CLI</a>kullanılarak yapılandırılabilir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

# <a name="single-database--elastic-pools"></a>[Tek veritabanı & elastik havuzlar](#tab/single-database)

| | |
|---|---|
|**Tek bir veritabanı ve elastik havuz oluşturma**||
| [Tek bir veritabanı oluşturma ve güvenlik duvarı kuralını yapılandırma](scripts/sql-database-create-and-configure-database-cli.md) | Bir Azure SQL veritabanı oluşturur ve sunucu düzeyinde bir güvenlik duvarı kuralı yapılandırır. |
| [Elastik havuzlar oluşturma ve havuza alınan veritabanlarını taşıma](scripts/sql-database-move-database-between-pools-cli.md) | SQL elastik havuzları oluşturur, havuza alınmış Azure SQL veritabanlarını taşımaktadır ve işlem boyutlarını değiştirir. |
|**Tek bir veritabanını ve elastik havuzu ölçekleme**||
| [Tek bir veritabanını ölçekleme](scripts/sql-database-monitor-and-scale-database-cli.md) | Veritabanı için boyut bilgilerini sorguladıktan sonra bir Azure SQL veritabanını farklı bir işlem boyutuna ölçeklendirir. |
| [Elastik havuzu ölçekleme](scripts/sql-database-scale-pool-cli.md) | SQL elastik havuzunu farklı bir işlem boyutuna ölçeklendirir. |
|**Coğrafi çoğaltmayı ve yük devretmeyi yapılandırma**||
| [Yük devretme grubuna tek veritabanı Ekle](scripts/sql-database-add-single-db-to-failover-group-cli.md)| Bir veritabanı ve yük devretme grubu oluşturur, veritabanını yük devretme grubuna ekler ve ardından ikincil sunucuya yük devretmeyi sınar. |
| [Elastik havuz için bir yük devretme grubu yapılandırma](scripts/sql-database-add-elastic-pool-to-failover-group-cli.md) | Bir veritabanı oluşturur, bir elastik havuza ekler, elastik havuzu yük devretme grubuna ekler ve ardından ikincil sunucuya yük devretmeyi sınar. |
| [Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanını yapılandırma ve tek bir veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-database-cli.md)| Bir Azure SQL veritabanı için etkin Coğrafi çoğaltmayı yapılandırır ve ikincil çoğaltmaya devreder. |
| [Etkin coğrafi çoğaltmayı kullanarak havuza alınan veritabanını yapılandırma ve havuza alınmış veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-pool-cli.md)| SQL elastik havuzundaki bir Azure SQL veritabanı için etkin Coğrafi çoğaltmayı yapılandırır ve ikincil çoğaltmaya yük devreder. |
| **Denetim ve tehdit algılama** |
| [Denetimi ve tehdit algılamayı yapılandırma](scripts/sql-database-auditing-and-threat-detection-cli.md)| Azure SQL veritabanı için denetim ve tehdit algılama ilkelerini yapılandırır. |
| **Veritabanını yedekleme, geri yükleme, kopyalama ve içeri aktarma**||
| [Veritabanını yedekleme](scripts/sql-database-backup-database-cli.md)| Bir Azure SQL veritabanını bir Azure depolama yedeklemesine yedekler. |
| [Veritabanını geri yükleme](scripts/sql-database-restore-database-cli.md)| Coğrafi olarak yedekli bir yedeklemeden Azure SQL veritabanını geri yükler ve silinen bir Azure SQL veritabanını en son yedeklemeye geri yükler. |
| [Bir veritabanını yeni bir sunucuya kopyalama](scripts/sql-database-copy-database-to-new-server-cli.md) | Yeni bir Azure SQL Server 'da mevcut bir Azure SQL veritabanının kopyasını oluşturur. |
| [Bacpac dosyasından bir veritabanını içeri aktarma](scripts/sql-database-import-from-bacpac-cli.md)| Bir veritabanını bir *. bacpac* DOSYASıNDAN Azure SQL Server 'a aktarır. |
|||

[Tek veritabanı Azure CLı API 'si](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases)hakkında daha fazla bilgi edinin.

# <a name="managed-instance"></a>[Yönetilen örnek](#tab/managed-instance)

Aşağıdaki tabloda Azure SQL veritabanı yönetilen örneği için Azure CLı betik örnekleri bağlantıları yer almaktadır.

| | |
|---|---|
| **Yönetilen Örnek oluşturma**||
| [Yönetilen örnek oluşturma](scripts/sql-database-create-configure-managed-instance-cli.md)| Yönetilen bir örnek oluşturur. |
| **Saydam Veri Şifrelemesi yapılandırma (TDE)**||
| [Azure Key Vault kullanarak yönetilen bir örnekteki Saydam Veri Şifrelemesi yönetme](scripts/transparent-data-encryption-byok-sql-managed-instance-cli.md)| Çeşitli anahtar senaryolarıyla Azure Key Vault kullanarak Azure SQL yönetilen örneği 'nde Saydam Veri Şifrelemesi (TDE) yapılandırır. |
|**Yük devretme grubu yapılandırma**||
| [Yönetilen örnek için bir yük devretme grubu yapılandırma](scripts/sql-database-add-managed-instance-to-failover-group-cli.md) | İki yönetilen örnek oluşturur, bunları bir yük devretme grubuna ekler, ardından birincil yönetilen örnekten yük devretmeyi ikinci yönetilen örneğe sınar. |
|||

Ek yönetilen örnek örnekleri için bkz. [oluşturma](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../create-azure-sql-managed-instance-using-azure-cli/), [güncelleştirme](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../modify-azure-sql-database-managed-instance-using-azure-cli/), [veritabanı taşıma](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/), [betiklerle çalışma](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) .

[Yönetilen örnek Azure CLı API 'si](sql-database-managed-instance-create-manage.md#azure-cli-create-and-manage-managed-instances)hakkında daha fazla bilgi edinin.

---
