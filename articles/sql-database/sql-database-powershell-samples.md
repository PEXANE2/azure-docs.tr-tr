---
title: Azure PowerShell komut dosyası örnekleri
description: Azure SQL Veritabanı sunucuları, elastik havuzlar, veritabanları ve güvenlik duvarları oluşturmanıza ve yönetmenize yardımcı olacak Azure PowerShell betik örnekleri.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: dd16753a9b057e441884b0a6a019701766aaa321
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73821392"
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Azure SQL Veritabanı için Azure PowerShell örnekleri

Azure SQL Veritabanı, Azure PowerShell kullanarak veritabanlarınızı, örneklerinizi ve havuzlarınızı yapılandırmanızı sağlar.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici AZ PowerShell 1.4.0 veya daha sonra gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="single-database-and-elastic-pools"></a>[Tek Veritabanı ve Elastik havuzlar](#tab/single-database)

Aşağıdaki tablo, Azure SQL Veritabanı’na yönelik örnek Azure PowerShell betiklerinin bağlantılarını içerir.

| |  |
|---|---|
|**Tek veritabanları ve esnek havuzlar oluşturma ve yapılandırma**||
| [Tek bir veritabanı oluşturun ve veritabanı sunucusu güvenlik duvarı kuralını yapılandırma](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, tek bir Azure SQL veritabanı oluşturur ve sunucu düzeyinde güvenlik duvarı kuralı yapılandırır. |
| [Elastik havuzlar oluşturma ve havuza alınan veritabanlarını taşıma](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL Veritabanı elastik havuzları oluşturur, havuza alınmış veritabanlarını taşır ve işlem boyutlarını değiştirir.|
|**Coğrafi çoğaltmayı ve yük devretmeyi yapılandırma**||
| [Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanını yapılandırma ve tek bir veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, tek bir Azure SQL veritabanı için etkin coğrafi çoğaltmayı yapılandırır ve ikincil bir çoğaltmaya yük devreder. |
| [Etkin coğrafi çoğaltmayı kullanarak havuza alınan veritabanını yapılandırma ve havuza alınmış veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, SQL elastik havuzundaki bir Azure SQL veritabanı için etkin coğrafi çoğaltmayı yapılandırır ve ikincil bir çoğaltmaya yük devreder. |
|**Bir başarısız grubu yapılandırma**||
| [Tek bir veritabanı için bir hata grubu yapılandırma](scripts/sql-database-add-single-db-to-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell komut dosyası bir veritabanı oluşturur ve bir failover grubu, başarısız gruba veritabanı ekler ve ikincil sunucuya başarısız testleri. | 
| [Elastik bir havuz için bir başarısız grup yapılandırma](scripts/sql-database-add-elastic-pool-to-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell komut dosyası bir veritabanı oluşturur, elastik bir havuza ekler, başarısız gruba elastik havuz ekler ve ikincil sunucuya başarısız testleri. | 
|**Tek bir veritabanını ve elastik havuzu ölçekleme**||
| [Tek bir veritabanını ölçekleme](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL veritabanının performans ölçümlerini izler, veritabanını daha yüksek bir işlem boyutuna ölçekler ve performans ölçümlerinden birinde bir uyarı kuralı oluşturur. |
| [Elastik havuzu ölçekleme](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL Veritabanı elastik havuzu performans ölçümlerini izler, veritabanını daha yüksek bir işlem boyutuna ölçekler ve performans ölçümlerinden birinde bir uyarı kuralı oluşturur. |
| **Denetim ve tehdit algılama** |
| [Denetimi ve tehdit algılamayı yapılandırma](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, Azure SQL veritabanı için denetim ve tehdit algılama ilkelerini yapılandırır. |
| **Veritabanını geri yükleme, kopyalama ve içeri aktarma**||
| [Veritabanını geri yükleme](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, coğrafi olarak yedekli bir yedeklemeden Azure SQL veritabanını geri yükler ve silinmiş bir Azure SQL veritabanını en son yedeklemeye geri yükler. |
| [Bir veritabanını yeni bir sunucuya kopyalama](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, yeni Azure SQL sunucusunda mevcut bir Azure SQL veritabanının kopyasını oluşturur. |
| [Bir bacpac dosyasından veritabanı alma](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği bir veritabanını bacpac dosyasından Azure SQL sunucusuna içeri aktarır. |
| **Veritabanları arasında verileri eşitleme**||
| [SQL veritabanları arasında verileri eşitleme](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Veri Eşitleme’yi birden çok Azure SQL veritabanı arasında eşitlenecek şekilde yapılandırır. |
| [SQL Veritabanı ile şirket içi SQL Server arasında verileri eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, bir Azure SQL veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme yapmak için Veri Eşitleme’yi yapılandırır. |
| [SQL Data Sync için eşitleme şemasını güncelleştirme](scripts/sql-database-sync-update-schema.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Veri Eşitleme için eşitleme şemasından öğeler ekler veya öğeleri kaldırır. |
|||

Tek Veritabanı [Azure PowerShell API](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases)hakkında daha fazla bilgi edinin.

## <a name="managed-instance"></a>[Yönetilen Örnek](#tab/managed-instance)

Aşağıdaki tablo, Azure SQL Veritabanı - Yönetilen Örnek için örnek Azure PowerShell komut dosyalarına bağlantılar içerir.

| |  |
|---|---|
|**Yönetilen örnekleri oluşturma ve yapılandırma**||
| [Yönetilen Örnek oluşturma ve yönetme](scripts/sql-database-create-configure-managed-instance-powershell.md) | Bu PowerShell betiği, Azure PowerShell kullanarak Yönetilen Örnek oluşturmayı ve yönetmeyi göstermektedir |
| [Azure Kaynak Yöneticisi şablonu kullanarak Yönetilen Örnek oluşturma ve yönetme](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell komut dosyası, Azure PowerShell ve Azure Kaynak Yöneticisi şablonu kullanarak Yönetilen Örnek'i nasıl oluşturabileceğinizi ve yönetdiğinizi gösterir.|
| [Veritabanını başka bir Coğrafi bölgede Yönetilen Örnek'e geri yükleme](scripts/sql-managed-instance-restore-geo-backup.md) | Bu PowerShell komut dosyası, bir veritabanının yedeğini alıyor ve başka bir bölgeye geri yükleniyor. Bu, Coğrafi Geri Yükleme olağanüstü durum kurtarma senaryosu olarak bilinir. |
| **Saydam Veri Şifreleme (TDE) Yapılandırma**||
| [Azure Anahtar Kasası'ndan kendi anahtarınızı kullanarak Yönetilen Örnekte Saydam Veri Şifrelemeyi Yönetin](scripts/transparent-data-encryption-byok-sql-managed-instance-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell komut dosyası, Azure Sql Yönetilen Örneği için Kendi AnahtarGetir senaryosunda Azure Key Vault'tan bir anahtar kullanarak Saydam Veri Şifrelemesini (TDE) yapılandırır|
|**Bir başarısız grubu yapılandırma**||
| [Yönetilen bir örnek için bir başarısız grup yapılandırma](scripts/sql-database-add-managed-instance-to-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell komut dosyası iki yönetilen örnek oluşturur, bunları bir başarısızlık grubuna ekler ve ardından birincil yönetilen örnekten ikincil yönetilen örne kadar başarısız testleri uygular. | 
|||

Yönetilen Örnek [Azure PowerShell API](sql-database-managed-instance-create-manage.md#powershell-create-and-manage-managed-instances)hakkında daha fazla bilgi edinin.

---

## <a name="additional-resources"></a>Ek kaynaklar

Bu sayfada listelenen örnekler, Azure SQL kaynaklarını oluşturmak ve yönetmek için [Azure SQL Veritabanı cmdlets'i](/powershell/module/az.sql/) kullanır. Sorguları çalıştırmak ve birçok veritabanı görevini gerçekleştirmek için ek cmdletler [sqlserver](/powershell/module/sqlserver/) modülünde bulunur. Daha fazla bilgi için [SQL Server PowerShell'e](/sql/powershell/sql-server-powershell/)bakın.
