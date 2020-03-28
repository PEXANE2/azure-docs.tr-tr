---
title: CLI örnek- Failover grubu - Azure SQL Veritabanı yönetilen örnek
description: Azure SQL Veritabanı yönetilen bir örnek oluşturmak, bir başarısız lık grubuna eklemek ve başarısız olmayı test etmek için Azure CLI örnek komut dosyası.
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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80061856"
---
# <a name="use-cli-to-add-an-azure-sql-database-managed-instance-to-a-failover-group"></a>Bir başarısız gruba Azure SQL Veritabanı yönetilen örneği eklemek için CLI'yi kullanın

Bu Azure CLI örneği iki yönetilen örnek oluşturur, bunları bir başarısızlık grubuna ekler ve ardından birincil yönetilen örnekten ikincil yönetilen örneğe kadar başarısız testleri uygular.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-scripts"></a>Örnek komut dosyaları

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/failover-groups/add-managed-instance-to-failover-group-az-cli.sh "Add managed instance to a failover group")]

### <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın. Kaynak grubunu iki kez kaldırmanız gerekir. Kaynak grubunu ilk kez kaldırma yönetilen örnek ve sanal kümeleri kaldırır, ancak `az group delete : Long running operation failed with status 'Conflict'.`daha sonra hata iletisi ile başarısız olur. Kaynak grubunun yanı sıra kalan kaynakları kaldırmak için az grubu silme komutunu ikinci kez çalıştırın.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Örnek referans

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| | |
|---|---|
| [az ağ vnet](/cli/azure/network/vnet) | Sanal ağ komutları.  |
| [az ağ vnet subnet](/cli/azure/network/vnet/subnet) | Sanal ağ alt ağ komutları. |
| [az ağ nsg](/cli/azure/network/nsg) | Ağ güvenlik grubu komutları. |
| [az ağ nsg kuralı](/cli/azure/network/nsg/rule)| Ağ güvenliği kuralı komutları. |
| [az ağ rota tablosu](/cli/azure/network/route-table) | Rota tablosu komutları. |
| [az sql mi](/cli/azure/sql/mi) | Yönetilen örnek komutları. |
| [az ağ public-ip](/cli/azure/network/public-ip) | Ağ ortak IP adresi komutları. |
| [az ağ vnet ağ geçidi](/cli/azure/network/vnet-gateway) | Sanal Ağ Ağ Geçidi komutları |
| [az sql instance-failover-group](/cli/azure/sql/instance-failover-group) | Yönetilen örnek grup komutları üzerinden başarısız. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../sql-database-cli-samples.md) içinde bulunabilir.
