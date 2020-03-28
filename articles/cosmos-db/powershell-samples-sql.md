---
title: Azure Cosmos DB - SQL (Core) API için Azure PowerShell örnekleri
description: Azure Cosmos DB SQL API hesaplarında çeşitli ortak görevleri gerçekleştirmek için Azure PowerShell örneklerini alın
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/26/2020
ms.author: mjbrown
ms.openlocfilehash: efc0ff8e6c198071d3906a0e7e999510198f73bf
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366182"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db---sql-core-api"></a>Azure Cosmos DB - SQL (Core) API için Azure PowerShell örnekleri

Aşağıdaki tablo, SQL (Core) API için Azure Cosmos DB için sık kullanılan Azure PowerShell komut dosyalarına bağlantılar içerir. Bu PowerShell örneklerini GitHub depomuzdan Cosmos DB için çatallamak istiyorsanız, [GitHub'daki Cosmos DB PowerShell Örnekleri'ni](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb)ziyaret edin.

SQL (Core) API ve belgeleri için ek Cosmos DB PowerShell örnekleri için [PowerShell kullanarak Azure Cosmos DB SQL API kaynaklarını yönet'e](manage-with-powershell.md)bakın. Diğer API'ler için Cosmos DB PowerShell örnekleri için [Cassandra API,](powershell-samples-cassandra.md) [MongoDB API,](powershell-samples-mongodb.md) [Gremlin API](powershell-samples-gremlin.md)ve [Tablo API'ye](powershell-samples-table.md)bakın.

> [!NOTE]
> Örneklerde [Az.CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) yönetim cmdletleri kullanılmaktadır. Cmdletlerin `Az.CosmosDB` hala önizlemede olduğunu ve yayınlanmadan önce değişebileceğini lütfen unutmayın. Lütfen güncellemeleridüzenli `Az.CosmosDB` olarak kontrol edin.

| | |
|---|---|
|[Hesap, veritabanı ve kapsayıcı oluşturma](scripts/powershell/sql/ps-sql-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure Cosmos DB hesabı, veritabanı ve kapsayıcı oluşturun. |
|[Büyük bölme anahtarı olan bir kapsayıcı oluşturma](scripts/powershell/sql/ps-sql-container-create-large-partition-key.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Büyük bölme anahtarı olan bir kapsayıcı oluşturun. |
|[Veritabanlarını veya kapsayıcıları listeleyin veya alın](scripts/powershell/sql/ps-sql-list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Veritabanı veya kapsayıcıları listeleyin veya alın. |
|[RU/s alın](scripts/powershell/sql/ps-sql-ru-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya kapsayıcı için RU/s alın. |
|[RU/s'yi güncelleştir](scripts/powershell/sql/ps-sql-ru-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya kapsayıcı için RU/s'yi güncelleştirin. |
|[Dizin ilkesi olmayan bir kapsayıcı oluşturma](scripts/powershell/sql/ps-sql-container-create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dizin ilkesi kapalı bir Azure Cosmos kapsayıcısı oluşturun.|
|[Hesabı güncelleştirme](scripts/powershell/common/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının varsayılan tutarlılık düzeyini güncelleştirin. |
|[Bir hesabın bölgelerini güncelleştirme](scripts/powershell/common/ps-account-update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının bölgelerini güncelleştirin. |
|[Başarısız önceliği veya tetikleyici failover'ı değiştirme](scripts/powershell/common/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabının bölgesel hata önceliğini değiştirin veya el ile başarısızlığı tetikle. |
|[Hesap anahtarları veya bağlantı dizeleri](scripts/powershell/common/ps-account-keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Birincil ve ikincil anahtarları, bağlantı dizelerini alın veya bir Azure Cosmos DB hesabının hesap anahtarını yeniden oluşturun. |
|[IP Güvenlik Duvarı ile Cosmos Hesabı Oluşturma](scripts/powershell/common/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| IP Güvenlik Duvarı etkinleştirilmiş bir Azure Cosmos DB hesabı oluşturun. |
|||
