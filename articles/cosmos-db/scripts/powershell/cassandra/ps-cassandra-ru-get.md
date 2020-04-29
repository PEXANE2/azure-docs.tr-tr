---
title: Azure Cosmos DB Cassandra API kaynakları için üretilen Iş (RU/s) almak için PowerShell betiği
description: Azure PowerShell betiği-Azure Cosmos DB Get üretilen Iş (RU/s) Cassandra API
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: f1cae958fce1de000ee4349a2fe8229d69f1a0ae
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80366160"
---
# <a name="get-throughput-rus-for-a-keyspace-or-table-for-azure-cosmos-db---cassandra-api"></a>Azure Cosmos DB Cassandra API için anahtar uzayı veya tablo için üretilen Iş (RU/s) Al

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-ru-get.ps1 "Get throughput on a keyspace or table for Cassandra API")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
|**Azure Cosmos DB**| |
| [Get-Azcosmosdbcassandrakeyspaceverimini](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbcassandrakeyspacethroughput) | Belirtilen Cassandra API anahtar alanının üretilen iş değerini alır. |
| [Get-Azcosmosdbcassandratableverimini](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbcassandratablethroughput) | Belirtilen Cassandra API tablosunun üretilen iş değerini alır. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.