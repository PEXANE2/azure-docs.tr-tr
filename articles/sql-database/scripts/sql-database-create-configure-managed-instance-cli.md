---
title: CLI örneği - Azure SQL Veritabanı'nda yönetilen bir örnek oluşturma
description: Azure SQL Veritabanı'nda yönetilen bir örnek oluşturmak için Azure CLI örnek komut dosyası
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/25/2019
ms.openlocfilehash: 998bd8e39863dd9520d1a05e7fff52095c3be6c1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80067438"
---
# <a name="use-cli-to-create-an-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı yönetilen bir örnek oluşturmak için CLI'yi kullanın

Bu Azure CLI komut dosyası örneği, yeni bir sanal ağ içinde özel bir alt ağda yönetilen bir Azure SQL Veritabanı örneği oluşturur. Ayrıca, sanal ağ için bir rota tablosu nu ve ağ güvenlik grubunu da yapılandırır. Komut dosyası başarıyla çalıştırıldıktan sonra, yönetilen örnek sanal ağ içinden veya şirket içi ortamdan erişilebilir. Azure [SQL Veritabanı Yönetilen Örneği'ne bağlanmak için Azure VM'yi yapılandırın](../sql-database-managed-instance-configure-vm.md) ve şirket [içinden Azure SQL Veritabanı Yönetilen Örneği'ne noktadan siteye bağlantı yapılandırın.](../sql-database-managed-instance-configure-p2s.md)

> [!IMPORTANT]
> Sınırlamalar için [desteklenen bölgelere](../sql-database-managed-instance-resource-limits.md#supported-regions) ve [desteklenen abonelik türlerine](../sql-database-managed-instance-resource-limits.md#supported-subscription-types)bakın.

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

## <a name="sample-reference"></a>Örnek referans

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| | |
|---|---|
| [az ağ vnet](/cli/azure/network/vnet) | Sanal ağ komutları. |
| [az ağ vnet subnet](/cli/azure/network/vnet/subnet) | Sanal ağ alt ağ komutları. |
| [az ağ rota tablosu](/cli/azure/network/route-table) | Ağ rota tablosu komutları. |
| [az sql mi](/cli/azure/sql/mi) | Yönetilen örnek komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../sql-database-cli-samples.md) içinde bulunabilir.
