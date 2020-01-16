---
title: Azure portalında ödeme hesaplarınızı görüntüleme | Microsoft Docs
description: Azure portalında ödeme hesaplarınızın nasıl görüntüleneceğini öğrenin.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: ''
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 2768d6e146a37e86bb36353f661179ebd7b5033d
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75994135"
---
# <a name="billing-accounts-and-scopes-in-the-azure-portal"></a>Azure portal faturalama hesapları ve kapsamları

Azure’ı kullanmak için kaydolduğunuzda bir ödeme hesabı oluşturulur. Faturalarınızı, ödemelerinizi yönetmek ve maliyetleri izlemek için ödeme hesabınızı kullanırsınız. Birden çok ödeme hesabına erişiminiz olabilir. Örneğin, kişisel projeleriniz için Azure’a kaydolmuş olabilirsiniz. Ayrıca kuruluşunuzun Kurumsal Anlaşması veya Microsoft Müşteri Sözleşmesi aracılığıyla da erişiminiz olabilir. Bu senaryoların her biri için ayrı bir ödeme hesabınız olur.

Azure portal, aşağıdaki faturalandırma hesabı türlerini destekler:

- **Microsoft Online Services programı**: Azure için Azure Web sitesi üzerinden kaydolduğunuzda, Microsoft Online Services programı için bir faturalandırma hesabı oluşturulur. Örneğin, bir [Ücretsiz Azure Hesabına](https://azure.microsoft.com/offers/ms-azr-0044p/), [kullandıkça öde fiyatlarına tabi hesaba](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [Visual Studio abonesi](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) olarak kaydolduğunuzda.

- **Kurumsal Anlaşma**: Kuruluşunuz Azure kullanmak üzere bir [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) kaydolduğunda bir kurumsal anlaşma faturalandırma hesabı oluşturulur.

- **Microsoft Müşteri Sözleşmesi**: Microsoft Müşteri Sözleşmesi için bir Microsoft temsilcisiyle birlikte çalıştırıldığında, Microsoft Müşteri Sözleşmesi için bir faturalandırma hesabı oluşturulur. Select bölgelerindeki bazı müşteriler, [Kullandıkça Öde tarifesine](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [ücretsiz bir Azure hesabına](https://azure.microsoft.com/offers/ms-azr-0044p/) sahip bir hesap için Azure Web sitesi üzerinden kaydolan, Microsoft Müşteri Sözleşmesi için de bir faturalandırma hesabına sahip olabilir. Daha fazla bilgi için bkz. [Microsoft Müşteri Sözleşmesi için ödeme hesabınızı kullanmaya başlama](../understand/mca-overview.md).

- **Microsoft Iş ortağı sözleşmesi**: bulut çözümü sağlayıcısı (CSP) iş ortakları için, yeni ticaret deneyiminde müşterileri yönetmek üzere Microsoft Iş ortağı sözleşmesi için bir faturalandırma hesabı oluşturulur. İş ortaklarının, Azure portal faturalandırma hesabını yönetmek için bir [Azure planına](https://docs.microsoft.com/partner-center/purchase-azure-plan) sahip en az bir müşterisi olması gerekir. Daha fazla bilgi için bkz. [Microsoft Iş ortağı sözleşmesi için Faturalandırma hesabınızla çalışmaya başlama](../understand/mpa-overview.md).

Ödeme hesabınızın türünü belirlemek için bkz. [Ödeme hesabınızın türünü denetleme](#check-the-type-of-your-account).

## <a name="scopes-for-billing-accounts"></a>Ödeme hesapları için kapsamlar
Kapsam, faturalandırma hesabı içindeki, faturalandırmayı görüntülemek ve yönetmek için kullandığınız bir düğümdür. Bu, faturalandırma verilerini, ödemeleri, faturaları ve genel hesap yönetimini yönettiğinizde yer alabilir.

### <a name="microsoft-online-services-program"></a>Microsoft Online Services Programı

 ![MOSP hiyerarşisini gösteren ekran görüntüsü](./media/view-all-accounts/mosp-hierarchy.png)

|Kapsam  |Tanım  |
|---------|---------|
|Fatura hesabı     | Müşterinin Azure kullanımı için kabul ettiği bir sözleşmeyi temsil eder. Bir veya daha fazla abonelik içerir.  |
|Abonelik     |  Bir Azure kaynakları gruplamasını temsil eder. Fatura bu kapsamda oluşturulur. Ödeme yöntemleri ve kullanım adresi gibi diğer faturalandırma bilgileri bu kapsamla ilişkilendirilir.|

### <a name="enterprise-agreement"></a>Kurumsal Sözleşme

![EA hiyerarşisini gösteren ekran görüntüsü](./media/view-all-accounts/ea-hierarchy.png)

|Kapsam  |Tanım  |
|---------|---------|
|Fatura hesabı    | Bir Kurumsal Anlaşma kaydını temsil eder. Bir veya daha fazla departman ve hesap içerir. Fatura bu kapsamda oluşturulur. |
|Bölüm     |  Maliyetleri mantıksal gruplamalarda segmentlere ayırmak ve bütçe ayarlamak için isteğe bağlı hesap gruplandırması.     |
|Hesap     |  Tek bir hesap sahibini temsil eder. Hesap sahiplerinin, kayıt için faturalandırılan Azure abonelikleri oluşturma ve yönetme izinleri vardır. |

### <a name="microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi

![MCA hiyerarşisini gösteren ekran görüntüsü](./media/view-all-accounts/mca-hierarchy.png)

|Kapsam  |Görevler  |
|---------|---------|
|Fatura hesabı     |   Müşterinin Microsoft ürünlerini ve hizmetlerini kullanmak için kabul ettiği bir sözleşmeyi temsil eder. Bir veya daha fazla faturalandırma profili içerir. |
|Faturalama profili     |   Bir faturayı ve ödeme yöntemleri ve faturalandırma adresi gibi ilgili faturalandırma bilgilerini temsil eder. Bir veya daha fazla fatura bölümü içerir. |
|Fatura bölümü     |   Bir faturadaki maliyet gruplandırmasını temsil eder. Azure abonelikleri ve Azure Marketi ve uygulama kaynak ürünleri gibi diğer satın alma işlemleri bu kapsamla ilişkilendirilir.    |

### <a name="microsoft-partner-agreement"></a>Microsoft Iş ortağı sözleşmesi

![MPA hiyerarşisini gösteren ekran görüntüsü](./media/view-all-accounts/mpa-hierarchy.png)

|Kapsam  |Görevler  |
|---------|---------|
|Fatura hesabı     |   Yeni ticaret deneyiminde müşterilerin Microsoft ürünlerini ve hizmetlerini yönetmek için bir iş ortağı anlaşmasını temsil eder. Bir veya daha fazla faturalandırma profili ve müşterisi içerir.   |
|Faturalama profili     |   Para birimi için bir faturayı temsil eder.     |
|Müşteri    |   Bir bulut çözümü sağlayıcısı (CSP) iş ortağı için bir müşteriyi temsil eder.  Azure abonelikleri ve Azure Marketi ve uygulama kaynak ürünleri gibi diğer satın alma işlemleri bu kapsamla ilişkilendirilir.  |
|Kurumsal bayi    |   Müşteriye hizmet sağlayan satıcı. Bu, abonelik için isteğe bağlı bir alandır ve yalnızca CSP iki katmanlı modeldeki dolaylı sağlayıcılar için geçerlidir.     |

## <a name="switch-billing-scope-in-the-azure-portal"></a>Azure portalında faturalama kapsamını değiştirme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Azure portalı aramasını gösteren ekran görüntüsü](./media/view-all-accounts/billing-search-cost-management-billing.png)

3. Genel Bakış sayfasında **Kapsam Değiştir**' i seçin.

   ![Faturalama kapsamlarını gösteren ekran görüntüsü](./media/view-all-accounts/overview-select-scopes.png)

   > [!Note]
    >
    > Yalnızca bir kapsama erişiminiz varsa, anahtar kapsamını görmezsiniz.

4. Ayrıntıları görüntülemek için bir kapsam seçin.

   ![Faturalama kapsamlarını gösteren ekran görüntüsü](./media/view-all-accounts/list-of-scopes.png)

## <a name="check-the-type-of-your-account"></a>Hesabınızın türünü denetleyin
[!INCLUDE [billing-check-mca](../../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Maliyetlerinizi analiz etmeye](../costs/quick-acm-cost-analysis.md) nasıl başlayacağınızı öğrenin.
