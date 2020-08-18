---
title: Örnek PowerShell betikleri
description: Ön ucun PowerShell betikleri aracılığıyla nasıl kullanılacağını gösteren örnekler
author: florianborn71
ms.author: flborn
ms.date: 02/12/2020
ms.topic: sample
ms.openlocfilehash: 07055025eff9ab81c7321624daed9b4a6e993a60
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88506520"
---
# <a name="example-powershell-scripts"></a>Örnek PowerShell betikleri

Azure uzaktan Işleme aşağıdaki iki REST API 'yi sağlar:

- [Dönüştürme REST API](../how-tos/conversion/conversion-rest-api.md)
- [Oturum REST API](../how-tos/session-rest-api.md)

[ARR örnekleri deposu](https://github.com/Azure/azure-remote-rendering) , hizmetin REST API 'leriyle etkileşim kurmak için *Scripts* klasöründeki örnek betikleri içerir. Bu makalede kullanımları açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Örnek betikleri yürütmek için [Azure PowerShell](https://docs.microsoft.com/powershell/azure/)işlevsel kurulumuna ihtiyacınız vardır.

1. Azure PowerShell'i yükleme:
    1. Yönetici haklarına sahip bir PowerShell penceresi açın.
    1. Çalışmaz `Install-Module -Name Az -AllowClobber`

1. Betikleri çalıştırma hakkında hata alırsanız [yürütme ilkenizin](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) uygun şekilde ayarlandığından emin olun:
    1. Yönetici haklarına sahip bir PowerShell penceresi açın.
    1. Çalışmaz `Set-ExecutionPolicy -ExecutionPolicy Unrestricted`

1. [Azure depolama hesabı hazırlama](../how-tos/conversion/blob-storage.md#prepare-azure-storage-accounts)

1. Azure uzaktan Işleme hesabınızı içeren aboneliğinizde oturum açın:
    1. PowerShell penceresini açın.
    1. Şunu çalıştırın: `Connect-AzAccount` ve ekrandaki yönergeleri izleyin.

    > [!NOTE]
    > Kuruluşunuzun birden fazla aboneliğine sahip olması durumunda, SubscriptionID ve Tenant bağımsız değişkenlerini belirtmeniz gerekebilir. [Connect-AzAccount belgelerindeki](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount)ayrıntıları bulun.

1. [Azure uzaktan Işleme GithHub deposundan](https://github.com/Azure/azure-remote-rendering) *betikler* klasörünü indirin.

## <a name="configuration-file"></a>Yapılandırma dosyası

Dosyaların yanında, `.ps1` doldurmanız gereken bir sorun var `arrconfig.json` :

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
> Çift ters eğik çizgi kullanarak LocalAssetDirectoryPath yolundaki ters eğik çizgileri doğru bir şekilde atladığınızdan emin olun: " \\ \\ " ve ınputfolderpath ve ınputassetpath gibi tüm diğer yollarda eğik çizgileri "/" kullanın.

> [!CAUTION]
> İsteğe bağlı değerler doldurulmalıdır veya anahtar ve değeri tamamen kaldırmanız gerekir. Örneğin,  `"outputAssetFileName"` parametresini kullanmazsanız, içindeki tüm satırı silmeniz gerekir `arrconfig.json` .

### <a name="accountsettings"></a>accountSettings

`arrAccountId`Ve için `arrAccountKey` bkz. [Azure uzaktan işleme hesabı oluşturma](../how-tos/create-an-account.md).
İçin `region` [kullanılabilir bölgelerin listesini](../reference/regions.md)görüntüleyin.

### <a name="renderingsessionsettings"></a>renderingSessionSettings

**RenderingSession.ps1**çalıştırmak istiyorsanız bu yapının doldurulması gerekir:

- **VMSize:** Sanal makinenin boyutunu seçer. [*Standart*](../reference/vm-sizes.md) veya [*Premium*](../reference/vm-sizes.md)' u seçin. Artık ihtiyacınız olmadığında işleme oturumlarını kapatın.
- **Maxleasetime:** VM 'nin kiralanmasını istediğiniz süre. Kira süresi dolarsa kapatılacak. Kira süresi daha sonra genişletilebilir (aşağıya bakın).

### <a name="assetconversionsettings"></a>assetConversionSettings

**Conversion.ps1**çalıştırmak istiyorsanız bu yapının doldurulması gerekir.

Ayrıntılar için bkz. [Azure Storage hesabı hazırlama](../how-tos/conversion/blob-storage.md#prepare-azure-storage-accounts).

## <a name="script-renderingsessionps1"></a>Betik: RenderingSession.ps1

Bu betik, işleme oturumlarını oluşturmak, sorgulamak ve durdurmak için kullanılır.

> [!IMPORTANT]
> Üzerinde arrconfig.js*Accountsettings* ve *Renderingsessionsettings* bölümlerinin doldurulduğundan emin olun.

### <a name="create-a-rendering-session"></a>İşleme oturumu oluşturma

Üzerinde tamamen doldurulmuş arrconfig.jsnormal kullanım:

```PowerShell
.\RenderingSession.ps1
```

Betik, belirtilen ayarlarla bir işleme sanal makinesi çalıştırmak için [oturum yönetimi REST API](../how-tos/session-rest-api.md) çağırır. Başarılı olduğunda, *SessionID*'yi alır. Ardından, oturum hazırlanana veya bir hata gerçekleşene kadar oturum özelliklerini yoklayacaktır.

**Alternatif bir yapılandırma** dosyası kullanmak için:

```PowerShell
.\RenderingSession.ps1 -ConfigFile D:\arr\myotherconfigFile.json
```

Yapılandırma dosyasından **bağımsız ayarları geçersiz kılabilirsiniz** :

```PowerShell
.\RenderingSession.ps1 -Region <region> -VmSize <vmsize> -MaxLeaseTime <hh:mm:ss>
```

Yalnızca **yoklama olmadan bir oturum başlatmak**için şunu kullanabilirsiniz:

```PowerShell
.\RenderingSession.ps1 -CreateSession
```

Betiğin aldığı *SessionID* diğer oturum komutlarının çoğuna geçirilmesi gerekir.

### <a name="retrieve-session-properties"></a>Oturum özelliklerini al

Bir oturumun özelliklerini almak için şunu çalıştırın:

```PowerShell
.\RenderingSession.ps1 -GetSessionProperties -Id <sessionID> [-Poll]
```

`-Poll`Oturum *başlamaya* veya bir hata oluşuncaya kadar beklemek için kullanın.

### <a name="list-active-sessions"></a>Etkin oturumları listeleme

```PowerShell
.\RenderingSession.ps1 -GetSessions
```

### <a name="stop-a-session"></a>Oturumu Durdur

```PowerShell
.\RenderingSession.ps1 -StopSession -Id <sessionID>
```

### <a name="change-session-properties"></a>Oturum özelliklerini değiştirme

Şu anda yalnızca bir oturumun maxLeaseTime değerini değiştirmeyi destekliyoruz.

> [!NOTE]
> Kira süresi her zaman, oturum VM 'sinin ilk olarak oluşturulduğu zamandan itibaren sayılır. Bu nedenle, oturum kiralamasını başka bir saate göre uzatmak için *Maxleasetime* değerini bir saat artırın.

```PowerShell
.\RenderingSession.ps1 -UpdateSession -Id <sessionID> -MaxLeaseTime <hh:mm:ss>
```

## <a name="script-conversionps1"></a>Betik: Conversion.ps1

Bu betik, giriş modellerini Azure uzaktan Işleme özel çalışma zamanı biçimine dönüştürmek için kullanılır.

> [!IMPORTANT]
> Üzerinde arrconfig.js*accountsettings* ve *assetconversionsettings* bölümlerinin doldurulduğundan emin olun.

Betik, depolama hesaplarını hizmeti ile birlikte kullanmak için iki seçeneği gösterir:

- Azure uzaktan Işleme hesabıyla bağlantılı depolama hesabı
- Paylaşılan erişim Imzaları (SAS) aracılığıyla depolamaya erişim sağlama

### <a name="linked-storage-account"></a>Bağlı depolama hesabı

Bir depolama hesabını tam olarak doldurduktan sonra arrconfig.jsve bağlı olduktan sonra aşağıdaki komutu kullanabilirsiniz. Depolama hesabınızı bağlama, [Hesap oluşturma](../how-tos/create-an-account.md#link-storage-accounts)bölümünde açıklanmaktadır.

Bağlı depolama hesabı kullanmak, paylaşılan erişim Imzaları oluşturmaya gerek olmadığı için dönüştürme hizmetinin kullanılması için tercih edilen yoldur.

```PowerShell
.\Conversion.ps1
```

1. İçindeki içindeki tüm dosyaları verilen ' `assetConversionSettings.modelLocation` ın altındaki giriş blobu kapsayıcısına `inputFolderPath` yükleyin.
1. Model [dönüştürmeyi](../how-tos/conversion/model-conversion.md) kapatmak için [model dönüştürme REST API](../how-tos/conversion/conversion-rest-api.md) çağırın
1. Dönüştürme başarılı veya başarısız olana kadar dönüştürme durumunu yoklayın.
1. Dönüştürülmüş dosya konumunun çıkış ayrıntıları (depolama hesabı, çıkış kapsayıcısı, kapsayıcıdaki dosya yolu).

### <a name="access-to-storage-via-shared-access-signatures"></a>Paylaşılan erişim Imzaları aracılığıyla depolamaya erişim

```PowerShell
.\Conversion.ps1 -UseContainerSas
```

Şunları yapmak gerekir:

1. Yerel dosyayı öğesinden `assetConversionSettings.localAssetDirectoryPath` giriş blobu kapsayıcısına yükleyin.
1. Giriş kapsayıcısı için bir SAS URI 'SI oluşturun.
1. Çıkış kapsayıcısı için bir SAS URI 'SI oluşturun.
1. Model [dönüştürmeyi](../how-tos/conversion/model-conversion.md)kapatmak için [model dönüştürme REST API](../how-tos/conversion/conversion-rest-api.md) çağırın.
1. Dönüştürme başarılı veya başarısız olana kadar dönüştürme durumunu yoklayın.
1. Dönüştürülmüş dosya konumunun çıkış ayrıntıları (depolama hesabı, çıkış kapsayıcısı, kapsayıcıdaki dosya yolu).
1. Çıkış blob kapsayıcısında dönüştürülen modele bir SAS URI 'SI çıkışı.

### <a name="additional-command-line-options"></a>Ek komut satırı seçenekleri

**Alternatif bir yapılandırma** dosyası kullanmak için:

```PowerShell
.\Conversion.ps1 -ConfigFile D:\arr\myotherconfigFile.json
```

Yalnızca **yoklama olmadan model dönüştürmeyi başlatmak**için şunu kullanabilirsiniz:

```PowerShell
.\Conversion.ps1 -ConvertAsset
```

Aşağıdaki komut satırı anahtarlarını kullanarak yapılandırma dosyasından **bağımsız ayarları geçersiz kılabilirsiniz** :

* **Kimliği:** GetConversionStatus ile kullanılan Conversionıd
* **ArrAccountId:** accountsettings ArrAccountId
* **ArrAccountKey:** Accountsettings ArrAccountKey için override
* **Bölge:** accountsettings bölgesi için geçersiz kılma
* **ResourceGroup:** assetConversionSettings resourcegroup için geçersiz kılma
* **StorageAccountName:** assetConversionSettings 'In storageAccountName için geçersiz kıl
* **Blobinputcontainername:** assetConversionSettings 'In blobInputContainer için geçersiz kılma
* **Localassetdirectorypath:** assetConversionSettings Için localassetdirectorypath geçersiz kılma
* **Inputassetpath:** assetConversionSettings 'In ınputassetpath için geçersiz kılma
* **Bloi putcontainername:** assetConversionSettings 'In Bloi putcontainername için geçersiz kılma
* **Outputfolderpath:** Outputconversionsettings için outputfolderpath için override
* **Outputassetfilename:** assetConversionSettings 'In outputassetfilename için geçersiz kılma

Örneğin, aşağıdaki gibi bir dizi verilen seçeneği birleştirebilirsiniz:

```PowerShell
.\Conversion.ps1 -LocalAssetDirectoryPath "C:\\models\\box" -InputAssetPath box.fbx -OutputFolderPath another/converted/box -OutputAssetFileName newConversionBox.arrAsset
```

### <a name="run-the-individual-conversion-stages"></a>Bireysel dönüştürme aşamalarını çalıştırma

İşlemin tek tek adımlarını çalıştırmak istiyorsanız şunları kullanabilirsiniz:

Yalnızca verilen LocalAssetDirectoryPath öğesinden verileri karşıya yükleyin.

```PowerShell
.\Conversion.ps1 -Upload
```

Yalnızca blob depolamaya zaten yüklenmiş bir modelin dönüştürme işlemini başlatın (karşıya yükleme çalıştırılmayın, dönüştürme durumunu yoklamayın) betik bir *Conversionıd*döndürür.

```PowerShell
.\Conversion.ps1 -ConvertAsset
```

Bu dönüştürmenin dönüştürme durumunu şu şekilde alabilirsiniz:

```PowerShell
.\Conversion.ps1 -GetConversionStatus -Id <conversionId> [-Poll]
```

`-Poll`Dönüştürme işlemi tamamlanana veya bir hata oluştuğundan beklemek için kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: Unity ile model Işleme](../quickstarts/render-model.md)
- [Hızlı başlangıç: bir modeli işleme için dönüştürme](../quickstarts/convert-model.md)
- [Model dönüştürme](../how-tos/conversion/model-conversion.md)
