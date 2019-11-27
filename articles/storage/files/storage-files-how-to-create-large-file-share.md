---
title: Büyük dosya paylaşımlarını etkinleştirme ve oluşturma-Azure dosyaları
description: Bu makalede, büyük dosya paylaşımlarını etkinleştirme ve oluşturma hakkında bilgi edineceksiniz.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 34b8af56a8f2f108b96ca07fa73f90bb9eb5bf13
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422736"
---
# <a name="how-to-enable-and-create-large-file-shares"></a>Büyük dosya paylaşımlarını etkinleştirme ve oluşturma

İlk olarak standart dosya paylaşımları yalnızca 5 TiB 'a kadar ölçeklendirilebilir, artık büyük dosya paylaşımları sayesinde 100 TiB 'ye kadar ölçeklenebilirler. Premium dosya paylaşımları, varsayılan olarak 100 TiB 'ye kadar ölçeklenir. 

Standart dosya paylaşımlarını kullanarak 100 TiB 'ye kadar ölçeklendirebilmek için depolama hesabınızı büyük dosya paylaşımlarını kullanacak şekilde etkinleştirmeniz gerekir. Büyük dosya paylaşımlarını kullanmak için mevcut bir hesabı etkinleştirebilir veya yeni bir hesap oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- Azure CLı 'yı kullanmayı planlıyorsanız, [en son sürümü](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)yüklediğinizden emin olun.
- Azure PowerShell kullanmayı düşünüyorsanız, [en son sürümü](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0)yüklediğinizden emin olun.

## <a name="restrictions"></a>Kısıtlamalar

Büyük dosya paylaşımları etkin hesaplar LRS veya ZRS 'yi destekler. Şimdilik, büyük dosya paylaşımları etkinleştirilmiş hesaplar GZRS, GRS veya RA-GRS 'yi desteklemez. Hesapta büyük dosya paylaşımlarının etkinleştirilmesi geri alınamaz bir işlemdir, etkin olduktan sonra hesabınız GZRS, GRS veya RA-GRS ' e dönüştürülemez.

## <a name="create-a-new-storage-account"></a>Yeni depolama hesabı oluşturma

### <a name="portal"></a>Portal

[Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portalda **Tüm hizmetler**’i seçin. Kaynak listesinde **Depolama Hesapları** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Depolama Hesapları**’nı seçin.
1. Açılan **Depolama Hesapları** penceresinde **Ekle**'yi seçin.
1. Depolama hesabının oluşturulacağı aboneliği seçin.
1. **Kaynak grubu** alanı altında **Yeni oluştur**’u seçin. Aşağıdaki görüntüde gösterildiği gibi yeni kaynak grubunuz için bir ad girin.

    ![Portalda kaynak grubu oluşturmayı gösteren ekran görüntüsü](media/storage-files-how-to-create-large-file-share/create-large-file-share.png)

1. Ardından, depolama hesabınız için bir ad girin. Seçtiğiniz ad Azure genelinde benzersiz olmalıdır. Ad ayrıca 3 - 24 karakter uzunluğunda olmalıdır ve yalnızca rakam ve küçük harf içerebilir.
1. Depolama hesabınız için bir konum seçin, [LFS için desteklenen bölgelerin biri](storage-files-planning.md#regional-availability)olduğundan emin olun.
1. Çoğaltmayı **yerel olarak yedekli depolama** ya da bölgesel olarak **yedekli depolama**olarak ayarlayın.
1. Bu alanları varsayılan değerlerine ayarlanmış olarak bırakın:

   |Alan  |Value  |
   |---------|---------|
   |Dağıtım modeli     |Resource Manager         |
   |Performans     |Standart         |
   |Hesap türü     |StorageV2 (genel amaçlı v2)         |
   |Erişim katmanı     |Sık Erişimli         |

1. **Gelişmiş** ' i seçin ve **büyük dosya paylaşımları**için **etkin** ' i seçin.
1. Depolama hesabı ayarlarınızı gözden geçirmek ve hesabı oluşturmak için **Gözden Geçir + Oluştur**’u seçin.

    ![Large-File-Shares-Advanced-Enable. png](media/storage-files-how-to-create-large-file-share/large-file-shares-advanced-enable.png)

1. **Oluştur**'u seçin.


### <a name="cli"></a>CLI

İlk olarak, bu özelliği etkinleştirebilmeniz [için en son sürümü](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)yüklediğinizden emin olun.

Büyük dosya paylaşımları etkinleştirilmiş bir depolama hesabı oluşturmak için `<yourStorageAccountName>`, `<yourResourceGroup>`ve `<yourDesiredRegion>` değerlerini değerlerinizle değiştirin, ardından aşağıdaki komutu kullanın:

```azurecli-interactive
## This command creates a large file share enabled account, it will not support GZRS, GRS, or RA-GRS
az storage account create –name <yourStorageAccountName> -g <yourResourceGroup> -l <yourDesiredRegion> –sku Standard_LRS --kind StorageV2 –enable-large-file-share
```

### <a name="powershell"></a>PowerShell

İlk olarak, bu özelliği etkinleştirebilmeniz [için en son sürümü](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0)yüklediğinizden emin olun.

Büyük dosya paylaşımları etkinleştirilmiş bir depolama hesabı oluşturmak için `<yourStorageAccountName>`, `<yourResourceGroup>`ve `<yourDesiredRegion>` değerlerini değerlerinizle değiştirin, ardından aşağıdaki komutu kullanın:

```PowerShell
## This command creates a large file share enabled account, it will not support GZRS, GRS, or RA-GRS
New-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -Location <yourDesiredRegion> -SkuName Standard_LRS -EnableLargeFileShare;
```

## <a name="enable-on-existing-account"></a>Mevcut hesapta etkinleştir

### <a name="portal"></a>Portal

Ayrıca, var olan hesaplarda büyük dosya paylaşımlarını etkinleştirebilirsiniz. Bunu yaparsanız, hesap artık GZRS, GRS veya RA-GRS ' e dönüştürülemez. Bu seçenek bu hesapta geri alınamaz.

1. [Azure Portal](https://portal.azure.com) açın ve büyük dosya paylaşımlarını etkinleştirmek istediğiniz depolama hesabına gidin.
1. Depolama hesabını açın ve **yapılandırma**' yı seçin.
1. **Büyük dosya paylaşımlarında** **etkin** ' i seçin ve ardından Kaydet ' i seçin.
1. **Genel bakış** ' ı ve **Yenile**' yi seçin.

![Enable-large-File-Shares-on-Existing. png](media/storage-files-how-to-create-large-file-share/enable-large-file-shares-on-existing.png)

Artık depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdiniz.

Şu hatayı alırsanız: "hesabın büyük dosya paylaşımları henüz kullanılamıyor." Bölgeniz büyük olasılıkla tamamlanarak veya acil gereksinimleriniz varsa, destek almak için bir süre beklemeniz gerekir, ancak bu durumda bir süre bekleyebilirsiniz.

### <a name="cli"></a>CLI

Mevcut hesaplarınızda büyük dosya paylaşımlarını etkinleştirebilirsiniz. Bunu yaparsanız, hesap artık GZRS, GRS veya RA-GRS ' e dönüştürülemez. Bu seçenek bu hesapta geri alınamaz.

Aşağıdaki komutta `<yourStorageAccountName>` ve `<yourResourceGroup>` değiştirin, ardından mevcut hesabınızda büyük dosya paylaşımlarını etkinleştirmek için kullanın:

```azurecli-interactive
az storage account update –name <yourStorageAccountName> -g <yourResourceGroup> –enable-large-file-share
```

### <a name="powershell"></a>PowerShell

Mevcut hesaplarınızda büyük dosya paylaşımlarını etkinleştirebilirsiniz. Bunu yaparsanız, hesap artık GZRS, GRS veya RA-GRS ' e dönüştürülemez. Bu seçenek bu hesapta geri alınamaz.

Aşağıdaki komutta `<yourStorageAccountName>` ve `<yourResourceGroup>` değiştirin, ardından mevcut hesabınızda büyük dosya paylaşımlarını etkinleştirmek için kullanın:

```PowerShell
Set-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -EnableLargeFileShare
```

## <a name="create-a-large-file-share"></a>Büyük bir dosya paylaşma oluşturma

### <a name="portal"></a>Portal

Büyük bir dosya paylaşımının oluşturulması, standart bir dosya paylaşımının oluşturulmasıyla neredeyse aynıdır. Temel fark, 100 TiB 'ye kadar bir kota ayarlayabilmeniz gerektiğidir.

1. Depolama hesabınızdan **dosya paylaşımları**' nı seçin.
1. **+ Dosya paylaşma**' yı seçin.
1. Dosya paylaşımınız için bir ad girin ve (isteğe bağlı olarak) istediğiniz kota boyutunu 100 TiB 'ye kadar, ardından **Oluştur**' u seçin. 

![Large-File-Shares-Create-Share. png](media/storage-files-how-to-create-large-file-share/large-file-shares-create-share.png)

### <a name="cli"></a>CLI

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdikten sonra, bu hesapta daha yüksek kotalarla dosya paylaşımları oluşturabilirsiniz. Aşağıdaki komutta `<yourStorageAccountName>`, `<yourStorageAccountKey>`ve `<yourFileShareName>` değerlerini değerlerinizle değiştirin ve ardından büyük bir dosya paylaşma oluşturmak için kullanabilirsiniz:

```azurecli-interactive
az storage share create --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName>
```

### <a name="powershell"></a>PowerShell

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdikten sonra, bu hesapta daha yüksek kotalarla dosya paylaşımları oluşturabilirsiniz. Aşağıdaki komutta `<YourStorageAccountName>`, `<YourStorageAccountKey>`ve `<YourStorageAccountFileShareName>` değerlerini değerlerinizle değiştirin ve ardından büyük bir dosya paylaşma oluşturmak için kullanabilirsiniz:

```PowerShell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
New-AzStorageShare -Name $shareName -Context $ctx
```

## <a name="expand-existing-file-shares"></a>Mevcut dosya paylaşımlarını Genişlet

### <a name="portal"></a>Portal

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdikten sonra, mevcut paylaşımları daha yüksek kotaya genişletebilirsiniz.

1. Depolama hesabınızdan **dosya paylaşımları**' nı seçin.
1. Dosya paylaşımınıza sağ tıklayıp **Kota**' i seçin.
1. İstediğiniz yeni boyutu girip **Tamam**' ı seçin.

![Update-Large-File-Share-Quota. png](media/storage-files-how-to-create-large-file-share/update-large-file-share-quota.png)

### <a name="cli"></a>CLI

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdikten sonra, bu hesaptaki mevcut dosya paylaşımlarını daha yüksek kotalara genişletebilirsiniz. Aşağıdaki komutta `<yourStorageAccountName>`, `<yourStorageAccountKey>`ve `<yourFileShareName>` değerlerini değerlerinizle değiştirin, daha sonra kotayı en büyük boyuta ayarlamak için kullanabilirsiniz:

```azurecli-interactive
az storage share update --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName> --quota 102400
```

### <a name="powershell"></a>PowerShell

Depolama hesabınızda büyük dosya paylaşımlarını etkinleştirdikten sonra, bu hesaptaki mevcut dosya paylaşımlarını daha yüksek kotalara genişletebilirsiniz. Aşağıdaki komutta `<YourStorageAccountName>`, `<YourStorageAccountKey>`ve `<YourStorageAccountFileShareName>` değerlerini değerlerinizle değiştirin, daha sonra kotayı en büyük boyuta ayarlamak için kullanabilirsiniz:

```PowerShell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
# update quota
Set-AzStorageShareQuota -ShareName $shareName -Context $ctx -Quota 102400
```

## <a name="next-steps"></a>Sonraki adımlar

* [Dosya Paylaşımını Bağlama - Windows](storage-how-to-use-files-windows.md)
* [Dosya Paylaşımını Bağlama - Linux](../storage-how-to-use-files-linux.md)
* [Dosya Paylaşımını Bağlama - macOS](storage-how-to-use-files-mac.md)