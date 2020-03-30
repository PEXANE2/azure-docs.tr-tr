---
title: 'Quickstart: Az mysql up kullanarak MySQL için Azure Veritabanı oluşturma'
description: MySQL sunucusu için Azure CLI (komut satırı arabirimi) komutunu kullanarak Azure Veritabanı oluşturmak için hızlı başlangıç kılavuzu.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 3/18/2020
ms.custom: mvc
ms.openlocfilehash: 7b81e88fe6f658fdf4c1857c6082100894c6f2f6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80067712"
---
# <a name="quickstart-create-an-azure-database-for-mysql-using-a-simple-azure-cli-command---az-mysql-up-preview"></a>Quickstart: Basit bir Azure CLI komutunu kullanarak MySQL için bir Azure Veritabanı oluşturma - az mysql up (önizleme)

> [!IMPORTANT]
> [Az mysql up](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) Azure CLI komutu önizlemede.

MySQL için Azure Veritabanı, bulutta yüksek oranda kullanılabilir olan MySQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure CLI, komut satırından veya komut dosyasından Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç, Azure CLI'yi kullanarak MySQL sunucusu için bir Azure Veritabanı oluşturmak için [az mysql up](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) komutunu nasıl kullanacağınızı gösterir. `az mysql up` Komut, sunucuoluşturmaya ek olarak, veritabanında kök kullanıcı olan örnek bir veritabanı oluşturur, Azure hizmetleri için güvenlik duvarını açar ve istemci bilgisayar için varsayılan güvenlik duvarı kuralları oluşturur. Bu, geliştirme sürecini hızlandırmaya yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu makalede, Azure CLI sürüm 2.0 veya daha sonra yerel olarak çalıştırdığınızı gerektirir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

[Az giriş](/cli/azure/authenticate-azure-cli?view=interactive-log-in) komutunu kullanarak hesabınıza giriş yapmanız gerekir. Komut çıktısındaki ilgili abonelik adına karşılık gelen **id** özelliğinin değerini not edin.

```azurecli
az login
```

Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. [az account set](/cli/azure/account) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin. Az **giriş** çıkışından **abonelik kimliği** özelliğini abonelik kimliği yer tutucuya abone liğinize değiştirin.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma

Komutları kullanmak için [db-up](/cli/azure/ext/db-up) uzantısını yükleyin. Bir hata döndürülürse, Azure CLI'nin en son sürümünü yüklediğinizden emin olun. Bkz. [Azure CLI'yi Yükle](/cli/azure/install-azure-cli).

```azurecli
az extension add --name db-up
```

Aşağıdaki komutu kullanarak MySQL sunucusu için bir Azure Veritabanı oluşturun:

```azurecli
az mysql up
```

Sunucu aşağıdaki varsayılan değerlerle oluşturulur (bunları el ile geçersiz kılmadığınız sürece):

**Ayar** | **Varsayılan değer** | **Açıklama**
---|---|---
server-name | Oluşturulan sistem | Azure veritabanınızı MySQL sunucusuna tanıtan benzersiz bir ad.
resource-group | Oluşturulan sistem | Yeni bir Azure kaynak grubu.
sku-name | GP_Gen5_2 | Sku'nun adı. Kısaca {fiyatlandırma katmanı}\_{işlem nesli}\_{sanal çekirdek sayısı} kuralına uyar. Varsayılan 2 vCores ile Genel Amaçlı Gen5 sunucusudur. Katmanlar hakkında daha fazla bilgi için [fiyatlandırma sayfamıza](https://azure.microsoft.com/pricing/details/mysql/) bakın.
backup-retention | 7 | Yedeklemenin ne kadar süreyle tutulacağı. Birim olarak gün kullanılır.
geo-redundant-backup | Devre dışı | Coğrafi olarak yedekli yedeklemelerin bu sunucu için etkinleştirilip etkinleştirilmeyeceği.
location | westus2 | Sunucu için Azure konumu.
ssl-enforcement | Devre dışı | SSL'nin bu sunucu için etkin olup olmaması gerektiği.
storage-size | 5120 | Sunucunun depolama kapasitesi (birim olan megabayt kullanılır).
version | 5.7 | MySQL ana sürümü.
admin-user | Oluşturulan sistem | Yöneticinin oturum açma kullanıcı adı.
admin-password | Oluşturulan sistem | Yönetici kullanıcının parolası.

> [!NOTE]
> Komut ve ek parametreleri hakkında daha fazla bilgi için [Azure CLI belgelerine](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up)bakın. `az mysql up`

Sunucunuz oluşturulduktan sonra aşağıdaki ayarlarla birlikte gelir:

- "Devbox" adlı bir güvenlik duvarı kuralı oluşturulur. Azure CLI, `az mysql up` komutun çalıştırdığı makinenin IP adresini algılamaya çalışır ve IP adresinin beyaz listelerini listeler.
- "Azure hizmetlerine erişime izin ver" aveya aveya açık. Bu ayar, sunucunun güvenlik duvarını aboneliğinizde olmayan kaynaklar da dahil olmak üzere tüm Azure kaynaklarından gelen bağlantıları kabul etmek üzere yapılandırır.
- `wait_timeout` Parametre 8 saat olarak ayarlanır
- "sampledb" adlı boş bir veritabanı oluşturulur
- "Sampledb" ayrıcalıkları ile "kök" adlı yeni bir kullanıcı oluşturulur

> [!NOTE]
> MySQL için Azure Veritabanı, 3306 bağlantı noktası üzerinden iletişim kurar. Bir şirket ağı içinden bağlanırken, 3306 bağlantı noktası üzerindeki giden trafiğe ağınızın güvenlik duvarı izin verilmeyebilir. Sunucunuza bağlanmak için BT departmanınızın 3306 bağlantı noktasını açmasını sağlar.

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

`az mysql up` Komut tamamlandıktan sonra, popüler programlama dilleri için bağlantı dizeleri listesi size döndürülür. Bu bağlantı dizeleri, MySQL sunucusu için yeni oluşturduğunuz Azure Veritabanı'nın özel özellikleriyle önceden yapılandırılmıştır.

Bu bağlantı dizelerini yeniden listelemek için [az mysql show-connection-string](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-show-connection-string) komutunu kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aşağıdaki komutu kullanarak hızlı başlat'ta oluşturduğunuz tüm kaynakları temizleyin. Bu komut, MySQL sunucusu ve kaynak grubu için Azure Veritabanını siler.

```azurecli
az mysql down --delete-group
```

Eğer sadece yeni oluşturulan sunucu silmek istiyorsanız, [az mysql aşağı](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-down) komutu çalıştırabilirsiniz.

```azurecli
az mysql down
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI ile bir MySQL Veritabanı tasarlama](./tutorial-design-database-using-cli.md)
