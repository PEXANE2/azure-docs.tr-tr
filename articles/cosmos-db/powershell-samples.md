---
title: Azure Cosmos DB Core (SQL) API 'SI için Azure PowerShell örnekleri
description: Çekirdek (SQL) API 'SI için Azure Cosmos DB ortak görevleri gerçekleştirmek üzere Azure PowerShell örnekleri alın
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 01/20/2021
ms.author: mjbrown
ms.openlocfilehash: ab9904127d085113ba09bf6fcd3616842dade14d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102503322"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-core-sql-api"></a>Azure Cosmos DB Core (SQL) API 'SI için Azure PowerShell örnekleri
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Aşağıdaki tabloda, Azure Cosmos DB için yaygın olarak kullanılan Azure PowerShell betiklerin bağlantıları yer almaktadır. API 'ye özel örneklere gitmek için sağdaki bağlantıları kullanın. Ortak örnekler tüm API 'lerde aynıdır. Tüm Azure Cosmos DB PowerShell cmdlet 'lerinin başvuru sayfaları [Azure PowerShell başvurusunda](/powershell/module/az.cosmosdb)bulunabilir. `Az.CosmosDB`Modül artık modülün bir parçasıdır `Az` . Azure Cosmos DB cmdlet 'lerini almak için az Module ' un en son sürümünü [indirip yükleyin](/powershell/azure/install-az-ps) . [PowerShell Galerisi](https://www.powershellgallery.com/packages/Az/5.4.0)en son sürümü de edinebilirsiniz. GitHub depomızdan Cosmos DB için bu PowerShell örneklerini de çatalla Cosmos DB, [GitHub 'Da PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb)' ni kullanabilirsiniz.

Diğer API 'Ler için PowerShell cmdlet 'leri için bkz. [Cassandra Için PowerShell örnekleri](powershell-samples-cassandra.md), [MONGODB API için PowerShell](powershell-samples-mongodb.md)örnekleri, [Gremlin PowerShell](powershell-samples-gremlin.md)örnekleri, [tablo için PowerShell örnekleri](powershell-samples-table.md)

## <a name="common-samples"></a>Ortak örnekler

|Görev | Açıklama |
|---|---|
|[Hesap güncelleştirme](scripts/powershell/common/account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının varsayılan tutarlılık düzeyini güncelleştirin. |
|[Hesabın bölgelerini güncelleştirme](scripts/powershell/common/update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının bölgelerini güncelleştirin. |
|[Yük devretme önceliğini değiştirme veya yük devretmeyi tetikleme](scripts/powershell/common/failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabının bölgesel yük devretme önceliğini değiştirin veya el ile yük devretme tetikleyin. |
|[Hesap anahtarları veya bağlantı dizeleri](scripts/powershell/common/keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Birincil ve ikincil anahtarları, bağlantı dizelerini alın veya Azure Cosmos DB hesabının hesap anahtarını yeniden oluşturun. |
|[IP güvenlik duvarı ile Cosmos hesabı oluşturma](scripts/powershell/common/firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| IP Güvenlik Duvarı etkin olan bir Azure Cosmos DB hesabı oluşturun. |
|||

## <a name="core-sql-api-samples"></a>Çekirdek (SQL) API örnekleri

|Görev | Açıklama |
|---|---|
|[Hesap, veritabanı ve kapsayıcı oluşturma](scripts/powershell/sql/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos DB bir hesap, veritabanı ve kapsayıcı oluşturun. |
|[Otomatik ölçeklendirme ile hesap, veritabanı ve kapsayıcı oluşturma](scripts/powershell/sql/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Otomatik ölçeklendirme ile Azure Cosmos DB bir hesap, veritabanı ve kapsayıcı oluşturun. |
|[Büyük bölüm anahtarı ile bir kapsayıcı oluşturma](scripts/powershell/sql/create-large-partition-key.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Büyük bölüm anahtarı ile bir kapsayıcı oluşturun. |
|[Dizin ilkesi olmadan kapsayıcı oluşturma](scripts/powershell/sql/create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dizin ilkesi kapalıyken bir Azure Cosmos kapsayıcısı oluşturun.|
|[Veritabanları veya kapsayıcıları listeleme veya edinme](scripts/powershell/sql/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Veritabanı veya kapsayıcıları listeleyin veya alın. |
|[Aktarım hızı işlemleri](scripts/powershell/sql/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya kapsayıcı için, otomatik ölçeklendirme ve standart verimlilik arasında al, Güncelleştir ve geçir dahil işleme işlemleri. |
|[Kaynakları silinmeye karşı kilitle](scripts/powershell/sql/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin. |
|||
