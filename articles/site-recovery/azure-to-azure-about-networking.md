---
title: Azure Site Kurtarma ile Azure VM olağanüstü durum kurtarma ağ hakkında
description: Azure Site Kurtarma'yı kullanarak Azure VM'lerinin çoğaltılması için ağ ağına genel bir bakış sağlar.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 3/13/2020
ms.author: sutalasi
ms.openlocfilehash: 58348c9aed14a5cc9126be780fe01817274a0b47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283268"
---
# <a name="about-networking-in-azure-vm-disaster-recovery"></a>Azure VM olağanüstü durum kurtarma da ağ hakkında



Bu makale, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Azure VM'lerini bir bölgeden diğerine çoğaltırken ve kurtarırken ağ kılavuzu sağlar.

## <a name="before-you-start"></a>Başlamadan önce

Site [Kurtarma'nın bu senaryo](azure-to-azure-architecture.md)için olağanüstü durum kurtarmayı nasıl sağladığını öğrenin.

## <a name="typical-network-infrastructure"></a>Tipik ağ altyapısı

Aşağıdaki diyagram, Azure VM'lerde çalışan uygulamalar için tipik bir Azure ortamını göstermektedir:

![müşteri-çevre](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Azure ExpressRoute kullanıyorsanız veya şirket içi ağınızdan Azure'a VPN bağlantısı kullanıyorsanız, ortam aşağıdaki gibidir:

![müşteri-çevre](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Genellikle, ağlar güvenlik duvarları ve ağ güvenlik grupları (NSG'ler) kullanılarak korunur. Güvenlik duvarları, ağ bağlantısını denetlemek için URL veya IP tabanlı beyaz liste kullanır. NSG'ler ağ bağlantısını denetlemek için IP adres aralıklarını kullanan kurallar sağlar.

>[!IMPORTANT]
> Ağ bağlantısını denetlemek için kimlik doğrulaması yapılan bir proxy kullanmak Site Kurtarma tarafından desteklenmez ve çoğaltma etkinleştirilemiyor.


## <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için URL tabanlı bir güvenlik duvarı proxy'si kullanıyorsanız, şu Site Kurtarma URL'lerine izin verin:


**URL** | **Şey**
--- | ---
*.blob.core.windows.net | Verilerin VM'den kaynak bölgedeki önbellek depolama hesabına yazılabilmesi için gereklidir. VM'lerinizin tüm önbellek depolama hesaplarını biliyorsanız, *.blob.core.windows.net yerine belirli depolama hesabı URL'lerine (Ör: cache1.blob.core.windows.net ve cache2.blob.core.windows.net) erişime izin verebilirsiniz
login.microsoftonline.com | Site Kurtarma hizmet URL'lerine yetkilendirme ve kimlik doğrulama için gereklidir.
*.hypervrecoverymanager.windowsazure.com | VM'den Site Kurtarma hizmeti iletişiminin gerçekleşebilmeleri için gereklidir.
*.servicebus.windows.net | Site Kurtarma izleme ve tanılama verilerinin VM'den yazılabilmesi için gereklidir.
*.vault.azure.net | Portal üzerinden ADE özellikli sanal makineler için çoğaltmayı etkinleştirme olanağı sağlar
*.automation.ext.azure.com | Portal üzerinden çoğaltılan bir öğe için mobilite aracısının otomatik olarak yükseltilmesine olanak sağlar

## <a name="outbound-connectivity-using-service-tags"></a>Hizmet Etiketleri kullanarak giden bağlantı

Giden bağlantımı denetlemek için bir NSG kullanıyorsanız, bu hizmet etiketlerine izin verilmesi gerekir.

- Kaynak bölgedeki depolama hesapları için:
    - Kaynak bölge için [Depolama hizmet etiketi](../virtual-network/security-overview.md#service-tags) tabanlı NSG kuralı oluşturun.
    - Verilerin VM'den önbellek depolama hesabına yazılabilmesi için bu adreslere izin verin.
- AAD'ye karşılık gelen tüm IP adreslerine erişime izin vermek için [Azure Active Directory (AAD) hizmet etiketi](../virtual-network/security-overview.md#service-tags) tabanlı NSG kuralı oluşturun
- Hedef bölge için Site Kurtarma izlemesine erişim sağlayan EventsHub hizmet etiketi tabanlı NSG kuralı oluşturun.
- Herhangi bir bölgedeki Site Kurtarma hizmetine erişime izin vermek için AzureSiteRecovery hizmet etiketi tabanlı NSG kuralı oluşturun.
- AzureKeyVault hizmet etiketi tabanlı NSG kuralı oluşturun. Bu yalnızca portal üzerinden ADE özellikli sanal makinelerin çoğaltılmasını etkinleştirmek için gereklidir.
- GuestAndHybridManagement hizmet etiketi tabanlı NSG kuralı oluşturun. Bu yalnızca portal üzerinden çoğaltılan bir öğe için mobilite aracısının otomatik olarak yükseltilen etkinleştirilmesi için gereklidir.
- Bir test NSG'de gerekli NSG kurallarını oluşturmanızı ve üretim NSG'si ile ilgili kuralları oluşturmadan önce herhangi bir sorun olmadığını doğrulamanızı öneririz.

## <a name="example-nsg-configuration"></a>Örnek NSG yapılandırması

Bu örnek, bir VM'nin çoğaltması için NSG kurallarının nasıl yapılandırılabildiğini gösterir.

- Giden bağlantıyı kontrol etmek için NSG kurallarını kullanıyorsanız, gerekli tüm IP adres aralıkları için "HTTPS giden gidene izin ver" kurallarını bağlantı noktasına:443'ü kullanın.
- Örnek, VM kaynak konumunun "Doğu ABD" ve hedef konumun "Orta ABD" olduğunu varsayılr.

### <a name="nsg-rules---east-us"></a>NSG kuralları - Doğu ABD

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG'deki "Storage.EastUS" için giden HTTPS (443) güvenlik kuralı oluşturun.

      ![depolama etiketi](./media/azure-to-azure-about-networking/storage-tag.png)

2. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG'deki "AzureActiveDirectory" için giden HTTPS (443) güvenlik kuralı oluşturun.

      ![aad etiketi](./media/azure-to-azure-about-networking/aad-tag.png)

3. Yukarıdaki güvenlik kurallarına benzer şekilde, NSG'de hedef konuma karşılık gelen "EventHub.CentralUS" için giden HTTPS (443) güvenlik kuralını oluşturun. Bu, Site Kurtarma izleme erişimi sağlar.

4. NSG'de "AzureSiteRecovery" için giden HTTPS (443) güvenlik kuralı oluşturun. Bu, herhangi bir bölgedeki Site Kurtarma Hizmetine erişim sağlar.

### <a name="nsg-rules---central-us"></a>NSG kuralları - Orta ABD

Çoğaltmanın hedef bölgeden kaynak bölgeye geçemedisonrası etkinleştirilebilmek için bu kurallar gereklidir:

1. NSG'de "Storage.CentralUS" için giden HTTPS (443) güvenlik kuralı oluşturun.

2. NSG'de "AzureActiveDirectory" için giden HTTPS (443) güvenlik kuralı oluşturun.

3. Yukarıdaki güvenlik kurallarına benzer şekilde, NSG'de kaynak konuma karşılık gelen "EventHub.EastUS" için giden HTTPS (443) güvenlik kuralını oluşturun. Bu, Site Kurtarma izleme erişimi sağlar.

4. NSG'de "AzureSiteRecovery" için giden HTTPS (443) güvenlik kuralı oluşturun. Bu, herhangi bir bölgedeki Site Kurtarma Hizmetine erişim sağlar.

## <a name="network-virtual-appliance-configuration"></a>Ağ sanal cihaz yapılandırması

VM'lerden giden ağ trafiğini kontrol etmek için ağ sanal cihazları (NVAs) kullanıyorsanız, tüm çoğaltma trafiği NVA'dan geçerse cihaz daraltılmış olabilir. Çoğaltma trafiğinin NVA'ya gitmemesi için sanal ağımızda "Depolama" için bir ağ hizmeti bitiş noktası oluşturmanızı öneririz.

### <a name="create-network-service-endpoint-for-storage"></a>Depolama için ağ hizmeti bitiş noktası oluşturma
Çoğaltma trafiğinin Azure sınırını terk etmeyecek şekilde sanal ağınızda "Depolama" için bir ağ hizmeti bitiş noktası oluşturabilirsiniz.

- Azure sanal ağınızı seçin ve 'Hizmet bitiş noktaları'na tıklayın

    ![depolama bitiş noktası](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- 'Ekle' ve 'Hizmet uç noktaları ekle' sekmesi açılır
- 'Hizmet' altında 'Microsoft.Storage'ı ve 'Alt Ağlar' alanı altında gerekli alt ağları seçin ve 'Ekle'ye tıklayın

>[!NOTE]
>ASR için kullanılan depolama hesaplarınıza sanal ağ erişimini kısıtlamayın. 'Tüm ağlardan' erişime izin vermelisiniz

### <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

Azure'un varsayılan sistem rotasını özel bir [rotayla](../virtual-network/virtual-networks-udr-overview.md#custom-routes) 0.0.0.0/0 adres öneki için geçersiz kılabilir ve VM trafiğini şirket içi bir ağ sanal cihazına (NVA) yönlendirebilirsiniz, ancak bu yapılandırma Site Kurtarma çoğaltmaiçin önerilmez. Özel rotalar kullanıyorsanız, çoğaltma trafiğinin Azure sınırını terk etmesin diye sanal ağınızda "Depolama" için [bir sanal ağ hizmeti bitiş noktası oluşturmanız](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure sanal makinelerini çoğaltarak](site-recovery-azure-to-azure.md)iş yüklerinizi korumaya başlayın.
- Azure sanal makine başarısız olmak için [IP adresi tutma](site-recovery-retain-ip-azure-vm-failover.md) hakkında daha fazla bilgi edinin.
- [ExpressRoute ile Azure sanal makinelerinin](azure-vm-disaster-recovery-with-expressroute.md)olağanüstü durum kurtarma hakkında daha fazla bilgi edinin.
