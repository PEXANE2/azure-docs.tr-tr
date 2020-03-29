---
title: Azure'da ayrıntılı uzak masaüstü sorun giderme | Microsoft Dokümanlar
description: Azure'daki Windows sanal makinelerine giremediğiniz uzak masaüstü hataları için ayrıntılı sorun giderme adımlarını gözden geçirin
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: uzak masaüstüne bağlanamaz, uzak masaüstüne sorun gideremez, uzak masaüstü bağlanamaz, uzak masaüstü hataları, uzak masaüstü sorun giderme, uzak masaüstü sorunları
ms.assetid: 9da36f3d-30dd-44af-824b-8ce5ef07e5e0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: ea448b87f9e6954abecead2934bfb7f4ed04a9c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920153"
---
# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Azure'daki Windows VM’lerine uzak masaüstü bağlantısı sorunlarında ayrıntılı sorun giderme adımları
Bu makalede, Windows tabanlı Azure sanal makineleri için karmaşık Uzak Masaüstü hatalarını tanılamak ve düzeltmek için ayrıntılı sorun giderme adımları sağlanmaktadır.

> [!IMPORTANT]
> Daha yaygın Uzak Masaüstü hatalarını ortadan kaldırmak için, devam etmeden önce [Uzak Masaüstü için temel sorun giderme makalesini](troubleshoot-rdp-connection.md) okuduğunuzdan emin olun.

[Temel Uzak Masaüstü sorun giderme kılavuzunda](troubleshoot-rdp-connection.md)kapsanan belirli hata iletilerinin hiçbirine benzemez bir Uzak Masaüstü hata iletisi ile karşılaşabilirsiniz. Uzak Masaüstü (RDP) istemcisinin Azure VM'deki RDP hizmetine neden bağlanamadığını belirlemek için aşağıdaki adımları izleyin.


Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı da dosyalayabilirsiniz. [Azure Destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**tıklatın. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure Destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.

## <a name="components-of-a-remote-desktop-connection"></a>Uzak Masaüstü bağlantısının bileşenleri
Aşağıdaki bileşenler bir RDP bağlantısında yer almaktadır:

![](./media/detailed-troubleshoot-rdp/tshootrdp_0.png)

Devam etmeden önce, VM'ye yapılan son başarılı Uzak Masaüstü bağlantısından bu yana nelerin değiştiğini zihinsel olarak gözden geçirmeye yardımcı olabilir. Örnek:

* VM'nin genel IP adresi veya VM içeren bulut hizmeti (sanal IP adresi [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)olarak da adlandırılır) değişti. RDP hatası, DNS istemci önbelleğinizin hala DNS adı için kayıtlı *eski IP adresine* sahip olması olabilir. DNS istemci önbelleğinizi temizle ve VM'yi yeniden bağlamayı deneyin. Veya doğrudan yeni VIP ile bağlanmayı deneyin.
* Azure portalı tarafından oluşturulan bağlantıyı kullanmak yerine Uzak Masaüstü bağlantılarınızı yönetmek için üçüncü taraf bir uygulama kullanıyorsunuz. Uygulama yapılandırmasının Uzak Masaüstü trafiği için doğru TCP bağlantı noktasını içerdiğini doğrulayın. VM'nin Ayarlar > Bitiş Noktaları'nı tıklatarak Azure [portalında](https://portal.azure.com)bu bağlantı noktasını klasik bir sanal makine için kontrol edebilirsiniz.

## <a name="preliminary-steps"></a>Ön adımlar
Ayrıntılı sorun gidermeye geçmeden önce,

* Azure portalındaki sanal makinenin durumunu açık sorunlar için denetleyin.
* Temel [sorun giderme kılavuzunda sık karşılaşılan RDP hataları için hızlı düzeltme adımlarını](troubleshoot-rdp-connection.md#quick-troubleshooting-steps)izleyin.
* Özel görüntüler için, VHD'nizin yüklemeden önce düzgün bir şekilde hazırolduğundan emin olun. Daha fazla bilgi için azure'a [yüklemek için Windows VHD veya VHDX Hazırlama 'ya](../windows/prepare-for-upload-vhd-image.md)bakın.


Bu adımlardan sonra Uzak Masaüstü üzerinden VM'ye yeniden bağlanmayı deneyin.

## <a name="detailed-troubleshooting-steps"></a>Ayrıntılı sorun giderme adımları
Uzak Masaüstü istemcisi, aşağıdaki kaynaklardaki sorunlar nedeniyle Azure VM'deki Uzak Masaüstü hizmetine erişemeyebilir:

* [Uzak Masaüstü istemci bilgisayar](#source-1-remote-desktop-client-computer)
* [Organizasyon intranet kenar cihazı](#source-2-organization-intranet-edge-device)
* [Bulut hizmeti bitiş noktası ve erişim denetim listesi (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Ağ güvenlik grupları](#source-4-network-security-groups)
* [Windows tabanlı Azure VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Kaynak 1: Uzak Masaüstü istemci bilgisayar
Bilgisayarınızın windows tabanlı başka bir bilgisayara Uzak Masaüstü bağlantısı kurabileceğini doğrulayın.

![](./media/detailed-troubleshoot-rdp/tshootrdp_1.png)

Yapamıyorsanız, bilgisayarınızdaki aşağıdaki ayarları denetleyin:

* Uzak Masaüstü trafiğini engelleyen yerel bir güvenlik duvarı ayarı.
* Uzak Masaüstü bağlantılarını engelleyen yerel olarak yüklenmiş istemci proxy yazılımı.
* Uzak Masaüstü bağlantılarını engelleyen yerel olarak yüklenmiş ağ izleme yazılımı.
* Trafiği izleyen veya Uzak Masaüstü bağlantılarını engelleyen belirli trafik türlerine izin veren/izin veren diğer güvenlik yazılımı türleri.

Tüm bu durumlarda, yazılımı geçici olarak devre dışı bırakın ve Uzak Masaüstü üzerinden şirket içi bir bilgisayara bağlanmaya çalışın. Gerçek nedenini bu şekilde öğrenebiliyorsanız, Uzak Masaüstü bağlantılarına izin vermek için yazılım ayarlarını düzeltmek için ağ yöneticinizle birlikte çalışın.

## <a name="source-2-organization-intranet-edge-device"></a>Kaynak 2: Organizasyon intranet kenar cihazı
Doğrudan Internet'e bağlı bir bilgisayarın Azure sanal makinenize Uzak Masaüstü bağlantıları kurabileceğini doğrulayın.

![](./media/detailed-troubleshoot-rdp/tshootrdp_2.png)

Doğrudan Internet'e bağlı bir bilgisayarınız yoksa, kaynak grubunda veya bulut hizmetinde yeni bir Azure sanal makinesi oluşturun ve test edin. Daha fazla bilgi için bkz: [Azure'da Windows çalıştıran sanal bir makine oluştur.](../virtual-machines-windows-hero-tutorial.md) Testten sonra sanal makineyi ve kaynak grubunu veya bulut hizmetini silebilirsiniz.

Doğrudan Internet'e bağlı bir bilgisayarla Uzak Masaüstü bağlantısı oluşturabiliyorsanız, kuruluşunuzun intranet kenar aygıtını şu şekilde denetleyin:

* Internet'e HTTPS bağlantılarını engelleyen dahili bir güvenlik duvarı.
* Uzak Masaüstü bağlantılarını engelleyen bir proxy sunucusu.
* Kenar ağınızdaki aygıtlarda çalışan ve Uzak Masaüstü bağlantılarını engelleyen izinsiz giriş algılama veya ağ izleme yazılımı.

HTTPS tabanlı Uzak Masaüstü bağlantılarının Internet'e bağlanmasına izin vermek için kuruluşunuzun intranet kenar aygıtının ayarlarını düzeltmek için ağ yöneticinizle birlikte çalışın.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Kaynak 3: Bulut hizmeti bitiş noktası ve ACL

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Klasik dağıtım modeli kullanılarak oluşturulan Sanal M'ler için, aynı bulut hizmetinde veya sanal ağda bulunan başka bir Azure VM'nin Azure VM'nize Uzak Masaüstü bağlantıları kurabileceğini doğrulayın.

![](./media/detailed-troubleshoot-rdp/tshootrdp_3.png)

> [!NOTE]
> Kaynak Yöneticisi'nde oluşturulan sanal makineler için [Kaynak 4: Ağ Güvenlik Grupları'na](#source-4-network-security-groups)atlayın.

Aynı bulut hizmetinde veya sanal ağda başka bir sanal makineniz yoksa, bir tane oluşturun. Azure'da [Windows çalıştıran sanal bir makine oluştur'daki](../virtual-machines-windows-hero-tutorial.md)adımları izleyin. Test tamamlandıktan sonra sanal test makinesini silin.

Uzak Masaüstü üzerinden aynı bulut hizmetinde veya sanal ağdaki sanal bir makineye bağlanabiliyorsanız, aşağıdaki ayarları denetleyin:

* Hedef VM'deki Uzak Masaüstü trafiği için bitiş noktası yapılandırması: Bitiş noktasının özel TCP bağlantı noktası, VM'nin Uzak Masaüstü hizmetinin dinlediği TCP bağlantı noktasıyla eşleşmelidir (varsayılan değer 3389'dur).
* Hedef VM'deki Uzak Masaüstü trafik bitiş noktası için ACL: ACL'ler, kaynak IP adresine bağlı olarak Internet'ten gelen veya reddedilen trafiği belirtmenize olanak sağlar. Yanlış yapılandırılmış APC'ler, bitiş noktasına gelen Uzak Masaüstü trafiğini engelleyebilir. Proxy veya diğer kenar sunucunuzun genel IP adreslerinden gelen trafiğe izin verilebilmesini sağlamak için ALA'larınızı kontrol edin. Daha fazla bilgi için [bkz: Ağ Erişim Denetim Listesi (ACL) nedir?](../../virtual-network/virtual-networks-acl.md)

Bitiş noktasının sorunun kaynağı olup olmadığını kontrol etmek için, geçerli bitiş noktasını kaldırın ve dış bağlantı noktası numarası için 49152-65535 aralığında rasgele bir bağlantı noktası seçerek yeni bir nokta oluşturun. Daha fazla bilgi için, [sanal bir makinenin uç noktalarını nasıl ayarlayınız' a](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)bakın.

## <a name="source-4-network-security-groups"></a>Kaynak 4: Ağ Güvenlik Grupları
Ağ Güvenlik Grupları, izin verilen gelen ve giden trafiğin daha ayrıntılı denetimine izin verir. Bir Azure sanal ağında alt ağları ve bulut hizmetlerini kapsayan kurallar oluşturabilirsiniz.

[IP akışı doğrulamayı](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) kullanarak Ağ Güvenlik Grubu’ndaki bir kuralın bir sanal makineye giden veya gelen trafiği engelleyip engellemediğini doğrulayın. Gelen "İzin Ver" NSG kuralının var olduğundan ve RDP bağlantı noktası (varsayılan 3389) için öncelike olduğundan emin olmak için etkili güvenlik grubu kurallarını da gözden geçirebilirsiniz. Daha fazla bilgi için [VM trafik akışını gidermek için Etkili Güvenlik Kurallarını Kullanma'ya](../../virtual-network/diagnose-network-traffic-filter-problem.md)bakın.

## <a name="source-5-windows-based-azure-vm"></a>Kaynak 5: Windows tabanlı Azure VM
![](./media/detailed-troubleshoot-rdp/tshootrdp_5.png)

[Bu makaledeki](../windows/reset-rdp.md)yönergeleri izleyin. Bu makalede, sanal makinede Uzak Masaüstü hizmeti sıfırlanır:

* "Uzak Masaüstü" Windows Güvenlik Duvarı varsayılan kuralını etkinleştirin (TCP bağlantı noktası 3389).
* HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections kayıt defteri değerini 0'a ayarlayarak Uzak Masaüstü bağlantılarını etkinleştirin.

Bilgisayarınızdaki bağlantıyı yeniden deneyin. Uzak Masaüstü üzerinden hala bağlanamıyorsanız, aşağıdaki olası sorunları kontrol edin:

* Uzak Masaüstü hizmeti hedef VM üzerinde çalışmıyor.
* Uzak Masaüstü hizmeti TCP bağlantı noktası 3389'da dinlemiyor.
* Windows Güvenlik Duvarı veya başka bir yerel güvenlik duvarı, Uzak Masaüstü trafiğini engelleyen giden bir kurala sahiptir.
* Azure sanal makinesinde çalışan izinsiz giriş algılama veya ağ izleme yazılımı Uzak Masaüstü bağlantılarını engelliyor.

Klasik dağıtım modeli kullanılarak oluşturulan VM'ler için, Azure sanal makinesinde uzak bir Azure PowerShell oturumu kullanabilirsiniz. İlk olarak, sanal makinenin barındırma bulut hizmeti için bir sertifika yüklemeniz gerekir. Azure [Sanal Makinelere Güvenli Uzaktan Güç Shell Erişimini Yapılandırma'ya](https://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) gidin ve **InstallWinRMCertAzureVM.ps1** komut dosyası dosyasını yerel bilgisayarınıza indirin.

Ardından, henüz yapmadıysanız Azure PowerShell'i yükleyin. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Ardından, bir Azure PowerShell komut istemi açın ve geçerli klasörü **InstallWinRMCertAzureVM.ps1** komut dosyasının konumuyla değiştirin. Azure PowerShell komut dosyasını çalıştırmak için doğru yürütme ilkesini ayarlamanız gerekir. Geçerli ilke düzeyinizi belirlemek için **Get-ExecutionPolicy** komutunu çalıştırın. Uygun düzeyin ayarlanması hakkında bilgi için [Set-ExecutionPolicy'ye](https://technet.microsoft.com/library/hh849812.aspx)bakın.

Ardından, Azure abonelik adınızı, bulut hizmeti adınızı ve sanal makine adınızı (< ve > karakterleri kaldırma) doldurun ve bu komutları çalıştırın.

```powershell
$subscr="<Name of your Azure subscription>"
$serviceName="<Name of the cloud service that contains the target virtual machine>"
$vmName="<Name of the target virtual machine>"
.\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName
```

AzureAbonelik komutunun ekranının *AbonelikAdı* özelliğinden doğru **Get-AzureSubscription** abonelik adını alabilirsiniz. **AzureVM al** komutunun ekranındaki *ServiceName* sütunundan sanal makinenin bulut hizmeti adını alabilirsiniz.

Yeni sertifikaya sahip olup olmadığını kontrol edin. Geçerli kullanıcı için sertifikalar tutturup açın ve **Güvenilen Kök Sertifika Yetkilileri\Sertifikalar** klasörüne bakın. VerilenLer sütununda bulut hizmetinizin DNS adını içeren bir sertifika görmeniz gerekir (örnek: cloudservice4testing.cloudapp.net).

Ardından, bu komutları kullanarak uzak bir Azure PowerShell oturumu başlatın.

```powershell
$uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
$creds = Get-Credential
Enter-PSSession -ConnectionUri $uri -Credential $creds
```

Geçerli yönetici kimlik bilgilerini girdikten sonra, aşağıdaki Azure PowerShell istemine benzer bir şey görmeniz gerekir:

```powershell
[cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>
```

Bu komut isteminin ilk bölümü, "cloudservice4testing.cloudapp.net"dan farklı olabilecek hedef VM'yi içeren bulut hizmet adınızdır. Artık söz konusu sorunları araştırmak ve yapılandırmayı düzeltmek için bu bulut hizmeti için Azure PowerShell komutları verebilirsiniz.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>TCP bağlantı noktasını dinleyerek Uzak Masaüstü Hizmetlerini el ile düzeltmek için
Uzak Azure PowerShell oturum isteminde bu komutu çalıştırın.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

PortNumber özelliği geçerli bağlantı noktası numarasını gösterir. Gerekirse, bu komutu kullanarak Uzak Masaüstü bağlantı noktası numarasını varsayılan değerine (3389) geri değiştirin.

```powershell
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389
```

Bu komutu kullanarak bağlantı noktasının 3389 olarak değiştirildiğini doğrulayın.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

Bu komutu kullanarak uzak Azure PowerShell oturumundan çıkın.

```powershell
Exit-PSSession
```

Azure VM için Uzak Masaüstü bitiş noktasının dahili bağlantı noktası olarak TCP bağlantı noktası 3398'i de kullandığını doğrulayın. Azure VM'yi yeniden başlatın ve Uzak Masaüstü bağlantısını yeniden deneyin.

## <a name="additional-resources"></a>Ek kaynaklar
[Windows sanal makineleri için parola veya Uzak Masaüstü hizmetini sıfırlama](../windows/reset-rdp.md)

[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)

[Linux tabanlı Azure sanal makinesine Güvenli Kabuk (SSH) bağlantılarıyla sorun giderme](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure sanal makinesinde çalışan bir uygulamaya erişimi giderme sorunu](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

