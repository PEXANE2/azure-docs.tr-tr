---
title: Yayımlama seçeneğinizi belirleme-Microsoft ticari Market
description: Bu makalede, Microsoft AppSource ve Azure Market 'Te teklifler yayımlamaya yönelik uygunluk ölçütleri ve gereksinimleri açıklanmaktadır.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 07/30/2020
ms.openlocfilehash: 37647a9591c0f686e4fc3f1fd858baa46e01f7ac
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498081"
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

|   | **Liste (kişi)**  | **Liste (deneme)**  | **Ücretsiz** | **KLG** | **İşlem**|
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

## <a name="choose-a-call-to-action"></a>Eyleme çağrı seçin

Kullanılabilir yayımlama seçenekleri, müşteri adayı paylaşma ve [ticari Market avantajlarına](https://docs.microsoft.com/azure/marketplace/gtm-your-marketplace-benefits)erişmenizi sağlarken farklılaştırılan müşteri katılımı sunar. Yayımlama seçeneğiyle karşılık gelen eylem çağrılarını unutmayın:

| **Yayımlama seçeneği**    | **Açıklama**  |
| :------------------- | :-------------------|
| **Liste** | Uygulama veya hizmetinizin, bir ticari Market kullanıcısına **Ilgili kişi** eylemi çağrısı yoluyla müşteriyle bağlantı kurma isteğinde bulunan basit listesi. |
| **Deneme** | Ticari Market 'i kullanarak, çözümünüzün deneme deneyiminizin sağlamasını geliştirin ve bu sayede, olası kullanıcıların satın alınmadan önce sınırlı bir süre boyunca SaaS, IaaS veya Microsoft uygulama içi deneyiminizi kullanmasına olanak sağlar. Deneme yayımlama seçeneği için kullanılan çağrı eylemi, **ücretsiz deneme** veya **test sürücüsü**olabilir. |
| **KLG** | Keşfedilebilirliğini geliştirmek ve çözümünüzün sağlamasını otomatik hale getirmek için ticari Market 'i kullanın ve mali işlemi ayrı olarak doldurun. KLG teklif türleri şirket içinde bulut geçişleri için idealdir. Eyleme çağrı **Şimdi**alınır.
| **İşlem** | Transact teklifleri ticari Market aracılığıyla satılır. Microsoft, faturalandırma ve koleksiyonlardan sorumludur. Eyleme çağrı **Şimdi**alınır.|

> [!Note]
> Transact Publishing seçeneğini kullanırken, bir teklif türü seçmeden ve teklifinizi oluşturmadan önce fiyatlandırma, faturalandırma, faturalama ve ödeme konularını anlamak önemlidir. Daha fazla bilgi edinmek için [ticari Market Transact özellikleri](./marketplace-commercial-transaction-capabilities-and-considerations.md)makalesine bakın.

## <a name="selecting-a-storefront"></a>Storefront seçme

Her storefront benzersiz müşteri gereksinimlerine hizmet eder ve belirli izleyicileri hedefler. Teklif türü, Transact özellikleri ve kategorili, teklifinizin nerede yayımlanacağını saptacaktır. Kategoriler ve alt kategoriler, hedef kitleye göre her storefront eşlenir:

**Microsoft AppSource** , Dynamics 365, Microsoft 365 ve güç platformu için iş kolu veya sektör çözümleri ve danışmanlık hizmetleri arayan iş kullanıcılarını hedefler.

Azure **marketi** , BT uzmanlarına ve geliştiricilere Azure 'un kullanımını hızlandırmaya yönelik danışmanlık hizmetleri ve Azure için oluşturulmuş çözümler arayan BT uzmanlarına ve geliştiricilere yöneliktir.

Hedef kitlenize en uygun kategoriyi ve alt kategorisini seçin. Örneğin, Web uygulaması güvenlik duvarı, hedeflenen hedef kitle BT uzmanları olduğu için güvenlik kategorisi altında Azure Marketi 'Nde yayımlanmalıdır. Hedeflenen hedef kitle iş kullanıcıları olduğundan, bir sözleşme yönetimi uygulamasının satış kategorisi altında AppSource 'a yayımlanması gerekir. Yanlış kategori veya alt kategori seçilmesi, teklifinizin yanlış storefront yayımlanmasına neden olabilir.

### <a name="publishing-to-both-storefronts-saas-offers-only"></a>Her iki Stoporonts 'e yayımlama (yalnızca SaaS teklifleri)

SaaS teklifleri, Azure Market veya AppSource 'ta yayımlanabilir. SaaS teklifiniz hem teknik bir dinleyici (Azure Marketi) hem *de* iş kitlesi (appsource) için tasarlanıyorsa, her storefront için geçerli bir kategori ve/veya alt kategori seçin. Her iki Storede yayımlanan teklifler, BT uzmanlarına *ve* iş kullanıcılarına genişleyen bir değer teklifi içermelidir.

> [!IMPORTANT]
> Tarifeli faturalandırma ile SaaS teklifleri, Azure Marketi ve Azure portal aracılığıyla kullanılabilir. Yalnızca özel planlara sahip SaaS teklifleri Azure portal aracılığıyla kullanılabilir.

| Ölçülen faturalandırma | Ortak plan | Özel plan | Kullanılabilir: |
|---|---|---|---|
| Yes             | Evet         | Hayır           | Azure Marketi ve Azure portal |
| Yes             | Yes         | Yes          | Azure Marketi ve Azure portal * |
| Yes             | No          | Yes          | Yalnızca Azure portal |
| Hayır              | Hayır          | Yes          | Yalnızca Azure portal |

&#42; teklifin özel planı yalnızca Azure portal ile kullanılabilir

Örneğin, tarifeli faturalandırma ve yalnızca özel planı (genel plan olmadan) içeren bir teklif, Azure portal müşteriler tarafından satın alınacaktır. [Microsoft ticari Market 'Teki özel teklifler](private-offers.md)hakkında daha fazla bilgi edinin.

### <a name="categories"></a>Kategoriler

Kategoriler ve alt kategoriler, hedef kitleye göre her storefront eşlenir. Teklifinizi ve hedeflenen kitleyi en iyi şekilde hizalamanızı sağlayan kategorileri ve alt kategorileri seçin. Şunları seçebilirsiniz:

- En az bir ve en fazla iki kategori. Birincil ve ikincil kategori seçme seçeneğiniz vardır.
- Her birincil ve/veya ikincil kategori için en fazla iki alt kategori. Herhangi bir alt kategori seçmezseniz, teklifiniz seçili kategori altında keşfedilebilir olmaya devam eder.

[!INCLUDE [categories and subcategories](./includes/categories.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Bir yayımlama seçeneğine karar verdikten sonra, teklifinizi sunmak için kullanılacak [teklif türünü seçmek](./publisher-guide-by-offer-type.md) için hazır olursunuz.
- Teklifinizin seçimini ve yapılandırmasını tamamlamak için teklif türüne göre yayımlama seçeneklerinde uygunluk gereksinimlerini gözden geçirin.
- Çözümünüzün bir teklif türü ve yapılandırmaya nasıl eşlendiğini gösteren örnekler için storefront göre yayımlama düzenlerini gözden geçirin.
