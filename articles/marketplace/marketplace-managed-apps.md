---
title: Azure Uygulamaları Yönetilen Uygulama Teklifi Yayımlama Kılavuzu
description: Bu makalede, Yönetilen bir uygulamayı Market'te yayımlama gereksinimleri açıklanmaktadır
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: dsindona
ms.openlocfilehash: 56281ff8faba6b4b950334e2b0018d48c8e7aeb3
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687574"
---
# <a name="azure-applications-managed-application-offer-publishing-guide"></a>Azure Uygulamaları: Yönetilen Uygulama Teklifi Yayımlama Kılavuzu

Yönetilen bir uygulama, Çözüm Market'te yayımlanmanın ana yollarından biridir. Bu teklifin gereksinimlerini anlamak için bu kılavuzu kullanın. 

Bunlar, Market aracılığıyla dağıtılan ve faturalanan işlem teklifleridir. Bir kullanıcının gördüğü eylem çağrısı "Şimdi Al" dır.

Aşağıdaki koşullar gerektiğinde Azure uygulamasını kullanın: yönetilen uygulama teklifi türü:
- VM veya IaaS tabanlı bir çözüm kullanarak müşteriniz için abonelik tabanlı bir çözüm dağıtabilirsiniz.
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
>Bulut Çözüm Sağlayıcıları (CSP) iş ortağı kanal tercihi artık kullanılabilir.  Microsoft CSP iş ortağı kanalları aracılığıyla teklifinizi pazarlama hakkında daha fazla bilgi için lütfen [Bulut Çözüm Sağlayıcıları'na](./cloud-solution-providers.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Eğer bunu yapmadıysanız, 

- Pazar yeri hakkında [bilgi edinin.](https://azuremarketplace.microsoft.com/sell)

Ortak Merkezi'ne kaydolmak için yeni bir teklif oluşturmaya veya varolan bir teklif üzerinde çalışmaya başlayın:

- Teklifinizi oluşturmak veya tamamlamak için [Partner Center'da oturum açın.](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership)
- Daha fazla bilgi için [bir Azure uygulama teklifi oluşturma](./partner-center-portal/create-new-azure-apps-offer.md) bilgisine bakın.
