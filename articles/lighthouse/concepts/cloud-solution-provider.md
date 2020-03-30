---
title: Bulut Çözüm Sağlayıcısı programı hakkında önemli noktalar
description: Azure yetkin kaynak yönetimi, CSP iş ortakları için parçalı izinleri etkinleştirerek güvenliği ve denetimi geliştirmeye yardımcı olur.
ms.date: 12/18/2019
ms.topic: conceptual
ms.openlocfilehash: 66ea74751f12a499a1e2d9e083497da31746e3c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75456919"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure Deniz Feneri ve Bulut Çözüm Sağlayıcısı programı

[Bir CSP (Bulut Çözüm Sağlayıcısı)](https://docs.microsoft.com/partner-center/csp-overview) iş ortağıysanız, CSP programı aracılığıyla müşterileriniz için oluşturulan Azure aboneliklerine ,Adına [Yönet (AOBO)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO) işlevini kullanarak zaten erişebilirsiniz. Bu erişim, müşterilerinizin aboneliklerini doğrudan desteklemenize, yapılandırmanıza ve yönetmenize olanak tanır.

[Azure Deniz Feneri](../overview.md)ile AOBO ile birlikte Azure temsilci kaynak yönetimini kullanabilirsiniz. Bu, güvenliğini artırmaya yardımcı olur ve kullanıcılarınız için daha ayrıntılı izinler sağlayarak gereksiz erişimi azaltır. Ayrıca, kullanıcılarınız kiracınızda tek bir giriş kullanarak birden fazla müşteri aboneliğinde çalışabildiği için daha fazla verimlilik ve ölçeklenebilirlik sağlar.

> [!TIP]
> Müşteri kaynaklarının korunmasına yardımcı olmak için, [iş ortağı güvenlik gereksinimleriyle](https://docs.microsoft.com/partner-center/partner-security-requirements)birlikte önerilen güvenlik [uygulamalarımızı](recommended-security-practices.md) gözden geçirip takip edin.

## <a name="administer-on-behalf-of-aobo"></a>(AOBO) Adına Yönet

AOBO ile, kiracınızda [Yönetici Aracısı](https://docs.microsoft.com/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) rolü ne olursa olsun, CSP programı aracılığıyla oluşturduğunuz Azure aboneliklerine AOBO erişimi ne olur. Herhangi bir müşterinin aboneliğine erişmesi gereken tüm kullanıcılar bu grubun üyesi olmalıdır. AOBO, farklı müşterilerle çalışan farklı gruplar oluşturma veya gruplar veya kullanıcılar için farklı roller etkinleştirme esnekliğine izin vermez.

![AOBO kullanarak kiracı yönetimi](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Azure tarafından atanan temsilcinin kaynak yönetimi

Azure temsilci kaynak yönetimini kullanarak, aşağıdaki diyagramda gösterildiği gibi farklı müşterilere veya rollere farklı gruplar atayabilirsiniz. Kullanıcılar Azure temsilcikaynak yönetimi aracılığıyla uygun erişim düzeyine sahip olacaklarından, Yönetici Aracısı rolüne sahip kullanıcı sayısını azaltabilirsiniz (ve böylece tam AOBO erişimine sahip olabilirsiniz). Bu, müşterilerinizin kaynaklarına gereksiz erişimi sınırlayarak güvenliği artırmaya yardımcı olur. Ayrıca, birden fazla müşteriyi ölçekte yönetmek için daha fazla esneklik sağlar.

CSP programı aracılığıyla oluşturduğunuz bir aboneye binme, [Onboard'da Azure temsilcikaynak yönetimine abone](../how-to/onboard-customer.md)olunan adımları izler. Kiracınızda Yönetici Aracısı rolü olan herhangi bir kullanıcı bunu onboarding olarak gerçekleştirebilir.

![AOBO ve Azure temsilci kaynak yönetimini kullanarak kiracı yönetimi](../media/csp-2.jpg)

> [!NOTE]
> [Azure portalındaki **müşterilerim** sayfasında,](../how-to/view-manage-customers.md) Microsoft Müşteri Sözleşmesi 'ni [(MCA) imzalayan](https://docs.microsoft.com/partner-center/confirm-customer-agreement) ve Azure planı [altında](https://docs.microsoft.com/partner-center/azure-plan-get-started)olan CSP müşterileri için fatura bilgilerini ve kaynaklarını görüntüleyen bir Bulut Çözüm **Sağlayıcısı (Önizleme)** bölümü bulunmaktadır. Daha fazla bilgi için Microsoft [İş Ortağı Sözleşmesi faturalandırma hesabınızla başlayın.](../../billing/mpa-overview.md)
>
> CSP müşterileri, Azure temsilci kaynak yönetimi için de binmiş olsun veya olmasın bu bölümde görünebilir. Varsa, Görünüm'de açıklandığı gibi **Müşteriler** bölümünde de görünürler [ve müşterileri ve devredilen kaynakları yönetirler.](../how-to/view-manage-customers.md) Benzer şekilde, Azure temsilci kaynak yönetimi için bu müşterilere binmeniz için bir CSP müşterisinin **müşterilerimin** **Bulut Çözüm Sağlayıcısı (Önizleme)** bölümünde görünmesi gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

- Kiracılar [arası yönetim deneyimleri](cross-tenant-management-experience.md)hakkında bilgi edinin.
- [Azure temsilcikaynak yönetimine abone](../how-to/onboard-customer.md)olmayı öğrenin.
- [Bulut Çözüm Sağlayıcısı programı](https://docs.microsoft.com/partner-center/csp-overview)hakkında bilgi edinin.
