---
title: Microsoft ticari Market 'teki özel teklifler
description: Uygulama ve hizmet yayımcıları için Microsoft ticari Market 'teki özel teklifler.
ms.subservice: partnercenter-marketplace-publisher
ms.service: marketplace
ms.topic: article
author: vikrambmsft
ms.author: vikramb
ms.date: 07/01/2020
ms.openlocfilehash: 7e93772a77ac1edb88c8e1a56c8db216c6e9fbd3
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87420116"
---
# <a name="private-offers-in-the-microsoft-commercial-marketplace"></a>Microsoft ticari Market 'teki özel teklifler

Özel teklifler [Microsoft Azure Market](https://azuremarketplace.microsoft.com/) , yayımcıların yalnızca hedeflenen müşterilere görünen planlar oluşturmalarına olanak tanır. Bu makalede özel tekliflerin seçenekleri ve avantajları ele alınmaktadır.

## <a name="unlock-enterprise-deals-with-private-offers"></a>Özel tekliflerle kurumsal anlaşmalar üzerinde yararlanın

Kurumsal müşteriler, bulut çözümlerini bulmak, denemek ve satın almak için çevrimiçi pazar yerleri giderek daha fazla kullanır. Artık özel tekliflerle, yayımcılar, kuruluşların ihtiyaç duyduğu özellikleri karşılayan hedeflenen müşterilerle özelleştirilmiş çözümleri özel olarak paylaşmak için Market kullanabilir:

- *Anlaşmalı fiyatlandırma* , yayımcıların genel kullanıma sunulan tekliflerden indirimleri ve liste fiyatlarını genişletmesini sağlar.
- *Özel hüküm ve koşullar,* yayımcıların hüküm ve koşulları belirli bir müşteriye uyarlayabilmenizi sağlar.
- *Özelleştirilmiş konfigürasyonlar* , yayımcıların sanal makinelerini, Azure uygulamalarını ve SaaS uygulamalarını tek bir müşterinin ihtiyaçlarına uyarlamasına olanak sağlar. Bu seçenek, yayımcıların tüm müşterilere daha geniş bir şekilde başlatılmadan önce yeni ürün özelliklerine önizleme erişimi sağlamasına de olanak sağlar.

Özel teklifler, yayımcıların genel bir market 'in ölçek ve küresel kullanılabilirliğinden yararlanmasını sağlar ve özel anlaşmalar ve Konfigürasyonlar sağlamak için gereken esnekliği ve denetimi sağlar. Bu özellikler birlikte, bulut Pazar yerlerinin sağlam kurumsal benimseme kapağına yönelik kapıyı açar. Kuruluşlar artık bekledikleri ve talep ettikleri yollarla satın alabilir ve satılarlar.

Özel teklifler artık sanal makine, Azure uygulaması (çözüm şablonları veya yönetilen uygulamalar olarak uygulanır) ve SaaS uygulamaları tarafından sunulmaktadır.

<!--- Like public offers, private offers can be created and managed via the [Cloud Partner Portal](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md). Customers can be granted or revoked access to private offers in minutes.
--->

## <a name="creating-private-offers-using-plans"></a>Planlar kullanarak özel teklifler oluşturma

*Planlarla yeni veya mevcut teklifler*için, yayımcılar yeni planlar (eski adıyla SKU 'lar) oluşturarak ve bunları özel olarak işaretleyerek kolayca yeni ve özel Çeşitlemeler oluşturabilir.  

<!--- [Private SKUs](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) --->

Özel planlar bir teklifin bileşenleridir ve hedeflenen müşteriler tarafından yalnızca görünür ve satın alınabilir alınırken. Özel planlar, temel görüntüleri yeniden kullanabilir ve/veya zaten ortak bir plan için yayımlanmış meta verileri sunabilir. Bu seçenek, yayımcıların aynı temel görüntünün birden çok sürümünü yayınlamak ve meta verileri sunmak zorunda kalmadan ortak bir teklifin birden çok özel çeşitlemelerini oluşturmalarına olanak tanır. Yalnızca sanal makine ve Azure uygulaması için özel bir plan ortak bir plana sahip bir temel görüntüyü paylaşıyorsa, teklifin temel görüntüsündeki tüm değişiklikler, bu temel görüntüyü kullanarak tüm genel ve özel planlara yayılır.

*Yalnızca özel planlar içeren yeni teklifler*için yayımcılar, tekliflerini başka bir teklif olarak oluşturabilir ve sonra planları özel olarak işaretleyebilir. Yalnızca özel planları olan teklifler, Microsoft ticari marketi veya teklifle ilişkilendirilmemiş müşteriler tarafından [Azure Portal](https://azure.microsoft.com/features/azure-portal/) bulunabilir veya erişilebilir olmayacaktır.

>[!NOTE]
>Yalnızca özel planları içeren bir teklif, genel Azure Market veya AppSource 'ta görünmez.

## <a name="targeting-customers-with-private-offers"></a>Özel tekliflerle müşterileri hedefleme

Yayımcılar, hem yeni hem de mevcut özel teklifleriniz için abonelik tanımlayıcılarını kullanarak müşterileri hedefleyebilir. Bir sanal makine veya Azure Uygulama teklifi kullanan yayımcılar, özel bir planın kullanılabilirliğini tek bir Azure abonelik kimliğiyle kısıtlayabilir veya en fazla 20.000 Azure abonelik kimliği olan bir CSV 'yi karşıya yükleyebilir. SaaS uygulaması özel teklifi kullanılırken, yayımcılar, el ile veya CSV karşıya yükleme yaklaşımını kullanarak bir özel planın kullanılabilirliğini kısıtlamak için bir kiracı KIMLIĞINI ilişkilendirebilir.

Bir teklif sertifikalandırıldıktan ve yayımlandıktan sonra, müşteriler eşitleme özel abonelikleri özelliği kullanılarak dakika içinde plana göre güncelleştirilebilirler. Bu özellik, yayımcıların özel planı sertifikalamadan veya teklifi yayımlamadan sunulan müşterilerin listesini hızlı ve kolay bir şekilde güncelleştirmesine olanak sağlar.

## <a name="deploying-private-offers"></a>Özel teklifleri dağıtma

Özel Teklifler yalnızca [Azure Portal](https://azure.microsoft.com/features/azure-portal/) aracılığıyla bulunabilir ve [Microsoft AppSource](https://appsource.microsoft.com/) veya [Azure Marketi](https://azuremarketplace.microsoft.com)'nde gösterilmez. Farklı ticari Market stotörlerinin yayımlanması hakkında daha fazla bilgi edinmek için bkz. [Yayımlama seçeneğinizi belirleme](./determine-your-listing-type.md).

Müşteriler, Azure portal oturum açtıktan sonra özel tekliflerinden erişmek için Market gezinti öğesini seçebilir. Özel teklifler de arama sonuçlarında görünür ve diğer teklifler gibi komut satırı ve Azure Resource Manager şablonları aracılığıyla dağıtılabilir.

![[Özel teklifler]](./media/marketplace-publishers-guide/private-offer.png)

Özel teklifler, arama sonuçlarında da görünür. Yalnızca **özel** rozet 'yi arayın.

>[!Note]
>Özel teklifler, bulut çözümü sağlayıcısı (CSP) programının satıcısı aracılığıyla oluşturulan aboneliklerle desteklenmez.


<!---
## Next steps

To start using private offers, follow the steps in the [Private SKUs and Plans](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) guide.
--->
