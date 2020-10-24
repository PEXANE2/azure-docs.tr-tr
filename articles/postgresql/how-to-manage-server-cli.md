---
title: Sunucuyu yönetme-Azure CLı-PostgreSQL için Azure veritabanı
description: Azure CLı 'dan PostgreSQL için Azure veritabanı sunucusunu yönetmeyi öğrenin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 9/22/2020
ms.openlocfilehash: d99634388b9c4db99c996cfccb9bb5f12682f217
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92490127"
---
# <a name="manage-an-azure-database-for-postgresql-single-server-using-the-azure-cli"></a>Azure CLı kullanarak PostgreSQL için Azure veritabanı tek sunucu yönetme

Bu makalede, Azure 'da dağıtılan tek sunucularınızın nasıl yönetileceği gösterilmektedir. Yönetim görevleri işlem ve depolama ölçeklendirmesi, yönetici parola sıfırlama ve sunucu ayrıntılarını görüntüleme içerir.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. Bu makalede, Azure CLı sürüm 2,0 veya üstünü yerel olarak çalıştırıyor olmanız gerekir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

[Az Login](/cli/azure/reference-index#az-login) komutunu kullanarak hesabınızda oturum açmanız gerekir. Azure hesabınızın **ABONELIK kimliğini** ifade eden **ID** özelliğine göz önüne alın.

```azurecli-interactive
az login
```

[Az Account set](/cli/azure/account) komutunu kullanarak hesabınız altındaki belirli bir aboneliği seçin. Komutta **abonelik** bağımsız değişkeninin değeri olarak kullanılacak **az Login** çıktısından **ID** değerini bir yere unutmayın. Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. Aboneliğinizi tamamen almak için [az Account List](/cli/azure/account#az-account-list)kullanın.

```azurecli
az account set --subscription <subscription id>
```

Zaten bir sunucu oluşturmadıysanız, oluşturmak için bu [hızlı](quickstart-create-server-database-azure-cli.md) başlangıca bakın.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

## <a name="scale-compute-and-storage"></a>İşlem ve depolamayı ölçeklendirme

Aşağıdaki komutu kullanarak fiyatlandırma katmanınızı, işlem ve depolamayı kolayca ölçeklendirebilirsiniz. Gerçekleştirebileceğiniz tüm sunucu işlemlerini görebilirsiniz [az Postgres Server genel bakış](/cli/azure/mysql/server)

```azurecli-interactive
az postgres server update --resource-group myresourcegroup --name mydemoserver --sku-name GP_Gen5_4 --storage-size 6144
```

Yukarıdaki bağımsız değişkenlerin ayrıntıları aşağıda verilmiştir:

**Ayar** | **Örnek değer** | **Açıklama**
---|---|---
name | mydemoserver | PostgreSQL için Azure veritabanı sunucusu için benzersiz bir ad girin. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ila 63 karakter arası içermelidir.
resource-group | myresourcegroup | Azure kaynak grubunun adını sağlayın.
sku-name|GP_Gen5_2|Fiyatlandırma katmanının adını ve işlem yapılandırmasını girin. Toplu olarak {fiyatlandırma katmanı}_{COMPUTE Generation}_{vçekirdekler} kuralını izler. Daha fazla bilgi için [fiyatlandırma katmanlarına](./concepts-pricing-tiers.md) bakın.
storage-size | 6144 | Sunucunun depolama kapasitesi (birim olan megabayt kullanılır). Minimum 5120, 1024 artışlarla artar.

> [!Important]
> - Depolama alanı yukarı ölçeklendirilebilir (ancak, depolamayı ölçeklendirmezsiniz)
> - Temel, genel amaçlı veya bellek için iyileştirilmiş fiyatlandırma katmanının ölçeğini artırma desteklenmez. [Bash betiği kullanarak](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/upgrade-from-basic-to-general-purpose-or-memory-optimized-tiers/ba-p/830404) ya da [PostgreSQL çalışma ekranı kullanarak](https://techcommunity.microsoft.com/t5/azure-database-support-blog/how-to-scale-up-azure-database-for-mysql-from-basic-tier-to/ba-p/369134) el ile ölçeği izleyebilirsiniz


## <a name="manage-postgresql-databases-on-a-server"></a>Bir sunucudaki PostgreSQL veritabanlarını yönetin.
Sunucunuzdaki bir veritabanının veritabanı özelliklerini oluşturmak, silmek, listelemek ve görüntülemek için aşağıdaki komutlardan herhangi birini kullanabilirsiniz

| Cmdlet | Kullanım| Açıklama |
| --- | ---| --- |
|[az Postgres DB Create](/cli/azure/sql/db#az-mysql-db-create)|```az postgres db create -g myresourcegroup -s mydemoserver -n mydatabasename``` |Bir veritabanı oluşturur|
|[az Postgres DB Delete](/cli/azure/sql/db#az-mysql-db-delete)|```az postgres db delete -g myresourcegroup -s mydemoserver -n mydatabasename```|Veritabanınızı sunucudan silin. Bu komut, sunucunuzu silmez. |
|[az Postgres DB listesi](/cli/azure/sql/db#az-mysql-db-list)|```az postgres db list -g myresourcegroup -s mydemoserver```|sunucudaki tüm veritabanlarını listeler|
|[az Postgres DB Show](/cli/azure/sql/db#az-mysql-db-show)|```az postgres db show -g myresourcegroup -s mydemoserver -n mydatabasename```|Veritabanının daha fazla ayrıntılarını gösterir|

## <a name="update-admin-password"></a>Yönetici parolasını Güncelleştir
Bu komutla yönetici rolü parolasını değiştirebilirsiniz
```azurecli-interactive
az postgres server update --resource-group myresourcegroup --name mydemoserver --admin-password <new-password>
```

> [!Important]
>  Parolanın en az 8 karakter ve en fazla 128 karakter uzunluğunda olduğundan emin olun.
> Parola şu kategorilerden üçünden karakterler içermelidir: Ingilizce büyük harfler, Ingilizce küçük harfler, sayılar ve alfasayısal olmayan karakterler.

## <a name="delete-a-server"></a>Sunucu silme
Yalnızca PostgreSQL tek sunucusunu silmek istiyorsanız [az Postgres Server DELETE](/cli/azure/mysql/server#az-mysql-server-delete) komutunu çalıştırabilirsiniz.

```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar
- [Sunucuyu yeniden başlatma](howto-restart-server-cli.md)
- [Bir sunucuyu hatalı durumda geri yükleme](howto-restore-server-cli.md)
- [Sunucuyu izleme ve ayarlama](concepts-monitoring.md)