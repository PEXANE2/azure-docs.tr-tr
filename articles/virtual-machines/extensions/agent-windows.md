---
title: Azure Sanal Makine Aracısı Genel Bakış
description: Azure Sanal Makine Aracısı Genel Bakış
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
ms.openlocfilehash: f29a20ddeb93ec3d4aa98bbcb36f50456b543667
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81452579"
---
# <a name="azure-virtual-machine-agent-overview"></a>Azure Sanal Makine Aracısı'na genel bakış
Microsoft Azure Sanal Makine Aracısı (VM Agent), Azure Kumaş Denetleyicisi ile sanal makine (VM) etkileşimini yöneten güvenli ve hafif bir işlemdir. VM Aracısı, Azure sanal makine uzantılarını etkinleştirme ve yürütmede birincil role sahiptir. VM Uzantıları, yazılım yükleme ve yapılandırma gibi VM'nin dağıtım sonrası yapılandırmasını sağlar. VM uzantıları, vm'nin yönetim parolasını sıfırlama gibi kurtarma özelliklerini de sağlar. Azure VM Aracısı olmadan VM uzantıları çalıştırılamaz.

Bu makalede, Azure Sanal Makine Aracısı'nın yüklenmesi ve algılanması ayrıntılı olarak anlatılıyor.

## <a name="install-the-vm-agent"></a>VM Aracısını Yükleyin

### <a name="azure-marketplace-image"></a>Azure Marketi görüntüsü

Azure VM Aracısı varsayılan olarak Azure Marketi görüntüden dağıtılan herhangi bir Windows VM'ye yüklenir. Portaldan, PowerShell'den, Komut Satırı Arabiriminden veya Azure Kaynak Yöneticisi şablonundan bir Azure Marketi görüntüsü dağıttığınızda, Azure VM Aracısı da yüklenir.

Windows Konuk Aracı Paketi iki bölüme ayrılmıştır:

- Provizyon Aracısı (PA)
- Windows Konuk Aracı (WinGA)

VM önyükleme için PA VM yüklü olması gerekir, ancak WinGA yüklü olması gerekmez. VM dağıtım zamanında WinGA'yı yüklememe seçeneğini belirleyebilirsiniz. Aşağıdaki örnekte, Azure Kaynak Yöneticisi şablonuyla *provisionVmAgent* seçeneğinin nasıl seçilen gösterilmektedir:

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

Aracılar yüklü değilse, Azure Yedekleme veya Azure Güvenliği gibi bazı Azure hizmetlerini kullanamazsınız. Bu hizmetlerin yüklenmesi için bir uzantı gerekir. WinGA olmadan bir VM dağıttıysanız, aracının en son sürümünü daha sonra yükleyebilirsiniz.

### <a name="manual-installation"></a>El ile yükleme
Windows VM aracısı, Windows yükleyici paketiyle el ile yüklenebilir. Azure'a dağıtılan özel bir VM görüntüsü oluşturduğunuzda el ile yükleme gerekebilir. Windows VM Aracısını el ile yüklemek için [VM Agent yükleyicisini indirin.](https://go.microsoft.com/fwlink/?LinkID=394789) VM Aracısı Windows Server 2008 R2 ve daha sonra desteklenir.

> [!NOTE]
> ProvisionVMAgent etkinleştirilmeden görüntüden dağıtılan bir VM VM vm vm el ile yükledikten sonra AllowExtensionOperations seçeneğini güncelleştirmek önemlidir.

```powershell
$vm.OSProfile.AllowExtensionOperations = $true
$vm | Update-AzVM
```

### <a name="prerequisites"></a>Ön koşullar
- Windows VM Aracısı'nın .Net Framework 4.0 ile çalışması için en az Windows Server 2008 R2 (64 bit) gerekir. [Azure'da sanal makine aracıları için minimum sürüm desteğine](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) bakın

- VM'nizin IP adresi 168.63.129.16'ya erişebilmesini sağlayın. Daha fazla bilgi için ip [adresi nedir 168.63.129.16](https://docs.microsoft.com/azure/virtual-network/what-is-ip-address-168-63-129-16)bakın.

## <a name="detect-the-vm-agent"></a>VM Aracısını algılama

### <a name="powershell"></a>PowerShell

Azure Kaynak Yöneticisi PowerShell modülü, Azure VM'leri hakkında bilgi almak için kullanılabilir. Azure VM Aracısı'nın sağlama durumu gibi bir VM hakkındaki bilgileri görmek için [Get-AzVM'yi](https://docs.microsoft.com/powershell/module/az.compute/get-azvm)kullanın:

```powershell
Get-AzVM
```

Aşağıdaki yoğunlaştırılmış örnek çıktı, *OSProfile'ın*içinde iç içe olan *ProvisionVMAgent* özelliğini gösterir. Bu özellik, VM aracısının VM'ye dağıtılmış olup olmadığını belirlemek için kullanılabilir:

```powershell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Aşağıdaki komut dosyası, VM adlarının kısa bir listesini ve VM Aracısı'nın durumunu döndürmek için kullanılabilir:

```powershell
$vms = Get-AzVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Manuel Algılama

Bir Windows VM'de oturum açtığınızda, Görev Yöneticisi çalışan işlemleri incelemek için kullanılabilir. Azure VM Aracısını denetlemek için Görev Yöneticisi'ni açın, *Ayrıntılar* sekmesini tıklatın ve **windowsAzureGuestAgent.exe**bir işlem adı arayın. Bu işlemin varlığı VM aracısının yüklü olduğunu gösterir.


## <a name="upgrade-the-vm-agent"></a>VM Aracısını Yükselt
Windows için Azure VM Aracısı otomatik olarak yükseltilir. Yeni VM'ler Azure'a dağıtılınca, VM sağlama zamanında en son VM aracısını alırlar. Özel VM görüntüleri, görüntü oluşturma zamanında yeni VM aracısını içerecek şekilde el ile güncelleştirilmelidir.

## <a name="windows-guest-agent-automatic-logs-collection"></a>Windows Guest Agent Otomatik Günlükler Koleksiyonu
Windows Guest Agent'ın bazı günlükleri otomatik olarak toplamak için bir özelliği vardır. Bu özellik CollectGuestLogs.exe işlemi tarafından denetlenir. Hem PaaS Bulut Hizmetleri hem de IaaS Sanal Makineler için vardır ve amacı, bir VM'den bazı tanılama günlüklerini hızlı bir şekilde toplamak & , böylece çevrimdışı analiz için kullanılabilirler. Toplanan günlükler Olay Günlükleri, İşletim Sistemi Günlükleri, Azure Günlükleri ve bazı kayıt defteri anahtarlarıdır. VM'nin Ana Bilgisayar'ına aktarılan bir ZIP dosyası üretir. Bu ZIP dosyası daha sonra Mühendislik Ekipleri ve Destek uzmanları tarafından VM sahibi müşterinin isteği üzerine sorunları araştırmak için bakılabilir.

## <a name="next-steps"></a>Sonraki adımlar
VM uzantıları hakkında daha fazla bilgi için [Azure sanal makine uzantılarına ve özelliklere genel bakış](overview.md)alabakın.
