---
title: PowerShell komut dosyası, tek bir Azure Cosmos hesabı için başarısız önceliği değiştirmek için
description: Azure PowerShell komut dosyası örneği - Azure Cosmos DB tek ana hesabı için başarısız önceliği veya tetikleyici hata yı değiştirme
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: a81938675e72d9ec3a18c920121951e38580b91e
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366117"
---
# <a name="change-failover-priority-or-trigger-failover-for-an-azure-cosmos-db-single-master-account-using-powershell"></a>PowerShell'i kullanarak Azure Cosmos DB tek ana hesabı için başarısız önceliği veya tetikleyici hatayı değiştirme

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

> [!NOTE]
> Bir bölge `failoverPriority=0` için herhangi bir değişiklik bir el ile başarısız tetikler ve sadece el ile başarısız için yapılandırılmış bir hesap yapılabilir. Diğer tüm bölgelerde yapılan değişiklikler, cosmos hesabının başarısız lık önceliğini değiştirir.
> [!NOTE]
> Bu örnek, bir SQL (Core) API hesabı nın kullanılmasını gösterir. Bu örneği diğer API'ler için kullanmak için, ilgili özellikleri kopyalayın ve API'nize özel komut dosyanıza uygulayın

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-failover-priority-update.ps1 "Update failover priority for an Azure Cosmos account or trigger a manual failover")]

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
| [Güncelleme-AzCosmosDBAccountFailoverPriority](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbaccountfailoverpriority) | Cosmos DB Hesabı'nın bölgelerinin başarısız öncelik sırasını güncelleştirin. |
|**Azure Kaynak Grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.
