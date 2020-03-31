---
title: Güvenlik duvarı kurallarını yönetme - Azure CLI - MySQL için Azure Veritabanı
description: Bu makalede, Azure CLI komut satırı kullanarak MySQL güvenlik duvarı kuralları için Azure Veritabanı'nın nasıl oluşturulup yönetilen anlatılmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 11aa4a80deba4df14c239e69910ea38bac1b9c55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063525"
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Azure CLI'yi kullanarak MySQL güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme
Sunucu düzeyinde güvenlik duvarı kuralları, MySQL Server için bir Azure Veritabanına erişimi belirli bir IP adresinden veya çeşitli IP adreslerinden yönetmek için kullanılabilir. Kullanışlı Azure CLI komutlarını kullanarak, sunucunuzu yönetmek için güvenlik duvarı kurallarını oluşturabilir, güncelleyebilir, silebilir, listeleyebilir ve gösterebilirsiniz. MySQL güvenlik duvarları için Azure Veritabanı'na genel bakış için [MySQL sunucu güvenlik duvarı kuralları için Azure Veritabanı'na](./concepts-firewall-rules.md)bakın.

Sanal Ağ (VNet) kuralları sunucunuza erişimi güvence altına almak için de kullanılabilir. [Azure CLI'yi kullanarak Sanal Ağ hizmeti uç noktalarını ve kurallarını oluşturma ve yönetme](howto-manage-vnet-using-cli.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar
* [Azure CLI’yı yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli).
* [MySQL sunucusu ve veritabanı için](quickstart-create-mysql-server-database-using-azure-cli.md)bir Azure Veritabanı.

## <a name="firewall-rule-commands"></a>Güvenlik duvarı kuralı komutları:
**Az mysql sunucu güvenlik duvarı kuralı** komutu, Güvenlik Duvarı kurallarını oluşturmak, silmek, listelemek, göstermek ve güncelleştirmek için Azure CLI'den kullanılır.

Komut:
- create : Azure MySQL sunucu güvenlik duvarı kuralı **oluşturun.**
- **delete**: Azure MySQL sunucu güvenlik duvarı kuralını silin.
- **liste**: Azure MySQL sunucu güvenlik duvarı kurallarını listeleyin.
- **göster**: Azure MySQL sunucu güvenlik duvarı kuralının ayrıntılarını göster.
- **update**: Azure MySQL sunucu güvenlik duvarı kuralını güncelleştirin.

## <a name="sign-in-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Azure'da oturum açın ve MySQL Sunucuları için Azure Veritabanınızı listeleyin
**Az giriş** komutunu kullanarak Azure CLI'yi Azure hesabınıza güvenli bir şekilde bağlayın.

1. Komut satırından aşağıdaki komutu çalıştırın:
    ```azurecli
    az login
    ```
   Bu komut, bir sonraki adımda kullanılacak bir kod çıktırıyor.

2. Sayfayı [https://aka.ms/devicelogin](https://aka.ms/devicelogin)açmak için bir web tarayıcısı kullanın ve ardından kodu girin.

3. Anında Azure kimlik bilgilerinizi kullanarak oturum açın.

4. Giriş izniniz onaylandıktan sonra konsola bir abonelik listesi yazdırılır. Geçerli aboneliği kullanmak üzere ayarlamak için istenen aboneliğin kimliğini kopyalayın. az [hesap kümesi](/cli/azure/account#az-account-set) komutunu kullanın.
    ```azurecli-interactive
    az account set --subscription <your subscription id>
    ```

5. İsimlerden emin değilseniz, aboneliğiniz ve kaynak grubunuz için MySQL sunucuları için Azure Veritabanlarını listeleyin. az [mysql sunucu listesi](/cli/azure/mysql/server#az-mysql-server-list) komutunu kullanın.

    ```azurecli-interactive
    az mysql server list --resource-group myresourcegroup
    ```

   Üzerinde çalışmak için MySQL sunucusunu belirtmeniz gereken girişteki ad özniteliğine dikkat edin. Gerekirse, bu sunucunun ayrıntılarını onaylayın ve doğru olduğundan emin olmak için ad özniteliğini kullanarak. az [mysql server show](/cli/azure/mysql/server#az-mysql-server-show) komutunu kullanın.

    ```azurecli-interactive
    az mysql server show --resource-group myresourcegroup --name mydemoserver
    ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>MySQL Server için Azure Veritabanı'nda güvenlik duvarı kurallarını listele 
Sunucu adını ve kaynak grubu adını kullanarak, sunucudaki varolan sunucu güvenlik duvarı kurallarını listeleyin. az [mysql sunucu güvenlik duvarı listesi](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-list) komutunu kullanın.  Sunucu adı özniteliğinin **--sunucu** anahtarında değil, **-ad** anahtarında belirtildiğine dikkat edin. 
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Çıktı, varsa kuralları JSON biçiminde (varsayılan olarak) listeler. Sonuçları daha okunabilir bir tablo biçiminde çıktılamak için **--çıktı tablosu** anahtarını kullanabilirsiniz.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>MySQL Server için Azure Veritabanı'nda güvenlik duvarı kuralı oluşturma
Azure MySQL sunucu adını ve kaynak grubu adını kullanarak sunucuda yeni bir güvenlik duvarı kuralı oluşturun. Az [mysql sunucu güvenlik duvarı oluşturma](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create) komutunu kullanın. Kural için kuralın yanı sıra başlangıç IP'si ve son IP 'yi (çeşitli IP adreslerine erişim sağlamak için) kuralın bir adı sağlayın.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Tek bir IP adresine erişim sağlamak için, bu örnekte olduğu gibi Start IP ve End IP ile aynı IP adresini sağlayın.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```

Azure IP adreslerinden uygulamaların MySQL sunucusu için Azure Veritabanınıza bağlanmasına izin vermek için, bu örnekte olduğu gibi IP başlat ve SON IP olarak 0.0.0.0 IP adresini sağlayın.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mysql --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

Başarıya uyunüzerine, her bir komut çıktısı, oluşturduğunuz güvenlik duvarı kuralının ayrıntılarını JSON biçiminde (varsayılan olarak) listeler. Bir hata varsa, çıktı yerine hata iletisi metnini gösterir.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure Veritabanı'nda bir güvenlik duvarı kuralını güncelleştirme 
Azure MySQL sunucu adını ve kaynak grubu adını kullanarak sunucudaki varolan bir güvenlik duvarı kuralını güncelleştirin. az [mysql server güvenlik duvarı güncelleme](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-update) komutunu kullanın. Varolan güvenlik duvarı kuralının adını giriş olarak ve güncelleştirmek için IP'yi başlat ve son ucunu sağlayın.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Başarıya uyunüzerine, komut çıktısı güncellediğiniz güvenlik duvarı kuralının ayrıntılarını JSON biçiminde (varsayılan olarak) listeler. Bir hata varsa, çıktı yerine hata iletisi metnini gösterir.

> [!NOTE]
> Güvenlik duvarı kuralı yoksa, kural güncelleştirme komutu tarafından oluşturulur.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>MySQL Server için Azure Veritabanı'nda güvenlik duvarı kuralı ayrıntılarını göster
Azure MySQL sunucu adını ve kaynak grubu adını kullanarak, sunucudan varolan güvenlik duvarı kuralı ayrıntılarını gösterir. az [mysql server güvenlik duvarı göster](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-show) komutunu kullanın. Varolan güvenlik duvarı kuralının adını giriş olarak sağlayın.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Başarıya uyunüzerine, komut çıktısı belirttiğiniz güvenlik duvarı kuralının ayrıntılarını JSON biçiminde (varsayılan olarak) listeler. Bir hata varsa, çıktı yerine hata iletisi metnini gösterir.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>MySQL Server için Azure Veritabanı'nda bir güvenlik duvarı kuralını silme
Azure MySQL sunucu adını ve kaynak grubu adını kullanarak, varolan bir güvenlik duvarı kuralını sunucudan kaldırın. az [mysql server güvenlik duvarı silme](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-delete) komutunu kullanın. Varolan güvenlik duvarı kuralının adını sağlayın.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Başarı üzerine, hiçbir çıkış yoktur. Hata üzerine, hata iletisi metin görüntüler.

## <a name="next-steps"></a>Sonraki adımlar
- [MySQL Server güvenlik duvarı kuralları için Azure Veritabanı](./concepts-firewall-rules.md)hakkında daha fazla bilgi edinin.
- [Azure portalını kullanarak MySQL güvenlik duvarı kuralları için Azure Veritabanı oluşturun ve yönetin.](./howto-manage-firewall-using-portal.md)
- [Azure CLI'yi kullanarak Sanal Ağ hizmeti uç noktalarını ve kurallarını oluşturarak ve yöneterek sunucunuza](howto-manage-vnet-using-cli.md)daha fazla erişim sağleyin.