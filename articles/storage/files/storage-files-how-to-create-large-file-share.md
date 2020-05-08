---
title: Büyük dosya paylaşımlarını etkinleştirme ve oluşturma-Azure dosyaları
description: Bu makalede, büyük dosya paylaşımlarını etkinleştirme ve oluşturma hakkında bilgi edineceksiniz.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: add2805d9a360d3d9cd45ab54f476a6852fb7bd5
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858573"
---
# <a name="enable-and-create-large-file-shares"></a>Büyük dosya paylaşımlarını etkinleştirme ve oluşturma

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdiğinizde, dosya paylaşımlarınız 100 TiB 'ye kadar ölçeklenebilir. Mevcut dosya paylaşımlarınız için bu ölçeklendirmeyi mevcut depolama hesaplarınızda etkinleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- Azure CLı 'yı kullanmayı planlıyorsanız [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
- Azure PowerShell kullanmayı düşünüyorsanız, [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0).

## <a name="restrictions"></a>Kısıtlamalar

Şimdilik, yalnızca yerel olarak yedekli depolama (LRS) veya bölgesel olarak yedekli depolama (ZRS), büyük dosya paylaşımında etkinleştirilmiş hesaplarda kullanabilirsiniz. Coğrafi bölge yedekli depolama (GZRS), coğrafi olarak yedekli depolama (GRS), Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) veya Okuma Erişimli Coğrafi bölge-yedekli depolama (RA-GZRS) kullanamazsınız.

Hesapta büyük dosya paylaşımlarının etkinleştirilmesi geri alınamaz bir işlemdir. Bu uygulamayı etkinleştirdikten sonra, hesabınızı GZRS, GRS, RA-GRS veya RA-GZRS ' e dönüştüremeyeceksiniz.

## <a name="create-a-new-storage-account"></a>Yeni depolama hesabı oluşturma

### <a name="portal"></a>Portal

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure portal, **tüm hizmetler**' i seçin. 
1. Kaynak listesinde **depolama hesapları**' nı girin. Siz yazarken, liste, girişinizi temel alarak filtreler. **Depolama Hesapları**’nı seçin.
1. Görüntülenen **depolama hesapları** penceresinde **Ekle**' yi seçin.
1. Depolama hesabını oluşturmak için kullanacağınız aboneliği seçin.
1. **Kaynak grubu** alanı altında **Yeni oluştur**’u seçin. Yeni kaynak grubunuz için bir ad girin.

    ![Portalda kaynak grubu oluşturmayı gösteren ekran görüntüsü](media/storage-files-how-to-create-large-file-share/create-large-file-share.png)

1. Ardından, depolama hesabınız için bir ad girin. AD Azure genelinde benzersiz olmalıdır. Ad ayrıca 3 ile 24 karakter uzunluğunda olmalı ve yalnızca rakamlardan ve küçük harflerden oluşabilir.
1. Depolama hesabınız için bir konum seçin.
1. Çoğaltmayı **yerel olarak yedekli depolama** ya da bölgesel olarak **yedekli depolama**olarak ayarlayın.
1. Bu alanları varsayılan değerlerinde bırakın:

   |Alan  |Değer  |
   |---------|---------|
   |Dağıtım modeli     |Resource Manager         |
   |Performans     |Standart         |
   |Hesap türü     |StorageV2 (genel amaçlı v2)         |
   |Erişim katmanı     |Sık Erişimli         |

1. **Gelişmiş**' i seçin ve ardından **büyük dosya paylaşımlarının**sağ tarafındaki **etkin** seçenek düğmesini seçin.
1. Depolama hesabı ayarlarınızı gözden geçirmek ve hesabı oluşturmak için **Gözden Geçir + Oluştur**’u seçin.

    ![Azure portal yeni bir depolama hesabında "etkin" seçenek düğmesi ile ekran görüntüsü](media/storage-files-how-to-create-large-file-share/large-file-shares-advanced-enable.png)

1. **Oluştur**’u seçin.

### <a name="cli"></a>CLI

İlk olarak, büyük dosya paylaşımlarını etkinleştirebilmeniz için [Azure CLI 'nin en son sürümünü yüklemeniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) gerekir.

Büyük dosya paylaşımları etkin olan bir depolama hesabı oluşturmak için aşağıdaki komutu kullanın. , `<yourStorageAccountName>` `<yourResourceGroup>`Ve `<yourDesiredRegion>` bilgilerinizi ile değiştirin.

```azurecli-interactive
## This command creates a large file share–enabled account. It will not support GZRS, GRS, RA-GRS, or RA-GZRS.
az storage account create --name <yourStorageAccountName> -g <yourResourceGroup> -l <yourDesiredRegion> --sku Standard_LRS --kind StorageV2 --enable-large-file-share
```

### <a name="powershell"></a>PowerShell

İlk olarak, büyük dosya paylaşımlarını etkinleştirebilmeniz için [en son PowerShell sürümünü yüklemeniz](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0) gerekir.

Büyük dosya paylaşımları etkin olan bir depolama hesabı oluşturmak için aşağıdaki komutu kullanın. , `<yourStorageAccountName>` `<yourResourceGroup>`Ve `<yourDesiredRegion>` bilgilerinizi ile değiştirin.

```powershell
## This command creates a large file share–enabled account. It will not support GZRS, GRS, RA-GRS, or RA-GZRS.
New-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -Location <yourDesiredRegion> -SkuName Standard_LRS -EnableLargeFileShare;
```

## <a name="enable-large-files-shares-on-an-existing-account"></a>Mevcut bir hesapta büyük dosya paylaşımlarını etkinleştir

Ayrıca, mevcut hesaplarınızda büyük dosya paylaşımlarını etkinleştirebilirsiniz. Büyük dosya paylaşımlarını etkinleştirirseniz, GZRS, GRS, RA-GRS veya RA-GZRS 'e dönüştüremezsiniz. Bu depolama hesabında büyük dosya paylaşımlarının etkinleştirilmesi geri alınamaz.

### <a name="portal"></a>Portal

1. [Azure Portal](https://portal.azure.com)açın ve büyük dosya paylaşımlarını etkinleştirmek istediğiniz depolama hesabına gidin.
1. Depolama hesabını açın ve **yapılandırma**' yı seçin.
1. **Büyük dosya paylaşımlarında** **etkin** ' i seçin ve ardından **Kaydet**' i seçin.
1. **Genel bakış** ' ı ve **Yenile**' yi seçin.

![Azure portal var olan bir depolama hesabında etkin seçenek düğmesini seçme](media/storage-files-how-to-create-large-file-share/enable-large-file-shares-on-existing.png)

Artık depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdiniz. Daha sonra, daha fazla kapasitenin ve ölçeğin avantajlarından yararlanmak için mevcut paylaşımın kotasını güncelleştirmeniz gerekir.

"Hesap için büyük dosya paylaşımları henüz kullanılamıyor" hata iletisini alırsanız, bölgeniz dağıtımı tamamlama ortasında olabilir. Büyük dosya paylaşımları için acil bir gereksinimimiz varsa desteğe başvurun.

### <a name="cli"></a>CLI

Mevcut hesabınızda büyük dosya paylaşımlarını etkinleştirmek için aşağıdaki komutu kullanın. Ve `<yourStorageAccountName>` `<yourResourceGroup>` bilgilerinizi ile değiştirin.

```azurecli-interactive
az storage account update --name <yourStorageAccountName> -g <yourResourceGroup> --enable-large-file-share
```

### <a name="powershell"></a>PowerShell

Mevcut hesabınızda büyük dosya paylaşımlarını etkinleştirmek için aşağıdaki komutu kullanın. Ve `<yourStorageAccountName>` `<yourResourceGroup>` bilgilerinizi ile değiştirin.

```powershell
Set-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -EnableLargeFileShare
```

## <a name="create-a-large-file-share"></a>Büyük bir dosya paylaşma oluşturma

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdikten sonra, bu hesapta daha yüksek kotalarla dosya paylaşımları oluşturabilirsiniz. 

### <a name="portal"></a>Portal

Büyük bir dosya paylaşımının oluşturulması, standart bir dosya paylaşımının oluşturulmasıyla neredeyse aynıdır. Temel fark, 100 TiB 'ye kadar bir kota ayarlayabilmeniz gerektiğidir.

1. Depolama hesabınızdan **dosya paylaşımları**' nı seçin.
1. **+ Dosya paylaşımı**'nı seçin.
1. Dosya paylaşımınız için bir ad girin. İstediğiniz kota boyutunu 100 TiB 'ye kadar de ayarlayabilirsiniz. Ardından **Oluştur**’u seçin. 

![Ad ve kota kutularını gösteren Azure portal Kullanıcı arabirimi](media/storage-files-how-to-create-large-file-share/large-file-shares-create-share.png)

### <a name="cli"></a>CLI

Büyük bir dosya paylaşma oluşturmak için aşağıdaki komutu kullanın. , `<yourStorageAccountName>` `<yourStorageAccountKey>`Ve `<yourFileShareName>` bilgilerinizi ile değiştirin.

```azurecli-interactive
az storage share create --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName>
```

### <a name="powershell"></a>PowerShell

Büyük bir dosya paylaşma oluşturmak için aşağıdaki komutu kullanın. , `<YourStorageAccountName>` `<YourStorageAccountKey>`Ve `<YourStorageAccountFileShareName>` bilgilerinizi ile değiştirin.

```powershell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
New-AzStorageShare -Name $shareName -Context $ctx
```

## <a name="expand-existing-file-shares"></a>Mevcut dosya paylaşımlarını Genişlet

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdikten sonra, söz konusu hesaptaki mevcut dosya paylaşımlarını daha yüksek kota olarak da genişletebilirsiniz. 

### <a name="portal"></a>Portal

1. Depolama hesabınızdan **dosya paylaşımları**' nı seçin.
1. Dosya paylaşımınıza sağ tıklayıp **Kota**' i seçin.
1. İstediğiniz yeni boyutu girip **Tamam**' ı seçin.

![Mevcut dosya paylaşımlarının kotasına sahip Azure portal Kullanıcı arabirimi](media/storage-files-how-to-create-large-file-share/update-large-file-share-quota.png)

### <a name="cli"></a>CLI

Kotayı en büyük boyuta ayarlamak için aşağıdaki komutu kullanın. , `<yourStorageAccountName>` `<yourStorageAccountKey>`Ve `<yourFileShareName>` bilgilerinizi ile değiştirin.

```azurecli-interactive
az storage share update --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName> --quota 102400
```

### <a name="powershell"></a>PowerShell

Kotayı en büyük boyuta ayarlamak için aşağıdaki komutu kullanın. , `<YourStorageAccountName>` `<YourStorageAccountKey>`Ve `<YourStorageAccountFileShareName>` bilgilerinizi ile değiştirin.

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

* [Windows üzerinde bir dosya paylaşımının bağlantısını oluşturma ve bağlama](storage-how-to-use-files-windows.md)
* [Linux 'ta bir dosya paylaşımının bağlantısını oluşturma ve bağlama](storage-how-to-use-files-linux.md)
* [MacOS 'ta bir dosya paylaşımının bağlantısını oluşturma ve bağlama](storage-how-to-use-files-mac.md)
