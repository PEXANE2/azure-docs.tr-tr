---
title: Bir hata için ağ yapılandırmalarını özelleştirin VM | Microsoft Dokümanlar
description: Azure Site Kurtarma'yı kullanarak Azure VM'lerinin çoğaltılmasında başarısız bir VM için ağ yapılandırmalarını özelleştirmeye genel bir bakış sağlar.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/21/2019
ms.author: rajanaki
ms.openlocfilehash: 96ffa34166797945afc04c66b03fe151d26c65bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76292867"
---
# <a name="customize-networking-configurations-of-the-target-azure-vm"></a>Hedef Azure VM’nin ağ yapılandırmalarını özelleştirme

Bu makalede, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Azure VM'lerini bir bölgeden diğerine çoğaltırken ve kurtarırken hedef Azure sanal makinesinde (VM) ağ yapılandırmalarını özelleştirme konusunda kılavuz lar verilmektedir.

## <a name="before-you-start"></a>Başlamadan önce

Site [Kurtarma'nın bu senaryo](azure-to-azure-architecture.md)için olağanüstü durum kurtarmayı nasıl sağladığını öğrenin.

## <a name="supported-networking-resources"></a>Desteklenen ağ kaynakları

Azure VM'leri çoğaltırken başarısız VM için aşağıdaki önemli kaynak yapılandırmalarını sağlayabilirsiniz:

- [Dahili yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)
- [Genel IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses)
- [Hem](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group) alt ağ hem de NIC için ağ güvenlik grubu

## <a name="prerequisites"></a>Ön koşullar

- Kurtarma tarafı yapılandırmalarınızı önceden planladığınızdan emin olun.
- Ağ kaynaklarını önceden oluşturun. Azure Site Kurtarma hizmetinin bu ayarları yerine getirebilmesi ve başarısız VM'nin bu ayarlara uymasını sağlaması için bunu bir girdi olarak sağlayın.

## <a name="customize-failover-and-test-failover-networking-configurations"></a>Ağ yapılandırmalarında failover'ı özelleştirme ve başarısızlık yapılandırmalarını test etme

1. **Çoğaltılan Öğeler'e**gidin. 
2. İstediğiniz Azure VM'sini seçin.
3. **İşlem ve Ağ'ı** seçin ve **Edit'i**seçin. NIC yapılandırma ayarlarının kaynaktaki ilgili kaynakları içerdiğine dikkat edin. 

     ![Failover ağ yapılandırmalarını özelleştirme](media/azure-to-azure-customize-networking/edit-networking-properties.png)

4. Sanal ağ üzerinden bir test başarısız seçin. Boş bırakmayı seçebilir ve test başarısız olduğu sırada birini seçebilirsiniz.
5. Failover ağı, yapılandırmak istediğiniz NIC'nin yakınında **Seç'tir.** Açılan bir sonraki bıçakta, test başarısızlığı ve başarısız konumundaki ilgili önceden oluşturulmuş kaynakları seçin.

    ![NIC yapılandırmasını düzenleme](media/azure-to-azure-customize-networking/nic-drilldown.png) 

6. **Tamam'ı**seçin.

Site Kurtarma artık bu ayarları onurlandıracak ve failover'daki VM'nin ilgili NIC aracılığıyla seçilen kaynağa bağlanmasını sağlayacaktır.

Kurtarma Planı ile test başarısızolmasını tetiklediğinizde, her zaman Azure sanal ağına sorar. Bu sanal ağ, önceden yapılandırılan test başarısız ayarları olmayan makineler için test başarısızlığı için kullanılacaktır.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unable-to-view-or-select-a-resource"></a>Kaynağı görüntüleyemiyor veya seçemiyor

Bir ağ kaynağını seçemez veya görüntüleyemezseniz, aşağıdaki denetimleri ve koşulları gözden geçirin:

- Bir ağ kaynağının hedef alanı, yalnızca kaynak VM'nin karşılık gelen bir girdisi varsa etkinleştirilir. Bu, bir olağanüstü durum kurtarma senaryosu için kaynağınızın tam veya ölçeklenmiş sürümünü isteyeceğiniz ilkesine dayanır.
- Her ağ kaynağı için, başarısız VM'nin kendisini seçilen kaynağa iliştirebilmesini ve başarısızlık güvenilirliğinin korunabilmesini sağlamak için açılır listede bazı filtreler uygulanır. Bu filtreler, kaynak VM'yi yapılandırdığınızda doğrulanmış olan aynı ağ koşullarını temel almaktadır.

Dahili yük dengeleyici doğrulamaları:

- Yük bakiyesi ile hedef VM'nin Aboneliği ve Bölgesi aynı olmalıdır.
- İç yük dengeleyicisi ve hedef VM ile ilişkili sanal ağ aynı olmalıdır.
- Hedef VM'nin halka açık IP SKU'su ve dahili yük dengeleyicisinin SKU'su aynı olmalıdır.
- Hedef VM bir kullanılabilirlik bölgesine yerleştirilecek şekilde yapılandırılırsa, yük dengeleyicisinin bölge yedekli mi yoksa herhangi bir kullanılabilirlik bölgesinin bir parçası mı olduğunu kontrol edin. (Temel SKU yük dengeleyicileri bölgeleri desteklemez ve bu durumda açılan listede gösterilmez.)
- Dahili yük dengeleyicisinin önceden oluşturulmuş bir arka uç havuzuna ve ön uç yapılandırmasına sahip olduğundan emin olun.

Genel IP adresi:

- Genel IP ve hedef VM'nin Aboneliği ve Bölgesi aynı olmalıdır.
- Hedef VM'nin halka açık IP SKU'su ve dahili yük dengeleyicisinin SKU'su aynı olmalıdır.

Ağ güvenlik grubu:
- Ağ güvenlik grubunun ve hedef VM'nin Abonelik ve Bölgesi aynı olmalıdır.


> [!WARNING]
> Hedef VM bir kullanılabilirlik kümesiyle ilişkiliyse, aynı SKU'nun genel IP'sini ve dahili yük dengeleyicisini, kullanılabilirlik kümesindeki diğer VM'nin genel IP'si ve dahili yük dengeleyicisiyle ilişkilendirmeniz gerekir. Eğer yapmazsan, başarısız lık başarılı olmayabilir.
