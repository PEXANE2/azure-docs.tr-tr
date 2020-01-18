---
title: Azure sanal makine aracısına genel bakış
description: Azure sanal makine aracısına genel bakış
services: virtual-machines-windows
documentationcenter: virtual-machines
author: MicahMcKittrick-MSFT
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/20/2019
ms.author: akjosh
ms.openlocfilehash: 24369ed547b811b212518193a2ae2f76ed197754
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264636"
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
Windows VM Aracısı bir Windows Installer paketiyle el ile yüklenebilir. Azure 'a dağıtılan özel bir VM görüntüsü oluşturduğunuzda el ile yükleme gerekli olabilir. Windows VM aracısını el ile yüklemek için [VM Aracısı yükleyicisini indirin](https://go.microsoft.com/fwlink/?LinkID=394789). VM Aracısı Windows Server 2008 R2 ve üzeri sürümlerde desteklenir.

> [!NOTE]
> VMAgent 'ı, ProvisionVMAgent Enable olmadan görüntüden dağıtılan bir VM 'ye el ile yükledikten sonra AllowExtensionOperations seçeneğinin güncelleştirilmesi önemlidir.

```powershell
$vm.OSProfile.AllowExtensionOperations = $true
$vm | Update-AzVM
```

### <a name="prerequisites"></a>Ön koşullar
- Windows VM aracısının, .NET Framework 4,0 ile en az Windows Server 2008 R2 (64-bit) çalıştırması gerekir. Bkz. [Azure 'da sanal makine aracıları Için en düşük sürüm desteği](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)

- VM 'nizin 168.63.129.16 IP adresine erişimi olduğundan emin olun. Daha fazla bilgi için bkz. [IP adresi 168.63.129.16 nedir](https://docs.microsoft.com/azure/virtual-network/what-is-ip-address-168-63-129-16).

## <a name="detect-the-vm-agent"></a>VM aracısını Algıla

### <a name="powershell"></a>PowerShell

Azure Resource Manager PowerShell modülü, Azure VM 'Leri hakkında bilgi almak için kullanılabilir. Azure VM aracısının sağlama durumu gibi bir VM hakkındaki bilgileri görmek için [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm)' yi kullanın:

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

Bir Windows VM 'de oturum açıldığında, çalışan işlemlerin incelenmesi için Görev Yöneticisi kullanılabilir. Azure VM aracısını denetlemek için, Görev Yöneticisi 'ni açın, *Ayrıntılar* sekmesine tıklayın ve bir Işlem adı **WindowsAzureGuestAgent. exe**' yi arayın. Bu işlemin varlığı, VM aracısının yüklü olduğunu gösterir.


## <a name="upgrade-the-vm-agent"></a>VM aracısını yükseltme
Windows için Azure VM Aracısı otomatik olarak yükseltilir. Yeni VM 'Ler Azure 'a dağıtıldığında, sanal makine sağlama zamanında en son VM aracısını alırlar. Özel VM görüntülerinin görüntü oluşturma zamanına yeni VM aracısını içerecek şekilde el ile güncelleştirilmeleri gerekir.

## <a name="windows-guest-agent-automatic-logs-collection"></a>Windows Konuk Aracısı otomatik Günlükler koleksiyonu
Windows Konuk Aracısı, bazı günlükleri otomatik olarak toplamak için bir özelliğe sahiptir. Bu özellik, CollectGuestLogs. exe işlemi tarafından denetleyici 'dir. Hem PaaS Cloud Services hem de IaaS sanal makineleri ve hedefi &, bir VM 'den bazı tanılama günlüklerini otomatik olarak toplamaktır ve bu nedenle çevrimdışı analizler için kullanılabilirler. Toplanan Günlükler olay günlüklerdir, işletim sistemi günlükleri, Azure günlükleri ve bazı kayıt defteri anahtarlarıdır. VM 'nin konağa aktarılan bir ZIP dosyası üretir. Bu ZIP dosyası daha sonra mühendislik ekiplerine ve destek uzmanlarına bakarak VM 'ye sahip olan müşterinin isteğiyle ilgili sorunları araştırmak için göz ardı edilebilir.

## <a name="next-steps"></a>Sonraki adımlar
VM uzantıları hakkında daha fazla bilgi için bkz. [Azure sanal makine uzantılarına ve özelliklerine genel bakış](overview.md).
