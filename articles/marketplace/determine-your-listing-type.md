---
title: Ticari pazarda yayımlama seçeneğinizi belirleyin
description: Bu makalede, uygulamaların ticari pazarda nasıl yayımlandırılabildiğini anlamaya çalışan uygunluk ölçütleri ve yayımlama gereksinimleri açıklanmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: dsindona
ms.openlocfilehash: 84d5c351f3997fe5430ea1ce517c2dca13203c03
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261695"
---
# <a name="determine-your-publishing-option"></a>Yayımlama seçeneğinizi belirleme

Teklifiniz için seçtiğiniz yayımlama seçeneği, hem uygunluk gereksinimleri hem de ticari pazar daki GTM avantajlarıyla doğrudan ilgilidir. Daha da önemlisi, yayımlama seçeneği nin ve teklif türünün seçimi, kullanıcıların ticari pazar teklifinizle nasıl etkileşimde bulunacağını tanımlar.

Teklifinizi yapılandırmak için aşağıdaki temel ticari pazar kavramlarını anlamanız gerekir: yayımlama seçenekleri, teklif türleri ve yapılandırması ve teklifinizin ticari pazar vitrinlerinde nasıl ve nerede sunulacağını yönetecek eylem çağrıları.

![Microsoft ticari pazar vitrinleri ve yayımlama seçenekleri](./media/marketplace-publishers-guide/storefronts_options_table.png)

Bu makalede aşağıdakileri öğreneceksiniz:

- Çözümünüz için uygun vitrin ibelirleyin
- Her vitrinde hangi yayımlama seçenekleri ve eylem çağrısı mevcuttur
- Her yayımlama seçeneği için hangi teklif türleri kullanılabilir

## <a name="selecting-a-storefront-publishing-option-and-offer-type-for-your-solution"></a>Çözümünüz için bir mağaza, yayımlama seçeneği ve teklif türü seçme

Bir yayımlama seçeneği seçmeden önce, ticari pazar çözümleri, uygulamalar ve hizmetler için vitrin uygunluk gereksinimlerini anlamak önemlidir:

**Microsoft AppSource** uygulamaları, Yerleşik Azure veya yerleşik kullanılabilir: Dynamics 365, Office 365, Power BI veya Power Apps gibi iş yeri çözümleridir. AppSource danışmanlık hizmetleri, müşterilerin Dynamics 365 ve Power BI'nin kullanımını hızlandırmalarına veya yardımcı olan profesyonel hizmetler sunar.

**Azure Marketi** uygulamaları, azure için yerleşik veya yerleşik teknik "yapı bloğu" çözümleridir ve bir BT veya geliştirici kitlesi için tasarlanmıştır. Azure Marketi danışmanlık hizmetleri, müşterilerin Azure'un kullanımına başlamasına veya kullanımını hızlandırmalarına yardımcı olan profesyonel hizmetlersunar.

## <a name="understand-storefront-selection"></a>Mağaza ön seçimini anlama

Her vitrin benzersiz müşteri gereksinimleri sunmaktadır. Teklifinizin yayınlanacağı mağaza, teklif türüne göre belirlenecektir, belirli bir kitleyi hedeflemek için işlem özellikleri ve kategori ve/veya endüstri seçimi sunar. 

>[!Note]
>"Çapraz giriş" (yalnızca SaaS Uygulamaları için): Bir liste veya deneme tabanlı teklif hem teknik hem de iş amaçlı kullanıcı kitlesi için ölçütleri karşıladığında, teklifiniz her iki mağazada da listelenir. Aşağıdaki yayımlama seçenekleri hakkında daha fazla bilgi edinin.

## <a name="choose-a-publishing-option"></a>Yayımlama seçeneği ni seçin

Mevcut yayın seçenekleri, müşteri paylaşımı ve ticari pazar [avantajlarına](https://docs.microsoft.com/azure/marketplace/gtm-your-marketplace-benefits)erişim sağlarken farklı müşteri katılımı sunar.  Yayımlama seçeneğiyle karşılık gelen eylem çağrılarına dikkat edin:

| **Yayımlama seçeneği**    | **Açıklama**  |
| :------------------- | :-------------------|
| **Liste** | Ticari bir pazar yeri **kullanıcısının, İletişim Beni'den** eyleme çağrı yoluyla müşteriyle bağlantı kurmanızı istemesini sağlayan uygulamanızın veya hizmetinizin basit bir şekilde listelenmesi. |
| **Deneme** | Olası kullanıcıların Satın almadan önce sınırlı bir süre için hiçbir ücret ödemeden SaaS, IaaS veya Microsoft uygulama içi deneyiminizi kullanmalarını sağlayarak, çözümünizin deneme deneyiminin kullanılabilirliğini artırmak ve otomatikleştirmek için ticari pazarı kullanın. Deneme yayımlama seçeneği için kullanılan eylem çağrısı ya **Ücretsiz Deneme** veya **Test Sürüşü'tür.** |
| **BYOL** | Keşfedilebilirliği artırmak ve çözümünüzün sağlanmasını otomatikleştirmek ve finansal işlemi ayrı ayrı tamamlamak için ticari pazarı kullanın. BYOL teklif türleri şirket içi bulut geçişleri için idealdir. Eylem çağrısı **Şimdi alın.**
| **Transact** | Transact teklifleri ticari pazar üzerinden satılmaktadır. Microsoft, faturalandırma ve tahsilatlardan sorumludur. Eylem çağrısı **Şimdi alın.**|

>[!Note]
>Transact yayımlama seçeneğini kullanırken, bir teklif türünü seçmeden ve teklifinizi oluşturmadan önce fiyatlandırma, faturalandırma, faturalandırma ve ödeme hususlarını anlamak önemlidir. Daha [fazla bilgi için Marketplace Billing ve Ticari Hususlar makalesini gözden geçirin.](./marketplace-commercial-transaction-capabilities-and-considerations.md)

## <a name="next-steps"></a>Sonraki adımlar

- Yayımlama seçeneğine karar veredikten sonra, teklifinizi sunmak için kullanılacak [teklif türünü seçmeye](./publisher-guide-by-offer-type.md) hazırsınız.
- Teklifinizin seçimini ve yapılandırmasını sonuçlandırmak için teklif türü bölümüne göre yayımlama seçeneklerindeki uygunluk gereksinimlerini gözden geçirin.
- Çözümünüzün teklif türü ve yapılandırmasına nasıl eşlerini oluşturduğuna ilişkin örnekler için yayın desenlerini vitrine göre inceleyin.
