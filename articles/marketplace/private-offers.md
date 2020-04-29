---
title: Özel teklifler | Azure Marketi
description: Uygulama ve hizmet yayımcıları için Azure Market 'te özel teklifler.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/19/2020
ms.author: dsindona
ms.openlocfilehash: efda9fb66a65f1f2cd40de8bf32a8a3fa99aea7f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81681425"
---
# <a name="private-offers"></a>Özel teklifler

Özel teklifler [Microsoft Azure Market](https://azuremarketplace.microsoft.com/) , yayımcıları yalnızca hedeflenen müşterilere görünen SKU 'lar oluşturmak üzere etkinleştirir.

## <a name="unlock-enterprise-deals-with-private-offers"></a>Özel tekliflerle kurumsal anlaşmalar üzerinde yararlanın

Kurumsal müşteriler, bulut çözümlerini bulmak, denemek ve satın almak için çevrimiçi pazar yerleri giderek daha fazla kullanır. Artık özel tekliflerle, yayımcılar, kuruluşların ihtiyaç duyduğu özellikleri karşılayan hedeflenen müşterilerle özelleştirilmiş çözümleri özel olarak paylaşmak için Market kullanabilir:

- *Anlaşmalı fiyatlandırma* , yayımcıların genel kullanıma sunulan tekliflerden indirimleri ve liste fiyatlarını genişletmesini sağlar.
- *Özel hüküm ve koşullar,* yayımcıların hüküm ve koşulları belirli bir müşteriye uyarlayabilmenizi sağlar.
- *Özelleştirilmiş konfigürasyonlar* , yayımcıların sanal makinelerini, Azure uygulamalarını ve SaaS uygulamalarını tek bir müşterinin ihtiyaçlarına uyarlamasına olanak sağlar. Bu seçenek, yayımcıların tüm müşterilere daha geniş bir şekilde başlatılmadan önce yeni ürün özelliklerine önizleme erişimi sağlamasına de olanak sağlar.

Özel teklifler, yayımcıların genel bir market 'in ölçek ve küresel kullanılabilirliğinden yararlanmasını sağlar ve özel anlaşmalar ve Konfigürasyonlar sağlamak için gereken esnekliği ve denetimi sağlar. Bu özellikler birlikte, bulut Pazar yerlerinin sağlam kurumsal benimseme kapağına yönelik kapıyı açar. Kuruluşlar artık bekledikleri ve talep ettikleri yollarla satın alabilir ve satılarlar.

Özel teklifler artık sanal makine, Azure uygulaması (çözüm şablonları veya yönetilen uygulamalar olarak uygulanır) ve SaaS uygulamaları tarafından sunulmaktadır. 

<!--- Like public offers, private offers can be created and managed via the [Cloud Partner Portal](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md).  Customers can be granted or revoked access to private offers in minutes.
--->

## <a name="creating-private-offers-using-skus-and-plans"></a>SKU 'Ları ve planları kullanarak özel teklifler oluşturma

*Kamu SKU 'ları veya planlarıyla yeni veya mevcut teklifler*için, yayımcılar yeni SKU 'lar veya planlar oluşturarak ve bunları özel olarak işaretleyerek kolayca yeni, özel Çeşitlemeler oluşturabilir.  

<!--- [Private SKUs](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) ---> 

Özel SKU 'Lar ve planlar bir teklifin bileşenleridir ve hedeflenen müşteriler tarafından yalnızca görünür ve satın alınabilir alınırken. Özel SKU 'Lar ve planlar, temel görüntüleri ve/veya zaten ortak bir SKU veya plan için yayımlanmış meta verileri kullanabilir. Bu seçenek, yayımcıların aynı temel görüntünün birden çok sürümünü yayınlamak ve meta verileri sunmak zorunda kalmadan ortak bir teklifin birden çok özel çeşitlemelerini oluşturmalarına olanak tanır. Yalnızca sanal makine ve Azure uygulaması için özel bir SKU ortak SKU 'SU olan bir temel görüntüyü paylaşıyorsa, teklifin temel görüntüsündeki tüm değişiklikler, bu temel görüntüyü kullanarak tüm ortak ve özel SKU 'Lara yayılır.

*Yalnızca özel SKU 'ları veya planları içeren yeni teklifler*için yayımcılar, tekliflerini başka bir teklif olarak oluşturabilir ve ardından SKU 'ları veya planları özel olarak işaretleyebilir. Yalnızca özel SKU 'Ların veya planların bulunduğu teklifler Microsoft ticari marketi veya teklifle ilişkilendirilmemiş müşteriler tarafından [Azure Portal](https://azure.microsoft.com/features/azure-portal/) bulunabilir veya erişilebilir olmayacaktır.

## <a name="targeting-customers-with-private-offers"></a>Özel tekliflerle müşterileri hedefleme

Yayımcılar, hem yeni hem de mevcut özel teklifleriniz için abonelik tanımlayıcılarını kullanarak müşterileri hedefleyebilir. Bir sanal makine veya Azure Uygulama teklifi kullanan yayımcılar, özel SKU 'nun kullanılabilirliğini tek bir Azure abonelik kimliğiyle kısıtlayabilir veya en fazla 20.000 Azure abonelik kimliği olan bir CSV 'yi karşıya yükleyebilir. SaaS uygulaması özel teklifi kullanılırken, yayımcılar, el ile veya CSV karşıya yükleme yaklaşımını kullanarak bir özel planın kullanılabilirliğini kısıtlamak için bir kiracı KIMLIĞINI ilişkilendirebilir.

Bir teklif sertifikalandırıldıktan ve yayımlandıktan sonra, müşteriler özel abonelikleri Eşitle özelliği kullanılarak SKU 'dan veya dakikalar içinde planlayabilir. Bu özellik, yayımcıların özel SKU veya planın sunulduğu müşteri listesini hızlı ve kolay bir şekilde güncelleştirmesine veya teklifi yayımlamadan veya yeniden yayımlamasına olanak sağlar.

## <a name="deploying-private-offers"></a>Özel teklifleri dağıtma

Özel Teklifler yalnızca [Azure Portal](https://azure.microsoft.com/features/azure-portal/) aracılığıyla bulunabilir ve [Microsoft AppSource](https://appsource.microsoft.com/) veya [Azure Marketi](https://azuremarketplace.microsoft.com)'nde gösterilmez. Farklı ticari Market stotörlerinin yayımlanması hakkında daha fazla bilgi edinmek için bkz. [Yayımlama seçeneğinizi belirleme](./determine-your-listing-type.md).

Müşteriler, Azure portal oturum açtıktan sonra özel tekliflerinden erişmek için Market gezinti öğesini seçebilir. Özel teklifler de arama sonuçlarında görünür ve diğer teklifler gibi komut satırı ve Azure Resource Manager şablonları aracılığıyla dağıtılabilir.

![[Özel teklifler]](./media/marketplace-publishers-guide/private-offer.png)

Özel teklifler, arama sonuçlarında da görünür. Yalnızca "özel" rozetini bulun.

>[!Note]
>Özel teklifler, bulut çözümü sağlayıcısı programı (CSP) satıcısı aracılığıyla oluşturulan aboneliklerle desteklenmez.

<!---
## Next steps

To start using private offers, follow the steps in the [Private SKUs and Plans](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) guide.

--->