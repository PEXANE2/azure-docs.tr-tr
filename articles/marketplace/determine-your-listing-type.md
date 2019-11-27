---
title: Yayımlama belirleme seçeneği Azure Marketi'nde | Azure
description: Bu makalede, uygulamalarını Azure Market'te yayımlama işlemini anlama çalışılırken yayımlama gereksinimleri iş ortakları ve uygunluk ölçütlerini açıklanır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: ellacroi
manager: pabutler
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 06/13/2018
ms.author: ellacroi
ms.openlocfilehash: ed25b92dc759f687e35a8424558453a830369a57
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74548349"
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

Bir yayımlama seçeneği seçmeden önce Market çözümleri, uygulamalar ve hizmetler için storefront uygunluk gereksinimlerinin anlaşılması önemlidir:

**Azure Market** uygulamaları, Azure için yerleşik veya yerleşik olarak bulunan ve BT ya da geliştirici kitlesi için tasarlanan teknik "derleme bloğu" çözümleridir. Azure Market danışmanlık hizmetleri, Profesyonel hizmetler müşterilere yardımcı tekliflerini kullanmaya başlayın veya Azure kullanımını hızlandırın.

**Appsource** uygulamaları, Azure veya yerleşik olarak kullanılabilecek iş kolu çözümleridir: Dynamics 365, Office 365, Power BI veya Power Apps. AppSource danışmanlık hizmetleri kullanmaya başlayın ya da Dynamics 365 ve Power BI kullanımını hızlandırın müşterilere yardımcı Profesyonel hizmetler tekliflerdir.


## <a name="understand-storefront-selection"></a>Vitrin seçimi anlama

Her storefront benzersiz müşteri gereksinimlerine hizmet eder. Teklifinizin yayımlanacağı storefront, belirli bir izleyiciyi hedeflemek için teklif türü, Transact özellikleri ve kategori ve/veya sektör seçimi sunacak şekilde belirlenir. 

>[!Note]
>"Çapraz listeleme" (yalnızca SaaS uygulamaları için): bir liste veya deneme tabanlı teklif, hem teknik hem de iş Kullanıcı kitleleriyle ilgili ölçütleri karşıladığında, teklifiniz her iki stoporonun içinde de listelenir. Aşağıdaki Yayımlama seçenekleri hakkında daha fazla bilgi edinin.

## <a name="choose-a-publishing-option"></a>Yayımlama seçeneği

Kullanılabilir yayımlama seçenekleri, müşteri adayı paylaşma ve [ticari Market avantajlarına](https://docs.microsoft.com/azure/marketplace/gtm-your-marketplace-benefits)erişmenizi sağlarken farklılaştırılan müşteri katılımı sunar.  Çağrıları yayımlama seçeneğiyle karşılık gelen eylem dikkat edin:

| **Yayımlama seçeneği**    | **Açıklama**  |
| :------------------- | :-------------------|
| **Listele** | Uygulama veya hizmetinizin, bir market kullanıcısına **Ilgili kişi** eylemi çağrısı aracılığıyla müşteriye bağlanmanızı istemesini sağlayan basit listesi. |
| **Deneme** | Market 'i kullanarak, çözümünüzün deneme deneyiminizin sağlamasını geliştirin ve bu sayede, olası kullanıcıların satın almadan önce sınırlı bir süre boyunca SaaS, IaaS veya Microsoft uygulama içi deneyiminizi kullanmasına olanak sağlar. Deneme yayımlama seçeneği için kullanılan eylem çağrıları: **ücretsiz deneme** veya **test sürücüsü**. |
|**KLG**  |Keşfedilebilirliğini geliştirmek ve çözümünüzün sağlamasını otomatik hale getirmek için Market 'i kullanın ve mali işlemi ayrı olarak doldurun. KLG teklif türleri şirket içinde bulut geçişleri için idealdir. Eyleme çağrı **Şimdi**alınır.
| **Transact** | Uygulamanız Azure 'da çalışır ve müşteri **Şimdi bu uygulamayı şimdi al** eylemini seçtiğinde müşterinin Azure aboneliğine doğrudan bir kaynak olarak sağlanabilir. Yazılım lisans ücretleri, isteğe bağlı olarak müşterinin ödeme aracı ve koşulları aracılığıyla satın alınabilir ve faturalandırılır ve **ücretsiz yazılım denemenize** (yalnızca Azure Marketi 'nde kullanılabilir) zamana sınırlı erişim sunmayı tercih edebilirsiniz. |

>[!Note]
>Yayımlama seçeneği Transact kullanırken, fiyatlandırma, faturalandırma, faturalama ve ödeme konuları teklif türünü seçme ve teklifinizi oluşturma önce anlamak önemlidir. [Daha fazla bilgi edinmek Için Market faturalandırma ve ticari hususlar makalesini](./marketplace-commercial-transaction-capabilities-and-considerations.md)gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

*   Bir yayımlama seçeneğine karar verdikten sonra, teklifinizi sunmak için kullanılacak [teklif türünü seçmek](./publisher-guide-by-offer-type.md) için hazır olursunuz.
*   Seçme ve yapılandırma teklifinizin sonlandırmak için Teklif türü bölümünde yayımlama seçeneklerini uygunluk gereksinimlerini gözden geçirin.
*   Yayımlama modelleri, çözümünüzü bir teklif türüne ve yapılandırmayı eşlemelerini nasıl ilişkin örnekler için mağaza tarafından gözden geçirin.



