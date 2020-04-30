---
title: Azure uygulamaları çözüm şablonu teklif yayımlama Kılavuzu | Azure Marketi
description: Bu makalede, Azure Marketi 'nde bir çözüm şablonu yayımlama gereksinimleri açıklanmaktadır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: dsindona
ms.openlocfilehash: 6533fa930716552c91fffd13b196bdbf78158816
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82084864"
---
# <a name="azure-applications-solution-template-offer-publishing-requirements"></a>Azure uygulamaları: çözüm şablonu teklif yayımlama gereksinimleri

Bu makalede, Azure Market 'te Azure uygulaması teklifi yayımlamanın bir yolu olan çözüm şablonu teklif türü için gereksinimler açıklanmaktadır. Çözüm altyapınızı otomatik olarak dağıtmak için çözüm şablonu teklif türü bir [Azure Resource Manager şablonu (ARM şablonu)](../azure-resource-manager/templates/overview.md) gerektirir.

Aşağıdaki koşullar gerektiğinde Azure Uygulama çözümü şablonu teklif türünü kullanın:

- Çözümünüz, VM 'lerin, ağın ve depolama kaynaklarının birleşimi gibi tek bir VM 'nin ötesinde ek dağıtım ve yapılandırma Otomasyonu gerektirir.
- Müşterinizin çözümü kendileri yönetecekdir.

Kullanıcının bu teklif türü için gördüğü eyleme yapılan çağrı "Şimdi al" dır.

## <a name="requirements-for-solution-template-offers"></a>Çözüm şablonu teklifleri için gereksinimler

| **Gereksinimler** | **Bilgileri**  |
| ---------------  | -----------  |
|Faturalandırma ve ölçüm    |  Çözüm şablonu teklifleri Transact teklifleri değildir, ancak Microsoft ticari Marketi aracılığıyla faturalandırılan ücretli VM tekliflerini dağıtmak için kullanılabilir. Çözümün ARM şablonunun dağıttığı kaynaklar, müşterinin Azure aboneliğinden temin edilir. Kullandıkça Öde (PAYGO) sanal makineleri, müşterinin Azure aboneliği aracılığıyla faturalandırılan Microsoft aracılığıyla müşteri ile işlem görür.<br/> Kendi lisansını getir (KLG) durumunda Microsoft, müşteri aboneliğinde oluşan altyapı maliyetlerini faturalandırtıracaktır, ancak yazılım lisans ücretlerinizi müşteriye doğrudan Transact.   |
|Azure ile uyumlu sanal sabit disk (VHD)  |   VM 'Ler Windows veya Linux üzerinde oluşturulmalıdır. Daha fazla bilgi için bkz. <ul> <li>[Azure Uygulama teklifi oluşturma](./partner-center-portal/create-new-azure-apps-offer.md)(Windows VHD 'ler için)</li><li>[Azure 'da desteklenen Linux dağıtımları](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) (Linux VHD 'ler için).</li></ul> |
| Müşteri kullanım Attributıon | Azure Marketi 'nde yayınlanan tüm çözüm şablonlarında müşteri kullanım attributıon özelliğinin etkinleştirilmesi gerekir. Müşteri kullanımı atımı ve nasıl etkinleştirileceği hakkında daha fazla bilgi için bkz. [Azure iş ortağı müşteri kullanımı atısyonu](./azure-partner-customer-usage-attribution.md).  |
| Yönetilen Diskleri kullanma | [Yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) , Azure 'Da IaaS VM 'lerinin kalıcı diskleri için varsayılan seçenektir. Çözüm şablonlarında yönetilen diskleri kullanmanız gerekir. <br> <br> 1. çözüm şablonlarınızı güncelleştirmek için Azure ARM şablonlarındaki yönetilen diskleri kullanmaya yönelik [yönergeleri](https://docs.microsoft.com/azure/virtual-machines/windows/using-managed-disks-template-deployments) ve [örnekleri](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md) izleyin. <br> <br> 2. VHD 'yi Market 'te görüntü olarak yayımlamak için yönetilen disklerin temel VHD 'sini bir depolama hesabına aktarmak üzere aşağıdaki yönergeleri izleyin: <br> <ul> <li> [PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd?toc=%2fpowershell%2fmodule%2ftoc.json) </li> <li> [CLI](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd?toc=%2fcli%2fmodule%2ftoc.json) </li> </ul> |

## <a name="next-steps"></a>Sonraki adımlar

- Daha önce yapmadıysanız Azure Marketi hakkında [bilgi edinin](https://azuremarketplace.microsoft.com/sell) .
- Teklifinizi oluşturmak veya tamamlayabilmeniz için [Iş Ortağı Merkezi ' nde oturum açın](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) .
- Daha fazla bilgi için [bir Azure Uygulama teklifi oluşturun](./partner-center-portal/create-new-azure-apps-offer.md) .
