---
title: Azure portalında ödeme hesaplarınızı görüntüleme
description: Azure portalında ödeme hesaplarınızın nasıl görüntüleneceğini öğrenin. Kuruluş, Microsoft Müşteri ve Microsoft İş Ortağı Sözleşmeleri için kapsam bilgilerine bakın.
author: amberbhargava
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: b18e08f93df143f9844fe774eee59452978d7437
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88686403"
---
# <a name="billing-accounts-and-scopes-in-the-azure-portal"></a>Azure portalındaki faturalama hesapları ve kapsamlar

Azure’ı kullanmak için kaydolduğunuzda bir ödeme hesabı oluşturulur. Faturalarınızı, ödemelerinizi yönetmek ve maliyetleri izlemek için ödeme hesabınızı kullanırsınız. Birden çok ödeme hesabına erişiminiz olabilir. Örneğin, kişisel projeleriniz için Azure’a kaydolmuş olabilirsiniz. Ayrıca kuruluşunuzun Kurumsal Anlaşması veya Microsoft Müşteri Sözleşmesi aracılığıyla da erişiminiz olabilir. Bu senaryoların her biri için ayrı bir ödeme hesabınız olur.

Azure portal aşağıdaki ödeme hesapları türünü destekler:

- **Microsoft Online Services Programı**: Azure web sitesi üzerinden Azure’a kaydolduğunuzda, Microsoft Online Services Programı için bir ödeme hesabı oluşturulur. Örneğin, bir [Ücretsiz Azure Hesabına](https://azure.microsoft.com/offers/ms-azr-0044p/), [kullandıkça öde fiyatlarına tabi hesaba](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [Visual Studio abonesi](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) olarak kaydolduğunuzda.

- **Kurumsal Anlaşma**: Kuruluşunuz Azure’ı kullanmak için bir [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) imzaladığında Kurumsal Anlaşma için bir ödeme hesabı oluşturulur. Kurumsal Anlaşma kapsamında en fazla 2000 aboneliğiniz olabilir.

- **Microsoft Müşteri Sözleşmesi**: Kuruluşunuz bir Microsoft Müşteri Sözleşmesi imzalamak için Microsoft temsilcisiyle çalıştığında Microsoft Müşteri Sözleşmesi için bir ödeme hesabı oluşturulur. Azure web sitesinden [kullandıkça öde fiyatlarına tabi hesaba](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [Ücretsiz Azure Hesabına](https://azure.microsoft.com/offers/ms-azr-0044p/) kaydolan belirli bölgelerdeki bazı müşterilerin Microsoft Müşteri Sözleşmesi için ödeme hesabı da olabilir. Microsoft Müşteri Sözleşmesi kapsamında en fazla 20 aboneliğiniz olabilir. Daha fazla bilgi için bkz. [Microsoft Müşteri Sözleşmesi için ödeme hesabınızı kullanmaya başlama](../understand/mca-overview.md).

- **Microsoft İş Ortağı Sözleşmesi**: Müşterilerini yeni ticaret deneyiminde yönetmelerine olanak tanımak için Bulut Çözümü Sağlayıcısı (CSP) iş ortakları için bir Microsoft İş Ortağı Sözleşmesi’ne yönelik faturalama hesabı oluşturulur. Azure portalındaki faturalama hesabını yönetmek için iş ortaklarının en az bir müşterisi olan bir [Azure planına](https://docs.microsoft.com/partner-center/purchase-azure-plan) sahip olması gerekir. Daha fazla bilgi için bkz. [Microsoft İş Ortağı Sözleşmesi için ödeme hesabınızı kullanmaya başlama](../understand/mpa-overview.md).

Ödeme hesabınızın türünü belirlemek için bkz. [Ödeme hesabınızın türünü denetleme](#check-the-type-of-your-account).

## <a name="scopes-for-billing-accounts"></a>Ödeme hesapları için kapsamlar
Kapsam, faturalamayı görüntülemek ve yönetmek için kullandığınız ödeme hesabı içindeki bir düğümdür. Bu kapsam dahilinde faturalama verilerini, ödemeleri, faturaları yönetir ve genel hesap yönetimi gerçekleştirirsiniz.

### <a name="microsoft-online-services-program"></a>Microsoft Online Services Programı

![MOSP hiyerarşisini gösteren ekran görüntüsü](./media/view-all-accounts/mosp-hierarchy.png)

|Kapsam  |Tanım  |
|---------|---------|
|Fatura hesabı     | Bir müşterinin Azure kullanmayı kabul ettiğini beyan eden bir anlaşmayı temsil eder. Bir veya birden fazla abonelik içerir.  |
|Abonelik     |  Bir Azure kaynakları gruplamasını temsil eder. Fatura bu kapsamda oluşturulur. Ödeme yöntemleri ve kullanım adresi gibi diğer faturalama bilgileri bu kapsamla ilişkilendirilir.|

### <a name="enterprise-agreement"></a>Kurumsal Anlaşma

![EA hiyerarşisini gösteren ekran görüntüsü](./media/view-all-accounts/ea-hierarchy.png)

|Kapsam  |Tanım  |
|---------|---------|
|Fatura hesabı    | Bir Kurumsal Anlaşma kaydını temsil eder. Bir veya daha fazla departmanı ve hesabı içerir. Fatura bu kapsamda oluşturulur. |
|Bölüm     |  Maliyetleri mantıksal gruplara ayırmak ve bütçeyi belirlemeye yönelik isteğe bağlı hesap gruplandırması.     |
|Hesap     |  Tek bir hesap sahibini temsil eder. Hesap sahipleri, kayda faturalandırılan Azure abonelikleri oluşturup bunları yönetme izinlerine sahiptir. |

### <a name="microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi

![MCA hiyerarşisini gösteren ekran görüntüsü](./media/view-all-accounts/mca-hierarchy.png)

|Kapsam  |Görevler  |
|---------|---------|
|Fatura hesabı     |   Bir müşterinin Microsoft ürünlerini ve hizmetlerini kullanmayı kabul ettiğini beyan eden bir anlaşmayı temsil eder. Bir veya daha fazla faturalama profilini içerir. |
|Faturalama profili     |   Bir faturayı ve ödeme yöntemleri ve fatura adresi gibi ilgili ödeme bilgilerini temsil eder. Bir veya daha fazla fatura bölümü içerir. |
|Fatura bölümü     |   Faturadaki bir maliyet grubunu temsil eder. Azure abonelikleri ve Azure Market ve Uygulama kaynağı ürünleri gibi diğer satın alma işlemleri bu kapsamla ilişkilendirilir.    |

### <a name="microsoft-partner-agreement"></a>Microsoft İş Ortağı Sözleşmesi

![MPA hiyerarşisini gösteren ekran görüntüsü](./media/view-all-accounts/mpa-hierarchy.png)

|Kapsam  |Görevler  |
|---------|---------|
|Fatura hesabı     |   Müşterilerin Microsoft ürünlerini ve hizmetlerini yeni ticaret deneyiminde yönetmesine olanak tanıyan bir iş ortağı anlaşmasını temsil eder. Bir veya daha fazla faturalama profilini ve müşteriyi içerir.   |
|Faturalama profili     |   Bir para birimi için faturayı temsil eder.     |
|Müşteri    |   Bir Bulut Çözümü Sağlayıcısı (CSP) iş ortağının müşterisini temsil eder.  Azure abonelikleri ve Azure Market ve Uygulama kaynağı ürünleri gibi diğer satın alma işlemleri bu kapsamla ilişkilendirilir.  |
|Kurumsal bayi    |   Müşteriye hizmet sağlayan kurumsal bayi. Bu, abonelik için isteğe bağlı bir alandır ve yalnızca CSP iki katmanlı modeldeki Dolaylı sağlayıcılar için geçerlidir.     |

## <a name="switch-billing-scope-in-the-azure-portal"></a>Azure portalında faturalama kapsamını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Azure portalı aramasını gösteren ekran görüntüsü](./media/view-all-accounts/billing-search-cost-management-billing.png)

3. Genel bakış sayfasında **Kapsamı değiştir** seçeneğini belirleyin.

   ![Faturalama kapsamlarını gösteren ekran görüntüsü](./media/view-all-accounts/overview-select-scopes.png)

   > [!Note]
    >
    > Yalnızca bir kapsama erişiminiz varsa Kapsamı değiştir seçeneğini görmezsiniz.

4. Ayrıntıları görüntülemek için bir kapsam seçin.

   ![Faturalama kapsamlarını gösteren ekran görüntüsü](./media/view-all-accounts/list-of-scopes.png)

## <a name="check-the-type-of-your-account"></a>Hesabınızın türünü denetleme
[!INCLUDE [billing-check-mca](../../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Maliyetlerinizi analiz etmeye](../costs/quick-acm-cost-analysis.md) nasıl başlayacağınızı öğrenin.
