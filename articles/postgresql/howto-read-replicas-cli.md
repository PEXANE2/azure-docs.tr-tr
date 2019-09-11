---
title: PostgreSQL için Azure veritabanı için okuma çoğaltmalarını yönetme-Azure CLı 'dan tek sunucu
description: PostgreSQL için Azure veritabanı-Azure CLı 'dan tek bir sunucu için okuma çoğaltmalarını yönetme hakkında bilgi edinin.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/04/2019
ms.openlocfilehash: 5946c74d0075e04112e840d78dd9f5f57bec7475
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309391"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli"></a>Azure CLı 'dan okuma çoğaltmaları oluşturma ve yönetme

Bu makalede, Azure CLı 'dan PostgreSQL için Azure veritabanı 'nda okuma çoğaltmaları oluşturmayı ve yönetmeyi öğreneceksiniz. Okuma çoğaltmaları hakkında daha fazla bilgi edinmek için bkz. [genel bakış](concepts-read-replicas.md).

## <a name="prerequisites"></a>Önkoşullar
- [PostgreSQL Için Azure veritabanı sunucusunun](quickstart-create-server-up-azure-cli.md) ana sunucu olması.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 


## <a name="prepare-the-master-server"></a>Ana sunucuyu hazırlama
Bu adımların Genel Amaçlı veya bellek için Iyileştirilmiş katmanlarda bir ana sunucu hazırlamak için kullanılması gerekir.

Parametrenin ana sunucuda çoğaltma olarak ayarlanması gerekir. `azure.replication_support` Bu statik parametre değiştirildiğinde, değişikliğin etkili olması için sunucu yeniden başlatması gerekir.

1. Çoğaltma `azure.replication_support` olarak ayarlayın.

   ```azurecli-interactive
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

2. Değişikliği sunucuya uygulamak için yeniden başlatın.

   ```azurecli-interactive
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

## <a name="create-a-read-replica"></a>Salt okunur bir çoğaltma oluşturma

[Az Postgres Server Replication Create](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-create) komutu aşağıdaki parametreleri gerektirir:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group | myresourcegroup |  Çoğaltma sunucusunun oluşturulacağı kaynak grubu.  |
| name | mydemoserver-çoğaltma | Oluşturulan yeni çoğaltma sunucusunun adı. |
| source-server | mydemoserver | Çoğaltılacak var olan ana sunucunun adı veya kaynak KIMLIĞI. |

Aşağıdaki CLı örneğinde, çoğaltma, ana öğe ile aynı bölgede oluşturulur.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

Bir çapraz bölge okuma çoğaltması oluşturmak için `--location` parametresini kullanın. Aşağıdaki CLı örneği Batı ABD içinde çoğaltmayı oluşturur.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> İçinde bir çoğaltma oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek için [çoğaltma kavramlarını oku makalesini](concepts-read-replicas.md)ziyaret edin. 

`azure.replication_support` Parametreyi genel amaçlı veya bellek için iyileştirilmiş ana sunucuda **çoğaltma** olarak ayarlamadıysanız ve sunucuyu yeniden başlattıktan sonra bir hata alırsınız. Bir çoğaltma oluşturmadan önce bu iki adımı uygulayın.

Bir çoğaltma, ana öğe ile aynı işlem ve depolama ayarları kullanılarak oluşturulur. Bir çoğaltma oluşturulduktan sonra, birden fazla ayar ana sunucudan bağımsız olarak değiştirilebilir: işlem oluşturma, sanal çekirdek, depolama ve yedekleme saklama süresi. Fiyatlandırma Katmanı, temel katmandan veya dışında bağımsız olarak da değiştirilebilir.

> [!IMPORTANT]
> Ana sunucu ayarı yeni bir değere güncellenmesinden önce, çoğaltma ayarını eşit veya daha büyük bir değere güncelleştirin. Bu eylem, çoğaltmanın ana üzerinde yapılan değişikliklerle devam etmesine yardımcı olur.

## <a name="list-replicas"></a>Çoğaltmaları Listele
[Az Postgres Server Replica List](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-list) komutunu kullanarak bir ana sunucunun çoğaltmalarının listesini görüntüleyebilirsiniz.

```azurecli-interactive
az postgres server replica list --server-name mydemoserver --resource-group myresourcegroup 
```

## <a name="stop-replication-to-a-replica-server"></a>Bir çoğaltma sunucusu için çoğaltma durdurma
[Az Postgres Server Replication stop](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-stop) komutunu kullanarak, bir ana sunucu ve okuma çoğaltması arasındaki çoğaltmayı durdurabilirsiniz.

Bir ana sunucu ve bir okuma çoğaltması için çoğaltmayı durdurduktan sonra geri alınamaz. Okuma çoğaltması, hem okuma hem de yazma işlemlerini destekleyen tek başına bir sunucu haline gelir. Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

## <a name="delete-a-master-or-replica-server"></a>Ana veya çoğaltma sunucusunu silme
Ana veya çoğaltma sunucusunu silmek için [az Postgres Server DELETE](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-delete) komutunu kullanın.

Bir ana sunucuyu sildiğinizde, tüm okuma çoğaltmalarına çoğaltma durdurulur. Okuma çoğaltmaları artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucular haline gelir.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar
* [PostgreSQL Için Azure veritabanı 'nda okuma çoğaltmaları](concepts-read-replicas.md)hakkında daha fazla bilgi edinin.
* [Azure Portal okuma çoğaltmaları oluşturmayı ve yönetmeyi](howto-read-replicas-portal.md)öğrenin.