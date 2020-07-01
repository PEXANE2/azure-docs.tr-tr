---
title: Azure Cosmos DB-SQL (Core) API Azure PowerShell örnekleri
description: Azure Cosmos DB SQL API hesaplarında çeşitli ortak görevleri gerçekleştirmek için Azure PowerShell örnekleri alın
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 06/12/2020
ms.author: mjbrown
ms.openlocfilehash: fc4ec916e71f2fcfd3b411420879d42b2fa90f18
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85563853"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db---sql-core-api"></a>Azure Cosmos DB-SQL (Core) API Azure PowerShell örnekleri

Aşağıdaki tabloda, SQL (Core) API 'sine yönelik Azure Cosmos DB için yaygın olarak kullanılan Azure PowerShell betiklerin bağlantıları yer almaktadır. GitHub deponuzdan Cosmos DB için bu PowerShell örneklerini çatala eklemek istiyorsanız [GitHub 'da Cosmos DB PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb)' ni ziyaret edin.

SQL (Core) API 'SI ve belgelerinin ek Cosmos DB PowerShell örnekleri için bkz. [PowerShell kullanarak Azure Cosmos db SQL API kaynaklarını yönetme](manage-with-powershell.md). Diğer API 'Ler için Cosmos DB PowerShell örnekleri için bkz. [Cassandra API](powershell-samples-cassandra.md), [MongoDB API](powershell-samples-mongodb.md), [gremlin API](powershell-samples-gremlin.md)ve [tablo API'si](powershell-samples-table.md).

> [!NOTE]
> Örnekler [az. CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) Management cmdlet 'lerini kullanır. Lütfen düzenli olarak güncelleştirmeleri denetleyin `Az.CosmosDB` .

|Görev | Description |
|---|---|
|[Hesap, veritabanı ve kapsayıcı oluşturma](scripts/powershell/sql/ps-sql-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos DB bir hesap, veritabanı ve kapsayıcı oluşturun. |
|[Büyük bölüm anahtarı ile bir kapsayıcı oluşturma](scripts/powershell/sql/ps-sql-container-create-large-partition-key.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Büyük bölüm anahtarı ile bir kapsayıcı oluşturun. |
|[Veritabanları veya kapsayıcıları listeleme veya edinme](scripts/powershell/sql/ps-sql-list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Veritabanı veya kapsayıcıları listeleyin veya alın. |
|[RU/s 'yi al](scripts/powershell/sql/ps-sql-ru-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya kapsayıcı için RU/s 'yi alın. |
|[RU/s 'yi Güncelleştir](scripts/powershell/sql/ps-sql-ru-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya kapsayıcı için RU/s 'yi güncelleştirin. |
|[Dizin ilkesi olmadan kapsayıcı oluşturma](scripts/powershell/sql/ps-sql-container-create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dizin ilkesi kapalıyken bir Azure Cosmos kapsayıcısı oluşturun.|
|[Hesap güncelleştirme](scripts/powershell/common/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının varsayılan tutarlılık düzeyini güncelleştirin. |
|[Hesabın bölgelerini güncelleştirme](scripts/powershell/common/ps-account-update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının bölgelerini güncelleştirin. |
|[Yük devretme önceliğini değiştirme veya yük devretmeyi tetikleme](scripts/powershell/common/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabının bölgesel yük devretme önceliğini değiştirin veya el ile yük devretme tetikleyin. |
|[Hesap anahtarları veya bağlantı dizeleri](scripts/powershell/common/ps-account-keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Birincil ve ikincil anahtarları, bağlantı dizelerini alın veya Azure Cosmos DB hesabının hesap anahtarını yeniden oluşturun. |
|[IP güvenlik duvarı ile Cosmos hesabı oluşturma](scripts/powershell/common/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| IP Güvenlik Duvarı etkin olan bir Azure Cosmos DB hesabı oluşturun. |
|[Kaynakları silinmeye karşı kilitle](scripts/powershell/sql/powershell-sql-lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin. |
|||
