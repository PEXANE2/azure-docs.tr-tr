---
title: Yedekleme ve geri yükleme - Azure CLI - MariaDB için Azure Veritabanı
description: Azure CLI'yi kullanarak MariaDB için Azure Veritabanı'ndaki bir sunucuyu nasıl yedekleyip geri yükleyin öğrenin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 3/27/2020
ms.openlocfilehash: 6faae80c78fe07d33579cc3fb7c76ce668969992
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369263"
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mariadb-using-the-azure-cli"></a>Azure CLI'yi kullanarak MariaDB için Azure Veritabanı'ndaki bir sunucuyu yedekleme ve geri yükleme

MariaDB sunucuları için Azure Veritabanı, Geri Yükleme özelliklerini etkinleştirmek için düzenli aralıklarla yedeklenir. Bu özelliği kullanarak, sunucuyu ve tüm veritabanlarını yeni bir sunucuda daha önceki bir zaman diliminde geri yükleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:

- [MariaDB sunucusu ve veritabanı için](quickstart-create-mariadb-server-database-using-azure-cli.md) bir Azure Veritabanı

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılsa kılavuzu, Azure CLI sürüm 2.0 veya sonrası kullanmanızı gerektirir. Sürümü onaylamak için Azure CLI komut istemine girin. `az --version` Yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye]( /cli/azure/install-azure-cli)bakın.

## <a name="set-backup-configuration"></a>Yedekleme yapılandırması ayarlama

Sunucunuzu yerel olarak yedekli yedeklemeler veya sunucu oluşturmada coğrafi olarak yedek yedeklemeler için yapılandırmak arasında seçim yaparsınız.

> [!NOTE]
> Bir sunucu oluşturulduktan sonra, sahip olduğu artıklık türü, coğrafi olarak gereksiz vs yerel gereksiz, değiştirilemez.
>

`az mariadb server create` Komut aracılığıyla bir sunucu oluştururken, `--geo-redundant-backup` parametre Yedek Artıklık Seçeneği'ne karar verir. , `Enabled`coğrafi yedek yedekler alınırsa. Veya `Disabled` yerel olarak gereksiz yedeklemeler alınırsa.

Yedekleme bekletme süresi parametre `--backup-retention`tarafından ayarlanır.

Oluşturma sırasında bu değerleri ayarlama hakkında daha fazla bilgi için [MariaDB sunucusu CLI Quickstart için Azure Veritabanı'na](quickstart-create-mariadb-server-database-using-azure-cli.md)bakın.

Bir sunucunun yedekleme bekletme süresi aşağıdaki gibi değiştirilebilir:

```azurecli-interactive
az mariadb server update --name mydemoserver --resource-group myresourcegroup --backup-retention 10
```

Önceki örnek, mydemoserver yedekleme bekletme süresini 10 güne değiştirir.

Yedekleme bekletme süresi, kullanılabilir yedeklemelere dayandığı için zaman içinde bir geri yüklemenin ne kadar geri alınabileceğini yönetir. Zaman içinde geri yükleme sonraki bölümde daha ayrıntılı olarak açıklanmıştır.

## <a name="server-point-in-time-restore"></a>Sunucu zamanında geri yükleme

Sunucuyu önceki bir noktaya geri yükleyebilirsiniz. Geri yüklenen veriler yeni bir sunucuya kopyalanır ve varolan sunucu olduğu gibi bırakılır. Örneğin, bugün öğlen yanlışlıkla bir tablo düşerse, öğlenden hemen önce zamangeri yükleyebilirsiniz. Ardından, eksik tabloyu ve verileri sunucunun geri yüklenen kopyasından alabilirsiniz.

Sunucuyu geri yüklemek için Azure CLI [az mariadb sunucu geri yükleme](/cli/azure/mariadb/server#az-mariadb-server-restore) komutunu kullanın.

### <a name="run-the-restore-command"></a>Geri yükleme komutunu çalıştırın

Sunucuyu geri yüklemek için Azure CLI komut isteminde aşağıdaki komutu girin:

```azurecli-interactive
az mariadb server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time 2018-03-13T13:59:00Z --source-server mydemoserver
```

Komut `az mariadb server restore` aşağıdaki parametreleri gerektirir:

| Ayar | Önerilen değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Kaynak sunucunun bulunduğu kaynak grubu.  |
| ad | mydemoserver-restored | Geri yükleme komutu tarafından oluşturulan yeni sunucunun adı. |
| restore-point-in-time | 2018-03-13T13:59:00Z | Geri yüklemek için zaman içinde bir nokta seçin. Bu tarih ve saat, kaynak sunucunun yedekleme saklama dönemi içinde olmalıdır. ISO8601 tarih ve saat biçimini kullanın. Örneğin, kendi yerel saat diliminizi kullanabilirsiniz, örneğin. `2018-03-13T05:59:00-08:00` UtC Zulu biçimini de kullanabilirsiniz, `2018-03-13T13:59:00Z`örneğin. |
| source-server | mydemoserver | Geri yükleme kaynağı olarak kullanılacak sunucunun adı veya kimliği. |

W Sunucuydan daha önceki bir noktaya geri yüklediğinizde, yeni bir sunucu oluşturulur. Zaman içinde belirtilen noktadan orijinal sunucu ve veritabanları yeni sunucuya kopyalanır.

Geri yüklenen sunucunun konum ve fiyatlandırma katmanı değerleri özgün sunucuyla aynı kalır. 

Geri yükleme işlemi bittikten sonra, yeni sunucuyu bulun ve verilerin beklendiği gibi geri yüklenmiş olduğundan doğrulayın. Yeni sunucu, geri yüklemenin başlatıldığı sırada varolan sunucu için geçerli olan sunucu yöneticisi giriş adı ve parolaya sahiptir. Parola, yeni sunucunun **Genel Bakış** sayfasından değiştirilebilir.

Geri yükleme sırasında oluşturulan yeni sunucuda özgün sunucuda var olan VNet hizmet bitiş noktaları yoktur. Bu yeni sunucu için bu kuralların ayrı olarak ayarlanılması gerekir. Orijinal sunucudan gelen güvenlik duvarı kuralları geri yüklenir.

## <a name="geo-restore"></a>Coğrafi geri yükleme

Sunucunuzu coğrafi olarak gereksiz yedeklemeler için yapılandıysanız, varolan sunucunun yedeklemesinden yeni bir sunucu oluşturulabilir. Bu yeni sunucu, MariaDB için Azure Veritabanı'nın kullanılabilen herhangi bir bölgesinde oluşturulabilir.  

Coğrafi yedek liyedek için bir sunucu oluÅ `az mariadb server georestore` mak için Azure CLI komutunu kullanın.

> [!NOTE]
> Bir sunucu ilk oluşturulduğunda, coğrafi geri yükleme için hemen kullanılamayabilir. Gerekli meta verilerin doldurulması birkaç saat sürebilir.
>

Sunucuyu coğrafi olarak geri yüklemek için Azure CLI komut isteminde aşağıdaki komutu girin:

```azurecli-interactive
az mariadb server georestore --resource-group myresourcegroup --name mydemoserver-georestored --source-server mydemoserver --location eastus --sku-name GP_Gen5_8
```

Bu komut, Doğu ABD'de *mydemoserver-georestored* adlı yeni bir sunucu oluşturur ve *bu sunucu myresourcegroup'a*ait olacaktır. Bu genel amaçlı, 8 vCores ile Gen 5 sunucu. Sunucu *mydemoserver*coğrafi yedekli yedekleme oluşturulur , aynı zamanda kaynak grubu *myresourcegrubunda*

Yeni sunucuyu varolan sunucudan farklı bir kaynak grubunda oluşturmak `--source-server` istiyorsanız, parametrede sunucu adını aşağıdaki örnekte olduğu gibi nitelendirebilirsiniz:

```azurecli-interactive
az mariadb server georestore --resource-group newresourcegroup --name mydemoserver-georestored --source-server "/subscriptions/$<subscription ID>/resourceGroups/$<resource group ID>/providers/Microsoft.DBforMariaDB/servers/mydemoserver" --location eastus --sku-name GP_Gen5_8

```

Komut `az mariadb server georestore` aşağıdaki parametreleri gerektirir:

| Ayar | Önerilen değer | Açıklama  |
| --- | --- | --- |
|resource-group| myresourcegroup | Yeni sunucunun ait olacağı kaynak grubunun adı.|
|ad | mydemoserver-georestored | Yeni sunucunun adı. |
|source-server | mydemoserver | Coğrafi yedek li yedekleri kullanılan varolan sunucunun adı. |
|location | eastus | Yeni sunucunun konumu. |
|sku-name| GP_Gen5_8 | Bu parametre, yeni sunucunun fiyatlandırma katmanını, işlem oluşturmayı ve vCore sayısını ayarlar. Genel Amaçlı, 8 vCores ile Gen 5 sunucu için GP_Gen5_8 haritalar.|

Coğrafi yükleme ile yeni bir sunucu oluştururken, kaynak sunucuyla aynı depolama boyutu ve fiyatlandırma katmanını devralır. Bu değerler oluşturma sırasında değiştirilemez. Yeni sunucu oluşturulduktan sonra depolama boyutu büyütülebilir.

Geri yükleme işlemi bittikten sonra, yeni sunucuyu bulun ve verilerin beklendiği gibi geri yüklenmiş olduğundan doğrulayın. Yeni sunucu, geri yüklemenin başlatıldığı sırada varolan sunucu için geçerli olan sunucu yöneticisi giriş adı ve parolaya sahiptir. Parola, yeni sunucunun **Genel Bakış** sayfasından değiştirilebilir.

Geri yükleme sırasında oluşturulan yeni sunucuda özgün sunucuda var olan VNet hizmet bitiş noktaları yoktur. Bu yeni sunucu için bu kuralların ayrı olarak ayarlanılması gerekir. Orijinal sunucudan gelen güvenlik duvarı kuralları geri yüklenir.

## <a name="next-steps"></a>Sonraki adımlar

- Hizmetin [yedekleri](concepts-backup.md) hakkında daha fazla bilgi edinin
- [Yinelemeler](concepts-read-replicas.md) hakkında bilgi edinin
- [İş sürekliliği](concepts-business-continuity.md) seçenekleri hakkında daha fazla bilgi edinin
