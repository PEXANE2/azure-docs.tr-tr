---
title: Azure uygulamaları yönetilen uygulama teklifi Yayımlama Kılavuzu | Azure Marketi
description: Bu makalede, Azure Marketi 'nde yönetilen bir uygulama yayımlama gereksinimleri açıklanmaktadır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: dsindona
ms.openlocfilehash: 946e7524eada600d5ef17b2663a3fea066dcfaa2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82084881"
---
# <a name="azure-applications-managed-application-offer-publishing-requirements"></a>Azure uygulamaları: yönetilen uygulama teklifi yayımlama gereksinimleri

Bu makalede, Azure Market 'te Azure uygulaması teklifi yayımlamanın bir yolu olan yönetilen uygulama teklif türü için gereksinimler açıklanmaktadır. Yönetilen uygulamalar, Azure Marketi aracılığıyla dağıtılan ve faturalandırılan Transact teklifleridir. Kullanıcının gördüğü eyleme yapılan çağrı "Şimdi al" dır.

Aşağıdaki koşullar gerekli olduğunda, yönetilen uygulama teklifi türünü kullanın:

- Bir VM veya tüm IaaS tabanlı bir çözümü kullanarak müşteriniz için abonelik tabanlı bir çözüm dağıtırsınız.
- Siz veya müşteriniz, çözümün bir iş ortağı tarafından yönetilmesini gerektirir.

>[!NOTE]
>Örneğin, bir iş ortağı bir sı veya yönetilen hizmet sağlayıcısı (MSP) olabilir.  

## <a name="managed-application-offer"></a>Yönetilen uygulama teklifi

|Gereksinimler |Ayrıntılar  |
|---------|---------|
|Müşterinin Azure aboneliğine dağıtılır | Yönetilen uygulamalar müşterinin aboneliğine dağıtılmalıdır ve üçüncü bir taraf tarafından yönetilebilir. |
|Faturalandırma ve ölçüm    |  Kaynaklar, müşterinin Azure aboneliğinden temin edilir. Kullandıkça Öde (PAYGO) sanal makineleri, müşterinin Azure aboneliği (PAYGO) aracılığıyla faturalandırılır. <br> Kendi lisansını getir durumunda, Microsoft, müşteri aboneliğinde oluşan altyapı maliyetlerini faturalandırtıracağız, ancak yazılım lisans ücretlerinizi müşteriye doğrudan kullanacaksınız.        |
|Azure ile uyumlu sanal sabit disk (VHD)    |   VM 'Ler Windows veya Linux üzerinde oluşturulmalıdır.<ul> <ul> <li>Linux VHD oluşturma hakkında daha fazla bilgi için bkz. [Azure 'da desteklenen Linux dağıtımları](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Windows VHD oluşturma hakkında daha fazla bilgi için bkz. [Azure Uygulama teklifi oluşturma](./partner-center-portal/create-new-azure-apps-offer.md).</li> </ul> |

>[!NOTE]
> Yönetilen uygulamalar Market üzerinden dağıtılabilir olmalıdır. Müşteri iletişimi sorun oluşturacaksa, müşteri adayı paylaşımını etkinleştirdikten sonra ilgilenen müşterilere ulaşmalısınız.  

>[!Note]
>Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı kabul etme artık kullanılabilir. Teklifinizi Microsoft CSP iş ortağı kanalları aracılığıyla pazarlama hakkında daha fazla bilgi için bkz. [Cloud Solution Providers](./cloud-solution-providers.md).

## <a name="next-steps"></a>Sonraki adımlar

- Daha önce yapmadıysanız Azure Marketi hakkında [bilgi edinin](https://azuremarketplace.microsoft.com/sell) .
- Teklifinizi oluşturmak veya tamamlayabilmeniz için [Iş Ortağı Merkezi ' nde oturum açın](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) .
- Daha fazla bilgi için [bir Azure Uygulama teklifi oluşturun](./partner-center-portal/create-new-azure-apps-offer.md) .
