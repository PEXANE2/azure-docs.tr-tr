---
title: 'Hızlı başlangıç: sunucu oluşturma-Azure CLı-PostgreSQL için Azure veritabanı-tek sunucu'
description: Azure CLı (komut satırı arabirimi) kullanarak PostgreSQL için Azure veritabanı oluşturma kılavuzu-tek sunucu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 06/25/2020
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: cb88f085e18526a17621d3c4960a5aad6db727ad
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496602"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak bir PostgreSQL için Azure veritabanı oluşturma-tek sunucu

> [!TIP]
> Daha basit [az Postgres up](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) Azure CLI komutunu kullanmayı düşünün (Şu anda önizleme aşamasındadır). [Hızlı](./quickstart-create-server-up-azure-cli.md)başlangıcı deneyin.

Bu hızlı başlangıçta, beş dakika içinde PostgreSQL için Azure veritabanı sunucusu oluşturmak üzere [Azure Cloud Shell](https://shell.azure.com) ' de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) komutlarının nasıl kullanılacağı gösterilmektedir.  Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

>[!Note]
>CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, Azure CLı sürüm 2,0 veya üstünü yerel olarak çalıştırıyor olmanız gerekir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

[Az Login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) komutunu kullanarak hesabınızda oturum açmanız gerekir. Azure hesabınızın **ABONELIK kimliğine** başvuran **ID** özelliğine göz önüne alın. 

```azurecli-interactive
az login
```

[az account set](/cli/azure/account) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin. Komutta **abonelik** bağımsız değişkeninin değeri olarak kullanılacak **az Login** çıktısından **ID** değerini bir yere unutmayın. Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. Aboneliğinizi tamamen almak için [az Account List](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-list)kullanın.

```azurecli
az account set --subscription <subscription id>
```
## <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

[Az Group Create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) komutunu kullanarak bir [Azure Kaynak grubu](../azure-resource-manager/management/overview.md) oluşturun ve ardından bu kaynak grubunun Içinde PostgreSQL sunucunuzu oluşturun. Benzersiz bir ad sağlamanız gerekir. Aşağıdaki örnek `westus` konumunda `myresourcegroup` adlı bir kaynak grubu oluşturur.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

[az postgres server create](/cli/azure/postgres/server) komutunu kullanarak [PostgreSQL sunucusu için Azure SQL Veritabanı ](overview.md) oluşturun. Bir sunucu birden çok veritabanı içerebilir.

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 
```
Yukarıdaki bağımsız değişkenlerin ayrıntıları aşağıda verilmiştir: 

**Ayar** | **Örnek değer** | **Açıklama**
---|---|---
name | mydemoserver | Azure veritabanınızı PostgreSQL sunucusuna tanıtan benzersiz bir ad seçin. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ila 63 karakter arası içermelidir.
resource-group | myresourcegroup | Azure kaynak grubunun adını sağlayın.
location | westus | Sunucu için Azure konumu.
admin-user | myadmin | Yöneticinin oturum açma kullanıcı adı. Şu değerler kullanılamaz: **azure_superuser**, **admin**, **administrator**, **root**, **guest** veya **public**.
admin-password | *güvenli parola* | Yönetici kullanıcının parolası. 8 ile 128 arasında karakter içermelidir. Parolanız şu üç kategoride yer alan karakterlerden oluşmalıdır: İngilizce büyük ve küçük harfler, sayılar ve alfasayısal olmayan karakterler.
sku-name|GP_Gen5_2|Fiyatlandırma katmanının adını ve işlem yapılandırmasını girin. Toplu olarak {fiyatlandırma katmanı}_{COMPUTE Generation}_{vçekirdekler} kuralını izler. Daha fazla bilgi için bkz. [PostgreSQL Için Azure veritabanı](https://azure.microsoft.com/pricing/details/postgresql/server/).

>[!IMPORTANT] 
>- Sunucunuzdaki varsayılan PostgreSQL sürümü 9,6 ' dir. [Burada](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)desteklenen tüm sürümlerimizi görüntüleyin.
>- **Az Postgres Server Create** komutuyla ilgili tüm bağımsız değişkenleri görüntülemek için, bu [Başvuru belgesine](https://docs.microsoft.com/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) bakın
>- SSL, sunucunuzda varsayılan olarak etkinleştirilmiştir. SSL üzerinde daha fazla bilgi için bkz. [SSL bağlantısını yapılandırma](./concepts-ssl-connection-security.md)

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma 
Varsayılan olarak, oluşturulan sunucu herkese açık bir şekilde erişilebilir değildir ve güvenlik duvarı kurallarıyla korunmaz. Sunucunuzdaki güvenlik duvarı kuralını, yerel ortamınıza sunucuya bağlanmak üzere erişim sağlamak için [az Postgres Server Firewall-Rule Create](/cli/azure/postgres/server/firewall-rule) komutunu kullanarak yapılandırabilirsiniz. 

Aşağıdaki örnek `AllowMyIP` adında ve 192.168.0.1 IP adresinden gelen bağlantılara izin veren bir güvenlik duvarı kuralı oluşturur. Bağlantı oluşturacağınız yere karşılık gelen IP adresini veya IP adresi aralığını değiştirin.  IP 'nize nasıl bakabileceğinizi bilmiyorsanız [https://whatismyipaddress.com/](https://whatismyipaddress.com/) IP adresinizi almak için adresine gidin.


```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
>  Bağlantı sorunlarından kaçınmak için lütfen ağınızın güvenlik duvarının PostgreSQL için Azure veritabanı sunucuları tarafından kullanılan bağlantı noktası 5432 ' e izin verdiğinden emin olun. 

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mydemoserver
```

Sonuç JSON biçimindedir. **administratorLogin** ve **fullyQualifiedDomainName** bilgilerini not alın.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-to-azure-database-for-postgresql-server-using-psql"></a>Psql kullanarak PostgreSQL için Azure veritabanı sunucusuna bağlanma
[**psql**](https://www.postgresql.org/docs/current/static/app-psql.html) , PostgreSQL sunucularına bağlanmak için kullanılan popüler istemcdir. [Azure Cloud Shell](../cloud-shell/overview.md)ile **psql** kullanarak sunucunuza bağlanabilirsiniz. Alternatif olarak, varsa, yerel ortamınızda psql 'i de kullanabilirsiniz. ' Postgres ' boş bir veritabanı, aşağıda gösterildiği gibi psql ile bağlantı kurmak için kullanabileceğiniz yeni PostgreSQL sunucunuz ile zaten oluşturulmuş 

   ```bash
 psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```

   > [!TIP]
   > Postgres 'e bağlanmak için bir URL yolu kullanmayı tercih ediyorsanız, URL, Kullanıcı adında @ işaretini ile kodlayın `%40` . Örneğin, psql için bağlantı dizesi,
   > ```
   > psql postgresql://myadmin%40mydemoserver@mydemoserver.postgres.database.azure.com:5432/postgres
   > ```


## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu kaynaklara başka bir hızlı başlangıç/öğretici için ihtiyacınız yoksa, aşağıdaki komutu çalıştırarak bunları silebilirsiniz: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Yeni oluşturulan tek bir sunucuyu silmek istiyorsanız [az postgres server delete](/cli/azure/postgres/server) komutunu kullanabilirsiniz.
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)
> 
> [PostgreSQL ile Docgo Web uygulaması dağıtma](../app-service/containers/tutorial-python-postgresql-app.md)
>
> [Node.JS uygulamayla bağlantı](./connect-nodejs.md)

