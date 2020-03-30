---
title: Azure Sitesi Kurtarma ile Azure VM başarısız olduktan sonra IP adreslerini tutma
description: Azure Site Kurtarma ile ikincil bir bölgeye olağanüstü durum kurtarma için Azure VM'leri üzerinde başarısız olduğunda IP adreslerinin nasıl tutulup tutulabildiğini açıklar
ms.service: site-recovery
ms.date: 4/9/2019
author: mayurigupta13
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 650fb7f0877a98ef53ed3868550f9c084ecb5885
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257569"
---
# <a name="retain-ip-addresses-during-failover"></a>Başarısız dönemde IP adreslerini koruyun

[Azure Site Kurtarma,](site-recovery-overview.md) VM'leri başka bir Azure bölgesine kopyalayarak, bir kesinti olduğunda başarısız olarak ve işler normale döndüğünde birincil bölgeye geri dönülerek Azure VM'leri için olağanüstü durum kurtarma olanağı sağlar.

Başarısız olurken, IP adresini hedef bölgede kaynak bölgeyle aynı tutmak isteyebilirsiniz:

- Varsayılan olarak, Azure VM'leri için olağanüstü durum kurtarmayı etkinleştirdiğinizde, Site Kurtarma kaynak kaynak ayarlarını temel alan hedef kaynakları oluşturur. Statik IP adresleriyle yapılandırılan Azure VM'leri için Site Kurtarma, kullanımda değilse hedef VM için aynı IP adresini sağlamaya çalışır. Site Kurtarma'nın adresleme işlemlerini nasıl işlediğine ilişkin tam bir açıklama için [bu makaleyi inceleyin.](azure-to-azure-network-mapping.md#set-up-ip-addressing-for-target-vms)
- Basit uygulamalar için varsayılan yapılandırma yeterlidir. Daha karmaşık uygulamalar için, bağlantının başarısız olduktan sonra beklendiği gibi çalıştığından emin olmak için ek kaynak sağlamanız gerekebilir.


Bu makalede, daha karmaşık örnek senaryolarda IP adreslerini saklamak için bazı örnekler sağlar. Örnekler şunlardır:

- Azure'da çalışan tüm kaynaklara sahip bir şirket için failover
- Karma dağıtımı olan bir şirket ve hem şirket içinde hem de Azure'da çalışan kaynaklar için başarısız

## <a name="resources-in-azure-full-failover"></a>Azure'daki kaynaklar: tam başarısız

Şirket A tüm uygulamaları Azure'da çalışır.

### <a name="before-failover"></a>Başarısız olmadan önce

İşte başarısız olmadan önceki mimari.

- Şirket A, kaynak ve hedef Azure bölgelerinde aynı ağlara ve alt ağlara sahiptir.
- Kurtarma süresi hedefini (RTO) azaltmak için, şirket SQL Server Always On, etki alanı denetleyicileri vb. için çoğaltma düğümleri kullanır. Bu çoğaltma düğümleri hedef bölgede farklı bir VNet'tedir, böylece kaynak ve hedef bölgeler arasında VPN siteden siteye bağlantı kurabilirler. Kaynak ve hedefte aynı IP adresi alanı kullanılırsa bu mümkün değildir.  
- Başarısız olmadan önce, ağ mimarisi aşağıdaki gibidir:
    - Birincil bölge Azure Doğu Asya
        - Doğu Asya adres alanı 10.1.0.0/16 ile bir VNet **(Kaynak VNet)** vardır.
        - Doğu Asya'nın VNet'teki üç alt ağa bölünmüş iş yükleri vardır:
            - **Alt Ağ 1**: 10.1.1.0/24
            - **Alt Ağ 2**: 10.1.2.0/24
            - **Alt Ağ 3**: 10.1.3.0/24
    - İkincil (hedef) bölge Azure Güneydoğu Asya'dır
        - Güneydoğu Asya bir kurtarma VNet **(Kurtarma VNet**) **Kaynak VNet**aynı vardır.
        - Güneydoğu Asya'da adres alanı 10.2.0.0/16 olan ek bir VNet **(Azure VNet)** vardır.
        - **Azure VNet,** adres alanı 10.2.4.0/24 olan bir alt ağ **(Subnet 4)** içerir.
        - SQL Server Always On, etki alanı denetleyicisi vb. için çoğaltma düğümleri **Subnet 4'te**yer almaktadır.
    - **Kaynak VNet** ve **Azure VNet,** VPN siteden siteye bağlantıyla bağlantılıdır.
    - **Kurtarma VNet** başka bir sanal ağ ile bağlı değildir.
    - **Şirket A,** çoğaltılan öğeler için hedef IP adreslerini atar/doğrular. Hedef IP, her VM için kaynak IP ile aynıdır.

![Tam başarısız olmadan önce Azure'daki kaynaklar](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-before-failover2.png)

### <a name="after-failover"></a>Başarısız olduktan sonra

Bir kaynak bölgesel kesinti oluşursa, A Şirketi hedef bölgeye tüm kaynakları üzerinde başarısız olabilir.

- Başarısız olmadan önce hedef IP adresleri zaten mevcut ken, A Şirketi, **Recovery VNet** ve Azure **VNet**arasındaki başarısızlıktan sonra başarısız lığını düzenleyebilir ve otomatik olarak bağlantılar kurabilir. Bu aşağıdaki diyagramda gösterilmiştir...
- Uygulama gereksinimlerine bağlı olarak, hedef bölgedeki iki VNet **(Recovery VNet** ve **Azure VNet)** arasındaki bağlantılar, başarısız olmadan önce, ara adım sırasında veya sonrasında kurulabilir.
  - Şirket, bağlantıların ne zaman kurulacağını belirtmek için [kurtarma planlarını](site-recovery-create-recovery-plans.md) kullanabilir.
  - VNet peering veya siteden siteye VPN kullanarak VNets arasında bağlanabilir.
      - VNet peering VPN ağ geçidi kullanmaz ve farklı kısıtlamaları vardır.
      - VNet peering [fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-network) sı VNet-to-VNet VPN Ağ Geçidi [fiyatlandırmafarklı](https://azure.microsoft.com/pricing/details/vpn-gateway)hesaplanır. Başarısız olmak için, genellikle öngörülemeyen ağ olaylarını en aza indirmek için bağlantı türü de dahil olmak üzere kaynak ağlarla aynı bağlantı yöntemini kullanmanızı tavsiye ederiz.

    ![Azure'daki kaynaklar tam başarısız](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-full-region-failover2.png)



## <a name="resources-in-azure-isolated-app-failover"></a>Azure'daki kaynaklar: yalıtılmış uygulama başarısız

Uygulama düzeyinde başarısız olmanız gerekebilir. Örneğin, özel bir alt ağda bulunan belirli bir uygulama veya uygulama katmanı üzerinde başarısız olmak.

- Bu senaryoda, IP adresleme sini koruyabilirsiniz, ancak bağlantı tutarsızlıkları olasılığını artırdığı için genellikle tavsiye edilmez. Ayrıca, aynı Azure VNet'teki diğer alt ağlara alt ağ bağlantısını da kaybedersiniz.
- Alt ağ düzeyinde uygulama başarısızlığını yapmanın daha iyi bir yolu, başarısız olmak için farklı hedef IP adreslerini kullanmaktır (kaynak VNet'teki diğer alt ağlara bağlantıya ihtiyacınız varsa) veya her uygulamayı kaynak bölgede kendi özel VNet'inde yalıtmaktır. İkinci yaklaşımla kaynak bölgedeki ağlar arasında bağlantı kurabilir ve hedef bölgeye başarısız olduğunuzda aynı davranışı taklit edebilirsiniz.  

Bu örnekte, Şirket A uygulamaları kaynak bölgeye özel VNet'lere yerleştirir ve bu VNet'ler arasında bağlantı kurar. Bu tasarımla, yalıtılmış uygulama başarısızlığını gerçekleştirebilir ve hedef ağdaki kaynak özel IP adreslerini saklayabilirler.

### <a name="before-failover"></a>Başarısız olmadan önce

Başarısız olmadan önce, mimari aşağıdaki gibidir:

- Uygulama VM'leri birincil Azure Doğu Asya bölgesinde barındırılır:
    - **Uygulama1** VM'ler VNet **Kaynak VNet 1**: 10.1.0.0/16'da bulunur.
    - **Uygulama2** VM'ler VNet **Kaynak VNet 2:** 10.2.0.0/16'da bulunur.
    - **Kaynak VNet 1'in** iki alt ağı vardır.
    - **Kaynak VNet 2** iki alt ağlar vardır.
- İkincil (hedef) bölge Azure Güneydoğu Asya ' dır - Güneydoğu Asya' da **Kaynak VNet 1** ve **Kaynak VNet 2**ile aynı olan kurtarma VNet'leri **(Recovery VNet 1** ve Recovery **VNet 2)** vardır.
        - **Recovery VNet 1** ve **Recovery VNet 2'nin** her birinde Kaynak **VNet 1** ve Source **VNet 2'deki** alt ağlarla eşleşen iki alt ağ vardır - Güneydoğu Asya'da adres alanı 10.3.0.0/16 olan ek bir VNet **(Azure VNet)** vardır.
        - **Azure VNet,** adres alanı 10.3.4.0/24 olan bir alt ağ **(Subnet 4)** içerir.
        - SQL Server Always On, domain controller vb. için çoğaltma düğümleri **Subnet 4'te**yer almaktadır.
- Siteden siteye VPN bağlantıları vardır: 
    - **Kaynak VNet 1** ve **Azure VNet**
    - **Kaynak VNet 2** ve **Azure VNet**
    - **Kaynak VNet 1** ve **Kaynak VNet 2** VPN siteden siteye bağlıdır
- **Recovery VNet 1** ve **Recovery VNet 2** başka hiçbir VNet'e bağlı değildir.
- **Şirket A,** RTO'yü azaltmak için **Recovery VNet 1** ve **Recovery VNet 2'de**VPN ağ geçitlerini yapılandırır.  
- **Recovery VNet1** ve **Recovery VNet2** başka bir sanal ağa bağlı değildir.
- Kurtarma süresi hedefini (RTO) azaltmak için VPN ağ geçitleri, başarısız olmadan önce **Recovery VNet1** ve **Recovery VNet2** üzerinde yapılandırılır.

    ![Uygulama başarısız olmadan önce Azure'daki kaynaklar](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-before-failover2.png)

### <a name="after-failover"></a>Başarısız olduktan sonra

Tek bir uygulamayı etkileyen bir kesinti veya sorun durumunda (örneğimizde **Kaynak VNet 2'de), A Şirketi etkilenen uygulamayı aşağıdaki gibi kurtarabilir:


- **Kaynak VNet1** ve Kaynak **VNet2**arasındaki VE **Kaynak VNet2** ile Azure **VNet** arasındaki VPN bağlantılarını kes.
- **Kaynak VNet1** ve Recovery **VNet2**arasında ve **Recovery VNet2** ile **Azure VNet**arasında VPN bağlantıları kurun.
- **Kurtarma VNet2** **Kaynak VNet2** VM üzerinde başarısız .

![Azure uygulamasındaki kaynaklar başarısız](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-after-failover2.png)


- Bu örnek, daha fazla uygulama ve ağ bağlantısı içerecek şekilde genişletilebilir. Öneri, kaynaktan hedefe başarısız olduğunda mümkün olduğunca benzer bir bağlantı modeli izlemektir.
- VPN Ağ Geçitleri bağlantıları kurmak için ortak IP adreslerini ve ağ geçidi atlamalarını kullanır. Genel IP adreslerini kullanmak istemiyorsanız veya ek atlamalardan kaçınmak istiyorsanız, [desteklenen Azure bölgelerindesanal](../virtual-network/virtual-network-manage-peering.md#cross-region)ağlara [bakarak Azure VNet'i](../virtual-network/virtual-network-peering-overview.md) kullanabilirsiniz.

## <a name="hybrid-resources-full-failover"></a>Karma kaynaklar: tam başarısız

Bu senaryoda, **B Şirketi,** uygulama altyapısının bir kısmı Azure'da, geri kalanı şirket içinde çalışan karma bir işletme çalıştırır. 

### <a name="before-failover"></a>Başarısız olmadan önce

Ağ mimarisi nin başarısız olmadan önce nasıl göründüğü aşağıda veda edilmiştir.

- Uygulama VM'leri Azure Doğu Asya'da barındırılır.
- Doğu Asya adres alanı 10.1.0.0/16 ile bir VNet **(Kaynak VNet)** vardır.
  - Doğu Asya **Kaynak VNet**üç alt ağlar arasında bölünmüş iş yükleri vardır:
    - **Alt Ağ 1**: 10.1.1.0/24
    - **Alt Ağ 2**: 10.1.2.0/24
    - **Alt Ağ 3**: 10.1.3.0/24, adres alanı 10.1.0.0/16 olan bir Azure sanal ağı kullanır. Bu sanal ağın adı **Kaynak VNet**
      - İkincil (hedef) bölge Azure Güneydoğu Asya'dır:
  - Güneydoğu Asya bir kurtarma VNet **(Kurtarma VNet**) **Kaynak VNet**aynı vardır.
- Doğu Asya'daki VM'ler, Azure ExpressRoute veya siteden siteye VPN içeren şirket içi bir veri merkezine bağlıdır.
- RTO'u azaltmak için, B Şirketi, azure güneydoğu Asya'daki Recovery VNet'te başarısız olmadan önce ağ geçitlerini sağlar.
- B şirketi, çoğaltılan VM'ler için hedef IP adreslerini atar/doğrular. Hedef IP adresi, her VM için kaynak IP adresiyle aynıdır.


![Başarısız olmadan önce şirket içinde Azure bağlantısı](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-before-failover2.png)

### <a name="after-failover"></a>Başarısız olduktan sonra


Kaynak bölgesel kesintisi oluşursa, B Şirketi hedef bölgeye giden tüm kaynakları üzerinde başarısız olabilir.

- Başarısız olmadan önce hedef IP adresleri zaten mevcut olduğundan, B Şirketi başarısızlığı organize edebilir ve **Recovery VNet** ile Azure **VNet**arasındaki başarısızlıktan sonra otomatik olarak bağlantılar kurabilir.
- Uygulama gereksinimlerine bağlı olarak, hedef bölgedeki iki VNet **(Recovery VNet** ve **Azure VNet)** arasındaki bağlantılar, başarısız olmadan önce, ara adım sırasında veya sonrasında kurulabilir. Şirket, bağlantıların ne zaman kurulacağını belirtmek için [kurtarma planlarını](site-recovery-create-recovery-plans.md) kullanabilir.
- Azure Doğu Asya ile şirket içi veri merkezi arasındaki orijinal bağlantı, Azure Güneydoğu Asya ile şirket içi veri merkezi arasındaki bağlantıyı kurmadan önce kesilmelidir.
- Şirket içi yönlendirme, hedef bölgeye işaret edecek şekilde yeniden yapılandırılır ve ağ geçitleri başarısız lık sonrası dır.

![Başarısız olduktan sonra şirket içinde Azure bağlantısı](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-after-failover2.png)

## <a name="hybrid-resources-isolated-app-failover"></a>Hibrit kaynaklar: yalıtılmış uygulama başarısız

B şirketi, alt net düzeyinde yalıtılmış uygulamalar üzerinde başarısız olamaz. Bunun nedeni, kaynak ve kurtarma VNet'lerinde adres alanının aynı olması ve şirket içi bağlantının orijinal kaynağının etkin olmasıdır.

 - Uygulama esnekliği için B Şirketinin her uygulamayı kendi özel Azure VNet'ine yerleştirmesi gerekir.
 - Ayrı bir VNet'teki her uygulamayla, B Şirketi yalıtılmış uygulamalar üzerinde başarısız olabilir ve kaynak bağlantılarını hedef bölgeye yönlendirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Kurtarma [planları](site-recovery-create-recovery-plans.md)hakkında bilgi edinin.
