---
title: Yayımlama seçeneğinizi belirleme-Microsoft ticari Market
description: Bu makalede, Microsoft AppSource ve Azure Market 'Te teklifler yayımlamaya yönelik uygunluk ölçütleri ve gereksinimleri açıklanmaktadır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/22/2020
ms.author: dsindona
ms.openlocfilehash: 457ff90c21fee2b569fa24d90fffc6ce80cdf969
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86027112"
---
# <a name="determine-your-publishing-option"></a>Yayımlama seçeneğinizi belirleme

Teklifiniz için seçtiğiniz yayımlama seçeneği, doğrudan uygunluk gereksinimlerine ve ticari Market GTM avantajlarına yöneliktir. Daha önemlisi, yayımlama seçeneği ve teklif türü seçimi, kullanıcıların ticari Market teklifinizle nasıl etkileşime gireceğini tanımlar.

Teklifinizi yapılandırmak için aşağıdaki temel ticari Market kavramlarını anlamanız gerekir: Yayımlama seçenekleri, teklif türleri ve yapılandırma ve teklifinizin ticari Market stokörlerde nasıl ve nerede sunulduğunu belirleyen, eylem çağrıları.

Bu makalede aşağıdakileri öğreneceksiniz:

- Çözümünüz için uygun storefront belirleme
- Her storefront için hangi yayımlama seçenekleri ve eylem çağrıları kullanılabilir
- Her yayımlama seçeneği için kullanılabilen teklif türleri

## <a name="commercial-marketplace-publishing-options"></a>Ticari Market Yayımlama seçenekleri

Aşağıdaki tabloda, Microsoft AppSource ve Azure Marketi 'nde teklif türleri için yayımlama seçenekleri gösterilmektedir.

|   | **Liste (kişi)**  | **Liste (deneme)**  | **Ücretsiz** | **KLG** | **Transact**|
| :--------- | :----------- | :------------ | :----------- | :---------- |:---------- |
| **Sanal Makine** |  |  |  | Azure Market |  Azure Market |
| **Azure uygulamaları (çoklu VM)** |  |  | Azure Market | Azure Market | Azure Market  |
| **Kapsayıcı görüntüsü** |  |  | Azure Market | Azure Market |   |
| **IoT Edge modülü** |  |  | Azure Market | Azure Market |   |
| **Yönetilen hizmetler** |  |  |  | Azure Market |   |
| **Danışmanlık hizmetleri** | Her iki vitrinler |  |  |  |   |
| **SaaS uygulaması** | Her iki vitrinler | Her iki vitrinler | Her iki vitrinler |  | Her iki storefronts * |
| **Microsoft 365 uygulaması** | AppSource | AppSource |  |  | AppSource * *  |
| **Dynamics 365 eklentisi** |  AppSource | AppSource |  |  |   |
| **PowerApps** | AppSource |AppSource  |  |  |   |

Microsoft AppSource SaaS uygulaması Transact tekliflerinin &#42; Şu anda yalnızca kredi kartınlardır.

&#42;&#42; Microsoft 365 teklifleri yüklenmeye ücretsizdir ve bir lisans hizmeti olarak SaaS teklifi aracılığıyla oluşturulabilir. Daha fazla bilgi için bkz. [Microsoft Commercial Market aracılığıyla Office 365 eklentilerinizi Moneleştirme](/office/dev/store/monetize-addins-through-microsoft-commercial-marketplace).

## <a name="selecting-a-storefront"></a>Storefront seçme

Bir yayımlama seçeneği seçmeden önce, ticari Market çözümleri, uygulamalar ve hizmetler için storefront uygunluk gereksinimlerinin anlaşılması önemlidir. Her storefront benzersiz müşteri gereksinimlerine hizmet eder ve belirli izleyicileri hedefler. Teklif türü, Transact özellikleri ve kategori ya da sektörünüz, teklifinizin nerede yayımlanacağını saptacaktır.

**Microsoft AppSource** uygulamalar, Azure veya yerleşik olarak kullanılabilecek iş kolu çözümleridir: Dynamics 365, Office 365, Power BI veya Power Apps. AppSource Danışmanlık Hizmetleri, müşterilerin Dynamics 365 ve Power BI kullanımını kullanmaya veya hızlandırmasına yardımcı olan profesyonel hizmet tekliflerdir.

**Azure Market** uygulamaları, Azure için yerleşik veya yerleşik olarak bulunan ve BT ya da geliştirici kitlesi için tasarlanan teknik "derleme bloğu" çözümleridir. Azure Market Danışmanlık Hizmetleri, müşterilerin Azure kullanımını kullanmaya veya hızlandırmasına yardımcı olan profesyonel hizmet teklifleridir.

>[!Note]
>"Çapraz listeleme" (yalnızca SaaS uygulamaları için): SaaS teklifiniz bir teknik hedef kitle (Azure Marketi) ve bir iş kitlesi (AppSource) için tasarlanıyorsa, Stoporonts için geçerli bir kategori ve/veya alt kategori seçebilirsiniz. Teklifinizin "çapraz listeleme" amacını, her iki hedef kitleye genişleten bir değer teklifini temel almalıdır. Her storefront için geçerli olan kategorileri görüntülemek için [buraya](./gtm-offer-listing-best-practices.md#categories) tıklayın.

## <a name="choose-a-publishing-option"></a>Yayımlama seçeneği seçin

Kullanılabilir yayımlama seçenekleri, müşteri adayı paylaşma ve [ticari Market avantajlarına](https://docs.microsoft.com/azure/marketplace/gtm-your-marketplace-benefits)erişmenizi sağlarken farklılaştırılan müşteri katılımı sunar. Yayımlama seçeneğiyle karşılık gelen eylem çağrılarını unutmayın:

| **Yayımlama seçeneği**    | **Açıklama**  |
| :------------------- | :-------------------|
| **Liste** | Uygulama veya hizmetinizin, bir ticari Market kullanıcısına **Ilgili kişi** eylemi çağrısı yoluyla müşteriyle bağlantı kurma isteğinde bulunan basit listesi. |
| **Deneme** | Ticari Market 'i kullanarak, çözümünüzün deneme deneyiminizin sağlamasını geliştirin ve bu sayede, olası kullanıcıların satın alınmadan önce sınırlı bir süre boyunca SaaS, IaaS veya Microsoft uygulama içi deneyiminizi kullanmasına olanak sağlar. Deneme yayımlama seçeneği için kullanılan çağrı eylemi, **ücretsiz deneme** veya **test sürücüsü**olabilir. |
| **KLG** | Keşfedilebilirliğini geliştirmek ve çözümünüzün sağlamasını otomatik hale getirmek için ticari Market 'i kullanın ve mali işlemi ayrı olarak doldurun. KLG teklif türleri şirket içinde bulut geçişleri için idealdir. Eyleme çağrı **Şimdi**alınır.
| **Transact** | Transact teklifleri ticari Market aracılığıyla satılır. Microsoft, faturalandırma ve koleksiyonlardan sorumludur. Eyleme çağrı **Şimdi**alınır.|

> [!Note]
> Transact Publishing seçeneğini kullanırken, bir teklif türü seçmeden ve teklifinizi oluşturmadan önce fiyatlandırma, faturalandırma, faturalama ve ödeme konularını anlamak önemlidir. Daha fazla bilgi edinmek için [ticari Market Transact özellikleri](./marketplace-commercial-transaction-capabilities-and-considerations.md)makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Bir yayımlama seçeneğine karar verdikten sonra, teklifinizi sunmak için kullanılacak [teklif türünü seçmek](./publisher-guide-by-offer-type.md) için hazır olursunuz.
- Teklifinizin seçimini ve yapılandırmasını tamamlamak için teklif türüne göre yayımlama seçeneklerinde uygunluk gereksinimlerini gözden geçirin.
- Çözümünüzün bir teklif türü ve yapılandırmaya nasıl eşlendiğini gösteren örnekler için storefront göre yayımlama düzenlerini gözden geçirin.
