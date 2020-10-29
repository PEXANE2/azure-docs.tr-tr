---
title: Sunucuyu yönetme-Azure CLı-PostgreSQL için Azure veritabanı-esnek sunucu
description: Azure CLı 'dan bir PostgreSQL için Azure veritabanı-esnek sunucu yönetme hakkında bilgi edinin.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 8e24dd6cb8a1fa90f1a6caf9117ab3c344c00b12
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92913883"
---
# <a name="manage-an-azure-database-for-postgresql---flexible-server-using-the-azure-cli"></a>Azure CLı kullanarak bir PostgreSQL için Azure veritabanı 'nı yönetme-esnek sunucu

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

Bu makalede, Azure 'da dağıtılan esnek sunucunuzu nasıl yöneteceğiniz gösterilmektedir. Yönetim görevleri işlem ve depolama ölçeklendirmesi, yönetici parola sıfırlama ve sunucu ayrıntılarını görüntüleme içerir.

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

> [!Important]
> Henüz esnek bir sunucu oluşturmadıysanız, bu nasıl yapılır Kılavuzu ile çalışmaya başlamak için lütfen bir tane oluşturun.

## <a name="scale-compute-and-storage"></a>İşlem ve depolamayı ölçeklendirme

Aşağıdaki komutu kullanarak işlem katmanınızı, sanal çekirdekleri ve depolamayı kolayca ölçeklendirebilirsiniz. Çalıştırabilmeniz için tüm sunucu işlemlerini görebilirsiniz [az Postgres esnek-sunucu sunucusuna genel bakış](https://docs.microsoft.com/cli/azure/postgres/flexible-server)

```azurecli-interactive
az postgres flexible-server update --resource-group myresourcegroup --name mydemoserver --sku-name Standard_D4ds_v3 --storage-size 6144
```

Yukarıdaki bağımsız değişkenlerin ayrıntıları aşağıda verilmiştir:

**Ayar** | **Örnek değer** | **Açıklama**
---|---|---
name | mydemoserver | Sunucunuz için benzersiz bir ad girin. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ila 63 karakter arası içermelidir.
resource-group | myresourcegroup | Azure kaynak grubunun adını sağlayın.
sku-name|Standard_D4ds_v3|İşlem katmanının adını ve boyutunu girin. Toplu Standard_ {VM size} kuralına uyar. Daha fazla bilgi için [fiyatlandırma katmanlarına](../concepts-pricing-tiers.md) bakın.
storage-size | 6144 | Sunucunun depolama kapasitesi (birim olan megabayt kullanılır). Minimum 5120, 1024 artışlarla artar.

> [!IMPORTANT]
> Depolama alanı aşağı ölçeklendirilemez. 

## <a name="manage-postgresql-databases-on-a-server"></a>Sunucu üzerinde PostgreSQL veritabanlarını yönetme

PostgreSQL sunucusu için Azure veritabanınıza çeşitli uygulamalar kullanarak bağlanabilirsiniz. İstemci bilgisayarınızda PostgreSQL yüklüyse, [psql](https://www.postgresql.org/docs/current/static/app-psql.html)’nin yerel bir örneğini kullanarak Azure PostgreSQL sunucusuna bağlanabilirsiniz. Şimdi psql komut satırı yardımcı programını kullanarak Azure PostgreSQL sunucusuna bağlanalım.

1. PostgreSQL için Azure Veritabanı sunucusuna bağlanmak üzere aşağıdaki psql komutunu çalıştırın

   ```bash
   psql --host=<servername> --port=<port> --username=<user> --dbname=<dbname>
   ```

   Örneğin aşağıdaki komut, erişim kimlik bilgilerini kullanarak **mydemoserver.postgres.database.azure.com** PostgreSQL sunucunuzda **postgres** adlı varsayılan veritabanına bağlanır. Parola istendiğinde seçtiğiniz `<server_admin_password>` değerini girin.
  
   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin --dbname=postgres
   ```

   Bağlantı kurduktan sonra, psql yardımcı programı sql komutlarını girdiğiniz bir postgres istemi görüntüler. Kullandığınız psql ile PostgreSQL için Azure Veritabanı sunucu sürümü farklı olabileceğinden, ilk bağlantı çıkışında bir uyarı gösterilebilir.

   Örnek psql çıktısı:

   ```bash
   psql (11.3, server 12.1)
   WARNING: psql major version 11, server major version 12.
            Some psql features might not work.
   SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
   Type "help" for help.

   postgres=>
   ```

   > [!TIP]
   > Güvenlik duvarı istemcinizin IP adreslerine izin verecek biçimde yapılandırılmamışsa aşağıdaki hata oluşur:
   >
   > "psql: ÖNEMLI: Ana bilgisayar `<IP address>` ," myadmin ", veritabanı" Postgres "için bir pg_hba. conf girişi, önemli olan SSL: SSL bağlantısı gerekiyor. SSL seçeneklerini belirtin ve yeniden deneyin.
   >
   > Yukarıdaki güvenlik duvarı kuralları adımındaki istemci IP 'nizin izin verildiğini doğrulayın.

2. Aşağıdaki komutu yazarak istemde "postgresdb" adlı boş bir veritabanı oluşturun:

    ```bash
    CREATE DATABASE postgresdb;
    ```

3. Komut isteminde, bağlantıları yeni oluşturulan veritabanı **postgresdb** ile değiştirmek için aşağıdaki komutu yürütün:

    ```bash
    \c postgresdb
    ```

4. `\q` yazın ve ardından Enter tuşunu seçerek psql'den çıkın.

Psql aracılığıyla PostgreSQL için Azure Veritabanı sunucusuna bağlandınız ve boş bir kullanıcı veritabanı oluşturdunuz.

## <a name="reset-admin-password"></a>Yönetici parolasını sıfırla
Bu komutla yönetici rolü parolasını değiştirebilirsiniz
```azurecli-interactive
az postgres flexible-server update --resource-group myresourcegroup --name mydemoserver --admin-password <new-password>
```

> [!IMPORTANT]
> Parolanın en az 8 karakter ve en fazla 128 karakter uzunluğunda olduğundan emin olun.
> Parola şu kategorilerden üçünden karakterler içermelidir: Ingilizce büyük harfler, Ingilizce küçük harfler, sayılar ve alfasayısal olmayan karakterler.

## <a name="delete-a-server"></a>Sunucu silme

PostgreSQL esnek sunucusunu silmek istiyorsanız [az Postgres esnek-sunucu Delete](https://docs.microsoft.com/cli/azure/postgres/flexible-server#az-PostgreSQL-flexible-server-delete) komutunu çalıştırabilirsiniz.

```azurecli-interactive
az postgres flexible-server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar

- [Yedekleme ve geri yükleme kavramlarını anlama](concepts-backup-restore.md)
- [Sunucuyu ayarlama ve izleme](concepts-monitoring.md)