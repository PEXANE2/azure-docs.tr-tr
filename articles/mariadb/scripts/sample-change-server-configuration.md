---
title: CLı betiği-sunucu parametrelerini değiştirme-MariaDB için Azure veritabanı
description: Bu örnek CLI betiği, kullanılabilir tüm sunucu yapılandırmalarını listeler ve innodb_lock_wait_timeout değerini güncelleştirir.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: 1425ee7b91ea1b357939c8953fdedc09df7864c6
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87502248"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>Azure CLı kullanarak MariaDB sunucusu için Azure veritabanı 'nın yapılandırmasını listeleme ve güncelleştirme
Bu örnek CLı betiği, tüm kullanılabilir yapılandırma parametrelerini ve ayrıca, MariaDB sunucusu için Azure veritabanı için izin verilen değerleri listeler ve *innodb_lock_wait_timeout* varsayılan değer dışında bir değere ayarlar.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte, vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/change-server-configurations/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az MariaDB sunucu oluştur](/cli/azure/mariadb/server#az-mariadb-server-create) | Veritabanlarını barındıran bir MariaDB sunucusu oluşturur. |
| [az MariaDB sunucu yapılandırma listesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) | MariaDB sunucusu için Azure veritabanı 'nın yapılandırmasını listeleyin. |
| [az MariaDB sunucu yapılandırma kümesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) | MariaDB sunucusu için Azure veritabanı yapılandırmasını güncelleştirin. |
| [az MariaDB sunucu yapılandırma Show](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-show) | MariaDB sunucusu için Azure veritabanı yapılandırmasını gösterir. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgeleri](/cli/azure).
- Ek betikleri deneyin: [MariaDB Için Azure veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- Sunucu parametreleri hakkında daha fazla bilgi için bkz. [MariaDB Için Azure veritabanı 'Nda sunucu parametrelerini yapılandırma](../howto-server-parameters.md).
