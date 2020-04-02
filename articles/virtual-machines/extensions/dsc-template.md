---
title: Azure Kaynak Yöneticisi şablonları ile İstenen Durum Yapılandırması uzantısı
description: Azure'da İstenilen Durum Yapılandırması (DSC) uzantısı için Kaynak Yöneticisi şablon tanımı hakkında bilgi edinin.
services: virtual-machines-windows
author: bobbytreed
manager: carmonm
tags: azure-resource-manager
keywords: Dsc
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/05/2018
ms.author: robreed
ms.openlocfilehash: 89bae60c30cfe82d38c61f385dbaef574b4152d8
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80547791"
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonları ile İstenen Durum Yapılandırması uzantısı

Bu makalede, [İstenilen Durum Yapılandırması (DSC) uzantıiş](dsc-overview.md)için Azure Kaynak Yöneticisi şablonu açıklanır. Örneklerin çoğu, Azure Otomasyonu ile yerleşik kullanıma **registrationurl** (String olarak sağlanan) ve **RegistrationKey'i** [(PSCredential](/dotnet/api/system.management.automation.pscredential)olarak sağlanmıştır) kullanır. Bu değerleri edinme hakkında ayrıntılı bilgi için Azure [Automation State Configuration - Güvenli kayıt tarafından yönetilen Onboarding makineleri](/azure/automation/automation-dsc-onboarding#onboarding-securely-using-registration)ne bıyiklerine bakın.

> [!NOTE]
> Biraz farklı şema örnekleri karşılaşabilirsiniz. Şemadaki değişiklik Ekim 2016 sürümünde meydana geldi. Ayrıntılar için önceki [biçimden Güncelleştirme'ye](#update-from-a-previous-format)bakın.

## <a name="template-example-for-a-windows-vm"></a>Windows VM için şablon örneği

Aşağıdaki parçacık şablonun **Kaynak** bölümüne gider.
DSC uzantısı varsayılan uzantı özelliklerini devralır.
Daha fazla bilgi için [VirtualMachineExtension sınıfına](/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet)bakın.

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "Microsoft.Powershell.DSC",
  "apiVersion": "2018-06-30",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Powershell",
    "type": "DSC",
    "typeHandlerVersion": "2.77",
    "autoUpgradeMinorVersion": true,
    "protectedSettings": {
      "Items": {
        "registrationKeyPrivate": "[listKeys(resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName')), '2018-06-30').Keys[0].value]"
      }
    },
    "settings": {
      "Properties": [
        {
          "Name": "RegistrationKey",
          "Value": {
            "UserName": "PLACEHOLDER_DONOTUSE",
            "Password": "PrivateSettingsRef:registrationKeyPrivate"
          },
          "TypeName": "System.Management.Automation.PSCredential"
        },
        {
          "Name": "RegistrationUrl",
          "Value": "[reference(concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))).registrationUrl]",
          "TypeName": "System.String"
        },
        {
          "Name": "NodeConfigurationName",
          "Value": "[parameters('nodeConfigurationName')]",
          "TypeName": "System.String"
        }
      ]
    }
  }
}
```

## <a name="template-example-for-windows-virtual-machine-scale-sets"></a>Windows sanal makine ölçek kümeleri için şablon örneği

Sanal makine ölçeği kümesi düğümü, **VirtualMachineProfile, extensionProfile** özniteliği olan bir **özellik** bölümüne sahiptir.
**Uzantıları**altında, DSC Uzantısı için ayrıntıları ekleyin.

DSC uzantısı varsayılan uzantı özelliklerini devralır.
Daha fazla bilgi için [VirtualMachineScaleSetExtension sınıfına](/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet)bakın.

```json
"extensionProfile": {
    "extensions": [
      {
        "name": "Microsoft.Powershell.DSC",
        "properties": {
          "publisher": "Microsoft.Powershell",
          "type": "DSC",
          "typeHandlerVersion": "2.77",
          "autoUpgradeMinorVersion": true,
          "protectedSettings": {
            "Items": {
              "registrationKeyPrivate": "[listKeys(resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName')), '2018-06-30').Keys[0].value]"
            }
          },
          "settings": {
            "Properties": [
              {
                "Name": "RegistrationKey",
                "Value": {
                  "UserName": "PLACEHOLDER_DONOTUSE",
                  "Password": "PrivateSettingsRef:registrationKeyPrivate"
                },
                "TypeName": "System.Management.Automation.PSCredential"
              },
              {
                "Name": "RegistrationUrl",
                "Value": "[reference(concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))).registrationUrl]",
                "TypeName": "System.String"
              },
              {
                "Name": "NodeConfigurationName",
                "Value": "[parameters('nodeConfigurationName')]",
                "TypeName": "System.String"
              }
            ]
          }
        }
      }
    ]
  }
```

## <a name="detailed-settings-information"></a>Ayrıntılı ayarlar bilgileri

Kaynak Yöneticisi şablonundaki Azure DSC uzantısının **ayarlar** bölümünde aşağıdaki şema'yı kullanın.

Varsayılan yapılandırma komut dosyası için kullanılabilen bağımsız değişkenlerin listesi için varsayılan [yapılandırma komut dosyasına](#default-configuration-script)bakın.

```json
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
        "downloadMappings": {
            "customWmfLocation": "http://myWMFlocation"
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
```

## <a name="details"></a>Ayrıntılar

| Özellik adı | Tür | Açıklama |
| --- | --- | --- |
| settings.wmfVersion |string |VM'nize yüklenmesi gereken Windows Yönetim Çerçevesi (WMF) sürümünü belirtir. Bu özelliği **en son** olarak ayarlamak WMF'nin en son sürümünü yükler. Şu anda, bu özellik için tek olası değerler **4.0**, **5.0**, **5.1**, ve **en son**. Bu olası değerler güncelleştirmelere tabidir. Varsayılan değer **en sondur.** |
| settings.configuration.url |string |DSC yapılandırmanızı indirmek için URL konumunu belirtir .zip dosyası. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, **protectedSettings.configurationUrlSasToken** özelliğini SAS belirteçinizin değerine ayarlayın. **ayarlar.configuration.configuration veya** **settings.configuration.function** tanımlanırsa bu özellik gereklidir. Bu özellikler için değer verilmezse, uzantı Konum Yapılandırma Yöneticisi (LCM) meta verilerini ayarlamak için varsayılan yapılandırma komut dosyasını çağırır ve bağımsız değişkenler sağlanmalıdır. |
| settings.configuration.script |string |DSC yapılandırmanızın tanımını içeren komut dosyasının dosya adını belirtir. Bu komut dosyası, **settings.configuration.url** özelliği tarafından belirtilen URL'den indirilen .zip dosyasının kök klasöründe olmalıdır. **settings.configuration.url** veya **settings.configuration.script** tanımlanırsa bu özellik gereklidir. Bu özellikler için değer verilmezse, uzantı LCM meta verilerini ayarlamak için varsayılan yapılandırma komut dosyasını çağırır ve bağımsız değişkenler sağlanmalıdır. |
| settings.configuration.function |string |DSC yapılandırmanızın adını belirtir. Adlandırılmış **yapılandırma, settings.configuration.script'in** tanımladığı komut dosyasına dahil edilmelidir. **ayarlar.configuration.url** veya **settings.configuration.function** tanımlanırsa bu özellik gereklidir. Bu özellikler için değer verilmezse, uzantı LCM meta verilerini ayarlamak için varsayılan yapılandırma komut dosyasını çağırır ve bağımsız değişkenler sağlanmalıdır. |
| settings.configurationBağımsız Değişkenler |Koleksiyon |DSC yapılandırmanıza geçirmek istediğiniz parametreleri tanımlar. Bu özellik şifrelenmez. |
| settings.configurationData.url |string |DSC yapılandırmanız için giriş olarak kullanılacak yapılandırma verilerinizi (.psd1) dosyanızı indirecek URL'yi belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, **protectedSettings.configurationDataUrlSasToken** özelliğini SAS belirteçinizin değerine ayarlayın. |
| settings.privacy.dataCollection |string |Telemetri koleksiyonunu etkinleştirer veya devre dışı kılabilir. Bu özellik için tek olası değerler **Etkinleştir**, **Devre Dışı ,** **''** veya **$null.** Bu özelliği boş veya null bırakarak telemetri sağlar. Varsayılan değer **''** olur. Daha fazla bilgi için Azure [DSC uzantısı veri koleksiyonuna](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/)bakın. |
| settings.advancedOptions.downloadMappings |Koleksiyon |WMF'yi karşıdan yükleyeceğimiz alternatif konumları tanımlar. Daha fazla bilgi için Azure [DSC uzantısı 2.8'e ve uzantı bağımlılıklarının kendi konumunuza nasıl indirilir haritasını](https://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx)çıkarın. |
| protectedSettings.configurationBağımsız Değişkenler |Koleksiyon |DSC yapılandırmanıza geçirmek istediğiniz parametreleri tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationUrlSasToken |string |**Settings.configuration.url'nin** tanımladığı URL'ye erişmek için kullanılacak SAS belirteci belirtir. Bu özellik şifrelenir. |
| protectedSettings.configurationDataUrlSasToken |string |**Settings.configurationData.url'nin** tanımladığı URL'ye erişmek için kullanılacak SAS belirteci belirtir. Bu özellik şifrelenir. |

## <a name="default-configuration-script"></a>Varsayılan yapılandırma komut dosyası

Aşağıdaki değerler hakkında daha fazla bilgi için [Yerel Yapılandırma Yöneticisi temel ayarlarına](/powershell/scripting/dsc/managing-nodes/metaConfig#basic-settings)bakın.
Yalnızca aşağıdaki tabloda listelenen LCM özelliklerini yapılandırmak için DSC uzantı varsayılan yapılandırma komut dosyasını kullanabilirsiniz.

| Özellik adı | Tür | Açıklama |
| --- | --- | --- |
| protectedSettings.configurationArguments.RegistrationKey |PSKİnER |Gerekli özellik. Bir düğüm için kullanılan anahtarın Azure Otomasyon hizmetine kaydolması için kullanılan anahtarı powershell kimlik nesnesinin parolası olarak belirtir. Bu değer, Otomasyon hesabına karşı **listkeys** yöntemi kullanılarak otomatik olarak keşfedilebilir.  [Örneğe](#example-using-referenced-azure-automation-registration-values)bakın. |
| settings.configurationArguments.RegistrationUrl |string |Gerekli özellik. Düğümün kaydolmaya çalıştığı Otomasyon bitiş noktasının URL'sini belirtir. Bu değer, Otomasyon hesabına karşı **referans** yöntemi kullanılarak otomatik olarak keşfedilebilir. |
| settings.configurationArguments.NodeConfigurationName |string |Gerekli özellik. Düğüme atamak için Otomasyon hesabındaki düğüm yapılandırmasını belirtir. |
| settings.configurationArguments.ConfigurationMode |string |LCM modunu belirtir. Geçerli seçenekler arasında **ApplyOnly**, **ApplyandMonitor**ve **ApplyandAutoCorrect sayılabilir.**  Varsayılan değer **ApplyandMonitor'dur.** |
| settings.configurationArguments.RefreshFrequencyMins | uint32 | LCM'nin güncelleştirmeler için Otomasyon hesabıyla ne sıklıkta kontrol etmeye çalıştığı belirtilir.  Varsayılan değer **30'dur.**  Minimum değer **15'tir.** |
| settings.configurationArguments.ConfigurationModeFrequencyMins | uint32 | LCM'nin geçerli yapılandırmayı ne sıklıkta doğruladığını belirtir. Varsayılan değer **15'tir.** Minimum değer **15'tir.** |
| settings.configurationArguments.RebootNodeIfNeeded | boole | Bir DSC işlemi isterse düğümün otomatik olarak yeniden başlatılıp yeniden başlatılamayacağını belirtir. Varsayılan değer **yanlıştır.** |
| settings.configurationArguments.ActionAfterReboot | string | Yapılandırma uygularken yeniden başlatıldıktan sonra ne olacağını belirtir. Geçerli seçenekler **ContinueConfiguration** ve **StopConfiguration**vardır. Varsayılan değer **ContinueConfiguration'dır.** |
| settings.configurationArguments.AllowModuleOverwrite | boole | LCM'nin düğümdeki varolan modüllerin üzerine yazıp yazmadığını belirtir. Varsayılan değer **yanlıştır.** |

## <a name="settings-vs-protectedsettings"></a>ayarlar ve protectedSettings

Tüm ayarlar VM'deki ayarlar metin dosyasına kaydedilir.
**Ayarlar** altında listelenen özellikler ortak özelliklerdir.
Ortak özellikler ayarlar metin dosyasında şifrelenmez.
**protectedSettings** altında listelenen özellikler bir sertifika ile şifrelenir ve VM'deki ayarlar dosyasında düz metin olarak gösterilmez.

Yapılandırmanın kimlik bilgilerine ihtiyacı varsa, kimlik bilgilerini **protectedSettings'e**ekleyebilirsiniz:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
               "userName": "UsernameValue1",
               "password": "PasswordValue1"
        }
    }
}
```

## <a name="example-configuration-script"></a>Örnek yapılandırma komut dosyası

Aşağıdaki örnek, LCM'ye meta veri ayarları sağlamak ve Automation DSC hizmetine kaydolmak olan DSC uzantısı için varsayılan davranışı gösterir.
Yapılandırma bağımsız değişkenleri gereklidir.
Yapılandırma bağımsız değişkenleri, LCM meta verilerini ayarlamak için varsayılan yapılandırma komut dosyasına geçirilir.

```json
"settings": {
    "configurationArguments": {
        "RegistrationUrl" : "[parameters('registrationUrl1')]",
        "NodeConfigurationName" : "nodeConfigurationNameValue1"
    }
},
"protectedSettings": {
    "configurationArguments": {
        "RegistrationKey": {
            "userName": "NOT_USED",
            "Password": "registrationKey"
        }
    }
}
```

## <a name="example-using-the-configuration-script-in-azure-storage"></a>Azure Depolama'da yapılandırma komut dosyasını kullanma örneği

Aşağıdaki [örnek, DSC uzantı işleyicisi genel bakış.](dsc-overview.md)
Bu örnek, uzantıyı dağıtmak için cmdlets yerine Kaynak Yöneticisi şablonlarını kullanır.
IisInstall.ps1 yapılandırmasını kaydedin, .zip dosyasına `iisinstall.zip`yerleştirin (örnek: ) ve ardından dosyayı erişilebilir bir URL'ye yükleyin.
Bu örnekte Azure Blob depolama alanı kullanır, ancak .zip dosyalarını herhangi bir rasgele konumdan indirebilirsiniz.

Kaynak Yöneticisi şablonunda, aşağıdaki kod VM'ye doğru dosyayı karşıdan yüklemesini ve ardından uygun PowerShell işlevini çalıştırmasını bildirir:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/iisinstall.zip",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="example-using-referenced-azure-automation-registration-values"></a>Başvurulan Azure Otomasyon kayıt değerlerini kullanma örneği

Aşağıdaki örnek, Azure Otomasyon hesap özelliklerine başvurarak ve Birincil Anahtar (0) almak için **listkeys** yöntemini kullanarak **RegistrationUrl** ve **RegistrationKey'i** alır.  Bu örnekte, şablona **otomasyonHesap Adı** ve **NodeConfigName** parametreleri sağlanmıştır.

```json
"settings": {
    "RegistrationUrl" : "[reference(concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))).registrationUrl]",
    "NodeConfigurationName" : "[parameters('NodeConfigName')]"
},
"protectedSettings": {
    "configurationArguments": {
        "RegistrationKey": {
            "userName": "NOT_USED",
            "Password": "[listKeys(resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName')), '2018-01-15').Keys[0].value]"
        }
    }
}
```

## <a name="update-from-a-previous-format"></a>Önceki biçimden güncelleştirme

Uzantının önceki biçimindeki tüm ayarlar (ve ortak özelliklere sahip **ModulesUrl**, **ModuleSource**, **ModuleVersion**, **ConfigurationFunction**, **SasToken**, veya **Özellikler**) otomatik olarak uzantının geçerli biçimine uyum sağlar.
Daha önce olduğu gibi kaçıyorlar.

Aşağıdaki şema önceki ayarları şema neye benzediğini gösterir:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties": {
        "ParameterToConfigurationFunction1": "Value1",
        "ParameterToConfigurationFunction2": "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1"
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": {
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Önceki biçim geçerli biçime şu şekilde uyarlanmıştır:

| Geçerli Özellik adı | Önceki şema eşdeğeri |
| --- | --- |
| settings.wmfVersion |Ayarlar. WMFVersion |
| settings.configuration.url |Ayarlar. ModüllerUrl |
| settings.configuration.script |Ayarların ilk bölümü. ConfigurationFunction (önce) \\ \\ |
| settings.configuration.function |Ayarların ikinci bölümü. ConfigurationFunction (sonra) \\ \\ |
| settings.configuration.module.name | Ayarlar. ModülKaynak |
| settings.configuration.module.version | Ayarlar. ModülVersiyonu |
| settings.configurationBağımsız Değişkenler |Ayarlar. Özellikler |
| settings.configurationData.url |protectedSettings.DataBlobUri (SAS belirteci olmadan) |
| settings.privacy.dataCollection |Ayarlar. Privacy.dataCollection |
| settings.advancedOptions.downloadMappings |Ayarlar. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationBağımsız Değişkenler |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |Ayarlar. SasToken |
| protectedSettings.configurationDataUrlSasToken |protectedSettings.DataBlobUri gelen SAS belirteci |

## <a name="troubleshooting"></a>Sorun giderme

Bunlarla karşınıza gelebilecek hatalardan bazıları ve bunları nasıl düzeltebileceğiniz aşağıda verilmiştir.

### <a name="invalid-values"></a>Geçersiz değerler

"Privacy.dataCollection '{0}'dır.
Olası tek değerler '', 'Etkinleştir', 'Devre Dışı' ve 'Devre Dışı' dır.
"WmfVersion '{0}'.
Sadece olası değerler ... ve 'son'".

**Sorun**: Sağlanan bir değere izin verilmez.

**Çözüm**: Geçersiz değeri geçerli bir değerle değiştirin.
Daha fazla bilgi için [Ayrıntılar](#details)tablosuna bakın.

### <a name="invalid-url"></a>Geçersiz URL

"ConfigurationData.url '{0}'dir. Bu geçerli bir URL değil" "DataBlobUri '{0}'. Bu geçerli bir URL değildir" "Configuration.url '{0}'. Bu geçerli bir URL değil"

**Sorun**: Sağlanan BIR URL geçerli değildir.

**Çözüm**: Sağlanan tüm URL'lerinizi kontrol edin.
Tüm URL'lerin, uzantın uzak makinede erişebileceği geçerli konumlara çözüm verdiğinden emin olun.

### <a name="invalid-registrationkey-type"></a>Geçersiz KayıtAnahtar türü

"Parametre Kaydı Için geçersiz türüPSCredential türüKey."

**Sorun**: protectedSettings.configurationArguments'taki *RegistrationKey* değeri PSCredential dışında herhangi bir tür olarak sağlanamaz.

**Çözüm :** ProtectedSettings.configurationRegistrationKey için aşağıdaki biçimi kullanarak PSCredential türüne giriş yapın:

```json
"configurationArguments": {
    "RegistrationKey": {
        "userName": "NOT_USED",
        "Password": "RegistrationKey"
    }
}
```

### <a name="invalid-configurationargument-type"></a>Geçersiz ConfigurationArgument türü

"Geçersiz yapılandırmaBağımsız {0}Değişkenler türü "

**Sorun**: *ConfigurationArguments* özelliği **Karma tablo** nesnesine çözümolamaz.

**Çözüm**: ConfigurationArguments özelliğinizi **karma tablo**haline *getirin.*
Önceki örneklerde sağlanan biçimi izleyin. Tırnak işaretlerine, virgüllere ve parantezlere dikkat edin.

### <a name="duplicate-configurationarguments"></a>Yinelenen Yapılandırma Bağımsız Değişkenleri

"Hem genel{0}hem de korumalı yapılandırma Bağımsız Değişkenlerinde yinelenen bağımsız değişkenler ' ' bulundu"

**Sorun**: Ortak ayarlardaki *ConfigurationArguments* ve korumalı ayarlardaki *ConfigurationArguments* aynı ada sahip özelliklere sahiptir.

**Çözüm**: Yinelenen özelliklerden birini kaldırın.

### <a name="missing-properties"></a>Eksik özellikler

"ayarlar. configuration.function ayarlar.configuration.url veya settings.configuration.module belirtilmiş gerektirir"

"ayarlar. configuration.url ayarlar.configuration.script belirtilmesi gerektiğini gerektirir"

"ayarlar. Configuration.script ayarlar.configuration.url belirtilmesi gerektiğini gerektirir"

"ayarlar. configuration.url ayarlar.configuration.function belirtilmesi ni gerektirir"

"protectedSettings.ConfigurationUrlSasToken ayarlar.configuration.url belirtilmiş gerektirir"

"protectedSettings.ConfigurationDataUrlSasToken ayarlar.configurationData.url belirtilmelidir"

**Sorun**: Tanımlanmış bir özelliğin eksik olan başka bir özelliğe ihtiyacı vardır.

**Çözümler**:

- Kayıp mülkü sağlayın.
- Eksik özelliğin ihtiyacı olan özelliği kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [DSC uzantısı ile sanal makine ölçek kümelerini kullanma](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)hakkında bilgi edinin.
- [DSC'nin güvenli kimlik bilgileri yönetimi](dsc-credentials.md)hakkında daha fazla bilgi edinin.
- Azure [DSC uzantı işleyicisine giriş](dsc-overview.md)alın.
- PowerShell DSC hakkında daha fazla bilgi için [PowerShell dokümantasyon merkezine](/powershell/scripting/dsc/overview/overview)gidin.
