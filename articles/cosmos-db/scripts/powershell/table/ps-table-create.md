---
title: Azure Cosmos DB Tablo API'si bir tablo oluşturmak için PowerShell betiği
description: Bir veritabanı veya Azure Cosmos DB bir kapsayıcı için üretilen işi güncelleştirmek üzere bir PowerShell betiği kullanmayı öğrenin Tablo API'si
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: 9ea03996c793c3d53e4a3657c537f9354892c647
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80365594"
---
# <a name="create-a-table-for-azure-cosmos-db---table-api"></a>Azure Cosmos DB Tablo API'si için tablo oluşturma

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/table/ps-table-create.ps1 "Create a table for Table API")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
|**Azure kaynakları**| |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | Bir kaynak oluşturur. |
|**Azure Cosmos DB**| |
| [Set-AzCosmosDBTable](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbtable) | Bir Cosmos DB Tablo API'si tablosu oluşturur veya güncelleştirir. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.