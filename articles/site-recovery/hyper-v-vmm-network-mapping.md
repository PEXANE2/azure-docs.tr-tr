---
title: Site Kurtarma ile Hyper-V (VMM ile) ağ eşleme hakkında
description: Hyper-V V VM'lerin (VMM bulutlarında yönetilen) Azure'a olağanüstü durum kurtarma için ağ eşlemesinin Azure'a nasıl ayarlandığını açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: raynew
ms.openlocfilehash: 6b68b4c943ec96620427978c2309f27e1fb1f217
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74082570"
---
# <a name="prepare-network-mapping-for-hyper-v-vm-disaster-recovery-to-azure"></a>Hyper-V VM olağanüstü durum kurtarma için Azure'a ağ eşlemesini hazırlama


Bu makale, System Center Virtual Machine Manager (VMM) bulutlarını Azure'da veya [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak ikincil bir siteye çoğalttığınızda ağ eşlemesini anlamanıza ve hazırlanmanıza yardımcı olur.


## <a name="prepare-network-mapping-for-replication-to-azure"></a>Azure'a çoğaltma için ağ eşlemesini hazırlama

Azure'a çoğalırken, kaynak VMM sunucusundaki VM ağları arasındaki ağ eşleme haritaları ve Azure sanal ağlarını hedefleyin. Eşleme sürecinde şu işlemler gerçekleştirilir:
-  **Ağ bağlantısı**—Çoğaltılan Azure VM'lerinin eşlenen ağa bağlanmasını sağlar. Aynı ağüzerinde başarısız olan tüm makineler, farklı kurtarma planlarında başarısız olsalar bile birbirlerine bağlanabilir.
- **Ağ ağ geçidi**—Hedef Azure ağında bir ağ ağ geçidi ayarlanmışsa, VM'ler diğer şirket içi sanal makinelere bağlanabilir.

Ağ eşleme aşağıdaki gibi çalışır:

- Bir kaynak VMM VM ağını azure sanal ağıyla eşlersiniz.
- Kaynak ağdaki Azure VM'ler başarısız olduktan sonra eşlenen hedef sanal ağa bağlanır.
- Kaynak VM ağına eklenen yeni VM'ler, çoğaltma gerçekleştiğinde eşlenen Azure ağına bağlanır.
- Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı adı taşıyorsa, çoğaltılan sanal makine yük devretme işleminin ardından hedef alt ağa bağlanır.
- Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.

## <a name="prepare-network-mapping-for-replication-to-a-secondary-site"></a>İkincil bir siteye çoğaltma için ağ eşlemesini hazırlama

İkincil bir siteye çoğalırken, kaynak VMM sunucusundaki VM ağları arasındaki ağ eşleme haritaları ve hedef VMM sunucusundaki VM ağları. Eşleme sürecinde şu işlemler gerçekleştirilir:

- **Ağ bağlantısı**—Başarısız olduktan sonra VM'leri uygun ağlara bağlar. Çoğaltma VM, kaynak ağa eşlenen hedef ağa bağlanır.
- **Optimal VM yerleşimi**—Çoğaltma VM'lerini Hyper-V ana bilgisayar sunucularında en iyi şekilde yerleştirir. Çoğaltma VM'leri, eşlenen VM ağlarına erişebilen ana bilgisayarlara yerleştirilir.
- **Ağ eşlemesi yok**—Ağ eşlemesi yapılandırmazsanız, çoğaltma VM'leri başarısız olduktan sonra hiçbir VM ağına bağlanmaz.

Ağ eşleme aşağıdaki gibi çalışır:

- Ağ eşleme, iki VMM sunucusunda VM ağları arasında veya iki site aynı sunucu tarafından yönetiliyorsa tek bir VMM sunucusunda yapılandırılabilir.
- Eşleme doğru yapılandırıldığında ve çoğaltma etkinleştirildiğinde, birincil konumdaki bir VM ağa bağlanır ve hedef konumdaki yinelemesi eşlenen ağına bağlanır.
- Site Kurtarma'da ağ eşleme sırasında hedef bir VM ağı seçtiğinizde, kaynak VM ağını kullanan VMM kaynak bulutları ve koruma için kullanılan hedef bulutlardaki kullanılabilir hedef VM ağları görüntülenir.
- Hedef ağda birden çok alt ağ varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağla aynı ada sahipse, çoğaltma VM başarısız olduktan sonra bu hedef alt ağa bağlanır. Eşleşen bir ada sahip hedef alt ağ yoksa, VM ağdaki ilk alt ağa bağlanır.

## <a name="example"></a>Örnek

Bu mekanizmayı göstermek için bir örnek aşağıda verilmiştir. New York ve Chicago'da iki yeri olan bir organizasyon alalım.

**Konum** | **VMM sunucusu** | **VM ağları** | **Eşlenen**
---|---|---|---
New York | VMM-New York| VMNetwork1-NewYork | VMNetwork1-Chicago eşlendi
 |  | VMNetwork2-NewYork | Eşlenmedi
Chicago | VMM-Chicago| VMNetwork1-Chicago | VMNetwork1-NewYork eşlendi
 | | VMNetwork2-Chicago | Eşlenmedi

Bu örnekte:

- VMNetwork1-NewYork'a bağlı herhangi bir VM için bir yineleme VM oluşturulduğunda, VMNetwork1-Chicago'ya bağlanır.
- VMNetwork2-NewYork veya VMNetwork2-Chicago için bir yineleme VM oluşturulduğunda, herhangi bir ağa bağlı olmayacaktır.

Örnek kuruluşumuzda VMM bulutlarının nasıl ayarlanır ve bulutlarla ilişkili mantıksal ağlar aşağıda açıklanmıştır.

### <a name="cloud-protection-settings"></a>Bulut koruması ayarları

**Korumalı bulut** | **Bulutu koruma** | **Mantıksal ağ (New York)**  
---|---|---
Altın Bulut1 | Altın Bulut2 |
SilverCloud1| SilverCloud2 |
Altın Bulut2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Mantıksal ve VM ağ ayarları

**Konum** | **Mantıksal ağ** | **İlişkili VM ağı**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | MantıksalAğ2Chicago | VMNetwork2-Chicago

### <a name="target-network-settings"></a>Hedef ağ ayarları

Bu ayarlara göre, hedef VM ağını seçtiğinizde, aşağıdaki tabloda kullanılabilecek seçenekleri gösterilmektedir.

**Seç** | **Korumalı bulut** | **Bulutu koruma** | **Hedef ağ kullanılabilir**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Kullanılabilir
 | Altın Bulut1 | Altın Bulut2 | Kullanılabilir
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Kullanılamaz
 | Altın Bulut1 | Altın Bulut2 | Kullanılabilir


Hedef ağda birden çok alt ağ varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağla aynı ada sahipse, çoğaltma sanal makinesi başarısız olduktan sonra bu hedef alt ağa bağlanır. Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.


### <a name="failback-behavior"></a>Failback davranışı

Geri dönüş (ters çoğaltma) durumunda neler olduğunu görmek için, VMNetwork1-NewYork'un vmnetwork1-Chicago'ya aşağıdaki ayarlarla eşlenmiş olduğunu varsayalım.


**Vm** | **VM ağına bağlı**
---|---
VM1 | VMNetwork1-Ağ
VM2 (VM1'in kopyası) | VMNetwork1-Chicago

Bu ayarlarla, birkaç olası senaryoda neler olduğunu gözden geçirelim.

**Senaryo** | **Sonuç**
---|---
Başarısız olduktan sonra VM-2'nin ağ özelliklerinde değişiklik yok. | VM-1 kaynak ağa bağlı kalır.
VM-2'nin ağ özellikleri arıza dan sonra değiştirilir ve bağlantısı kesilir. | VM-1'in bağlantısı kesildi.
VM-2'nin ağ özellikleri arıza dan sonra değiştirilir ve VMNetwork2-Chicago'ya bağlanır. | VMNetwork2-Chicago eşlenmezse, VM-1'in bağlantısı kesilir.
VMNetwork1-Chicago ağ eşleme değiştirildi. | VM-1 şimdi VMNetwork1-Chicago eşlenen ağa bağlı olacaktır.



## <a name="next-steps"></a>Sonraki adımlar

- [Hakkında bilgi edinin](hyper-v-vmm-networking.md) İkincil bir VMM sitesine başarısız olduktan sonra IP adresi.
- [Hakkında bilgi edinin](concepts-on-premises-to-azure-networking.md) Azure'a geçemeden IP adresi.
