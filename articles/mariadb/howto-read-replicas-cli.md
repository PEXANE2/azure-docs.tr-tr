---
title: MariaDB için Azure veritabanı 'nda okuma çoğaltmaları oluşturma ve yönetme
description: Bu makalede, Azure CLı kullanarak MariaDB için Azure veritabanı 'nda okuma çoğaltmalarını ayarlama ve yönetme işlemlerinin nasıl yapılacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 07/26/2019
ms.openlocfilehash: ba0f3bd002b2675c33ea5106ce64c7957c9989d0
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309116"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mariadb-using-the-azure-cli"></a>Azure CLı kullanarak MariaDB için Azure veritabanı 'nda okuma çoğaltmaları oluşturma ve yönetme

Bu makalede, Azure CLı kullanarak MariaDB hizmeti için Azure veritabanı 'nda ana öğe ile aynı Azure bölgesinde bulunan okuma çoğaltmaları oluşturmayı ve yönetmeyi öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

- [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Ana sunucu olarak kullanılacak [MariaDB sunucusu Için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-portal.md) . 

> [!IMPORTANT]
> Çoğaltma oku özelliği yalnızca Genel Amaçlı veya bellek için Iyileştirilmiş fiyatlandırma katmanlarında bulunan MariaDB sunucuları için Azure veritabanı 'nda kullanılabilir. Bu fiyatlandırma katmanlarından birini ana sunucusu olduğundan emin olun.

## <a name="create-a-read-replica"></a>Salt okunur bir çoğaltma oluşturma

Salt okunur çoğaltma sunucusu, aşağıdaki komutu kullanarak oluşturulabilir:

```azurecli-interactive
az mariadb server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

`az mariadb server replica create` Komut takip eden parametreleri gerektiriyor:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusu oluşturulacağı kaynak grubu.  |
| ad | mydemoreplicaserver | Oluşturulan yeni çoğaltma sunucusunun adı. |
| source-server | mydemoserver | Adı veya çoğaltma kaynağı için mevcut ana sunucu kimliği. |

Bir çapraz bölge okuma çoğaltması oluşturmak için `--location` parametresini kullanın. Aşağıdaki CLı örneği Batı ABD içinde çoğaltmayı oluşturur.

```azurecli-interactive
az mariadb server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> İçinde bir çoğaltma oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek için [çoğaltma kavramlarını oku makalesini](concepts-read-replicas.md)ziyaret edin. 

> [!NOTE]
> Okuma çoğaltmaları aynı sunucu yapılandırma yöneticisi olarak oluşturulur. Çoğaltma sunucusu yapılandırması, oluşturulduktan sonra değiştirilebilir. Çoğaltma sunucusunun yapılandırmasını çoğaltma ana ayak olduğundan emin olmak için ana daha eşit veya daha fazla değerlerinde tutulması gereken önerilir.

## <a name="stop-replication-to-a-replica-server"></a>Bir çoğaltma sunucusu için çoğaltma durdurma

> [!IMPORTANT]
> Bir sunucuya çoğaltma durdurma işlemi geri alınamaz. Bir ana ve çoğaltma arasında çoğaltmayı durdurdu sonra geri alınamaz. Çoğaltma sunucusu bir tek başına sunucu olur ve artık hem okuma hem de yazma işlemleri destekler. Bu sunucu bir yinelemeye yeniden yapılamıyor.

Aşağıdaki komutu kullanarak bir salt okunur çoğaltma sunucusuna çoğaltma durdurulabilir:

```azurecli-interactive
az mariadb server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

`az mariadb server replica stop` Komut takip eden parametreleri gerektiriyor:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusunun mevcut olduğu kaynak grubu.  |
| ad | mydemoreplicaserver | Üzerindeki çoğaltma durdurma çoğaltma sunucusunun adı. |

## <a name="delete-a-replica-server"></a>Çoğaltma sunucusunu Sil

**[Az MariaDB Server DELETE](/cli/azure/mariadb/server)** komutu çalıştırılarak bir okuma çoğaltması sunucusunu silme işlemi gerçekleştirilebilir.

```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

## <a name="delete-a-master-server"></a>Bir ana sunucu silme

> [!IMPORTANT]
> Ana sunucu silme tüm çoğaltma sunucuları için çoğaltma durdurulur ve ana sunucusunu siler. Artık hem okuma hem de yazma işlemleri destekleyen tek başına sunucular çoğaltma sunucusu olur.

Ana Sunucuyu silmek için **[az MariaDB Server DELETE](/cli/azure/mariadb/server)** komutunu çalıştırabilirsiniz.

```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="list-replicas-for-a-master-server"></a>Ana sunucu için liste çoğaltmaları

Verilen bir ana sunucu için tüm çoğaltmaları görüntülemek için aşağıdaki komutu çalıştırın: 

```azurecli-interactive
az mariadb server replica list --server-name mydemoserver --resource-group myresourcegroup
```

`az mariadb server replica list` Komut takip eden parametreleri gerektiriyor:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusu oluşturulacağı kaynak grubu.  |
| server-name | mydemoserver | Adı veya ana sunucu kimliği. |

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [çoğaltmaları okuyun](concepts-read-replicas.md)
