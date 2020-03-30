---
title: Büyük dosya paylaşımlarını etkinleştirme ve oluşturma - Azure Dosyaları
description: Bu makalede, büyük dosya paylaşımlarını etkinleştirmeyi ve nasıl oluşturabileceğinizi öğreneceksiniz.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: c683e9847864de4e3409fb6dbd533497a5ae3cea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061005"
---
# <a name="enable-and-create-large-file-shares"></a>Büyük dosya paylaşımlarını etkinleştirme ve oluşturma

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdiğinizde, dosya paylaşımlarınız 100 TiB'ye kadar ölçeklenebilir. Varolan dosya paylaşımlarınız için varolan depolama hesaplarınızda bu ölçeklemesini etkinleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
- Azure CLI'yi kullanmak istiyorsanız, [en son sürümü yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Azure PowerShell'i kullanmak istiyorsanız, [en son sürümü yükleyin.](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0)

## <a name="restrictions"></a>Kısıtlamalar

Şimdilik, yalnızca yerel olarak yedekli depolama (LRS) veya bölge yedekli depolama (ZRS) büyük dosya paylaşımı etkin hesaplarda kullanabilirsiniz. Coğrafi bölge yedekli depolama (GZRS), coğrafi yedekli depolama (GRS) veya okuma erişimi coğrafi yedekli depolama (RA-GRS) kullanamazsınız.
Bir hesapta büyük dosya paylaşımlarını etkinleştirmek geri döndürülemez bir işlemdir. Etkinleştirdikten sonra hesabınızı GZRS, GRS veya RA-GRS'ye dönüştüremezsiniz.

## <a name="create-a-new-storage-account"></a>Yeni depolama hesabı oluşturma

### <a name="portal"></a>Portal

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure portalında **Tüm hizmetler'i**seçin. 
1. Kaynaklar **listesinde, Depolama Hesapları**girin. Siz yazarken, girişinize göre liste filtreler. **Depolama Hesapları**’nı seçin.
1. Görünen **Depolama Hesapları** penceresinde **Ekle'yi**seçin.
1. Depolama hesabı oluşturmak için kullanacağınız aboneliği seçin.
1. **Kaynak grubu** alanı altında **Yeni oluştur**’u seçin. Yeni kaynak grubunuzun adını girin.

    ![Portalda kaynak grubu oluşturmayı gösteren ekran görüntüsü](media/storage-files-how-to-create-large-file-share/create-large-file-share.png)

1. Ardından, depolama hesabınız için bir ad girin. Bu ad Azure'da benzersiz olmalıdır. Ad da uzunluğu 3 ila 24 karakter olmalıdır ve sadece sayılar ve küçük harfler olabilir.
1. Depolama hesabınız için bir konum seçin ve [büyük dosya paylaşımları için desteklenen çoğaltmadan biri](storage-files-planning.md#regional-availability)olduğundan emin olun.
1. Çoğaltmayı **Yerel olarak yedekli depolama** veya **Bölge yedekli depolama**alanına ayarlayın.
1. Bu alanları varsayılan değerlerinde bırakın:

   |Alan  |Değer  |
   |---------|---------|
   |Dağıtım modeli     |Resource Manager         |
   |Performans     |Standart         |
   |Hesap türü     |StorageV2 (genel amaçlı v2)         |
   |Erişim katmanı     |Sık Erişimli         |

1. **Gelişmiş'i**seçin ve ardından Büyük dosya **paylaşımlarının**sağındaki **Etkin** seçenek düğmesini seçin.
1. Depolama hesabı ayarlarınızı gözden geçirmek ve hesabı oluşturmak için **Gözden Geçir + Oluştur**’u seçin.

    ![Azure portalındaki yeni bir depolama hesabında "etkin" seçeneği düğmesiyle ekran görüntüsü](media/storage-files-how-to-create-large-file-share/large-file-shares-advanced-enable.png)

1. **Oluştur'u**seçin.

### <a name="cli"></a>CLI

İlk olarak, büyük dosya paylaşımlarını etkinleştirmek için [Azure CLI'nin en son sürümünü yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

Büyük dosya paylaşımları etkinleştirilmiş bir depolama hesabı oluşturmak için aşağıdaki komutu kullanın. `<yourStorageAccountName>`Değiştirin `<yourResourceGroup>`, `<yourDesiredRegion>` ve bilgilerinizle.

```azurecli-interactive
## This command creates a large file share–enabled account. It will not support GZRS, GRS, or RA-GRS.
az storage account create --name <yourStorageAccountName> -g <yourResourceGroup> -l <yourDesiredRegion> --sku Standard_LRS --kind StorageV2 --enable-large-file-share
```

### <a name="powershell"></a>PowerShell

İlk olarak, büyük dosya paylaşımlarını etkinleştirmek için [PowerShell'in en son sürümünü yükleyin.](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0)

Büyük dosya paylaşımları etkinleştirilmiş bir depolama hesabı oluşturmak için aşağıdaki komutu kullanın. `<yourStorageAccountName>`Değiştirin `<yourResourceGroup>`, `<yourDesiredRegion>` ve bilgilerinizle.

```powershell
## This command creates a large file share–enabled account. It will not support GZRS, GRS, or RA-GRS.
New-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -Location <yourDesiredRegion> -SkuName Standard_LRS -EnableLargeFileShare;
```

## <a name="enable-large-files-shares-on-an-existing-account"></a>Varolan bir hesapta büyük dosya paylaşımlarını etkinleştirme

Ayrıca, varolan hesaplarınızda büyük dosya paylaşımlarını da etkinleştirebilirsiniz. Büyük dosya paylaşımlarını etkinleştiriseniz, GZRS, GRS veya RA-GRS'ye dönüştüremezsinize izin vermezsiniz. Büyük dosya paylaşımlarını etkinleştirmek bu depolama hesabında geri alınamaz.

### <a name="portal"></a>Portal

1. Azure [portalını](https://portal.azure.com)açın ve büyük dosya paylaşımlarını etkinleştirmek istediğiniz depolama hesabına gidin.
1. Depolama hesabını açın ve **Yapılandırma'yı**seçin.
1. **Büyük dosya paylaşımlarında** **Etkin'i** seçin ve ardından **Kaydet'i**seçin.
1. **Genel Bakış'ı** seçin ve **Yenile'yi**seçin.

![Azure portalındaki varolan bir depolama hesabında Etkin seçeneği düğmesini seçme](media/storage-files-how-to-create-large-file-share/enable-large-file-shares-on-existing.png)

Artık depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdin. Ardından, artan kapasite ve ölçekavantajlarından yararlanmak için varolan paylaşımın kotasını güncelleştirmeniz gerekir.

"Büyük dosya paylaşımları henüz hesap için kullanılamıyor" hata iletisini alırsanız, bölgeniz kullanıma sunulmasının ortasında olabilir. Büyük dosya paylaşımları için acil bir ihtiyacınız varsa desteğe başvurun.

### <a name="cli"></a>CLI

Varolan hesabınızda büyük dosya paylaşımlarını etkinleştirmek için aşağıdaki komutu kullanın. `<yourStorageAccountName>` Değiştirin `<yourResourceGroup>` ve bilgilerinizle değiştirin.

```azurecli-interactive
az storage account update --name <yourStorageAccountName> -g <yourResourceGroup> --enable-large-file-share
```

### <a name="powershell"></a>PowerShell

Varolan hesabınızda büyük dosya paylaşımlarını etkinleştirmek için aşağıdaki komutu kullanın. `<yourStorageAccountName>` Değiştirin `<yourResourceGroup>` ve bilgilerinizle değiştirin.

```powershell
Set-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -EnableLargeFileShare
```

## <a name="create-a-large-file-share"></a>Büyük bir dosya paylaşımı oluşturma

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdikten sonra, bu hesapta daha yüksek kotalarla dosya paylaşımları oluşturabilirsiniz. 

### <a name="portal"></a>Portal

Büyük bir dosya paylaşımı oluşturmak, standart bir dosya paylaşımı oluşturmakla hemen hemen aynıdır. Temel fark, 100 TiB'e kadar bir kota ayarlayabiliyor olabilirsiniz.

1. Depolama hesabınızdan **Dosya paylaşımlarını**seçin.
1. **+ Dosya paylaşımı**'nı seçin.
1. Dosya paylaşımınız için bir ad girin. İstediğiniz kota boyutunu da 100 TiB'ye kadar ayarlayabilirsiniz. Ardından **Oluştur**’u seçin. 

![Ad ve Kota kutularını gösteren Azure portalı Kullanıcı Arabirimi](media/storage-files-how-to-create-large-file-share/large-file-shares-create-share.png)

### <a name="cli"></a>CLI

Büyük bir dosya paylaşımı oluşturmak için aşağıdaki komutu kullanın. `<yourStorageAccountName>`Değiştirin `<yourStorageAccountKey>`, `<yourFileShareName>` ve bilgilerinizle.

```azurecli-interactive
az storage share create --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName>
```

### <a name="powershell"></a>PowerShell

Büyük bir dosya paylaşımı oluşturmak için aşağıdaki komutu kullanın. `<YourStorageAccountName>`Değiştirin `<YourStorageAccountKey>`, `<YourStorageAccountFileShareName>` ve bilgilerinizle.

```powershell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
New-AzStorageShare -Name $shareName -Context $ctx
```

## <a name="expand-existing-file-shares"></a>Varolan dosya paylaşımlarını genişletme

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdikten sonra, bu hesaptaki varolan dosya paylaşımlarını daha yüksek kotaya da genişletebilirsiniz. 

### <a name="portal"></a>Portal

1. Depolama hesabınızdan **Dosya paylaşımlarını**seçin.
1. Dosya paylaşımınızı sağ tıklatın ve ardından **Kota'yı**seçin.
1. İstediğiniz yeni boyutu girin ve ardından **Tamam'ı**seçin.

![Varolan dosya paylaşımlarının Kotalı Azure portalı UI](media/storage-files-how-to-create-large-file-share/update-large-file-share-quota.png)

### <a name="cli"></a>CLI

Kotayı en yüksek boyuta ayarlamak için aşağıdaki komutu kullanın. `<yourStorageAccountName>`Değiştirin `<yourStorageAccountKey>`, `<yourFileShareName>` ve bilgilerinizle.

```azurecli-interactive
az storage share update --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName> --quota 102400
```

### <a name="powershell"></a>PowerShell

Kotayı en yüksek boyuta ayarlamak için aşağıdaki komutu kullanın. `<YourStorageAccountName>`Değiştirin `<YourStorageAccountKey>`, `<YourStorageAccountFileShareName>` ve bilgilerinizle.

```powershell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
# update quota
Set-AzStorageShareQuota -ShareName $shareName -Context $ctx -Quota 102400
```

## <a name="next-steps"></a>Sonraki adımlar

* [Windows'da dosya paylaşımını bağlama ve bağlama](storage-how-to-use-files-windows.md)
* [Linux'ta dosya paylaşımını bağlama ve bağlama](storage-how-to-use-files-linux.md)
* [macOS'ta dosya paylaşımını bağlama ve bağlama](storage-how-to-use-files-mac.md)
