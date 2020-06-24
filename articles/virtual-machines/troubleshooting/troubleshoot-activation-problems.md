---
title: Azure 'da Windows sanal makine etkinleştirme sorunlarını giderme | Microsoft Docs
description: Azure 'da Windows sanal makine etkinleştirme sorunlarını gidermeye yönelik sorun giderme adımlarını sağlar
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 11/15/2018
ms.author: genli
ms.openlocfilehash: 44c86dae3c7df8293404c253b94164c37d574158
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84736943"
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a>Azure Windows sanal makinesi etkinleştirme sorunlarını giderme

Özel görüntüden oluşturulan Azure Windows sanal makinesini (VM) etkinleştirirken sorun yaşıyorsanız, sorunu gidermek için bu belgede belirtilen bilgileri kullanabilirsiniz. 

## <a name="understanding-azure-kms-endpoints-for-windows-product-activation-of-azure-virtual-machines"></a>Azure Sanal Makinelerin Windows ürün etkinleştirmesi için Azure KMS uç noktalarını anlama

Azure, sanal makinenin bulunduğu bulut bölgesine bağlı olarak KMS için farklı uç noktalar (Anahtar Yönetim Hizmetleri) etkinleştirme kullanır. Bu sorun giderme kılavuzunu kullanırken, bölgeniz için geçerli olan uygun KMS uç noktasını kullanın.

* Azure genel bulut bölgeleri: kms.core.windows.net:1688
* Azure Çin 21Vianet Ulusal bulut bölgeleri: kms.core.chinacloudapi.cn:1688
* Azure Almanya Ulusal bulut bölgeleri: kms.core.cloudapi.de:1688
* Azure US Gov Ulusal bulut bölgeleri: kms.core.usgovcloudapi.net:1688

## <a name="symptom"></a>Belirti

Azure Windows VM 'yi etkinleştirmeye çalıştığınızda, aşağıdaki örneğe benzer bir hata iletisi alırsınız:

**Hata: 0xC004F074 yazılım LicensingService bilgisayarın etkinleştirilemediğini bildirdi. Anahtar Yönetim hizmeti (KMS) ile iletişim kurulamadı. Daha fazla bilgi için lütfen uygulama olay günlüğü 'ne bakın.**

## <a name="cause"></a>Nedeni

Azure VM etkinleştirme sorunları genellikle Windows VM uygun KMS istemci ayarı anahtarı kullanılarak yapılandırılmadığında veya Windows VM ile Azure KMS hizmeti (kms.core.windows.net, bağlantı noktası 1688) arasında bir bağlantı sorunu olduğunda oluşur. 

## <a name="solution"></a>Çözüm

>[!NOTE]
>Siteden siteye VPN ve Zorlamalı tünel kullanıyorsanız, bkz. [zorlamalı tünelle KMS etkinleştirmesini etkinleştirmek Için Azure özel yollarını kullanma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling). 
>
>ExpressRoute kullanıyorsanız ve varsayılan bir yol yayımladıysanız, bkz. [ExpressRoute devrelerine bağlı sanal ağlarla Internet bağlantısını engelleyebilir miyim?](https://docs.microsoft.com/azure/expressroute/expressroute-faqs).

### <a name="step-1-configure-the-appropriate-kms-client-setup-key"></a>1. adım uygun KMS istemci kurulum anahtarını yapılandırma

Özel görüntüden oluşturulan sanal makine için, sanal makine için uygun KMS istemci kurulum anahtarını yapılandırmanız gerekir.

1. Yükseltilmiş bir komut isteminde **slmgr. vbs/dlv** komutunu çalıştırın. Çıktıda açıklama değerini kontrol edin ve ardından perakende (perakende kanalı) veya toplu (VOLUME_KMSCLIENT) lisans medyasından oluşturulup oluşturulmayacağını saptayın.
  

    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. **slmgr.vbs /dlv** alanında RETAIL kanalı gösteriliyorsa, [KMS istemci ayarı anahtarını](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) kullanılmakta olan Windows Server sürümüne göre ayarlamak için aşağıdaki komutları çalıştırın ve etkinleştirmeyi yeniden denemeye zorlayın: 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    Örneğin, Windows Server 2016 Datacenter için aşağıdaki komutu çalıştırın:

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-the-connectivity-between-the-vm-and-azure-kms-service"></a>2. adım VM ile Azure KMS hizmeti arasındaki bağlantıyı doğrulama

1. Etkin olmayan VM 'deki yerel bir klasöre [Psping](https://docs.microsoft.com/sysinternals/downloads/psping) aracını indirip ayıklayın. 

2. Başlat ' a gidin, Windows PowerShell üzerinde arama yapın, Windows PowerShell ' e sağ tıklayın ve ardından yönetici olarak Çalıştır ' ı seçin.

3. VM’nin doğru Azure KMS sunucusunu kullanacak şekilde yapılandırıldığından emin olun. Bunu yapmak için aşağıdaki komutu çalıştırın:
  
    ```powershell
    Invoke-Expression "$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms kms.core.windows.net:1688"
    ```

    Komut şunu döndürmelidir: anahtar yönetim hizmeti makine adı kms.core.windows.net:1688 başarıyla ayarlandı.

4. Psping kullanarak KMS sunucusuna bağlandığınızı doğrulayın. Pstools.zip dosyasını ayıkladığınız klasöre geçin ve sonra aşağıdaki komutu çalıştırın:
  
    ```
    .\psping.exe kms.core.windows.net:1688
    ```
   Çıktının ikinci-son satırına şunu görtığınızdan emin olun: gönderilen = 4, alınan = 4, kayıp = 0 (%0 kayıp).

   Kayıp 0 ' dan büyükse (sıfır), VM 'nin KMS sunucusuna bağlantısı yoktur. Bu durumda, VM bir sanal ağda ise ve özel bir DNS sunucusu belirtildiyse, DNS sunucusunun kms.core.windows.net çözümleyebileceğinden emin olmanız gerekir. Veya, DNS sunucusunu kms.core.windows.net çözümleyecek bir şekilde değiştirin.

   Sanal bir ağdan tüm DNS sunucularını kaldırırsanız VM 'Lerin Azure 'un iç DNS hizmetini kullanmasını unutmayın. Bu hizmet, kms.core.windows.net çözümleyebilir.
  
    Ayrıca, 1688 bağlantı noktası ile KMS uç noktasına giden ağ trafiğinin VM 'deki güvenlik duvarı tarafından engellenmediğinden emin olun.

5. [Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) 'ni kullanarak bir sonraki atlama türünün, söz konusu VM 'den hedef IP 23.102.135.246 (KMS.Core.Windows.NET için) veya bölgeniz için geçerli olan uygun KMS uç noktasının **IP 'si ile**bir sonraki atlama türünü kullanmayı doğrulayın.  Sonuç VirtualAppliance veya VirtualNetworkGateway ise, bu, büyük olasılıkla varsayılan bir yol olabilir.  Doğru eylem arasındaki kursu öğrenmek için ağ yöneticinize başvurun ve bunlarla çalışın.  Bu çözüm kuruluşunuzun ilkeleriyle tutarlıdır, bu [özel bir yol](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/custom-routes-enable-kms-activation) olabilir.

6. kms.core.windows.net bağlantısının başarılı olduğunu doğruladıktan sonra, yükseltilmiş Windows PowerShell isteminde aşağıdaki komutu çalıştırın. Bu komut, etkinleştirmeyi birden çok kez dener.

    ```powershell
    1..12 | ForEach-Object { Invoke-Expression "$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato" ; start-sleep 5 }
    ```

    Başarılı bir etkinleştirme aşağıdakine benzer bilgileri döndürür:
    
    **Windows (R), ServerDatacenter Edition (12345678-1234-1234-1234-12345678) etkinleştiriliyor...  Ürün başarıyla etkinleştirildi.**

## <a name="faq"></a>SSS 

### <a name="i-created-the-windows-server-2016-from-azure-marketplace-do-i-need-to-configure-kms-key-for-activating-the-windows-server-2016"></a>Azure Marketi 'nden Windows Server 2016 oluşturdum. Windows Server 2016 ' I etkinleştirmek için KMS anahtarını yapılandırmam gerekir mi? 

 
Hayır. Azure Marketi 'ndeki görüntüde uygun KMS istemci kurulum anahtarı zaten yapılandırılmış. 

### <a name="does-windows-activation-work-the-same-way-regardless-if-the-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a>Windows etkinleştirme, VM 'nin Azure hibrit kullanım teklifi 'Ni (HUB) kullanıyor olmasına bakılmaksızın aynı şekilde çalışır mı? 

 
Evet. 
 

### <a name="what-happens-if-windows-activation-period-expires"></a>Windows etkinleştirme süresi sona erdiğinde ne olur? 

 
Yetkisiz kullanım süresi dolduğunda ve Windows hala etkinleştirilmemişse, Windows Server 2008 R2 ve sonraki Windows sürümlerinde, etkinleştirme ile ilgili ek bildirimler gösterilir. Masaüstü duvar kağıdı siyah kalır ve Windows Update yalnızca güvenlik ve kritik güncelleştirmeleri yükler, ancak isteğe bağlı güncelleştirmeleri yüklemez. [Lisanslama Koşulları](https://technet.microsoft.com/library/ff793403.aspx) sayfasının altındaki bildirimler bölümüne bakın.   

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.

Yine de yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
