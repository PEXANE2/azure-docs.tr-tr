---
title: Azure Marketi için Sanal Makine Teklif Yayımlama Kılavuzu
description: Bu makalede, bir sanal makine ve Market'ten dağıtılacak yazılımsız deneme sürümü yayımlama gereksinimleri açıklanmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: dsindona
ms.openlocfilehash: 34de5f59e96a37282063741a1664f512697c167b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288725"
---
# <a name="virtual-machine-offer-publishing-guide"></a>Sanal Makine Teklif Yayın Kılavuzu

Sanal Makine görüntüleri, Azure Marketi'nde bir çözüm yayınlamanın başlıca yollarından biridir. Bu teklifin gereksinimlerini anlamak için bu kılavuzu kullanın. 

Bunlar, Market aracılığıyla dağıtılan ve faturalanan işlem teklifleridir. Bir kullanıcının gördüğü eylem çağrısı "Şimdi Al" dır.

## <a name="free-trial"></a>Ücretsiz Deneme 

Kendi Lisansını Getir (BYOL) faturalandırma modelini kullanırken sınırlı süreli yazılım lisanslarına erişerek kullanıcıların teklifinizi test etmesini sağlayabilirsiniz. 

## <a name="test-drive"></a>Test Sürüşü

Bir veya daha fazla sanal makineyi hizmet olarak altyapı (IaaS) veya hizmet olarak yazılım (SaaS) uygulamaları aracılığıyla dağıtMış sınız. Test sürücüsü yayımlama seçeneğinin bir yararı, sanal bir makinenin veya iş ortağıtarafından barındırılan rehberli tur un tümünün otomatik olarak sağlanmasıdır. Test sürüşü, müşterinize ek ücret ödemeden bir değerlendirme sağlar. Deneme deneyimiyle etkileşimde bulunmak için müşterinizin mevcut bir Azure müşterisi olması gerekmez. 

Başlamak için [amp-testdrive](mailto:amp-testdrive@microsoft.com) bize ulaşın. 

|Gereksinimler  |Ayrıntılar |
|---------|---------|
| Bir Market uygulamanız var   |    IaaS veya SaaS üzerinden bir veya daha fazla sanal makine.      |

## <a name="interactive-demo"></a>İnteraktif Demo

Etkileşimli bir gösteri kullanarak müşterilerinize çözümünüz hakkında rehberli bir deneyim sunarsınız. Etkileşimli demo yayıncılık seçeneğinin yararı, karmaşık çözümünüzün karmaşık bir şekilde sağlanması olmadan bir deneme deneyimi sağlamanızdır. 

## <a name="virtual-machine-offer"></a>Sanal Makine Teklifi

Müşterinizle ilişkili aboneye sanal bir cihaz dağıttığınızda sanal makine teklif türünü kullanın. VM'ler kullandıkça öde veya kendi lisansını getir (BYOL) lisans modelleri kullanılarak tamamen ticaret ekidir. Microsoft, ticaret işlemini barındırıyor ve müşterinizi sizin adınıza faturaediyor. Müşteriniz ve Microsoft arasında, tüm Kurumsal Sözleşmeler de dahil olmak üzere tercih edilen ödeme ilişkisini kullanma avantajından yararlanırsınız.

> [!NOTE]
> Şu anda, Kurumsal Sözleşme ile ilişkili parasal taahhütler VM'nizin Azure kullanımına karşı kullanılabilir, ancak yazılım lisans ücretlerinize karşı kullanılamaz.  
> 
> [!NOTE]
> Resmi ve fiyatlandırmayı Özel teklif olarak yayımlayarak VM'nizin keşfini ve dağıtımını belirli bir müşteri kümesiyle sınırlandırabilirsiniz. Özel teklifler, en yakın müşterileriniz için özel teklifler oluşturma ve özelleştirilmiş yazılım ve terimler sunma olanağının kilidini açar. Özelleştirilmiş terimler, özel leştirilmiş fiyatlandırma ve terimlerle alan liderliğindeki anlaşmaların yanı sıra sınırlı sürüm yazılımlarına erken erişim de dahil olmak üzere çeşitli senaryoları vurgulamanızı sağlar. Özel teklifler, bu ayrıntılarla yeni bir SKU oluşturarak sınırlı sayıda müşteriye belirli fiyatlandırma veya ürünler vermenizi sağlar.  
> *   Özel Teklifler hakkında daha fazla bilgi için, azure.microsoft.com/blog/private-offers-on-azure-marketplace'da bulunan Azure Marketi'ndeki Özel Teklifler sayfasını ziyaret [edin.](https://azure.microsoft.com/blog/private-offers-on-azure-marketplace)  

| Gereksinim | Ayrıntılar |  
|:--- |:--- | 
| Faturalama ve ölçüm | VM'niz BYOL veya Pay-As-You-Go aylık faturalandırmayı desteklemelidir. |  
| Azure uyumlu sanal sabit disk (VHD) | VM'ler Windows veya Linux üzerine inşa edilmelidir. <ul> <li>Linux VHD oluşturma hakkında daha fazla bilgi için [Azure'da onaylanan Linux dağıtımlarına](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)bakın.</li> <li>Windows VHD oluşturma hakkında daha fazla bilgi için [bkz.](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md)</li> </ul> |  

>[!Note]
>Bulut Çözüm Sağlayıcıları (CSP) iş ortağı kanal tercihi artık kullanılabilir.  Microsoft CSP iş ortağı kanalları aracılığıyla teklifinizi pazarlama hakkında daha fazla bilgi için lütfen [Bulut Çözüm Sağlayıcıları'na](./cloud-solution-providers.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Eğer bunu yapmadıysanız, 

- Pazara [kaydolun.](https://azuremarketplace.microsoft.com/sell)

Kayıtlıysanız ve yeni bir teklif oluşturuyorsanız veya varolan bir teklif üzerinde çalışıyorsanız,

- Teklifinizi oluşturmak veya tamamlamak için [Cloud İş Ortağı Portalı'nda oturum](https://cloudpartner.azure.com) açın.
- Daha fazla bilgi için [Sanal makine teklifine](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-virtual-machine-offer) bakın.
