---
title: 'Quickstart: Sunucu oluşturma - az postgres up - PostgreSQL için Azure Veritabanı - Tek Sunucu'
description: PostgreSQL için Azure Veritabanı - Tek Sunucu için Azure CLI (komut satırı arabirimi) komutunu kullanarak Azure Veritabanı oluşturmak için hızlı başlangıç kılavuzu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 05/06/2019
ms.openlocfilehash: fe15c02286223ec0829b31664811b7f589cf16aa
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74774841"
---
# <a name="quickstart-use-an-azure-cli-command-az-postgres-up-preview-to-create-an-azure-database-for-postgresql---single-server"></a>Hızlı başlatma: PostgreSQL için bir Azure Veritabanı oluşturmak için bir Azure CLI komutu kullanın, az postgres up (önizleme),

> [!IMPORTANT]
> Azure CLI [komutuna göre az postgres](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) önizlemede.

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç, Azure CLI'yi kullanarak PostgreSQL sunucusu için bir Azure Veritabanı oluşturmak için [az postgres up](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) komutunu nasıl kullanacağınızı gösterir. `az postgres up` Komut, sunucuoluşturmaya ek olarak, veritabanında kök kullanıcı olan örnek bir veritabanı oluşturur, Azure hizmetleri için güvenlik duvarını açar ve istemci bilgisayar için varsayılan güvenlik duvarı kuralları oluşturur. Bu varsayılanlar geliştirme işlemini hızlandırmaya yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu makalede, Azure CLI sürüm 2.0 veya daha sonra yerel olarak çalıştırdığınızı gerektirir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

Az giriş komutunu kullanarak hesabınızda oturum [açmanız](/cli/azure/authenticate-azure-cli?view=interactive-log-in) gerekir. İlgili abonelik adı için komut çıktısından **kimlik** özelliğine dikkat edin.

```azurecli
az login
```

Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. [az account set](/cli/azure/account) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin. Az **giriş** çıkışından **abonelik kimliği** özelliğini abonelik kimliği yer tutucuya abone liğinize değiştirin.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

Komutları kullanmak için [db-up](/cli/azure/ext/db-up) uzantısını yükleyin. Bir hata döndürülürse, Azure CLI'nin en son sürümünü yüklediğinizden emin olun. Bkz. [Azure CLI'yi Yükle](/cli/azure/install-azure-cli).

```azurecli
az extension add --name db-up
```

Aşağıdaki komutu kullanarak PostgreSQL sunucusu için bir Azure Veritabanı oluşturun:

```azurecli
az postgres up
```

Sunucu aşağıdaki varsayılan değerlerle oluşturulur (bunları el ile geçersiz kılmadığınız sürece):

**Ayar** | **Varsayılan değer** | **Açıklama**
---|---|---
server-name | Oluşturulan sistem | PostgreSQL için Azure Veritabanı sunucunuzu tanıtan benzersiz bir ad.
resource-group | Oluşturulan sistem | Yeni bir Azure kaynak grubu.
sku-name | GP_Gen5_2 | Sku'nun adı. Kısaca {fiyatlandırma katmanı}\_{işlem nesli}\_{sanal çekirdek sayısı} kuralına uyar. Varsayılan 2 vCores ile Genel Amaçlı Gen5 sunucusudur. Katmanlar hakkında daha fazla bilgi için [fiyatlandırma sayfamıza](https://azure.microsoft.com/pricing/details/postgresql/) bakın.
backup-retention | 7 | Yedekleme nin ne kadar süreyle tutulduğu. Birim olarak gün kullanılır.
geo-redundant-backup | Devre dışı | Coğrafi olarak yedekli yedeklemelerin bu sunucu için etkinleştirilip etkinleştirilmeyeceği.
location | westus2 | Sunucu için Azure konumu.
ssl-enforcement | Devre dışı | Bu sunucu için ssl'in etkinleştirilip etkinleştirilmeyeceği.
storage-size | 5120 | Sunucunun depolama kapasitesi (birim olan megabayt kullanılır).
version | 10 | PostgreSQL ana sürümü.
admin-user | Oluşturulan sistem | Yöneticinin kullanıcı adı.
admin-password | Oluşturulan sistem | Yönetici kullanıcının parolası.

> [!NOTE]
> Komut ve ek parametreleri hakkında daha fazla bilgi için [Azure CLI belgelerine](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up)bakın. `az postgres up`

Sunucunuz oluşturulduktan sonra aşağıdaki ayarlarla birlikte gelir:

- "Devbox" adlı bir güvenlik duvarı kuralı oluşturulur. Azure CLI, `az postgres up` komutun çalıştırdığı makinenin IP adresini algılamaya çalışır ve IP adresinin beyaz listelerini listeler.
- "Azure hizmetlerine erişime izin ver" aveya aveya açık. Bu ayar, sunucunun güvenlik duvarını aboneliğinizde olmayan kaynaklar da dahil olmak üzere tüm Azure kaynaklarından gelen bağlantıları kabul etmek üzere yapılandırır.
- "sampledb" adlı boş bir veritabanı oluşturulur
- "Sampledb" ayrıcalıkları ile "kök" adlı yeni bir kullanıcı oluşturulur

> [!NOTE]
> PostgreSQL için Azure Veritabanı, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanıyorsanız ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Sunucunuza bağlanmak için BT departmanınızın 5432 bağlantı noktasını açmasını sağlar.

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

`az postgres up` Komut tamamlandıktan sonra, popüler programlama dilleri için bağlantı dizeleri listesi size döndürülür. Bu bağlantı dizeleri, PostgreSQL sunucusu için yeni oluşturduğunuz Azure Veritabanı'nın özel özellikleriyle önceden yapılandırılmıştır.

Bu bağlantı dizelerini yeniden listelemek için [az postgres show-connection-string](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-show-connection-string) komutunu kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aşağıdaki komutu kullanarak hızlı başlat'ta oluşturduğunuz tüm kaynakları temizleyin. Bu komut, PostgreSQL sunucusu ve kaynak grubu için Azure Veritabanını siler.

```azurecli
az postgres down --delete-group
```

Eğer sadece yeni oluşturulan sunucu silmek istiyorsanız, [az postgres aşağı](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-down) komutu çalıştırabilirsiniz.

```azurecli
az postgres down
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)
