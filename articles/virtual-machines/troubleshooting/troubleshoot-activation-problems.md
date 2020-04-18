---
title: Azure'da Windows sanal makine etkinleştirme sorunlarını giderme| Microsoft Dokümanlar
description: Azure'da Windows sanal makine etkinleştirme sorunlarını gidermek için sorun giderme adımları sağlar
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
ms.openlocfilehash: fd38f646b8dfc58839cd2645f7fadf7332693854
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605997"
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a>Azure Windows sanal makinesi etkinleştirme sorunlarını giderme

Özel bir görüntüden oluşturulan Azure Windows sanal makinesini (VM) etkinleştirirken sorun yaşıyorsanız, sorunu gidermek için bu belgede sağlanan bilgileri kullanabilirsiniz. 

## <a name="understanding-azure-kms-endpoints-for-windows-product-activation-of-azure-virtual-machines"></a>Azure Sanal Makinelerin Windows ürün etkinleştirmesi için Azure KMS uç noktalarını anlama

Azure, VM'nin bulunduğu bulut bölgesine bağlı olarak KMS (Anahtar Yönetim Hizmetleri) etkinleştirme için farklı uç noktalar kullanır. Bu sorun giderme kılavuzunu kullanırken, bölgeniz için geçerli olan uygun KMS bitiş noktasını kullanın.

* Azure genel bulut bölgeleri: kms.core.windows.net:1688
* Azure China 21Vianet ulusal bulut bölgeleri: kms.core.chinacloudapi.cn:1688
* Azure Almanya ulusal bulut bölgeleri: kms.core.cloudapi.de:1688
* Azure ABD Gov ulusal bulut bölgeleri: kms.core.usgovcloudapi.net:1688

## <a name="symptom"></a>Belirti

Bir Azure Windows VM'sini etkinleştirmeye çalıştığınızda, aşağıdaki örneğe benzeyen bir hata iletisi alırsınız:

**Hata: 0xC004F074 Yazılım LisansHizmeti bilgisayarın etkinleştirilemediğini bildirdi. Anahtar Yönetim Hizmeti (KMS) ile bağlantı kurulamadı. Ek bilgi için lütfen Uygulama Etkinlik Günlüğü'ne bakın.**

## <a name="cause"></a>Nedeni

Azure VM etkinleştirme sorunları genellikle Windows VM uygun KMS istemci ayarı anahtarı kullanılarak yapılandırılmadığında veya Windows VM ile Azure KMS hizmeti (kms.core.windows.net, bağlantı noktası 1688) arasında bir bağlantı sorunu olduğunda oluşur. 

## <a name="solution"></a>Çözüm

>[!NOTE]
>Siteden siteye VPN ve zorunlu tünel leme kullanıyorsanız, [kms etkinleştirmesini zorunlu tünelleme yle etkinleştirmek için Azure özel yollarını kullanın'a](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling)bakın. 
>
>ExpressRoute kullanıyorsanız ve yayınlanan varsayılan bir rotanız varsa, bkz. [ExpressRoute devrelerine bağlı sanal ağlara Internet bağlantısını engelleyebilir miyim?](https://docs.microsoft.com/azure/expressroute/expressroute-faqs)

### <a name="step-1-configure-the-appropriate-kms-client-setup-key"></a>Adım 1 Uygun KMS istemci kurulum anahtarını yapılandırma

Özel bir görüntüden oluşturulan VM için, VM için uygun KMS istemci kurulum anahtarını yapılandırmanız gerekir.

1. **slmgr.vbs /dlv'yi** yüksek komut istemiyle çalıştırın. Çıktıdaki Açıklama değerini denetleyin ve ardından perakende (PERAKENDE kanalı) veya hacim (VOLUME_KMSCLIENT) lisans medyasından oluşturulup oluşturulmadığını belirleyin:
  

    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. **slmgr.vbs /dlv** alanında RETAIL kanalı gösteriliyorsa, [KMS istemci ayarı anahtarını](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) kullanılmakta olan Windows Server sürümüne göre ayarlamak için aşağıdaki komutları çalıştırın ve etkinleştirmeyi yeniden denemeye zorlayın: 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    Örneğin, Windows Server 2016 Datacenter için aşağıdaki komutu çalıştırabilirsiniz:

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-the-connectivity-between-the-vm-and-azure-kms-service"></a>Adım 2 VM ve Azure KMS hizmeti arasındaki bağlantıyı doğrulayın

1. [PSping](https://docs.microsoft.com/sysinternals/downloads/psping) aracını VM'de etkinleştirmeyen yerel bir klasöre indirin ve ayıklayın. 

2. Başlat'a gidin, Windows PowerShell'de arama yapın, Windows PowerShell'i sağ tıklatın ve ardından yönetici olarak Çalıştır'ı seçin.

3. VM’nin doğru Azure KMS sunucusunu kullanacak şekilde yapılandırıldığından emin olun. Bunu yapmak için aşağıdaki komutu çalıştırın:
  
    ```powershell
    Invoke-Expression "$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms kms.core.windows.net:1688"
    ```

    Komut geri dönmelidir: Anahtar Yönetimi Hizmeti makine adı başarıyla kms.core.windows.net:1688 için ayarlanmış.

4. Psping kullanarak KMS sunucusuna bağlandığınızı doğrulayın. Pstools.zip dosyasını ayıkladığınız klasöre geçin ve sonra aşağıdaki komutu çalıştırın:
  
    ```
    \psping.exe kms.core.windows.net:1688
    ```
   Çıktının ikinciden sona satırında, gördüğünüzden emin olun: Gönderilen = 4, Alınan = 4, Kayıp = 0 (%0 kayıp).

   Lost 0'dan (sıfır) büyükse, VM'nin KMS sunucusuna bağlantısı yoktur. Bu durumda, VM sanal bir ağdaysa ve özel bir DNS sunucusu belirtilmişse, DNS sunucusunun kms.core.windows.net çözebildiğinizden emin olmalısınız. Veya DNS sunucusunu kms.core.windows.net çözen bir sunucuyla değiştirin.

   Tüm DNS sunucularını sanal ağdan kaldırırsanız, Sanal Bilgisayarlar Azure'un dahili DNS hizmetini kullanır. Bu hizmet kms.core.windows.net çözebilir.
  
    Ayrıca, 1688 bağlantı noktası ile KMS bitiş noktasına giden ağ trafiğinin VM'deki güvenlik duvarı tarafından engellenmediğinden emin olun.

5. Network [Watcher Next Hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) kullanarak, söz konusu VM'den hedef IP 23.102.135.246 'ya (kms.core.windows.net için) veya bölgenize uygulanan uygun KMS bitiş noktasının IP'sinin **Internet**olduğunu doğrulayın.  Sonuç VirtualAppliance veya VirtualNetworkGateway ise, varsayılan bir rota var olabilir.  Doğru eylem rotasını belirlemek için ağ yöneticinize başvurun ve onlarla birlikte çalışın.  Bu çözüm kuruluşunuzun ilkeleriyle tutarlıysa, bu özel bir [rota](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/custom-routes-enable-kms-activation) olabilir.

6. kms.core.windows.net bağlantısının başarılı olduğunu doğruladıktan sonra, yükseltilmiş Windows PowerShell isteminde aşağıdaki komutu çalıştırın. Bu komut, etkinleştirmeyi birden çok kez dener.

    ```powershell
    1..12 | ForEach-Object { Invoke-Expression "$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato" ; start-sleep 5 }
    ```

    Başarılı bir etkinleştirme aşağıdakine benzer bilgileri döndürür:
    
    **Windows(R) aktive, ServerDatacenter sürümü (12345678-1234-1234-1234-12345678) ...  Ürün başarıyla etkinleştirildi.**

## <a name="faq"></a>SSS 

### <a name="i-created-the-windows-server-2016-from-azure-marketplace-do-i-need-to-configure-kms-key-for-activating-the-windows-server-2016"></a>Windows Server 2016'yı Azure Marketi'nden oluşturdum. Windows Server 2016'yı etkinleştirmek için KMS tuşunu yapılandırmam gerekiyor mu? 

 
Hayır. Azure Marketi'ndeki resimde zaten yapılandırılmış uygun KMS istemci kurulum anahtarı vardır. 

### <a name="does-windows-activation-work-the-same-way-regardless-if-the-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a>VM Azure Karma Kullanım Avantajı (HUB) kullanıyorsa veya kullanmıyorsa, Windows etkinleştirme de aynı şekilde çalışıyor mu? 

 
Evet. 
 

### <a name="what-happens-if-windows-activation-period-expires"></a>Windows etkinleştirme süresi sona ererse ne olur? 

 
Yetkisiz kullanım süresi dolduğunda ve Windows hala etkinleştirilmediğinde, Windows Server 2008 R2 ve daha sonraki Windows sürümleri etkinleştirme yle ilgili ek bildirimler gösterir. Masaüstü duvar kağıdı siyah kalır ve Windows Update yalnızca güvenlik ve kritik güncelleştirmeleri yükler, ancak isteğe bağlı güncelleştirmeleri yüklemez. [Lisans Koşulları](https://technet.microsoft.com/library/ff793403.aspx) sayfasının altındaki Bildirimler bölümüne bakın.   

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.

Yine de yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
