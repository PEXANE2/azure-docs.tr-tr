---
title: Azure Cosmos DB hesabı için anahtar ve bağlantı dize işlemleri almak için PowerShell komut dosyası
description: Azure PowerShell komut dosyası örneği - Azure Cosmos DB hesabı için hesap anahtarı ve bağlantı dize işlemleri
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: f6dd9d1290ea9d18fc6a5f18196585926b2ab91a
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366095"
---
# <a name="connection-string-and-account-key-operations-for-an-azure-cosmos-db-account-using-powershell"></a>PowerShell'i kullanarak bir Azure Cosmos DB hesabı için bağlantı dizesi ve hesap anahtarı işlemleri

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

> [!NOTE]
> Bu örnek, bir SQL API hesabı nın kullanılmasını gösterir. Bu örneği diğer API'ler için kullanmak için, ilgili özellikleri kopyalayın ve API'ye özgü komut dosyanıza uygulayın

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-keys-connection-strings.ps1 "Connection strings and account keys for Azure Cosmos account")]

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
| [Al-AzCosmosDBAccountKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbaccountkey) | Cosmos DB Hesabı için bağlantı dizesini veya anahtarını (salt okuma yazma veya salt okunur) alır. |
| [Yeni-AzCosmosDBAccountKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbaccountkey) | Cosmos DB Hesabı için belirtilen anahtarı yeniden oluşturun. |
|**Azure Kaynak Grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.
