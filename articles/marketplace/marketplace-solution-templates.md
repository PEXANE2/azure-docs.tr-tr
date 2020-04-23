---
title: Azure uygulamaları çözüm şablonu teklif yayımlama kılavuzu | Azure Marketi
description: Bu makalede, Azure Marketi'nde bir çözüm şablonu yayımlama gereksinimleri açıklanmaktadır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: dsindona
ms.openlocfilehash: 6533fa930716552c91fffd13b196bdbf78158816
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084864"
---
# <a name="azure-applications-solution-template-offer-publishing-requirements"></a>Azure Uygulamaları: Çözüm şablonu yayımlama gereksinimleri sunar

Bu makalede, Azure Marketi'nde bir Azure uygulama teklifi yayınlamanın bir yolu olan çözüm şablonu teklif türü için gereksinimleri açıklanmaktadır. Çözüm şablonu teklif türü, çözüm altyapınızı otomatik olarak dağıtmak için bir [Azure Kaynak Yöneticisi şablonu (ARM şablonu)](../azure-resource-manager/templates/overview.md) gerektirir.

Aşağıdaki koşullar gerektiğinde Azure uygulama çözümü teklifi türünü kullanın:

- Çözümünüz, VM, ağ ve depolama kaynaklarının birleşimi gibi tek bir VM'nin ötesinde ek dağıtım ve yapılandırma otomasyonu gerektirir.
- Müşteriniz çözümü kendileri yönetecek.

Bir kullanıcının bu teklif türü için gördüğü eylem çağrısı "Şimdi Al" dır.

## <a name="requirements-for-solution-template-offers"></a>Çözüm şablonu teklifleri için gereksinimler

| **Gereksinimler** | **Şey**  |
| ---------------  | -----------  |
|Faturalama ve ölçüm    |  Çözüm şablonu teklifleri işyapma teklifleri değildir, ancak Microsoft ticari pazar aracılığıyla faturalanan ücretli VM tekliflerini dağıtmak için kullanılabilir. Çözümün ARM şablonuna dağıtılan kaynaklar müşterinin Azure aboneliğinde sağlanacaktır. Gittiğiniz kadar öde (PAYGO) sanal makineler, müşteri yle Birlikte Microsoft üzerinden işlenir ve müşterinin Azure aboneliği üzerinden faturalandırılır.<br/> Kendi lisansınızı getir (BYOL) durumunda, Microsoft müşteri aboneliğinde oluşan altyapı maliyetlerini faturalayacaktır, ancak yazılım lisans ücretlerinizi doğrudan müşteriye aktarabilirsiniz.   |
|Azure uyumlu sanal sabit disk (VHD)  |   VM'ler Windows veya Linux üzerine inşa edilmelidir. Daha fazla bilgi için bkz. <ul> <li>[Azure uygulama teklifi oluşturma](./partner-center-portal/create-new-azure-apps-offer.md)(Windows VHD'ler için)</li><li>[Linux dağıtımları Azure'da onaylanmıştır](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) (Linux VHD'leri için).</li></ul> |
| Müşteri Kullanımı İlişkisi | Azure Marketi'nde yayınlanan tüm çözüm şablonlarında müşteri kullanımı ilişkilendirmesini etkinleştirmek gerekir. Müşteri kullanımı atıfve nasıl etkinleştirilen hakkında daha fazla bilgi için [Azure iş ortağı müşteri kullanımı atıf](./azure-partner-customer-usage-attribution.md)bakın.  |
| Yönetilen Diskleri kullanma | [Yönetilen Diskler,](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) Azure'daki IaaS VM'lerin kalıcı diskleri için varsayılan seçenektir. Çözüm şablonlarında Yönetilen Diskler'i kullanmanız gerekir. <br> <br> 1. Çözüm şablonlarınızı güncelleştirmek için Azure ARM şablonlarında Yönetilen Diskleri kullanma [kılavuzunu](https://docs.microsoft.com/azure/virtual-machines/windows/using-managed-disks-template-deployments) ve [örneklerini](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md) izleyin. <br> <br> 2. Yönetilen Disklerin temel VHD'sini pazar yerinde görüntü olarak yayınlamak için bir depolama hesabına almak için aşağıdaki yönergeleri izleyin: <br> <ul> <li> [PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd?toc=%2fpowershell%2fmodule%2ftoc.json) </li> <li> [CLI](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd?toc=%2fcli%2fmodule%2ftoc.json) </li> </ul> |

## <a name="next-steps"></a>Sonraki adımlar

- Henüz yapmadıysanız Azure Marketi hakkında [bilgi edinin.](https://azuremarketplace.microsoft.com/sell)
- Teklifinizi oluşturmak veya tamamlamak için [Partner Center'da oturum açın.](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership)
- Daha fazla bilgi için [bir Azure uygulama teklifi oluşturun.](./partner-center-portal/create-new-azure-apps-offer.md)
