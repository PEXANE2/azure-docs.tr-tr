---
title: SQL Veritabanı için Azure CLI betik örnekleri | Microsoft Docs
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
manager: craigg
ms.date: 02/03/2019
ms.openlocfilehash: ad4706ce038feb316d238c1cabc12b27621c8085
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443342"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>Azure SQL Veritabanı için Azure CLI örnekleri

Azure SQL veritabanı, <a href="/cli/azure">Azure CLI</a>kullanılarak yapılandırılabilir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="single-database--elastic-pools"></a>Tek veritabanı & elastik havuzlar

Aşağıdaki tablo, Azure SQL Veritabanı için Azure CLI betik örneklerinin bağlantılarını içerir.

| |  |
|---|---|
|**Tek bir veritabanı ve elastik havuz oluşturma**||
| [Tek bir veritabanı oluşturma ve güvenlik duvarı kuralını yapılandırma](scripts/sql-database-create-and-configure-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bu CLI betik örneği, tek bir Azure SQL veritabanı oluşturur ve sunucu düzeyinde güvenlik duvarı kuralı yapılandırır. |
| [Elastik havuzlar oluşturma ve havuza alınan veritabanlarını taşıma](scripts/sql-database-move-database-between-pools-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bu CLI betik örneği, SQL elastik havuzları oluşturur, havuza alınmış Azure SQL veritabanlarını taşır ve işlem boyutlarını değiştirir.|
|**Tek bir veritabanını ve elastik havuzu ölçekleme**||
| [Tek bir veritabanını ölçekleme](scripts/sql-database-monitor-and-scale-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bu CLI betiği örneği, veritabanının boyut bilgilerini sorguladıktan sonra tek bir Azure SQL veritabanı örneğini farklı bir işlem boyutuna ölçeklendirir. |
| [Elastik havuzu ölçekleme](scripts/sql-database-scale-pool-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bu CLI betik örneği, SQL elastik havuzunu farklı bir işlem boyutuna ölçekler.  |
|**Yük devretme grupları**||
| [Yük devretme grubuna tek veritabanı Ekle](scripts/sql-database-add-single-db-to-failover-group-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Bu CLı betiği bir veritabanı ve bir yük devretme grubu oluşturur, veritabanını yük devretme grubuna ekler ve ikincil sunucuya yük devretmeyi sınar.|
|||

[Azure CLı apı tek veritabanı](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases)hakkında daha fazla bilgi edinin.

## <a name="managed-instance"></a>Yönetilen Örnek

Aşağıdaki tabloda Azure SQL veritabanı yönetilen örneği için Azure CLı betik örnekleri bağlantıları yer almaktadır.

| |  |
|---|---|
| [Yönetilen Örnek oluşturma](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../create-azure-sql-managed-instance-using-azure-cli/) | Bu CLı betiği yönetilen bir örnek oluşturmayı gösterir. |
| [Yönetilen örneği güncelleştirme](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../modify-azure-sql-database-managed-instance-using-azure-cli/) | Bu CLı betiği, yönetilen bir örneği nasıl güncelleştiğinizi gösterir. |
| [Veritabanını başka bir yönetilen örneğe taşıma](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) | Bu CLı betiği, bir veritabanının yedeğini bir örnekten diğerine geri yüklemeyi gösterir. |
|||

[Yönetilen örnek Azure CLı API 'si](sql-database-managed-instance-create-manage.md#azure-cli-create-and-manage-managed-instances) hakkında daha fazla bilgi edinin ve [burada ek örnekler](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)bulabilirsiniz.
