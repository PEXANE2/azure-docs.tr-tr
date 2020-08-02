---
title: CLı betiği-yavaş sorgu günlüklerini Indirme-MariaDB için Azure veritabanı
description: Bu örnek Azure CLı betiği, MariaDB sunucusu için Azure veritabanı 'nın yavaş sorgu günlüklerini etkinleştirme ve indirme işlemini gösterir.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: 5212f4eb17ef7b4ed1b89604fc12a1e13bae78f4
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87502197"
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>Azure CLı kullanarak MariaDB için Azure veritabanı sunucusunun yavaş sorgu günlüklerini etkinleştirme ve indirme
Bu örnek CLı betiği, MariaDB sunucusu için tek bir Azure veritabanı 'nın yavaş sorgu günlüklerini sunar ve indirir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte, vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin. &lt; &gt; `az monitor` Komutdaki Log_file_name kendi sunucu günlük dosyası adınızla değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/server-logs/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az MariaDB sunucu oluştur](/cli/azure/mariadb/server#az-mariadb-server-create) | Veritabanlarını barındıran bir MariaDB sunucusu oluşturur. |
| [az MariaDB sunucu yapılandırma listesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) | Bir sunucu için yapılandırma değerlerini listeleyin. |
| [az MariaDB sunucu yapılandırma kümesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) | Bir sunucunun yapılandırmasını güncelleştirin. |
| [az MariaDB Server-Logs List](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list) | Bir sunucunun günlük dosyalarını listeleyin. |
| [az MariaDB Server-Günlükler indir](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download) | Günlük dosyalarını indirin. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgeleri](/cli/azure).
- Ek betikleri deneyin: [MariaDB Için Azure veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
