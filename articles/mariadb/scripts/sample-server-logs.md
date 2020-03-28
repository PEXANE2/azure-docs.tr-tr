---
title: CLI script - Yavaş sorgu günlüklerini indirin - MariaDB için Azure Veritabanı
description: Bu örnek Azure CLI komut dosyası, MariaDB sunucusu için bir Azure Veritabanının yavaş sorgu günlüklerini nasıl etkinleştirip indireceklerini gösterir.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 12/02/2019
ms.openlocfilehash: a6d4ed70418f39b24a68362d5006f3f374693cb6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74777370"
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>Azure CLI kullanarak MariaDB sunucusu için bir Azure Veritabanının sunucu yavaş sorgu günlüklerini etkinleştirme ve indirme
Bu örnek CLI komut dosyası, MariaDB sunucusu için tek bir Azure Veritabanının yavaş sorgu günlüklerini etkinleştirer ve indirir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte, vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin. Komutlarda &lt;&gt; log_file_name kendi sunucu günlük dosya adınız ile değiştirin. `az monitor`
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/server-logs/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az mariadb sunucu oluşturmak](/cli/azure/mariadb/server#az-mariadb-server-create) | Veritabanlarını barındıran bir MariaDB sunucusu oluşturur. |
| [az mariadb sunucu yapılandırma listesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) | Bir sunucu için yapılandırma değerlerini listeleyin. |
| [az mariadb sunucu yapılandırma seti](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) | Bir sunucunun yapılandırmasını güncelleştirin. |
| [az mariadb sunucu günlükleri listesi](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list) | Bir sunucunun günlük dosyalarını listeleyin. |
| [az mariadb server-logs indir](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download) | Günlük dosyalarını indirin. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgeleri](/cli/azure).
- Ek komut dosyaları deneyin: [MariaDB için Azure Veritabanı için Azure CLI örnekleri](../sample-scripts-azure-cli.md)
