---
title: Güvenlik duvarı kurallarını yönetme-Azure CLı-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure CLı komut satırını kullanarak PostgreSQL için Azure veritabanı-tek sunuculu Güvenlik Duvarı kurallarının nasıl oluşturulacağı ve yönetileceği açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 4af0fb288961689fb051bab8091c838f793cfcc3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74765656"
---
# <a name="create-and-manage-firewall-rules-in-azure-database-for-postgresql---single-server-using-azure-cli"></a>Azure CLı kullanarak PostgreSQL için Azure veritabanı 'nda güvenlik duvarı kuralları oluşturma ve yönetme-tek sunucu
Sunucu düzeyinde güvenlik duvarı kuralları, bir PostgreSQL için Azure veritabanı sunucusuna erişimi belirli bir IP adresinden veya IP adresi aralığından yönetmek için kullanılabilir. Uygun Azure CLı komutlarını kullanarak sunucunuzu yönetmek için güvenlik duvarı kuralları oluşturabilir, güncelleştirebilir, silebilir, listeleyebilir ve gösterebilirsiniz. PostgreSQL için Azure veritabanı güvenlik duvarı kurallarına genel bakış için bkz. [PostgreSQL Için Azure veritabanı sunucu güvenlik duvarı kuralları](concepts-firewall-rules.md).

Sanal ağ (VNet) kuralları, sunucunuza erişimi güvenli hale getirmek için de kullanılabilir. [Azure CLI kullanarak sanal ağ hizmet uç noktaları ve kuralları oluşturma ve yönetme](howto-manage-vnet-using-cli.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programını yükler veya tarayıcıda Azure Cloud Shell kullanın.
- [PostgreSQL Için Azure veritabanı sunucusu ve veritabanı](quickstart-create-server-database-azure-cli.md).

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı güvenlik duvarı kurallarını yapılandırma
[Az Postgres Server Firewall-Rule](/cli/azure/postgres/server/firewall-rule) komutları, güvenlik duvarı kurallarını yapılandırmak için kullanılır.

## <a name="list-firewall-rules"></a>Güvenlik duvarı kurallarını Listele 
Mevcut sunucu güvenlik duvarı kurallarını listelemek için [az Postgres Server Firewall-Rule List](/cli/azure/postgres/server/firewall-rule) komutunu çalıştırın.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Çıktı, varsa, varsayılan olarak JSON biçiminde güvenlik duvarı kurallarını listeler. Anahtarı, `--output table` çıkış olarak daha okunabilir bir tablo biçimi için kullanabilirsiniz.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-firewall-rule"></a>Güvenlik duvarı kuralı oluşturma
Sunucuda yeni bir güvenlik duvarı kuralı oluşturmak için [az Postgres Server Firewall-Rule Create](/cli/azure/postgres/server/firewall-rule) komutunu çalıştırın. 

```
To allow access to a singular IP address, provide the same address in the `--start-ip-address` and `--end-ip-address`, as in this example, replacing the IP shown here with your specific IP.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Azure IP adreslerinden gelen uygulamaların PostgreSQL için Azure veritabanı sunucusuna bağlanmasına izin vermek için, bu örnekte olduğu gibi, başlangıç IP 'si ve bitiş IP 'si olarak 0.0.0.0 IP adresini sağlayın.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowAllAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

Başarılı olduğunda komut çıktısı, varsayılan olarak JSON biçiminde oluşturduğunuz güvenlik duvarı kuralının ayrıntılarını listeler. Bir hata oluşursa, çıkış bunun yerine bir hata iletisi gösterir.

## <a name="update-firewall-rule"></a>Güvenlik duvarı kuralını Güncelleştir 
[Az Postgres Server Firewall-Rule Update](/cli/azure/postgres/server/firewall-rule) komutunu kullanarak sunucuda var olan bir güvenlik duvarı kuralını güncelleştirin. Mevcut güvenlik duvarı kuralının adını girdi olarak ve güncelleştirilecek başlangıç IP ve bitiş IP özniteliklerini belirtin.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.0
```
Başarılı olduğunda komut çıktısı, varsayılan olarak JSON biçiminde, güncelleştirdiğiniz güvenlik duvarı kuralının ayrıntılarını listeler. Bir hata oluşursa, çıkış bunun yerine bir hata iletisi gösterir.
> [!NOTE]
> Güvenlik duvarı kuralı yoksa, Update komutu tarafından oluşturulur.

## <a name="show-firewall-rule-details"></a>Güvenlik duvarı kuralı ayrıntılarını göster
[Az Postgres Server Firewall-Rule Show](/cli/azure/postgres/server/firewall-rule) komutunu çalıştırarak var olan sunucu düzeyi güvenlik duvarı kuralının ayrıntılarını da gösterebilirsiniz.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Başarılı olduğunda komut çıktısı, belirttiğiniz güvenlik duvarı kuralının, varsayılan olarak JSON biçiminde ayrıntılarını listeler. Bir hata oluşursa, çıkış bunun yerine bir hata iletisi gösterir.

## <a name="delete-firewall-rule"></a>Güvenlik duvarı kuralını Sil
Sunucuya bir IP aralığı erişimini iptal etmek için [az Postgres Server Firewall-Rule Delete](/cli/azure/postgres/server/firewall-rule) komutunu yürüterek mevcut bir güvenlik duvarı kuralını silin. Mevcut güvenlik duvarı kuralının adını belirtin.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Başarılı olduğunda, çıkış yok. Hata durumunda hata iletisi metni döndürülür.

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, [Azure Portal kullanarak PostgreSQL Için Azure veritabanı güvenlik duvarı kuralları oluşturmak ve yönetmek](howto-manage-firewall-using-portal.md)üzere bir Web tarayıcısını kullanabilirsiniz.
- [PostgreSQL Için Azure veritabanı sunucu güvenlik duvarı kuralları](concepts-firewall-rules.md)hakkında daha fazla bilgi edinin.
- [Azure CLI kullanarak sanal ağ hizmet uç noktaları ve kuralları oluşturup yöneterek](howto-manage-vnet-using-cli.md)sunucunuza daha güvenli bir şekilde erişin.
- PostgreSQL için Azure veritabanı sunucusuna bağlanma konusunda yardım için bkz. [PostgreSQL Için Azure veritabanı bağlantı kitaplıkları](concepts-connection-libraries.md).
