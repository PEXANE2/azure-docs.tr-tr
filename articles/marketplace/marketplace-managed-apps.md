---
title: Azure uygulamaları yönetilen uygulama teklifi yayımlama kılavuzu | Azure Marketi
description: Bu makalede, Azure Marketi'nde yönetilen bir uygulama yayımlama gereksinimleri açıklanmaktadır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: dsindona
ms.openlocfilehash: 946e7524eada600d5ef17b2663a3fea066dcfaa2
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084881"
---
# <a name="azure-applications-managed-application-offer-publishing-requirements"></a>Azure Uygulamaları: Yönetilen uygulama yayımlama gereksinimleri sunar

Bu makalede, Azure Marketi'nde bir Azure uygulama teklifi yayınlamanın bir yolu olan yönetilen uygulama teklifi türü için gereksinimleri açıklanmaktadır. Yönetilen uygulamalar, Azure Marketi üzerinden dağıtılan ve faturalanan işlemiş tekliflerdir. Bir kullanıcının gördüğü eylem çağrısı "Şimdi Al" dır.

Aşağıdaki koşullar gerektiğinde yönetilen uygulama teklifi türünü kullanın:

- VM veya IaaS tabanlı bir çözümün tamamını kullanarak müşteriniz için abonelik tabanlı bir çözüm dağıtabilirsiniz.
- Siz veya müşteriniz çözümün bir iş ortağı tarafından yönetilmesini gerektirir.

>[!NOTE]
>Örneğin, bir iş ortağı Bir SI veya yönetilen hizmet sağlayıcısı (MSP) olabilir.  

## <a name="managed-application-offer"></a>Yönetilen Uygulama Teklifi

|Gereksinimler |Ayrıntılar  |
|---------|---------|
|Müşterinin Azure aboneliğine dağıtıldı | Yönetilen Uygulamalar müşterinin aboneliğinde dağıtılmalı ve üçüncü bir taraf tarafından yönetilebilir. |
|Faturalama ve ölçüm    |  Kaynaklar müşterinin Azure aboneliğinde sağlanacaktır. Gittiğiniz kadar öde (PAYGO) sanal makineler, müşteri yle Birlikte Microsoft üzerinden işlenir ve müşterinin Azure aboneliği (PAYGO) üzerinden faturalandırılır. <br> Kendi lisansınızı getirme durumunda, Microsoft müşteri aboneliğinde tahakkuk eden altyapı maliyetlerini faturalayacaktır, ancak yazılım lisans ücretlerinizi doğrudan müşteriye aktarabilirsiniz.        |
|Azure uyumlu sanal sabit disk (VHD)    |   VM'ler Windows veya Linux üzerine inşa edilmelidir.<ul> <ul> <li>Linux VHD oluşturma hakkında daha fazla bilgi için [Azure'da onaylanan Linux dağıtımlarına](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)bakın.</li> <li>Windows VHD oluşturma hakkında daha fazla bilgi için [bir Azure uygulama teklifi oluşturma](./partner-center-portal/create-new-azure-apps-offer.md)bilgisine bakın.</li> </ul> |

>[!NOTE]
> Yönetilen uygulamalar Market aracılığıyla dağıtılabilir olmalıdır. Müşteri iletişimi bir sorunsa, müşteri adayı paylaşımını etkinleştirdikten sonra ilgili müşterilere ulaşmalısınız.  

>[!Note]
>Bulut Çözüm Sağlayıcıları (CSP) iş ortağı kanal tercihi artık kullanılabilir. Microsoft CSP iş ortağı kanalları aracılığıyla teklifinizi pazarlama hakkında daha fazla bilgi için [Bulut Çözüm Sağlayıcıları'na](./cloud-solution-providers.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Henüz yapmadıysanız Azure Marketi hakkında [bilgi edinin.](https://azuremarketplace.microsoft.com/sell)
- Teklifinizi oluşturmak veya tamamlamak için [Partner Center'da oturum açın.](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership)
- Daha fazla bilgi için [bir Azure uygulama teklifi oluşturun.](./partner-center-portal/create-new-azure-apps-offer.md)
