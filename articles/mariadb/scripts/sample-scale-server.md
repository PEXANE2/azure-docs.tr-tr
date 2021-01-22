---
title: CLı betiği-ölçek sunucusu-MariaDB için Azure veritabanı
description: Bu örnek CLı betiği, ölçümleri sorguladıktan sonra MariaDB sunucusu için Azure veritabanı 'nı farklı bir performans düzeyine ölçeklendirir.
author: savjani
ms.author: pariks
ms.service: jroth
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: ac59ee30b75f4d6cab7d773b3561fcea542cb778
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98664559"
---
# <a name="monitor-and-scale-an-azure-database-for-mariadb-server-using-azure-cli"></a>Azure CLı kullanarak bir MariaDB sunucusu için Azure veritabanını izleme ve ölçeklendirme
Bu örnek CLı betiği, ölçümleri sorguladıktan sonra MariaDB sunucusu için tek bir Azure veritabanı için işlem ve depolamayı ölçeklendirir. İşlem ölçeği yukarı veya aşağı olabilir. Depolama alanı yalnızca ölçeği değiştirebilir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Bu makale, Azure CLı 'nin 2,0 veya sonraki bir sürümünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür. 

## <a name="sample-script"></a>Örnek betik
Betiği abonelik KIMLIĞINIZLE güncelleştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/scale-mariadb-server.sh "Create and scale Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az MariaDB sunucu oluştur](/cli/azure/mariadb/server#az-mariadb-server-create) | Veritabanlarını barındıran bir MariaDB sunucusu oluşturur. |
| [az MariaDB Server Update](/cli/azure/mariadb/server#az-mariadb-server-update) | MariaDB sunucusunun özelliklerini güncelleştirir. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Kaynaklar için ölçüm değerini listeleyin. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- [MariaDB işlem ve depolama Için Azure veritabanı](../concepts-pricing-tiers.md) hakkında daha fazla bilgi edinin
- Ek betikleri deneyin: [MariaDB Için Azure veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- [Azure CLI](/cli/azure) hakkında daha fazla bilgi edinin
