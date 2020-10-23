---
title: Azure Cosmos DB için Azure PowerShell örnekleri Tablo API'si
description: Azure Cosmos DB ortak görevleri gerçekleştirmek için Azure PowerShell örnekleri alın Tablo API'si
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/13/2020
ms.author: mjbrown
ms.openlocfilehash: d48d16d13bf35636950366cba7cd946f01afcff0
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92285118"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-table-api"></a>Azure Cosmos DB için Azure PowerShell örnekleri Tablo API'si

Aşağıdaki tabloda, Azure Cosmos DB için yaygın olarak kullanılan Azure PowerShell betiklerin bağlantıları yer almaktadır. API 'ye özel örneklere gitmek için sağdaki bağlantıları kullanın. Ortak örnekler tüm API 'lerde aynıdır. Tüm Azure Cosmos DB PowerShell cmdlet 'lerinin başvuru sayfaları [Azure PowerShell başvurusunda](/powershell/module/az.cosmosdb)bulunabilir. Lütfen düzenli olarak güncelleştirmeleri denetleyin `Az.CosmosDB` . GitHub depomızdan Cosmos DB için bu PowerShell örneklerini de çatalla Cosmos DB, [GitHub 'Da PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb)' ni kullanabilirsiniz.

## <a name="common-samples"></a>Ortak örnekler

|Görev | Açıklama |
|---|---|
|[Hesap güncelleştirme](scripts/powershell/common/account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının varsayılan tutarlılık düzeyini güncelleştirin. |
|[Hesabın bölgelerini güncelleştirme](scripts/powershell/common/update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının bölgelerini güncelleştirin. |
|[Yük devretme önceliğini değiştirme veya yük devretmeyi tetikleme](scripts/powershell/common/failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabının bölgesel yük devretme önceliğini değiştirin veya el ile yük devretme tetikleyin. |
|[Hesap anahtarları veya bağlantı dizeleri](scripts/powershell/common/keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Birincil ve ikincil anahtarları, bağlantı dizelerini alın veya Azure Cosmos DB hesabının hesap anahtarını yeniden oluşturun. |
|[IP güvenlik duvarı ile Cosmos hesabı oluşturma](scripts/powershell/common/firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| IP Güvenlik Duvarı etkin olan bir Azure Cosmos DB hesabı oluşturun. |
|||

## <a name="table-api-samples"></a>Tablo API'si örnekleri

|Görev | Açıklama |
|---|---|
|[Hesap ve tablo oluşturma](scripts/powershell/table/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure Cosmos hesabı ve tablosu oluşturur. |
|[Otomatik ölçeklendirme ile hesap ve tablo oluşturma](scripts/powershell/table/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabı ve tablo otomatik ölçeklendirmeyi oluşturur. |
|[Tabloları listeleme veya al](scripts/powershell/table/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tabloları listeleyin veya alın. |
|[Aktarım hızı işlemleri](scripts/powershell/table/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Otomatik ölçeklendirme ve standart verimlilik arasında al, Güncelleştir ve geçir dahil olmak üzere bir tablo için işleme işlemleri. |
|[Kaynakları silinmeye karşı kilitle](scripts/powershell/table/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin. |
|||
