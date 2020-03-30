---
title: Azure Site Kurtarma ile Azure ile Azure olağanüstü durum kurtarma bağlantısı nın giderme
description: Azure VM olağanüstü durum kurtarmada bağlantı sorunlarını giderme
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 08/05/2019
ms.openlocfilehash: b082e1aca094dcb335a7268e4c116376d756fd3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80292020"
---
# <a name="troubleshoot-azure-to-azure-vm-network-connectivity-issues"></a>Azure'dan Azure'a VM ağ bağlantısı sorunları yla ilgili sorun giderme

Bu makalede, Azure sanal makinelerini bir bölgeden başka bir bölgeye kopyalayıp kurtardığınızda ağ bağlantısıyla ilgili yaygın sorunlar açıklanmaktadır. Ağ gereksinimleri hakkında daha fazla bilgi için [Azure VM'lerini çoğaltmak için bağlantı gereksinimlerine](azure-to-azure-about-networking.md)bakın.

Site Kurtarma çoğaltmasının çalışması için VM'den belirli URL'lere veya IP aralıklarına giden bağlantı gereklidir. VM'niz bir güvenlik duvarının arkasındaysa veya giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kurallarını kullanıyorsa, bu sorunlardan biriyle karşılaşabilirsiniz.

**URL** | **Şey**  
--- | ---
*.blob.core.windows.net | Verilerin VM'den kaynak bölgedeki önbellek depolama hesabına yazılabilmesi için gereklidir. VM'lerinizin tüm önbellek depolama hesaplarını biliyorsanız, *.blob.core.windows.net yerine belirli depolama hesabı URL'lerini (örneğin, cache1.blob.core.windows.net ve cache2.blob.core.windows.net) listeleyebilirsiniz
login.microsoftonline.com | Site Kurtarma hizmet URL'lerine yetkilendirme ve kimlik doğrulama için gereklidir.
*.hypervrecoverymanager.windowsazure.com | VM'den Site Kurtarma hizmeti iletişiminin gerçekleşebilmeleri için gereklidir. Güvenlik duvarı proxy'niz IP'leri destekliyorsa ilgili 'Site Kurtarma IP'sini kullanabilirsiniz.
*.servicebus.windows.net | Site Kurtarma izleme ve tanılama verilerinin VM'den yazılabilmesi için gereklidir. Güvenlik duvarı proxy'niz IP'leri destekliyorsa ilgili 'Site Kurtarma İzleme IP'sini kullanabilirsiniz.

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site Kurtarma URL'leri veya IP aralıkları için giden bağlantı (hata kodu 151037 veya 151072)

## <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a><a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Sorun 1: Azure sanal makinesini Site Kurtarma (151195) ile kaydedileme </br>
- **Olası nedeni** </br>
  - DNS çözüm hatası nedeniyle Site Kurtarma uç noktalarına bağlantı kurulamaz.
  - Sanal makine yükünü devretmenize rağmen DNS sunucusuna DR bölgesinden erişilemediğinde yeniden koruma sırasında bu durum daha sık görülür.

- **Çözünürlük**
   - Özel DNS kullanıyorsanız, DNS sunucusuna Olağanüstü Durum Kurtarma bölgesinden erişilebildiğinden emin olun. Özel bir DNS'niz olup olmadığını kontrol etmek için VM> Olağanüstü Durum Kurtarma ağına> DNS sunucularına gidin. DNS sunucusuna sanal makineden erişmeyi deneyin. Erişilemiyorsa, DNS sunucusu üzerinden başarısız olarak veya DR ağı ile DNS arasındaki site hattını oluşturarak erişilebilir hale getirin.

    ![com hatası](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)


## <a name="issue-2-site-recovery-configuration-failed-151196"></a>Sorun 2: Site Kurtarma yapılandırması başarısız oldu (151196)

> [!NOTE]
> Sanal makineler **Standart** dahili yük dengeleyicisinin arkasındaysa, varsayılan olarak O365 IP'lerine (yani login.microsoftonline.com) erişimi olmaz. **Temel** dahili yük dengeleyici türüne değiştirin veya [makalede](https://aka.ms/lboutboundrulescli)belirtildiği gibi giden erişim oluşturun.

- **Olası nedeni** </br>
  - Office 365 kimlik doğrulama ve kimlik IP4 uç noktalarına bağlantı kurulamaz.

- **Çözünürlük**
  - Azure Site Kurtarma kimlik doğrulaması için Office 365 IP aralıklarına erişim gerekir.
    VM'de giden ağ bağlantısını denetlemek için Azure Ağ güvenlik grubu (NSG) kurallarını /güvenlik duvarı proxy'sini kullanıyorsanız, O365 IPranges ile iletişime izin verdiniz. Azure AD'ye karşılık gelen tüm IP adreslerine erişime izin vermek için [Azure Active Directory (Azure AD) hizmet etiketi](../virtual-network/security-overview.md#service-tags) tabanlı NSG kuralı oluşturun
      - Gelecekte Azure AD'ye yeni adresler eklenirse, yeni NSG kuralları oluşturmanız gerekir.

### <a name="example-nsg-configuration"></a>Örnek NSG yapılandırması

Bu örnek, bir VM'nin çoğaltması için NSG kurallarının nasıl yapılandırılabildiğini gösterir.

- Giden bağlantıyı kontrol etmek için NSG kurallarını kullanıyorsanız, gerekli tüm IP adres aralıkları için "HTTPS giden gidene izin ver" kurallarını bağlantı noktasına:443'ü kullanın.
- Örnek, VM kaynak konumunun "Doğu ABD" ve hedef konumun "Orta ABD" olduğunu varsayılr.

### <a name="nsg-rules---east-us"></a>NSG kuralları - Doğu ABD

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG'deki "Storage.EastUS" için giden HTTPS (443) güvenlik kuralı oluşturun.

      ![depolama etiketi](./media/azure-to-azure-about-networking/storage-tag.png)

2. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG'deki "AzureActiveDirectory" için giden HTTPS (443) güvenlik kuralı oluşturun.

      ![aad etiketi](./media/azure-to-azure-about-networking/aad-tag.png)

3. Hedef konuma karşılık gelen Site Kurtarma IP'leri için giden HTTPS (443) kuralları oluşturun:

   **Konum** | **Site Kurtarma IP adresi** |  **Site Kurtarma izleme IP adresi**
    --- | --- | ---
   Orta ABD | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>NSG kuralları - Orta ABD

Çoğaltmanın hedef bölgeden kaynak bölgeye geçemedisonrası etkinleştirilebilmek için bu kurallar gereklidir:

1. NSG'de "Storage.CentralUS" için giden HTTPS (443) güvenlik kuralı oluşturun.

2. NSG'de "AzureActiveDirectory" için giden HTTPS (443) güvenlik kuralı oluşturun.

3. Kaynak konuma karşılık gelen Site Kurtarma IP'leri için giden HTTPS (443) kuralları oluşturun:

   **Konum** | **Site Kurtarma IP adresi** |  **Site Kurtarma izleme IP adresi**
    --- | --- | ---
   Orta ABD | 13.82.88.226 | 104.45.147.24
## <a name="issue-3-site-recovery-configuration-failed-151197"></a>Sorun 3: Site Kurtarma yapılandırması başarısız oldu (151197)
- **Olası nedeni** </br>
  - Azure Site Kurtarma hizmeti uç noktalarına bağlantı kurulamaz.

- **Çözünürlük**
  - Azure Site Recovery bölgeye bağlı olarak [Site Recovery IP aralıklarına](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-using-service-tags) erişim gerektiriyor. Gerekli IP aralıklarının sanal makineden erişilebildiğinden emin olun.


## <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>Sorun 4: Ağ trafiği şirket içi proxy sunucusundan geçtiğinde A2A çoğaltma başarısız oldu (151072)
- **Olası nedeni** </br>
  - Özel proxy ayarları geçersizdir ve Azure Site Kurtarma Mobilite Hizmeti aracısı IE'den proxy ayarlarını otomatik olarak algılamadı


- **Çözünürlük**
  1. Mobilite Hizmeti aracısı, Windows'ta IE'den proxy ayarlarını algılar ve Linux'ta /etc/environment.
  2. Proxy'yi yalnızca Azure Sitesi Kurtarma Mobilite Hizmeti için ayarlamayı tercih ederseniz, proxyinfo.conf adresinde bulunan proxy ayrıntılarını sağlayabilirsiniz:</br>
     - ``/usr/local/InMage/config/``***Linux*** üzerinde
     - ``C:\ProgramData\Microsoft Azure Site Recovery\Config``***Windows'da***
  3. ProxyInfo.conf dosyasında ara sunucu ayarları şu INI biçiminde bulunmalıdır.</br>
                *[proxy]*</br>
                *Adres=http://1.2.3.4*</br>
                *Bağlantı noktası=567*</br>
  4. Azure Site Kurtarma Mobilite Hizmeti aracısı yalnızca ***kimlik doğrulamamış vekilleri***destekler.

### <a name="fix-the-problem"></a>Sorunu çözme
Gerekli [URL'lere](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) veya [gerekli IP aralıklarına](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags)izin vermek için, [ağ kılavuz belgesindeki](site-recovery-azure-to-azure-networking-guidance.md)adımları izleyin.


## <a name="next-steps"></a>Sonraki adımlar
[Azure sanal makinelerini çoğaltma](site-recovery-replicate-azure-to-azure.md)
