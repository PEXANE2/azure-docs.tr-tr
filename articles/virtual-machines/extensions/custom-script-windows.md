---
title: Windows için Azure Özel Komut Dosyası Uzantısı
description: Özel Komut Dosyası uzantısını kullanarak Windows VM yapılandırma görevlerini otomatikleştirin
services: virtual-machines-windows
manager: carmonm
author: bobbytreed
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/02/2019
ms.author: robreed
ms.openlocfilehash: 2c7cad2dfdcd55073a1cf09d79e5223b666ced5f
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478157"
---
# <a name="custom-script-extension-for-windows"></a>Windows için Özel Betik Uzantısı

Özel Komut Dosyası Uzantısı, Azure sanal makinelerinde komut dosyalarını indirir ve yürütür. Bu uzantı, dağıtım sonrası yapılandırma, yazılım yüklemesi veya diğer yapılandırma veya yönetim görevleri için yararlıdır. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Özel Komut Dosyası Uzantısı Azure Kaynak Yöneticisi şablonlarıyla tümleşir ve Azure CLI, PowerShell, Azure portalı veya Azure Sanal Makine REST API'si kullanılarak çalıştırılabilir.

Bu belge, Azure PowerShell modülasyonlarını kullanarak Özel Komut Dosyası Uzantısı'nın nasıl kullanılacağını, Azure Kaynak Yöneticisi şablonlarını ve Windows sistemlerindeki sorun giderme adımlarını ayrıntılarıyla anlatır.

## <a name="prerequisites"></a>Ön koşullar

> [!NOTE]  
> Update-AzVM'yi parametresi ile aynı VM ile çalıştırmak için Özel Komut Dosyası Uzantısı'nı kullanmayın, çünkü kendisini bekleyecektir.  

### <a name="operating-system"></a>İşletim Sistemi

Windows için Özel Komut Dosyası Uzantısı, uzantı destekli uzantılı işletim sisteminde çalışacaktır, daha fazla bilgi için bu [Azure Uzantısı destekli işletim sistemlerine](https://support.microsoft.com/help/4078134/azure-extension-supported-operating-systems)bakın.

### <a name="script-location"></a>Komut Dosyası Konumu

Uzantıyı Azure Blob depolama alanına erişmek için Azure Blob depolama kimlik bilgilerinizi kullanacak şekilde yapılandırabilirsiniz. Komut dosyası konumu, VM'nin GitHub veya dahili bir dosya sunucusu gibi bu bitiş noktasına yönlendirebildiği sürece her yerde olabilir.

### <a name="internet-connectivity"></a>İnternet Bağlantısı

GitHub veya Azure Depolama gibi harici bir komut dosyası indirmeniz gerekiyorsa, ek güvenlik duvarı ve Ağ Güvenlik Grubu bağlantı noktalarının açılması gerekir. Örneğin, komut dosyanız Azure Depolama'da bulunuyorsa, [Depolama](../../virtual-network/security-overview.md#service-tags)için Azure NSG Hizmet Etiketleri'ni kullanarak erişime izin verebilirsiniz.

Komut dosyanız yerel bir sunucudaysa, yine de ek güvenlik duvarı ve Ağ Güvenlik Grubu bağlantı noktalarının açılması gerekebilir.

### <a name="tips-and-tricks"></a>İpuçları ve Püf Noktalar

* Bu uzantı için en yüksek hata oranı komut dosyasısözdizimi hataları nedeniyle, komut dosyası hatasız çalışır test ve aynı zamanda başarısız nerede bulmak için daha kolay hale getirmek için komut dosyasına ek günlüğe kaydetme.
* Idempotent komut dosyaları yazın. Bu, yanlışlıkla yeniden çalıştırılırsa sistem değişikliklerine neden olmamasını sağlar.
* Betiklerin çalıştırma sırasında kullanıcı girişi gerektirmediğinden emin olun.
* Betiğin çalışması için 90 dakikalık bir süre ayrılmıştır. Betiklerin daha uzun sürmesi durumunda uzantı sağlama işlemi başarısız olur.
* Betik içine yeniden başlatma eylemi eklemeyin. Aksi halde yüklenmekte olan diğer uzantılarla ilgili sorun yaşanabilir. Yeniden başlatma gerçekleştirildiğinde önyükleme sonrasında uzantı, kaldığı yerden devam etmeyecektir.
* Yeniden başlatmaya, ardından uygulamaları yükleyip komut dosyalarını çalıştıracak bir komut dosyanız varsa, yeniden başlatmayı Windows Zamanlanmış Görev'i kullanarak zamanlayabilir veya DSC, Chef veya Puppet uzantıları gibi araçları kullanabilirsiniz.
* Uzantı, betiği yalnızca bir kez çalıştıracaktır. Betiğin her önyükleme sırasında çalıştırılmasını istiyorsanız uzantıyı kullanarak bir Windows Zamanlanmış Görevi oluşturmanız gerekir.
* Betiğin çalıştırılacağı zamanı belirlemek istiyorsanız uzantıyı kullanarak bir Windows Zamanlanmış Görevi oluşturmanız gerekir.
* Betik çalışırken Azure portalı veya CLI üzerinden uzantı durumunu yalnızca "geçiş durumunda" şeklinde görürsünüz. Çalışan bir betikle ilgili daha sık durum güncelleştirmesi almak isterseniz kendi çözümünüzü oluşturmanız gerekir.
* Özel Komut Dosyası uzantısı proxy sunucularını doğal olarak desteklemez, ancak komut dosyanızdaki *Curl* gibi proxy sunucularını destekleyen bir dosya aktarım aracı kullanabilirsiniz
* Betiğinizin veya komutlarınızın kullandığı varsayılan olmayan dizin konumlarına dikkat edin ve bu durumu işlemek için bir mantık oluşturun.
* Özel Komut Dosyası Uzantısı LocalSystem Hesabı altında çalışacaktır

## <a name="extension-schema"></a>Uzantı şeması

Özel Komut Dosyası Uzantısı yapılandırması komut dosyası konumu ve çalıştırılacak komut gibi şeyleri belirtir. Bu yapılandırmayı yapılandırma dosyalarında depolayabilir, komut satırında belirtebilir veya Azure Kaynak Yöneticisi şablonunda belirtebilirsiniz.

Hassas verileri, şifrelenmiş ve yalnızca sanal makinenin içinde şifresi çözülmüş korumalı bir yapılandırmada depolayabilirsiniz. Korumalı yapılandırma, yürütme komutu parola gibi sırlar içeriyorsa yararlıdır.

Bu öğeler hassas veri olarak ele alınmalıdır ve uzantılar korumalı ayar yapılandırmasında belirtilmelidir. Azure VM uzantı korumalı ayar verileri şifrelenir ve yalnızca hedef sanal makinede şifresi çözülür.

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "virtualMachineName/config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.10",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ],
            "timestamp":123456789
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey",
            "managedIdentity" : {}
        }
    }
}
```

> [!NOTE]
> yönetilenIdentity özelliği storageAccountName veya storageAccountKey özellikleri ile birlikte **kullanılmamalıdır**

> [!NOTE]
> Aynı VM için aynı Kaynak Yöneticisi şablonunda iki kez özel komut dosyası belirterek, bir uzantının yalnızca bir sürümü bir VM'ye zamanında yüklenebilir.

> [!NOTE]
> Bu şemı VirtualMachine kaynağının içinde veya bağımsız bir kaynak olarak kullanabiliriz. Bu uzantı ARM şablonunda bağımsız bir kaynak olarak kullanılıyorsa, kaynağın adı bu biçimde "virtualMachineName/extensionName" olmalıdır.

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / Örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| yayımcı | Microsoft.Compute | string |
| type | CustomScriptExtension | string |
| typeHandlerVersion | 1.10 | int |
| fileUris (örn. ) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 | array |
| zaman damgası (örn. | 123456789 | 32 bit'lik bir sayı |
| commandToExecute (örn. ) | powershell -ExecutionPolicy Sınırsız -Dosya yapılandırma-müzik-app.ps1 | string |
| storageAccountName (örn.) | örnek storageacct | string |
| storageAccountKey (örn.) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | string |
| yönetilen Kimlik (örn. ) | { } veya { "clientId": "31b403aa-c364-4240-a7ff-d85fb6cd7232" } veya { "objectId": "12dd289c-0583-46e5-b9b4-115d5c19ef4b" } | json nesne |

>[!NOTE]
>Bu özellik adları büyük/küçük harf duyarlıdır. Dağıtım sorunlarını önlemek için, burada gösterildiği gibi adları kullanın.

#### <a name="property-value-details"></a>Özellik değeri ayrıntıları

* `commandToExecute`: (**gerekli**, dize) yürütmek için giriş noktası komut dosyası. Komutunuz parolalar gibi sırlar içeriyorsa veya fileUris'iniz hassassa bunun yerine bu alanı kullanın.
* `fileUris`: (isteğe bağlı, dize dizisi) dosya(lar) için URL'ler indirilecek.
* `timestamp`(isteğe bağlı, 32 bit tamsayı) bu alanı yalnızca bu alanın değerini değiştirerek komut dosyasının yeniden çalışmasını tetiklemek için kullanın.  Herhangi bir tümsavar değeri kabul edilebilir; yalnızca önceki değerden farklı olmalıdır.
* `storageAccountName`: (isteğe bağlı, string) depolama hesabının adı. Depolama kimlik bilgilerini belirtirseniz, tümü `fileUris` Azure Blobs için URL olmalıdır.
* `storageAccountKey`: (isteğe bağlı, dize) depolama hesabının erişim anahtarı
* `managedIdentity`: (isteğe bağlı, json nesnesi) dosya(lar) indirmek için [yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
  * `clientId`: (isteğe bağlı, dize) yönetilen kimliğin istemci kimliği
  * `objectId`: (isteğe bağlı, dize) yönetilen kimliğin nesne kimliği

Aşağıdaki değerler ortak veya korumalı ayarlarda ayarlanabilir, uzantı aşağıdaki değerlerin hem genel hem de korumalı ortamlarda ayarlandığı yapılandırmayı reddeder.

* `commandToExecute`

Genel ayarları kullanarak hata ayıklama için yararlı olabilir, ancak korumalı ayarları kullanmanız önerilir.

Genel ayarlar, komut dosyasının yürütüleceği VM'ye açık metin olarak gönderilir.  Korumalı ayarlar, yalnızca Azure ve VM tarafından bilinen bir anahtar kullanılarak şifrelenir. Ayarlar gönderildiklerinde VM'ye kaydedilir, yani ayarlar şifrelenmişse VM'de şifrelenir. Şifrelenmiş değerlerin şifresini çözmek için kullanılan sertifika VM'de depolanır ve çalışma zamanında ayarların şifresini çözmek için (gerekirse) kullanılır.

####  <a name="property-managedidentity"></a>Özellik: yönetilenIdentity
> [!NOTE]
> Bu özellik yalnızca korumalı ayarlarda **belirtilmelidir.**

CustomScript (sürüm 1.10' dan itibaren), "fileUris" ayarında sağlanan URL'lerden dosya(lar) indirmek için [yönetilen kimliği](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) destekler. CustomScript'in, kullanıcı SAS belirteçleri veya depolama hesabı anahtarları gibi sırları aktarmak zorunda kalmadan Azure Depolama özel bloblarına veya kapsayıcılarına erişmesine olanak tanır.

Bu özelliği kullanmak için, kullanıcının CustomScript'in çalışması beklenen VM veya VMSS'ye [sistemle atanmış](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet#add-a-system-assigned-identity) veya [kullanıcı tarafından atanmış](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet#add-a-user-assigned-identity) bir kimlik eklemesi ve Azure Depolama [kapsayıcısına veya blob'a yönetilen kimlik erişimi vermesi](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/tutorial-vm-windows-access-storage#grant-access)gerekir.

Hedef VM/VMSS'de sistem tarafından atanan kimliği kullanmak için boş bir json nesnesine "yönetilen kimlik" alanını ayarlayın. 

> Örnek:
>
> ```json
> {
>   "fileUris": ["https://mystorage.blob.core.windows.net/privatecontainer/script1.ps1"],
>   "commandToExecute": "powershell.exe script1.ps1",
>   "managedIdentity" : {}
> }
> ```

Hedef VM/VMSS'de kullanıcı tarafından atanan kimliği kullanmak için, "yönetilen kimlik" alanını istemci kimliği yle veya yönetilen kimliğin nesne kimliğiyle yapılandırın.

> Örnekler:
>
> ```json
> {
>   "fileUris": ["https://mystorage.blob.core.windows.net/privatecontainer/script1.ps1"],
>   "commandToExecute": "powershell.exe script1.ps1",
>   "managedIdentity" : { "clientId": "31b403aa-c364-4240-a7ff-d85fb6cd7232" }
> }
> ```
> ```json
> {
>   "fileUris": ["https://mystorage.blob.core.windows.net/privatecontainer/script1.ps1"],
>   "commandToExecute": "powershell.exe script1.ps1",
>   "managedIdentity" : { "objectId": "12dd289c-0583-46e5-b9b4-115d5c19ef4b" }
> }
> ```

> [!NOTE]
> yönetilenIdentity özelliği storageAccountName veya storageAccountKey özellikleri ile birlikte **kullanılmamalıdır**

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Önceki bölümde ayrıntılı olarak açıklanan JSON şeması, dağıtım sırasında Özel Komut Dosyası Uzantısı'nı çalıştırmak için azure kaynak yöneticisi şablonunda kullanılabilir. Aşağıdaki örnekler, Özel Komut Dosyası uzantısının nasıl kullanılacağını gösterir:

* [Öğretici: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtma](../../azure-resource-manager/templates/template-tutorial-deploy-vm-extensions.md)
* [Windows ve Azure SQL DB'de İki Katmanlı Uygulama Dağıtma](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

## <a name="powershell-deployment"></a>PowerShell dağıtımı

Komut, `Set-AzVMCustomScriptExtension` Varolan bir sanal makineye Özel Komut uzantısıeklemek için kullanılabilir. Daha fazla bilgi için [Set-AzVMCustomScriptExtension'a](/powershell/module/az.compute/set-azvmcustomscriptextension)bakın.

```powershell
Set-AzVMCustomScriptExtension -ResourceGroupName <resourceGroupName> `
    -VMName <vmName> `
    -Location myLocation `
    -FileUri <fileUrl> `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="additional-examples"></a>Ek örnekler

### <a name="using-multiple-scripts"></a>Birden çok komut dosyası kullanma

Bu örnekte, sunucunuzu oluşturmak için kullanılan üç komut dosyanız vardır. **KomutToExecute** ilk komut çağırır, sonra diğerleri nasıl çağrılır seçenekleri var. Örneğin, doğru hata işleme, günlüğe kaydetme ve durum yönetimi yle yürütmeyi denetleyen bir ana komut dosyasınız olabilir. Komut dosyaları çalıştırmak için yerel makineye indirilir. Örneğin komut `1_Add_Tools.ps1` dosyasına `2_Add_Features.ps1` ekleyerek `.\2_Add_Features.ps1` çağırır ve tanımladığınız diğer komut dosyaları için `$settings`bu işlemi yinelersiniz.

```powershell
$fileUri = @("https://xxxxxxx.blob.core.windows.net/buildServer1/1_Add_Tools.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/2_Add_Features.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/3_CompleteInstall.ps1")

$settings = @{"fileUris" = $fileUri};

$storageAcctName = "xxxxxxx"
$storageKey = "1234ABCD"
$protectedSettings = @{"storageAccountName" = $storageAcctName; "storageAccountKey" = $storageKey; "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File 1_Add_Tools.ps1"};

#run command
Set-AzVMExtension -ResourceGroupName <resourceGroupName> `
    -Location <locationName> `
    -VMName <vmName> `
    -Name "buildserver1" `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion "1.10" `
    -Settings $settings    `
    -ProtectedSettings $protectedSettings `
```

### <a name="running-scripts-from-a-local-share"></a>Yerel bir paylaşımdan komut dosyalarını çalıştırma

Bu örnekte, komut dosyası konumunuz için yerel bir SMB sunucusu kullanmak isteyebilirsiniz. Bunu yaparak, **commandToExecute**dışında başka ayarlar sağlamanız gerekmez.

```powershell
$protectedSettings = @{"commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File \\filesvr\build\serverUpdate1.ps1"};

Set-AzVMExtension -ResourceGroupName <resourceGroupName> `
    -Location <locationName> `
    -VMName <vmName> `
    -Name "serverUpdate"
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion "1.10" `
    -ProtectedSettings $protectedSettings

```

### <a name="how-to-run-custom-script-more-than-once-with-cli"></a>CLI ile özel komut dosyası birden fazla kez çalıştırma

Özel komut dosyası uzantısını birden çok kez çalıştırmak istiyorsanız, bu eylemi yalnızca şu koşullar altında yapabilirsiniz:

* Uzantı **Adı** parametresi, uzantının önceki dağıtımıyla aynıdır.
* Yapılandırmayı güncelleştiraksi aksi takdirde komut yeniden yürütülmez. Zaman damgası gibi komuta dinamik bir özellik ekleyebilirsiniz.

Alternatif olarak, [ForceUpdateTag](/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension.forceupdatetag) özelliğini **doğru**ayarlayabilirsiniz.

### <a name="using-invoke-webrequest"></a>Invoke-WebRequest'i Kullanma

Komut dosyanızda [Invoke-WebRequest](/powershell/module/microsoft.powershell.utility/invoke-webrequest) kullanıyorsanız, parametreyi `-UseBasicParsing` belirtmeniz gerekir, aksi takdirde ayrıntılı durumu kontrol ederken aşağıdaki hatayı alırsınız:

```error
The response content cannot be parsed because the Internet Explorer engine is not available, or Internet Explorer's first-launch configuration is not complete. Specify the UseBasicParsing parameter and try again.
```
## <a name="virtual-machine-scale-sets"></a>Sanal Makine Ölçek Kümeleri

Özel Komut Dosyası Uzantısını Ölçek Kümesi'ne dağıtmak için [Bkz. Add-AzVmssExtension](https://docs.microsoft.com/powershell/module/az.compute/add-azvmssextension?view=azps-3.3.0)

## <a name="classic-vms"></a>Klasik VM'ler

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Klasik VM'lerde Özel Komut Dosyası Uzantısı'nı dağıtmak için Azure portalını veya Klasik Azure PowerShell cmdlets'ini kullanabilirsiniz.

### <a name="azure-portal"></a>Azure portal

Klasik VM kaynağınıza gidin. **Ayarlar**altında **Uzantıları** seçin.

+ **Ekle'yi** tıklatın ve kaynak listesinde **Özel Komut Dosyası Uzantısı'nı**seçin.

Yükle **uzantısı** sayfasında, yerel PowerShell dosyasını seçin ve bağımsız değişkenleri doldurun ve **Tamam'ı**tıklatın.

### <a name="powershell"></a>PowerShell

[Set-AzureVMCustomScriptExtension](/powershell/module/servicemanagement/azure/set-azurevmcustomscriptextension) cmdlet'i kullanarak Varolan bir sanal makineye Özel Komut Dosyası uzantısını eklemek için kullanılabilir.

```powershell
# define your file URI
$fileUri = 'https://xxxxxxx.blob.core.windows.net/scripts/Create-File.ps1'

# create vm object
$vm = Get-AzureVM -Name <vmName> -ServiceName <cloudServiceName>

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri $fileUri -Run 'Create-File.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için aşağıdaki komutu çalıştırın:

```powershell
Get-AzVMExtension -ResourceGroupName <resourceGroupName> -VMName <vmName> -Name myExtensionName
```

Uzantı çıktısı, hedef sanal makinede aşağıdaki klasörün altında bulunan dosyalara kaydedilir.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Belirtilen dosyalar hedef sanal makinede aşağıdaki klasöre indirilir.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```

uzantının `<n>` yürütülmesi arasında değişebilen ondalık tamsayı nerededir.  Değer, `1.*` uzantının `typeHandlerVersion` gerçek, geçerli değeriyle eşleşir.  Örneğin, gerçek dizin `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Komutu `commandToExecute` yürüterken, uzantı bu dizini `...\Downloads\2`(örneğin,) geçerli çalışma dizini olarak ayarlar. Bu işlem, `fileURIs` özellik üzerinden indirilen dosyaları bulmak için göreli yolların kullanılmasını sağlar. Örnekler için aşağıdaki tabloya bakın.

Mutlak indirme yolu zaman içinde değişebileceğinden, mümkün olduğunca `commandToExecute` dizedeki göreli komut dosyası/dosya yollarını tercih etmek daha iyidir. Örnek:

```json
"commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

`fileUris` Özellik listesi üzerinden indirilen dosyalar için ilk URI segmentinden sonraki yol bilgileri tutulur.  Aşağıdaki tabloda gösterildiği gibi, indirilen dosyalar `fileUris` değerlerin yapısını yansıtacak şekilde karşıdan yükleme alt dizinlerine eşlenir.  

#### <a name="examples-of-downloaded-files"></a>İndirilen Dosya Örnekleri

| URI fileUris içinde | Göreli indirilen konum | Mutlak indirilen konum <sup>1</sup> |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

<sup>1</sup> Mutlak dizin yolları VM'nin ömrü boyunca değişir, ancak CustomScript uzantısının tek bir yürütülmesi içinde değil.

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Bir Azure destek olayı da dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
