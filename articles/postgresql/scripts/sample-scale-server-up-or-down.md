---
title: Azure CLı betiği-PostgreSQL için Azure veritabanı 'nı ölçeklendirme ve izleme
description: Örnek Azure CLI Betiği - Ölçümleri sorguladıktan sonra PostgreSQL için Azure Veritabanı sunucusunu farklı bir performans düzeyinde olacak şekilde ölçeklendirin.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.custom: mvc
ms.topic: sample
ms.date: 08/01/2019
ms.openlocfilehash: 6e1b6e5b09a3b9f3da5760fc50c531ee524dc8d4
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68728770"
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Azure CLI kullanarak tek bir PostgreSQL sunucusunu izleme ve ölçeklendirme
Bu örnek CLı betiği, ölçümleri sorguladıktan sonra, tek bir PostgreSQL için Azure veritabanı sunucusu için işlem ve depolamayı ölçeklendirir. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin. `az monitor` komutlarında kullanılan SubscriptionID değerini kendi abonelik kimliğinizle değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komutu** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az postgres server create](/cli/azure/postgres/server#az-postgres-server-create) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az Postgres Server Update](/cli/azure/postgres/server#az-postgres-server-update) | PostgreSQL sunucusunun özelliklerini güncelleştirir. |
| [az monitor metrics list](/cli/azure/monitor/metrics) | Kaynaklar için ölçüm değerini listeleyin. |
| [az group delete](/cli/azure/group) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- [PostgreSQL Için Azure veritabanı işlem ve depolama](../concepts-pricing-tiers.md) hakkında daha fazla bilgi edinin
- Ek betikleri deneyin: [PostgreSQL için Azure Veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- [Azure CLI](/cli/azure) hakkında daha fazla bilgi edinin
