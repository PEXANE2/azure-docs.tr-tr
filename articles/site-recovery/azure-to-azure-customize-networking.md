---
title: Yük devretme VM 'si için ağ yapılandırmasını özelleştirme | Microsoft Docs
description: Azure Site Recovery kullanarak Azure VM 'lerinin çoğaltmasında yük devretme sanal makinesi için ağ yapılandırmalarının özelleştirilmesine genel bir bakış sağlar.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/07/2019
ms.author: rajanaki
ms.openlocfilehash: 1905d6afb023b1bed15f1359fed8477d815acb45
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71087699"
---
# <a name="customize-networking-configurations-of-the-target-azure-vm"></a>Hedef Azure VM’nin ağ yapılandırmalarını özelleştirme

Bu makalede, Azure sanal makinelerini bir bölgeden diğerine çoğaltırken ve kurtarırken, [Azure Site Recovery](site-recovery-overview.md)kullanarak hedef Azure VM 'de ağ yapılandırmalarının özelleştirilmesi hakkında rehberlik sağlanır.

## <a name="before-you-start"></a>Başlamadan önce

Site Recovery [Bu senaryo](azure-to-azure-architecture.md)için olağanüstü durum kurtarma nasıl sağladığını öğrenin.

## <a name="supported-networking-resources"></a>Desteklenen ağ kaynakları

Azure VM 'Leri çoğaltılırken yük devretme VM 'si için aşağıdaki temel kaynak konfigürasyonları belirtilebilir.

- [İç yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#what-is-standard-load-balancer)
- [Genel IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses)
- Hem alt ağ hem de NIC için [ağ güvenlik grubu](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)

 > [!IMPORTANT]
  > Bu ayarlar, yük devretme testi için değil, yalnızca şu anda yük devretme işleminde desteklenir.

## <a name="pre-requisites"></a>Ön koşullar

- Kurtarma tarafı yapılandırmalarının önceden planlantığınızdan emin olun.
- Ağ kaynaklarını önceden oluşturmanız gerekir. Azure Site Recovery hizmetin bu ayarları kabul edebilmesi ve yük devretme VM 'sinin bu ayarlara uyduğundan emin olması için bunu bir giriş olarak sağlayın.

## <a name="steps-to-customize-failover-networking-configurations"></a>Yük devretme ağ yapılandırmalarının özelleştirilmesi için adımlar

1. **Çoğaltılan öğelere**gidin. 
2. İstenen Azure VM 'sine tıklayın.
3. **İşlem ve ağ ' a**ve **Düzenle**' ye tıklayın. NIC yapılandırma ayarlarının kaynaktaki ilgili kaynakları içerdiğine dikkat edin. 

     ![Özelleştir](media/azure-to-azure-customize-networking/edit-networking-properties.png)

4. Yapılandırmak istediğiniz NIC 'in yakınında **Düzenle** ' ye tıklayın. Açılan sonraki dikey pencerede, hedefte karşılık gelen önceden oluşturulmuş kaynakları seçin.

    ![NIC-ayrıntıya gitme](media/azure-to-azure-customize-networking/nic-drilldown.png) 

5. **Tamam**'ı tıklatın.

Site Recovery artık bu ayarları kabul eder ve yük devretmede VM 'nin ilgili NIC aracılığıyla seçili kaynağa bağlı olduğundan emin olur.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unable-to-view-or-select-a-resource"></a>Kaynak görüntülenemiyor veya kullanılamıyor

Bir ağ kaynağını seçip görüntüleyeerişemiyorsanız, lütfen aşağıdaki denetim koşullarını gözden geçirin &:

- Ağ kaynağının hedef alanı yalnızca kaynak VM 'ye karşılık gelen bir giriş varsa etkindir. Bu bir olağanüstü durum kurtarma senaryosunda, kaynağınızın tam veya ölçeği genişletilmiş bir sürümünü isteyeceksiniz.
- Söz konusu ağ kaynaklarının her biri için, yük devretme sanal makinesinin kendisini seçilen kaynağa iliştirediğinden ve yük devretme güvenilirliği korunduğundan emin olmak için açılan listede bazı filtreler uygulanır. Bu filtreler, kaynak VM 'yi yapılandırdığınız sırada doğrulanacak olan ağ koşullarını temel alır.

İç yük dengeleyici doğrulamaları:

1. LB ve hedef VM 'nin aboneliği ve bölgesi aynı olmalıdır.
2. Iç Load Balancer ilişkili sanal ağ ve hedef Vmin aynı olması gerekir.
3. Hedef VM 'nin genel IP SKU 'SU ve Iç yük dengeleyici SKU 'SU aynı olmalıdır.
4. Hedef VM bir kullanılabilirlik alanına yerleştirilecek şekilde yapılandırıldıysa, yük dengeleyicinin bölge için erişilebilir olup olmadığını veya herhangi bir kullanılabilirlik bölgesinin bir parçasını kontrol edin. (Temel SKU yük dengeleyiciler bölgeleri desteklemez ve bu durumda açılan kutuda gösterilmeyecektir.)
5. Iç LoadBalancer 'ın önceden oluşturulmuş bir arka uç havuzuna ve ön uç yapılandırmasına sahip olduğundan emin olun.


Genel IP adresi:
    
1. Ortak IP ve hedef VM 'nin aboneliği ve bölgesi aynı olmalıdır.
2. Hedef VM 'nin genel IP SKU 'SU ve Iç yük dengeleyici SKU 'SU aynı olmalıdır.

Ağ güvenlik grubu:
1. Ağ güvenlik grubunun ve hedef VM 'nin aboneliği ve bölgesi aynı olmalıdır.


> [!WARNING]
> Hedef VM bir kullanılabilirlik kümesiyle ilişkiliyse, kullanılabilirlik kümesindeki diğer VM 'nin genel IP/Iç yük dengeleyicisiyle aynı SKU 'nun genel IP/Iç yük dengeleyiciyi ilişkilendirmeniz gerekir. Bunun başarısız olması, yük devretme hatasına neden olabilir.
