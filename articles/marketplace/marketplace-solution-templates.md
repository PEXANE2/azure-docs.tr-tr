---
title: Azure Uygulamaları Çözüm Şablonu Teklif Yayın Kılavuzu | Azure Marketi
description: Bu makalede, Azure Marketi'nde bir çözüm şablonu yayımlama gereksinimleri açıklanmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: dsindona
ms.openlocfilehash: faff12adbf6c0f88f3161e1e4f2760da3b7966ca
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687522"
---
# <a name="azure-applications-solution-template-offer-publishing-guide"></a>Azure Uygulamaları: Çözüm Şablonu Teklif Yayımlama Kılavuzu

Çözüm şablonları, Çözüm Market'te yayımlanmanın başlıca yollarından biridir. Bu teklifin gereksinimlerini anlamak için bu kılavuzu kullanın. 

Çözümünüz tek bir VM'nin ötesinde ek dağıtım ve yapılandırma otomasyonu gerektirdiğinde Azure uygulamasını kullanın: çözüm şablonu teklif türü. Azure uygulamalarını kullanarak bir veya daha fazla VM sağlamayı otomatikleştirebilirsiniz: çözüm şablonları. Ağ ve depolama kaynaklarını da sağlayabilirsiniz. Azure uygulamaları: çözüm şablonları, tek bir VM ve tüm IaaS tabanlı çözümler için otomasyon avantajları sunar.

Bu çözüm şablonları işlem teklifleri değildir, ancak Microsoft'un ticari pazarı aracılığıyla faturalanan ücretli VM tekliflerini dağıtmak için kullanılabilir. Bir kullanıcının gördüğü eylem çağrısı "Şimdi Al" dır.


## <a name="requirements-for-solution-templates"></a>Çözüm Şablonları Için Gereksinimler

| **Gereksinimler** | **Şey**  |
| ---------------  | -----------  |
|Faturalama ve ölçüm    |  Kaynaklar müşterinin Azure aboneliğinde sağlanacaktır. Gittiğiniz kadar öde (PAYGO) sanal makineler, müşteri yle Birlikte Microsoft üzerinden işlenir ve müşterinin Azure aboneliği (PAYGO) üzerinden faturalandırılır.  <br/> Kendi lisansınızı getir (BYOL) durumunda, Microsoft müşteri aboneliğinde oluşan altyapı maliyetlerini faturalayacaktır, ancak yazılım lisans ücretlerinizi doğrudan müşteriye aktarabilirsiniz.   |
|Azure uyumlu sanal sabit disk (VHD)  |   VM'ler Windows veya Linux üzerine inşa edilmelidir.  Daha fazla bilgi için [bir Azure uygulama teklifi oluşturmaya](./partner-center-portal/create-new-azure-apps-offer.md)bakın. |
| Müşteri Kullanımı İlişkisi | Azure Marketi'nde yayınlanan tüm çözüm şablonlarında müşteri kullanımı ilişkilendirmesini etkinleştirmek gerekir. Müşteri kullanımı atıfve nasıl etkinleştirilen hakkında daha fazla bilgi için [Azure iş ortağı müşteri kullanımı atıf](./azure-partner-customer-usage-attribution.md)bakın.  |
| Yönetilen Diskleri kullanma | [Yönetilen Diskler,](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) Azure'daki IaaS VM'lerin kalıcı diskleri için varsayılan seçenektir. Çözüm Şablonlarında Yönetilen Diskler'i kullanmanız gerekir. <br> <br> 1. Çözüm şablonlarınızı güncelleştirmek için Azure ARM şablonlarında Yönetilen Diskleri kullanma [kılavuzunu](https://docs.microsoft.com/azure/virtual-machines/windows/using-managed-disks-template-deployments) ve [örneklerini](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md) izleyin. <br> <br> 2. Yönetilen Disklerin temel VHD'sini Market'te görüntü olarak yayınlamak için Bir Depolama hesabına almak için aşağıdaki yönergeleri izleyin: <br> <ul> <li> [PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd?toc=%2fpowershell%2fmodule%2ftoc.json) </li> <li> [CLI](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd?toc=%2fcli%2fmodule%2ftoc.json) </li> </ul> |

## <a name="next-steps"></a>Sonraki adımlar

Eğer bunu yapmadıysanız, 

- Pazar yeri hakkında [bilgi edinin.](https://azuremarketplace.microsoft.com/sell)

Ortak Merkezi'ne kaydolmak için yeni bir teklif oluşturmaya veya varolan bir teklif üzerinde çalışmaya başlayın:

- Teklifinizi oluşturmak veya tamamlamak için [Partner Center'da oturum açın.](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership)
- Daha fazla bilgi için [bir Azure uygulama teklifi oluşturma](./partner-center-portal/create-new-azure-apps-offer.md) bilgisine bakın.

