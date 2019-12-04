---
title: Yavaş sorgu günlüklerine erişme-Azure CLı-MariaDB için Azure veritabanı
description: Bu makalede, Azure CLı komut satırı yardımcı programı kullanılarak MariaDB için Azure veritabanı 'ndaki yavaş günlüklere nasıl erişebileceğiniz açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 06/12/2019
ms.openlocfilehash: 32e73835732538813f90de5cb737429373c3762a
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74767390"
---
# <a name="configure-and-access-slow-query-logs-by-using-azure-cli"></a>Azure CLı kullanarak yavaş sorgu günlüklerini yapılandırma ve erişme
Azure komut satırı yardımcı programı olan Azure CLı 'yı kullanarak MariaDB yavaş sorgu günlükleri için Azure veritabanı 'nı indirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- [MariaDB sunucusu için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)
- Tarayıcıda [Azure CLI](/cli/azure/install-azure-cli) veya Azure Cloud Shell

## <a name="configure-logging-for-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı günlüğünü yapılandırma
Aşağıdaki adımları gerçekleştirerek, sunucuyu MariaDB yavaş sorgu günlüğüne erişecek şekilde yapılandırabilirsiniz:
1. **Yavaş\_sorgu\_günlük** parametresini açık olarak ayarlayarak günlüğe kaydetmeyi etkinleştirin.
2. **Uzun\_sorgu\_zaman** ve **günlük\_yavaş\_yönetici\_deyimleri**gibi diğer parametreleri ayarlayın.

Bu parametrelerin değerini Azure CLı aracılığıyla ayarlamayı öğrenmek için bkz. [sunucu parametrelerini yapılandırma](howto-configure-server-parameters-cli.md).

Örneğin, aşağıdaki CLı komutu yavaş sorgu günlüğünü açar, uzun sorgu süresini 10 saniye olarak ayarlar ve sonra yavaş yönetici bildiriminin günlüğe kaydedilmesini kapatır. Son olarak, gözden geçirmeniz için yapılandırma seçeneklerini listeler.
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mariadb server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mariadb server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure veritabanı günlüklerini listeleme
Sunucunuzun kullanılabilir yavaş sorgu günlük dosyalarını listelemek için [az MariaDB Server-Logs List](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list) komutunu çalıştırın.

Sunucu **mydemoserver.MariaDB.Database.Azure.com** için günlük dosyalarını **myresourcegroup**kaynak grubu altında listeleyebilirsiniz. Ardından günlük dosyaları listesini **günlük\_dosyaları\_List. txt**adlı bir metin dosyasına yönlendirin.
```azurecli-interactive
az mariadb server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Günlükleri sunucudan indir
[Az MariaDB Server-Logs Download](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download) komutuyla, sunucunuza ait günlük dosyalarını tek tek indirebilirsiniz.

Yerel ortamınıza **myresourcegroup** kaynak grubu altında sunucu **mydemoserver.MariaDB.Database.Azure.com** için belirli günlük dosyasını indirmek üzere aşağıdaki örneği kullanın.
```azurecli-interactive
az mariadb server-logs download --name mysql-slow-mydemoserver-2018110800.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar
- [MariaDB Için Azure veritabanı 'nda yavaş sorgu günlükleri](concepts-server-logs.md)hakkında bilgi edinin.
