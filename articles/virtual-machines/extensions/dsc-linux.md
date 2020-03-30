---
title: Linux için Azure DSC uzantısı
description: Bir Azure Linux VM'nin İstenilen Durum Yapılandırması kullanılarak yapılandırılabilmesi için OMI ve DSC paketlerini yükler.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250627"
---
# <a name="dsc-extension-for-linux-microsoftostcextensionsdscforlinux"></a>Linux için DSC uzantısı (Microsoft.OSTCExtensions.DSCForLinux)

İstenilen Durum Yapılandırması (DSC), BT ve geliştirme altyapınızı kod olarak yapılandırma ile yönetmek için kullanabileceğiniz bir yönetim platformudur.

> [!NOTE]
> Linux için DSC uzantısı ve [Linux için Azure Monitor sanal makine uzantısı](/azure/virtual-machines/extensions/oms-linux) şu anda bir çakışma sunar ve yan yana yapılandırmada desteklenmez. İki çözümü aynı VM'de birlikte kullanmayın.

DSCForLinux uzantısı Microsoft tarafından yayınlanır ve desteklenir. Uzantı, OMI ve DSC aracısını Azure sanal makinelerine yükler. DSC uzantısı da aşağıdaki eylemleri yapabilirsiniz:


- Azure Otomasyon hizmetinden (Register ExtensionAction) yapılandırmaları çekmek için Linux VM'yi Azure Otomasyonu hesabına kaydedin.
- MOF yapılandırmalarını Linux VM'ye (Push ExtensionAction) itin.
- Düğüm yapılandırmasını çekmek için bir çekme sunucusunu yapılandırmak için Linux VM'ye meta MOF yapılandırması uygulayın (Çekme UzantısıEylem).
- Linux VM'ye özel DSC modülleri yükleyin (Install ExtensionAction).
- Özel DSC modüllerini Linux VM'den kaldırın (Uzantısı KaldırEylemi).

 

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

DSC Linux uzantısı, [aşağıdakiler dışında Azure'da onaylanan](/azure/virtual-machines/linux/endorsed-distros) tüm Linux dağıtımlarını destekler:

| Dağıtım | Sürüm |
|---|---|
| Debian | Tüm sürümler |
| Ubuntu| 18.04 |
 
### <a name="internet-connectivity"></a>İnternet bağlantısı

DSCForLinux uzantısı hedef sanal makinenin internete bağlanmasını gerektirir. Örneğin, Kayıt uzantısı Otomasyon hizmetine bağlantı gerektirir. Çekme, Çekme, Yükleme gibi diğer işlemler için Azure Depolama ve GitHub'a bağlantı gerekir. Müşteri tarafından sağlanan ayarlara bağlıdır.

## <a name="extension-schema"></a>Uzantı şeması

### <a name="public-configuration"></a>Genel yapılandırma

Desteklenen tüm genel yapılandırma parametreleri şunlardır:

* `FileUri`: (isteğe bağlı, string) MOF dosyasının uri, meta MOF dosyası veya özel kaynak zip dosyası.
* `ResourceName`: (isteğe bağlı, string) Özel kaynak modülünün adı.
* `ExtensionAction`: (isteğe bağlı, dize) Bir uzantınne ne yaptığını belirtir. Geçerli değerler Kaydol, Itme, Çekme, Yükleme ve Kaldır'dır. Belirtilmemişse, varsayılan olarak Push Action olarak kabul edilir.
* `NodeConfigurationName`: (isteğe bağlı, dize) Uygulanacak düğüm yapılandırmasının adı.
* `RefreshFrequencyMins`: (isteğe bağlı, int) DSC'nin yapılandırmayı çekme sunucusundan ne sıklıkta (dakika içinde) elde etmeye çalıştığınızı belirtir. 
       Çekme sunucusundaki yapılandırma hedef düğümdeki geçerli olandan farklıysa, bekleyen depoya kopyalanır ve uygulanır.
* `ConfigurationMode`: (isteğe bağlı, dize) DSC'nin yapılandırmayı nasıl uygulaması gerektiğini belirtir. Geçerli değerler ApplyOnly, ApplyAndMonitor ve ApplyAndAutoCorrect'tir.
* `ConfigurationModeFrequencyMins`: (isteğe bağlı, int) DSC'nin yapılandırmanın istenilen durumda olmasını ne sıklıkta (dakika içinde) sağlar.

> [!NOTE]
> 2.3'ten önce bir sürüm kullanıyorsanız, mod parametresi ExtensionAction ile aynıdır. Mod aşırı yüklü bir terim gibi görünüyor. Karışıklığı önlemek için, ExtensionAction sürüm 2.3'ten itibaren kullanılır. Geriye dönük uyumluluk için uzantı hem modu hem de ExtensionAction'ı destekler. 
>

### <a name="protected-configuration"></a>Korumalı yapılandırma

Desteklenen tüm korumalı yapılandırma parametreleri şunlardır:

* `StorageAccountName`: (isteğe bağlı, string) Dosyayı içeren depolama hesabının adı
* `StorageAccountKey`: (isteğe bağlı, dize) Dosyayı içeren depolama hesabının anahtarı
* `RegistrationUrl`: (isteğe bağlı, string) Azure Otomasyon hesabının URL'si
* `RegistrationKey`: (isteğe bağlı, string) Azure Otomasyon hesabının erişim anahtarı


## <a name="scenarios"></a>Senaryolar

### <a name="register-an-azure-automation-account"></a>Azure Otomasyon hesabı kaydetme
protected.json
```json
{
  "RegistrationUrl": "<azure-automation-account-url>",
  "RegistrationKey": "<azure-automation-account-key>"
}
```
public.json
```json
{
  "ExtensionAction" : "Register",
  "NodeConfigurationName" : "<node-configuration-name>",
  "RefreshFrequencyMins" : "<value>",
  "ConfigurationMode" : "<ApplyAndMonitor | ApplyAndAutoCorrect | ApplyOnly>",
  "ConfigurationModeFrequencyMins" : "<value>"
}
```

PowerShell formatı
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

### <a name="apply-an-mof-configuration-file-in-an-azure-storage-account-to-the-vm"></a>VM'ye bir MOF yapılandırma dosyası (Azure depolama hesabında) uygulama

protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```

public.json
```json
{
  "FileUri": "<mof-file-uri>",
  "ExtensionAction": "Push"
}
```

PowerShell formatı
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


### <a name="apply-an-mof-configuration-file-in-public-storage-to-the-vm"></a>VM'ye bir MOF yapılandırma dosyası (ortak depolamada) uygulama

public.json
```json
{
  "FileUri": "<mof-file-uri>"
}
```

PowerShell formatı
```powershell
$publicConfig = '{
  "FileUri": "<mof-file-uri>"
}'
```

### <a name="apply-a-meta-mof-configuration-file-in-an-azure-storage-account-to-the-vm"></a>VM'ye bir meta MOF yapılandırma dosyası (Azure depolama hesabında) uygulama

protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```

public.json
```json
{
  "ExtensionAction": "Pull",
  "FileUri": "<meta-mof-file-uri>"
}
```

PowerShell formatı
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

### <a name="apply-a-meta-mof-configuration-file-in-public-storage-to-the-vm"></a>VM'ye bir meta MOF yapılandırma dosyası (ortak depolamada) uygulama
public.json
```json
{
  "FileUri": "<meta-mof-file-uri>",
  "ExtensionAction": "Pull"
}
```
PowerShell formatı
```powershell
$publicConfig = '{
  "FileUri": "<meta-mof-file-uri>",
  "ExtensionAction": "Pull"
}'
```

### <a name="install-a-custom-resource-module-a-zip-file-in-an-azure-storage-account-to-the-vm"></a>VM'ye özel kaynak modülü (Azure depolama hesabındaki zip dosyası) yükleme
protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```
public.json
```json
{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}
```

PowerShell formatı
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

### <a name="install-a-custom-resource-module-a-zip-file-in-public-storage-to-the-vm"></a>VM'ye özel kaynak modülü (genel depolamada bir zip dosyası) yükleme
public.json
```json
{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}
```
PowerShell formatı
```powershell
$publicConfig = '{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}'
```

### <a name="remove-a-custom-resource-module-from-the-vm"></a>VM'den özel kaynak modüllerini kaldırma
public.json
```json
{
  "ResourceName": "<resource-name>",
  "ExtensionAction": "Remove"
}
```
PowerShell formatı
```powershell
$publicConfig = '{
  "ResourceName": "<resource-name>",
  "ExtensionAction": "Remove"
}'
```

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Şablonlar, Azure Otomasyonu'na binme gibi dağıtım sonrası yapılandırma gerektiren bir veya daha fazla sanal makine dağıttığınızda idealdir. 

Örnek Kaynak Yöneticisi şablonu [201-dsc-linux-azure-depolama-on-ubuntu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-dsc-linux-azure-storage-on-ubuntu) ve [201-dsc-linux-public-storage-on-ubuntu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-dsc-linux-public-storage-on-ubuntu)olduğunu.

Azure Kaynak Yöneticisi şablonu hakkında daha fazla bilgi için [bkz.](../../azure-resource-manager/templates/template-syntax.md)


## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı

### <a name="use-azure-cliazure-cli"></a>[Azure CLI][azure-cli] kullanın
DSCForLinux uzantısını dağıtmadan önce, `public.json` `protected.json` bölüm 3'teki farklı senaryolara göre yapılandırın.

#### <a name="classic"></a>Klasik

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Klasik dağıtım modu, Azure Hizmet Yönetimi modu olarak da adlandırılır. Çalıştırarak geçiş yapabilirsiniz:
```
$ azure config mode asm
```

Aşağıdakileri çalıştırarak DSCForLinux uzantısını dağıtabilirsiniz:
```
$ azure vm extension set <vm-name> DSCForLinux Microsoft.OSTCExtensions <version> \
--private-config-path protected.json --public-config-path public.json
```

Kullanılabilir en son uzantı sürümünü öğrenmek için çalıştırın:
```
$ azure vm extension list
```

#### <a name="resource-manager"></a>Resource Manager
Çalıştırarak Azure Kaynak Yöneticisi moduna geçebilirsiniz:
```
$ azure config mode arm
```

Aşağıdakileri çalıştırarak DSCForLinux uzantısını dağıtabilirsiniz:
```
$ azure vm extension set <resource-group> <vm-name> \
DSCForLinux Microsoft.OSTCExtensions <version> \
--private-config-path protected.json --public-config-path public.json
```
> [!NOTE]
> Azure Kaynak Yöneticisi `azure vm extension list` modunda, şimdilik kullanılamıyor.
>

### <a name="use-azure-powershellazure-powershell"></a>[Azure PowerShell][azure-powershell] kullanın

#### <a name="classic"></a>Klasik

Şunları çalıştırarak Azure hesabınızda Azure Hizmet Yönetimi modunda oturum açabilirsiniz:

```powershell>
Add-AzureAccount
```

Ve çalıştırarak DSCForLinux uzantısı dağıtmak:

```powershell>
$vmname = '<vm-name>'
$vm = Get-AzureVM -ServiceName $vmname -Name $vmname
$extensionName = 'DSCForLinux'
$publisher = 'Microsoft.OSTCExtensions'
$version = '< version>'
```

Önceki bölümdeki farklı senaryolara göre $privateConfig ve $publicConfig içeriğini değiştirin.
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

Şunları çalıştırarak Azure hesabınızda Azure Kaynak Yöneticisi modunda oturum açabilirsiniz:

```powershell>
Login-AzAccount
```

Azure Kaynak Yöneticisi ile Azure PowerShell'i nasıl kullanacağınız hakkında daha fazla bilgi edinmek için [Azure PowerShell'i kullanarak Azure kaynaklarını yönet'e](../../azure-resource-manager/management/manage-resources-powershell.md)bakın.

Aşağıdakileri çalıştırarak DSCForLinux uzantısını dağıtabilirsiniz:

```powershell>
$rgName = '<resource-group-name>'
$vmName = '<vm-name>'
$location = '< location>'
$extensionName = 'DSCForLinux'
$publisher = 'Microsoft.OSTCExtensions'
$version = '< version>'
```

Önceki bölümdeki farklı senaryolara göre $privateConfig ve $publicConfig içeriğini değiştirin.
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

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıktısı aşağıdaki dosyaya kaydedilir:

```
/var/log/azure/<extension-name>/<version>/extension.log file.
```

Hata kodu: 51 desteklenmeyen dağıtımı veya desteklenmeyen uzantı eylemini temsil eder.
Bazı durumlarda, OMI daha yüksek bir sürümü zaten makinede var olduğunda DSC Linux uzantısı OMI yüklemek için başarısız olur. [hata yanıtı: (000003)Düşürmeye izin verilmiyor]



### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/community/)Azure uzmanlarına başvurun. Alternatif olarak, bir Azure Destek olayı dosyalayabilirsiniz. [Azure Destek sitesine](https://azure.microsoft.com/support/options/)gidin ve destek **al'ı**seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure Destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.

## <a name="next-steps"></a>Sonraki adımlar
Uzantılar hakkında daha fazla bilgi [için, Linux için Sanal makine uzantıları ve özellikleri](features-linux.md)ne bakın.
