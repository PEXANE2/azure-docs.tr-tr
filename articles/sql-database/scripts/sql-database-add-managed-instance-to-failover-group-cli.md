---
title: CLı örneği-yük devretme grubu-Azure SQL yönetilen örneği
description: Azure CLı örnek betiği Azure SQL yönetilen örneği oluşturma, bir yük devretme grubuna ekleme ve yük devretme testi.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 07/16/2019
ms.openlocfilehash: e9efb4e222e74f97a4f3d88639b02a86a3a6d660
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84115426"
---
# <a name="use-cli-to-add-an-azure-sql-managed-instance-to-a-failover-group"></a>CLı kullanarak bir yük devretme grubuna Azure SQL yönetilen örneği ekleme

Bu Azure CLı örneği, iki yönetilen örnek oluşturur, bunları bir yük devretme grubuna ekler ve ardından birincil yönetilen örnekten yük devretmeyi ikinci yönetilen örneğe sınar.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/failover-groups/add-managed-instance-to-failover-group-az-cli.sh "Add managed instance to a failover group")]

### <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın. Kaynak grubunu iki kez kaldırmanız gerekecektir. Kaynak grubunun ilk kez kaldırılması yönetilen örneği ve sanal kümeleri kaldırır, ancak ardından hata iletisiyle başarısız olur `az group delete : Long running operation failed with status 'Conflict'.` . Kalan kaynakları ve kaynak grubunu kaldırmak için az Group Delete komutunu ikinci kez çalıştırın.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Örnek başvuru

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| | |
|---|---|
| [az Network VNET](/cli/azure/network/vnet) | Sanal ağ komutları.  |
| [az Network VNET subnet](/cli/azure/network/vnet/subnet) | Sanal ağ alt ağ komutları. |
| [az Network NSG](/cli/azure/network/nsg) | Ağ güvenlik grubu komutları. |
| [az Network Route-Table](/cli/azure/network/route-table) | Rota tablosu komutları. |
| [az SQL mi](/cli/azure/sql/mi) | SQL yönetilen örnek komutları. |
| [az Network public-ip](/cli/azure/network/public-ip) | Ağ genel IP adresi komutları. |
| [az Network VNET-Gateway](/cli/azure/network/vnet-gateway) | Sanal ağ geçidi komutları. |
| [az SQL Instance-yük devretme-Grup](/cli/azure/sql/instance-failover-group) | SQL yönetilen örnek yük devretme grubu komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../../azure-sql/database/az-cli-script-samples-content-guide.md) içinde bulunabilir.
