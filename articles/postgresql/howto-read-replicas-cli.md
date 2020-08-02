---
title: Okuma çoğaltmalarını yönetme-Azure CLı, REST API-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı-Azure CLı ve REST API arasındaki okuma çoğaltmalarını yönetme hakkında bilgi edinin
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: how-to
ms.date: 07/10/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 491b3ecfc950fa5f76bfe78eec52e81433294c23
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87500087"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli-rest-api"></a>Azure CLı 'dan okuma çoğaltmaları oluşturun ve yönetin REST API

Bu makalede, Azure CLı ve REST API kullanarak PostgreSQL için Azure veritabanı 'nda okuma çoğaltmaları oluşturmayı ve yönetmeyi öğreneceksiniz. Okuma çoğaltmaları hakkında daha fazla bilgi edinmek için bkz. [genel bakış](concepts-read-replicas.md).

## <a name="azure-replication-support"></a>Azure çoğaltma desteği
[Okuma çoğaltmaları](concepts-read-replicas.md) ve [mantıksal kod çözme](concepts-logical.md) , bilgi Için doğrudan Postgres yazma günlüğüne (Wal) bağlıdır. Bu iki özellik, Postgres 'den farklı günlük düzeylerine sahip olmalıdır. Mantıksal kod çözme, okuma Çoğaltmalarından daha yüksek bir günlüğe kaydetme düzeyine sahip olmalıdır.

Doğru günlük kaydını yapılandırmak için Azure çoğaltma desteği parametresini kullanın. Azure çoğaltma desteğinin üç ayar seçeneği vardır:

* **Kapalı** -en az bilgiyi Wal 'e yerleştirir. Bu ayar, çoğu PostgreSQL için Azure veritabanı sunucuları üzerinde kullanılamaz.  
* **Çoğaltma** -daha ayrıntılı bir **şekilde.** Bu, [okuma çoğaltmalarının](concepts-read-replicas.md) çalışması için gereken en düşük günlüğe kaydetme düzeyidir. Bu ayar, çoğu sunucuda varsayılandır.
* **Çoğaltmadan**daha ayrıntılı **mantıksal** . Bu, mantıksal kod çözmenin çalışması için en düşük günlük kayıt düzeyidir. Okuma çoğaltmaları bu ayarda de çalışır.

Bu parametrenin bir değişikliğinden sonra sunucunun yeniden başlatılması gerekiyor. Dahili olarak, bu parametre Postgres parametrelerini, `wal_level` `max_replication_slots` ve ' ı ayarlar `max_wal_senders` .

## <a name="azure-cli"></a>Azure CLI
Azure CLı kullanarak okuma çoğaltmaları oluşturabilir ve yönetebilirsiniz.

### <a name="prerequisites"></a>Ön koşullar

- [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [PostgreSQL Için Azure veritabanı sunucusunun](quickstart-create-server-up-azure-cli.md) ana sunucu olması.


### <a name="prepare-the-master-server"></a>Ana sunucuyu hazırlama

1. Ana sunucunun `azure.replication_support` değerini denetleyin. Okuma çoğaltmalarının çalışması için en az çoğaltma olmalıdır.

   ```azurecli-interactive
   az postgres server configuration show --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support
   ```

2. `azure.replication_support`En az çoğaltma değilse, ayarlayın. 

   ```azurecli-interactive
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

3. Değişikliği uygulamak için sunucuyu yeniden başlatın.

   ```azurecli-interactive
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

### <a name="create-a-read-replica"></a>Okuma çoğaltması oluşturma

[Az Postgres Server Replication Create](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-create) komutu aşağıdaki parametreleri gerektirir:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group | myresourcegroup |  Çoğaltma sunucusunun oluşturulacağı kaynak grubu.  |
| name | mydemoserver-çoğaltma | Oluşturulan yeni çoğaltma sunucusunun adı. |
| source-server | mydemoserver | Çoğaltılacak var olan ana sunucunun adı veya kaynak KIMLIĞI. Çoğaltmanın ve ana sunucunun kaynak gruplarının farklı olmasını istiyorsanız kaynak KIMLIĞI ' ni kullanın. |

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

`azure.replication_support`Parametreyi genel amaçlı veya bellek Için iyileştirilmiş ana sunucuda **çoğaltma** olarak ayarlamadıysanız ve sunucuyu yeniden başlattıktan sonra bir hata alırsınız. Bir çoğaltma oluşturmadan önce bu iki adımı uygulayın.

> [!IMPORTANT]
> [Okuma çoğaltmasına genel bakış konusunun Konular bölümünü](concepts-read-replicas.md#considerations)gözden geçirin.
>
> Ana sunucu ayarı yeni bir değere güncellenmesinden önce, çoğaltma ayarını eşit veya daha büyük bir değere güncelleştirin. Bu eylem, çoğaltmanın ana üzerinde yapılan değişikliklerle devam etmesine yardımcı olur.

### <a name="list-replicas"></a>Çoğaltmaları Listele
[Az Postgres Server Replica List](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-list) komutunu kullanarak bir ana sunucunun çoğaltmalarının listesini görüntüleyebilirsiniz.

```azurecli-interactive
az postgres server replica list --server-name mydemoserver --resource-group myresourcegroup 
```

### <a name="stop-replication-to-a-replica-server"></a>Çoğaltma sunucusuna çoğaltmayı durdur
[Az Postgres Server Replication stop](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-stop) komutunu kullanarak, bir ana sunucu ve okuma çoğaltması arasındaki çoğaltmayı durdurabilirsiniz.

Bir ana sunucu ve bir okuma çoğaltması için çoğaltmayı durdurduktan sonra geri alınamaz. Okuma çoğaltması, hem okuma hem de yazma işlemlerini destekleyen tek başına bir sunucu haline gelir. Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

### <a name="delete-a-master-or-replica-server"></a>Ana veya çoğaltma sunucusunu silme
Ana veya çoğaltma sunucusunu silmek için [az Postgres Server DELETE](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-delete) komutunu kullanın.

Bir ana sunucuyu sildiğinizde, tüm okuma çoğaltmalarına çoğaltma durdurulur. Okuma çoğaltmaları artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucular haline gelir.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="rest-api"></a>REST API
[Azure REST API](/rest/api/azure/)kullanarak okuma çoğaltmaları oluşturabilir ve yönetebilirsiniz.

### <a name="prepare-the-master-server"></a>Ana sunucuyu hazırlama

1. Ana sunucunun `azure.replication_support` değerini denetleyin. Okuma çoğaltmalarının çalışması için en az çoğaltma olmalıdır.

   ```http
   GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/configurations/azure.replication_support?api-version=2017-12-01
   ```

2. `azure.replication_support`En az çoğaltma değilse, ayarlayın.

   ```http
   PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/configurations/azure.replication_support?api-version=2017-12-01
   ```

   ```JSON
   {
    "properties": {
    "value": "replica"
    }
   }
   ```

2. Değişikliği uygulamak için [sunucuyu yeniden başlatın](/rest/api/postgresql/servers/restart) .

   ```http
   POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/restart?api-version=2017-12-01
   ```

### <a name="create-a-read-replica"></a>Okuma çoğaltması oluşturma
[Oluşturma API](/rest/api/postgresql/servers/create)'sini kullanarak bir okuma çoğaltması oluşturabilirsiniz:

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{replicaName}?api-version=2017-12-01
```

```json
{
  "location": "southeastasia",
  "properties": {
    "createMode": "Replica",
    "sourceServerId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}"
  }
}
```

> [!NOTE]
> İçinde bir çoğaltma oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek için [çoğaltma kavramlarını oku makalesini](concepts-read-replicas.md)ziyaret edin. 

`azure.replication_support`Parametreyi genel amaçlı veya bellek Için iyileştirilmiş ana sunucuda **çoğaltma** olarak ayarlamadıysanız ve sunucuyu yeniden başlattıktan sonra bir hata alırsınız. Bir çoğaltma oluşturmadan önce bu iki adımı uygulayın.

Bir çoğaltma, ana öğe ile aynı işlem ve depolama ayarları kullanılarak oluşturulur. Bir çoğaltma oluşturulduktan sonra, birden fazla ayar ana sunucudan bağımsız olarak değiştirilebilir: işlem oluşturma, sanal çekirdek, depolama ve yedekleme saklama süresi. Fiyatlandırma Katmanı, temel katmandan veya dışında bağımsız olarak da değiştirilebilir.


> [!IMPORTANT]
> Ana sunucu ayarı yeni bir değere güncellenmesinden önce, çoğaltma ayarını eşit veya daha büyük bir değere güncelleştirin. Bu eylem, çoğaltmanın ana üzerinde yapılan değişikliklerle devam etmesine yardımcı olur.

### <a name="list-replicas"></a>Çoğaltmaları Listele
[Çoğaltma LISTESI API](/rest/api/postgresql/replicas/listbyserver)'sini kullanarak bir ana sunucunun çoğaltmalarının listesini görüntüleyebilirsiniz:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>Çoğaltma sunucusuna çoğaltmayı durdur
[GÜNCELLEŞTIRME API](/rest/api/postgresql/servers/update)'sini kullanarak bir ana sunucu ve okuma çoğaltması arasındaki çoğaltmayı durdurabilirsiniz.

Bir ana sunucu ve bir okuma çoğaltması için çoğaltmayı durdurduktan sonra geri alınamaz. Okuma çoğaltması, hem okuma hem de yazma işlemlerini destekleyen tek başına bir sunucu haline gelir. Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.

```http
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}?api-version=2017-12-01
```

```json
{
  "properties": {
    "replicationRole":"None"  
   }
}
```

### <a name="delete-a-master-or-replica-server"></a>Ana veya çoğaltma sunucusunu silme
Ana veya çoğaltma sunucusunu silmek için, [SILME API](/rest/api/postgresql/servers/delete)'sini kullanın:

Bir ana sunucuyu sildiğinizde, tüm okuma çoğaltmalarına çoğaltma durdurulur. Okuma çoğaltmaları artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucular haline gelir.

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{serverName}?api-version=2017-12-01
```

## <a name="next-steps"></a>Sonraki adımlar
* [PostgreSQL Için Azure veritabanı 'nda okuma çoğaltmaları](concepts-read-replicas.md)hakkında daha fazla bilgi edinin.
* [Azure Portal okuma çoğaltmaları oluşturmayı ve yönetmeyi](howto-read-replicas-portal.md)öğrenin.
