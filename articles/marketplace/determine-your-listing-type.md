---
title: Yayımlama belirleme seçeneği Azure Marketi'nde | Azure
description: Bu makalede, uygulamalarını Azure Market'te yayımlama işlemini anlama çalışılırken yayımlama gereksinimleri iş ortakları ve uygunluk ölçütlerini açıklanır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: jm-aditi-ms
manager: pabutler
ms.service: marketplace
ms.topic: article
ms.date: 06/13/2018
ms.author: ellacroi
ms.openlocfilehash: 49723a80bb198e8151c7e85a90453183068c9d2b
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70962814"
---
# <a name="determine-your-publishing-option"></a>Yayımlama seçeneğinizi belirleyin
Teklifiniz için seçtiğiniz yayımlama seçeneği doğrudan hem uygunluk gereksinimleri ve Market Avantajlarımız ilişkilendirir. Daha önemlisi, yayımlama seçeneği ve teklif türü seçimi, kullanıcıların Market teklifinizle nasıl etkileşime gireceğini tanımlar.

Teklifinizi yapılandırmak için aşağıdaki ana Market kavramlarını anlamanız gerekir: Yayımlama seçenekleri, teklif türleri ve yapılandırma ve teklifinizin Market stokörlerde nasıl ve nerede sunulduğunu belirleyen, eylem çağrıları.

![](./media/marketplace-publishers-guide/storefronts_options_table.png)


Bu makalede, öğreneceksiniz...
<ul><li>    Çözümünüz için uygun mağazada belirleme </ul></li>
<ul><li>    Her vitrin hangi Yayımlama seçenekleri ve çağrıları eylemi kullanılabilir </ul></li>
<ul><li>    Sunan türleri, yayımlama seçeneklerinin her biri için kullanılabilir </ul></li>


## <a name="selecting-a-storefront-publishing-option-and-offer-type-for-your-solution"></a>Bir mağaza seçme, seçenek ve çözümünüz için Teklif türü yayımlama

Yayımlama seçeneği seçmeden önce Market çözümleri, uygulamalar ve hizmetler için mağaza uygunluk gereksinimlerini anlamak önemlidir:

**Azure Market** uygulamaları, Azure için yerleşik veya yerleşik olarak bulunan ve BT ya da geliştirici kitlesi için tasarlanan teknik "derleme bloğu" çözümleridir. Azure Market danışmanlık hizmetleri, Profesyonel hizmetler müşterilere yardımcı tekliflerini kullanmaya başlayın veya Azure kullanımını hızlandırın.

**Appsource** uygulamaları, Azure 'da veya yerleşik olarak yerleşik olabilecek iş kolu çözümleridir: Dynamics 365, Office 365, Power BI veya Power Apps. AppSource danışmanlık hizmetleri kullanmaya başlayın ya da Dynamics 365 ve Power BI kullanımını hızlandırın müşterilere yardımcı Profesyonel hizmetler tekliflerdir.


## <a name="understand-storefront-selection"></a>Vitrin seçimi anlama

Teklifinizin sunulacak olan storefront, Azure Marketi ve/veya AppSource, teklif ayrıntılarınız ve hedef kitlelerinizin yanı sıra teklifinizi oluştururken seçtiğiniz Kategoriler ve endüstriler tarafından otomatik olarak belirlenir. 

>[!Note]
>"Arası listeleme" (SaaS uygulamaları için yalnızca): bir liste veya deneme tabanlı teklif hem bir teknik ve işletmeye kullanıcı kitlesi ölçütleri karşıladığında teklifinizi hem vitrinler içinde listelenir. Aşağıdaki Yayımlama seçenekleri hakkında daha fazla bilgi edinin.

## <a name="choose-a-publishing-option"></a>Yayımlama seçeneği

Kullanılabilir yayımlama seçenekleri, müşteri adayı paylaşma ve [ticari Market avantajlarına](https://docs.microsoft.com/azure/marketplace/gtm-your-marketplace-benefits)erişmenizi sağlarken farklılaştırılan müşteri katılımı sunar.  Çağrıları yayımlama seçeneğiyle karşılık gelen eylem dikkat edin:

| **Yayımlama seçeneği**    | **Açıklama**  |
| :------------------- | :-------------------|
| **Liste** | Uygulamanız veya hizmetiniz bir Market kullanıcının isteği müşteri ile bağlanmanıza olanak sağlayan listesi basit **benimle iletişim kurun** eylem çağrısı. |
| **Deneme** | Market 'i kullanarak, çözümünüzün deneme deneyiminizin sağlamasını geliştirin ve bu sayede, olası kullanıcıların satın almadan önce sınırlı bir süre boyunca SaaS, IaaS veya Microsoft uygulama içi deneyiminizi kullanmasına olanak sağlar. Deneme yayımlama seçeneği için kullanılan çağrı eylemi şunlardır: **Ücretsiz deneme** veya **test sürücüsü**. |
|**KLG**  |Keşfedilebilirliğini geliştirmek ve çözümünüzün sağlamasını otomatik hale getirmek için Market 'i kullanın ve mali işlemi ayrı olarak doldurun. KLG teklif türleri şirket içinde bulut geçişleri için idealdir. Eyleme çağrı **Şimdi**alınır.
| **Transact** | Uygulamanız Azure 'da çalışır ve müşteri **Şimdi bu uygulamayı şimdi al** eylemini seçtiğinde müşterinin Azure aboneliğine doğrudan bir kaynak olarak sağlanabilir. Yazılım Lisans ücretleri, isteğe bağlı olarak satın alınıp müşterinin tercih ettiğiniz ödeme ve koşulları faturalandırılır ve süre sınırlı erişim sunmak seçebilirsiniz, **ücretsiz yazılım deneme sürümü** (yalnızca Azure için kullanılabilir Market.) |

>[!Note]
>Yayımlama seçeneği Transact kullanırken, fiyatlandırma, faturalandırma, faturalama ve ödeme konuları teklif türünü seçme ve teklifinizi oluşturma önce anlamak önemlidir. Gözden geçirme [daha fazla bilgi için Market faturalama ve ticari konuları makale](./marketplace-commercial-transaction-capabilities-and-considerations.md).

## <a name="next-steps"></a>Sonraki adımlar

*   Bir yayımlama seçeneği karar verdiğinizde, hazır olduğunuz [teklif türünü seçin](./publisher-guide-by-offer-type.md) teklifinizi sunmak için kullanılır.
*   Seçme ve yapılandırma teklifinizin sonlandırmak için Teklif türü bölümünde yayımlama seçeneklerini uygunluk gereksinimlerini gözden geçirin.
*   Yayımlama modelleri, çözümünüzü bir teklif türüne ve yapılandırmayı eşlemelerini nasıl ilişkin örnekler için mağaza tarafından gözden geçirin.



