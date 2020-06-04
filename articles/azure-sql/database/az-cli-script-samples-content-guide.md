---
title: Azure CLı betik örnekleri
titleSuffix: Azure SQL Database & SQL Managed Instance
description: Azure SQL veritabanı ve Azure SQL yönetilen örneği oluşturup yönetmek için Azure CLı betik örnekleri
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc, sqldbrb=2
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 02/03/2019
ms.openlocfilehash: a17b5d80817637f1038996b034f926ee74cf54e9
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344671"
---
# <a name="azure-cli-samples-for-azure-sql-database-and-sql-managed-instance"></a>Azure SQL veritabanı ve SQL yönetilen örneği için Azure CLı örnekleri 
 
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL veritabanı ve SQL yönetilen örneğini <a href="/cli/azure">Azure CLI</a>kullanarak yapılandırabilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

# <a name="azure-sql-database"></a>[Azure SQL Veritabanı](#tab/single-database)

Aşağıdaki tabloda Azure SQL veritabanı 'nda tek ve havuza alınmış veritabanlarını yönetmek için Azure CLı betik örneklerinin bağlantıları yer almaktadır. 

| | |
|---|---|
|**Azure SQL veritabanı 'nda veritabanları oluşturma**||
| [Tek bir veritabanı oluşturma ve güvenlik duvarı kuralını yapılandırma](scripts/create-and-configure-database-cli.md) | Bir SQL veritabanı oluşturur ve sunucu düzeyinde bir güvenlik duvarı kuralı yapılandırır. |
| [Elastik havuzlar oluşturma ve havuza alınan veritabanlarını taşıma](scripts/move-database-between-elastic-pools-cli.md) | Elastik havuzlar oluşturur, havuza alınmış SQL veritabanlarını taşımaktadır ve işlem boyutlarını değiştirir. |
|**Azure SQL veritabanı 'nda veritabanlarını ölçeklendirme**||
| [Tek bir veritabanını ölçekleme](scripts/monitor-and-scale-database-cli.md) | Veritabanı için boyut bilgilerini sorguladıktan sonra SQL veritabanındaki bir veritabanını farklı bir işlem boyutuna ölçeklendirir. |
| [Elastik havuzu ölçekleme](scripts/scale-pool-cli.md) | SQL elastik havuzunu farklı bir işlem boyutuna ölçeklendirir. |
|**Coğrafi çoğaltmayı ve yük devretmeyi yapılandırma**||
| [Yük devretme grubuna tek bir veritabanı ekleme](scripts/add-database-to-failover-group-cli.md)| Bir veritabanı ve yük devretme grubu oluşturur, veritabanını yük devretme grubuna ekler ve ardından ikincil sunucuya yük devretmeyi sınar. |
| [Elastik havuz için bir yük devretme grubu yapılandırma](../../sql-database/scripts/sql-database-add-elastic-pool-to-failover-group-cli.md) | Bir veritabanı oluşturur, bir elastik havuza ekler, elastik havuzu yük devretme grubuna ekler ve ardından ikincil sunucuya yük devretmeyi sınar. |
| [Etkin coğrafi çoğaltma kullanarak tek bir veritabanını yapılandırma ve yük devretme](../../sql-database/scripts/sql-database-setup-geodr-and-failover-database-cli.md)| Bir Azure SQL veritabanı için etkin Coğrafi çoğaltmayı yapılandırır ve ikincil çoğaltmaya devreder. |
| [Etkin coğrafi çoğaltma kullanarak havuza alınmış bir veritabanını yapılandırma ve yük devretme](../../sql-database/scripts/sql-database-setup-geodr-and-failover-pool-cli.md)| SQL elastik havuzundaki bir Azure SQL veritabanı için etkin Coğrafi çoğaltmayı yapılandırır ve ikincil çoğaltmaya yük devreder. |
| **Denetim ve tehdit algılama** |
| [Denetimi ve tehdit algılamayı yapılandırma](../../sql-database/scripts/sql-database-auditing-and-threat-detection-cli.md)| Azure SQL veritabanı 'nda bir veritabanı için denetim ve tehdit algılama ilkelerini yapılandırır. |
| **Veritabanını yedekleme, geri yükleme, kopyalama ve içeri aktarma**||
| [Veritabanını yedekleme](../../sql-database/scripts/sql-database-backup-database-cli.md)| SQL veritabanında bir veritabanını bir Azure depolama yedeklemesine yedekler. |
| [Veritabanını geri yükleme](../../sql-database/scripts/sql-database-restore-database-cli.md)| SQL veritabanında bir veritabanını coğrafi olarak yedekli bir yedekten geri yükler ve silinen bir veritabanını en son yedeklemeye geri yükler. |
| [Bir veritabanını yeni bir sunucuya kopyalama](../../sql-database/scripts/sql-database-copy-database-to-new-server-cli.md) | Yeni bir sunucuda SQL veritabanı 'nda var olan veritabanının bir kopyasını oluşturur. |
| [BACPAC dosyasından veritabanını içeri aktarma](../../sql-database/scripts/sql-database-import-from-bacpac-cli.md)| BACPAC dosyasından bir veritabanını SQL veritabanına aktarır. |
|||

[Tek veritabanı Azure CLı API 'si](single-database-manage.md#the-azure-cli)hakkında daha fazla bilgi edinin.

# <a name="azure-sql-managed-instance"></a>[Azure SQL Yönetilen Örnek](#tab/managed-instance)

Aşağıdaki tabloda Azure SQL yönetilen örneği için Azure CLı betik örnekleri bağlantıları yer almaktadır.

| | |
|---|---|
| **SQL yönetilen örneği oluşturma**||
| [SQL yönetilen örneği oluşturma](../../sql-database/scripts/sql-database-create-configure-managed-instance-cli.md)| Bir SQL yönetilen örneği oluşturur. |
| **Saydam Veri Şifrelemesi yapılandırma (TDE)**||
| [Azure Key Vault kullanarak bir SQL yönetilen örneğindeki Saydam Veri Şifrelemesi yönetme](../../sql-database/scripts/transparent-data-encryption-byok-sql-managed-instance-cli.md)| Çeşitli anahtar senaryolarıyla Azure Key Vault kullanarak SQL yönetilen örneği 'nde Saydam Veri Şifrelemesi (TDE) yapılandırır. |
|**Yük devretme grubu yapılandırma**||
| [SQL yönetilen örneği için bir yük devretme grubu yapılandırma](../../sql-database/scripts/sql-database-add-managed-instance-to-failover-group-cli.md) | İki SQL yönetilen örneği örneği oluşturur, bunları bir yük devretme grubuna ekler ve ardından birincil SQL yönetilen örneğinden ikincil SQL yönetilen örneğine yük devretmeyi sınar. |
|||

Diğer SQL yönetilen örnek örnekleri için bkz. [oluşturma](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../create-azure-sql-managed-instance-using-azure-cli/), [güncelleştirme](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../modify-azure-sql-database-managed-instance-using-azure-cli/), [veritabanı taşıma](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/)ve [betiklerle çalışma](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) .

[SQL yönetilen örnek Azure CLı API 'si](../managed-instance/api-references-create-manage-instance.md#azure-cli-create-and-manage-managed-instances)hakkında daha fazla bilgi edinin.

---
