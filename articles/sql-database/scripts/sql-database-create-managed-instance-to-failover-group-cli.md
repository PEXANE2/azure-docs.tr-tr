---
title: 'Azure CLı: yük devretme grubuna yönetilen örnek ekleme'
description: Azure CLı örnek betiği Azure SQL yönetilen örneği oluşturma, bir yük devretme grubuna ekleme ve yük devretme testi.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: devx-track-azurecli
ms.devlang: azurecli
ms.topic: sample
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 07/16/2019
ms.openlocfilehash: b29fcd33c8181ba61f57d7105d08e7e4956d80bc
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87497265"
---
# <a name="use-cli-to-create-an-azure-sql-managed-instance-to-a-failover-group"></a>CLı kullanarak bir yük devretme grubuna Azure SQL yönetilen örneği oluşturma

Bu Azure CLı örneği, iki yönetilen örnek oluşturur, bunları bir yük devretme grubuna ekler ve ardından birincil yönetilen örnekten yük devretmeyi ikinci yönetilen örneğe sınar.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-scripts"></a>Örnek betikler

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

| Komut | Açıklama |
|---|---|
| [az Network VNET](/cli/azure/network/vnet) | Sanal ağ komutları.  |
| [az Network VNET subnet](/cli/azure/network/vnet/subnet) | Sanal ağ alt ağ komutları. |
| [az Network NSG](/cli/azure/network/nsg) | Ağ güvenlik grubu komutları. |
| [az Network NSG kuralı](/cli/azure/network/nsg/rule)| Ağ güvenlik kuralı komutları. |
| [az Network Route-Table](/cli/azure/network/route-table) | Rota tablosu komutları. |
| [az SQL mi](/cli/azure/sql/mi) | SQL yönetilen örnek komutları. |
| [az Network public-ip](/cli/azure/network/public-ip) | Ağ genel IP adresi komutları. |
| [az Network VNET-Gateway](/cli/azure/network/vnet-gateway) | Sanal ağ geçidi komutları |
| [az SQL Instance-yük devretme-Grup](/cli/azure/sql/instance-failover-group) | SQL yönetilen örnek yük devretme grubu komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../../azure-sql/database/az-cli-script-samples-content-guide.md) içinde bulunabilir.
