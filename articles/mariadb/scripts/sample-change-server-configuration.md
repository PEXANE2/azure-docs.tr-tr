---
title: CLı betiği-sunucu parametrelerini değiştirme-MariaDB için Azure veritabanı
description: Bu örnek CLı betiği, tüm kullanılabilir sunucu yapılandırmalarının ve MariaDB için Azure veritabanı güncelleştirmelerinin listesini listeler.
author: savjani
ms.author: pariks
ms.service: jroth
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: 41b298b60377b8f6b3ad667157f8915876a0baa8
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98664656"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>Azure CLı kullanarak MariaDB sunucusu için Azure veritabanı 'nın yapılandırmasını listeleme ve güncelleştirme
Bu örnek CLı betiği, tüm kullanılabilir yapılandırma parametrelerini ve ayrıca, MariaDB sunucusu için Azure veritabanı için izin verilen değerleri listeler ve *innodb_lock_wait_timeout* varsayılan değer dışında bir değere ayarlar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Bu makale, Azure CLı 'nin 2,0 veya sonraki bir sürümünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.

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
