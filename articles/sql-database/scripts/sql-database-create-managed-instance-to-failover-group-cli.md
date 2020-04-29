---
title: CLı örneği-yük devretme grubu-Azure SQL veritabanı yönetilen örneği
description: Azure CLı örnek betiği Azure SQL veritabanı yönetilen örneği oluşturma, bir yük devretme grubuna ekleme ve yük devretme testi.
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
ms.openlocfilehash: 792746ec3bfcf429afb7919458b9ac7ec8446b60
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80061856"
---
# <a name="use-cli-to-add-an-azure-sql-database-managed-instance-to-a-failover-group"></a>CLı kullanarak bir yük devretme grubuna Azure SQL veritabanı yönetilen örneği ekleme

Bu Azure CLı örneği, iki yönetilen örnek oluşturur, bunları bir yük devretme grubuna ekler ve ardından birincil yönetilen örnekten yük devretmeyi ikinci yönetilen örneğe sınar.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-scripts"></a>Örnek betikler

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/failover-groups/add-managed-instance-to-failover-group-az-cli.sh "Add managed instance to a failover group")]

### <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın. Kaynak grubunu iki kez kaldırmanız gerekecektir. Kaynak grubunun ilk kez kaldırılması yönetilen örneği ve sanal kümeleri kaldırır, ancak ardından hata iletisiyle `az group delete : Long running operation failed with status 'Conflict'.`başarısız olur. Kalan kaynakları ve kaynak grubunu kaldırmak için az Group Delete komutunu ikinci kez çalıştırın.

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
| [az Network NSG kuralı](/cli/azure/network/nsg/rule)| Ağ güvenlik kuralı komutları. |
| [az Network Route-Table](/cli/azure/network/route-table) | Rota tablosu komutları. |
| [az SQL mi](/cli/azure/sql/mi) | Yönetilen örnek komutları. |
| [az Network public-ip](/cli/azure/network/public-ip) | Ağ genel IP adresi komutları. |
| [az Network VNET-Gateway](/cli/azure/network/vnet-gateway) | Sanal ağ geçidi komutları |
| [az SQL Instance-yük devretme-Grup](/cli/azure/sql/instance-failover-group) | Yönetilen örnek yük devretme grubu komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../sql-database-cli-samples.md) içinde bulunabilir.
