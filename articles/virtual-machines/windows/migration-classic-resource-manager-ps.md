---
title: PowerShell ile Kaynak Yöneticisine geçiş
description: Bu makale, Azure PowerShell komutlarını kullanarak sanal makineler (VM'ler), sanal ağlar ve klasikten Azure Kaynak Yöneticisi'ne depolama hesapları gibi IaaS kaynaklarının platform destekli geçişinden geçer
author: tanmaygore
manager: vashan
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: 314d7a4725709f00ba5cdbf54595857502bc5805
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81865955"
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-powershell"></a>PowerShell'i kullanarak IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirin

> [!IMPORTANT]
> Bugün, IaaS VM'lerinin yaklaşık %90'ı [Azure Kaynak Yöneticisi](https://azure.microsoft.com/features/resource-manager/)kullanıyor. 28 Şubat 2020 itibariyle, klasik VM'ler amortismana kaldırılmıştır ve 1 Mart 2023'te tamamen emekliye ayırılacaktır. Bu amortisman ve sizi [nasıl etkilediği](https://docs.microsoft.com/azure/virtual-machines/classic-vm-deprecation#how-does-this-affect-me)hakkında [daha fazla bilgi edinin.]( https://aka.ms/classicvmretirement)

Bu adımlar, altyapıyı hizmet (IaaS) kaynaklarını klasik dağıtım modelinden Azure Kaynak Yöneticisi dağıtım modeline geçirmek için Azure PowerShell komutlarını nasıl kullanacağınızı gösterir.

İsterseniz, [Azure CLI'yi](../linux/migration-classic-resource-manager-cli.md)kullanarak kaynakları da geçirebilirsiniz.

* Desteklenen geçiş senaryoları hakkında arka plan için, [IaaS kaynaklarının Klasik'ten Azure Kaynak Yöneticisi'ne Platform destekli geçişine](migration-classic-resource-manager-overview.md)bakın.
* Ayrıntılı rehberlik ve geçiş walkthrough için, [klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişte Teknik derin dalış'a](migration-classic-resource-manager-deep-dive.md)bakın.
* [En yaygın geçiş hatalarını gözden geçirin.](migration-classic-resource-manager-errors.md)

<br>
Geçiş işlemi sırasında adımların yürütülmesi gereken sırayı tanımlamak için bir akış şeması aşağıda veda edebilirsiniz.

![Geçiş adımlarını gösteren ekran görüntüsü](media/migration-classic-resource-manager/migration-flow.png)

 

## <a name="step-1-plan-for-migration"></a>Adım 1: Geçiş planı
IaaS kaynaklarını klasikten Kaynak Yöneticisi'ne geçirip geçirmemenizi değerlendirirken önerdiğimiz en iyi birkaç uygulama şunlardır:

* [Desteklenen ve desteklenmeyen özellikleri ve yapılandırmaları](migration-classic-resource-manager-overview.md)okuyun. Desteklenmeyen yapılandırmalar veya özellikler kullanan sanal makineleriniz varsa, yapılandırma veya özellik desteğinin duyurulmasını bekleyin. Alternatif olarak, gereksinimlerinize uygunsa, geçişi etkinleştirmek için bu özelliği kaldırın veya bu yapılandırmanın dışına taşıyın.
* Bugün altyapınızı ve uygulamalarınızı dağıtan otomatik komut dosyalarınız varsa, geçiş için bu komut dosyalarını kullanarak benzer bir test kurulumu oluşturmaya çalışın. Alternatif olarak, Azure portalını kullanarak örnek ortamlar ayarlayabilirsiniz.

> [!IMPORTANT]
> Uygulama ağ geçitleri şu anda klasikten Kaynak Yöneticisi'ne geçiş için desteklenmez. Bir uygulama ağ geçidi olan sanal bir ağı geçirmek için, ağı taşımak için bir hazırlama işlemi çalıştırmadan önce ağ geçidini kaldırın. Geçişi tamamladıktan sonra, Azure Kaynak Yöneticisi'ndeki ağ geçidini yeniden bağlayın.
>
> Başka bir abonelikteki ExpressRoute devrelerine bağlanan Azure ExpressRoute ağ geçitleri otomatik olarak geçirilemiyor. Bu gibi durumlarda, ExpressRoute ağ geçidini kaldırın, sanal ağı geçirin ve ağ geçidini yeniden oluşturun. Daha fazla bilgi için, [klasikten Kaynak Yöneticisi dağıtım modeline geçiş ExpressRoute devreleri ve ilişkili sanal ağlara](../../expressroute/expressroute-migration-classic-resource-manager.md)bakın.

## <a name="step-2-install-the-latest-version-of-powershell"></a>Adım 2: PowerShell'in en son sürümünü yükleyin
Azure PowerShell'i yüklemek için iki ana seçenek vardır: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) veya [Web Platform Installer (WebPI)](https://aka.ms/webpi-azps). WebPI aylık güncelleştirmeler alır. PowerShell Gallery sürekli olarak güncellemeleri alır. Bu makale, Azure PowerShell sürüm 2.1.0'ı temel alınr.

Yükleme yönergeleri için [Azure PowerShell'in nasıl yüklenir ve yapılandırılabildiğini](/powershell/azure/overview)öğrenin.

## <a name="step-3-ensure-that-youre-an-administrator-for-the-subscription"></a>Adım 3: Abonelik için yönetici olduğunuzdan emin olun
Bu geçişi gerçekleştirmek için, [Azure portalındaki](https://portal.azure.com)abonelik için yardımcı yönetici olarak eklenmeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Hub** menüsünde **Abonelik'i**seçin. Görmüyorsanız, **Tüm hizmetler'i**seçin.
3. Uygun abonelik girişini bulun ve ardından **MY ROLE** alanına bakın. Bir yardımcı yönetici için değer _Hesap yöneticisi_olmalıdır.

Bir yardımcı yönetici ekleyemediyseniz, ekinizin eklenmesi için bir hizmet yöneticisine veya yardımcı yöneticiye başvurun.

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a>Adım 4: Aboneliğinizi ayarlayın ve geçiş için kaydolun
İlk olarak, bir PowerShell istemi başlatın. Geçiş için ortamınızı hem klasik hem de Kaynak Yöneticisi için ayarlayın.

Kaynak Yöneticisi modeli için hesabınızda oturum açın.

```powershell
    Connect-AzAccount
```

Aşağıdaki komutu kullanarak kullanılabilir abonelikleri alın:

```powershell
    Get-AzSubscription | Sort Name | Select Name
```

Geçerli oturum için Azure aboneliğinizi ayarlayın. Bu örnekte varsayılan abonelik adı **Azure Aboneliğim**olarak ayarlanır. Örnek abonelik adını kendi adınızla değiştirin.

```powershell
    Select-AzSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> Kayıt tek seferlik bir adımdır, ancak geçiş girişiminde bulunmadan önce bunu bir kez yapmanız gerekir. Kaydolmadan aşağıdaki hata iletisini görürsünüz:
>
> *BadRequest : Abonelik geçiş için kayıtlı değildir.*

Aşağıdaki komutu kullanarak geçiş kaynak sağlayıcısına kaydolun:

```powershell
    Register-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Kaydın bitmesi için beş dakika bekleyin. Aşağıdaki komutu kullanarak onay durumunu kontrol edin:

```powershell
    Get-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Devam etmeden önce `Registered` RegistrationState'in olduğundan emin olun.

Klasik dağıtım modeline geçmeden önce, geçerli dağıtımınızın veya sanal ağınızın Azure bölgesinde yeterli Azure Kaynak Yöneticisi sanal makine vCPU'nuz olduğundan emin olun. Azure Kaynak Yöneticisi'nde mevcut vCPUs sayısını denetlemek için aşağıdaki PowerShell komutunu kullanabilirsiniz. vCPU kotaları hakkında daha fazla bilgi edinmek için [Sınırlar ve Azure Kaynak Yöneticisi'ne](../../azure-resource-manager/management/azure-subscription-service-limits.md#managing-limits)bakın.

Bu örnek, Batı **ABD** bölgesindeki kullanılabilirliği denetler. Örnek bölge adını kendi bölgenizle değiştirin.

```powershell
    Get-AzVMUsage -Location "West US"
```

Şimdi, klasik dağıtım modeli için hesabınızda oturum açın.

```powershell
    Add-AzureAccount
```

Aşağıdaki komutu kullanarak kullanılabilir abonelikleri alın:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Geçerli oturum için Azure aboneliğinizi ayarlayın. Bu örnekte Azure **Aboneliğim**için varsayılan abonelik ayarlanır. Örnek abonelik adını kendi adınızla değiştirin.

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```


## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Adım 5: IaaS kaynaklarınızı geçirmek için komutları çalıştırın
* [Sanal M'leri bulut hizmetine geçirme (sanal ağda değil)](#step-51-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network)
* [Sanal ağda VM'leri geçirme](#step-51-option-2---migrate-virtual-machines-in-a-virtual-network)
* [Depolama hesabını geçirme](#step-52-migrate-a-storage-account)

> [!NOTE]
> Burada açıklanan tüm işlemler idempotent vardır. Desteklenmeyen bir özellik veya yapılandırma hatası dışında bir sorununuz varsa, işlemi hazırlamayı, iptal etmeyi veya işleme yi yeniden denemenizi öneririz. Platform daha sonra eylemi yeniden dener.


### <a name="step-51-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Adım 5.1: Seçenek 1 - Sanal makineleri bulut hizmetinde geçirin (sanal ağda değil)
Aşağıdaki komutu kullanarak bulut hizmetlerinin listesini alın. Ardından, geçirmek istediğiniz bulut hizmetini seçin. Bulut hizmetindeki VM'ler sanal ağdaysa veya web veya çalışan rolleri varsa, komut bir hata iletisi döndürür.

```powershell
    Get-AzureService | ft Servicename
```

Bulut hizmetinin dağıtım adını alın. Bu örnekte, hizmet adı **Hizmetim'dir.** Örnek hizmet adını kendi hizmet adınız ile değiştirin.

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Bulut hizmetindeki sanal makineleri geçiş için hazırlayın. Aralarından seçim yapabileceğiniz iki seçeneğiniz var.

* **Seçenek 1: VM'leri platform tarafından oluşturulan sanal ağa geçirin.**

    İlk olarak, aşağıdaki komutları kullanarak bulut hizmetini geçirebileceğinizi doğrulayın:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Aşağıdaki komut, geçişi engelleyen tüm uyarıları ve hataları görüntüler. Doğrulama başarılı olursa, Hazırla adımına geçebilirsiniz.

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* **Seçenek 2: Kaynak Yöneticisi dağıtım modelinde varolan bir sanal ağa geçiş.**

    Bu örnek, kaynak grubu adını **myResourceGroup'a,** sanal ağ adını **myVirtualNetwork'e**ve alt ağ adını **mySubNet'e**ayarlar. Örnekteki adları kendi kaynaklarınızın adlarıyla değiştirin.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    İlk olarak, aşağıdaki komutu kullanarak sanal ağı geçirebileceğinizi doğrulayın:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Aşağıdaki komut, geçişi engelleyen tüm uyarıları ve hataları görüntüler. Doğrulama başarılı olursa, aşağıdaki Hazırlama adımı ile devam edebilirsiniz:

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

Hazırlama işlemi önceki seçeneklerden biriyle başarılı olduktan sonra, VM'lerin geçiş durumunu sorgulayın. `Prepared` Eyalette olduklarından emin olun.

Bu örnek, VM adını **myVM**olarak ayarlar. Örnek adı kendi VM adınız ile değiştirin.

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

PowerShell veya Azure portalını kullanarak hazırlanan kaynakların yapılandırmasını denetleyin. Geçiş için hazır değilseniz ve eski duruma dönmek istiyorsanız, aşağıdaki komutu kullanın:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Hazırlanan yapılandırma iyi görünüyorsa, aşağıdaki komutu kullanarak ileriye taşıyabilir ve kaynakları adaabilirsiniz:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="step-51-option-2---migrate-virtual-machines-in-a-virtual-network"></a>Adım 5.1: Seçenek 2 - Sanal ağdaki sanal makineleri geçirin

Sanal makineleri sanal ağda geçirmek için sanal ağı geçirebilirsiniz. Sanal makineler sanal ağ ile otomatik olarak geçiş. Geçirmek istediğiniz sanal ağı seçin.
> [!NOTE]
> Sanal makinenin VHD (OS ve veri) dosyalarını kullanarak Yönetilen Diskler ile yeni bir Kaynak Yöneticisi sanal makine oluşturarak klasik dağıtım modeli kullanılarak oluşturulan [tek bir sanal makineyi geçirin.](migrate-single-classic-to-resource-manager.md)
<br>

> [!NOTE]
> Sanal ağ adı, yeni portalda gösterilenden farklı olabilir. Yeni Azure portalı, adı `[vnet-name]`, ancak gerçek sanal ağ `Group [resource-group-name] [vnet-name]`adı türünde olarak görüntüler. Geçişe başlamadan önce, komutu `Get-AzureVnetSite | Select -Property Name` kullanarak gerçek sanal ağ adını arayın veya eski Azure portalında görüntüleyin. 

Bu örnek, sanal ağ adını **myVnet**olarak ayarlar. Örnek sanal ağ adını kendi adınızla değiştirin.

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> Sanal ağ web veya çalışan rolleri veya desteklenmeyen yapılandırmaları olan VM'ler içeriyorsa, bir doğrulama hatası iletisi alırsınız.

İlk olarak, aşağıdaki komutu kullanarak sanal ağı geçirebileceğinizi doğrulayın:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Aşağıdaki komut, geçişi engelleyen tüm uyarıları ve hataları görüntüler. Doğrulama başarılı olursa, aşağıdaki Hazırlama adımı ile devam edebilirsiniz:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Azure PowerShell veya Azure portalını kullanarak hazırlanan sanal makinelerin yapılandırmasını kontrol edin. Geçiş için hazır değilseniz ve eski duruma dönmek istiyorsanız, aşağıdaki komutu kullanın:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Hazırlanan yapılandırma iyi görünüyorsa, aşağıdaki komutu kullanarak ileriye taşıyabilir ve kaynakları adaabilirsiniz:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="step-52-migrate-a-storage-account"></a>Adım 5.2: Depolama hesabını geçirme
Sanal makineleri geçirmeyi bitirdikten sonra, depolama hesaplarını geçirmeden önce aşağıdaki ön koşul denetimlerini gerçekleştirin.

> [!NOTE]
> Depolama hesabınızda ilişkili diskler veya VM verileri yoksa, doğrudan "Depolama hesaplarını doğrula ve geçişbaşlatmayı başlat" bölümüne atlayabilirsiniz.

* Herhangi bir VM'yi geçirip geçirmediğinizi veya depolama hesabınızda disk kaynakları varsa ön koşul denetimleri:
    * Diskleri depolama hesabında depolanan sanal makineleri geçirin.

        Aşağıdaki komut, depolama hesabındaki tüm VM disklerinin RoleName ve DiskName özelliklerini döndürür. RoleName, bir diskin bağlı olduğu sanal makinenin adıdır. Bu komut diskleri döndürürse, depolama hesabını geçirmeden önce bu disklerin bağlı olduğu sanal makinelerin geçirildiğine emin olun.
        ```powershell
         $storageAccountName = 'yourStorageAccountName'
          Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
          DiskName | Format-List -Property RoleName, DiskName

        ```
    * Depolama hesabında depolanan eklenmemiş VM disklerini silin.

        Aşağıdaki komutu kullanarak depolama hesabında bekar VM diskleri bulun:

        ```powershell
            $storageAccountName = 'yourStorageAccountName'
            Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

        ```
        Önceki komut diskleri döndürürse, aşağıdaki komutu kullanarak bu diskleri silin:

        ```powershell
           Remove-AzureDisk -DiskName 'yourDiskName'
        ```
    * Depolama hesabında depolanan VM görüntülerini silin.

        Aşağıdaki komut, depolama hesabında depolanan işletim sistemi diskleri ile tüm VM görüntülerini döndürür.
         ```powershell
            Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                    } | Select-Object -Property ImageName, ImageLabel
         ```
         Aşağıdaki komut, depolama hesabında depolanan veri diskleri ile tüm VM görüntülerini döndürür.
         ```powershell

            Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                             -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                            } | Select-Object -Property ImageName, ImageLabel
         ```
        Bu komutu kullanarak önceki komutlar tarafından döndürülen tüm VM görüntülerini silin:
        ```powershell
        Remove-AzureVMImage -ImageName 'yourImageName'
        ```
* Depolama hesaplarını doğrulayın ve geçişbaşlatmayı başlatın.

    Aşağıdaki komutu kullanarak geçiş için her depolama hesabını doğrulayın. Bu örnekte, depolama hesabı adı **myStorageAccount'tır.** Örnek adı kendi depolama hesabınızın adı ile değiştirin.

    ```powershell
        $storageAccountName = "myStorageAccount"
        Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
    ```

    Bir sonraki adım, depolama hesabını geçiş için hazırlamaktır.

    ```powershell
        $storageAccountName = "myStorageAccount"
        Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
    ```

    Azure PowerShell veya Azure portalını kullanarak hazırlanan depolama hesabı yapılandırmasını denetleyin. Geçiş için hazır değilseniz ve eski duruma dönmek istiyorsanız, aşağıdaki komutu kullanın:

    ```powershell
        Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
    ```

    Hazırlanan yapılandırma iyi görünüyorsa, aşağıdaki komutu kullanarak ileriye taşıyabilir ve kaynakları adaabilirsiniz:

    ```powershell
        Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişine genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirmek için CLI'yi kullanın](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne geçişine yardımcı olmak için topluluk araçları](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirme yle ilgili en sık sorulan soruları gözden geçirin](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
