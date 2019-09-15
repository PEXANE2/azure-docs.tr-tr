---
title: MySQL için Azure veritabanı 'nda okuma çoğaltmaları oluşturma ve yönetme-Azure CLı, REST API
description: Bu makalede, Azure CLı kullanarak MySQL için Azure veritabanı 'nda okuma çoğaltmalarının nasıl ayarlanacağı ve yönetileceği açıklanmaktadır REST API
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 09/14/2019
ms.openlocfilehash: 5bec4e7284e78506372d395bf022055fa31998e3
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70993548"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-cli-and-rest-api"></a>Azure CLı ve REST API kullanarak MySQL için Azure veritabanı 'nda okuma çoğaltmaları oluşturma ve yönetme

Bu makalede, Azure CLı ve REST API kullanarak MySQL için Azure veritabanı hizmetinde okuma çoğaltmaları oluşturmayı ve yönetmeyi öğreneceksiniz. Okuma çoğaltmaları hakkında daha fazla bilgi edinmek için bkz. [genel bakış](concepts-read-replicas.md).

## <a name="azure-cli"></a>Azure CLI
Azure CLı kullanarak okuma çoğaltmaları oluşturabilir ve yönetebilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

- [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Bir [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md) ana sunucu olarak kullanılır. 

> [!IMPORTANT]
> Salt okunur çoğaltma özelliği yalnızca için Azure veritabanı genel amaçlı veya bellek için iyileştirilmiş fiyatlandırma katmanları MySQL sunucuları için kullanılabilir. Bu fiyatlandırma katmanlarından birini ana sunucusu olduğundan emin olun.

### <a name="create-a-read-replica"></a>Salt okunur bir çoğaltma oluşturma

Salt okunur çoğaltma sunucusu, aşağıdaki komutu kullanarak oluşturulabilir:

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

`az mysql server replica create` Komut takip eden parametreleri gerektiriyor:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusu oluşturulacağı kaynak grubu.  |
| ad | mydemoreplicaserver | Oluşturulan yeni çoğaltma sunucusunun adı. |
| source-server | mydemoserver | Adı veya çoğaltma kaynağı için mevcut ana sunucu kimliği. |

Bir çapraz bölge okuma çoğaltması oluşturmak için `--location` parametresini kullanın. Aşağıdaki CLı örneği Batı ABD içinde çoğaltmayı oluşturur.

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> İçinde bir çoğaltma oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek için [çoğaltma kavramlarını oku makalesini](concepts-read-replicas.md)ziyaret edin. 

> [!NOTE]
> Okuma çoğaltmaları aynı sunucu yapılandırma yöneticisi olarak oluşturulur. Çoğaltma sunucusu yapılandırması, oluşturulduktan sonra değiştirilebilir. Çoğaltma sunucusunun yapılandırmasını çoğaltma ana ayak olduğundan emin olmak için ana daha eşit veya daha fazla değerlerinde tutulması gereken önerilir.


### <a name="list-replicas-for-a-master-server"></a>Ana sunucu için liste çoğaltmaları

Verilen bir ana sunucu için tüm çoğaltmaları görüntülemek için aşağıdaki komutu çalıştırın: 

```azurecli-interactive
az mysql server replica list --server-name mydemoserver --resource-group myresourcegroup
```

`az mysql server replica list` Komut takip eden parametreleri gerektiriyor:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusu oluşturulacağı kaynak grubu.  |
| server-name | mydemoserver | Adı veya ana sunucu kimliği. |

### <a name="stop-replication-to-a-replica-server"></a>Bir çoğaltma sunucusu için çoğaltma durdurma

> [!IMPORTANT]
> Bir sunucuya çoğaltma durdurma işlemi geri alınamaz. Bir ana ve çoğaltma arasında çoğaltmayı durdurdu sonra geri alınamaz. Çoğaltma sunucusu bir tek başına sunucu olur ve artık hem okuma hem de yazma işlemleri destekler. Bu sunucu bir yinelemeye yeniden yapılamıyor.

Aşağıdaki komutu kullanarak bir salt okunur çoğaltma sunucusuna çoğaltma durdurulabilir:

```azurecli-interactive
az mysql server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

`az mysql server replica stop` Komut takip eden parametreleri gerektiriyor:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusunun mevcut olduğu kaynak grubu.  |
| ad | mydemoreplicaserver | Üzerindeki çoğaltma durdurma çoğaltma sunucusunun adı. |

### <a name="delete-a-replica-server"></a>Çoğaltma sunucusunu Sil

Salt okunur çoğaltma sunucusu silme yapılabilir çalıştırarak **[az mysql server delete](/cli/azure/mysql/server)** komutu.

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

### <a name="delete-a-master-server"></a>Bir ana sunucu silme

> [!IMPORTANT]
> Ana sunucu silme tüm çoğaltma sunucuları için çoğaltma durdurulur ve ana sunucusunu siler. Artık hem okuma hem de yazma işlemleri destekleyen tek başına sunucular çoğaltma sunucusu olur.

Ana sunucu silmek için çalıştırabileceğiniz **[az mysql server delete](/cli/azure/mysql/server)** komutu.

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```


## <a name="rest-api"></a>REST API
[Azure REST API](/rest/api/azure/)kullanarak okuma çoğaltmaları oluşturabilir ve yönetebilirsiniz.

### <a name="create-a-read-replica"></a>Salt okunur bir çoğaltma oluşturma
[Oluşturma API](/rest/api/mysql/servers/create)'sini kullanarak bir okuma çoğaltması oluşturabilirsiniz:

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{replicaName}?api-version=2017-12-01
```

```json
{
  "location": "southeastasia",
  "properties": {
    "createMode": "Replica",
    "sourceServerId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{masterServerName}"
  }
}
```

> [!NOTE]
> İçinde bir çoğaltma oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek için [çoğaltma kavramlarını oku makalesini](concepts-read-replicas.md)ziyaret edin. 

`azure.replication_support` Parametreyi genel amaçlı veya bellek için iyileştirilmiş ana sunucuda **çoğaltma** olarak ayarlamadıysanız ve sunucuyu yeniden başlattıktan sonra bir hata alırsınız. Bir çoğaltma oluşturmadan önce bu iki adımı uygulayın.

Bir çoğaltma, ana öğe ile aynı işlem ve depolama ayarları kullanılarak oluşturulur. Bir çoğaltma oluşturulduktan sonra, birden fazla ayar ana sunucudan bağımsız olarak değiştirilebilir: işlem oluşturma, sanal çekirdek, depolama ve yedekleme saklama süresi. Fiyatlandırma Katmanı, temel katmandan veya dışında bağımsız olarak da değiştirilebilir.


> [!IMPORTANT]
> Ana sunucu ayarı yeni bir değere güncellenmesinden önce, çoğaltma ayarını eşit veya daha büyük bir değere güncelleştirin. Bu eylem, çoğaltmanın ana üzerinde yapılan değişikliklerle devam etmesine yardımcı olur.

### <a name="list-replicas"></a>Çoğaltmaları Listele
[Çoğaltma LISTESI API](/rest/api/mysql/replicas/listbyserver)'sini kullanarak bir ana sunucunun çoğaltmalarının listesini görüntüleyebilirsiniz:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>Bir çoğaltma sunucusu için çoğaltma durdurma
[GÜNCELLEŞTIRME API](/rest/api/mysql/servers/update)'sini kullanarak bir ana sunucu ve okuma çoğaltması arasındaki çoğaltmayı durdurabilirsiniz.

Bir ana sunucu ve bir okuma çoğaltması için çoğaltmayı durdurduktan sonra geri alınamaz. Okuma çoğaltması, hem okuma hem de yazma işlemlerini destekleyen tek başına bir sunucu haline gelir. Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.

```http
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{masterServerName}?api-version=2017-12-01
```

```json
{
  "properties": {
    "replicationRole":"None"  
   }
}
```

### <a name="delete-a-master-or-replica-server"></a>Ana veya çoğaltma sunucusunu silme
Ana veya çoğaltma sunucusunu silmek için, [SILME API](/rest/api/mysql/servers/delete)'sini kullanın:

Bir ana sunucuyu sildiğinizde, tüm okuma çoğaltmalarına çoğaltma durdurulur. Okuma çoğaltmaları artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucular haline gelir.

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{serverName}?api-version=2017-12-01
```


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [çoğaltmaları okuyun](concepts-read-replicas.md)
