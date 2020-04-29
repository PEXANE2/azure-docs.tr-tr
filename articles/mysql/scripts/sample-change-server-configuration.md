---
title: CLı betiği-sunucu parametrelerini değiştirme-MySQL için Azure veritabanı
description: Bu örnek CLI betiği, kullanılabilir tüm sunucu yapılandırmalarını listeler ve innodb_lock_wait_timeout değerini güncelleştirir.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 12/02/2019
ms.openlocfilehash: c8781ec34cb54afc4040d858722b28e10d68bccd
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74765805"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Azure CLI kullanarak bir MySQL için Azure Veritabanı sunucusunun yapılandırmalarını listeleme ve güncelleştirme
Bu örnek CLI betiği, MySQL için Azure Veritabanı sunucusuna ait tüm kullanılabilir yapılandırma parametrelerini izin verilen değerleriyle birlikte listeler ve *innodb_lock_wait_timeout* öğesini varsayılandan farklı bir değere ayarlar.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte, vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az mysql server create](/cli/azure/mysql/server#az-mysql-server-create) | Veritabanlarını barındıran bir MySQL sunucusu oluşturur. |
| [az mysql server configuration list](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-list) | Bir MySQL için Azure Veritabanı sunucusunun yapılandırmalarını listeleyin. |
| [az mysql server configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) | Bir MySQL için Azure Veritabanı sunucusunun yapılandırmasını güncelleştirin. |
| [az mysql server configuration show](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-show) | Bir MySQL için Azure Veritabanı sunucusunun yapılandırmasını görüntüleyin. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgeleri](/cli/azure).
- Ek betikleri deneyin: [MySQL için Azure Veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- Sunucu parametreleri hakkında daha fazla bilgi için bkz. [MySQL için Azure Veritabanı’nda Sunucu Parametrelerini Yapılandırma](../howto-server-parameters.md).
