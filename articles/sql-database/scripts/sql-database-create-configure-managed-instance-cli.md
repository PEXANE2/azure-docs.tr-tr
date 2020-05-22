---
title: CLı örneği-Azure SQL veritabanı 'nda yönetilen bir örnek oluşturma
description: Azure SQL veritabanı 'nda yönetilen bir örnek oluşturmak için Azure CLı örnek betiği
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/25/2019
ms.openlocfilehash: f52a460be33b23da3da0cb32075ab5f75bb8d129
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83772639"
---
# <a name="use-cli-to-create-an-azure-sql-database-managed-instance"></a>CLı kullanarak Azure SQL veritabanı yönetilen örneği oluşturma

Bu Azure CLı betik örneği, yeni bir sanal ağ içindeki ayrılmış bir alt ağda Azure SQL veritabanı yönetilen örneği oluşturur. Ayrıca, bir yol tablosu ve sanal ağ için bir ağ güvenlik grubu yapılandırır. Betik başarıyla çalıştırıldığında, yönetilen örneğe sanal ağ içinden veya şirket içi bir ortamdan erişilebilir. Azure [SQL veritabanı yönetilen örneğine bağlanmak için](../sql-database-managed-instance-configure-vm.md) bkz. Azure VM 'yi yapılandırma ve [ŞIRKET Içi Azure SQL veritabanı yönetilen örneği ile noktadan siteye bağlantı yapılandırma](../sql-database-managed-instance-configure-p2s.md).

> [!IMPORTANT]
> Sınırlamalar için bkz. [Desteklenen bölgeler](../sql-database-managed-instance-resource-limits.md#supported-regions) ve [desteklenen Abonelik türleri](../sql-database-managed-instance-resource-limits.md#supported-subscription-types).

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/managed-instance/create-managed-instance.sh "Create managed instance")]

### <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Örnek başvuru

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| | |
|---|---|
| [az Network VNET](/cli/azure/network/vnet) | Sanal ağ komutları. |
| [az Network VNET subnet](/cli/azure/network/vnet/subnet) | Sanal ağ alt ağ komutları. |
| [az Network Route-Table](/cli/azure/network/route-table) | Ağ yol tablosu komutları. |
| [az SQL mi](/cli/azure/sql/mi) | Yönetilen örnek komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../sql-database-cli-samples.md) içinde bulunabilir.
