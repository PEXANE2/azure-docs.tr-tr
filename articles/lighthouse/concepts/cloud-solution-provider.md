---
title: Bulut Çözüm Sağlayıcısı programı hakkında önemli noktalar
description: CSP iş ortakları için, Azure tarafından atanan kaynak yönetimi, ayrıntılı izinleri etkinleştirerek güvenliğin ve denetimin artırılmasına yardımcı olur.
ms.date: 12/18/2019
ms.topic: conceptual
ms.openlocfilehash: 66ea74751f12a499a1e2d9e083497da31746e3c7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75456919"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure açık Thouse ve bulut çözümü sağlayıcısı programı

Bir [CSP (bulut çözümü sağlayıcısı)](https://docs.microsoft.com/partner-center/csp-overview) iş ortağıysanız, Kullanıcı [adına yönetme (Aobo)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO) işlevini kullanarak, CSP programı aracılığıyla müşterileriniz için oluşturulan Azure aboneliklerine zaten erişebilirsiniz. Bu erişim, müşterilerinizin aboneliklerini doğrudan desteketmenize, yapılandırmanıza ve yönetmenize olanak sağlar.

[Azure](../overview.md)'da açık olan Azure Kaynak YÖNETIMINI, Aoinin yanı bir şekilde kullanabilirsiniz. Bu sayede, kullanıcılarınız için daha ayrıntılı izinler etkinleştirerek güvenliği artırmaya ve gereksiz erişimi azaltmanıza yardımcı olur. Ayrıca, kullanıcılarınızın kiracınızda tek bir oturum açma kullanarak birden çok müşteri aboneliği arasında çalışabilmesi için daha fazla verimlilik ve ölçeklenebilirlik sağlar.

> [!TIP]
> Müşteri kaynaklarının korunmasına yardımcı olmak için, [Önerilen güvenlik uygulamalarınızı](recommended-security-practices.md) , [iş ortağı güvenlik gereksinimleriyle](https://docs.microsoft.com/partner-center/partner-security-requirements)birlikte gözden geçirip izlediğinizden emin olun.

## <a name="administer-on-behalf-of-aobo"></a>Adına yönetme (AOBO)

AOLE, kiracınızda [yönetici aracı](https://docs.microsoft.com/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) rolüne sahip herhangi bir Kullanıcı, CSP programı aracılığıyla oluşturduğunuz Azure aboneliklerine YÖNELIK olarak aocektir. Müşterilerin aboneliklerine erişmesi gereken tüm kullanıcılar bu grubun bir üyesi olmalıdır. AOIMIZ, farklı müşterilerle çalışan ayrı gruplar oluşturma veya gruplar veya kullanıcılar için farklı rolleri etkinleştirme esnekliği sağlar.

![AOI kullanarak Kiracı Yönetimi](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Azure tarafından atanan temsilcinin kaynak yönetimi

Azure Temsilcili kaynak yönetimini kullanarak, aşağıdaki diyagramda gösterildiği gibi farklı müşterilere veya rollere farklı gruplar atayabilirsiniz. Kullanıcılar, Azure tarafından atanan kaynak yönetimi aracılığıyla uygun düzeyde erişime sahip olacağı için, Yönetim Aracısı rolüne sahip kullanıcı sayısını azaltabilir (ve bu nedenle tam AOBO erişimi vardır). Bu, müşterilerinizin kaynaklarına gereksiz erişimi sınırlayarak güvenliği artırmaya yardımcı olur. Ayrıca, birden çok müşteriyi ölçekli olarak yönetme konusunda daha fazla esneklik sağlar.

CSP programı aracılığıyla oluşturduğunuz bir aboneliği [eklemek, Azure 'un Temsilcili kaynak yönetimine abonelik](../how-to/onboard-customer.md)ekleme bölümünde açıklanan adımları izler. Kiracınızda yönetici Aracısı rolüne sahip olan tüm kullanıcılar bu ekleme işlemini gerçekleştirebilir.

![AOI ve Azure tarafından atanan kaynak yönetimini kullanan Kiracı Yönetimi](../media/csp-2.jpg)

> [!NOTE]
> Azure portal [ **müşterilerimiz** sayfasında](../how-to/view-manage-customers.md) , [MICROSOFT Müşteri Sözleşmesi 'ni (MCA) imzalayan](https://docs.microsoft.com/partner-center/confirm-customer-agreement) ve [Azure planı kapsamında](https://docs.microsoft.com/partner-center/azure-plan-get-started)olan CSP müşterilerinin fatura bilgilerini ve kaynaklarını görüntüleyen bir **bulut çözümü sağlayıcısı (Önizleme)** bölümü vardır. Daha fazla bilgi için bkz. [Microsoft Iş ortağı sözleşmesi Faturalandırma hesabınızı kullanmaya başlama](../../billing/mpa-overview.md).
>
> CSP müşterileri, Azure tarafından yetkilendirilen kaynak yönetimi için de eklendi olup olmadığı konusunda bu bölümde görünebilir. Varsa [müşterileri ve temsilcileri yönetme](../how-to/view-manage-customers.md)bölümünde açıklandığı gibi **müşteriler** bölümünde da görünürler. Benzer şekilde, bir CSP müşterisi, Azure tarafından atanan kaynak yönetimine yönelik olarak bunları eklemek için **müşterilerimin** **bulut çözümü sağlayıcısı (Önizleme)** bölümünde görünmesini gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar

- [Çapraz kiracı yönetim deneyimleri](cross-tenant-management-experience.md)hakkında bilgi edinin.
- [Azure 'un Temsilcili kaynak yönetimine nasıl bir abonelik](../how-to/onboard-customer.md)ekleneceğini öğrenin.
- [Bulut çözümü sağlayıcısı programı](https://docs.microsoft.com/partner-center/csp-overview)hakkında bilgi edinin.
