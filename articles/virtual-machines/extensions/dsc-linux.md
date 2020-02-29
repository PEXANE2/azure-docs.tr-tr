---
title: Linux için Azure DSC Uzantısı
description: Azure Linux VM 'sinin Istenen durum yapılandırması kullanılarak yapılandırılmasını sağlamak için OMı ve DSC paketlerini yükleyerek.
services: virtual-machines-linux
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: robreed
ms.openlocfilehash: 2f04b5ecb2019a77bbb38e97c3869cc0a9447955
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77921530"
---
# <a name="dsc-extension-for-linux-microsoftostcextensionsdscforlinux"></a>Linux için DSC Uzantısı (Microsoft. OSTCExtensions. DSCForLinux)

İstenen Durum Yapılandırması (DSC), BT ve geliştirme altyapınızı kod olarak yapılandırmayla yönetmek için kullanabileceğiniz bir yönetim platformudur.

> [!NOTE]
> Linux için DSC Uzantısı ve [Linux Için Azure izleyici sanal makine uzantısı](/azure/virtual-machines/extensions/oms-linux) Şu anda bir çakışma sunmakta ve yan yana yapılandırmada desteklenmemektedir. İki çözümü aynı VM 'de birlikte kullanmayın.

DSCForLinux uzantısı Microsoft tarafından yayımlanır ve desteklenir. Uzantı, Azure sanal makinelerine OMı ve DSC aracısını yükleme. DSC Uzantısı aşağıdaki eylemleri de gerçekleştirebilir:


- Azure Otomasyonu hizmetinden (ExtensionAction 'ı Kaydet) yapılandırma çekmek için Linux VM 'yi bir Azure Otomasyonu hesabına kaydettirin.
- MOF yapılandırmasını Linux VM 'ye gönderin (ExtensionAction öğesini Gönder).
- Düğüm yapılandırmasını (çekme ExtensionAction) çekmek için bir çekme sunucusu yapılandırmak üzere Linux sanal makinesine meta MOF yapılandırması uygulayın.
- Linux VM 'ye özel DSC modülleri (Install ExtensionAction) yükler.
- Linux VM 'den özel DSC modüllerini kaldırın (ExtensionAction 'ı kaldırın).

 

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

DSC Linux uzantısı, aşağıdakiler dışında [Azure 'da onaylı tüm Linux dağıtımlarını](/azure/virtual-machines/linux/endorsed-distros) destekler:

| Dağıtım | Sürüm |
|---|---|
| Debian | Tüm sürümler |
| Ubuntu| 18,04 |
 
### <a name="internet-connectivity"></a>İnternet bağlantısı

DSCForLinux uzantısı, hedef sanal makinenin internet 'e bağlanmasını gerektirir. Örneğin, YAZMAÇ uzantısının Automation hizmetine bağlantısı olması gerekir. Çekme, çekme, çekme gibi diğer eylemler için Azure depolama ve GitHub bağlantısı gerekir. Bu, müşteri tarafından sunulan ayarlara bağlıdır.

## <a name="extension-schema"></a>Uzantı şeması

### <a name="public-configuration"></a>Ortak yapılandırma

Desteklenen tüm ortak yapılandırma parametreleri şunlardır:

* `FileUri`: (isteğe bağlı, dize) MOF dosyasının URI 'si, meta MOF dosyası veya özel kaynak ZIP dosyası.
* `ResourceName`: (isteğe bağlı, dize) özel kaynak modülünün adı.
* `ExtensionAction`: (isteğe bağlı, dize) bir uzantının ne yaptığını belirtir. Geçerli değerler kayıt, gönderim, çekme, yükleme ve kaldırma. Belirtilmemişse, varsayılan olarak bir gönderme eylemi olarak kabul edilir.
* `NodeConfigurationName`: (isteğe bağlı, dize) uygulanacak düğüm yapılandırmasının adı.
* `RefreshFrequencyMins`: (isteğe bağlı, int) DSC 'nin çekme sunucusundan yapılandırmayı ne sıklıkta (dakika cinsinden) alacağını belirtir. 
       Çekme sunucusundaki yapılandırma hedef düğümdeki geçerli olandan farklıysa, bu, bekleyen depoya kopyalanıp uygulanır.
* `ConfigurationMode`: (isteğe bağlı, dize) DSC 'nin yapılandırmayı nasıl uygulanacağını belirtir. Geçerli değerler yalnızca Apply, ApplyAndMonitor ve Applyandadutocorrect.
* `ConfigurationModeFrequencyMins`: (isteğe bağlı, int), ne sıklıkta (dakika cinsinden) DSC 'nin yapılandırmanın istenen durumda olmasını sağlar.

> [!NOTE]
> 2,3 'den önceki bir sürümü kullanıyorsanız, mode parametresi ExtensionAction ile aynı olur. Mod aşırı yüklenmiş bir terim gibi görünüyor. Karışıklığın önüne geçmek için, 2,3. sürümden ExtensionAction kullanılır. Uzantı, geriye dönük uyumluluk için hem modu hem de ExtensionAction 'ı destekler. 
>

### <a name="protected-configuration"></a>Korumalı yapılandırma

Desteklenen tüm korumalı yapılandırma parametreleri aşağıda verilmiştir:

* `StorageAccountName`: (isteğe bağlı, dize) dosyayı içeren depolama hesabının adı
* `StorageAccountKey`: (isteğe bağlı, dize) dosyayı içeren depolama hesabının anahtarı
* `RegistrationUrl`: (isteğe bağlı, dize) Azure Otomasyonu hesabının URL 'SI
* `RegistrationKey`: (isteğe bağlı, dize) Azure Otomasyonu hesabının erişim anahtarı


## <a name="scenarios"></a>Senaryolar

### <a name="register-an-azure-automation-account"></a>Azure Otomasyonu hesabı kaydetme
Protected. JSON
```json
{
  "RegistrationUrl": "<azure-automation-account-url>",
  "RegistrationKey": "<azure-automation-account-key>"
}
```
public. JSON
```json
{
  "ExtensionAction" : "Register",
  "NodeConfigurationName" : "<node-configuration-name>",
  "RefreshFrequencyMins" : "<value>",
  "ConfigurationMode" : "<ApplyAndMonitor | ApplyAndAutoCorrect | ApplyOnly>",
  "ConfigurationModeFrequencyMins" : "<value>"
}
```

PowerShell biçimi
```powershell
$privateConfig = '{
  "RegistrationUrl": "<azure-automation-account-url>",
  "RegistrationKey": "<azure-automation-account-key>"
}'

$publicConfig = '{
  "ExtensionAction" : "Register",
  "NodeConfigurationName": "<node-configuration-name>",
  "RefreshFrequencyMins": "<value>",
  "ConfigurationMode": "<ApplyAndMonitor | ApplyAndAutoCorrect | ApplyOnly>",
  "ConfigurationModeFrequencyMins": "<value>"
}'
```

### <a name="apply-an-mof-configuration-file-in-an-azure-storage-account-to-the-vm"></a>SANAL makineye bir MOF yapılandırma dosyası (Azure Storage hesabında) uygulama

Protected. JSON
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```

public. JSON
```json
{
  "FileUri": "<mof-file-uri>",
  "ExtensionAction": "Push"
}
```

PowerShell biçimi
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "FileUri": "<mof-file-uri>",
  "ExtensionAction": "Push"
}'
```


### <a name="apply-an-mof-configuration-file-in-public-storage-to-the-vm"></a>SANAL makineye bir MOF yapılandırma dosyası (ortak depolamada) uygulama

public. JSON
```json
{
  "FileUri": "<mof-file-uri>"
}
```

PowerShell biçimi
```powershell
$publicConfig = '{
  "FileUri": "<mof-file-uri>"
}'
```

### <a name="apply-a-meta-mof-configuration-file-in-an-azure-storage-account-to-the-vm"></a>VM 'ye bir meta MOF yapılandırma dosyası (bir Azure depolama hesabında) uygulama

Protected. JSON
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```

public. JSON
```json
{
  "ExtensionAction": "Pull",
  "FileUri": "<meta-mof-file-uri>"
}
```

PowerShell biçimi
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "ExtensionAction": "Pull",
  "FileUri": "<meta-mof-file-uri>"
}'
```

### <a name="apply-a-meta-mof-configuration-file-in-public-storage-to-the-vm"></a>VM 'ye bir meta MOF yapılandırma dosyası (genel depolamada) uygulama
public. JSON
```json
{
  "FileUri": "<meta-mof-file-uri>",
  "ExtensionAction": "Pull"
}
```
PowerShell biçimi
```powershell
$publicConfig = '{
  "FileUri": "<meta-mof-file-uri>",
  "ExtensionAction": "Pull"
}'
```

### <a name="install-a-custom-resource-module-a-zip-file-in-an-azure-storage-account-to-the-vm"></a>VM 'ye özel bir kaynak modülü (bir Azure depolama hesabına ZIP dosyası) yüklemek
Protected. JSON
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```
public. JSON
```json
{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}
```

PowerShell biçimi
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}'
```

### <a name="install-a-custom-resource-module-a-zip-file-in-public-storage-to-the-vm"></a>VM 'ye özel bir kaynak modülü (ortak depolamaya bir ZIP dosyası) yüklemek
public. JSON
```json
{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}
```
PowerShell biçimi
```powershell
$publicConfig = '{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}'
```

### <a name="remove-a-custom-resource-module-from-the-vm"></a>Özel bir kaynak modülünü VM 'den kaldırma
public. JSON
```json
{
  "ResourceName": "<resource-name>",
  "ExtensionAction": "Remove"
}
```
PowerShell biçimi
```powershell
$publicConfig = '{
  "ResourceName": "<resource-name>",
  "ExtensionAction": "Remove"
}'
```

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Azure Otomasyonu 'na ekleme gibi dağıtım sonrası yapılandırma gerektiren bir veya daha fazla sanal makine dağıttığınızda şablonlar idealdir. 

Örnek Kaynak Yöneticisi şablonu [201-DSC-Linux-Azure-Storage-on-Ubuntu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-dsc-linux-azure-storage-on-ubuntu) ve [201-DSC-Linux-genel-Storage-on-Ubuntu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-dsc-linux-public-storage-on-ubuntu)olur.

Azure Resource Manager şablonu hakkında daha fazla bilgi için bkz. [yazma Azure Resource Manager şablonları](../../azure-resource-manager/templates/template-syntax.md).


## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

### <a name="use-azure-cliazure-cli"></a>[Azure CLı] [Azure-CLI] kullanın
DSCForLinux uzantısını dağıtmadan önce, Bölüm 3 ' teki farklı senaryolara göre `public.json` ve `protected.json` yapılandırın.

#### <a name="classic"></a>Klasik

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Klasik dağıtım moduna Azure hizmet yönetimi modu da denir. Şunu çalıştırarak geçiş yapabilirsiniz:
```
$ azure config mode asm
```

Şu işlemi çalıştırarak DSCForLinux uzantısını dağıtabilirsiniz:
```
$ azure vm extension set <vm-name> DSCForLinux Microsoft.OSTCExtensions <version> \
--private-config-path protected.json --public-config-path public.json
```

Kullanılabilir en son uzantı sürümünü öğrenmek için şunu çalıştırın:
```
$ azure vm extension list
```

#### <a name="resource-manager"></a>Resource Manager
Şunu çalıştırarak Azure Resource Manager moduna geçebilirsiniz:
```
$ azure config mode arm
```

Şu işlemi çalıştırarak DSCForLinux uzantısını dağıtabilirsiniz:
```
$ azure vm extension set <resource-group> <vm-name> \
DSCForLinux Microsoft.OSTCExtensions <version> \
--private-config-path protected.json --public-config-path public.json
```
> [!NOTE]
> Azure Resource Manager modunda, `azure vm extension list` şimdilik kullanılamaz.
>

### <a name="use-azure-powershellazure-powershell"></a>[Azure PowerShell] [Azure-PowerShell] kullanın

#### <a name="classic"></a>Klasik

Aşağıdakileri çalıştırarak Azure hizmet yönetimi modunda Azure hesabınızda oturum açabilirsiniz:

```powershell>
Add-AzureAccount
```

Ve şunu çalıştırarak DSCForLinux uzantısını dağıtın:

```powershell>
$vmname = '<vm-name>'
$vm = Get-AzureVM -ServiceName $vmname -Name $vmname
$extensionName = 'DSCForLinux'
$publisher = 'Microsoft.OSTCExtensions'
$version = '< version>'
```

$PrivateConfig ve $publicConfig içeriğini önceki bölümdeki farklı senaryolara göre değiştirin.
```
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'
```

```
$publicConfig = '{
  "ExtensionAction": "Push",
  "FileUri": "<mof-file-uri>"
}'
```

```
Set-AzureVMExtension -ExtensionName $extensionName -VM $vm -Publisher $publisher `
  -Version $version -PrivateConfiguration $privateConfig `
  -PublicConfiguration $publicConfig | Update-AzureVM
```

#### <a name="resource-manager"></a>Resource Manager

Aşağıdakileri çalıştırarak Azure hesabınızda Azure Resource Manager modunda oturum açabilirsiniz:

```powershell>
Login-AzAccount
```

Azure PowerShell Azure Resource Manager ile birlikte kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure PowerShell kullanarak Azure kaynaklarını yönetme](../../azure-resource-manager/management/manage-resources-powershell.md).

Şu işlemi çalıştırarak DSCForLinux uzantısını dağıtabilirsiniz:

```powershell>
$rgName = '<resource-group-name>'
$vmName = '<vm-name>'
$location = '< location>'
$extensionName = 'DSCForLinux'
$publisher = 'Microsoft.OSTCExtensions'
$version = '< version>'
```

$PrivateConfig ve $publicConfig içeriğini önceki bölümdeki farklı senaryolara göre değiştirin.
```
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'
```

```
$publicConfig = '{
  "ExtensionAction": "Push",
  "FileUri": "<mof-file-uri>"
}'
```

```
Set-AzVMExtension -ResourceGroupName $rgName -VMName $vmName -Location $location `
  -Name $extensionName -Publisher $publisher -ExtensionType $extensionName `
  -TypeHandlerVersion $version -SettingString $publicConfig -ProtectedSettingString $privateConfig
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtımlarının durumuyla ilgili veriler Azure portal ve Azure CLı kullanılarak alınabilir. Belirli bir VM için uzantıların dağıtım durumunu görmek için, Azure CLı kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıkış aşağıdaki dosyasına kaydedilir:

```
/var/log/azure/<extension-name>/<version>/extension.log file.
```

Hata kodu: 51, desteklenmeyen dağıtım veya desteklenmeyen uzantı eylemini temsil eder.
Bazı durumlarda, daha yüksek bir OMı sürümü makinede zaten mevcutsa DSC Linux uzantısı OMı 'yi yükleyemez. [hata yanıtı: (000003) Indirgeme izin verilmiyor]



### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/community/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/)gidin ve **Destek Al**' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.

## <a name="next-steps"></a>Sonraki adımlar
Uzantılar hakkında daha fazla bilgi için bkz. [Linux Için sanal makine uzantıları ve özellikleri](features-linux.md).
