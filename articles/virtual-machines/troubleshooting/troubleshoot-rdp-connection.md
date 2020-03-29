---
title: AZURE'da BIR Windows VM'ye RDP ile bağlanamıyor | Microsoft Dokümanlar
description: Uzak Masaüstü'nü kullanarak Azure'daki Windows sanal makinenize bağlanamadığınızda sorunları giderme sorunları
keywords: Uzak masaüstü hatası, uzak masaüstü bağlantı hatası, VM,uzak masaüstü sorun giderme bağlanamıyor
services: virtual-machines-windows
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 0d740f8e-98b8-4e55-bb02-520f604f5b18
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/23/2018
ms.author: akjosh
ms.openlocfilehash: cbca8e631da8b99aa0ea4bdc6d099f3dbd2ed9b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77916617"
---
# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Azure sanal makinesine uzak masaüstü bağlantılarını giderme
Windows tabanlı Azure sanal makinenize (VM) olan Uzak Masaüstü Protokolü (RDP) bağlantısı çeşitli sebeplerle başarısız olabilir ve VM'nize erişememenize yol açabilir. Bu sorun VM'deki Uzak Masaüstü hizmetinden, ağ bağlantısından veya ana bilgisayarınızdaki Uzak Masaüstü istemcisinden kaynaklanabilir. Bu makale RDP bağlantı sorunlarını çözmeye yönelik en yaygın yöntemlerin bazılarında size yol gösterir. 

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.

 

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Hızlı sorun giderme adımları
Her sorun giderme adımından sonra VM'ye yeniden bağlanmayı deneyin:

1. Uzak Masaüstü yapılandırması sıfırla.
2. Ağ Güvenlik Grubu kurallarını / Bulut Hizmetleri bitiş noktalarını denetleyin.
3. VM konsol günlüklerini gözden geçirin.
4. VM için NIC'yi sıfırla.
5. VM Kaynak Durumu'nu kontrol edin.
6. VM parolanızı sıfırla.
7. VM'nizi yeniden başlatın.
8. VM'nizi yeniden dağıtın.

Daha ayrıntılı adımlar ve açıklamalar gerekiyorsa okumaya devam edin. Yönlendiriciler ve güvenlik duvarları gibi yerel ağ [donanımlarının, ayrıntılı RDP sorun giderme senaryolarında](detailed-troubleshoot-rdp.md)belirtildiği gibi giden TCP bağlantı noktası 3389'u engellemediğini doğrulayın.

> [!TIP]
> VM'nizin **Bağlan** düğmesi portalda gri renkteyse ve [Bir Ekspres Rota](../../expressroute/expressroute-introduction.md) veya Siteden Siteye VPN bağlantısı yla Azure'a bağlı değilseniz, RDP'yi kullanmadan önce VM'nizi genel bir IP adresi oluşturmanız ve atamanız gerekir. [Site-to-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) [Azure’da genel IP adresleri](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) hakkında daha fazlasını okuyabilirsiniz.


## <a name="ways-to-troubleshoot-rdp-issues"></a>RDP sorunlarını gidermenin yolları
Kaynak Yöneticisi dağıtım modelini kullanarak oluşturulan VM'leri aşağıdaki yöntemlerden birini kullanarak sorun giderebilirsiniz:

* Azure portalı - RDP yapılandırmasını veya kullanıcı kimlik bilgilerini hızlı bir şekilde sıfırlamanız gerekiyorsa ve Azure araçlarını yüklü yormuyorsanız harika.
* Azure PowerShell - PowerShell komut istemiyle rahatsanız, Azure PowerShell cmdlets'i kullanarak RDP yapılandırmasını veya kullanıcı kimlik bilgilerini hızlı bir şekilde sıfırlayın.

[Ayrıca, Klasik dağıtım modeli](#troubleshoot-vms-created-using-the-classic-deployment-model)kullanılarak oluşturulan sorun giderme vm'leri ile ilgili adımları da bulabilirsiniz.

<a id="fix-common-remote-desktop-errors"></a>

## <a name="troubleshoot-using-the-azure-portal"></a>Azure portalını kullanarak sorun giderme
Her sorun giderme adımından sonra VM'nize yeniden bağlanmayı deneyin. Hala bağlanamıyorsanız, bir sonraki adımı deneyin.

1. **RDP bağlantınızı sıfırla.** Bu sorun giderme adımı, Uzak Bağlantılar devre dışı bırakıldığında veya Windows Güvenlik Duvarı kuralları RDP'yi engellediğini zdettir, örneğin RDP yapılandırmasını sıfırlar.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini listenin en altına yakın **destek + sorun giderme** bölümüne kaydırın. **Parolayı Sıfırla** düğmesini tıklatın. **Yapılandırmayı sıfırlama** **modunu** ayarlayın ve ardından **Güncelleştir** düğmesini tıklatın:
   
    ![Azure portalında RDP yapılandırmasını sıfırlama](./media/troubleshoot-rdp-connection/reset-rdp.png)
2. **Ağ Güvenlik Grubu kurallarını doğrulayın.** [IP akışı doğrulamayı](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) kullanarak Ağ Güvenlik Grubu’ndaki bir kuralın bir sanal makineye giden veya gelen trafiği engelleyip engellemediğini doğrulayın. Gelen "İzin Ver" NSG kuralının var olduğundan ve RDP bağlantı noktası (varsayılan 3389) için öncelike olduğundan emin olmak için etkili güvenlik grubu kurallarını da gözden geçirebilirsiniz. Daha fazla bilgi için [VM trafik akışını gidermek için Etkili Güvenlik Kurallarını Kullanma'ya](../../virtual-network/diagnose-network-traffic-filter-problem.md)bakın.

3. **VM önyükleme tanılama gözden geçirin.** Bu sorun giderme adımı, VM'nin bir sorun bildirip bildirmeyini belirlemek için VM konsol günlüklerini gözden geçirir. Tüm VM'ler önyükleme tanılama etkin, bu nedenle bu sorun giderme adımı isteğe bağlı olabilir.
   
    Belirli sorun giderme adımları bu makalenin kapsamı dışındadır, ancak RDP bağlantısını etkileyen daha geniş bir soruna işaret edebilir. Konsol günlüklerini ve VM ekran görüntüsünü gözden geçirme hakkında daha fazla bilgi [için VM'ler için Önyükleme Tanılama'ya](boot-diagnostics.md)bakın.

4. **VM için NIC'yi sıfırla.** Daha fazla bilgi için [Azure Windows VM için NIC'yi nasıl sıfırlarısınız.](../windows/reset-network-interface.md)
5. **VM Kaynak Durumu'nu kontrol edin.** Bu sorun giderme adımı, Azure platformunda VM'ye bağlantıyı etkileyebilecek bilinen bir sorun olmadığını doğrular.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini listenin en altına yakın **destek + sorun giderme** bölümüne kaydırın. Kaynak **sistem durumu** düğmesini tıklatın. **Mevcut**olarak sağlıklı bir VM raporları:
   
    ![Azure portalında VM kaynak durumunu denetleme](./media/troubleshoot-rdp-connection/check-resource-health.png)
6. **Kullanıcı kimlik bilgilerini sıfırla.** Bu sorun giderme adımı, emin olmadığınız veya kimlik bilgilerini unuttuğunuz zaman parolayı yerel bir yönetici hesabında sıfırlar.  VM'ye giriş yaptıktan sonra, bu kullanıcının parolasını sıfırlamanız gerekir.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini listenin en altına yakın **destek + sorun giderme** bölümüne kaydırın. **Parolayı Sıfırla** düğmesini tıklatın. **Modun** **parolayı sıfırlamak** için ayarlandıklarına emin olun ve ardından kullanıcı adınızı ve yeni bir parola girin. Son olarak, **Güncelleştir** düğmesini tıklatın:
   
    ![Azure portalındaki kullanıcı kimlik bilgilerini sıfırlama](./media/troubleshoot-rdp-connection/reset-password.png)
7. **VM'nizi yeniden başlatın.** Bu sorun giderme adımı, VM'nin kendisinin yaşadığı temel sorunları düzeltebilir.
   
    Azure portalında VM'nizi seçin ve **Genel Bakış** sekmesini tıklatın. **Yeniden Başlat** düğmesini tıklatın:
   
    ![Azure portalında VM'yi yeniden başlatın](./media/troubleshoot-rdp-connection/restart-vm.png)
8. **VM'nizi yeniden dağıtın.** Bu sorun giderme adımı, temel platform veya ağ sorunlarını düzeltmek için VM'nizi Azure'daki başka bir ana bilgisayara yeniden dağır.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini listenin en altına yakın **destek + sorun giderme** bölümüne kaydırın. Yeniden **Dağıt** düğmesini tıklatın ve sonra **Yeniden Dağıt'ı**tıklatın:
   
    ![Azure portalında VM'yi yeniden dağıtma](./media/troubleshoot-rdp-connection/redeploy-vm.png)
   
    Bu işlem bittikten sonra, geçici disk verileri kaybolur ve VM ile ilişkili dinamik IP adresleri güncelleştirilir.

9. **Yönlendirmeyi doğrulayın.** Bir rotanın trafiğin sanal bir makineye veya sanal makineden yönlendirilmesine engel olmadığını doğrulamak için Ağ İzleyicisi'nin [Sonraki atlama](../../network-watcher/network-watcher-check-next-hop-portal.md) özelliğini kullanın. Ayrıca, ağ arabirimi için tüm etkili yolları görmek için etkili yolları gözden geçirebilirsiniz. Daha fazla bilgi için [vm trafik akışını gidermek için etkili yolları](../../virtual-network/diagnose-network-routing-problem.md)kullanma'ya bakın.

10. Bilgisayarınızdaki şirket içi güvenlik duvarının veya güvenlik duvarının, TCP 3389 trafiğinin Azure'a giden trafiğine izin verdiğinden emin olun.

RDP sorunlarıyla hala karşılaşıyorsanız, [bir destek isteği açabilir](https://azure.microsoft.com/support/options/) veya daha ayrıntılı [RDP sorun giderme kavramlarını ve adımlarını](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)okuyabilirsiniz.

## <a name="troubleshoot-using-azure-powershell"></a>Azure PowerShell'i kullanarak sorun giderme
Henüz yapmadıysanız, [en son Azure PowerShell'i yükleyin ve yapılandırıyorsunuz.](/powershell/azure/overview)

Aşağıdaki örneklerde , , `myResourceGroup` `myVM`ve `myVMAccessExtension`. gibi değişkenler kullanılır. Bu değişken adlarını ve konumlarını kendi değerlerinizle değiştirin.

> [!NOTE]
> [Set-AzVMAccessExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmaccessextension) PowerShell cmdlet'i kullanarak kullanıcı kimlik bilgilerini ve RDP yapılandırmasını sıfırlarsınız. Aşağıdaki örneklerde, `myVMAccessExtension` işlemin bir parçası olarak belirttiğiniz bir ad dır. VMAccessAgent ile daha önce çalıştıysanız, VM'nin özelliklerini denetlemek `Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"` için kullanarak varolan uzantının adını alabilirsiniz. Adı görüntülemek için çıktının 'Uzantılar' bölümünün altına bakın.

Her sorun giderme adımından sonra VM'nize yeniden bağlanmayı deneyin. Hala bağlanamıyorsanız, bir sonraki adımı deneyin.

1. **RDP bağlantınızı sıfırla.** Bu sorun giderme adımı, Uzak Bağlantılar devre dışı bırakıldığında veya Windows Güvenlik Duvarı kuralları RDP'yi engellediğini zdettir, örneğin RDP yapılandırmasını sıfırlar.
   
    Aşağıdaki örnek, `myVM` `WestUS` rdp bağlantısını konumda ve kaynak grubunda adlı `myResourceGroup`bir VM'de sıfırlar:
   
    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```
2. **Ağ Güvenlik Grubu kurallarını doğrulayın.** Bu sorun giderme adımı, Ağ Güvenlik Grubu'nuzda RDP trafiğine izin veren bir kuralınız olduğunu doğrular. RDP için varsayılan bağlantı noktası TCP bağlantı noktası 3389'dur. VM'nizi oluşturduğunuzda RDP trafiğine izin verme kuralı otomatik olarak oluşturulamayabilir.
   
    İlk olarak, Ağ Güvenlik Grubunuz için tüm `$rules` yapılandırma verilerini değişkene atayın. Aşağıdaki örnek, adlı `myNetworkSecurityGroup` `myResourceGroup`kaynak grubunda adı geçen Ağ Güvenlik Grubu hakkında bilgi alır:
   
    ```powershell
    $rules = Get-AzNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```
   
    Şimdi, bu Ağ Güvenlik Grubu için yapılandırılan kuralları görüntüleyin. Gelen bağlantılar için TCP bağlantı noktası 3389'a izin verecek bir kuralın var olduğunu doğrulayın:
   
    ```powershell
    $rules.SecurityRules
    ```
   
    Aşağıdaki örnek, RDP trafiğine izin veren geçerli bir güvenlik kuralını gösterir. Görebilirsiniz `Protocol`, `DestinationPortRange` `Access`, `Direction` , ve doğru yapılandırılır:
   
    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```
   
    RDP trafiğine izin veren bir kuralınız yoksa, [Ağ Güvenlik Grubu kuralı oluşturun.](../windows/nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) TCP bağlantı noktası 3389'a izin ver.
3. **Kullanıcı kimlik bilgilerini sıfırla.** Bu sorun giderme adımı, kimlik bilgilerinden emin olmadığınızı veya unuttuğunuz zaman belirttiğiniz yerel yönetici hesabındaki parolayı sıfırlar.
   
    İlk olarak, değişkene kimlik bilgilerini aşağıdaki gibi `$cred` atayarak kullanıcı adını ve yeni bir parola belirtin:
   
    ```powershell
    $cred=Get-Credential
    ```
   
    Şimdi, VM'nizdeki kimlik bilgilerini güncelleştirin. Aşağıdaki örnek, `myVM` `WestUS` konumda ve kaynak grubunda adı `myResourceGroup`geçen bir VM'deki kimlik bilgilerini güncelleştirir:
   
    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```
4. **VM'nizi yeniden başlatın.** Bu sorun giderme adımı, VM'nin kendisinin yaşadığı temel sorunları düzeltebilir.
   
    Aşağıdaki örnek, adlı `myVM` `myResourceGroup`kaynak grubunda adı geçen VM'yi yeniden başlatır:
   
    ```powershell
    Restart-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```
5. **VM'nizi yeniden dağıtın.** Bu sorun giderme adımı, temel platform veya ağ sorunlarını düzeltmek için VM'nizi Azure'daki başka bir ana bilgisayara yeniden dağır.
   
    Aşağıdaki örnek, `myVM` `WestUS` konumda ve kaynak grubunda adı geçen VM'yi yeniden dağıtır: `myResourceGroup`
   
    ```powershell
    Set-AzVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

6. **Yönlendirmeyi doğrulayın.** Bir rotanın trafiğin sanal bir makineye veya sanal makineden yönlendirilmesine engel olmadığını doğrulamak için Ağ İzleyicisi'nin [Sonraki atlama](../../network-watcher/network-watcher-check-next-hop-portal.md) özelliğini kullanın. Ayrıca, ağ arabirimi için tüm etkili yolları görmek için etkili yolları gözden geçirebilirsiniz. Daha fazla bilgi için [vm trafik akışını gidermek için etkili yolları](../../virtual-network/diagnose-network-routing-problem.md)kullanma'ya bakın.

7. Bilgisayarınızdaki şirket içi güvenlik duvarının veya güvenlik duvarının, TCP 3389 trafiğinin Azure'a giden trafiğine izin verdiğinden emin olun.

RDP sorunlarıyla hala karşılaşıyorsanız, [bir destek isteği açabilir](https://azure.microsoft.com/support/options/) veya daha ayrıntılı [RDP sorun giderme kavramlarını ve adımlarını](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)okuyabilirsiniz.

## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Klasik dağıtım modeli kullanılarak oluşturulan Sorun Giderme VM'leri

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]


Her sorun giderme adımından sonra VM'ye yeniden bağlanmayı deneyin.

1. **RDP bağlantınızı sıfırla.** Bu sorun giderme adımı, Uzak Bağlantılar devre dışı bırakıldığında veya Windows Güvenlik Duvarı kuralları RDP'yi engellediğini zdettir, örneğin RDP yapılandırmasını sıfırlar.
   
    Azure portalında VM'nizi seçin. Tıklayın **... Daha fazla** düğme, sonra **Uzaktan Erişimi Sıfırla'yı**tıklatın:
   
    ![Azure portalında RDP yapılandırmasını sıfırlama](./media/troubleshoot-rdp-connection/classic-reset-rdp.png)
2. **Bulut Hizmetleri uç noktalarını doğrulayın.** Bu sorun giderme adımı, RDP trafiğine izin vermek için Bulut Hizmetlerinizde uç noktanız olduğunu doğrular. RDP için varsayılan bağlantı noktası TCP bağlantı noktası 3389'dur. VM'nizi oluşturduğunuzda RDP trafiğine izin verme kuralı otomatik olarak oluşturulamayabilir.
   
   Azure portalında VM'nizi seçin. VM'niz için şu anda yapılandırılan uç noktaları görüntülemek için **Uç Noktalar** düğmesini tıklatın. TCP bağlantı noktası 3389'da RDP trafiğine izin veren uç noktaların olduğunu doğrulayın.
   
   Aşağıdaki örnek, RDP trafiğine izin veren geçerli uç noktaları gösterir:
   
   ![Azure portalındaki Bulut Hizmetleri uç noktalarını doğrula](./media/troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)
   
   RDP trafiğine izin veren bir bitiş noktanız yoksa, [bir Bulut Hizmetleri bitiş noktası oluşturun.](../windows/classic/setup-endpoints.md) TCP'nin özel bağlantı noktasına 3389'a izin ver.
3. **VM önyükleme tanılama gözden geçirin.** Bu sorun giderme adımı, VM'nin bir sorun bildirip bildirmeyini belirlemek için VM konsol günlüklerini gözden geçirir. Tüm VM'ler önyükleme tanılama etkin, bu nedenle bu sorun giderme adımı isteğe bağlı olabilir.
   
    Belirli sorun giderme adımları bu makalenin kapsamı dışındadır, ancak RDP bağlantısını etkileyen daha geniş bir soruna işaret edebilir. Konsol günlüklerini ve VM ekran görüntüsünü gözden geçirme hakkında daha fazla bilgi [için VM'ler için Önyükleme Tanılama'ya](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/)bakın.
4. **VM Kaynak Durumu'nu kontrol edin.** Bu sorun giderme adımı, Azure platformunda VM'ye bağlantıyı etkileyebilecek bilinen bir sorun olmadığını doğrular.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini listenin en altına yakın **destek + sorun giderme** bölümüne kaydırın. Kaynak **Durumu** düğmesini tıklatın. **Mevcut**olarak sağlıklı bir VM raporları:
   
    ![Azure portalında VM kaynak durumunu denetleme](./media/troubleshoot-rdp-connection/classic-check-resource-health.png)
5. **Kullanıcı kimlik bilgilerini sıfırla.** Bu sorun giderme adımı, emin olmadığınızı veya kimlik bilgilerini unuttuğunuz zaman belirttiğiniz yerel yönetici hesabındaki parolayı sıfırlar.  VM'ye giriş yaptıktan sonra, bu kullanıcının parolasını sıfırlamanız gerekir.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini listenin en altına yakın **destek + sorun giderme** bölümüne kaydırın. **Parolayı Sıfırla** düğmesini tıklatın. Kullanıcı adınızı ve yeni bir parolagirin. Son olarak, **Kaydet** düğmesini tıklatın:
   
    ![Azure portalındaki kullanıcı kimlik bilgilerini sıfırlama](./media/troubleshoot-rdp-connection/classic-reset-password.png)
6. **VM'nizi yeniden başlatın.** Bu sorun giderme adımı, VM'nin kendisinin yaşadığı temel sorunları düzeltebilir.
   
    Azure portalında VM'nizi seçin ve **Genel Bakış** sekmesini tıklatın. **Yeniden Başlat** düğmesini tıklatın:
   
    ![Azure portalında VM'yi yeniden başlatın](./media/troubleshoot-rdp-connection/classic-restart-vm.png)

7. Bilgisayarınızdaki şirket içi güvenlik duvarının veya güvenlik duvarının, TCP 3389 trafiğinin Azure'a giden trafiğine izin verdiğinden emin olun.

RDP sorunlarıyla hala karşılaşıyorsanız, [bir destek isteği açabilir](https://azure.microsoft.com/support/options/) veya daha ayrıntılı [RDP sorun giderme kavramlarını ve adımlarını](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)okuyabilirsiniz.

## <a name="troubleshoot-specific-rdp-errors"></a>Belirli RDP hatalarında sorun giderme
RDP üzerinden VM'nize bağlanmaya çalışırken belirli bir hata iletisi ile karşılaşabilirsiniz. En yaygın hata iletileri şunlardır:

* [Lisans sağlamak için kullanılabilir uzak masaüstü lisans sunucuları olmadığından uzak oturum kesildi.](troubleshoot-specific-rdp-errors.md#rdplicense)
* [Uzak Masaüstü bilgisayar "adı" bulamıyorum.](troubleshoot-specific-rdp-errors.md#rdpname)
* [Kimlik doğrulama hatası oluştu. Yerel Güvenlik Otoritesi ile bağlantı kurulamıyor.](troubleshoot-specific-rdp-errors.md#rdpauth)
* [Windows Security hatası: Kimlik bilgileriniz çalışmadı.](troubleshoot-specific-rdp-errors.md#wincred)
* [Bu bilgisayar uzak bilgisayara bağlanamıyor.](troubleshoot-specific-rdp-errors.md#rdpconnect)

## <a name="additional-resources"></a>Ek kaynaklar
Bu hataların hiçbiri oluştuysa ve hala Uzak Masaüstü üzerinden VM'ye bağlanamıyorsanız, Uzak Masaüstü için ayrıntılı [sorun giderme kılavuzunu](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)okuyun.
* VM'de çalışan uygulamalara erişmedeki sorun giderme adımları [için, Azure VM'de çalışan bir uygulamaya sorun giderme erişimi](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)ne bakın.
* Azure'daki bir Linux VM'ye bağlanmak için Secure Shell (SSH) kullanarak sorun yaşıyorsanız, [Azure'daki bir Linux VM'ye olan Sorun Giderme SSH bağlantılarına](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.


