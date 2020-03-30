---
title: Yavaş sorgu günlüklerine erişin - Azure CLI - MariaDB için Azure Veritabanı
description: Bu makalede, Azure CLI komut satırı yardımcı programını kullanarak MariaDB için Azure Veritabanı'ndaki yavaş günlüklere nasıl erişilen anlatılmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: f33a02ff0e287c135a7d63277cf3d8d3c0cd13d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527665"
---
# <a name="configure-and-access-slow-query-logs-by-using-azure-cli"></a>Azure CLI'yi kullanarak yavaş sorgu günlüklerini yapılandırma ve erişim
Azure komut satırı yardımcı programı Azure CLI'yi kullanarak MariaDB yavaş sorgu günlükleri için Azure Veritabanı'nı indirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [MariaDB sunucusu için Azure Veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)
- Tarayıcıdaki [Azure CLI](/cli/azure/install-azure-cli) veya Azure Bulut Kabuğu

## <a name="configure-logging-for-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı için günlük yapılandırma
Aşağıdaki adımları atarak mariaDB yavaş sorgu günlüğüne erişmek için sunucu yapılandırabilirsiniz:
1. **Yavaş\_sorgu\_günlüğü** parametresini AYAZMA olarak ayarlayarak günlüğe kaydetmeyi açın.
2. **Uzun\_sorgu\_süresi** ve günlük **\_yavaş\_yönetici\_ifadeleri**gibi diğer parametreleri ayarlayın.

Bu parametrelerin değerini Azure CLI aracılığıyla nasıl ayarlayabilirsiniz öğrenmek için [sunucu parametrelerini nasıl yapılandırılatırabilirsiniz.](howto-configure-server-parameters-cli.md)

Örneğin, aşağıdaki CLI komutu yavaş sorgu günlüğünü açar, uzun sorgu süresini 10 saniyeye ayarlar ve sonra yavaş yönetici deyiminin günlüğünü kapatır. Son olarak, incelemeniz için yapılandırma seçeneklerini listeler.
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mariadb server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mariadb server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure Veritabanı için liste günlükleri
Sunucunuz için kullanılabilir yavaş sorgu günlüğü dosyalarını listelemek için [az mariadb server-logs listesi](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list) komutunu çalıştırın.

Kaynak **grubu myresourcegroup**altında sunucu **mydemoserver.mariadb.database.azure.com** için günlük dosyaları listeleyebilirsiniz. Sonra günlük dosyaları listesini **log\_files\_list.txt**adlı bir metin dosyasına yönlendirin.
```azurecli-interactive
az mariadb server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Sunucudan günlükleri indirin
Az [mariadb server-logs indirme](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download) komutu ile sunucunuz için tek tek günlük dosyalarını indirebilirsiniz.

Kaynak **grubum altında** mydemoserver.mariadb.database.azure.com **sunucunun** belirli günlük dosyasını yerel ortamınıza indirmek için aşağıdaki örneği kullanın.
```azurecli-interactive
az mariadb server-logs download --name mysql-slow-mydemoserver-2018110800.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar
- [MariaDB için Azure Veritabanı'ndaki yavaş sorgu günlükleri](concepts-server-logs.md)hakkında bilgi edinin.
