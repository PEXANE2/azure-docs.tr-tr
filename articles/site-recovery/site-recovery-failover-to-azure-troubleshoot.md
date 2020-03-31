---
title: Azure hatalarının üzerinden sorunsuz bir şekilde giderme | Microsoft Dokümanlar
description: Bu makalede, Azure üzerinde başarısız sık karşılaşılan hataları gidermek için yollar açıklanmaktadır
author: ponatara
manager: abhemraj
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 01/08/2020
ms.author: mayg
ms.openlocfilehash: 6de37daa0b9e0ebc711a5dacbdce352e3675a3db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257751"
---
# <a name="troubleshoot-errors-when-failing-over-vmware-vm-or-physical-machine-to-azure"></a>VMware VM veya fiziksel makine üzerinden Azure'da başarısız olduğunda hata giderme

Sanal bir makinenin Azure'da başarısız olmasını yaparken aşağıdaki hatalardan birini alabilirsiniz. Sorun gidermek için, her hata koşulu için açıklanan adımları kullanın.

## <a name="failover-failed-with-error-id-28031"></a>Failover Hata Kimliği 28031 ile başarısız oldu

Site Kurtarma, Azure'da sanal makine üzerinde başarısız bir makine oluşturamadı. Aşağıdaki nedenlerden biri nedeniyle olabilir:

* Sanal makine oluşturmak için yeterli kota yok: Abonelik -> Kullanımı + kotalarına giderek kullanılabilir kotayı kontrol edebilirsiniz. Kotayı artırmak için yeni bir [destek isteği](https://aka.ms/getazuresupport) açabilirsiniz.

* Aynı kullanılabilirlik kümesinde farklı boyutlardaailelerin sanal makineleri üzerinden başarısız çalışıyorsunuz. Aynı kullanılabilirlik kümesindeki tüm sanal makineler için aynı boyutta bir aile seçtiğinizden emin olun. Sanal makinenin Bilgi İşlem ve Ağ ayarlarına giderek boyutu değiştirin ve ardından başarısız olmayı yeniden deneyin.

* Abonelikle ilgili sanal bir makinenin oluşturulmasını engelleyen bir ilke vardır. Sanal bir makine nin oluşturulmasına izin vermek ve ardından başarısız olmayı yeniden denemek için ilkeyi değiştirin.

## <a name="failover-failed-with-error-id-28092"></a>Failover Hata Kimliği 28092 ile başarısız oldu

Site Kurtarma sanal makine üzerinde başarısız için bir ağ arabirimi oluşturmak mümkün değildi. Abonelikte ağ arabirimleri oluşturmak için yeterli kotaya sahip olduğundan emin olun. Abonelik -> Kullanımı + kotalarına giderek kullanılabilir kotayı kontrol edebilirsiniz. Kotayı artırmak için yeni bir [destek isteği](https://aka.ms/getazuresupport) açabilirsiniz. Yeterli kotanız varsa, bu aralıklı bir sorun olabilir, işlemi yeniden deneyin. Sorun yeniden denemelerden sonra da devam ederse, bu belgenin sonuna bir yorum bırakın.  

## <a name="failover-failed-with-error-id-70038"></a>Hata Kimliği 70038 ile failover başarısız oldu

Site Kurtarma, Azure'da Klasik sanal makine üzerinde başarısız bir makine oluşturamadı. Bu olabilir çünkü:

* Sanal makinenin oluşturulması için gereken sanal ağ gibi kaynaklardan biri yok. Sanal makinenin Bilgi İşlem ve Ağ ayarları altında sağlandığı gibi sanal ağı oluşturun veya ayarı zaten var olan sanal ağla değiştirin ve ardından başarısız olmayı yeniden deneyin.

## <a name="failover-failed-with-error-id-170010"></a>Hata Kimliği 170010 ile başarısız oldu

Site Kurtarma, Azure'da sanal makine üzerinde başarısız bir makine oluşturamadı. Bu olabilir, çünkü şirket içi sanal makine için bir iç hidrasyon aktivitesi başarısız oldu.

Azure'daki herhangi bir makineyi gündeme getirmek için Azure ortamı, bazı sürücülerin önyükleme başlatma durumunda olmasını ve DHCP gibi hizmetlerin otomatik başlatma durumunda olmasını gerektirir. Böylece, hidrasyon aktivitesi, failover anda, **atapi, intelide, storflt, vmbus ve storvsc sürücüleri** başlangıç başlatmak için başlangıç türünü dönüştürür. Ayrıca, DHCP gibi birkaç hizmetin başlangıç türünü otomatik başlatmaya dönüştürür. Bu etkinlik, ortama özgü sorunlar nedeniyle başarısız olabilir. 

**Windows Guest OS**için başlangıç sürücülerinin başlangıç türünü el ile değiştirmek için aşağıdaki adımları izleyin:

1. Hidrasyonsuz komut dosyasını [indirin](https://download.microsoft.com/download/5/D/6/5D60E67C-2B4F-4C51-B291-A97732F92369/Script-no-hydration.ps1) ve aşağıdaki gibi çalıştırın. VM hidrasyon gerektiriyorsa bu komut dosyası denetler.

    `.\Script-no-hydration.ps1`

    Hidrasyon gerekiyorsa aşağıdaki sonucu verir:

        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0

        This system doesn't meet no-hydration requirement.

    VM'nin hidrasyon gereksinimini karşılaması durumunda, komut dosyası "Bu sistem hidrasyon gereksinimini karşılar" sonucunu verecektir. Bu durumda, tüm sürücüler ve hizmetler Azure tarafından gerekli olduğu şekilde durumdadır ve VM'de hidrasyon gerekmez.

2. VM hidrasyon gereksinimini karşılamazsa hidrasyon ayarsız komut dosyasını aşağıdaki gibi çalıştırın.

    `.\Script-no-hydration.ps1 -set`
    
    Bu, başlangıç sürücü türünü dönüştürür ve aşağıdaki gibi sonuç verecektir:
    
        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0 

        Updating registry:  REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc   start =  0 

        This system is now no-hydration compatible. 

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Sanal makinede gri dışarı Bağlan düğmesi nedeniyle sanal makine üzerinden başarısız olana/RDP/SSH'ye bağlanılamıyor

Azure'daki VM üzerinden başarısız olan **Connect** düğmesinin gri renkte olması ve Bir Ekspres Rota veya Siteden Siteye VPN bağlantısı yla Azure'a bağlı değilseniz,

1. Sanal **makine** > **Networking**gidin, gerekli ağ arabiriminin adını tıklatın.  ![ağ arabirimi](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Ip **Yapılandırmaları'na**gidin, ardından gerekli IP yapılandırmasının ad alanına tıklayın. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Genel IP adresini etkinleştirmek için **Etkinleştir'e**tıklayın. ![IP'yi etkinleştir](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4.  > Gerekli **ayarları Yapılandır'a**tıklayın**Yeni oluşturun**. ![Yeni oluşturma](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Ortak adresin adını girin, **SKU** ve **atama**için varsayılan seçenekleri seçin, ardından **Tamam'ı**tıklatın.
6. Şimdi, yapılan değişiklikleri kaydetmek için **Kaydet'i**tıklatın.
7. Panelleri kapatın ve bağlanmak/RDP'ye bağlanmak için sanal makinenin **Genel Bakış** bölümüne gidin.

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>Bağlanamıyor/RDP/SSH - VM Connect düğmesi kullanılabilir

Azure'da Başarısız VM üzerinde başarısız olan **Bağlan** düğmesi kullanılabilirse (gri renkte değilse), Sanal Makinenizde **Önyükleme tanılamalarını** denetleyin ve [bu makalede](../virtual-machines/windows/boot-diagnostics.md)listelenen hataları denetleyin.

1. Sanal makine başlatılmazsa, eski bir kurtarma noktasına kadar başarısız olmayı deneyin.
2. Sanal makinenin içindeki uygulama açılamazsa, uygulama tutarlı bir kurtarma noktasına başarısız olmayı deneyin.
3. Sanal makine etki alanı birleştirilmişse, etki alanı denetleyicisinin doğru çalıştığından emin olun. Bu aşağıdaki adımları izleyerek yapılabilir:

    a. Aynı ağda yeni bir sanal makine oluşturun.

    b.  Sanal makine üzerinde başarısız olması beklenen aynı etki alanına katılmak mümkün olduğundan emin olun.

    c. Etki alanı denetleyicisi düzgün **not** çalışmıyorsa, yerel bir yönetici hesabı kullanarak başarısız sanal makine üzerinden oturum açmayı deneyin.
4. Özel bir DNS sunucusu kullanıyorsanız, bu sunucuya erişilebildiğinizden emin olun. Bu aşağıdaki adımları izleyerek yapılabilir:

    a. Aynı ağda yeni bir sanal makine oluşturun ve

    b. Sanal makinenin özel DNS Server'ı kullanarak ad çözümlemesi yapailip kullanamayabildiğini kontrol edin

>[!Note]
>Önyükleme Tanılama dışında herhangi bir ayarı etkinleştirmek, azure VM Aracısının başarısız olmadan önce sanal makineye yüklenmesini gerektirir

## <a name="unable-to-open-serial-console-after-failover-of-a-uefi-based-machine-into-azure"></a>UEFI tabanlı bir makinenin Azure'a dönüşmesinden sonra seri konsolaçılamıyor

RDP kullanarak makineye bağlanabiliyor ancak seri konsolu açamıyorsanız aşağıdaki adımları izleyin:

* Makine işletim sistemi Red Hat veya Oracle Linux 7.*/8.0 ise, root izinleri ile başarısız Azure VM üzerinde aşağıdaki komutu çalıştırın. Komuttan sonra VM'yi yeniden başlatın.

        grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg

* Makine işletim sistemi CentOS 7 ise.*, aşağıdaki komutu root izinleriyle Azure VM üzerinde çalıştırın. Komuttan sonra VM'yi yeniden başlatın.

        grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg

## <a name="unexpected-shutdown-message-event-id-6008"></a>Beklenmeyen kapatma iletisi (Olay Kimliği 6008)

Bir Windows VM gönderisi başarısızlığa önyükleme yaparken, kurtarılan VM'de beklenmeyen bir kapatma iletisi alırsanız, başarısız olmak için kullanılan kurtarma noktasında vm kapatma durumunun yakalanmadığını gösterir. Bu, VM'nin tam olarak kapatılmadığını bir noktaya kurtardığınızda gerçekleşir.

Bu normalde endişe için bir neden değildir ve genellikle planlanmamış failovers için göz ardı edilebilir. Başarısız lık planlanıyorsa, VM'nin başarısız olmadan önce düzgün bir şekilde kapatıldığından emin olun ve bekleyen çoğaltma verilerinin azure'a gönderilmesi için yeterli zaman sağlayın. Ardından, Azure'da bekleyen verilerin bir kurtarma noktasına işlenmesi için [Failover ekranındaki](site-recovery-failover.md#run-a-failover) **En Son** seçeneğini kullanın ve bu seçeneği kullanın ve bu seçeneği vm failover için kullanın.

## <a name="unable-to-select-the-datastore"></a>Datastore'u seçemiyor

Bu sorun, azure'daki veri mağazasını göremediğinizde, başarısız olan sanal makineyi yeniden korumaya çalışırken portalı göremediğinizde belirtilir. Bunun nedeni, Azure Site Kurtarma'ya eklenen vCenters altında Master hedefinin sanal bir makine olarak tanınmamasıdır.

Bir titreşim makinesini yeniden koruma hakkında daha fazla bilgi için, Azure'a geçemeden yeniden koruma ve makineleri şirket içi bir siteye geri alma konusunda başarısız [olun.](vmware-azure-reprotect.md)

Sorunu gidermek için:

Kaynak makinenizi yöneten vCenter'da Ana hedefi el ile oluşturun. Datastore sonraki vCenter bulma ve yenileme kumaş işlemleri sonra kullanılabilir olacaktır.

> [!Note]
> 
> Bulma ve yenileme kumaş işlemleri tamamlamak için 30 dakika kadar sürebilir. 

## <a name="linux-master-target-registration-with-cs-fails-with-an-ssl-error-35"></a>CS ile Linux Master Target kaydı Bir SSL hatası 35 ile başarısız olur 

Yapılandırma sunucusuyla azure site kurtarma yöneticisi hedef kaydı, Orijinalleştirilmiş Proxy'nin Ana Hedef'te etkinleştirilmesi nedeniyle başarısız olur. 
 
Bu hata yükleme günlüğünde aşağıdaki dizeleri ile gösterilir: 

```
RegisterHostStaticInfo encountered exception config/talwrapper.cpp(107)[post] CurlWrapper Post failed : server : 10.38.229.221, port : 443, phpUrl : request_handler.php, secure : true, ignoreCurlPartialError : false with error: [at curlwrapperlib/curlwrapper.cpp:processCurlResponse:231]   failed to post request: (35) - SSL connect error. 
```

Sorunu gidermek için:
 
1. Yapılandırma sunucusu VM'de bir komut istemi açın ve aşağıdaki komutları kullanarak proxy ayarlarını doğrulayın:

    kedi /etc/environment echo $http_proxy yankı $https_proxy 

2. Önceki komutların çıktısı http_proxy veya https_proxy ayarlarının tanımlandığını gösteriyorsa, yapılandırma sunucusuyla Ana Hedef iletişiminin engelini kaldırmak için aşağıdaki yöntemlerden birini kullanın:
   
   - [PsExec aracını](https://aka.ms/PsExec)indirin.
   - Sistem kullanıcı bağlamına erişmek ve proxy adresinin yapılandırılıp yapılandırılmadığını belirlemek için aracı kullanın. 
   - Proxy yapılandırılırsa, PsExec aracını kullanarak iE'yi sistem kullanıcı bağlamında açın.
  
     **psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"**

   - Ana hedef sunucunun yapılandırma sunucusuyla iletişim kurabilmesi için:
  
     - Ana Hedef sunucu IP adresini proxy üzerinden atlamak için Internet Explorer'daki proxy ayarlarını değiştirin.   
     Veya
     - Ana Hedef sunucusundaki proxy'yi devre dışı kaldırın. 


## <a name="next-steps"></a>Sonraki adımlar
- Windows [VM'ye RDP bağlantısı](../virtual-machines/windows/troubleshoot-rdp-connection.md) sorunu giderme
- Linux [VM'ye Sorun](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md) Giderme SSH bağlantısı

Daha fazla yardıma ihtiyacınız varsa, sorgunuzu [Site Kurtarma forumunda](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) yayınlayın veya bu belgenin sonunda bir yorum bırakın. Size yardımcı olabilecek aktif bir topluluğumuz var.
