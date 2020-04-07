---
title: Örnek PowerShell komut dosyaları
description: PowerShell komut dosyaları aracılığıyla ön uç nasıl kullanılacağını gösteren örnekler
author: florianborn71
ms.author: flborn
ms.date: 02/12/2020
ms.topic: sample
ms.openlocfilehash: d7238c899301d76edde86d52c6db74468ffe18ad
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679481"
---
# <a name="example-powershell-scripts"></a>Örnek PowerShell komut dosyaları

Azure Uzaktan İşleme aşağıdaki iki REST API'si sağlar:

- [Dönüşüm REST API](../how-tos/conversion/conversion-rest-api.md)
- [Oturum REST API](../how-tos/session-rest-api.md)

[ARR örnekleri deposu,](https://github.com/Azure/azure-remote-rendering) hizmetin REST API'leri ile etkileşim kurmak için *Komut Dosyaları* klasöründe örnek komut dosyaları içerir. Bu makalede, kullanımları açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Örnek komut dosyalarını yürütmek için Azure [PowerShell'in](https://docs.microsoft.com/powershell/azure/)işlevsel bir kurulumuna ihtiyacınız vardır.

1. Azure PowerShell'i yükleme:
    1. Yönetici haklarıyla powershell açın
    1. Çalıştırmak:`Install-Module -Name Az -AllowClobber`

1. Komut dosyalarını çalıştırma yla ilgili hatalar alırsanız, [yürütme ilkenizin](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) uygun şekilde ayarlandığından emin olun:
    1. Yönetici haklarıyla powershell açın
    1. Çalıştırmak:`Set-ExecutionPolicy -ExecutionPolicy Unrestricted`

1. [Azure Depolama hesabı hazırlama](../how-tos/conversion/blob-storage.md#prepare-azure-storage-accounts)

1. Azure Uzaktan İşlem hesabınızı içeren aboneliğe giriş yapın:
    1. PowerShell aç
    1. Çalıştırmak:`Connect-AzAccount -Subscription "<your Azure subscription id>"`

1. [Azure Uzaktan İşleme GithHub deposundan](https://github.com/Azure/azure-remote-rendering) *Scriptler* klasörünü indirin.

## <a name="configuration-file"></a>Yapılandırma dosyası

`.ps1` Dosyaların yanında doldurmanız gereken `arrconfig.json` bir şey vardır:

```json
{
    "accountSettings": {
        "arrAccountId": "<fill in the account ID from the Azure Portal>",
        "arrAccountKey": "<fill in the account key from the Azure Portal>",
        "region": "<select from available regions>"
    },
    "renderingSessionSettings": {
        "vmSize": "<select standard or premium>",
        "maxLeaseTime": "<hh:mm:ss>"
    },
  "assetConversionSettings": {
    "resourceGroup": "<resource group which contains the storage account you created, only needed when uploading or generating SAS>",
    "storageAccountName": "<name of the storage account you created>",
    "blobInputContainerName": "<input container inside the storage container>",
    "blobOutputContainerName": "<output container inside the storage container>",
    "localAssetDirectoryPath": "<fill in a path to a local directory containing your asset (and files referenced from it like textures)>",
    "inputFolderPath": "<optional: base folderpath in the input container for asset upload. uses / as dir separator>",
    "inputAssetPath": "<the path to the asset under inputcontainer/inputfolderpath pointing to the input asset e.g. box.fbx>",
    "outputFolderPath": "<optional: base folderpath in the output container - the converted asset and log files will be placed here>",
    "outputAssetFileName": "<optional: filename for the converted asset, this will be placed in the output container under the outputpath>"
  }
}
```

> [!CAUTION]
> LocalAssetDirectoryPath yolunda çift eğik çizgi kullanarak ters eğik çizgilerden\\\\düzgün bir şekilde kaçtığa emin olun: " ve inputFolderPath ve inputAssetPath gibi diğer tüm yollarda "/" ileri eğik çizgilerini kullanın.

### <a name="accountsettings"></a>hesapAyarlar

Bunun `arrAccountId` `arrAccountKey`için ve , [bkz.](../how-tos/create-an-account.md)
Kullanılabilir `region` [bölgelerin listesine](../reference/regions.md)bakın.

### <a name="renderingsessionsettings"></a>renderSessionSettings

**RenderingSession.ps1**çalıştırmak istiyorsanız bu yapı nın doldurulması gerekir.

- **vmSize:** Sanal makinenin boyutunu seçer. *Standart* veya *premium'u*seçin. Artık ihtiyacınız olmadığında işleme oturumlarını kapatın.
- **maxLeaseTime:** VM'yi kiralamak istediğiniz süre. Kira süresi dolduğunda kapatılacak. Kiralama süresi daha sonra uzatılabilir (aşağıya bakın).

### <a name="assetconversionsettings"></a>varlıkDönüşüm Ayarları

**Conversion.ps1'i**çalıştırmak istiyorsanız bu yapının doldurulması gerekir.

Ayrıntılar için [bkz.](../how-tos/conversion/blob-storage.md#prepare-azure-storage-accounts)

## <a name="script-renderingsessionps1"></a>Komut Dosyası: RenderingSession.ps1

Bu komut dosyası, oturumları oluşturmak, sorgulamak ve durdurmak için kullanılır.

> [!IMPORTANT]
> Arrconfig.json'daki *hesap Ayarları* ve *renderSessionSettings* bölümlerini doldurduğundan emin olun.

### <a name="create-a-rendering-session"></a>Oluşturma oturumu oluşturma

Tam dolu arrconfig.json ile normal kullanım:

```PowerShell
.\RenderingSession.ps1
```

Komut dosyası, belirtilen ayarlarla bir render VM'i döndürmek için [oturum yönetimi REST API'yi](../how-tos/session-rest-api.md) çağırır. Başarı, *bu sessionId*alacak. Daha sonra oturum hazır olana veya bir hata oluşana kadar oturum özelliklerini yoklar.

Alternatif bir **config** dosyası kullanmak için:

```PowerShell
.\RenderingSession.ps1 -ConfigFile D:\arr\myotherconfigFile.json
```

Config dosyasından **tek tek ayarları geçersiz kılabilirsiniz:**

```PowerShell
.\RenderingSession.ps1 -Region <region> -VmSize <vmsize> -MaxLeaseTime <hh:mm:ss>
```

Bir **oturuma yalnızca yoklama olmadan başlamak**için şunları kullanabilirsiniz:

```PowerShell
.\RenderingSession.ps1 -CreateSession
```

Komut dosyasının geri alındığı *sessionId* diğer oturum komutlarının çoğuna geçirilmelidir.

### <a name="retrieve-session-properties"></a>Oturum özelliklerini alma

Bir oturumun özelliklerini almak için çalıştırın:

```PowerShell
.\RenderingSession.ps1 -GetSessionProperties -Id <sessionID> [-Poll]
```

Oturumun `-Poll` *hazır* olmasını veya bir hata oluştuğunu beklemek için kullanın.

### <a name="list-active-sessions"></a>Etkin oturumları listele

```PowerShell
.\RenderingSession.ps1 -GetSessions
```

### <a name="stop-a-session"></a>Oturumu durdurma

```PowerShell
.\RenderingSession.ps1 -StopSession -Id <sessionID>
```

### <a name="change-session-properties"></a>Oturum özelliklerini değiştirme

Şu anda, yalnızca bir oturumun maxLeaseTime'ını değiştirmeyi destekliyoruz.

> [!NOTE]
> Kiralama süresi her zaman VM oturumunun ilk oluşturulduğu andan itibaren sayılır. Yani oturum kira süresini bir saat daha uzatmak için *maxLeaseTime'ı* bir saat artırın.

```PowerShell
.\RenderingSession.ps1 -UpdateSession -Id <sessionID> -MaxLeaseTime <hh:mm:ss>
```

## <a name="script-conversionps1"></a>Komut Dosyası: Conversion.ps1

Bu komut dosyası, giriş modellerini Azure Uzaktan İşleme belirli çalışma zamanı biçimine dönüştürmek için kullanılır.

> [!IMPORTANT]
> Arrconfig.json'daki *hesap Ayarları* ve *varlıkDönüşüm Ayarları* bölümlerini doldurduğundan emin olun.

Komut dosyası, hizmetle birlikte depolama hesaplarını kullanmak için iki seçeneği gösterir:

- Azure Uzaktan İşlem hesabıyla bağlantılı depolama hesabı
- Paylaşılan Erişim İmzaları (SAS) aracılığıyla depolamaya erişim sağlama

### <a name="linked-storage-account"></a>Bağlantılı depolama hesabı

Arrconfig.json'u tamamen doldurduktan ve bir depolama hesabını bağladıktan sonra aşağıdaki komutu kullanabilirsiniz. Depolama hesabınızı bağlamak [Hesap Oluştur'da](../how-tos/create-an-account.md#link-storage-accounts)açıklanmıştır.

Paylaşılan Erişim İmzaları oluşturmaya gerek olmadığından, bağlantılı bir depolama hesabı kullanmak dönüşüm hizmetini kullanmak için tercih edilen yoldur.

```PowerShell
.\Conversion.ps1
```

1. Verilen altındaki giriş `assetConversionSettings.modelLocation` blob konteynerine tüm dosyaları yükleyin`inputFolderPath`
1. [Model dönüşümbaşlatmak](../how-tos/conversion/model-conversion.md) için [model dönüştürme REST API'yi](../how-tos/conversion/conversion-rest-api.md) arayın
1. Dönüşüm başarılı olana veya başarısız olana kadar dönüşüm durumunu yoklama
1. Dönüştürülen dosya konumunun çıktı ayrıntıları (depolama hesabı, çıkış kapsayıcısı, kapsayıcıdaki dosya yolu)

### <a name="access-to-storage-via-shared-access-signatures"></a>Paylaşılan Erişim İmzaları ile depolama alanına erişim

```PowerShell
.\Conversion.ps1 -UseContainerSas
```

Bu olacaktır:

1. Yerel dosyayı giriş `assetConversionSettings.localAssetDirectoryPath` blob kapsayıcısından yükleme
1. Giriş kapsayıcısı için bir SAS URI oluşturma
1. Çıkış kabı için bir SAS URI oluşturma
1. [Model dönüşümbaşlatmak](../how-tos/conversion/model-conversion.md) için [model dönüştürme REST API'yi](../how-tos/conversion/conversion-rest-api.md) arayın
1. Dönüşüm başarılı olana veya başarısız olana kadar dönüşüm durumunu yoklama
1. Dönüştürülen dosya konumunun çıktı ayrıntıları (depolama hesabı, çıkış kapsayıcısı, kapsayıcıdaki dosya yolu)
1. Çıkış blob kabında dönüştürülmüş modele bir SAS URI çıktı

### <a name="additional-command-line-options"></a>Ek komut satırı seçenekleri

Alternatif bir **config** dosyası kullanmak için:

```PowerShell
.\Conversion.ps1 -ConfigFile D:\arr\myotherconfigFile.json
```

Yalnızca **yoklama yapmadan model dönüştürmebaşlatmak**için şunları kullanabilirsiniz:

```PowerShell
.\Conversion.ps1 -ConvertAsset
```

Aşağıdaki komut satırı anahtarlarını kullanarak config dosyasından **tek tek ayarları geçersiz kılabilirsiniz:**

* **Id:** GetConversionStatus ile kullanılan ConversionId
* **ArrAccountId:** hesap Ayarları arrAccountId
* **ArrAccountKey:** hesap Ayarları arrAccountKey için geçersiz kılma
* **Bölge:** hesap Ayarları bölgesi için geçersiz kılma
* **ResourceGroup:** varlıkDönüşüm Ayarları kaynak Grubu için geçersiz kılma
* **StorageAccountName:** depolama için geçersiz kılmaHesaba varlıkDönüşüm Ayarları
* **BlobInputContainerName:** varlık Dönüşüm Ayarları blobInputContainer için geçersiz kılma
* **LocalAssetDirectoryPath:** yerelAssetDirectoryPath varlıkDönüşüm Ayarları için geçersiz kılma
* **InputAssetPath:** varlıkDönüşüm Ayarları nın inputAssetPath için geçersiz kılma
* **BlobOutputContainerName:** varlıkDönüşüm Ayarları blobOutputContainerName için geçersiz kılma
* **OutputFolderPath:** varlığın outputFolderPath için geçersiz kılmaConversionSettings
* **OutputAssetFileName:** outputAssetFileName için geçersiz kılmaAssetFileName of assetConversionSettings

Örneğin, aşağıdaki gibi verilen seçeneklerden bir dizisini birleştirebilirsiniz:

```PowerShell
.\Conversion.ps1 -LocalAssetDirectoryPath "C:\\models\\box" -InputAssetPath box.fbx -OutputFolderPath another/converted/box -OutputAssetFileName newConversionBox.arrAsset
```

### <a name="run-the-individual-conversion-stages"></a>Tek tek dönüşüm aşamalarını çalıştırma

İşlemin tek tek adımlarını çalıştırmak istiyorsanız şunları kullanabilirsiniz:

Yalnızca verilen LocalAssetDirectoryPath'ten veri yükleyin

```PowerShell
.\Conversion.ps1 -Upload
```

Yalnızca blob depolama alanına zaten yüklenen bir modelin dönüşüm işlemini başlatın (Upload'u çalıştırmayın, dönüşüm durumunu yoklamayın) Komut dosyası bir *conversionId*döndürür.

```PowerShell
.\Conversion.ps1 -ConvertAsset
```

Ve bu dönüşümün dönüşüm durumunu aşağıdakileri kullanarak alabilirsiniz:

```PowerShell
.\Conversion.ps1 -GetConversionStatus -Id <conversionId> [-Poll]
```

Dönüştürme `-Poll` yapılana veya bir hata oluşana kadar beklemek için kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Quickstart: Unity ile bir model oluşturma](../quickstarts/render-model.md)
- [Quickstart: Oluşturma için bir modeli dönüştürme](../quickstarts/convert-model.md)
- [Model dönüştürme](../how-tos/conversion/model-conversion.md)
