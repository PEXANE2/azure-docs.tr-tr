---
title: Azure Güvenlik Merkezi 'nde ağ güvenlik gruplarını etkinleştirme | Microsoft Docs
description: Bu belgede, Azure Güvenlik Merkezi önerisinin **ağ güvenlik gruplarının**nasıl uygulanacağı gösterilmektedir.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 14b7cc8f8162574380ca21ac515af8b7d3d5ded9
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "60911471"
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde ağ güvenlik gruplarını etkinleştirme
Azure Güvenlik Merkezi, zaten etkinleştirilmemişse bir ağ güvenlik grubu (NSG) etkinleştirmenizi önerir. NSG 'ler bir sanal ağdaki VM örneklerinizin ağ trafiğine izin veren veya reddeden Access Control listesi (ACL) kurallarının bir listesini içerir. NSG'ler alt ağlarla veya bu alt ağların içindeki tekil VM örnekleriyle ilişkili olabilir. NSG bir alt ağ ile ilişkili olduğunda ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerli olur. Ayrıca, tek bir VM 'ye giden trafik, bir NSG 'yi doğrudan bu VM 'ye ilişkilendirerek daha da kısıtlanabilir. Daha fazla bilgi edinmek için bkz. [ağ güvenlik grubu (NSG) nedir?](../virtual-network/security-overview.md)

NSG 'ler etkinleştirilmemişse, Güvenlik Merkezi size iki öneri sunar: Alt ağlarda ağ güvenlik gruplarını etkinleştirin ve sanal makinelerde ağ güvenlik gruplarını etkinleştirin. NSG 'leri uygulamak için hangi düzey, alt ağ veya VM 'yi seçin.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulayın
1. **Öneriler** dikey penceresinde alt ağlarda veya sanal makinelerde **ağ güvenlik gruplarını etkinleştir** ' i seçin.
   ![Ağ Güvenlik Gruplarını Etkinleştirme][1]
2. Bu, seçtiğiniz öneriye bağlı olarak alt ağlar veya sanal makineler için **eksik ağ güvenlik gruplarını yapılandırma** dikey penceresini açar. Bir NSG 'yi yapılandırmak için bir alt ağ veya sanal makine seçin.

   ![Alt ağ için NSG 'yi yapılandırma][2]

   ![VM için NSG 'yi yapılandırma][3]
3. **Ağ güvenlik grubu seç** dikey penceresinde, mevcut bir NSG seçin veya **Yeni oluştur** ' u seçerek NSG 'yi oluşturun.

   ![Ağ güvenlik grubu seç][4]

Bir NSG oluşturursanız, bir NSG oluşturmak ve güvenlik kurallarını ayarlamak için [ağ güvenlik grubunu yönetme](../virtual-network/manage-network-security-group.md) bölümündeki adımları izleyin.

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, alt ağlar veya sanal makineler için Güvenlik Merkezi 'nin "ağ güvenlik gruplarını etkinleştirme" önerisi nasıl uygulanacağı açıklanır. NSG 'leri etkinleştirme hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Ağ Güvenlik Grubu (NSG) Nedir?](../virtual-network/security-overview.md)
* [Ağ güvenlik grubunu yönetme](../virtual-network/manage-network-security-group.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --en son Azure Güvenlik haberlerini ve bilgilerini alın.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
