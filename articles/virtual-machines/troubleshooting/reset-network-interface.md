---
title: Azure Windows VM için ağ arabirimini sıfırlama| Microsoft Dokümanlar
description: Azure Windows VM için ağ arabirimini nasıl sıfırlarsınız gösterir
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 11/16/2018
ms.author: genli
ms.openlocfilehash: a8bd12d98b76d5848753987c4f7bcb76d4e2266d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250081"
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Azure Windows VM'de ağ arabirimini sıfırlama 

Bu makalede, Microsoft Azure Windows Sanal Makine'ye (VM) bağlandığınızda sorunları gidermek için Azure Windows VM'nin ağ arabirimini nasıl sıfırlayabileceğiniz gösterilmektedir:

* Varsayılan Ağ Arabirimini (NIC) devre dışı kılmış olursunuz. 
* NIC için statik bir IP'yi el ile ayarlarsınız. 

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="reset-network-interface"></a>Ağ arabirimini sıfırlama

### <a name="for-vms-deployed-in-resource-group-model"></a>Kaynak grup modelinde dağıtılan VM'ler için

1.  [Azure portalına](https://ms.portal.azure.com)gidin.
2.  Etkilenen Sanal Makine'yi seçin.
3.  **Ağ'ı** seçin ve ardından VM'nin ağ arabirimini seçin.

    ![Ağ arabirimi konumu](./media/reset-network-interface/select-network-interface-vm.png)
    
4.  **IP yapılandırmalarını**seçin.
5.  IP'yi seçin. 
6.  Özel **IP ataması** **Statik**değilse, **statik**olarak değiştirin.
7.  IP **adresini** Subnet'te bulunan başka bir IP adresiyle değiştirin.
8. Sanal makine, yeni NIC'yi sisteme başlatmak için yeniden başlatılacaktır.
9.  Makinenize RDP deneyin. Başarılı olursa, isterseniz Özel IP adresini orijinale geri değiştirebilirsiniz. Yoksa sende kalabilir. 

#### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

1. [En son Azure PowerShell yüklü](https://docs.microsoft.com/powershell/azure/overview) olduğundan emin olun
2. Yükseltilmiş bir Azure PowerShell oturumu açın (Yönetici olarak çalıştırın). Aşağıdaki komutları çalıştırın:

    ```powershell
    #Set the variables 
    $SubscriptionID = "<Subscription ID>"
    $VM = "<VM Name>"
    $ResourceGroup = "<Resource Group>"
    $VNET = "<Virtual Network>"
    $IP = "NEWIP"

    #Log in to the subscription 
    Add-AzAccount
    Select-AzSubscription -SubscriptionId $SubscriptionId 
    
    #Check whether the new IP address is available in the virtual network.
    Test-AzureStaticVNetIP –VNetName $VNET –IPAddress  $IP

    #Add/Change static IP. This process will not change MAC address
    Get-AzVM -ResourceGroupName $ResourceGroup -Name $VM | Set-AzureStaticVNetIP -IPAddress $IP | Update-AzVM
    ```
3. Makinenize RDP deneyin.  Başarılı olursa, isterseniz Özel IP adresini orijinale geri değiştirebilirsiniz. Yoksa sende kalabilir.

### <a name="for-classic-vms"></a>Klasik VM'ler için

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Ağ arabirimini sıfırlamak için aşağıdaki adımları izleyin:

#### <a name="use-azure-portal"></a>Azure portalı kullanma

1.  [Azure portalına]( https://ms.portal.azure.com)gidin.
2.  **Sanal Makineler (Klasik) seçin.**
3.  Etkilenen Sanal Makine'yi seçin.
4.  **IP adreslerini**seçin.
5.  Özel **IP ataması** **Statik**değilse, **statik**olarak değiştirin.
6.  IP **adresini** Subnet'te bulunan başka bir IP adresiyle değiştirin.
7.  **Kaydet'i**seçin.
8.  Sanal makine, yeni NIC'yi sisteme başlatmak için yeniden başlatılacaktır.
9.  Makinenize RDP deneyin. Başarılı olursa, Özel IP adresini orijinale geri döndürmeyi seçebilirsiniz.  

#### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

1. [En son Azure PowerShell yüklü](https://docs.microsoft.com/powershell/azure/overview) olduğundan emin olun.
2. Yükseltilmiş bir Azure PowerShell oturumu açın (Yönetici olarak çalıştırın). Aşağıdaki komutları çalıştırın:

    ```powershell
    #Set the variables 
    $SubscriptionID = "<Subscription ID>"
    $VM = "<VM Name>"
    $CloudService = "<Cloud Service>"
    $VNET = "<Virtual Network>"
    $IP = "NEWIP"

    #Log in to the subscription 
    Add-AzureAccount
    Select-AzureSubscription -SubscriptionId $SubscriptionId 

    #Check whether the new IP address is available in the virtual network.
    Test-AzureStaticVNetIP –VNetName $VNET –IPAddress  $IP
    
    #Add/Change static IP. This process will not change MAC address
    Get-AzureVM -ResourceGroupName $CloudService -Name $VM | Set-AzureStaticVNetIP -IPAddress $IP |Update-AzureVM
    ```
3. Makinenize RDP deneyin. Başarılı olursa, isterseniz Özel IP adresini orijinale geri değiştirebilirsiniz. Yoksa sende kalabilir. 

## <a name="delete-the-unavailable-nics"></a>Kullanılamayan NIC'leri silme
Masaüstünü makineye uzaktan kumanda ettikten sonra, olası bir sorunu önlemek için eski NIC'leri silmeniz gerekir:

1.  Cihaz Yöneticisi'ni açın.
2.  Gizli cihazları **Göster'i seçin.** > **Show hidden devices**
3.  **Ağ Bağdaştırıcılarını**seçin. 
4.  "Microsoft Hyper-V Ağ Bağdaştırıcısı" olarak adlandırılan bağdaştırıcıları denetleyin.
5.  Gri renkte kullanılamayan bir bağdaştırıcı görebilirsiniz. Bağdaştırıcıyı sağ tıklatın ve sonra Kaldır'ı seçin.

    ![NIC'nin görüntüsü](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Yalnızca "Microsoft Hyper-V Ağ Bağdaştırıcısı" adında kullanılamayan bağdaştırıcıları kaldırın. Diğer gizli bağdaştırıcılardan herhangi birini yüklerseniz, ek sorunlara neden olabilir.
    >
    >

6.  Artık kullanılamayan tüm bağdaştırıcılar sisteminizden temizlenmelidir.
