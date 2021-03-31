---
title: Azure Cosmos DB Gremlin API 'sinin Azure PowerShell örnekleri
description: Azure Cosmos DB Gremlin API 'de ortak görevleri gerçekleştirmek için Azure PowerShell örnekleri alın
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 01/20/2021
ms.author: mjbrown
ms.openlocfilehash: a0bd1c732a23ca123c391521f5895eb4dcb8c879
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102503411"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-gremlin-api"></a>Azure Cosmos DB Gremlin API 'sinin Azure PowerShell örnekleri
[!INCLUDE[appliesto-gremlin-api](includes/appliesto-gremlin-api.md)]

Aşağıdaki tabloda, Azure Cosmos DB için yaygın olarak kullanılan Azure PowerShell betiklerin bağlantıları yer almaktadır. API 'ye özel örneklere gitmek için sağdaki bağlantıları kullanın. Ortak örnekler tüm API 'lerde aynıdır. Tüm Azure Cosmos DB PowerShell cmdlet 'lerinin başvuru sayfaları [Azure PowerShell başvurusunda](/powershell/module/az.cosmosdb)bulunabilir. `Az.CosmosDB`Modül artık modülün bir parçasıdır `Az` . Azure Cosmos DB cmdlet 'lerini almak için az Module ' un en son sürümünü [indirip yükleyin](/powershell/azure/install-az-ps) . [PowerShell Galerisi](https://www.powershellgallery.com/packages/Az/5.4.0)en son sürümü de edinebilirsiniz. GitHub depomızdan Cosmos DB için bu PowerShell örneklerini de çatalla Cosmos DB, [GitHub 'Da PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb)' ni kullanabilirsiniz.

## <a name="common-samples"></a>Ortak örnekler

|Görev | Açıklama |
|---|---|
|[Hesap güncelleştirme](scripts/powershell/common/account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının varsayılan tutarlılık düzeyini güncelleştirin. |
|[Hesabın bölgelerini güncelleştirme](scripts/powershell/common/update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının bölgelerini güncelleştirin. |
|[Yük devretme önceliğini değiştirme veya yük devretmeyi tetikleme](scripts/powershell/common/failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabının bölgesel yük devretme önceliğini değiştirin veya el ile yük devretme tetikleyin. |
|[Hesap anahtarları veya bağlantı dizeleri](scripts/powershell/common/keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Birincil ve ikincil anahtarları, bağlantı dizelerini alın veya Azure Cosmos DB hesabının hesap anahtarını yeniden oluşturun. |
|[IP güvenlik duvarı ile Cosmos hesabı oluşturma](scripts/powershell/common/firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| IP Güvenlik Duvarı etkin olan bir Azure Cosmos DB hesabı oluşturun. |
|||

## <a name="gremlin-api-samples"></a>Gremlin API örnekleri

|Görev | Açıklama |
|---|---|
|[Hesap, veritabanı ve grafik oluşturma](scripts/powershell/gremlin/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabı, veritabanı ve grafik oluşturur. |
|[Otomatik ölçeklendirme ile hesap, veritabanı ve grafik oluşturma](scripts/powershell/gremlin/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Otomatik ölçeklendirme ile bir Azure Cosmos hesabı, veritabanı ve grafik oluşturur. |
|[Veritabanları veya grafikleri listeleme veya edinme](scripts/powershell/gremlin/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Veritabanını veya grafiği listeleyin veya alın. |
|[Aktarım hızı işlemleri](scripts/powershell/gremlin/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya grafiğe yönelik işleme, otomatik ölçeklendirme ve standart verimlilik arasında Get, Update ve Migrate işlemleri dahildir. |
|[Kaynakları silinmeye karşı kilitle](scripts/powershell/gremlin/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin. |
|||
