---
title: Azure uygulamaları için Yayımlama Kılavuzu çözüm şablonu teklifleri-Azure Marketi
description: Bu makalede, Azure Marketi 'nde çözüm şablonlarının yayımlanması için gerekenler açıklanmaktadır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: dsindona
ms.openlocfilehash: f62b3478c5c711423913b5918886b43b79ac691d
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858318"
---
# <a name="publishing-guide-for-azure-applications-solution-template-offers"></a>Azure uygulamaları çözüm şablonu teklifleri için Yayımlama Kılavuzu

Bu makalede, Azure Market 'te Azure Uygulama tekliflerini yayımlamanın bir yolu olan çözüm şablonu tekliflerini yayımlama gereksinimleri açıklanmaktadır. Çözüm altyapınızı otomatik olarak dağıtmak için çözüm şablonu teklif türü bir [Azure Resource Manager şablonu (ARM şablonu)](../azure-resource-manager/templates/overview.md) gerektirir.

Aşağıdaki koşullarda Azure uygulama *çözümü şablonu* teklif türünü kullanın:

- Çözümünüz, VM 'lerin, ağın ve depolama kaynaklarının birleşimi gibi tek bir sanal makinenin (VM) ötesinde ek dağıtım ve yapılandırma Otomasyonu gerektirir.
- Müşterilerinizin çözümü kendileri yöneteceklerdir.

Müşterinin bu teklif türü için gördüğü eyleme yapılan çağrı *Şimdi alınır*.

## <a name="requirements-for-solution-template-offers"></a>Çözüm şablonu teklifleri için gereksinimler

| **Gereksinimler** | **Bilgileri**  |
| ---------------  | -----------  |
|Faturalandırma ve ölçüm    |  Çözüm şablonu teklifleri işlem teklifleri değildir, ancak Microsoft ticari Marketi aracılığıyla faturalandırılan ücretli VM tekliflerini dağıtmak için kullanılabilirler. Çözümün ARM şablonunun dağıttığı kaynaklar müşterinin Azure aboneliğinde ayarlanır. Kullandıkça Öde sanal makineleri, müşteri ile Microsoft aracılığıyla gerçekleştirilir ve müşterinin Azure aboneliği aracılığıyla faturalandırılır.<br/> Kendi lisansını getir (KLG) faturanızı, Microsoft 'un müşteri aboneliğinde tahakkuk eden altyapı maliyetleri olmasına karşın, yazılım lisans ücretlerinizi müşteriyle doğrudan Transact.   |
|Azure ile uyumlu sanal sabit disk (VHD)  |   VM 'Ler Windows veya Linux üzerinde oluşturulmalıdır. Daha fazla bilgi için bkz. <ul> <li>[Bir Azure Uygulama teklifi oluşturun](./partner-center-portal/create-new-azure-apps-offer.md) (Windows VHD 'ler için).</li><li>[Azure 'da desteklenen Linux dağıtımları](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) (Linux VHD 'ler için).</li></ul> |
| Müşteri kullanımı ilişkilendirmesi | Azure Market 'te yayımlanan tüm çözüm şablonlarında müşteri kullanım attributıon özelliğinin etkinleştirilmesi gerekir. Müşteri kullanımı atımı ve nasıl etkinleştirileceği hakkında daha fazla bilgi için bkz. [Azure iş ortağı müşteri kullanımı atısyonu](./azure-partner-customer-usage-attribution.md).  |
| Yönetilen diskleri kullanma | [Yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) , Azure 'da hizmet olarak altyapı (IaaS) VM 'lerinin kalıcı diskleri için varsayılan seçenektir. Çözüm şablonlarında yönetilen diskleri kullanmanız gerekir. <ul><li>Çözüm şablonlarınızı güncelleştirmek için [Azure Resource Manager şablonlarda yönetilen diskleri kullanma](https://docs.microsoft.com/azure/virtual-machines/windows/using-managed-disks-template-deployments)bölümündeki yönergeleri izleyin ve sağlanan [örnekleri](https://github.com/Azure/azure-quickstart-templates)kullanın.<br><br> </li><li>VHD 'YI Azure Marketi 'nde bir görüntü olarak yayımlamak için, aşağıdaki yöntemlerden birini kullanarak yönetilen disklerin temel VHD 'sini bir depolama hesabına aktarın:<ul><li>[Azure PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd?toc=%2fpowershell%2fmodule%2ftoc.json) </li> <li> [Azure CLI](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd?toc=%2fcli%2fmodule%2ftoc.json) </li> </ul></ul> |

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, [Azure Marketi ile bulut işletmenizi nasıl büyütireceğinizi](https://azuremarketplace.microsoft.com/sell)öğrenin.

Iş Ortağı Merkezi 'nde çalışmaya kaydolmak ve başlamak için:

- Teklifinizi oluşturmak veya tamamlayabilmeniz için [Iş Ortağı Merkezi ' nde oturum açın](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) .
- Daha fazla bilgi için bkz. [Azure Uygulama teklifi oluşturma](./partner-center-portal/create-new-azure-apps-offer.md) .
