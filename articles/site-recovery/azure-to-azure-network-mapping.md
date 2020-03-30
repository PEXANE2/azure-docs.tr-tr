---
title: Azure Site Kurtarma'daki iki bölge arasındaki sanal ağları haritala
description: Azure Site Kurtarma ile Azure VM olağanüstü durum kurtarma için iki Azure bölgesi arasındaki sanal ağları eşleme hakkında bilgi edinin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: mayg
ms.openlocfilehash: b8f0512f978f25ca196ad6e9a7a03243c47f0662
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258089"
---
# <a name="set-up-network-mapping-and-ip-addressing-for-vnets"></a>VNets için ağ eşleme ve IP adresleme ayarlama

Bu makalede, farklı Azure bölgelerinde bulunan iki Azure sanal ağlarının (VNet) örneğinin nasıl eşlenedeğiştirileceği ve ağlar arasında IP adresinin nasıl ayarılanileceği açıklanmaktadır. Ağ eşleme, çoğaltmayı etkinleştirme sırasında kaynak ağa dayalı hedef ağ seçimi için varsayılan bir davranış sağlar.

## <a name="prerequisites"></a>Ön koşullar

Ağları haritalandırmadan önce kaynakta [Azure VNet'leri](../virtual-network/virtual-networks-overview.md) olmalı ve Azure bölgelerini hedeflemelisiniz. 

## <a name="set-up-network-mapping-manually-optional"></a>Ağ eşlemelerini el ile ayarlama (İsteğe bağlı)

Harita ağları aşağıdaki gibidir:

1. **Site Kurtarma Altyapısında** **+Ağ Eşlemesi'ni**tıklatın.

    ![ Ağ eşleme oluşturma](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)

3. **Ağ eşleme ekle'de**kaynak ve hedef konumlarını seçin. Örneğimizde, VM kaynağı Doğu Asya bölgesinde çalışıyor ve Güneydoğu Asya bölgesine çoğalıyor.

    ![Kaynak ve hedef seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)
3. Şimdi ters yönde bir ağ eşleme oluşturun. Bizim örneğimizde, kaynak şimdi Güneydoğu Asya olacak ve hedef Doğu Asya olacaktır.

    ![Ağ eşleme bölmesi ekleme - Hedef ağ için kaynak ve hedef konumları seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-networks-when-you-enable-replication"></a>Çoğaltmayı etkinleştirdiğinizde ağları eşle

Azure VM'ler için olağanüstü durum kurtarma yapılandırmadan önce ağ eşlemesini hazırlamadıysanız, [ayarlarken ve çoğaltmayı etkinleştirdiğinizde](azure-to-azure-how-to-enable-replication.md)bir hedef ağ belirtebilirsiniz. Bunu yaptığınızda aşağıdaki olur:

- Seçtiğiniz hedefe bağlı olarak, Site Kurtarma otomatik olarak kaynaktan hedef bölgeye ve hedeften kaynak bölgeye ağ eşlemeleri oluşturur.
- Varsayılan olarak, Site Kurtarma hedef bölgesinde kaynak ağla aynı olan bir ağ oluşturur. Site Kurtarma kaynak ağın adına sonek olarak **-asr** ekler. Hedef ağı özelleştirebilirsiniz.
- Bir kaynak ağ için ağ eşleme zaten oluştuysa, eşlenen hedef ağ her zaman daha fazla VM için çoğaltmaları etkinleştirme sırasında varsayılan olacaktır. Açılır açılır yerden diğer kullanılabilir seçenekleri seçerek hedef sanal ağı değiştirmeyi seçebilirsiniz. 
- Yeni çoğaltmalar için varsayılan hedef sanal ağı değiştirmek için varolan ağ eşlemesini değiştirmeniz gerekir.
- A bölgesinden B bölgesine bir ağ eşlemesi değiştirmek istiyorsanız, önce ağ eşlemesini B bölgesinden A bölgesine sildiğinizden emin olun. Ters eşleme silme işleminden sonra, ağ eşlemesini A bölgesinden B bölgesine değiştirin ve ardından ilgili ters eşlemesini oluşturun.

>[!NOTE]
>* Ağ eşlemesini değiştirmek yalnızca yeni VM çoğaltmaları için varsayılanları değiştirir. Varolan çoğaltmalar için hedef sanal ağ seçimlerini etkilemez. 
>* Hedef ağı varolan bir çoğaltma için değiştirmek istiyorsanız, çoğaltılan öğenin İşlem ve Ağ Ayarları'na gidin.

## <a name="specify-a-subnet"></a>Bir alt ağ belirtin

Hedef VM'nin alt ağı, kaynak VM'nin alt ağının adına göre seçilir.

- Hedef ağda kaynak VM alt ağıyla aynı ada sahip bir alt ağ varsa, bu alt ağ hedef VM için ayarlanır.
- Hedef ağda aynı ada sahip bir alt ağ yoksa, alfabetik sıradailk alt ağ hedef alt ağ olarak ayarlanır.
- VM için Bilgi İşlem **ve Ağ** ayarlarındaki hedef alt ağı değiştirebilirsiniz.

    ![İşlem ve Ağ işlem özellikleri penceresi](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="set-up-ip-addressing-for-target-vms"></a>Hedef VM'ler için IP adresleme ayarlama

Hedef sanal makinedeki her NIC'nin IP adresi aşağıdaki gibi yapılandırılır:

- **DHCP**: VM kaynağının NIC'i DHCP kullanıyorsa, hedef VM'nin NIC'i de DHCP kullanmak üzere ayarlanır.
- **Statik IP adresi**: Kaynak VM'nin NIC'i statik IP adresi kullanıyorsa, hedef VM NIC de statik ip adresi kullanır.


## <a name="ip-address-assignment-during-failover"></a>Yük devretme sırasında IP adresi atama

**Kaynak ve hedef alt ağlar** | **Şey**
--- | ---
Aynı adres alanı | Kaynak VM NIC IP adresi hedef VM NIC IP adresi olarak ayarlanır.<br/><br/> Adres kullanılamıyorsa, kullanılabilir bir sonraki IP adresi hedef olarak ayarlanır.
Farklı adres alanı | Hedef alt ağdaki bir sonraki kullanılabilir IP adresi hedef VM NIC adresi olarak ayarlanır.



## <a name="ip-address-assignment-during-test-failover"></a>Test başarısızmı sırasında IP adresi ataması

**Hedef ağ** | **Şey**
--- | ---
Hedef ağ, VNet'in üzerinde bir arızadır | - Hedef IP adresi aynı IP adresi ile statik olacaktır. <br/><br/>  - Aynı IP adresi zaten atanmışsa, ip adresi alt net aralığının sonunda bulunan bir sonraki adrestir. Örneğin: Kaynak IP adresi 10.0.0.19 ise ve failover ağı 10.0.0.0/24 aralığını kullanıyorsa, hedef VM'ye atanan bir sonraki IP adresi 10.0.0.254'dür.
Hedef ağ başarısız VNet değil | - Hedef IP adresi aynı IP adresi ile statik olacaktır.<br/><br/>  - Aynı IP adresi zaten atanmışsa, ip adresi alt net aralığının sonunda bulunan bir sonraki adrestir.<br/><br/> Örneğin: Kaynak statik IP adresi 10.0.0.19 ise ve failover 10.0.0.0/24 aralığı ile failover ağı olmayan bir ağ üzerinde ise, hedef statik IP adresi 10.0.0.0.19 olacak ve aksi takdirde 10.0.0.254 olacaktır.

- Failover VNet, olağanüstü durum kurtarmayı ayarlarken seçtiğiniz hedef ağdır.
- Test başarısız olmak için her zaman üretim dışı bir ağ kullanmanızı öneririz.
- VM'nin Bilgi İşlem **ve Ağ** ayarlarındaki hedef IP adresini değiştirebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- Azure VM olağanüstü durum kurtarma için [ağ kılavuzunu](site-recovery-azure-to-azure-networking-guidance.md) gözden geçirin.
- Başarısız olduktan sonra IP adreslerini saklama hakkında [daha fazla bilgi edinin.](site-recovery-retain-ip-azure-vm-failover.md)
