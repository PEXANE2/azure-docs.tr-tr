---
title: Parametreleri yapılandırma-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure CLı kullanarak PostgreSQL için Azure veritabanı-tek sunucu 'da Postgres parametrelerinin nasıl yapılandırılacağı açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: 4e029428a3709bacdbcd50a6ac3714e730377242
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74763632"
---
# <a name="customize-server-configuration-parameters-for-azure-database-for-postgresql---single-server-using-azure-cli"></a>Azure CLı kullanarak PostgreSQL için Azure veritabanı için sunucu yapılandırma parametrelerini özelleştirme-tek sunucu
Komut satırı arabirimi (Azure CLı) kullanarak bir Azure PostgreSQL sunucusu için yapılandırma parametrelerini listeleyebilir, gösterebilir ve güncelleştirebilirsiniz. Altyapı yapılandırmalarının bir alt kümesi sunucu düzeyinde sunulur ve değiştirilebilir. 

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- PostgreSQL için [Azure veritabanı oluşturma](quickstart-create-server-database-azure-cli.md) sunucusuna ve veritabanı için Azure veritabanı oluşturma
- Makinenizde [Azure CLI](/cli/azure/install-azure-cli) komut satırı arabirimi ' ni yükleyip tarayıcınızı kullanarak Azure Portal [Azure Cloud Shell](../cloud-shell/overview.md) kullanın.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>PostgreSQL için Azure veritabanı sunucusu için sunucu yapılandırma parametrelerini listeleyin
Bir sunucudaki tüm değiştirilebilir parametreleri ve değerlerini listelemek için [az Postgres Server Configuration List](/cli/azure/postgres/server/configuration) komutunu çalıştırın.

Sunucu **mydemoserver.Postgres.Database.Azure.com** için sunucu yapılandırma parametrelerini, **myresourcegroup**kaynak grubu altında listeleyebilirsiniz.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mydemoserver
```
## <a name="show-server-configuration-parameter-details"></a>Sunucu yapılandırma parametresi ayrıntılarını göster
Bir sunucu için belirli bir yapılandırma parametresi ayrıntılarını göstermek üzere [az Postgres Server Configuration Show](/cli/azure/postgres/server/configuration) komutunu çalıştırın.

Bu örnekte, **myresourcegroup** kaynak grubu altında sunucu **mydemoserver.postgres.database.azure.com** için **günlük\_min\_messages** sunucu yapılandırma parametresi günlüğü ayrıntıları gösterilmektedir.
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-server-configuration-parameter-value"></a>Sunucu yapılandırma parametre değerini değiştir
Ayrıca, PostgreSQL sunucu altyapısının temel yapılandırma değerini güncelleştiren belirli bir sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmayı güncelleştirmek için [az Postgres Server Configuration set](/cli/azure/postgres/server/configuration) komutunu kullanın. 

Günlük güncelleştirmek için, mydemoserver.postgres.database.azure.com kaynak grubu altında sunucu **\_min\_messages** sunucu yapılandırma parametresi **.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver --value INFO
```
Bir yapılandırma parametresinin değerini sıfırlamak istiyorsanız, isteğe bağlı `--value` parametresini bırakmayı seçmeniz yeterlidir ve hizmet varsayılan değeri uygular. Yukarıdaki örnekte, şöyle görünür:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
Bu komut, **günlük\_min\_ileti** yapılandırmasını varsayılan değer **uyarısı**olarak sıfırlar. Sunucu yapılandırması ve izin verilen değerler hakkında daha fazla bilgi için bkz. [sunucu yapılandırmasındaki](https://www.postgresql.org/docs/9.6/static/runtime-config.html)PostgreSQL belgeleri.

## <a name="next-steps"></a>Sonraki adımlar
- [Bir sunucuyu yeniden başlatmayı öğrenin](howto-restart-server-cli.md)
- Sunucu günlüklerini yapılandırmak ve erişmek için bkz. [PostgreSQL Için Azure veritabanı 'Nda sunucu günlükleri](concepts-server-logs.md)
