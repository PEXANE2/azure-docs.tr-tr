---
title: Azure PowerShell aracılığıyla Azure Stack Edge GPU cihazınızda VM 'Leri dağıtma
description: Azure PowerShell kullanarak bir Azure Stack Edge cihazında sanal makinelerin (VM 'Ler) nasıl oluşturulacağını ve yönetileceğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 6e776b6dfc233ffb12d3597a0e6bc203f1674abd
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89147074"
---
# <a name="deploy-vms-on-your-azure-stack-edge-device-via-azure-powershell-script"></a>Azure PowerShell betiği aracılığıyla Azure Stack Edge cihazınızda VM 'Leri dağıtma

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Bu öğretici, bir Azure PowerShell betiği kullanarak Azure Stack Edge cihazınızda bir sanal makinenin nasıl oluşturulduğunu ve yönetileceğini açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu betiği kullanarak Azure Stack Edge cihazınızda bir VM oluşturmaya ve yönetmeye başlamadan önce, aşağıdaki adımlarda listelenen önkoşulları tamamladığınızdan emin olmanız gerekir:

### <a name="for-azure-stack-edge-device-via-the-local-web-ui"></a>Yerel Web Kullanıcı arabirimi aracılığıyla Azure Stack Edge cihazı için

1. Azure Stack Edge cihazındaki ağ ayarlarını [Adım 1: yapılandırma Azure Stack Edge cihazını](azure-stack-edge-j-series-connect-resource-manager.md#step-1-configure-azure-stack-edge-device)açıklandığı gibi tamamladınız.

2. İşlem için bir ağ arabirimi etkinleştirildi. Bu ağ arabirimi IP 'si, VM dağıtımı için bir sanal anahtar oluşturmak üzere kullanılır. Aşağıdaki adımlar süreç boyunca size yol gösterir:

    1. **İşlem ayarları**' na gidin. Sanal anahtar oluşturmak için kullanacağınız ağ arabirimini seçin.

        > [!IMPORTANT] 
        > İşlem için yalnızca bir bağlantı noktası yapılandırabilirsiniz.

    2. Ağ arabiriminde işlem etkinleştirin. Azure Stack Edge, bu ağ arabirimine karşılık gelen bir sanal anahtar oluşturur ve yönetir.

3. Azure Stack Edge cihazınızda ve istemcinizin güvenilen kök deposunda tüm sertifikaları oluşturdunuz ve yüklediniz. [2. Adım: sertifika oluşturma ve yüklemede](azure-stack-edge-j-series-connect-resource-manager.md#step-2-create-and-install-certificates)açıklanan yordamı izleyin.

### <a name="for-your-windows-client"></a>Windows istemciniz için

1. Cihazın yerel Web Kullanıcı arabirimindeki **ağ** sayfanızda Azure ile tutarlı hizmetler sanal internet protokolünü (VIP) tanımladınız. Bu VIP 'yi şu şekilde eklemeniz gerekir:

    - İstemci üzerindeki konak dosyası veya,
    - DNS sunucusu yapılandırması
    
    > [!IMPORTANT]
    > Uç nokta adı çözümlemesi için DNS sunucusu yapılandırmasını değiştirmenizi öneririz.

    1. **Not defteri 'ni** yönetici olarak başlatın (dosyayı kaydetmek için yönetici ayrıcalıkları gereklidir) ve ardından konumunda bulunan **Hosts** dosyasını açın `C:\Windows\System32\Drivers\etc` .
    
        ![Windows Gezgini ana bilgisayarları dosyası](media/azure-stack-edge-j-series-connect-resource-manager/hosts-file.png)
    
    2. Aşağıdaki girdileri, cihazınız için uygun değerlerle değiştirerek **ana bilgisayar** dosyanıza ekleyin:
    
        ```
        <Azure consistent services VIP> login.<appliance name>.<DNS domain>
        <Azure consistent services VIP> management.<appliance name>.<DNS domain>
        <Azure consistent services VIP> <storage name>.blob.<appliance name>.<DNS domain>
        ```
        Depolama hesabı için, yeni bir depolama hesabı oluşturmak üzere betiğin daha sonra kullanmasını istediğiniz bir ad sağlayabilirsiniz. Komut dosyası, depolama hesabının mevcut olup olmadığını denetlemez.

    3. Başvuru için aşağıdaki görüntüyü kullanın. **Hosts** dosyasını kaydedin.

        ![Not defteri 'nde Hosts dosyası](media/azure-stack-edge-j-series-deploy-virtual-machine-cli-python/hosts-screenshot-boxed.png)

2. Bu yordamda kullanılan [PowerShell betiğini indirin](https://aka.ms/ase-vm-powershell) .

3. Windows istemcinizin PowerShell 5,0 veya sonraki bir sürümü çalıştırdığından emin olun.

4. `Azure.Storage Module version 4.5.0`' Nin sisteminizde yüklü olduğundan emin olun. Bu modülü [PowerShell Galerisi](https://www.powershellgallery.com/packages/Azure.Storage/4.5.0)edinebilirsiniz. Bu modülü yüklemek için şunu yazın:

    `Install-Module -Name Azure.Storage -RequiredVersion 4.5.0`

    Yüklü modülün sürümünü doğrulamak için şunu yazın:

    `Get-InstalledModule -name Azure.Storage`

    Diğer sürüm modüllerini kaldırmak için şunu yazın:

    `Uninstall-Module -Name Azure.Storage`

5. [AzCopy 10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10#download-azcopy) ' yı Windows istemcisine indirin. Betiği çalıştırırken bir parametre olarak geçitirsiniz, bu konumu unutmayın.

6. Windows istemcinizin TLS 1,2 veya sonraki bir sürümü çalıştırdığından emin olun.


## <a name="create-a-vm"></a>VM oluşturma

1. PowerShell'i yönetici olarak çalıştırın.
2. İstemcinizi betiği indirdiğiniz klasöre gidin.  
3. Betiği çalıştırmak için aşağıdaki komutu kullanın:
 
    `.\ArmPowershellClient.ps1 -VNetAddressSpace <AddressSpace> -NicPrivateIp <Private IP> -VHDPath <Path> -VHDFile <VHD File, with extension> -StorageAccountName <Name> -OS <Windows/Linux> -VMSize <Supported VM Size> -VMUserName <UserName to be used to login into VM> -VMPassword <Password for the VM login> --AzCopy10Path <Absolute Path>`

    Bir Windows VM ve bir Linux VM oluşturmak için betiğin çalıştırıldığı örnekler aşağıda verilmiştir.

    **Bir Windows sanal makinesi için:**

    `.\ArmPowershellClient.ps1 -VNetAddressSpace 5.5.0.0/16 -NicPrivateIp 5.5.168.73 -VHDPath \\intel01\d$\vm_vhds\AzureWindowsVMmode -VHDFile WindowsServer2016Datacenter.vhd -StorageAccountName teaaccount1 -OS Windows -VMSize Standard_D1_v2 -VMUserName Administrator -VMPassword Password1 -AzCopy10Path C:\azcopy10\azcopy.exe`

    **Bir Linux sanal makinesi için:**

    `.\ArmPowershellClient.ps1 -VNetAddressSpace 5.5.0.0/16 -NicPrivateIp 5.5.168.83 -VHDPath \\intel01\d$\vm_vhds\AzurestackLinux -VHDFile ubuntu13.vhd -StorageAccountName sa2 -OS Linux -VMSize Standard_D1_v2 -VMUserName Administrator -VMPassword Password1 -AzCopy10Path C:\azcopy10\azcopy.exe`
    
4. Betiğin oluşturduğu kaynakları temizlemek için aşağıdaki komutları kullanın:
    
    ```powershell
    Get-AzureRmVM | Remove-AzureRmVM -Force
    Get-AzureRmNetworkInterface | Remove-AzureRmNetworkInterface -Force
    Get-AzureRmResource | Remove-AzureRmResource -f
    Get-AzureRmResourceGroup | Remove-AzureRmResourceGroup -f
    ```


## <a name="next-steps"></a>Sonraki adımlar

[Azure PowerShell cmdlet 'lerini kullanarak VM 'Leri dağıtma](azure-stack-edge-j-series-deploy-virtual-machine-powershell.md)
