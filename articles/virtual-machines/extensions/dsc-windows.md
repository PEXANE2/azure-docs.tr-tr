---
title: Azure İstenilen Durum Yapılandırma Uzantısı İşleyicisi
description: DSC Uzantısını kullanarak Azure VM'ye PowerShell DSC yapılandırması yükleme ve uygulama
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: windows
ms.workload: ''
ms.date: 03/26/2018
ms.author: robreed
ms.openlocfilehash: 592c731d1851ac36cf9b57864750df0603b6c3fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253968"
---
# <a name="powershell-dsc-extension"></a>PowerShell DSC Uzantısı

## <a name="overview"></a>Genel Bakış

Windows için PowerShell DSC Uzantısı Microsoft tarafından yayımlanır ve desteklenir. Uzantı, Azure VM'de PowerShell DSC Yapılandırması yükler ve uygular. DSC Uzantı, VM'de alınan DSC yapılandırmasını yürürlüğe koymak için PowerShell DSC'ye çağrı dar. Bu belge, Windows için DSC sanal makine uzantısı için desteklenen platformları, yapılandırmaları ve dağıtım seçeneklerini ayrıntılarıyla açıklar.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

DSC Uzantısı aşağıdaki işletim sistemi destekler

Windows Server 2019, Windows Server 2016, Windows Server 2012R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows Istemci 7/8.1/10

### <a name="internet-connectivity"></a>İnternet bağlantısı

Windows için DSC uzantısı, hedef sanal makinenin Azure dışında bir konumda depolanırsa Azure ve yapılandırma paketinin (.zip dosyası) konumuyla iletişim kurabilmesini gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON, Azure Kaynak Yöneticisi şablonunda DSC Uzantısı'nın ayarlar bölümünün şeasını gösterir. 

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "Microsoft.Powershell.DSC",
  "apiVersion": "2018-10-01",
  "location": "<location>",
  "properties": {
    "publisher": "Microsoft.Powershell",
    "type": "DSC",
    "typeHandlerVersion": "2.77",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "wmfVersion": "latest",
        "configuration": {
            "url": "http://validURLToConfigLocation",
            "script": "ConfigurationScript.ps1",
            "function": "ConfigurationFunction"
        },
        "configurationArguments": {
            "argument1": "Value1",
            "argument2": "Value2"
        },
        "configurationData": {
            "url": "https://foo.psd1"
        },
        "privacy": {
            "dataCollection": "enable"
        },
        "advancedOptions": {
            "forcePullAndApply": false,
            "downloadMappings": {
                "specificDependencyKey": "https://myCustomDependencyLocation"
            }
        } 
    },
    "protectedSettings": {
        "configurationArguments": {
            "parameterOfTypePSCredential1": {
                "userName": "UsernameValue1",
                "password": "PasswordValue1"
            },
            "parameterOfTypePSCredential2": {
                "userName": "UsernameValue2",
                "password": "PasswordValue2"
            }
        },
        "configurationUrlSasToken": "?g!bber1sht0k3n",
        "configurationDataUrlSasToken": "?dataAcC355T0k3N"
    }
  }
}
```

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / Örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2018-10-01 | date |
| yayımcı | Microsoft.Powershell.DSC | string |
| type | DSC | string |
| typeHandlerVersion | 2.77 | int |

### <a name="settings-property-values"></a>Ayarlar Özellik değerleri

| Adı | Veri Türü | Açıklama
| ---- | ---- | ---- |
| settings.wmfVersion | string | Windows Yönetim Çerçevesi'nin VM'nize yüklenmesi gereken sürümünü belirtir. Bu özelliği 'en son' olarak ayarlamak WMF'nin en güncel sürümünü yükler. Bu özellik için geçerli tek geçerli değerler '4.0', '5.0', ve 'latest' 'dır. Bu olası değerler güncelleştirmelere tabidir. Varsayılan değer 'en son'dur. |
| settings.configuration.url | string | DSC yapılandırma zip dosyanızı indirmek için URL konumunu belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, protectedSettings.configurationUrlSasToken özelliğini SAS belirteçlerinizin değerine ayarlamanız gerekir. bu özellik, settings.configuration.configuration ve/veya settings.configuration.function tanımlanırsa gereklidir.
| settings.configuration.script | string | DSC yapılandırmanızın tanımını içeren komut dosyasının dosya adını belirtir. Bu komut dosyası configuration.url özelliği tarafından belirtilen URL'den indirilen zip dosyasının kök klasöründe olmalıdır. settings.configuration.url ve/veya settings.configuration.script tanımlanırsa bu özellik gereklidir.
| settings.configuration.function | string | DSC yapılandırmanızın adını belirtir. Adlandırılmış yapılandırma configuration.script tarafından tanımlanan komut dosyasında içermelidir. settings.configuration.url ve/veya settings.configuration.function tanımlanırsa bu özellik gereklidir.
| settings.configurationBağımsız Değişkenler | Koleksiyon | DSC yapılandırmanıza geçirmek istediğiniz parametreleri tanımlar. Bu özellik şifrelenmez.
| settings.configurationData.url | string | DSC yapılandırmanız için giriş olarak kullanılacak yapılandırma verilerinizi (.pds1) dosyanızı indirecek URL'yi belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, protectedSettings.configurationDataUrlSasToken özelliğini SAS belirteçlerinizin değerine ayarlamanız gerekir.
| settings.privacy.dataEtkin | string | Telemetri koleksiyonunu etkinleştirer veya devre dışı kılabilir. Bu özellik için tek olası değerler 'Etkinleştir', 'Devre Dışı' veya $null. Bu özelliği boş veya null bırakmak telemetri yi etkinleştirecektir
| settings.advancedOptions.forcePullAndApply | Bool | Bu ayar, düğümleri Azure Automation DSC'ye kaydetmek için uzantıyla çalışma deneyimini geliştirmek üzere tasarlanmıştır.  Değer ise, `$true`uzantı başarı/hata döndürmeden önce hizmetten çıkarılan yapılandırmanın ilk çalışmasını bekler.  Değer $false olarak ayarlanırsa, uzantı tarafından döndürülen durum yalnızca düğümün Azure Otomasyon Durumu Yapılandırması'na başarıyla kaydedilip kaydedilmediği ve düğüm yapılandırmasının çalıştırılmayacağı anlamına gelecektir.
| settings.advancedOptions.downloadMappings | Koleksiyon | WMF ve .NET gibi bağımlılıkları indirmek için alternatif konumlar tanımlar

### <a name="protected-settings-property-values"></a>Korumalı Ayarlar Özellik değerleri

| Adı | Veri Türü | Açıklama
| ---- | ---- | ---- |
| protectedSettings.configurationBağımsız Değişkenler | string | DSC yapılandırmanıza geçirmek istediğiniz parametreleri tanımlar. Bu özellik şifrelenecektir. |
| protectedSettings.configurationUrlSasToken | string | configuration.url tarafından tanımlanan URL'ye erişmek için SAS belirteci belirtir. Bu özellik şifrelenecektir. |
| protectedSettings.configurationDataUrlSasToken | string | ConfigurationData.url tarafından tanımlanan URL'ye erişmek için SAS belirteci belirtir. Bu özellik şifrelenecektir. |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir.
Şablonlar, dağıtım sonrası yapılandırma gerektiren bir veya daha fazla sanal makine dağıtılırken idealdir.
Windows için DSC uzantısını içeren örnek bir Kaynak Yöneticisi şablonu [Azure Hızlı Başlangıç Galerisi'nde](https://github.com/Azure/azure-quickstart-templates/blob/master/101-automation-configuration/nested/provisionServer.json#L91)bulunabilir.

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı paketi Azure VM'de indirilir ve bu konuma dağıtılır
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}
```

Uzantı durum dosyası, her uzantı çalışması için ayrıntılı hata ve açıklamanın yanı sıra alt durum ve durum başarı/hata kodlarını içerir.
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}\Status\{0}.Status  -> {0} being the sequence number
```

Uzantı çıktı günlükleri aşağıdaki dizine kaydedilir:

```
C:\WindowsAzure\Logs\Plugins\{Extension_Name}\{Extension_Version}
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

| Hata Kodu | Anlamı | Olası Eylem |
| :---: | --- | --- |
| 1000 | Genel hata | Bu hata nın iletisi, uzantı günlüklerinde belirli bir özel durum tarafından sağlanır |
| 52 | Uzantı Yükleme Hatası | Bu hata nın iletisi belirli bir özel durum tarafından sağlanır |
| 1002 | Wmf Yükleme Hatası | WMF yüklerken hata. |
| 1004 | Geçersiz Zip Paketi | Geçersiz zip ; Zip'i açma hatası |
| 1100 | Bağımsız Değişken Hatası | Kullanıcı tarafından sağlanan girişteki bir sorunu gösterir. Hata iletisi belirli bir özel durum tarafından sağlanır|


### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
