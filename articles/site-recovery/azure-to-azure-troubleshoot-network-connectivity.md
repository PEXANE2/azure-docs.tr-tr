---
title: Azure Site Kurtarma ile Azure ile Azure olağanüstü durum kurtarma bağlantısı nın giderme
description: Azure VM olağanüstü durum kurtarmada bağlantı sorunlarını giderme
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 04/06/2020
ms.openlocfilehash: d2cc4133e52e7cab812413d23948da6ac2660e77
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80884877"
---
# <a name="troubleshoot-azure-to-azure-vm-network-connectivity-issues"></a>Azure'dan Azure'a VM ağ bağlantısı sorunları yla ilgili sorun giderme

Bu makalede, Azure sanal makinelerini (VM) bir bölgeden başka bir bölgeye çoğaltTığınızda ve kurtardığınızda ağ bağlantısıyla ilgili yaygın sorunlar açıklanmaktadır. Ağ gereksinimleri hakkında daha fazla bilgi için [Azure VM'lerini çoğaltmak için bağlantı gereksinimlerine](azure-to-azure-about-networking.md)bakın.

Site Kurtarma çoğaltmasının çalışması için VM'den belirli URL'lere veya IP aralıklarına giden bağlantı gereklidir. VM'niz bir güvenlik duvarının arkasındaysa veya giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kurallarını kullanıyorsa, bu sorunlardan biriyle karşılaşabilirsiniz.

| URL'si | Ayrıntılar |
|---|---|
| `*.blob.core.windows.net` | Verilerin VM'den kaynak bölgedeki önbellek depolama hesabına yazılabilmesi için gereklidir. VM'lerinizin tüm önbellek depolama hesaplarını biliyorsanız, belirli depolama hesabı URL'leri için izin listesi kullanabilirsiniz. Örneğin, `cache1.blob.core.windows.net` ve `cache2.blob.core.windows.net` yerine `*.blob.core.windows.net`. |
| `login.microsoftonline.com` | Site Kurtarma hizmet URL'lerine yetkilendirme ve kimlik doğrulama için gereklidir. |
| `*.hypervrecoverymanager.windowsazure.com` | VM'den Site Kurtarma hizmeti iletişiminin gerçekleşebilmeleri için gereklidir. Güvenlik duvarı proxy'niz IP'leri destekliyorsa, ilgili _Site Kurtarma IP'sini_ kullanabilirsiniz. |
| `*.servicebus.windows.net` | Site Kurtarma izleme ve tanılama verilerinin VM'den yazılabilmesi için gereklidir. Güvenlik duvarı proxy'niz IP'leri destekliyorsa, ilgili _Site Kurtarma İzleme IP'sini_ kullanabilirsiniz. |

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site Kurtarma URL'leri veya IP aralıkları için giden bağlantı (hata kodu 151037 veya 151072)

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195"></a>Sorun 1: Azure sanal makinesini Site Kurtarma (151195) ile kaydedileme

#### <a name="possible-cause"></a>Olası nedeni

Alan Adı Sistemi (DNS) çözüm hatası nedeniyle Site Kurtarma uç noktalarına bağlantı kurulamaz. Bu sorun, VM üzerinde başarısız olduğunuzda yeniden koruma sırasında daha yaygındır, ancak DNS sunucusuna olağanüstü durum kurtarma (DR) bölgesinden erişilemez.

#### <a name="resolution"></a>Çözüm

Özel DNS kullanıyorsanız, DNS sunucusuna olağanüstü durum kurtarma bölgesinden erişilebildiğinden emin olun.

VM'nin özel bir DNS ayarı kullanıp kullanmayıp kullanmamasını denetlemek için:

1. **Sanal makineleri** açın ve VM'yi seçin.
1. VM **Ayarları'na** gidin ve **Ağ'ı**seçin.
1. **Sanal ağ/alt ağda,** sanal ağın kaynak sayfasını açmak için bağlantıyı seçin.
1. **Ayarlar'a** gidin ve **DNS sunucularını**seçin.

Sanal makineden DNS sunucusuna erişmeye çalışın. DNS sunucusuna erişilemiyorsa, DNS sunucusu üzerinde başarısız olarak veya DR ağı ile DNS arasındaki site hattını oluşturarak erişilebilir hale getirin.

  :::image type="content" source="./media/azure-to-azure-troubleshoot-errors/custom_dns.png" alt-text="com hatası":::

### <a name="issue-2-site-recovery-configuration-failed-151196"></a>Sorun 2: Site Kurtarma yapılandırması başarısız oldu (151196)

> [!NOTE]
> VM'ler **standart** bir iç yük dengeleyicisinin arkasındaysa, varsayılan olarak Office 365 IP'lerine `login.microsoftonline.com`erişemez. Temel **dahili** yük dengeleyici türüne değiştirin veya Azure [CLI'yi kullanarak Standart Yük Dengeleyicisi'nde yük dengeleme ve giden kuralları yapılandırın](/azure/load-balancer/configure-load-balancer-outbound-cli)makalede belirtildiği gibi giden erişim oluşturun.

#### <a name="possible-cause"></a>Olası nedeni

Office 365 kimlik doğrulama ve kimlik IP4 uç noktalarına bağlantı kurulamaz.

#### <a name="resolution"></a>Çözüm

- Azure Site Kurtarma, kimlik doğrulaması için Office 365 IP aralıklarına erişim gerektirir.
- VM'de giden ağ bağlantısını denetlemek için Azure Ağı güvenlik grubu (NSG) kurallarını /güvenlik duvarı proxy'sini kullanıyorsanız, Office 365 IP aralıklarına iletişime izin verdiniz. Azure AD'ye karşılık gelen tüm IP adreslerine erişimsağlayan [Bir Azure Etkin Dizin (Azure AD) hizmet etiketi](/azure/virtual-network/security-overview#service-tags) tabanlı NSG kuralı oluşturun.
- Gelecekte Azure AD'ye yeni adresler eklenirse, yeni NSG kuralları oluşturmanız gerekir.

### <a name="example-nsg-configuration"></a>Örnek NSG yapılandırması

Bu örnek, bir VM'nin çoğaltması için NSG kurallarının nasıl yapılandırılabildiğini gösterir.

- Giden bağlantıyı kontrol etmek için NSG kurallarını kullanıyorsanız, gerekli tüm IP adresi aralıkları için 443 bağlantı noktasına HTTPS giden kurallara **izin ver'i** kullanın.
- Örnek, VM kaynak konumunun **Doğu ABD** ve hedef konumun **Merkezi ABD**olduğunu varsayılır.

#### <a name="nsg-rules---east-us"></a>NSG kuralları - Doğu ABD

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG için bir HTTPS giden güvenlik kuralı oluşturun. Bu örnekte **Hedef servis etiketi**kullanır: _Storage.EastUS_ ve **Hedef bağlantı noktası aralıkları**: _443_.

     :::image type="content" source="./media/azure-to-azure-about-networking/storage-tag.png" alt-text="depolama etiketi":::

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG için bir HTTPS giden güvenlik kuralı oluşturun. Bu örnekte **Hedef hizmet etiketi**kullanır: _AzureActiveDirectory_ ve **Hedef bağlantı noktası aralıkları**: _443_.

     :::image type="content" source="./media/azure-to-azure-about-networking/aad-tag.png" alt-text="aad etiketi":::

1. Hedef konuma karşılık gelen Site Kurtarma IP'leri için HTTPS bağlantı noktası 443 giden kuralları oluşturun:

   | Konum | Site Kurtarma IP adresi | Site Kurtarma izleme IP adresi |
   | --- | --- | --- |
   | Orta ABD | 40.69.144.231 | 52.165.34.144 |

#### <a name="nsg-rules---central-us"></a>NSG kuralları - Orta ABD

Bu örnekte, çoğaltmanın hedef bölgeden kaynak bölgeye başarısız sonrası sağlanabilmesi için bu NSG kuralları gereklidir:

1. _Storage.CentralUS_için HTTPS giden güvenlik kuralı oluşturun :

   - **Hedef servis etiketi**: _Storage.CentralUS_
   - **Hedef bağlantı noktası aralıkları**: _443_

1. _AzureActiveDirectory_için bir HTTPS giden güvenlik kuralı oluşturun.

   - **Hedef servis etiketi**: _AzureActiveDirectory_
   - **Hedef bağlantı noktası aralıkları**: _443_

1. Site Kurtarma IP'leri için kaynak konuma karşılık gelen HTTPS bağlantı noktası 443 giden kuralları oluşturun:

   | Konum | Site Kurtarma IP adresi | Site Kurtarma izleme IP adresi |
   | --- | --- | --- |
   | Doğu ABD | 13.82.88.226 | 104.45.147.24 |

### <a name="issue-3-site-recovery-configuration-failed-151197"></a>Sorun 3: Site Kurtarma yapılandırması başarısız oldu (151197)

#### <a name="possible-cause"></a>Olası nedeni

Azure Site Kurtarma hizmeti bitiş noktalarına bağlantı kurulamaz.

#### <a name="resolution"></a>Çözüm

Azure Site Recovery bölgeye bağlı olarak [Site Recovery IP aralıklarına](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags) erişim gerektiriyor. Gerekli IP aralıklarına VM'den erişilebildiğinden emin olun.

### <a name="issue-4-azure-to-azure-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>Sorun 4: Ağ trafiği şirket içi proxy sunucusundan geçtiğinde Azure'dan Azure'a çoğaltma başarısız oldu (151072)

#### <a name="possible-cause"></a>Olası nedeni

Özel proxy ayarları geçersizdir ve Azure Site Kurtarma Mobilite hizmeti aracısı Internet Explorer'dan (IE) proxy ayarlarını otomatik olarak algılamadı.

#### <a name="resolution"></a>Çözüm

1. Mobilite servis aracısı, Windows ve `/etc/environment` Linux'ta IE proxy ayarlarını algılar.
1. Proxy'yi yalnızca Azure Site Kurtarma Mobilitesi hizmeti için ayarlamayı tercih ederseniz, _proxyinfo.conf_ adresinde bulunan proxy ayrıntılarını sağlayabilirsiniz:

   - **Linux**:`/usr/local/InMage/config/`
   - **Windows**:`C:\ProgramData\Microsoft Azure Site Recovery\Config`

1. _ProxyInfo.conf_ aşağıdaki _INI_ formatında proxy ayarları olmalıdır:

   ```plaintext
   [proxy]
   Address=http://1.2.3.4
   Port=567
   ```

> [!NOTE]
> Azure Site Kurtarma Mobilitesi hizmet aracısı yalnızca **kimliği doğrulanmamış yakınlıkları**destekler.

### <a name="fix-the-problem"></a>Sorunu çözme

Gerekli [URL'lere](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) veya [gerekli IP aralıklarına](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags)izin vermek için, [ağ kılavuz belgesindeki](site-recovery-azure-to-azure-networking-guidance.md)adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure VM'leri başka bir Azure bölgesine çoğaltma](azure-to-azure-how-to-enable-replication.md)
