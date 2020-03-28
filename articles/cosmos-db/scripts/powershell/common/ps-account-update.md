---
title: Azure Cosmos hesabındavarsayılan tutarlılık düzeyini güncelleştirmek için PowerShell komut dosyası
description: Azure PowerShell komut dosyası örneği - PowerShell'i kullanarak Azure Cosmos DB hesabında varsayılan tutarlılık düzeyini güncelleştirme
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/21/2020
ms.author: mjbrown
ms.openlocfilehash: d3df2e91624f9b5d82d534a1d525fa6866f1a489
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366082"
---
# <a name="update-the-regions-on-an-azure-cosmos-db-account-using-powershell"></a>PowerShell'i kullanarak Azure Cosmos DB hesabındaki bölgeleri güncelleştirme

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

> [!NOTE]
> Aynı işlemdeki bölgeleri ve diğer Cosmos hesap özelliklerini değiştiremezsiniz. Bunlar iki ayrı işlem olarak yapılmalıdır.
> [!NOTE]
> Bu örnek, bir SQL API hesabı nın kullanılmasını gösterir. Bu örneği diğer API'ler için kullanmak için, ilgili özellikleri kopyalayın ve API'ye özgü komut dosyanıza uygulayın.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-update.ps1 "Update an Azure Cosmos DB account")]

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
| [Get-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbaccount) | Cosmos DB Hesaplarını listeler veya belirli bir Cosmos DB Hesabı alır. |
| [Güncelleme-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbaccountfailoverpriority) | Cosmos DB Hesabını güncelleştirin. |
|**Azure Kaynak Grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.
