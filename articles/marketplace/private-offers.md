---
title: Microsoft ticari Market 'teki özel teklifler
description: Uygulama ve hizmet yayımcıları için Microsoft ticari Market 'teki özel teklifler.
ms.subservice: partnercenter-marketplace-publisher
ms.service: marketplace
ms.topic: article
author: vikrambmsft
ms.author: vikramb
ms.date: 02/22/2021
ms.openlocfilehash: 7c02c41379cfb886fd7e6f84486d815d3cdbfb5b
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097185"
---
# <a name="private-offers-in-the-microsoft-commercial-marketplace"></a>Microsoft ticari Market 'teki özel teklifler

Özel teklifler de denilen özel tekliflerdir, yayımcıların yalnızca hedeflenen müşterilere görünen planlar oluşturmalarına olanak tanır. Bu makalede özel tekliflerin seçenekleri ve avantajları ele alınmaktadır.

## <a name="unlock-enterprise-deals-with-private-offers"></a>Özel tekliflerle kurumsal anlaşmalar üzerinde yararlanın

Yayımcılar, özel teklifler oluşturarak hedeflenen müşterilere kuruluşların ihtiyaç duyduğu yeteneklere özel olarak özelleştirilmiş çözümler sunabilir:

- *Anlaşmalı fiyatlandırma* , yayımcıların genel kullanıma sunulan tekliflerden indirimleri ve liste fiyatlarını genişletmesini sağlar.
- *Özel hüküm ve koşullar,* yayımcıların hüküm ve koşulları belirli bir müşteriye uyarlayabilmenizi sağlar.
- *Özelleştirilmiş konfigürasyonlar* , yayımcıların sanal makinelerini, Azure uygulamalarını ve hizmet olarak yazılım (SaaS), bireysel bir müşterinin ihtiyaçlarına uyarmasına olanak tanır. Bu seçenek, yayımcıların yeni ürün özelliklerine, tüm müşterilere başlatılmadan önce önizleme erişimi sağlamasına de olanak sağlar.

Özel teklifler, yayımcıların özel anlaşmalar ve Konfigürasyonlar sunmak ve teslim etmek için gereken esneklik ve denetim ile genel bir market 'in ölçeklendirilmesi ve küresel kullanılabilirliğinden faydalanmasını sağlar. Kuruluşlar artık bekledikleri yollarla satın alabilir ve satılarlar.

## <a name="create-private-offers-using-plans"></a>Planlar kullanarak özel teklifler oluşturma

*Planlarla yeni veya mevcut teklifler* için, yayımcılar yeni planlar (eski adıyla SKU 'lar) oluşturarak ve bunları özel olarak işaretleyerek kolayca yeni ve özel Çeşitlemeler oluşturabilir. Her teklifin en fazla 45 özel planı olabilir.

<!--- [Private SKUs]() --->

Özel planlar aşağıdaki teklif türleri için kullanılabilir:

- Azure Sanal Makinesi
- Azure uygulaması (çözüm şablonları veya yönetilen uygulamalar olarak uygulanır)
- Yönetilen hizmet
- SaaS teklifleri

Özel planlar bir teklifin bileşenleridir ve hedeflenen müşteriler tarafından yalnızca görünür ve satın alınabilir alınırken. Özel planlar yalnızca hedeflenen müşteriler tarafından görülebilir ve satın alınabilir alınırken. Özel planlar, Azure genel ve Azure Kamu 'daki müşterilerin kullanımına sunulabilir.

Özel planlar, temel görüntüleri yeniden kullanabilir ve/veya zaten ortak bir plan için yayımlanmış meta verileri sunabilir. Bu seçenek, yayımcıların aynı temel görüntünün birden çok sürümünü yayınlamak ve meta verileri sunmak zorunda kalmadan ortak bir teklifin birden çok özel çeşitlemelerini oluşturmalarına olanak tanır. Yalnızca Azure sanal makinesi ve Azure uygulaması için özel bir plan ortak bir plana sahip bir temel görüntüyü paylaşıyorsa, teklifin temel görüntüsündeki tüm değişiklikler, bu temel görüntüyü kullanarak tüm genel ve özel planlara yayılır.

*Yalnızca özel planlar içeren yeni teklifler* için yayımcılar, tekliflerini başka bir teklif olarak oluşturabilir ve sonra planları özel olarak işaretleyebilir. Yalnızca özel planları olan teklifler, teklifle ilişkilendirilmemiş müşteriler tarafından [Azure Portal](https://azure.microsoft.com/features/azure-portal/) keşfedilemez veya erişilebilir olmayacaktır.

>[!NOTE]
>Yalnızca özel planları içeren bir teklif, genel Azure Market veya AppSource 'ta görünmez.

## <a name="target-customers-with-private-offers"></a>Özel teklifleri olan müşterileri hedefleyin

Yayımcılar, hem yeni hem de mevcut özel teklifleriniz için abonelik tanımlayıcılarını kullanarak müşterileri hedefleyebilir. Azure sanal makinesi, Azure uygulaması ve yönetilen hizmet teklifleri için, yayımcılar özel bir planın kullanılabilirliğini tek bir Azure abonelik kimliğiyle kısıtlayabilir veya en fazla 10.000 Azure abonelik kimliği olan bir CSV 'yi karşıya yükleyebilir. SaaS teklifleri için yayımcılar, el ile veya CSV yükleme yaklaşımını kullanarak bir özel planın kullanılabilirliğini kısıtlamak üzere bir Azure Active Directory kiracı KIMLIĞINI ilişkilendirebilirler.

Bir teklif sertifikalandırıldıktan ve yayımlandıktan sonra, özel abonelikleri Eşitle özelliği kullanılarak müşteriler plana göre güncelleştirilebilirler veya kaldırılabilir. Bu özellik, yayımcıların özel planı sertifikalamadan veya teklifi yayımlamadan sunulan müşterilerin listesini hızlı ve kolay bir şekilde güncelleştirmesine olanak sağlar.

## <a name="deploying-private-offers"></a>Özel teklifleri dağıtma

Azure portal oturum açtıktan sonra müşteriler özel tekliflerinizi seçmek için bu adımları takip edebilir.

1. [Azure Portal](https://ms.portal.azure.com/)oturum açın.
1. **Azure hizmetleri** altında **kaynak oluştur**' u seçin.
1. **Yeni** sayfada, **Azure Marketi**' nin yanında **Tümünü gör**' ü seçin. Market sayfası görüntülenir.
1. Sol gezinti bölmesinde **özel teklifler**' i seçin.

> [!NOTE]
> Özel Teklifler yalnızca [Azure Portal](https://azure.microsoft.com/features/azure-portal/)bulunabilir. [Microsoft AppSource](https://appsource.microsoft.com/) veya [Azure Marketi](https://azuremarketplace.microsoft.com)' nde gösterilmemektedir. Farklı ticari Market çevrimiçi mağazalarında yayımlama hakkında daha fazla bilgi edinmek için bkz. [listeye giriş seçenekleri](./determine-your-listing-type.md).

Özel teklifler de arama sonuçlarında görünür ve diğer tüm teklifler gibi komut satırı ve Azure Resource Manager şablonları aracılığıyla dağıtılabilir.

[![[Arama sonuçlarında görünen özel teklifler.]](media/marketplace-publishers-guide/private-offer.png)](media/marketplace-publishers-guide/private-offer.png#lightbox)

Özel teklifler, arama sonuçlarında da görünür. Yalnızca **özel** rozet 'yi arayın.

>[!Note]
>Özel teklifler, bulut çözümü sağlayıcısı (CSP) programının satıcısı aracılığıyla oluşturulan aboneliklerle desteklenmez.

<!---
## Next steps

To start using private offers, follow the steps in the [Private SKUs and Plans]() guide.
--->
