---
title: Okuma yinelemelerini yönetme - Azure CLI, REST API - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Azure CLI ve REST API'den PostgreSQL - Single Server için Azure Veritabanı'ndaki okuma kopyalarını nasıl yöneteceğimiz öğrenin
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/23/2020
ms.openlocfilehash: b10ac3b4bc9dacd723b8b1265911df721b781189
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76774798"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli-rest-api"></a>Azure CLI, REST API'den okuma yinelemeleri oluşturma ve yönetme

Bu makalede, Azure CLI ve REST API'sini kullanarak PostgreSQL için Azure Veritabanı'nda okuma yinelemelerinin nasıl oluşturulup yönetilenolduğunu öğrenirsiniz. Okuma yinelemeleri hakkında daha fazla bilgi edinmek için [genel bakışa](concepts-read-replicas.md)bakın.

## <a name="azure-cli"></a>Azure CLI
Azure CLI'yi kullanarak okuma yinelemeleri oluşturabilir ve yönetebilirsiniz.

### <a name="prerequisites"></a>Ön koşullar

- [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [PostgreSQL sunucusunun](quickstart-create-server-up-azure-cli.md) ana sunucu olması için bir Azure Veritabanı.


### <a name="prepare-the-master-server"></a>Ana sunucuyu hazırlama
Bu adımlar, Genel Amaç veya Bellek Optimize Edilmiş katmanlarda bir ana sunucu hazırlamak için kullanılmalıdır.

Parametre ana sunucuda ÇOĞALTMA olarak ayarlanmalıdır. **REPLICA** `azure.replication_support` Bu statik parametre değiştirildiğinde, değişikliğin etkili olması için sunucu yeniden başlatması gerekir.

1. REPLICA `azure.replication_support` olarak ayarlayın.

   ```azurecli-interactive
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

> [!NOTE]
> Azure CLI'den azure.replication_support ayarlamaya çalışırken "Geçersiz değer verildi" hatasını alırsanız, sunucunuzun varsayılan olarak ÇOĞALTMA kümesi ne kadar yüksek olması olasıdır. Hata, bu ayarın ÇOĞALTMA'nın iç varsayılan olduğu yeni sunucularda doğru şekilde yansıtılmasını engelliyor. <br><br>
> Ana adımları hazırlamayı atlayabilir ve yinelemeyi oluşturmaya gidebilirsiniz. <br><br>
> Sunucunuzun bu kategoride olduğunu doğrulamak istiyorsanız, Azure portalındaki sunucunun çoğaltma sayfasını ziyaret edin. "Çoğaltmayı devre dışı" gri renkte ve "Çoğaltma Ekle" araç çubuğunda etkin olacaktır.

2. Değişikliği uygulamak için sunucuyu yeniden başlatın.

   ```azurecli-interactive
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

### <a name="create-a-read-replica"></a>Okuma yinelemesi oluşturma

[Az postgres sunucu çoğaltma komutu oluşturmak](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-create) aşağıdaki parametreleri gerektirir:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group | myresourcegroup |  Çoğaltma sunucusunun oluşturulacağı kaynak grubu.  |
| ad | mydemoserver-çoğaltma | Oluşturulan yeni çoğaltma sunucusunun adı. |
| source-server | mydemoserver | Çoğaltmak için varolan ana sunucunun adı veya kaynak kimliği. |

Aşağıdaki CLI örneğinde, yineleme ana yla aynı bölgede oluşturulur.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

Çapraz bölge okuma yinelemesi `--location` oluşturmak için parametreyi kullanın. Aşağıdaki CLI örneği, Batı ABD'de çoğaltma oluşturur.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> Yineleme oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek [için, okuma yineleme kavramları makalesini](concepts-read-replicas.md)ziyaret edin. 

Parametreyi `azure.replication_support` Genel Amaç veya Bellek Optimize Edilmiş ana sunucuda **ÇOĞALTMA** olarak ayarlamadıysanız ve sunucuyu yeniden başlattıysanız, bir hata alırsınız. Bir yineleme oluşturmadan önce bu iki adımı tamamlayın.

Yineleme, ana bilgisayarla aynı bilgi işlem ve depolama ayarları kullanılarak oluşturulur. Yineleme oluşturulduktan sonra, ana sunucudan bağımsız olarak birkaç ayar değiştirilebilir: bilgi işlem oluşturma, vCores, depolama ve yedekleme bekletme süresi. Fiyatlandırma katmanı, Temel katman alabilen veya temel katmandan bağımsız olarak değiştirilebilir.

> [!IMPORTANT]
> Ana sunucu ayarı yeni bir değere güncelleştirilmeden önce, yineleme ayarını eşit veya daha büyük bir değerle güncelleştirin. Bu eylem, yinelemenin ana ustada yapılan değişiklikleri takip etmesini yardımcı olur.

### <a name="list-replicas"></a>Liste yinelemeleri
[Az postgres sunucu çoğaltma listesi](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-list) komutunu kullanarak ana sunucunun yineleme listesini görüntüleyebilirsiniz.

```azurecli-interactive
az postgres server replica list --server-name mydemoserver --resource-group myresourcegroup 
```

### <a name="stop-replication-to-a-replica-server"></a>Çoğaltma sunucusuna çoğaltmayı durdurma
[Az postgres server replica stop](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-stop) komutunu kullanarak ana sunucu ile okuma yinelemesi arasındaki çoğaltmayı durdurabilirsiniz.

Ana sunucuya ve okuma yinelemesine çoğaltmayı durdurduktan sonra geri alınamaz. Okuma yinelemesi hem okumaları hem de yazmaları destekleyen bağımsız bir sunucu haline gelir. Bağımsız sunucu yeniden çoğaltma yapılamaz.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

### <a name="delete-a-master-or-replica-server"></a>Ana veya çoğaltma sunucususilme
Bir ana veya çoğaltma sunucusunu silmek için [az postgres sunucusu silme](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-delete) komutunu kullanırsınız.

Bir ana sunucusildiğinizde, okunan tüm yinelemelere çoğaltma durdurulur. Okuma yinelemeleri, artık hem okumaları hem de yazmaları destekleyen bağımsız sunucular haline gelir.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="rest-api"></a>REST API
[Azure REST API'sini](/rest/api/azure/)kullanarak okuma yinelemeleri oluşturabilir ve yönetebilirsiniz.

### <a name="prepare-the-master-server"></a>Ana sunucuyu hazırlama
Bu adımlar, Genel Amaç veya Bellek Optimize Edilmiş katmanlarda bir ana sunucu hazırlamak için kullanılmalıdır.

Parametre ana sunucuda ÇOĞALTMA olarak ayarlanmalıdır. **REPLICA** `azure.replication_support` Bu statik parametre değiştirildiğinde, değişikliğin etkili olması için sunucu yeniden başlatması gerekir.

1. REPLICA `azure.replication_support` olarak ayarlayın.

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

2. Değişikliği uygulamak için [sunucuyu yeniden başlatın.](/rest/api/postgresql/servers/restart)

   ```http
   POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/restart?api-version=2017-12-01
   ```

### <a name="create-a-read-replica"></a>Okuma yinelemesi oluşturma
[Create API'yi](/rest/api/postgresql/servers/create)kullanarak okuma yinelemesi oluşturabilirsiniz:

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
> Yineleme oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek [için, okuma yineleme kavramları makalesini](concepts-read-replicas.md)ziyaret edin. 

Parametreyi `azure.replication_support` Genel Amaç veya Bellek Optimize Edilmiş ana sunucuda **ÇOĞALTMA** olarak ayarlamadıysanız ve sunucuyu yeniden başlattıysanız, bir hata alırsınız. Bir yineleme oluşturmadan önce bu iki adımı tamamlayın.

Yineleme, ana bilgisayarla aynı bilgi işlem ve depolama ayarları kullanılarak oluşturulur. Yineleme oluşturulduktan sonra, ana sunucudan bağımsız olarak birkaç ayar değiştirilebilir: bilgi işlem oluşturma, vCores, depolama ve yedekleme bekletme süresi. Fiyatlandırma katmanı, Temel katman alabilen veya temel katmandan bağımsız olarak değiştirilebilir.


> [!IMPORTANT]
> Ana sunucu ayarı yeni bir değere güncelleştirilmeden önce, yineleme ayarını eşit veya daha büyük bir değerle güncelleştirin. Bu eylem, yinelemenin ana ustada yapılan değişiklikleri takip etmesini yardımcı olur.

### <a name="list-replicas"></a>Liste yinelemeleri
[Çoğaltma listesi API'yi](/rest/api/postgresql/replicas/listbyserver)kullanarak ana sunucunun yineleme listesini görüntüleyebilirsiniz:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>Çoğaltma sunucusuna çoğaltmayı durdurma
[Güncelleştirme API'sini](/rest/api/postgresql/servers/update)kullanarak ana sunucu ile okuma yinelemesi arasındaki çoğaltmayı durdurabilirsiniz.

Ana sunucuya ve okuma yinelemesine çoğaltmayı durdurduktan sonra geri alınamaz. Okuma yinelemesi hem okumaları hem de yazmaları destekleyen bağımsız bir sunucu haline gelir. Bağımsız sunucu yeniden çoğaltma yapılamaz.

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

### <a name="delete-a-master-or-replica-server"></a>Ana veya çoğaltma sunucususilme
Bir ana veya çoğaltma sunucusunu silmek [için, silme API'sini](/rest/api/postgresql/servers/delete)kullanırsınız:

Bir ana sunucusildiğinizde, okunan tüm yinelemelere çoğaltma durdurulur. Okuma yinelemeleri, artık hem okumaları hem de yazmaları destekleyen bağımsız sunucular haline gelir.

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{serverName}?api-version=2017-12-01
```

## <a name="next-steps"></a>Sonraki adımlar
* [PostgreSQL için Azure Veritabanı'nda okuma yinelemeleri](concepts-read-replicas.md)hakkında daha fazla bilgi edinin.
* [Azure portalında okuma yinelemelerini](howto-read-replicas-portal.md)nasıl oluşturup yöneteceklerini öğrenin.
