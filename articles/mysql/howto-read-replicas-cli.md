---
title: Okuma yinelemelerini yönetme - Azure CLI, REST API - MySQL için Azure Veritabanı
description: Azure CLI veya REST API'yi kullanarak MySQL için Azure Veritabanı'nda okuma yinelemelerini nasıl ayarlayıp yöneteceklerini öğrenin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: ed57003c7a9a5a1a9d87aa2e8934af8c48b1d819
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063325"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-cli-and-rest-api"></a>Azure CLI ve REST API'sini kullanarak MySQL için Azure Veritabanı'nda okuma yinelemeleri oluşturma ve yönetme

Bu makalede, Azure CLI ve REST API'sini kullanarak MySQL hizmeti için Azure Veritabanı'nda okuma yinelemelerinin nasıl oluşturulacağını ve yöneteceğinizi öğreneceksiniz. Okuma yinelemeleri hakkında daha fazla bilgi edinmek için [genel bakışa](concepts-read-replicas.md)bakın.

## <a name="azure-cli"></a>Azure CLI
Azure CLI'yi kullanarak okuma yinelemeleri oluşturabilir ve yönetebilirsiniz.

### <a name="prerequisites"></a>Ön koşullar

- [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Ana sunucu olarak kullanılacak [MySQL sunucusu için](quickstart-create-mysql-server-database-using-azure-portal.md) bir Azure Veritabanı. 

> [!IMPORTANT]
> Okundu çoğaltma özelliği yalnızca Genel Amaç veya Bellek Optimize edilmiş fiyatlandırma katmanlarındaki MySQL sunucuları için Azure Veritabanı için kullanılabilir. Ana sunucunun bu fiyatlandırma katmanlarından birinde olduğundan emin olun.

### <a name="create-a-read-replica"></a>Okuma yinelemesi oluşturma

Okuma çoğaltma sunucusu aşağıdaki komut kullanılarak oluşturulabilir:

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

Komut `az mysql server replica create` aşağıdaki parametreleri gerektirir:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusunun oluşturulacağı kaynak grubu.  |
| ad | mydemoreplicaserver | Oluşturulan yeni çoğaltma sunucusunun adı. |
| source-server | mydemoserver | Çoğaltmak için varolan ana sunucunun adı veya kimliği. |

Çapraz bölge okuma yinelemesi `--location` oluşturmak için parametreyi kullanın. Aşağıdaki CLI örneği, Batı ABD'de çoğaltma oluşturur.

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> Yineleme oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek [için, okuma yineleme kavramları makalesini](concepts-read-replicas.md)ziyaret edin. 

> [!NOTE]
> Okuma yinelemeleri ana sunucu yapılandırmasıyla oluşturulur. Çoğaltma sunucusu yapılandırması oluşturulduktan sonra değiştirilebilir. Yinelemenin ana değere ayak uydurabilmesi için yineleme sunucusunun yapılandırmasının ana değerden eşit veya daha büyük değerlerde tutulması önerilir.


### <a name="list-replicas-for-a-master-server"></a>Ana sunucu için yinelemeleri listele

Belirli bir ana sunucunun tüm yinelemelerini görüntülemek için aşağıdaki komutu çalıştırın: 

```azurecli-interactive
az mysql server replica list --server-name mydemoserver --resource-group myresourcegroup
```

Komut `az mysql server replica list` aşağıdaki parametreleri gerektirir:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusunun oluşturulacağı kaynak grubu.  |
| server-name | mydemoserver | Ana sunucunun adı veya kimliği. |

### <a name="stop-replication-to-a-replica-server"></a>Çoğaltma sunucusuna çoğaltmayı durdurma

> [!IMPORTANT]
> Bir sunucuya çoğaltmayı durdurmak geri alınamaz. Çoğaltma bir ana ve yineleme arasında durdurulduktan sonra geri alınamaz. Çoğaltma sunucusu daha sonra bağımsız bir sunucu haline gelir ve şimdi hem okuma ve yazma destekler. Bu sunucu yeniden çoğaltma içine yapılamaz.

Okuma çoğaltma sunucusuna çoğaltma aşağıdaki komutu kullanarak durdurulabilir:

```azurecli-interactive
az mysql server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

Komut `az mysql server replica stop` aşağıdaki parametreleri gerektirir:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Yineleme sunucusunun bulunduğu kaynak grubu.  |
| ad | mydemoreplicaserver | Çoğaltmayı durdurmak için çoğaltma sunucusunun adı. |

### <a name="delete-a-replica-server"></a>Çoğaltma sunucususilme

Bir okuma çoğaltma sunucusu silme **[az mysql server delete](/cli/azure/mysql/server)** komutu çalıştırarak yapılabilir.

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

### <a name="delete-a-master-server"></a>Ana sunucusilme

> [!IMPORTANT]
> Bir ana sunucu durdurulduğunda, tüm çoğaltma sunucularına çoğaltma durdurulur ve ana sunucu silinir. Çoğaltma sunucuları artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucular haline gelir.

Bir ana sunucuyu silmek için **[az mysql server delete](/cli/azure/mysql/server)** komutunu çalıştırabilirsiniz.

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```


## <a name="rest-api"></a>REST API
[Azure REST API'sini](/rest/api/azure/)kullanarak okuma yinelemeleri oluşturabilir ve yönetebilirsiniz.

### <a name="create-a-read-replica"></a>Okuma yinelemesi oluşturma
[Create API'yi](/rest/api/mysql/servers/create)kullanarak okuma yinelemesi oluşturabilirsiniz:

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
> Yineleme oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek [için, okuma yineleme kavramları makalesini](concepts-read-replicas.md)ziyaret edin. 

Parametreyi `azure.replication_support` Genel Amaç veya Bellek Optimize Edilmiş ana sunucuda **ÇOĞALTMA** olarak ayarlamadıysanız ve sunucuyu yeniden başlattıysanız, bir hata alırsınız. Bir yineleme oluşturmadan önce bu iki adımı tamamlayın.

Yineleme, ana bilgisayarla aynı bilgi işlem ve depolama ayarları kullanılarak oluşturulur. Yineleme oluşturulduktan sonra, ana sunucudan bağımsız olarak birkaç ayar değiştirilebilir: bilgi işlem oluşturma, vCores, depolama ve yedekleme bekletme süresi. Fiyatlandırma katmanı, Temel katman alabilen veya temel katmandan bağımsız olarak değiştirilebilir.


> [!IMPORTANT]
> Ana sunucu ayarı yeni bir değere güncelleştirilmeden önce, yineleme ayarını eşit veya daha büyük bir değerle güncelleştirin. Bu eylem, yinelemenin ana ustada yapılan değişiklikleri takip etmesini yardımcı olur.

### <a name="list-replicas"></a>Liste yinelemeleri
[Çoğaltma listesi API'yi](/rest/api/mysql/replicas/listbyserver)kullanarak ana sunucunun yineleme listesini görüntüleyebilirsiniz:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>Çoğaltma sunucusuna çoğaltmayı durdurma
[Güncelleştirme API'sini](/rest/api/mysql/servers/update)kullanarak ana sunucu ile okuma yinelemesi arasındaki çoğaltmayı durdurabilirsiniz.

Ana sunucuya ve okuma yinelemesine çoğaltmayı durdurduktan sonra geri alınamaz. Okuma yinelemesi hem okumaları hem de yazmaları destekleyen bağımsız bir sunucu haline gelir. Bağımsız sunucu yeniden çoğaltma yapılamaz.

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

### <a name="delete-a-master-or-replica-server"></a>Ana veya çoğaltma sunucususilme
Bir ana veya çoğaltma sunucusunu silmek [için, silme API'sini](/rest/api/mysql/servers/delete)kullanırsınız:

Bir ana sunucusildiğinizde, okunan tüm yinelemelere çoğaltma durdurulur. Okuma yinelemeleri, artık hem okumaları hem de yazmaları destekleyen bağımsız sunucular haline gelir.

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{serverName}?api-version=2017-12-01
```


## <a name="next-steps"></a>Sonraki adımlar

- [Okuma yinelemeleri](concepts-read-replicas.md) hakkında daha fazla bilgi edinin
