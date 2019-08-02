---
title: Azure CLı betiği-MariaDB sunucusu için Azure veritabanı ölçeklendirme
description: Bu örnek CLı betiği, ölçümleri sorguladıktan sonra MariaDB sunucusu için Azure veritabanı 'nı farklı bir performans düzeyine ölçeklendirir.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 08/01/2019
ms.openlocfilehash: 99f85abe0c8f08bedb3e9808d2b740d4b7842257
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68720355"
---
# <a name="monitor-and-scale-an-azure-database-for-mariadb-server-using-azure-cli"></a>Azure CLı kullanarak bir MariaDB sunucusu için Azure veritabanını izleme ve ölçeklendirme
Bu örnek CLı betiği, ölçümleri sorguladıktan sonra MariaDB sunucusu için tek bir Azure veritabanı için işlem ve depolamayı ölçeklendirir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin. `az monitor` Komutlarında kullanılan abonelik kimliğini kendi abonelik Kimliğinizle değiştirin.  
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/scale-mariadb-server.sh?highlight=15-16 "Create and scale Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komutu** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az MariaDB sunucu oluştur](/cli/azure/mariadb/server#az-mariadb-server-create) | Veritabanlarını barındıran bir MariaDB sunucusu oluşturur. |
| [az MariaDB Server Update](/cli/azure/mariadb/server#az-mariadb-server-update) | MariaDB sunucusunun özelliklerini güncelleştirir. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Kaynaklar için ölçüm değerini listeleyin. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- [MariaDB işlem ve depolama Için Azure veritabanı](../concepts-pricing-tiers.md) hakkında daha fazla bilgi edinin
- Ek betikleri deneyin: [MariaDB için Azure veritabanı Azure CLı örnekleri](../sample-scripts-azure-cli.md)
- [Azure CLI](/cli/azure) hakkında daha fazla bilgi edinin
