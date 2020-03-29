---
title: Güvenlik duvarı kurallarını yönetme - Azure CLI - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, Azure CLI komut satırını kullanarak PostgreSQL - Single Server için Azure Veritabanı'nda güvenlik duvarı kurallarının nasıl oluşturulup yönetilen anlatılmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 4af0fb288961689fb051bab8091c838f793cfcc3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74765656"
---
# <a name="create-and-manage-firewall-rules-in-azure-database-for-postgresql---single-server-using-azure-cli"></a>Azure CLI kullanarak PostgreSQL - Single Server için Azure Veritabanı'nda güvenlik duvarı kuralları oluşturma ve yönetme
Sunucu düzeyinde güvenlik duvarı kuralları, belirli bir IP adresinden veya IP adresi aralığından PostgreSQL Server için Azure Veritabanına erişimi yönetmek için kullanılabilir. Kullanışlı Azure CLI komutlarını kullanarak, sunucunuzu yönetmek için güvenlik duvarı kurallarını oluşturabilir, güncelleyebilir, silebilir, listeleyebilir ve gösterebilirsiniz. PostgreSQL güvenlik duvarı kuralları için Azure Veritabanı'na genel bakış için [PostgreSQL Server güvenlik duvarı kuralları için Azure Veritabanı'na](concepts-firewall-rules.md)bakın.

Sanal Ağ (VNet) kuralları sunucunuza erişimi güvence altına almak için de kullanılabilir. [Azure CLI'yi kullanarak Sanal Ağ hizmeti uç noktalarını ve kurallarını oluşturma ve yönetme](howto-manage-vnet-using-cli.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programını yükleyin veya tarayıcıda Azure Bulut Kabuğu'nu kullanın.
- [PostgreSQL sunucusu ve veritabanı için](quickstart-create-server-database-azure-cli.md)bir Azure Veritabanı.

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı için güvenlik duvarı kurallarını yapılandırma
[Az postgres sunucusu güvenlik duvarı kuralı](/cli/azure/postgres/server/firewall-rule) komutları güvenlik duvarı kurallarını yapılandırmak için kullanılır.

## <a name="list-firewall-rules"></a>Güvenlik duvarı kurallarını listele 
Varolan sunucu güvenlik duvarı kurallarını listelemek için [az postgres sunucusu güvenlik duvarı kuralı listesini](/cli/azure/postgres/server/firewall-rule) çalıştırın.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Çıktı, varsa varsayılan olarak JSON biçiminde güvenlik duvarı kurallarını listeler. Çıkış olarak daha `--output table` okunabilir bir tablo biçimi için anahtarı kullanabilirsiniz.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-firewall-rule"></a>Güvenlik duvarı kuralı oluşturma
Sunucuda yeni bir güvenlik duvarı kuralı oluşturmak için az postgres sunucusu güvenlik duvarı kuralı oluşturma komutunu [çalıştırın.](/cli/azure/postgres/server/firewall-rule) 

```
To allow access to a singular IP address, provide the same address in the `--start-ip-address` and `--end-ip-address`, as in this example, replacing the IP shown here with your specific IP.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Azure IP adreslerinden uygulamaların PostgreSQL sunucusu için Azure Veritabanınıza bağlanmasına izin vermek için, bu örnekte olduğu gibi IP 0.0.0.0 ip adresini Başlat IP ve End IP olarak sağlayın.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowAllAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

Başarıya uyunüzerine, komut çıktısı varsayılan olarak JSON biçiminde oluşturduğunuz güvenlik duvarı kuralının ayrıntılarını listeler. Bir hata varsa, çıktı yerine bir hata iletisi gösterir.

## <a name="update-firewall-rule"></a>Güvenlik duvarı kuralını güncelleştirme 
[Az postgres sunucusu güvenlik duvarı kuralı güncelleştirme](/cli/azure/postgres/server/firewall-rule) komutunu kullanarak sunucuda varolan bir güvenlik duvarı kuralını güncelleştirin. Varolan güvenlik duvarı kuralının adını giriş olarak ve güncelleştirmek için IP ve end IP özniteliklerini başlatın.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.0
```
Başarıya uyunüzerine, komut çıktısı varsayılan olarak JSON biçiminde güncellediğiniz güvenlik duvarı kuralının ayrıntılarını listeler. Bir hata varsa, çıktı yerine bir hata iletisi gösterir.
> [!NOTE]
> Güvenlik duvarı kuralı yoksa, güncelleştirme komutu tarafından oluşturulur.

## <a name="show-firewall-rule-details"></a>Güvenlik duvarı kuralı ayrıntılarını göster
Ayrıca, [az postgres sunucusu güvenlik duvarı kuralı göster](/cli/azure/postgres/server/firewall-rule) komutunu çalıştırarak varolan sunucu düzeyindeki güvenlik duvarı kuralının ayrıntılarını da gösterebilirsiniz.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Başarıya uyunüzerine, komut çıktısı varsayılan olarak JSON biçiminde belirttiğiniz güvenlik duvarı kuralının ayrıntılarını listeler. Bir hata varsa, çıktı yerine bir hata iletisi gösterir.

## <a name="delete-firewall-rule"></a>Güvenlik duvarı kuralını silme
Sunucuya bir IP aralığı nın erişimini iptal etmek için, [az postgres sunucusu güvenlik duvarı kuralı silme](/cli/azure/postgres/server/firewall-rule) komutunu çalıştırarak varolan bir güvenlik duvarı kuralını silin. Varolan güvenlik duvarı kuralının adını sağlayın.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Başarı üzerine, hiçbir çıkış yoktur. Hata üzerine, hata iletisi metni döndürülür.

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, [Azure portalını kullanarak PostgreSQL güvenlik duvarı kuralları için Azure Veritabanı oluşturmak ve yönetmek için](howto-manage-firewall-using-portal.md)bir web tarayıcısı kullanabilirsiniz.
- [PostgreSQL Server güvenlik duvarı kuralları için Azure Veritabanı](concepts-firewall-rules.md)hakkında daha fazla bilgi edinin.
- [Azure CLI'yi kullanarak Sanal Ağ hizmeti uç noktalarını ve kurallarını oluşturarak ve yöneterek sunucunuza](howto-manage-vnet-using-cli.md)daha fazla erişim sağleyin.
- PostgreSQL sunucusu için bir Azure Veritabanına bağlanma konusunda yardım için, [PostgreSQL için Azure Veritabanı bağlantı kitaplıklarına](concepts-connection-libraries.md)bakın.
