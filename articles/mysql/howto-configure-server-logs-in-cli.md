---
title: Yavaş sorgu günlüklerine erişme-Azure CLı-MySQL için Azure veritabanı
description: Bu makalede, Azure CLı kullanarak MySQL için Azure veritabanı 'ndaki yavaş sorgu günlüklerine nasıl erişebileceğiniz açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 44c35d6e997b4a9a6d3dfcf3e7eba5328b125fdf
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770603"
---
# <a name="configure-and-access-slow-query-logs-by-using-azure-cli"></a>Azure CLı kullanarak yavaş sorgu günlüklerini yapılandırma ve erişme
Azure komut satırı yardımcı programı olan Azure CLı 'yı kullanarak MySQL için Azure veritabanı yavaş sorgu günlüklerini indirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- [MySQL için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-cli.md)
- Tarayıcıda [Azure CLI](/cli/azure/install-azure-cli) veya Azure Cloud Shell

## <a name="configure-logging"></a>Günlüğe kaydetmeyi yapılandırma
Aşağıdaki adımları gerçekleştirerek, sunucuyu MySQL yavaş sorgu günlüğüne erişecek şekilde yapılandırabilirsiniz:
1. **Yavaş\_sorgu\_günlük** parametresini açık olarak ayarlayarak yavaş sorgu günlüğünü açın.
2. **Uzun\_sorgu\_zaman** ve **günlük\_yavaş\_yönetici\_deyimleri**gibi diğer parametreleri ayarlayın.

Bu parametrelerin değerini Azure CLı aracılığıyla ayarlamayı öğrenmek için bkz. [sunucu parametrelerini yapılandırma](howto-configure-server-parameters-using-cli.md).

Örneğin, aşağıdaki CLı komutu yavaş sorgu günlüğünü açar, uzun sorgu süresini 10 saniye olarak ayarlar ve sonra yavaş yönetici bildiriminin günlüğe kaydedilmesini kapatır. Son olarak, gözden geçirmeniz için yapılandırma seçeneklerini listeler.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>MySQL için Azure veritabanı sunucusu için günlükleri listeleme
Sunucunuzun kullanılabilir yavaş sorgu günlük dosyalarını listelemek için [az MySQL Server-Logs List](/cli/azure/mysql/server-logs#az-mysql-server-logs-list) komutunu çalıştırın.

Sunucu **mydemoserver.MySQL.Database.Azure.com** için günlük dosyalarını **myresourcegroup**kaynak grubu altında listeleyebilirsiniz. Ardından günlük dosyaları listesini **günlük\_dosyaları\_List. txt**adlı bir metin dosyasına yönlendirin.
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Günlükleri sunucudan indir
[Az MySQL Server-Logs Download](/cli/azure/mysql/server-logs#az-mysql-server-logs-download) komutuyla, sunucunuza ait günlük dosyalarını tek tek indirebilirsiniz. 

Yerel ortamınıza **myresourcegroup** kaynak grubu altında sunucu **mydemoserver.MySQL.Database.Azure.com** için belirli günlük dosyasını indirmek üzere aşağıdaki örneği kullanın.
```azurecli-interactive
az mysql server-logs download --name 20170414-mydemoserver-mysql.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar
- [MySQL Için Azure veritabanı 'nda yavaş sorgu günlükleri](concepts-server-logs.md)hakkında bilgi edinin.
