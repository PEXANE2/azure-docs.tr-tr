---
title: CLI komut dosyası - Ölçek sunucusu - MariaDB için Azure Veritabanı
description: Bu örnek CLI komut dosyası, ölçümleri sorguladıktan sonra MariaDB sunucusu için Azure Veritabanı'nı farklı bir performans düzeyine ölçeklendiriyor.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 12/02/2019
ms.openlocfilehash: 562f265cccf444740c177a41e516f9066188613e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74771644"
---
# <a name="monitor-and-scale-an-azure-database-for-mariadb-server-using-azure-cli"></a>Azure CLI kullanarak MariaDB sunucusu için bir Azure Veritabanını izleyin ve ölçeklendirin
Bu örnek CLI komut dosyası, ölçümleri sorguladıktan sonra MariaDB sunucusu için tek bir Azure Veritabanı için bilgi işlem ve depolama yı ölçeklendiriyor. İşlem yukarı veya aşağı ölçeklendirilebilir. Depolama yalnızca ölçeklendirilebilir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Komut dosyasını abonelik kimliğinizle güncelleştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/scale-mariadb-server.sh "Create and scale Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az mariadb sunucu oluşturmak](/cli/azure/mariadb/server#az-mariadb-server-create) | Veritabanlarını barındıran bir MariaDB sunucusu oluşturur. |
| [az mariadb sunucu güncellemesi](/cli/azure/mariadb/server#az-mariadb-server-update) | MariaDB sunucusunun özelliklerini güncelleştirir. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Kaynaklar için ölçüm değerini listeleyin. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- [MariaDB bilgi işlem ve depolama için Azure Veritabanı](../concepts-pricing-tiers.md) hakkında daha fazla bilgi edinin
- Ek komut dosyaları deneyin: [MariaDB için Azure Veritabanı için Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- [Azure CLI](/cli/azure) hakkında daha fazla bilgi edinin
