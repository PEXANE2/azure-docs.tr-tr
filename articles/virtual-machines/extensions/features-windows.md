---
title: Windows için Azure VM uzantıları ve özellikleri
description: Azure sanal makineleri için hangi uzantıların kullanılabileceğini ve sağladıklarına veya geliştirdiklerine göre gruplandırılan uzantıları öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: akjosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cd6439bf1b1f52b8e63819e8e519fc4971d1bc2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066847"
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Windows için sanal makine uzantıları ve özellikleri

Azure sanal makine (VM) uzantıları, dağıtım sonrası yapılandırma ve Azure VM'lerinde otomasyon görevleri sunan küçük uygulamalardır. Örneğin bir sanal makinede yazılım yüklemesi gerekiyorsa, virüsten koruma gerekiyorsa veya içinde bir betik çalıştırılacaksa VM uzantısı kullanılabilir. Azure VM uzantıları Azure CLI, PowerShell, Azure Resource Manager şablonları ve Azure portal ile çalıştırılabilir. Uzantılar yeni bir VM dağıtımıyla paketlenebilir veya var olan herhangi bir sistemde çalıştırılabilir.

Bu makalede, VM uzantılarına genel bakış, Azure VM uzantılarını kullanmak için ön koşullar ve VM uzantılarının nasıl algılanıp, yöneteceğimize ve kaldırılacaklarına ilişkin kılavuzlar yer almaktadır. Bu makalede, her biri benzersiz bir yapılandırmaya sahip birçok VM uzantısı bulunduğundan genelleştirilmiş bilgiler sağlanmaktadır. Uzantıya özgü ayrıntılar, her belgede tek tek uzantıya özgü bulunabilir.

 

## <a name="use-cases-and-samples"></a>Servis taleplerini ve örneklerini kullanma

Her biri belirli bir kullanım örneğine sahip birkaç farklı Azure VM uzantısı mevcuttur. Bazı örnekler:

- PowerShell İstenilen Durum yapılandırmalarını Windows için DSC uzantısı olan bir VM'ye uygulayın. Daha fazla bilgi için Bkz. [Azure İstenilen Durum yapılandırma uzantısı.](dsc-overview.md)
- Log Analytics Agent VM uzantısı ile bir VM'nin izlemeyi yapılandırın. Daha fazla bilgi için azure [vm'lerini Azure Monitor günlüklerine bağlayın'](../../log-analytics/log-analytics-azure-vm-extension.md)a bakın.
- Chef'i kullanarak Azure VM'yi yapılandırın. Daha fazla bilgi için Bkz. [Chef ile Azure VM dağıtımını otomatikleştirin.](../../chef/chef-automation.md)
- Azure altyapınızın izleme denetimini Datadog uzantısı ile yapılandırın. Daha fazla bilgi için [Datadog bloguna](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/)bakın.


İşleme özel uzantılara ek olarak, hem Windows hem de Linux sanal makineleri için özel komut dosyası uzantısı kullanılabilir. Windows için Özel Komut Dosyası uzantısı, herhangi bir PowerShell komut dosyasının VM'de çalıştırılmasına izin verir. Özel komut dosyaları, yerel Azure araçlarının sağlayabileceğinin ötesinde yapılandırma gerektiren Azure dağıtımları tasarlamak için yararlıdır. Daha fazla bilgi için [Windows VM Özel Komut Dosyası uzantısına](custom-script-windows.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

VM'deki uzantıyı işlemek için Azure Windows Aracısı'nın yüklü olması gerekir. Bazı bireysel uzantıların kaynaklara erişim veya bağımlılıklar gibi ön koşulları vardır.

### <a name="azure-vm-agent"></a>Azure VM aracısı

Azure VM aracısı, Bir Azure VM ile Azure kumaş denetleyicisi arasındaki etkileşimleri yönetir. VM aracısı, VM uzantılarının çalıştırılması da dahil olmak üzere Azure VM'lerini dağıtmanın ve yönetmenin birçok işlevsel yönüne sorumludur. Azure VM aracısı Azure Marketi görüntülerine önceden yüklenir ve desteklenen işletim sistemlerine el ile yüklenebilir. Windows için Azure VM aracısı, Windows Guest aracısı olarak bilinir.

Desteklenen işletim sistemleri ve yükleme yönergeleri hakkında daha fazla bilgi için [Azure sanal makine aracısı'na](agent-windows.md)bakın.

#### <a name="supported-agent-versions"></a>Desteklenen aracı sürümleri

Mümkün olan en iyi deneyimi sağlamak için, aracının minimum sürümleri vardır. Daha fazla bilgi için [bu makaleye](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)bakın.

#### <a name="supported-oses"></a>Desteklenen OS'ler

Windows Guest aracısı birden çok os'ta çalışır, ancak uzantıları çerçeve, uzantıları OS'ler için bir sınırı vardır. Daha fazla bilgi için [bu makaleye](https://support.microsoft.com/help/4078134/azure-extension-supported-operating-systems
)bakın.

Bazı uzantılar tüm Işletim Sistemi'lerde desteklenmez ve *'Desteklenmeyen Işletim Sistemi' hata kodu 51'i*yayabilir. Desteklenebilirlik için tek tek uzantı belgelerini kontrol edin.

#### <a name="network-access"></a>Ağ erişimi

Uzantı paketleri Azure Depolama uzantısı deposundan indirilir ve uzantı durumu yüklemeleri Azure Depolama'ya nakledilir. Aracıların [desteklenen](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) sürümünü kullanıyorsanız, aracı iletişimi için Azure kumaş denetleyicisine yönlendirmek için aracıyı kullanabileceğiniz gibi, Aracının VM bölgesindeki Azure Depolama'ya erişmesine izin vermeniz gerekmez (özel IP [168.63.129.16'daki](https://docs.microsoft.com/azure/virtual-network/what-is-ip-address-168-63-129-16)ayrıcalıklı kanal aracılığıyla HostGAPlugin özelliği). Aracının desteklenmeyen bir sürümündeyseniz, VM'den o bölgedeki Azure depolama alanına giden erişime izin verebilirsiniz.

> [!IMPORTANT]
> Konuk güvenlik duvarını kullanarak veya proxy ile *168.63.129.16* erişimi engellediyseniz, uzantılar yukarıdakilerden bağımsız olarak başarısız olur. 80, 443 ve 32526 bağlantı noktaları gereklidir.

Aracılar yalnızca uzantı paketlerini ve raporlama durumunu indirmek için kullanılabilir. Örneğin, bir uzantı yüklemesi GitHub'dan (Özel Komut Dosyası) komut dosyası indirmesi gerekiyorsa veya Azure Depolama 'ya (Azure Yedeklemesi) erişmesi gerekiyorsa, ek güvenlik duvarı/Ağ Güvenlik Grubu bağlantı noktalarının açılması gerekir. Kendi başlarına uygulamalar olduğundan, farklı uzantıların farklı gereksinimleri vardır. Azure Depolama veya Azure Etkin Dizin'e erişim gerektiren uzantılar için, [Azure NSG Hizmet](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) Etiketleri'ni kullanarak Depolama veya AzureActiveDirectory'ye erişime izin verebilirsiniz.

Windows Guest Agent üzerinden aracı trafik isteklerini yönlendirmek için proxy sunucu desteği yok, bu da Windows Guest Agent internet veya IP üzerinden Ana Bilgisayar kaynakları erişmek için özel proxy (varsa) güvenecektir anlamına gelir 168.63.129.16.

## <a name="discover-vm-extensions"></a>VM uzantılarını keşfedin

Azure VM'leri ile kullanabileceğiniz birçok farklı VM uzantısı vardır. Tam bir liste yi görmek için [Get-AzVMExtensionImage'ı](https://docs.microsoft.com/powershell/module/az.compute/get-azvmextensionimage)kullanın. Aşağıdaki *örnekte, WestUS* konumundaki tüm kullanılabilir uzantılar listelenmektedir:

```powershell
Get-AzVmImagePublisher -Location "WestUS" | `
Get-AzVMExtensionImageType | `
Get-AzVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>VM uzantılarını çalıştırma

Azure VM uzantıları varolan VM'lerde çalışır, bu da zaten dağıtılmış bir VM'de yapılandırma değişiklikleri yapmanız veya bağlantı kurtarmanız gerektiğinde yararlıdır. VM uzantıları, Azure Kaynak Yöneticisi şablon dağıtımlarıyla da birlikte kullanılabilir. Azure VM'leri, Kaynak Yöneticisi şablonlarına sahip uzantılar kullanarak dağıtım sonrası müdahale olmadan dağıtılabilir ve yapılandırılabilir.

Aşağıdaki yöntemler, bir uzantıyı varolan bir VM'ye karşı çalıştırmak için kullanılabilir.

### <a name="powershell"></a>PowerShell

Tek tek uzantıları çalıştırmak için çeşitli PowerShell komutları vardır. Listeyi görmek için [Uzan'da Komut Al](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-command) ve filtreyi kullanın: *Extension*

```powershell
Get-Command Set-Az*Extension* -Module Az.Compute
```

Bu, aşağıdakilere benzer çıktı sağlar:

```powershell
CommandType     Name                                          Version    Source
-----------     ----                                          -------    ------
Cmdlet          Set-AzVMAccessExtension                       4.5.0      Az.Compute
Cmdlet          Set-AzVMADDomainExtension                     4.5.0      Az.Compute
Cmdlet          Set-AzVMAEMExtension                          4.5.0      Az.Compute
Cmdlet          Set-AzVMBackupExtension                       4.5.0      Az.Compute
Cmdlet          Set-AzVMBginfoExtension                       4.5.0      Az.Compute
Cmdlet          Set-AzVMChefExtension                         4.5.0      Az.Compute
Cmdlet          Set-AzVMCustomScriptExtension                 4.5.0      Az.Compute
Cmdlet          Set-AzVMDiagnosticsExtension                  4.5.0      Az.Compute
Cmdlet          Set-AzVMDiskEncryptionExtension               4.5.0      Az.Compute
Cmdlet          Set-AzVMDscExtension                          4.5.0      Az.Compute
Cmdlet          Set-AzVMExtension                             4.5.0      Az.Compute
Cmdlet          Set-AzVMSqlServerExtension                    4.5.0      Az.Compute
Cmdlet          Set-AzVmssDiskEncryptionExtension             4.5.0      Az.Compute
```

Aşağıdaki örnek, Bir GitHub deposundan hedef sanal makineye bir komut dosyası indirmek ve sonra komut dosyası çalıştırmak için Özel Komut Dosyası uzantısı kullanır. Özel Komut Dosyası uzantısı hakkında daha fazla bilgi için Bkz. [Özel Komut Dosyası uzantısıgenel bakış.](custom-script-windows.md)

```powershell
Set-AzVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

Aşağıdaki örnekte, VM Access uzantısı, windows vm'nin yönetim parolasını geçici bir parolaya sıfırlamak için kullanılır. VM Access uzantısı hakkında daha fazla bilgi için [Windows VM'deki Uzak Masaüstü hizmetini sıfırla'ya](../windows/reset-rdp.md)bakın. Bunu çalıştırdıktan sonra, ilk girişte parolayı sıfırlamanız gerekir:

```powershell
$cred=Get-Credential

Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

Komut `Set-AzVMExtension` herhangi bir VM uzantısı başlatmak için kullanılabilir. Daha fazla bilgi için [Set-AzVMExtension başvurusuna](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension)bakın.


### <a name="azure-portal"></a>Azure portalında

VM uzantıları, Azure portalı üzerinden varolan bir VM'ye uygulanabilir. Portaldaki VM'yi seçin, **Uzantıları**seçin, ardından **Ekle'yi**seçin. Kullanılabilir uzantılar listesinden istediğiniz uzantıyı seçin ve sihirbazdaki yönergeleri izleyin.

Aşağıdaki örnek, Azure portalından Microsoft Kötü Amaçlı Yazılımdan Koruma uzantısının yüklenmesini gösterir:

![Kötü amaçlı yazılımdan koruma uzantısı yükleme](./media/features-windows/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

VM uzantıları bir Azure Kaynak Yöneticisi şablonuna eklenebilir ve şablonun dağıtımıyla yürütülebilir. Bir uzantıyı şablonla dağıttığınızda, tam olarak yapılandırılmış Azure dağıtımları oluşturabilirsiniz. Örneğin, aşağıdaki JSON, bir Kaynak Yöneticisi şablonundan alınır, yük dengeli VM'ler kümesi ve Azure SQL veritabanı dağıtır ve her VM'ye bir .NET Core uygulaması yükler. VM uzantısı yazılım yükleme ilgilenir.

Daha fazla bilgi için [kaynak yöneticisi şablonuna](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)bakın.

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
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
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Kaynak Yöneticisi şablonları oluşturma hakkında daha fazla bilgi için bkz. Windows [VM uzantılarıyla Azure Kaynak Yöneticisi şablonları yazma.](../windows/template-description.md#extensions)

## <a name="secure-vm-extension-data"></a>Güvenli VM uzantısı verileri

Bir VM uzantısı çalıştırdığınızda, kimlik bilgileri, depolama hesabı adları ve depolama hesabı erişim anahtarları gibi hassas bilgileri eklemek gerekebilir. Birçok VM uzantısı, verileri şifreleyen ve yalnızca hedef VM'nin içindeki şifresini çözen korumalı bir yapılandırma içerir. Her uzantı belirli bir korumalı yapılandırma şeması vardır ve her biri uzantıya özgü belgelerde ayrıntılı olarak açıklanır.

Aşağıdaki örnek, Windows için Özel Komut Dosyası uzantısının bir örneğini gösterir. Yürütülecek komut bir kimlik bilgileri kümesi içerir. Bu örnekte, yürütülecek komut şifrelenmez:

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
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
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ],
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Özelliği **nizli yapılandırmaya taşımak,** aşağıdaki örnekte gösterildiği gibi yürütme dizesini korur: **protected**

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
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
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Uzantıları kullanan bir Azure IaaS VM'de, sertifikalar **_konsolunda, Windows Azure CRP Sertifika Üreteci_** konusuna sahip sertifikalar görebilirsiniz. Klasik RDFE VM'de, bu sertifikalar **_Uzantılar için Windows Azure Hizmet Yönetimi_** konusuna sahiptir.

Bu sertifikalar, uzantılar tarafından kullanılan korumalı ayarların (parola, diğer kimlik bilgileri) aktarılması sırasında VM ve ana bilgisayar arasındaki iletişimi güvence altına alarmaktadır. Sertifikalar Azure kumaş denetleyicisi tarafından oluşturulur ve VM Aracısı'na aktarılır. VM'yi her gün durdurur ve çalıştırın, kumaş denetleyicisi tarafından yeni bir sertifika oluşturulabilir. Sertifika bilgisayarın Kişisel sertifika deposunda depolanır. Bu sertifikalar silinebilir. VM Aracısı gerekirse sertifikaları yeniden oluşturur.

### <a name="how-do-agents-and-extensions-get-updated"></a>Aracılar ve uzantılar nasıl güncellenir?

Aracılar ve Uzantılar aynı güncelleştirme mekanizmasını paylaşır. Bazı güncelleştirmeler ek güvenlik duvarı kuralları gerektirmez.

Bir güncelleştirme kullanılabilir olduğunda, yalnızca uzantılarda bir değişiklik olduğunda vm'ye yüklenir ve aşağıdakiler gibi diğer VM Modeli değişiklikleri yapılır:

- Veri diskleri
- Uzantılar
- Önyükleme tanılama konteyneri
- Konuk İşletim Sistemi sırları
- VM boyutu
- Ağ profili

Yayıncılar güncelleştirmeleri farklı zamanlarda bölgelere kullanılabilir hale getirmektedir, bu nedenle farklı sürümlerde farklı bölgelerdeki VM'lere sahip olabilirsiniz.

#### <a name="listing-extensions-deployed-to-a-vm"></a>VM'ye Dağıtılan Uzantıları Listeleme

```powershell
$vm = Get-AzVM -ResourceGroupName "myResourceGroup" -VMName "myVM"
$vm.Extensions | select Publisher, VirtualMachineExtensionType, TypeHandlerVersion
```

```powershell
Publisher             VirtualMachineExtensionType          TypeHandlerVersion
---------             ---------------------------          ------------------
Microsoft.Compute     CustomScriptExtension                1.9
```

#### <a name="agent-updates"></a>Aracı güncelleştirmeleri

Windows Konuk Aracısı yalnızca *Uzantı İşleme kodu*içerir, *Windows Provisioning kodu* ayrıdır. Windows Konuk Aracısını kaldırabilirsiniz. Pencere Konuk Aracısı'nın otomatik güncelleştirmesini devre dışı kaldıramazsınız.

*Uzantı İşleme kodu,* Azure dokusuyla iletişim kurmak ve yüklemeler, durum bildirme, tek tek uzantıları güncelleştirme ve bunları kaldırma gibi VM uzantıları işlemlerini işlemekten sorumludur. Güncelleştirmeler güvenlik düzeltmeleri, hata düzeltmeleri ve *Uzantı İşleme kodundaki*geliştirmeleri içerir.

Hangi sürümü çalıştırdığınızı denetlemek için [yüklü Windows Konuk Aracısını Algılama'ya](agent-windows.md#detect-the-vm-agent)bakın.

#### <a name="extension-updates"></a>Uzantı güncelleştirmeleri

Bir uzantı güncelleştirmesi kullanılabilir olduğunda, Windows Guest Agent uzantıyı karşıdan yüklenir ve yükseltir. Otomatik uzantı güncelleştirmeleri *Minor* veya *Hotfix'tir.* Uzantıyı siz lerle birlikte verirken *Küçük* güncelleştirmeleri devre dışı kılabilir veya devre dışı kullanabilirsiniz. Aşağıdaki örnek, otomatik YükseltmeMinorVersion ile Kaynak Yöneticisi şablonundaki küçük sürümleri niçin otomatik olarak yükselteceklerini *gösterir": doğru,'*:

```json
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
```

En son küçük sürüm hata düzeltmelerini almak için, uzantı dağıtımlarınızda her zaman otomatik güncelleştirmeyi seçmeniz önerilir. Güvenlik veya anahtar hata düzeltmeleri taşıyan Hotfix güncelleştirmeleri devre dışı kalınamaz.

### <a name="how-to-identify-extension-updates"></a>Uzantı güncelleştirmeleri nasıl tanımlanır?

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Bir VM'de otomatik YükseltmeMinorVersion ile uzanTıolup olmadığını belirleme

Uzantın 'autoUpgradeMinorVersion' ile sağlanmış olup olmadığını VM modelinden görebilirsiniz. Kontrol etmek için [Get-AzVm'i](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) kullanın ve kaynak grubunu ve VM adını aşağıdaki gibi sağlayın:

```powerShell
 $vm = Get-AzVm -ResourceGroupName "myResourceGroup" -VMName "myVM"
 $vm.Extensions
```

Aşağıdaki örnek *çıktı, autoUpgradeMinorVersion'un* *doğru*olarak ayarladığını gösterir:

```powershell
ForceUpdateTag              :
Publisher                   : Microsoft.Compute
VirtualMachineExtensionType : CustomScriptExtension
TypeHandlerVersion          : 1.9
AutoUpgradeMinorVersion     : True
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>OtomatikYükseltmeMinorVersion'un ne zaman oluştuğunu tanımlama

Uzantıiçin bir güncelleştirmenin ne zaman gerçekleştiğini görmek için *C:\WindowsAzure\Logs\WaAppAgent.log* adresindeki VM'deki aracı günlüklerini inceleyin

Aşağıdaki örnekte, VM *Microsoft.Compute.CustomScriptExtension 1.8* yüklü vardı. Bir düzeltme sürüm *1.9*için kullanılabilir:

```powershell
[INFO]  Getting plugin locations for plugin 'Microsoft.Compute.CustomScriptExtension'. Current Version: '1.8', Requested Version: '1.9'
[INFO]  Auto-Upgrade mode. Highest public version for plugin 'Microsoft.Compute.CustomScriptExtension' with requested version: '1.9', is: '1.9'
```

## <a name="agent-permissions"></a>Aracı izinleri

Görevlerini gerçekleştirmek için aracının Yerel *Sistem*olarak çalışması gerekir.

## <a name="troubleshoot-vm-extensions"></a>Sorun Giderme VM uzantıları

Her VM uzantısı, uzantıya özgü sorun giderme adımları olabilir. Örneğin, Özel Komut Dosyası uzantısını kullandığınızda, komut dosyası yürütme ayrıntıları, uzantıntın çalıştırıldığı VM'de yerel olarak bulunabilir. Uzantıya özgü sorun giderme adımları uzantıya özgü belgelerde ayrıntılı olarak açıklanır.

Aşağıdaki sorun giderme adımları tüm VM uzantıları için geçerlidir.

1. Windows Guest Agent Log'u kontrol etmek için, uzantınız *C:\WindowsAzure\Logs\WaAppAgent.log'da* sağlanan etkinlikteki lere bakın

2. *C:\WindowsAzure\Logs\Plugins extensionName>\<* daha fazla bilgi için gerçek uzantı günlüklerini kontrol edin

3. Hata kodları, bilinen sorunlar vb. için uzantıya özgü belge sorun giderme bölümlerini denetleyin.

4. Sistem kayıtlarına bak. Özel paket yöneticisi erişimi gerektiren başka bir uygulamanın uzun süre devam eden yüklemesi gibi uzantıyı etkileyebilecek diğer işlemleri denetleyin.

### <a name="common-reasons-for-extension-failures"></a>Uzantı hatalarının yaygın nedenleri

1. Uzantıları çalıştırmak için 20 dakika var (özel durumlar CustomScript uzantıları, Şef ve 90 dakika var DSC vardır). Dağıtımınız bu zamanı aşarsa, zaman aşımı olarak işaretlenir. Bunun nedeni, düşük kaynak VM'leri, diğer VM yapılandırmaları/uzantı sağlamaya çalışırken yüksek miktarda kaynak tüketen görevleri başlatmadan kaynaklanabilir.

2. Asgari ön koşullar yerine getirilmedi. Bazı uzantıların HPC görüntüleri gibi VM SKU'lara bağımlı olduğu vardır. Uzantılar, Azure Depolama alanıyla veya kamu hizmetlerine iletişim kurmak gibi belirli ağ erişimi gereksinimleri gerektirebilir. Diğer örnekler paket depolarına erişim, disk alanının tükenmesi veya güvenlik kısıtlamaları olabilir.

3. Özel paket yöneticisi erişimi. Bazı durumlarda, her ikisinin de paket yöneticisine özel erişime ihtiyaç duyduğu uzun soluklu bir VM yapılandırması ve uzantı yüklemesi çakışıyor olabilir.

### <a name="view-extension-status"></a>Uzantı durumunu görüntüleme

VM uzantısı bir VM'ye karşı çalıştırıldıktan sonra, uzantı durumunu döndürmek için [Get-AzVM'yi](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) kullanın. *Alt durumlar[0]* uzantı sağlamanın başarılı olduğunu, yani VM'ye başarılı bir şekilde dağıtılmış olduğunu, ancak VM içindeki uzantının yürütülmesinin başarısız olduğunu gösterir, *Alt Durumlar[1]*.

```powershell
Get-AzVM -ResourceGroupName "myResourceGroup" -VMName "myVM" -Status
```

Çıktı aşağıdaki örnek çıktıya benzer:

```powershell
Extensions[0]           :
  Name                  : CustomScriptExtension
  Type                  : Microsoft.Compute.CustomScriptExtension
  TypeHandlerVersion    : 1.9
  Substatuses[0]        :
    Code                : ComponentStatus/StdOut/succeeded
    Level               : Info
    DisplayStatus       : Provisioning succeeded
    Message             : Windows PowerShell \nCopyright (C) Microsoft Corporation. All rights reserved.\n
  Substatuses[1]        :
    Code                : ComponentStatus/StdErr/succeeded
    Level               : Info
    DisplayStatus       : Provisioning succeeded
    Message             : The argument 'cseTest%20Scriptparam1.ps1' to the -File parameter does not exist. Provide the path to an existing '.ps1' file as an argument to the

-File parameter.
  Statuses[0]           :
    Code                : ProvisioningState/failed/-196608
    Level               : Error
    DisplayStatus       : Provisioning failed
    Message             : Finished executing command
```

Uzantı yürütme durumu Azure portalında da bulunabilir. Uzantının durumunu görüntülemek için VM'yi seçin, **Uzantıları**seçin ve ardından istediğiniz uzantıyı seçin.

### <a name="rerun-vm-extensions"></a>VM uzantılarını yeniden çalıştırın

VM uzantısının yeniden çalıştırılması gereken durumlar olabilir. Bir uzantıyı kaldırarak ve ardından istediğiniz yürütme yöntemiyle uzantıyı yeniden çalıştırarak yeniden çalıştırabilirsiniz. Uzantıyı kaldırmak için [Remove-AzVMExtension'ı](https://docs.microsoft.com/powershell/module/az.compute/Remove-AzVMExtension) aşağıdaki gibi kullanın:

```powershell
Remove-AzVMExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myExtensionName"
```

Azure portalındaki bir uzantıyı aşağıdaki gibi kaldırabilirsiniz:

1. Bir VM seçin.
2. **Uzantıları**seçin.
3. İstenilen uzantıyı seçin.
4. **Kaldır'ı**seçin.

## <a name="common-vm-extensions-reference"></a>Ortak VM uzantıları başvurusu
| Uzantı adı | Açıklama | Daha fazla bilgi |
| --- | --- | --- |
| Windows için Özel Betik Uzantısı |Komut dosyalarını Azure sanal makinesine karşı çalıştırma |[Windows için Özel Betik Uzantısı](custom-script-windows.md) |
| Windows için DSC Uzantısı |PowerShell DSC (Istenilen Durum Yapılandırması) uzantısı |[Windows için DSC Uzantısı](dsc-overview.md) |
| Azure Tanılama Uzantısı |Azure Tanılama'yı Yönetme |[Azure Tanılama Uzantısı](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure VM Erişim Uzantısı |Kullanıcıları ve kimlik bilgilerini yönetme |[Linux için VM Erişim Uzantısı](https://azure.microsoft.com/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

VM uzantıları hakkında daha fazla bilgi için [Azure sanal makine uzantılarına ve özelliklere genel bakış](overview.md)alabakın.
