---
title: Yavaş sorgu günlüklerine erişin - Azure CLI - MariaDB için Azure Veritabanı
description: Bu makalede, Azure CLI komut satırı yardımcı programını kullanarak MariaDB için Azure Veritabanı'ndaki yavaş günlüklere nasıl erişilen anlatılmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 4/13/2020
ms.openlocfilehash: 75efdd8ed855fe78651fce5828aacb2384052ae5
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81270545"
---
# <a name="configure-and-access-slow-query-logs-by-using-azure-cli"></a>Azure CLI'yi kullanarak yavaş sorgu günlüklerini yapılandırma ve erişim
Azure komut satırı yardımcı programı Azure CLI'yi kullanarak MariaDB yavaş sorgu günlükleri için Azure Veritabanı'nı indirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [MariaDB sunucusu için Azure Veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)
- Tarayıcıdaki [Azure CLI](/cli/azure/install-azure-cli) veya Azure Bulut Kabuğu

## <a name="configure-logging"></a>Günlük işlemlerini yapılandırma
Aşağıdaki adımları atarak sunucuyu MySQL yavaş sorgu günlüğüne erişmek üzere yapılandırabilirsiniz:
1. Yavaş sorgu günlüğü parametresini AYAZMA olarak ayarlayarak yavaş **\_sorgu\_** günlemesini açın.
2. **\_Günlük çıktısını**kullanmak için günlüklerin nerede çıkarılacağını seçin. Günlükleri hem yerel depolama alanına hem de Azure MonitörÜ Tanı Günlüklerine göndermek için **Dosya'yı**seçin. Günlükleri yalnızca Azure Monitör Günlükleri'ne göndermek için **Yok'u** seçin
3. **Uzun\_sorgu\_süresi** ve günlük **\_yavaş\_yönetici\_ifadeleri**gibi diğer parametreleri ayarlayın.

Bu parametrelerin değerini Azure CLI aracılığıyla nasıl ayarlayabilirsiniz öğrenmek için [sunucu parametrelerini nasıl yapılandırılatırabilirsiniz.](howto-configure-server-parameters-cli.md)

Örneğin, aşağıdaki CLI komutu yavaş sorgu günlüğünü açar, uzun sorgu süresini 10 saniyeye ayarlar ve sonra yavaş yönetici deyiminin günlüğünü kapatır. Son olarak, incelemeniz için yapılandırma seçeneklerini listeler.
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mariadb server configuration set --name log_output --resource-group myresourcegroup --server mydemoserver --value FILE
az mariadb server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mariadb server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure Veritabanı için liste günlükleri
**log_output** "Dosya" olarak yapılandırılırsa, günlüklere doğrudan sunucunun yerel depolama sundan erişebilirsiniz. Sunucunuz için kullanılabilir yavaş sorgu günlüğü dosyalarını listelemek için [az mariadb server-logs listesi](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list) komutunu çalıştırın.

Kaynak **grubu myresourcegroup**altında sunucu **mydemoserver.mariadb.database.azure.com** için günlük dosyaları listeleyebilirsiniz. Sonra günlük dosyaları listesini **log\_files\_list.txt**adlı bir metin dosyasına yönlendirin.
```azurecli-interactive
az mariadb server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Sunucudan günlükleri indirin
**log_output** "Dosya" olarak yapılandırılırsa, [az mariadb server-logs indirme](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download) komutu ile sunucunuzdan tek tek günlük dosyalarını indirebilirsiniz.

Kaynak **grubum altında** mydemoserver.mariadb.database.azure.com **sunucunun** belirli günlük dosyasını yerel ortamınıza indirmek için aşağıdaki örneği kullanın.
```azurecli-interactive
az mariadb server-logs download --name mysql-slow-mydemoserver-2018110800.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar
- [MariaDB için Azure Veritabanı'ndaki yavaş sorgu günlükleri](concepts-server-logs.md)hakkında bilgi edinin.
