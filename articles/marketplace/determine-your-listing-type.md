---
title: Yayımlama seçeneğinizi belirleme-Microsoft ticari Market
description: Bu makalede, Microsoft AppSource ve Azure Market 'Te teklifler yayımlamaya yönelik uygunluk ölçütleri ve gereksinimleri açıklanmaktadır.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 09/04/2020
ms.openlocfilehash: 7acfd5e1a1b1c44a9a109bb2b8bc38c7fcc57593
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89484876"
---
# <a name="determine-your-publishing-option"></a>Yayımlama seçeneğinizi belirleme

Teklifiniz için seçtiğiniz yayımlama seçeneği, doğrudan uygunluk gereksinimlerine ve ticari Market GTM avantajlarına yöneliktir. Daha önemlisi, yayımlama seçeneği ve teklif türü seçimi, kullanıcıların ticari Market teklifinizle nasıl etkileşime gireceğini tanımlar.

Teklifinizi yapılandırmak için aşağıdaki temel ticari Market kavramlarını anlamanız gerekir: Yayımlama seçenekleri, teklif türleri ve yapılandırma ve teklifinizin ticari Market online mağazalarında nasıl ve nerede sunulduğunu belirleyen listeleme seçenekleri.

Bu makalede aşağıdakileri öğreneceksiniz:

- Çözümünüz için uygun çevrimiçi mağazayı belirleme.
- Her çevrimiçi mağazada hangi yayımlama seçenekleri ve listeleme seçenekleri bulunur.
- Her yayımlama seçeneği için hangi teklif türleri kullanılabilir.

## <a name="commercial-marketplace-publishing-options"></a>Ticari Market Yayımlama seçenekleri

Aşağıdaki tabloda, Microsoft AppSource ve Azure Marketi 'nde teklif türleri için yayımlama seçenekleri gösterilmektedir.

|   | **Liste (kişi)**  | **Liste (deneme)**  | **Ücretsiz** | **KLG** | **İşlem**|
| :--------- | :----------- | :------------ | :----------- | :---------- |:---------- |
| **Sanal Makine** |  |  |  | Azure Market |  Azure Market |
| **Azure uygulamaları (çoklu VM)** |  |  | Azure Market | Azure Market | Azure Market  |
| **Kapsayıcı görüntüsü** |  |  | Azure Market | Azure Market |   |
| **IoT Edge modülü** |  |  | Azure Market | Azure Market |   |
| **Yönetilen hizmetler** |  |  |  | Azure Market |   |
| **Danışmanlık hizmetleri** | Her iki çevrimiçi mağaza |  |  |  |   |
| **SaaS uygulaması** | Her iki çevrimiçi mağaza | Her iki çevrimiçi mağaza | Her iki çevrimiçi mağaza |  | Her iki çevrimiçi mağaza * |
| **Microsoft 365 uygulaması** | AppSource | AppSource |  |  | AppSource * *  |
| **Dynamics 365 eklentisi** |  AppSource | AppSource |  |  |   |
| **PowerApps** | AppSource |AppSource  |  |  |   |

Microsoft AppSource SaaS uygulaması Transact tekliflerinin &#42; Şu anda yalnızca kredi kartınlardır.

&#42;&#42; Microsoft 365 teklifleri yüklenmeye ücretsizdir ve bir lisans hizmeti olarak SaaS teklifi aracılığıyla oluşturulabilir. Daha fazla bilgi için bkz. [Microsoft Commercial Market aracılığıyla Office 365 eklentilerinizi Moneleştirme](/office/dev/store/monetize-addins-through-microsoft-commercial-marketplace).

## <a name="choose-a-listing-option"></a>Listeleme seçeneği seçin

Mevcut olan liste seçenekleri, müşteri adayı paylaşma ve [ticari Market avantajlarına](https://docs.microsoft.com/azure/marketplace/gtm-your-marketplace-benefits)erişmenizi sağlarken farklılaştırılan müşteri katılımını sunmaktadır. Yayımlama seçeneğiyle karşılık gelen listeleme seçeneklerini unutmayın:

| **Yayımlama seçeneği**    | **Açıklama**  |
| :------------------- | :-------------------|
| **Liste** | Uygulama veya hizmetinizin, bir ticari Market kullanıcısına **Ilgili kişi** listeleme seçenekleri aracılığıyla müşteriye bağlanmanızı istemesini sağlayan basit listesi. |
| **Deneme** | Ticari Market 'i kullanarak, çözümünüzün deneme deneyiminizin sağlamasını geliştirin ve bu sayede, olası kullanıcıların satın alınmadan önce sınırlı bir süre boyunca SaaS, IaaS veya Microsoft uygulama içi deneyiminizi kullanmasına olanak sağlar. Deneme yayımlama seçeneği için kullanılan liste seçenekleri, **ücretsiz deneme** veya **Test sürüşdir**. |
| **KLG** | Keşfedilebilirliğini geliştirmek ve çözümünüzün sağlamasını otomatik hale getirmek için ticari Market 'i kullanın ve mali işlemi ayrı olarak doldurun. KLG teklif türleri şirket içinde bulut geçişleri için idealdir. Listeleme seçeneği **Şimdi alınır**.
| **İşlem** | Transact teklifleri ticari Market aracılığıyla satılır. Microsoft, faturalandırma ve koleksiyonlardan sorumludur. Listeleme seçeneği **Şimdi alınır**.|

> [!Note]
> Transact Publishing seçeneğini kullanırken, bir teklif türü seçmeden ve teklifinizi oluşturmadan önce fiyatlandırma, faturalandırma, faturalama ve ödeme konularını anlamak önemlidir. Daha fazla bilgi edinmek için [ticari Market Transact özellikleri](./marketplace-commercial-transaction-capabilities-and-considerations.md)makalesine bakın.

## <a name="selecting-an-online-store"></a>Çevrimiçi mağaza seçme

Her çevrimiçi mağaza, iş ve BT çözümleri için farklı müşteri gereksinimlerine hizmet eder. Teklif türü, Transact özellikleri ve kategorili, teklifinizin nerede yayımlanacağını saptacaktır. Kategoriler ve alt kategoriler, yayımladığınız çözüm türüne göre her bir çevrimiçi mağazaya eşlenir:

**Microsoft AppSource** , Dynamics 365, Microsoft 365 ve güç platformu için sektör çözümleri ve danışmanlık hizmetleri gibi iş çözümleri sunar.

Azure **marketi** , Azure 'Da veya Azure 'DA oluşturulan BT çözümlerini ve müşterilerinizin Azure kullanımını hızlandıran danışmanlık hizmetlerini sunar.

Çözüm türü ile en uygun kategoriyi ve alt kategorisini seçin. Örneğin, bir Web uygulaması güvenlik duvarı, güvenlik kategorisi altında Azure Marketi 'ne yayımlanması gereken bir BT çözümüdür. Bir sözleşme yönetimi uygulaması, satış kategorisi altında AppSource 'a yayımlanması gereken bir iş çözümüdür. Yanlış kategori veya alt kategori seçilmesi, teklifinizin yanlış çevrimiçi mağazaya yayımlanmasına neden olabilir.

### <a name="publishing-to-both-online-stores-saas-offers-only"></a>Çevrimiçi mağazalara yayımlama (yalnızca SaaS teklifleri)

SaaS teklifleri, Azure Market veya AppSource 'ta yayımlanabilir. SaaS teklifiniz hem BT çözümü (Azure Marketi) hem *de* bir iş çözümü (appsource) ise, her çevrimiçi mağaza için geçerli bir kategori ve/veya alt kategori seçin. Her iki çevrimiçi mağaza için yayımlanan teklifler, BT çözümü *ve* iş çözümü olarak bir değer teklifi içermelidir.

> [!IMPORTANT]
> [Tarifeli faturalandırma](partner-center-portal/saas-metered-billing.md) ile SaaS teklifleri, Azure marketi ve Azure Portal aracılığıyla kullanılabilir. Yalnızca özel planlara sahip SaaS teklifleri Azure portal aracılığıyla kullanılabilir.

| Ölçülen faturalandırma | Ortak plan | Özel plan | Kullanılabilir: |
|---|---|---|---|
| Yes             | Yes         | Hayır           | Azure Marketi ve Azure portal |
| Yes             | Yes         | Yes          | Azure Marketi ve Azure portal * |
| Yes             | Hayır          | Yes          | Yalnızca Azure portal |
| Hayır              | Hayır          | Yes          | Yalnızca Azure portal |

&#42; teklifin özel planı yalnızca Azure portal ile kullanılabilir

Örneğin, tarifeli faturalandırma ve yalnızca özel planı (genel plan olmadan) içeren bir teklif, Azure portal müşteriler tarafından satın alınacaktır. [Microsoft ticari Market 'Teki özel teklifler](private-offers.md)hakkında daha fazla bilgi edinin.

### <a name="categories"></a>Kategoriler

Kategoriler ve alt kategoriler, çözüm türüne göre her bir çevrimiçi mağazaya eşlenir. Çözümünüze en iyi şekilde uyum sağlayan kategorileri ve alt kategorileri seçin. Şunları seçebilirsiniz:

- En az bir ve en fazla iki kategori. Birincil ve ikincil bir kategori seçebilirsiniz.
- Her birincil ve/veya ikincil kategori için en fazla iki alt kategori. Herhangi bir alt kategori seçmezseniz, teklifiniz seçili kategori altında keşfedilebilir olmaya devam eder.

[!INCLUDE [categories and subcategories](./includes/categories.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Bir yayımlama seçeneğine karar verdikten sonra, [teklif türüne göre yayımlama kılavuzunu](./publisher-guide-by-offer-type.md)gözden geçirin.
