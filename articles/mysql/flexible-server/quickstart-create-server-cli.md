---
title: 'Hızlı başlangıç: sunucu oluşturma-Azure CLı-bir MySQL için Azure veritabanı-esnek sunucu'
description: Bu hızlı başlangıçta bir Azure Kaynak grubunda MySQL için Azure veritabanı esnek sunucusu oluşturmak üzere Azure CLı 'nın nasıl kullanılacağı açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 9/21/2020
ms.custom: mvc
ms.openlocfilehash: bae6e9f04eced02130ae628d5308a87a1baaa8fa
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948495"
---
# <a name="quickstart-create-an-azure-database-for-mysql-flexible-server-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak MySQL için Azure veritabanı esnek sunucusu oluşturma

Bu hızlı başlangıçta, beş dakikada bir MySQL için Azure veritabanı esnek sunucusu oluşturmak üzere [Azure Cloud Shell](https://shell.azure.com) [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) komutlarının nasıl kullanılacağı gösterilmektedir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!IMPORTANT] 
> MySQL için Azure veritabanı esnek sunucu şu anda genel önizlemede

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell’i başlatma

[Azure Cloud Shell](../../cloud-shell/overview.md) , bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca, ' a giderek ayrı bir tarayıcı sekmesinde Cloud Shell de açabilirsiniz [https://shell.azure.com/bash](https://shell.azure.com/bash) . Kod bloklarını kopyalamak için **Kopyala** ' yı seçin, Cloud Shell yapıştırın ve çalıştırmak için **ENTER** ' u seçin.

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu hızlı başlangıç, Azure CLı sürüm 2,0 veya üzerini gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="prerequisites"></a>Önkoşullar

[Az Login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) komutunu kullanarak hesabınızda oturum açmanız gerekir. Azure hesabınızın **ABONELIK kimliğini** ifade eden **ID** özelliğine göz önüne alın.

```azurecli-interactive
az login
```

[Az Account set](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set) komutunu kullanarak hesabınız altındaki belirli bir aboneliği seçin. Komutta **abonelik** bağımsız değişkeninin değeri olarak kullanılacak **az Login** çıktısından **ID** değerini bir yere unutmayın. Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. Aboneliğinizi tamamen almak için [az Account List](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-list)kullanın.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-a-flexible-server"></a>Esnek sunucu oluşturma

Komutunu kullanarak bir [Azure Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) oluşturun `az group create` ve ardından bu kaynak grubunun içinde MySQL esnek sunucunuzu oluşturun. Benzersiz bir ad sağlamanız gerekir. Aşağıdaki örnek `westus` konumunda `myresourcegroup` adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

Komutuyla esnek bir sunucu oluşturun `az mysql flexible-server create` . Bir sunucu birden çok veritabanı içerebilir. Aşağıdaki komut, Azure CLı 'nın [Yerel bağlamından](https://docs.microsoft.com/cli/azure/local-context?view=azure-cli-latest)hizmet varsayılanlarını ve değerlerini kullanarak bir sunucu oluşturur: 

```azurecli
az mysql flexible-server create
```

Oluşturulan sunucu aşağıdaki özniteliklere sahiptir: 
- Otomatik olarak oluşturulan sunucu adı, Yönetici Kullanıcı adı, yönetici parolası, kaynak grubu adı (zaten yerel bağlamda belirtilmemişse) ve kaynak grubunuzda aynı konumda 
- Kalan sunucu yapılandırmalarının hizmet Varsayılanları: İşlem Katmanı (Burstable), işlem boyutu/SKU (B1MS), yedekleme saklama süresi (7 gün) ve MySQL sürümü (5,7)
- Varsayılan bağlantı yöntemi, otomatik olarak oluşturulan bir sanal ağ ve alt ağ ile özel erişime (VNet tümleştirme) sahiptir

> [!NOTE] 
> Sunucu oluşturulduktan sonra bağlantı yöntemi değiştirilemez. Örneğin, oluştur sırasında *özel erişim (VNET tümleştirmesi)* seçtiyseniz, oluşturulduktan sonra *ortak erişime (izin verilen IP adresleri)* geçiş yapılamaz. VNet tümleştirmesini kullanarak sunucunuza güvenli bir şekilde erişmek için özel erişime sahip bir sunucu oluşturmanız önemle önerilir. [Kavramlar makalesinde](./concepts-networking.md)özel erişim hakkında daha fazla bilgi edinin.

Herhangi bir Varsayılanı değiştirmek istiyorsanız lütfen Azure CLı başvuru belgelerine başvurun <!--FIXME --> yapılandırılabilir CLı parametrelerinin tüm listesi için. 

> [!NOTE]
> MySQL için Azure Veritabanı bağlantıları 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece sunucunuza bağlanamazsınız.

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.

```azurecli-interactive
az mysql flexible-server show --resource-group myresourcegroup --name mydemoserver
```

Sonuç JSON biçimindedir. **fullyQualifiedDomainName** ve **administratorLogin** bilgilerini not alın.

<!--FIXME-->
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/flexibleServers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 1,
    "name": "Standard_B1ms",
    "size": null,
    "tier": "Burstable"
  },
  "publicAccess": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforMySQL/flexibleServers",
  "userVisibleState": "Ready",
  "version": "5.7"
}
```

## <a name="connect-using-mysql-command-line-client"></a>MySQL komut satırı istemcisini kullanarak bağlanma

Esnek sunucu *özel erişim (VNET tümleştirmesi)* ile oluşturulduğundan, sunucunuza aynı VNET içindeki bir kaynaktan sunucunuza bağlanmanız gerekir. Bir sanal makine oluşturup oluşturulan sanal ağa ekleyebilirsiniz. 

VM 'niz oluşturulduktan sonra makineye SSH oluşturabilir ve popüler istemci aracı **[mysql.exe](https://dev.mysql.com/downloads/)** komut satırı aracını yükleyebilirsiniz.

mysql.exe, aşağıdaki komutu kullanarak bağlanın. Değerleri gerçek sunucu adı ve parolasıyla değiştirin. 

```bash
 mysql -h mydemoserver.mysql.database.azure.com -u mydemouser -p
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklara başka bir hızlı başlangıç/öğretici için gereksinim duymuyorsanız aşağıdaki komutu çalıştırarak kaynakları silebilirsiniz:

```azurecli-interactive
az group delete --name myresourcegroup
```

Yalnızca yeni oluşturulan sunucuyu silmek istiyorsanız `az mysql server delete` komutunu çalıştırabilirsiniz.

```azurecli-interactive
az mysql flexible-server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[MySQL ile PHP (Laralevel) Web uygulaması oluşturma](tutorial-php-database-app.md)