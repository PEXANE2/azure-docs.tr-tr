---
title: Azure hatalarında yük devretme sorunlarını giderme | Microsoft Docs
description: Bu makalede, Azure 'a yük devretmenin yaygın hatalarını gidermeye yönelik yollar açıklanmaktadır
author: ponatara
manager: abhemraj
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 01/08/2020
ms.author: mayg
ms.openlocfilehash: 9ef38829a18f9e43f38cbdb291a799110d710cd7
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83834739"
---
# <a name="troubleshoot-errors-when-failing-over-vmware-vm-or-physical-machine-to-azure"></a>VMware VM veya fiziksel makinenin Azure 'a yük devretmesinde oluşan hata sorunlarını giderme

Bir sanal makinenin Azure 'a yük devretmesini yaparken aşağıdaki hatalardan birini alabilirsiniz. Sorunu gidermek için, her bir hata koşulu için açıklanan adımları kullanın.

## <a name="failover-failed-with-error-id-28031"></a>Yük devretme hata KIMLIĞI 28031 ile başarısız oldu

Site Recovery, Azure 'da yük devredilen sanal makine oluşturamadı. Aşağıdaki nedenlerden biri nedeniyle oluşabilir:

* Sanal makineyi oluşturmak için yeterli kullanılabilir kota yok: abonelik > kullanım + kotalar ' a giderek kullanılabilir kotayı kontrol edebilirsiniz. Kotayı artırmak için [Yeni bir destek isteği](https://aka.ms/getazuresupport) açabilirsiniz.

* Aynı Kullanılabilirlik kümesinde farklı büyüklükte ailelerde sanal makineler için yük devretmeye çalışıyorsunuz. Aynı Kullanılabilirlik kümesindeki tüm sanal makineler için aynı büyüklükte aile ' yi seçtiğinizden emin olun. Sanal makinenin Işlem ve ağ ayarlarına giderek boyutu değiştirin ve sonra yük devretmeyi yeniden deneyin.

* Abonelikte bir sanal makinenin oluşturulmasını önleyen bir ilke var. İlkeyi bir sanal makinenin oluşturulmasına izin verecek şekilde değiştirip yük devretmeyi yeniden deneyin.

## <a name="failover-failed-with-error-id-28092"></a>Yük devretme hata KIMLIĞI 28092 ile başarısız oldu

Site Recovery, yük devredilen sanal makine için bir ağ arabirimi oluşturamadı. Abonelikte ağ arabirimleri oluşturmak için yeterli kullanılabilir kotayı kullandığınızdan emin olun. Abonelik > kullanım + kotalar ' a giderek kullanılabilir kotayı kontrol edebilirsiniz. Kotayı artırmak için [Yeni bir destek isteği](https://aka.ms/getazuresupport) açabilirsiniz. Yeterli kotayı varsa, bu durum aralıklı bir sorun olabilir ve işlemi yeniden deneyin. Sorun yeniden denemeler yapıldıktan sonra bile devam ederse, bu belgenin sonunda bir yorum bırakın.  

## <a name="failover-failed-with-error-id-70038"></a>Yük devretme hata KIMLIĞI 70038 ile başarısız oldu

Site Recovery, Azure 'da başarısız bir klasik sanal makine oluşturamadı. Şu nedenle oluşabilir:

* Oluşturulacak sanal makine için gereken bir sanal ağ gibi kaynaklardan biri yok. Sanal makinenin Işlem ve ağ ayarları altında belirtilen sanal ağı oluşturun veya ayarı zaten var olan bir sanal ağ olarak değiştirin ve ardından yük devretmeyi yeniden deneyin.

## <a name="failover-failed-with-error-id-170010"></a>Yük devretme hata KIMLIĞI 170010 ile başarısız oldu

Site Recovery, Azure 'da yük devredilen sanal makine oluşturamadı. Şirket içi sanal makine için bir hidratıon iç etkinliği başarısız olduğu için bu durum oluşabilir.

Azure 'da herhangi bir makineyi getirmek için, Azure ortamı bazı sürücülerin önyükleme başlatma durumunda olmasını ve DHCP gibi hizmetlerin otomatik başlatma durumunda olmasını gerektirir. Bu nedenle, yük devretme sırasında hidrasyon etkinliği, **ATAPI, intelide, storflt, VMBus ve storvsc sürücülerinin** başlangıç türünü önyükleme başlatması ' ne dönüştürür. Ayrıca, DHCP gibi birkaç hizmetin başlangıç türünü autostart 'a dönüştürür. Bu etkinlik, ortama özgü sorunlar nedeniyle başarısız olabilir. 

**Windows Konuk işletim sistemi**için sürücülerin başlangıç türünü el ile değiştirmek için aşağıdaki adımları izleyin:

1. Komut dosyası içermeyen bir betiği [indirin](https://download.microsoft.com/download/5/D/6/5D60E67C-2B4F-4C51-B291-A97732F92369/Script-no-hydration.ps1) ve aşağıdaki gibi çalıştırın. Bu betik, sanal makinenin hidrasyon gerektirip gerektirmediğini denetler.

    `.\Script-no-hydration.ps1`

    Bu, hidrasyon gerekliyse aşağıdaki sonucu verir:

        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0

        This system doesn't meet no-hydration requirement.

    VM 'nin, hidrasyon gereksinimini karşılamasını sağlamak için, betik "Bu sistem, hiçbir hidrasyon gereksinimini karşılıyor" sonucunu verir. Bu durumda, tüm sürücüler ve hizmetler Azure 'un gerektirdiği şekilde durumundadır ve VM 'de doldurma gerekli değildir.

2. Sanal makine, hidrasyon gereksinimini karşılamıyorsa, hiçbir bir komut dosyasını aşağıdaki gibi ayarla ' yı çalıştırın.

    `.\Script-no-hydration.ps1 -set`
    
    Bu, sürücülerin başlangıç türünü dönüştürecek ve sonucu aşağıdaki gibi verecektir:
    
        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0 

        Updating registry:  REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc   start =  0 

        This system is now no-hydration compatible. 

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Sanal makinede gri bağlantı düğmesi nedeniyle yük devredilecek sanal makineye/RDP/SSH bağlantısı kurulamıyor

Azure 'daki yük devredilen VM 'deki **Bağlan** düğmesi gri Ise ve Azure 'A bir Express Route veya sıteden siteye VPN bağlantısı aracılığıyla bağlı değilseniz,

1. **Sanal makine**  >  **ağı**' na gidin, gerekli ağ arabiriminin adına tıklayın.  ![Ağ arabirimi](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. **IP yapılandırmaları**' na gidin ve gerekli IP yapılandırmasının ad alanına tıklayın. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Genel IP adresini etkinleştirmek için **Etkinleştir**' e tıklayın. ![IP 'yi etkinleştir](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. **Gerekli ayarları Yapılandır**  >  **Yeni oluştur**' a tıklayın. ![Yeni oluştur](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Ortak adres adını girin, **SKU** ve **atama**için varsayılan seçenekleri belirleyin ve ardından **Tamam**' a tıklayın.
6. Şimdi yaptığınız değişiklikleri kaydetmek için **Kaydet**' e tıklayın.
7. Panelleri kapatın ve sanal makinenin bağlanmak/RDP 'ye **genel bakış** bölümüne gidin.

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>Bağlantı kurulamıyor/RDP/SSH-VM Connect düğmesi kullanılabilir

Azure 'daki yük devredilen VM üzerindeki **Bağlan** düğmesi kullanılabiliyorsa (gri değil), sanal makinenizde **önyükleme tanılamayı** denetleyin ve [Bu makalede](../virtual-machines/windows/boot-diagnostics.md)listelenen hataları denetleyin.

1. Sanal makine başlatılmamışsa, daha eski bir kurtarma noktasına yük devretmeyi deneyin.
2. Sanal makinenin içindeki uygulama yoksa uygulamayla tutarlı bir kurtarma noktasına yük devretmeyi deneyin.
3. Sanal makine etki alanına katılmış ise, etki alanı denetleyicisinin doğru şekilde çalıştığından emin olun. Bu işlem, aşağıdaki verilen adımlar aşağıda verilmiştir:

    a. Aynı ağda yeni bir sanal makine oluşturun.

    b.  Yük devredilen sanal makinenin gelmesi beklenen aynı etki alanına katılabilmesini sağlayın.

    c. Etki alanı **denetleyicisi düzgün çalışmıyorsa** , yerel bir yönetici hesabı kullanarak yük devredilmekte olan sanal makinede oturum açmayı deneyin.
4. Özel bir DNS sunucusu kullanıyorsanız, erişilebilir olduğundan emin olun. Bu işlem, aşağıdaki verilen adımlar aşağıda verilmiştir:

    a. Aynı ağda yeni bir sanal makine oluşturun ve

    b. Sanal makinenin özel DNS sunucusunu kullanarak ad çözümlemesi yapabiliyor olup olmadığını denetleyin

>[!Note]
>Önyükleme tanılaması dışındaki ayarların etkinleştirilmesi, yük devretmeden önce sanal makinede Azure VM Aracısı 'nın yüklenmesini gerektirir

## <a name="unable-to-open-serial-console-after-failover-of-a-uefi-based-machine-into-azure"></a>UEFı tabanlı makinenin Azure 'a yük devretmesinin ardından seri konsol açılamıyor

RDP kullanarak makineye bağlanabiliyor ancak seri konsol açılamıyor, aşağıdaki adımları izleyin:

* Makine işletim sistemi Red hat veya Oracle Linux 7. */8.0 ise, yük devretme Azure VM 'de kök izinlerle aşağıdaki komutu çalıştırın. Komutundan sonra VM 'yi yeniden başlatın.

        grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg

* Makine işletim sistemi CentOS 7. * ise, yük devretme Azure VM 'de kök izinlerle aşağıdaki komutu çalıştırın. Komutundan sonra VM 'yi yeniden başlatın.

        grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg

## <a name="unexpected-shutdown-message-event-id-6008"></a>Beklenmeyen kapatılma iletisi (olay KIMLIĞI 6008)

Windows VM 'nin yük devretmesini geri yüklerken, kurtarılan VM 'de beklenmeyen bir kapalı ileti alırsanız, yük devretme için kullanılan kurtarma noktasında bir VM kapatılma durumunun yakalanmadığını belirtir. Bu durum, VM tam olarak kapatılmayan bir noktaya kurtardığınızda meydana gelir.

Bu durum normalde sorun için bir neden değildir ve genellikle planlanmamış yük devretme işlemleri için yoksayılabilir. Yük devretme planlandığından, yük devretmeden önce VM 'nin doğru şekilde kapatıldığından ve şirket içi bekleyen çoğaltma verilerinin Azure 'a gönderilmesi için yeterli zaman sağladığınızdan emin olun. Daha sonra, Azure üzerindeki bekleyen tüm verilerin bir kurtarma noktasına işlenmesini sağlamak için [Yük devretme ekranında](site-recovery-failover.md#run-a-failover) **en son** seçeneğini kullanın, daha sonra VM yük devretmesi için kullanılır.

## <a name="unable-to-select-the-datastore"></a>Veri deposu seçimi yapılamıyor

Bu sorun, yük devretmeyle karşılaşmış olan sanal makineyi yeniden korumaya çalışırken Azure 'da veri deposunu göremiyorsanız belirtilir. Bunun nedeni, ana hedefin vCenters altında Azure Site Recovery eklenen bir sanal makine olarak tanınmamaktadır.

Bir sanal makineyi yeniden koruma hakkında daha fazla bilgi için bkz. [Azure 'a yük devretmeden sonra makineleri şirket içi bir siteye yeniden koruma ve geri](vmware-azure-reprotect.md)dönme.

Sorunu çözmek için:

Ana hedefi, kaynak makinenizi yöneten vCenter 'da el ile oluşturun. Veri deposu, sonraki vCenter Discovery ve Refresh Fabric işlemlerinden sonra kullanılabilir olacaktır.

> [!Note]
> 
> Bulma ve yenileme yapı işlemlerinin tamamlanması 30 dakika kadar sürebilir. 

## <a name="linux-master-target-registration-with-cs-fails-with-a-tls-error-35"></a>CS ile Linux ana hedef kaydı, TLS hatasıyla başarısız oluyor 35 

Yapılandırma sunucusu ile Azure Site Recovery ana hedef kaydı, ana hedefte kimliği doğrulanmış proxy etkinleştirilmiş olduğundan başarısız olur. 
 
Bu hata, yükleme günlüğünde aşağıdaki dizeler tarafından belirtilir: 

```
RegisterHostStaticInfo encountered exception config/talwrapper.cpp(107)[post] CurlWrapper Post failed : server : 10.38.229.221, port : 443, phpUrl : request_handler.php, secure : true, ignoreCurlPartialError : false with error: [at curlwrapperlib/curlwrapper.cpp:processCurlResponse:231]   failed to post request: (35) - SSL connect error. 
```

Sorunu çözmek için:
 
1. Yapılandırma sunucusu VM 'sinde, bir komut istemi açın ve aşağıdaki komutları kullanarak proxy ayarlarını doğrulayın:

    cat/etc/Environment Echo $http _proxy Echo $https _proxy 

2. Önceki komutların çıktısı, http_proxy veya https_proxy ayarlarının tanımlandığını gösteriyorsa, yapılandırma sunucusu ile ana hedef iletişimlerinin engelini kaldırmak için aşağıdaki yöntemlerden birini kullanın:
   
   - [PsExec aracını](https://aka.ms/PsExec)indirin.
   - Aracı kullanarak sistem Kullanıcı bağlamına erişin ve proxy adresinin yapılandırılıp yapılandırılmadığını saptayın. 
   - Proxy yapılandırıldıysa, PsExec aracını kullanarak bir sistem kullanıcı bağlamında IE 'yi açın.
  
     **PsExec-s-i "%ProgramFiles%\Internet Explorer\iexplore.exe"**

   - Ana hedef sunucunun yapılandırma sunucusuyla iletişim kurabildiğinden emin olmak için:
  
     - Ana hedef sunucu IP adresini proxy üzerinden atlamak için Internet Explorer 'daki proxy ayarlarını değiştirin.   
     Veya
     - Ana hedef sunucusunda proxy 'yi devre dışı bırakın. 


## <a name="next-steps"></a>Sonraki adımlar
- [WINDOWS VM Ile RDP bağlantısı](../virtual-machines/windows/troubleshoot-rdp-connection.md) sorunlarını giderme
- [LINUX VM 'ye SSH bağlantısı](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md) sorunlarını giderme

Daha fazla yardıma ihtiyacınız varsa sorgunuzu [Microsoft Q&soru sayfasında Site Recovery](https://docs.microsoft.com/answers/topics/azure-site-recovery.html) gönderin veya bu belgenin sonunda bir yorum bırakın. Size yardımcı olabilmemiz gereken etkin bir topluluk var.
