---
title: Azure Site Recovery ile Azure VM olağanüstü durum kurtarma 'da ağ iletişimi hakkında
description: Azure Site Recovery kullanarak Azure VM 'lerinin çoğaltılmasına yönelik ağa genel bir bakış sağlar.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 1/23/2020
ms.author: sutalasi
ms.openlocfilehash: aeab1960b065538635fdd63c43d779287f8cd9ee
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78363134"
---
# <a name="about-networking-in-azure-vm-disaster-recovery"></a>Azure VM olağanüstü durum kurtarma 'da ağ iletişimi hakkında



Bu makalede, Azure VM 'lerini bir bölgeden diğerine çoğaltırken ve kurtarırken, [Azure Site Recovery](site-recovery-overview.md)kullanarak Ağ Kılavuzu sağlanmaktadır.

## <a name="before-you-start"></a>Başlamadan önce

Site Recovery [Bu senaryo](azure-to-azure-architecture.md)için olağanüstü durum kurtarma nasıl sağladığını öğrenin.

## <a name="typical-network-infrastructure"></a>Tipik ağ altyapısı

Aşağıdaki diyagramda, Azure VM 'lerde çalışan uygulamalar için tipik bir Azure ortamı gösterilmektedir:

![müşteri-ortam](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Azure ExpressRoute veya şirket içi ağınızdan Azure 'a bir VPN bağlantısı kullanıyorsanız, ortam aşağıdaki gibidir:

![müşteri-ortam](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Genellikle, ağlar güvenlik duvarları ve ağ güvenlik grupları (NSG 'ler) kullanılarak korunur. Güvenlik duvarları, ağ bağlantısını denetlemek için URL veya IP tabanlı beyaz liste kullanır. NSG 'ler, ağ bağlantısını denetlemek için IP adresi aralıklarını kullanan kurallar sağlar.

>[!IMPORTANT]
> Ağ bağlantısını denetlemek için kimliği doğrulanmış bir proxy kullanmak Site Recovery tarafından desteklenmez ve çoğaltma etkinleştirilemez.


## <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için URL tabanlı bir güvenlik duvarı proxy 'si kullanıyorsanız, bu Site Recovery URL 'Lerine izin verin:


**URL** | **Ayrıntılar**  
--- | ---
*.blob.core.windows.net | Verilerin, VM 'den kaynak bölgedeki önbellek depolama hesabına yazılabilmeleri için gereklidir. Sanal makinelerinize yönelik tüm önbellek depolama hesaplarını biliyorsanız, *. blob.core.windows.net yerine belirli depolama hesabı URL 'Lerine (örn: cache1.blob.core.windows.net ve cache2.blob.core.windows.net) erişime izin verebilirsiniz
login.microsoftonline.com | Site Recovery hizmeti URL 'Lerinde yetkilendirme ve kimlik doğrulaması için gereklidir.
*.hypervrecoverymanager.windowsazure.com | Site Recovery hizmeti iletişiminin sanal makineden gerçekleşebilmesi için gereklidir.
*.servicebus.windows.net | Site Recovery izleme ve tanılama verilerinin VM 'den yazılabilmesini sağlamak için gereklidir.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adresi aralıkları için giden bağlantı

Giden bağlantıyı denetlemek için bir NSG kullanıyorsanız, bu hizmet etiketlerine izin verilmesi gerekir.

- Kaynak bölgedeki depolama hesaplarına karşılık gelen tüm IP adresi aralıkları
    - Kaynak bölge için bir [depolama hizmeti etiketi](../virtual-network/security-overview.md#service-tags) tabanlı NSG kuralı oluşturun.
    - Bu adreslere, verilerin VM 'den önbellek depolama hesabına yazılabilmeleri için izin verin.
- AAD 'ye karşılık gelen tüm IP adreslerine erişime izin vermek için [Azure Active Directory (AAD) hizmet etiketi](../virtual-network/security-overview.md#service-tags) tabanlı NSG kuralı oluşturma
- Hedef bölge için Site Recovery izlemeye erişime izin veren bir EventsHub hizmet etiketi tabanlı NSG kuralı oluşturun.
- Herhangi bir bölgedeki Site Recovery hizmetine erişime izin vermek için bir Azuresterecovery hizmet etiketi tabanlı NSG kuralı oluşturun.
- Gerekli NSG kurallarını bir test NSG üzerinde oluşturmanızı ve bir üretim NSG 'de kuralları oluşturmadan önce hiçbir sorun olmadığını doğrulamanızı öneririz.

## <a name="example-nsg-configuration"></a>Örnek NSG yapılandırması

Bu örnek, bir VM 'nin yinelenmesi için NSG kurallarının nasıl yapılandırılacağını gösterir.

- Giden bağlantıyı denetlemek için NSG kuralları kullanıyorsanız, tüm gerekli IP adresi aralıkları için bağlantı noktası: 443 ' e Izin ver (HTTPS giden) kuralları ' nı kullanın.
- Örnek, VM kaynak konumunun "Doğu ABD" ve hedef konumun "Orta ABD" olduğunu varsayar.

### <a name="nsg-rules---east-us"></a>NSG kuralları-Doğu ABD

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG 'de "Storage. EastUS" için giden bir HTTPS (443) güvenlik kuralı oluşturun.

      ![depolama etiketi](./media/azure-to-azure-about-networking/storage-tag.png)

2. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG 'de "AzureActiveDirectory" için giden HTTPS (443) güvenlik kuralı oluşturun.

      ![aad etiketi](./media/azure-to-azure-about-networking/aad-tag.png)

3. Yukarıdaki güvenlik kurallarına benzer şekilde, hedef konuma karşılık gelen NSG 'de "EventHub. Merkezileştirus" için giden HTTPS (443) güvenlik kuralı oluşturun. Bu, Site Recovery izlemeye erişim sağlar.

4. NSG 'de "Azuresterecovery" için giden bir HTTPS (443) güvenlik kuralı oluşturun. Bu, herhangi bir bölgedeki Site Recovery hizmetine erişim sağlar.

### <a name="nsg-rules---central-us"></a>NSG kuralları-Orta ABD

Bu kurallar, çoğaltmanın hedef bölgeden kaynak bölgeye yük devretme sonrası etkinleştirilebilmesi için gereklidir:

1. NSG 'de "Storage. Merkezileştirus" için giden bir HTTPS (443) güvenlik kuralı oluşturun.

2. NSG 'de "AzureActiveDirectory" için giden HTTPS (443) güvenlik kuralı oluşturun.

3. Yukarıdaki güvenlik kurallarına benzer şekilde, kaynak konuma karşılık gelen NSG 'de "EventHub. EastUS" için giden HTTPS (443) güvenlik kuralı oluşturun. Bu, Site Recovery izlemeye erişim sağlar.

4. NSG 'de "Azuresterecovery" için giden bir HTTPS (443) güvenlik kuralı oluşturun. Bu, herhangi bir bölgedeki Site Recovery hizmetine erişim sağlar.

## <a name="network-virtual-appliance-configuration"></a>Ağ sanal gereç yapılandırması

VM 'lerden giden ağ trafiğini denetlemek için ağ sanal gereçlerini (NVA 'lar) kullanıyorsanız, tüm çoğaltma trafiği NVA üzerinden geçerse gereç azalmasıyla karşılaşabilirsiniz. Çoğaltma trafiğinin NVA 'ya gitmemesi için, sanal ağınızda "depolama" için bir ağ hizmeti uç noktası oluşturmanız önerilir.

### <a name="create-network-service-endpoint-for-storage"></a>Depolama için ağ hizmeti uç noktası oluşturma
Çoğaltma trafiğinin Azure sınırından çıkmadan, sanal ağınızda "depolama" için bir ağ hizmeti uç noktası oluşturabilirsiniz.

- Azure Sanal ağınızı seçin ve ' hizmet uç noktaları ' seçeneğine tıklayın

    ![depolama uç noktası](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- ' Ekle ' seçeneğine tıklayın ve ' hizmet uç noktaları Ekle ' sekmesi açılır
- ' Hizmet ' altında ' Microsoft. Storage ' ve ' alt ağlar ' alanı altında gerekli alt ağlar ' ı seçin ve ' Ekle ' seçeneğine tıklayın

>[!NOTE]
>ASR için kullanılan depolama hesaplarınıza sanal ağ erişimini kısıtlamayın. ' Tüm ağlar 'dan erişime izin vermeniz gerekir

### <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

[Özel bir rota](../virtual-network/virtual-networks-udr-overview.md#custom-routes) ile 0.0.0.0/0 adres ön eki için Azure 'un varsayılan sistem yolunu geçersiz KıLABILIR ve VM trafiğini şirket içi ağ sanal gerecine (NVA) yönlendirebilirsiniz, ancak bu yapılandırma Site Recovery çoğaltma için önerilmez. Özel yollar kullanıyorsanız, çoğaltma trafiğinin Azure sınırından ayrılmaması için sanal ağınızda "depolama" için [bir sanal ağ hizmet uç noktası oluşturmanız](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure sanal makinelerini çoğaltarak](site-recovery-azure-to-azure.md)iş yüklerinizi korumaya başlayın.
- Azure sanal makine yük devretmesi için [IP adresi saklama](site-recovery-retain-ip-azure-vm-failover.md) hakkında daha fazla bilgi edinin.
- [ExpressRoute Ile Azure sanal makinelerinin](azure-vm-disaster-recovery-with-expressroute.md)olağanüstü durum kurtarması hakkında daha fazla bilgi edinin.
