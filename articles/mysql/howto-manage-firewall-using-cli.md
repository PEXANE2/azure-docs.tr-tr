---
title: Güvenlik duvarı kurallarını yönetme-Azure CLı-MySQL için Azure veritabanı
description: Bu makalede, Azure CLı komut satırı kullanılarak MySQL için Azure Güvenlik duvarı kuralları oluşturma ve yönetme açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 11aa4a80deba4df14c239e69910ea38bac1b9c55
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80063525"
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Azure CLı kullanarak MySQL için Azure Güvenlik duvarı kuralları oluşturma ve yönetme
Sunucu düzeyinde güvenlik duvarı kuralları, belirli bir IP adresinden veya bir IP adresi aralığından MySQL için Azure veritabanı sunucusuna erişimi yönetmek için kullanılabilir. Uygun Azure CLı komutlarını kullanarak sunucunuzu yönetmek için güvenlik duvarı kuralları oluşturabilir, güncelleştirebilir, silebilir, listeleyebilir ve gösterebilirsiniz. MySQL güvenlik duvarları için Azure veritabanı 'na genel bakış için bkz. [MySQL Için Azure veritabanı sunucu güvenlik duvarı kuralları](./concepts-firewall-rules.md).

Sanal ağ (VNet) kuralları, sunucunuza erişimi güvenli hale getirmek için de kullanılabilir. [Azure CLI kullanarak sanal ağ hizmet uç noktaları ve kuralları oluşturma ve yönetme](howto-manage-vnet-using-cli.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar
* [Azure CLI’yı yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli).
* [MySQL Için Azure veritabanı sunucusu ve veritabanı](quickstart-create-mysql-server-database-using-azure-cli.md).

## <a name="firewall-rule-commands"></a>Güvenlik duvarı kuralı komutları:
**Az MySQL Server Firewall-Rule** komutu, Azure CLI 'dan güvenlik duvarı kuralları oluşturmak, silmek, listelemek, göstermek ve güncelleştirmek için kullanılır.

Komut
- **oluşturma**: Azure MySQL Server güvenlik duvarı kuralı oluşturun.
- **Sil**: bir Azure MySQL Server güvenlik duvarı kuralını silin.
- **liste**: Azure MySQL Server güvenlik duvarı kurallarını listeleyin.
- **göster**: bir Azure MySQL Server güvenlik duvarı kuralının ayrıntılarını göster.
- **güncelleştirme**: Azure MySQL Server güvenlik duvarı kuralını güncelleştirin.

## <a name="sign-in-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Azure 'da oturum açın ve MySQL sunucuları için Azure veritabanınızı listeleyin
**Az Login** komutunu kullanarak Azure CLI 'yı Azure hesabınızla güvenli bir şekilde bağlayın.

1. Komut satırından aşağıdaki komutu çalıştırın:
    ```azurecli
    az login
    ```
   Bu komut, bir sonraki adımda kullanmak üzere bir kod verir.

2. Sayfayı [https://aka.ms/devicelogin](https://aka.ms/devicelogin)açmak için bir Web tarayıcısı kullanın ve ardından kodu girin.

3. Sorulduğunda, Azure kimlik bilgilerinizi kullanarak oturum açın.

4. Oturum açma yetkilendirildikten sonra, konsolda aboneliklerin listesi yazdırılır. Geçerli aboneliği kullanmak üzere ayarlamak için istenen aboneliğin KIMLIĞINI kopyalayın. [Az Account set](/cli/azure/account#az-account-set) komutunu kullanın.
    ```azurecli-interactive
    az account set --subscription <your subscription id>
    ```

5. Adlarından emin değilseniz, aboneliğiniz ve kaynak grubunuz için MySQL sunucularının Azure veritabanlarını listeleyin. [Az MySQL Server List](/cli/azure/mysql/server#az-mysql-server-list) komutunu kullanın.

    ```azurecli-interactive
    az mysql server list --resource-group myresourcegroup
    ```

   Üzerinde çalışmak üzere MySQL sunucusunu belirtmeniz gereken, listede ad özniteliğini aklınızda bulabilirsiniz. Gerekirse, bu sunucunun ayrıntılarını onaylayın ve ad özniteliğini kullanarak doğru olduğundan emin olun. [Az MySQL Server Show](/cli/azure/mysql/server#az-mysql-server-show) komutunu kullanın.

    ```azurecli-interactive
    az mysql server show --resource-group myresourcegroup --name mydemoserver
    ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı 'nda güvenlik duvarı kurallarını listeleme 
Sunucu adını ve kaynak grubu adını kullanarak, sunucuda var olan sunucu güvenlik duvarı kurallarını listeleyin. [Az MySQL Server Firewall List](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-list) komutunu kullanın.  Sunucu adı özniteliğinin-- **Name** anahtarında değil, **--Server** anahtarında belirtildiğine dikkat edin. 
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Çıktı, varsa, JSON biçiminde (varsayılan olarak) kuralları listeler. Sonuçları daha okunabilir bir tablo biçiminde çıkarmak için **--output tablosu** anahtarını kullanabilirsiniz.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>MySQL için Azure veritabanı sunucusu 'nda bir güvenlik duvarı kuralı oluşturma
Azure MySQL sunucu adını ve kaynak grubu adını kullanarak sunucuda yeni bir güvenlik duvarı kuralı oluşturun. [Az MySQL Server Firewall Create](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create) komutunu kullanın. Kural için bir ad, başlangıç IP 'si ve bitiş IP 'si (bir IP adresi aralığına erişim sağlamak için) belirtin.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Tek bir IP adresinin erişimine izin vermek için, bu örnekte olduğu gibi, başlangıç IP adresi ve bitiş IP 'si ile aynı IP adresini sağlayın.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```

Azure IP adreslerinden gelen uygulamaların MySQL sunucusu için Azure veritabanı 'na bağlanmasına izin vermek için, bu örnekte olduğu gibi, başlangıç IP adresi ve bitiş IP 'si olarak 0.0.0.0 IP adresini sağlayın.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mysql --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

Başarılı olduğunda, her bir komut çıkışı oluşturma, oluşturduğunuz güvenlik duvarı kuralının (varsayılan olarak) JSON biçiminde ayrıntılarını listeler. Bir hata oluşursa, çıkış bunun yerine hata iletisi metnini gösterir.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı 'nda bir güvenlik duvarı kuralı güncelleştirme 
Azure MySQL sunucu adını ve kaynak grubu adını kullanarak, sunucuda var olan bir güvenlik duvarı kuralını güncelleştirin. [Az MySQL Server Firewall Update](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-update) komutunu kullanın. Mevcut güvenlik duvarı kuralının adını girdi olarak ve güncelleştirilecek başlangıç IP ve bitiş IP özniteliklerini belirtin.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Başarılı olduğunda, komut çıktısı güncelleştirdiğiniz güvenlik duvarı kuralının ayrıntılarını JSON biçiminde (varsayılan olarak) listeler. Bir hata oluşursa, çıkış bunun yerine hata iletisi metnini gösterir.

> [!NOTE]
> Güvenlik duvarı kuralı yoksa, kural Update komutu tarafından oluşturulur.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı 'nda güvenlik duvarı kuralı ayrıntılarını göster
Azure MySQL sunucu adını ve kaynak grubu adını kullanarak, sunucudan mevcut güvenlik duvarı kuralı ayrıntılarını gösterin. [Az MySQL Server Firewall Show](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-show) komutunu kullanın. Mevcut güvenlik duvarı kuralının adını girdi olarak belirtin.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Başarılı olduğunda komut çıktısı, belirttiğiniz güvenlik duvarı kuralının, JSON biçiminde (varsayılan olarak) ayrıntılarını listeler. Bir hata oluşursa, çıkış bunun yerine hata iletisi metnini gösterir.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı 'nda bir güvenlik duvarı kuralı silme
Azure MySQL sunucu adını ve kaynak grubu adını kullanarak, var olan bir güvenlik duvarı kuralını sunucudan kaldırın. [Az MySQL Server Firewall Delete](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-delete) komutunu kullanın. Mevcut güvenlik duvarı kuralının adını belirtin.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Başarılı olduğunda, çıkış yok. Hata durumunda hata iletisi metni görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar
- [MySQL Için Azure veritabanı sunucu güvenlik duvarı kuralları](./concepts-firewall-rules.md)hakkında daha fazla bilgi edinin.
- [Azure Portal kullanarak MySQL Için Azure Güvenlik duvarı kuralları oluşturun ve yönetin](./howto-manage-firewall-using-portal.md).
- [Azure CLI kullanarak sanal ağ hizmet uç noktaları ve kuralları oluşturup yöneterek](howto-manage-vnet-using-cli.md)sunucunuza daha güvenli bir şekilde erişin.