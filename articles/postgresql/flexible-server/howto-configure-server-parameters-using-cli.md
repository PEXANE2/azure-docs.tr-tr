---
title: Parametreleri yapılandırma-PostgreSQL için Azure veritabanı-esnek sunucu
description: Bu makalede, Azure CLı kullanarak PostgreSQL için Azure veritabanı-esnek sunucu 'da Postgres parametrelerinin nasıl yapılandırılacağı açıklanır.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.devlang: azurecli
ms.topic: how-to
ms.date: 9/22/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 43b14858cc99cac41e277b03171fd4cac4d6eafa
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90942127"
---
# <a name="customize-server-parameters-for-azure-database-for-postgresql---flexible-server-using-azure-cli"></a>Azure CLı kullanarak PostgreSQL için Azure veritabanı için sunucu parametrelerini özelleştirme-esnek sunucu

Komut satırı arabirimi (Azure CLı) kullanarak bir Azure PostgreSQL sunucusu için yapılandırma parametrelerini listeleyebilir, gösterebilir ve güncelleştirebilirsiniz. Altyapı parametrelerinin bir alt kümesi sunucu düzeyinde sunulur ve değiştirilebilir. 

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- PostgreSQL için [Azure veritabanı oluşturma](quickstart-create-server-cli.md) sunucusuna ve veritabanı için Azure veritabanı oluşturma
- Makinenizde [Azure CLI](/cli/azure/install-azure-cli) komut satırı arabirimi ' ni yükleyip tarayıcınızı kullanarak Azure Portal [Azure Cloud Shell](../../cloud-shell/overview.md) kullanın.

## <a name="list-server-parameters-for-a-flexible-server"></a>Esnek sunucu için sunucu parametrelerini listeleme

Bir sunucudaki tüm değiştirilebilir parametreleri ve değerlerini listelemek için [az Postgres esnek-sunucu parametre listesi](/cli/azure/postgres/flexible-server/parameter) komutunu çalıştırın.

Sunucu **mydemoserver.Postgres.Database.Azure.com** sunucu parametrelerini, **myresourcegroup**kaynak grubu altında listeleyebilirsiniz.

```azurecli-interactive
az postgres flexible-server parameter list --resource-group myresourcegroup --server-name mydemoserver
```

## <a name="show-server-parameter-details"></a>Sunucu parametresi ayrıntılarını göster

Bir sunucu için belirli bir parametre hakkındaki ayrıntıları göstermek için [az Postgres esnek-sunucu parametresi göster](/cli/azure/postgres/flexible-server/parameter)  komutunu çalıştırın.

Bu örnekte, **myresourcegroup** kaynak grubu altında sunucu **mydemoserver.Postgres.Database.Azure.com** için **günlük \_ Min \_ iletileri** sunucu parametresinin ayrıntıları gösterilmektedir.

```azurecli-interactive
az postgres flexible-server parameter show --name log_min_messages --resource-group myresourcegroup --server-name mydemoserver
```

## <a name="modify-server-parameter-value"></a>Sunucu parametre değerini değiştir

Ayrıca, PostgreSQL sunucu altyapısının temel yapılandırma değerini güncelleştiren belirli bir sunucu parametresinin değerini de değiştirebilirsiniz. Parametreyi güncelleştirmek için [az Postgres esnek-sunucu parametre kümesi](/cli/azure/postgres/flexible-server/parameter) komutunu kullanın. 

Sunucu **mydemoserver.Postgres.Database.Azure.com** 'nin **günlük \_ Min \_ iletileri** sunucu parametresini **myresourcegroup** kaynak grubu altında güncelleştirmek için.

```azurecli-interactive
az postgres flexible-server parameter set --name log_min_messages --value INFO --resource-group myresourcegroup --server-name mydemoserver
```

Bir parametrenin değerini sıfırlamak istiyorsanız, isteğe bağlı parametreyi bırakmayı seçmeniz yeterlidir `--value` ve hizmet varsayılan değeri uygular. Yukarıdaki örnekte, şöyle görünür:

```azurecli-interactive
az postgres flexible-server parameter set --name log_min_messages --resource-group myresourcegroup --server-name mydemoserver
```

Bu komut **günlük \_ Min \_ iletileri** parametresini varsayılan değer **uyarısı**olarak sıfırlar. Sunucu parametreleri ve izin verilen değerler hakkında daha fazla bilgi için bkz. [ayar parametreleri](https://www.postgresql.org/docs/12/config-setting.html)üzerinde PostgreSQL belgeleri.

## <a name="next-steps"></a>Sonraki adımlar

- Sunucu günlüklerini yapılandırmak ve erişmek için bkz. [PostgreSQL Için Azure veritabanı 'Nda sunucu günlükleri](concepts-logging.md)
