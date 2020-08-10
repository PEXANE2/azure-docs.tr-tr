---
title: Azure sanal makine aracısına genel bakış
description: Azure sanal makine aracısına genel bakış
services: virtual-machines-windows
documentationcenter: virtual-machines
author: mimckitt
manager: gwallace
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/20/2019
ms.author: akjosh
ms.openlocfilehash: 42470df5391a976e8023467758d2a3fd0890883e
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88041485"
---
# <a name="azure-virtual-machine-agent-overview"></a>Azure sanal makine aracısına genel bakış
Microsoft Azure sanal makine Aracısı (VM Aracısı), Azure yapı denetleyicisi ile sanal makine (VM) etkileşimini yöneten güvenli ve hafif bir işlemdir. VM aracısının Azure sanal makine uzantıları 'nı etkinleştirmek ve yürütmek için birincil bir rolü vardır. VM uzantıları, yazılım yükleme ve yapılandırma gibi VM 'nin dağıtım sonrası yapılandırmasını etkinleştirir. VM uzantıları, bir VM 'nin yönetici parolasını sıfırlama gibi kurtarma özelliklerini de etkinleştirir. Azure VM Aracısı olmadan VM uzantıları çalıştırılamaz.

Bu makalede, Azure sanal makine aracısının yüklenmesi ve algılanması ayrıntılı olarak anlatılmaktadır.

## <a name="install-the-vm-agent"></a>VM aracısını yükler

### <a name="azure-marketplace-image"></a>Azure Market görüntüsü

Azure VM Aracısı, Azure Marketi görüntüsünden dağıtılan tüm Windows VM 'leri için varsayılan olarak yüklenir. Portal, PowerShell, komut satırı arabirimi veya bir Azure Resource Manager şablonundan bir Azure Market görüntüsü dağıttığınızda, Azure VM Aracısı da yüklenir.

Windows Konuk Aracısı paketi iki kısma ayrılmıştır:

- Sağlama Aracısı (PA)
- Windows Konuk Aracısı (WinGA)

Bir VM 'yi önyüklemek için VM 'de PA yüklü olmalıdır, ancak WinGA 'nin yüklenmesi gerekmez. VM dağıtım zamanında, WinGA 'yi yüklememeyi seçebilirsiniz. Aşağıdaki örnek, bir Azure Resource Manager şablonuyla *Provisionvmagent* seçeneğinin nasıl kullanılacağını gösterir:

```json
"resources": [{
"name": "[parameters('virtualMachineName')]",
"type": "Microsoft.Compute/virtualMachines",
"apiVersion": "2016-04-30-preview",
"location": "[parameters('location')]",
"dependsOn": ["[concat('Microsoft.Network/networkInterfaces/', parameters('networkInterfaceName'))]"],
"properties": {
    "osProfile": {
    "computerName": "[parameters('virtualMachineName')]",
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "windowsConfiguration": {
        "provisionVmAgent": "false"
}
```

Yüklü aracılar yoksa, Azure Backup veya Azure güvenliği gibi bazı Azure hizmetlerini kullanamazsınız. Bu hizmetler için bir uzantı yüklenmesi gerekir. WinGA olmadan bir VM dağıttıysanız, daha sonra aracının en son sürümünü yükleyebilirsiniz.

### <a name="manual-installation"></a>El ile yükleme
Windows VM Aracısı bir Windows Installer paketiyle el ile yüklenebilir. Azure 'a dağıtılan özel bir VM görüntüsü oluşturduğunuzda el ile yükleme gerekli olabilir. Windows VM aracısını el ile yüklemek için [VM Aracısı yükleyicisini indirin](https://go.microsoft.com/fwlink/?LinkID=394789). VM Aracısı Windows Server 2008 (64 bit) ve sonraki sürümlerde desteklenir.

> [!NOTE]
> VMAgent 'ı, ProvisionVMAgent Enable olmadan görüntüden dağıtılan bir VM 'ye el ile yükledikten sonra AllowExtensionOperations seçeneğinin güncelleştirilmesi önemlidir.

```powershell
$vm.OSProfile.AllowExtensionOperations = $true
$vm | Update-AzVM
```

### <a name="prerequisites"></a>Önkoşullar

- Windows VM aracısının, .NET Framework 4,0 ile en az Windows Server 2008 (64-bit) çalıştırması gerekir. Bkz. [Azure 'da sanal makine aracıları Için en düşük sürüm desteği](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)

- VM 'nizin 168.63.129.16 IP adresine erişimi olduğundan emin olun. Daha fazla bilgi için bkz. [IP adresi 168.63.129.16 nedir](../../virtual-network/what-is-ip-address-168-63-129-16.md).

- Konuk VM içinde DHCP 'nin etkinleştirildiğinden emin olun. IaaS VM Aracısı ve uzantıları 'nın çalışması için DHCP 'den ana bilgisayar veya doku adresini almak için bu gereklidir. Statik bir özel IP gerekiyorsa, Azure portal veya PowerShell aracılığıyla yapılandırmanız gerekir ve VM içindeki DHCP seçeneğinin etkinleştirildiğinden emin olun. PowerShell ile statik IP adresi ayarlama hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-networks-static-private-ip-arm-ps#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface) .


## <a name="detect-the-vm-agent"></a>VM aracısını Algıla

### <a name="powershell"></a>PowerShell

Azure Resource Manager PowerShell modülü, Azure VM 'Leri hakkında bilgi almak için kullanılabilir. Azure VM aracısının sağlama durumu gibi bir VM hakkındaki bilgileri görmek için [Get-AzVM](/powershell/module/az.compute/get-azvm)' yi kullanın:

```powershell
Get-AzVM
```

Aşağıdaki sıkıştırılmış örnek çıktıda, *Osprofile*içinde iç Içe geçmiş *Provisionvmagent* özelliği gösterilmektedir. Bu özellik VM aracısının VM 'ye dağıtılıp dağıtılmadığını anlamak için kullanılabilir:

```powershell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Aşağıdaki betik, VM adlarının kısa bir listesini ve VM aracısının durumunu döndürmek için kullanılabilir:

```powershell
$vms = Get-AzVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>El ile algılama

Bir Windows VM 'de oturum açıldığında, çalışan işlemlerin incelenmesi için Görev Yöneticisi kullanılabilir. Azure VM aracısını denetlemek için, Görev Yöneticisi 'ni açın, *Ayrıntılar* sekmesine tıklayın ve **WindowsAzureGuestAgent.exe**bir işlem adı arayın. Bu işlemin varlığı, VM aracısının yüklü olduğunu gösterir.


## <a name="upgrade-the-vm-agent"></a>VM aracısını yükseltme
Windows için Azure VM Aracısı, Azure Marketi 'nden dağıtılan görüntülerde otomatik olarak yükseltilir. Yeni VM 'Ler Azure 'a dağıtıldığında, sanal makine sağlama zamanında en son VM aracısını alırlar. Aracıyı el ile yüklediyseniz veya özel VM görüntülerini dağıtıyorsanız, yeni VM aracısını görüntü oluşturma zamanına dahil etmek için el ile güncelleştirmeniz gerekir.

## <a name="windows-guest-agent-automatic-logs-collection"></a>Windows Konuk Aracısı otomatik Günlükler koleksiyonu
Windows Konuk Aracısı, bazı günlükleri otomatik olarak toplamak için bir özelliğe sahiptir. Bu özellik CollectGuestLogs.exe işlem tarafından denetleyici 'dir. Hem PaaS Cloud Services hem de IaaS sanal makineleri ve hedefi &, bir VM 'den bazı tanılama günlüklerini otomatik olarak toplamaktır ve bu nedenle çevrimdışı analizler için kullanılabilirler. Toplanan Günlükler olay günlüklerdir, işletim sistemi günlükleri, Azure günlükleri ve bazı kayıt defteri anahtarlarıdır. VM 'nin konağa aktarılan bir ZIP dosyası üretir. Bu ZIP dosyası daha sonra mühendislik ekiplerine ve destek uzmanlarına bakarak VM 'ye sahip olan müşterinin isteğiyle ilgili sorunları araştırmak için göz ardı edilebilir.

## <a name="next-steps"></a>Sonraki adımlar
VM uzantıları hakkında daha fazla bilgi için bkz. [Azure sanal makine uzantılarına ve özelliklerine genel bakış](overview.md).
