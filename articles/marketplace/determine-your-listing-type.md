---
title: Microsoft ticari pazar vitrinlerinde yayımlama seçeneğinizi belirleme
description: Bu makalede, uygulamaların Microsoft AppSource ve Azure Marketplace'te nasıl yayımlandırılabildiğini anlamaya çalışan iş ortakları için uygunluk ölçütleri ve yayımlama gereksinimleri açıklanmaktadır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: dsindona
ms.openlocfilehash: e91ca0334276dbe9118eb0e40d042f598e8b3c0f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415731"
---
# <a name="determine-your-publishing-option"></a>Yayımlama seçeneğinizi belirleme

Teklifiniz için seçtiğiniz yayımlama seçeneği, hem uygunluk gereksinimleri hem de ticari pazar daki GTM avantajlarıyla doğrudan ilgilidir. Daha da önemlisi, yayımlama seçeneği nin ve teklif türünün seçimi, kullanıcıların ticari pazar teklifinizle nasıl etkileşimde bulunacağını tanımlar.

Teklifinizi yapılandırmak için aşağıdaki temel ticari pazar kavramlarını anlamanız gerekir: yayımlama seçenekleri, teklif türleri ve yapılandırması ve teklifinizin ticari pazar vitrinlerinde nasıl ve nerede sunulacağını yönetecek eylem çağrıları.

Bu makalede aşağıdakileri öğreneceksiniz:

- Çözümünüz için uygun vitrin ibelirleyin
- Her vitrinde hangi yayımlama seçenekleri ve eylem çağrısı mevcuttur
- Her yayımlama seçeneği için hangi teklif türleri kullanılabilir

## <a name="commercial-marketplace-publishing-options"></a>Ticari pazar yayıncılık seçenekleri

Aşağıdaki tablo, Microsoft AppSource ve Azure Marketplace'teki teklif türleri için yayımlama seçeneklerini gösterir.

|   | **Liste (İletişim)**  | **Liste (Deneme)**  | **Ücretsiz** | **BYOL** | **Transact**|
| :--------- | :----------- | :------------ | :----------- | :---------- |:---------- |
| **Sanal Makine** |  |  |  | Azure Market |  Azure Market |
| **Azure Uygulamaları (çoklu VM)** |  |  | Azure Market | Azure Market | Azure Market  |
| **Konteyner görüntüsü** |  |  | Azure Market | Azure Market |   |
| **IoT Edge modülü** |  |  | Azure Market | Azure Market |   |
| **Yönetilen hizmetler** |  |  |  | Azure Market |   |
| **Danışmanlık hizmetleri** | Her iki vitrin |  |  |  |   |
| **SaaS uygulaması** | Her iki vitrin | Her iki vitrin | Her iki vitrin |  | Her iki vitrin* |
| **Microsoft 365 Uygulaması** | AppSource | AppSource |  |  | AppSource**  |
| **Dynamics 365 eklentisi** |  AppSource | AppSource |  |  |   |
| **PowerApps** | AppSource |AppSource  |  |  |   |

&#42; SaaS uygulaması Transact Microsoft AppSource teklifleri şu anda sadece kredi kartı vardır.

&#42;&#42; Microsoft 365 teklifleri yüklemek ücretsizdir ve bir lisans hizmeti olarak SaaS teklif üzerinden para kazanılabilir. Daha fazla bilgi için Bkz. [Microsoft ticari pazarı aracılığıyla Office 365 eklentinizden para kazanın.](/office/dev/store/monetize-addins-through-microsoft-commercial-marketplace)

## <a name="selecting-a-storefront"></a>Mağaza front seçme

Bir yayımlama seçeneği seçmeden önce, ticari pazar çözümleri, uygulamalar ve hizmetler için vitrin uygunluk gereksinimlerini anlamak önemlidir. Her vitrin benzersiz müşteri gereksinimlerine hizmet eder ve belirli kitleleri hedefler. Teklif türünüz, işlem özellikleriniz ve kategoriniz veya endüstriniz teklifinizi nerede yayınlayacağınızı belirler.

**Microsoft AppSource** uygulamaları, Yerleşik Azure veya yerleşik kullanılabilir: Dynamics 365, Office 365, Power BI veya Power Apps gibi iş yeri çözümleridir. AppSource danışmanlık hizmetleri, müşterilerin Dynamics 365 ve Power BI'nin kullanımını hızlandırmalarına veya yardımcı olan profesyonel hizmetler sunar.

**Azure Marketi** uygulamaları, azure için yerleşik veya yerleşik teknik "yapı bloğu" çözümleridir ve bir BT veya geliştirici kitlesi için tasarlanmıştır. Azure Marketi danışmanlık hizmetleri, müşterilerin Azure'un kullanımına başlamasına veya kullanımını hızlandırmalarına yardımcı olan profesyonel hizmetlersunar.

>[!Note]
>Çapraz giriş (yalnızca SaaS Uygulamaları için): Bir liste veya deneme tabanlı teklif hem teknik hem de iş amaçlı kullanıcı kitlesi için ölçütleri karşıladığında, teklifiniz her iki mağazada da listelenir. Aşağıdaki yayımlama seçenekleri hakkında daha fazla bilgi edinin.

## <a name="choose-a-publishing-option"></a>Yayımlama seçeneği ni seçin

Mevcut yayın seçenekleri, müşteri paylaşımı ve ticari pazar [avantajlarına](https://docs.microsoft.com/azure/marketplace/gtm-your-marketplace-benefits)erişim sağlarken farklı müşteri katılımı sunar. Yayımlama seçeneğiyle karşılık gelen eylem çağrılarına dikkat edin:

| **Yayımlama seçeneği**    | **Açıklama**  |
| :------------------- | :-------------------|
| **Liste** | Ticari bir pazar yeri **kullanıcısının, İletişim Beni'den** eyleme çağrı yoluyla müşteriyle bağlantı kurmanızı istemesini sağlayan uygulamanızın veya hizmetinizin basit bir şekilde listelenmesi. |
| **Deneme** | Olası kullanıcıların Satın almadan önce sınırlı bir süre için hiçbir ücret ödemeden SaaS, IaaS veya Microsoft uygulama içi deneyiminizi kullanmalarını sağlayarak, çözümünizin deneme deneyiminin kullanılabilirliğini artırmak ve otomatikleştirmek için ticari pazarı kullanın. Deneme yayımlama seçeneği için kullanılan eylem çağrısı ya **Ücretsiz Deneme** veya **Test Sürüşü'tür.** |
| **BYOL** | Keşfedilebilirliği artırmak ve çözümünüzün sağlanmasını otomatikleştirmek ve finansal işlemi ayrı ayrı tamamlamak için ticari pazarı kullanın. BYOL teklif türleri şirket içi bulut geçişleri için idealdir. Eylem çağrısı **Şimdi alın.**
| **Transact** | Transact teklifleri ticari pazar üzerinden satılmaktadır. Microsoft, faturalandırma ve tahsilatlardan sorumludur. Eylem çağrısı **Şimdi alın.**|

> [!Note]
> Transact yayımlama seçeneğini kullanırken, bir teklif türünü seçmeden ve teklifinizi oluşturmadan önce fiyatlandırma, faturalandırma, faturalandırma ve ödeme hususlarını anlamak önemlidir. Daha fazla bilgi için, makaleTicari pazar işlem yetenekleri gözden [geçirin.](./marketplace-commercial-transaction-capabilities-and-considerations.md)

## <a name="next-steps"></a>Sonraki adımlar

- Yayımlama seçeneğine karar veredikten sonra, teklifinizi sunmak için kullanılacak [teklif türünü seçmeye](./publisher-guide-by-offer-type.md) hazırsınız.
- Teklifinizin seçimini ve yapılandırmasını sonuçlandırmak için teklif türü bölümüne göre yayımlama seçeneklerindeki uygunluk gereksinimlerini gözden geçirin.
- Çözümünüzün teklif türü ve yapılandırmasına nasıl eşlerini oluşturduğuna ilişkin örnekler için yayın desenlerini vitrine göre inceleyin.
