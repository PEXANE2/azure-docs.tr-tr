---
title: Azure Uygulamaları Yönetilen Uygulama Teklifi Yayımlama Kılavuzu
description: Bu makalede, Yönetilen bir uygulamayı Market'te yayımlama gereksinimleri açıklanmaktadır
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: dsindona
ms.openlocfilehash: 38323ecfee69460b16542a3e004513f6a7389c41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288419"
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
|Azure uyumlu sanal sabit disk (VHD)    |   VM'ler Windows veya Linux üzerine inşa edilmelidir.<ul> <ul> <li>Linux VHD oluşturma hakkında daha fazla bilgi için [Azure'da onaylanan Linux dağıtımlarına](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)bakın.</li> <li>Windows VHD oluşturma hakkında daha fazla bilgi için [bkz.](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md)</li> </ul> |

>[!NOTE]
> Yönetilen uygulamalar Market aracılığıyla dağıtılabilir olmalıdır. Müşteri iletişimi bir sorunsa, müşteri adayı paylaşımını etkinleştirdikten sonra ilgili müşterilere ulaşmalısınız.  

>[!Note]
>Bulut Çözüm Sağlayıcıları (CSP) iş ortağı kanal tercihi artık kullanılabilir.  Microsoft CSP iş ortağı kanalları aracılığıyla teklifinizi pazarlama hakkında daha fazla bilgi için lütfen [Bulut Çözüm Sağlayıcıları'na](./cloud-solution-providers.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar
Eğer bunu yapmadıysanız, 

- Pazara [kaydolun.](https://azuremarketplace.microsoft.com/sell)

Kayıtlıysanız ve yeni bir teklif oluşturuyorsanız veya varolan bir teklif üzerinde çalışıyorsanız,

- Teklifinizi oluşturmak veya tamamlamak için [Bulut İş Ortağı Portalı'nda oturum açın.](https://cloudpartner.azure.com)
