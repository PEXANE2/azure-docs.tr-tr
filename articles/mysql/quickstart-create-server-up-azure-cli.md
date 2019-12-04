---
title: 'Hızlı başlangıç: az MySQL up kullanarak MySQL için Azure veritabanı oluşturma'
description: Azure CLı (komut satırı arabirimi) up komutunu kullanarak MySQL için Azure veritabanı sunucusu oluşturmak üzere hızlı başlangıç kılavuzu.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 12/02/2019
ms.custom: mvc
ms.openlocfilehash: 4bb5c62a7df53548ff59a03c6ccc8fb28f1503d3
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74765692"
---
# <a name="quickstart-create-an-azure-database-for-mysql-using-a-simple-azure-cli-command---az-mysql-up-preview"></a>Hızlı başlangıç: basit bir Azure CLı komutu kullanarak MySQL için Azure veritabanı oluşturma-az MySQL up (Önizleme)

> [!IMPORTANT]
> [Az MySQL](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) Azure CLI komutu önizlemededir.

MySQL için Azure Veritabanı, bulutta yüksek oranda kullanılabilir olan MySQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure CLI, komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta, Azure CLı kullanarak MySQL için Azure veritabanı sunucusu oluşturmak üzere [az MySQL up](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) komutunun nasıl kullanılacağı gösterilmektedir. Sunucu oluşturmaya ek olarak, `az mysql up` komutu bir örnek veritabanı, veritabanında bir kök kullanıcı oluşturur, Azure hizmetleri için güvenlik duvarını açar ve istemci bilgisayar için varsayılan güvenlik duvarı kuralları oluşturur. Bu, geliştirme sürecini hızlandırmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu makalede, Azure CLı sürüm 2,0 veya üstünü yerel olarak çalıştırıyor olmanız gerekir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

[Az Login](/cli/azure/authenticate-azure-cli?view=interactive-log-in) komutunu kullanarak hesabınızda oturum açmanız gerekir. Komut çıktısındaki ilgili abonelik adına karşılık gelen **id** özelliğinin değerini not edin.

```azurecli
az login
```

Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. [az account set](/cli/azure/account) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin. **Abonelik kimliği özelliğini aboneliğiniz** için **az oturum açma** çıktısından abonelik kimliği yer tutucusuna koyun.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma

Komutları kullanmak için, [DB-up](/cli/azure/ext/db-up) uzantısını yükler. Bir hata döndürülürse, Azure CLı 'nin en son sürümünü yüklediğinizden emin olun. Bkz. [Azure CLI 'Yi yüklemeyi](/cli/azure/install-azure-cli).

```azurecli
az extension add --name db-up
```

Aşağıdaki komutu kullanarak MySQL için Azure veritabanı sunucusu oluşturun:

```azurecli
az mysql up
```

Sunucu, aşağıdaki varsayılan değerlerle oluşturulur (bunları el ile geçersiz kılmadığınız müddetçe):

**Ayar** | **Varsayılan değer** | **Açıklama**
---|---|---
server-name | Sistem tarafından oluşturulan | Azure veritabanınızı MySQL sunucusuna tanıtan benzersiz bir ad.
resource-group | Sistem tarafından oluşturulan | Yeni bir Azure Kaynak grubu.
sku-name | GP_Gen5_2 | Sku'nun adı. Kısaca {fiyatlandırma katmanı}\_{işlem nesli}\_{sanal çekirdek sayısı} kuralına uyar. Varsayılan değer 2 sanal çekirdekte olan bir Genel Amaçlı 5. nesil sunucusudur. Katmanlar hakkında daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/mysql/) .
backup-retention | 7 | Yedeklemenin ne kadar süreyle tutulacağı. Birim olarak gün kullanılır.
geo-redundant-backup | Devre dışı | Coğrafi olarak yedekli yedeklemelerin bu sunucu için etkinleştirilip etkinleştirilmeyeceği.
location | westus2 | Sunucu için Azure konumu.
ssl-enforcement | Devre dışı | Bu sunucu için ssl'in etkinleştirilip etkinleştirilmeyeceği.
storage-size | 5120 | Sunucunun depolama kapasitesi (birim olan megabayt kullanılır).
version | 5.7 | MySQL ana sürümü.
admin-user | Sistem tarafından oluşturulan | Yöneticinin oturum açma kullanıcı adı.
admin-password | Sistem tarafından oluşturulan | Yönetici kullanıcının parolası.

> [!NOTE]
> `az mysql up` komutu ve ek parametreleri hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up).

Sunucunuz oluşturulduktan sonra, aşağıdaki ayarlarla birlikte gelir:

- "Devbox" adlı bir güvenlik duvarı kuralı oluşturulur. Azure CLı, `az mysql up` komutun çalıştırıldığı makinenin IP adresini algılamaya çalışır ve o IP adresini beyaz listeler.
- "Azure hizmetlerine erişime izin ver" ayarı açık olarak ayarlanmıştır. Bu ayar, sunucu güvenlik duvarını aboneliğinizdeki kaynaklar dahil olmak üzere tüm Azure kaynaklarından gelen bağlantıları kabul edecek şekilde yapılandırır.
- `wait_timeout` parametresi 8 saate ayarlanır
- "SampleDB" adlı boş bir veritabanı oluşturuldu
- "SampleDB" ayrıcalıklarına sahip "root" adlı yeni bir Kullanıcı oluşturuldu

> [!NOTE]
> MySQL için Azure veritabanı 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanırken, ağınızın güvenlik duvarı tarafından 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. BT departmanınızın sunucunuza bağlanmak için 3306 bağlantı noktasını açmasını sağlayabilirsiniz.

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

`az mysql up` komutu tamamlandıktan sonra, size popüler programlama dillerinin bağlantı dizelerinin bir listesi döndürülür. Bu bağlantı dizeleri, yeni oluşturduğunuz MySQL Server için Azure veritabanı 'nın belirli öznitelikleriyle önceden yapılandırılmıştır.

Bu bağlantı dizelerini yeniden listelemek için [az MySQL Show-Connection-String](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-show-connection-string) komutunu kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aşağıdaki komutu kullanarak hızlı başlangıçta oluşturduğunuz tüm kaynakları temizleyin. Bu komut, MySQL için Azure veritabanı sunucusunu ve kaynak grubunu siler.

```azurecli
az mysql down --delete-group
```

Yalnızca yeni oluşturulan sunucuyu silmek istiyorsanız [az MySQL](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-down) i komutunu çalıştırabilirsiniz.

```azurecli
az mysql down
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI ile bir MySQL Veritabanı tasarlama](./tutorial-design-database-using-cli.md)
