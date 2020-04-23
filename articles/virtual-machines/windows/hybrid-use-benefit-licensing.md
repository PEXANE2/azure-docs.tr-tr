---
title: Windows Server için Azure Hibrit Avantajı
description: Azure'a şirket içi lisanslar getirmek için Windows Yazılım Güvencesi avantajlarınızı nasıl en üst düzeye çıkarınızı öğrenin
author: xujing-ms
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 4/22/2018
ms.author: xujing
ms.openlocfilehash: f84d4fcd85f1e718f414e63bbe76fd29fa32427d
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869576"
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure Hibrit Avantajı
Yazılım Güvencesi olan müşteriler için, Windows Server için Azure Karma Avantajı, şirket içi Windows Server lisanslarınızı kullanmanıza ve Windows sanal makinelerini Azure'da daha düşük bir maliyetle çalıştırmanıza olanak tanır. Windows Işletim Sistemi ile yeni sanal makineler dağıtmak için Windows Server için Azure Karma Avantajı'nı kullanabilirsiniz. Bu makale, Windows Server için Azure Karma Avantajı ile yeni VM'lerin nasıl dağıtılanabildiği ve varolan çalışan VM'leri nasıl güncelleştirebileceğiniz le ilgili adımların üzerinden geçilir. Windows Server lisanslama ve maliyet tasarrufu için Azure Karma Avantajı hakkında daha fazla bilgi için [Windows Server lisanslama için Azure Karma Avantajı sayfasına](https://azure.microsoft.com/pricing/hybrid-use-benefit/)bakın.

Her 2 işlemcili lisans veya 16 çekirdekli lisansların her kümesi, 8 ana'ya kadar iki örnek veya 16 çeki ye kadar bir örnek hakkına sahiptir. Standart Sürüm lisansları için Azure Karma Avantajı yalnızca şirket içinde veya Azure'da bir kez kullanılabilir. Datacenter Edition avantajları, hem şirket içinde hem de Azure'da eşzamanlı kullanıma olanak tanır.

Windows Server işletim sistemi çalıştıran herhangi bir VM ile Windows Server için Azure Karma Avantajı'nın kullanılması artık SQL Server veya üçüncü taraf pazar yeri yazılımı gibi ek yazılımlarla birlikte VM'ler de dahil olmak üzere tüm bölgelerde desteklenir. 


## <a name="classic-vms"></a>Klasik VM'ler

Klasik VM'ler için, yalnızca şirket içinde yeni VM dağıtmaözel görüntüler desteklenir. Bu makalede desteklenen özelliklerden yararlanmak için öncelikle klasik VM'leri Kaynak Yöneticisi modeline geçirmeniz gerekir.

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]
 

## <a name="ways-to-use-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure Karma Avantajını kullanmanın yolları
Azure Karma Avantajı ile Windows sanal makinelerini kullanmanın birkaç yolu vardır:

1. Azure Marketi'nde sağlanan Windows Server görüntülerinden birinden VM dağıtabilirsiniz
2. Özel bir VM yükleyebilir ve Kaynak Yöneticisi şablonu veya Azure PowerShell kullanarak dağıtabilirsiniz
3. Azure Karma Avantajı ile çalışma arasında mevcut VM'yi geçiş yapabilir ve dönüştürebilir veya Windows Server için isteğe bağlı maliyet ödeyebilirsiniz
4. Windows Server için Azure Karma Avantajı'nı sanal makine ölçeği kümesinde de uygulayabilirsiniz


## <a name="create-a-vm-with-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure Karma Avantajı ile VM oluşturma
Windows Server işletim sistemi tabanlı tüm görüntüler, Windows Server için Azure Karma Avantajı için desteklenir. Azure platformu destek resimlerini kullanabilir veya kendi özel Windows Server resimlerinizi yükleyebilirsiniz. 

### <a name="portal"></a>Portal
Windows Server için Azure Karma Avantajı içeren bir VM oluşturmak için "Paradan tasarruf et" bölümünün altındaki geçişi kullanın.

### <a name="powershell"></a>PowerShell


```powershell
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -ImageName "Win2016Datacenter" `
    -LicenseType "Windows_Server"
```

### <a name="cli"></a>CLI
```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --license-type Windows_Server
```

### <a name="template"></a>Şablon
Kaynak Yöneticisi şablonlarınızda ek bir `licenseType` parametre belirtilmelidir. [Azure Kaynak Yöneticisi şablonlarını yazma](../../resource-group-authoring-templates.md) hakkında daha fazla bilgi edinebilirsiniz
```json
"properties": {
    "licenseType": "Windows_Server",
    "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
    }
```

## <a name="convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure Karma Avantajı'nı kullanarak varolan bir VM'yi dönüştürme
Windows Server için Azure Karma Avantajı'ndan yararlanmak için dönüştürmek istediğiniz mevcut bir VM'iniz varsa, aşağıdaki yönergeleri izleyerek VM'nizin lisans türünü güncelleştirebilirsiniz.

> [!NOTE]
> VM'deki lisans türünü değiştirmek, sistemin yeniden başlatılmasına veya bir hizmet interuptionine neden olmaz.  Bu yalnızca meta veri bayrağıiçin yapılan bir güncelleştirmedir.
> 

### <a name="portal"></a>Portal
Portal VM blade'den ,"Yapılandırma" seçeneğini seçerek ve "Azure karma avantajı" seçeneğini geçiş yaparak Azure Karma Avantajı'nı kullanacak şekilde VM'yi güncelleyebilirsiniz

### <a name="powershell"></a>PowerShell
- Varolan Windows Server VM'lerini Windows Server için Azure Karma Avantajı'na dönüştürme

    ```powershell
    $vm = Get-AzVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "Windows_Server"
    Update-AzVM -ResourceGroupName rg-name -VM $vm
    ```
    
- Windows Server VM'leri avantajla birlikte öde'ye dönüştürün

    ```powershell
    $vm = Get-AzVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "None"
    Update-AzVM -ResourceGroupName rg-name -VM $vm
    ```
    
### <a name="cli"></a>CLI
- Varolan Windows Server VM'lerini Windows Server için Azure Karma Avantajı'na dönüştürme

    ```azurecli
    az vm update --resource-group myResourceGroup --name myVM --set licenseType=Windows_Server
    ```

### <a name="how-to-verify-your-vm-is-utilizing-the-licensing-benefit"></a>VM'nizin lisans avantajından yararlandığını doğrulama
VM'inizi PowerShell, Resource Manager şablonu veya portalı aracılığıyla dağıttıktan sonra, ayarı aşağıdaki yöntemlerle doğrulayabilirsiniz.

### <a name="portal"></a>Portal
Portal VM blade'den" Yapılandırma" sekmesini seçerek Windows Server için Azure Karma Avantajı geçişini görüntüleyebilirsiniz.

### <a name="powershell"></a>PowerShell
Aşağıdaki örnekte, tek bir VM'nin lisans türünü gösterir
```powershell
Get-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Çıktı:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Bu çıktı, Windows Server lisanslama için Azure Karma Avantajı olmadan dağıtılan aşağıdaki VM ile tezat oluşturuyor:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

### <a name="cli"></a>CLI
```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVM --query "[?licenseType=='Windows_Server']" -o table
```

> [!NOTE]
> VM'deki lisans türünü değiştirmek, sistemin yeniden başlatılmasına veya bir hizmet interuptionine neden olmaz. Yalnızca meta veri lisanslama bayrağıdır.
>

## <a name="list-all-vms-with-azure-hybrid-benefit-for-windows-server-in-a-subscription"></a>Windows Server için Azure Karma Avantajı olan tüm VM'leri abonelikte listele
Windows Server için Azure Karma Avantajı ile dağıtılan tüm sanal makineleri görmek ve saymak için aboneliğinizden aşağıdaki komutu çalıştırabilirsiniz:

### <a name="portal"></a>Portal
Sanal Makine veya Sanal makine ölçeği kaynak bıçağı kümelerinden, tablo sütununu "Azure Karma Avantajı" içerecek şekilde yapılandırarak tüm VM(ler) ve lisans türünüzün listesini görüntüleyebilirsiniz. VM ayarı "Etkin", "Etkin değil" veya "Desteklenmeyen" durumunda olabilir.

### <a name="powershell"></a>PowerShell
```powershell
$vms = Get-AzVM
$vms | ?{$_.LicenseType -like "Windows_Server"} | select ResourceGroupName, Name, LicenseType
```

### <a name="cli"></a>CLI
```azurecli
az vm list --query "[?licenseType=='Windows_Server']" -o table
```

## <a name="deploy-a-virtual-machine-scale-set-with-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure Karma Avantajı ile Sanal Makine Ölçeği Seti dağıtma
Sanal makine ölçeği ayar Kaynak Yöneticisi şablonları `licenseType` içinde, VirtualMachineProfile özelliğiiçinde ek bir parametre belirtilmelidir. Bunu, ARM şablonu, PowerShell, Azure CLI veya REST aracılığıyla ölçek kümeniz için oluşturma veya güncelleştirme sırasında yapabilirsiniz.

Aşağıdaki örnekte, Windows Server 2016 Datacenter görüntüsüne sahip ARM şablonu kullanılır:
```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```
Ayrıca, ölçek kümenizi güncelleştirmenin daha fazla yolu için [sanal makine ölçeği kümesini](../../virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set.md) nasıl değiştirebileceğiniz hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Karma Avantajı ile nasıl tasarruf edebilirsiniz](https://azure.microsoft.com/pricing/hybrid-use-benefit/) hakkında daha fazla bilgi edinin
- [Azure Karma Avantajı için sık sorulan sorular](https://azure.microsoft.com/pricing/hybrid-use-benefit/faq/) hakkında daha fazla bilgi edinin
- [Windows Server lisanslama için Azure Karma Avantajı hakkında daha fazla](https://docs.microsoft.com/windows-server/get-started/azure-hybrid-benefit) bilgi edinin ayrıntılı kılavuz
- Windows Server ve Azure Site Kurtarma için Azure Karma Avantajı hakkında daha fazla bilgi edinin, [azure'a geçiş yapan uygulamaları daha da düşük maliyetli hale getirin](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/)
- [Multitenant Hosting Right ile Azure'da Windows 10](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) hakkında daha fazla bilgi edinin
- [Kaynak Yöneticisi şablonlarını kullanma](../../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin
