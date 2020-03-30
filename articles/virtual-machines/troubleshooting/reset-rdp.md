---
title: Uzak Masaüstü Hizmetlerini veya yönetici parolasını Windows VM'de sıfırlama | Microsoft Dokümanlar
description: Azure portalını veya Azure PowerShell'i kullanarak bir Windows VM'de hesap parolasını veya Uzak Masaüstü Hizmetlerini nasıl sıfırlayabileceğinizi öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/25/2019
ms.author: genli
ms.openlocfilehash: 580ec443dc087f270e30856c336a5699bbf1ae71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71058452"
---
# <a name="reset-remote-desktop-services-or-its-administrator-password-in-a-windows-vm"></a>Windows VM'de Uzak Masaüstü Hizmetlerini veya yönetici parolasını sıfırlama
Bir Windows sanal makinesine (VM) bağlanamıyorsanız, yerel yönetici parolanızı sıfırlayabilir veya Uzak Masaüstü Hizmetleri yapılandırmasını sıfırlayabilirsiniz (Windows etki alanı denetleyicilerinde desteklenmez). Parolayı sıfırlamak için Azure portalı veya Azure PowerShell'deki VM Erişimi uzantısını kullanın. VM'de oturum açtıktan sonra yerel yönetici parolasını sıfırlayın.  
PowerShell kullanıyorsanız, [en son PowerShell modülünün yüklendiğinden ve yapılandırıldığından](/powershell/azure/overview) ve Azure aboneliğinizde oturum açtığınızdan emin olun. Ayrıca [klasik dağıtım modeliyle oluşturulmuş olan VM'lerde bu adımları da gerçekleştirebilirsiniz](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

Uzak Masaüstü Hizmetleri ve kimlik bilgileri sıfırlamasını gerçekleştirmek için aşağıdaki yöntemleri kullanabilirsiniz:

- [Azure portalını kullanarak sıfırlama](#reset-by-using-the-azure-portal)

- [VMAccess uzantısı nı ve PowerShell'i kullanarak sıfırlama](#reset-by-using-the-vmaccess-extension-and-powershell)

## <a name="reset-by-using-the-azure-portal"></a>Azure portalını kullanarak sıfırlama

Önce [Azure portalında](https://portal.azure.com) oturum açın ve ardından sol menüdeki **Sanal makineleri** seçin. 

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parolasını sıfırlama**

1. Windows VM'nizi seçin ve ardından **Destek + Sorun Giderme**altında **parolayı sıfırla'yı** seçin. **Parolayı Sıfırla** penceresi görüntülenir.

2. **Parolayı Sıfırla'yı**seçin, bir kullanıcı adı ve parola girin ve ardından **Güncelleştir'i**seçin. 

3. VM'nize yeniden bağlanmayı deneyin.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Uzak Masaüstü Hizmetleri yapılandırmasını sıfırlama**

Bu işlem VM'de Uzak Masaüstü hizmetini etkinleştirecek ve varsayılan RDP bağlantı noktası 3389 için bir güvenlik duvarı kuralı oluşturur.

1. Windows VM'nizi seçin ve ardından **Destek + Sorun Giderme**altında **parolayı sıfırla'yı** seçin. **Parolayı Sıfırla** penceresi görüntülenir. 

2. **Yalnızca Yapılandırmayı Sıfırla'yı** seçin ve ardından **Güncelleştir'i**seçin. 

3. VM'nize yeniden bağlanmayı deneyin.

## <a name="reset-by-using-the-vmaccess-extension-and-powershell"></a>VMAccess uzantısı nı ve PowerShell'i kullanarak sıfırlama

İlk olarak, [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet'i kullanarak [en son PowerShell modülünü yüklediğinizden ve yapılandırdığınızdan](/powershell/azure/overview) ve Azure aboneliğinizde oturum açtığınızdan emin olun.

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parolasını sıfırlama**

- [Set-AzVMAccessExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmaccessextension) PowerShell cmdlet ile yönetici parolasını veya kullanıcı adını sıfırla. Sürüm `typeHandlerVersion` 1 amortismana kattığı için ayar 2.0 veya daha büyük olmalıdır. 

    ```powershell
    $SubID = "<SUBSCRIPTION ID>" 
    $RgName = "<RESOURCE GROUP NAME>" 
    $VmName = "<VM NAME>" 
    $Location = "<LOCATION>" 
 
    Connect-AzAccount 
    Select-AzSubscription -SubscriptionId $SubID 
    Set-AzVMAccessExtension -ResourceGroupName $RgName -Location $Location -VMName $VmName -Credential (get-credential) -typeHandlerVersion "2.0" -Name VMAccessAgent 
    ```

    > [!NOTE] 
    > VM'nizde geçerli yerel yönetici hesabından farklı bir ad girerseniz, VMAccess uzantısı bu ada sahip yerel bir yönetici hesabı ekler ve belirtilen parolanızı bu hesaba atar. VM'nizdeki yerel yönetici hesabı varsa, VMAccess uzantısı parolayı sıfırlar. Hesap devre dışı bırakılırsa, VMAccess uzantısı bunu etkinleştirecektir.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Uzak Masaüstü Hizmetleri yapılandırmasını sıfırlama**

1. [Set-AzVMAccessExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmaccessextension) PowerShell cmdlet ile VM'nize uzaktan erişimi sıfırla. Aşağıdaki örnek, kaynak grubunda `myVMAccess` adı geçen VM'de adı geçen `myVM` erişim uzantısını `myResourceGroup` sıfırlar:

    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
    ```

    > [!TIP]
    > Herhangi bir noktada, bir VM yalnızca tek bir VM erişim aracısı olabilir. VM erişim aracısı özelliklerini ayarlamak `-ForceRerun` için seçeneği kullanın. Kullandığınızda, `-ForceRerun`önceki komutlarda kullanmış olabileceğiniz VM erişim aracısı için aynı adı kullandığınızdan emin olun.

1. Sanal makinenize uzaktan bağlanamıyorsanız, Windows tabanlı bir Azure sanal makinesine sorun [giderme Uzak Masaüstü bağlantılarına](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın. Windows etki alanı denetleyicisine olan bağlantıyı kaybederseniz, etki alanı denetleyicisi yedeklemesinden geri yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM erişim uzantısı yanıt vermiyorsa ve parolayı sıfırlayamıyorsanız, [yerel Windows parolasını çevrimdışı sıfırlayabilirsiniz.](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Bu yöntem daha gelişmiştir ve sorunlu VM'nin sanal sabit diskini başka bir VM'ye bağlamanızı gerektirir. Önce bu makalede belgelenen adımları izleyin ve çevrimdışı parola sıfırlama yöntemini yalnızca bu adımlar işe yaramazsa dene.

- [Azure VM uzantıları ve özellikleri hakkında bilgi edinin.](../extensions/features-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

- [RDP veya SSH ile bir Azure sanal makinesine bağlanın.](https://msdn.microsoft.com/library/azure/dn535788.aspx)

- [Windows tabanlı bir Azure sanal makinesine Uzak Masaüstü bağlantılarını giderin.](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

