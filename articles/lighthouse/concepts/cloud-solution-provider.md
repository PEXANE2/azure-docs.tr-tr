---
title: Azure açık Thouse ve bulut çözümü sağlayıcısı programı
description: Azure Temsilcili kaynak yönetimini kullanırken, güvenlik ve erişim denetimi göz önünde bulundurmanız önemlidir.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 08/22/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 399d2cb829c0425e3c9ee70a61cafde8568f903b
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70012098"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure açık Thouse ve bulut çözümü sağlayıcısı programı

Bir [CSP (bulut çözümü sağlayıcısı)](https://docs.microsoft.com/partner-center/csp-overview) iş ortağıysanız, Kullanıcı [adına yönetme (Aobo)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO) işlevini kullanarak, CSP programı aracılığıyla müşterileriniz için oluşturulan Azure aboneliklerine zaten erişebilirsiniz. Bu erişim, müşterilerinizin aboneliklerini doğrudan desteketmenize, yapılandırmanıza ve yönetmenize olanak sağlar.

Azure 'da açık olan Azure Kaynak yönetimini, AOININ yanı bir şekilde kullanabilirsiniz. Bu sayede, kullanıcılarınız için daha ayrıntılı izinler etkinleştirerek güvenliği artırmaya ve gereksiz erişimi azaltmanıza yardımcı olur. Ayrıca, kullanıcılarınızın kiracınızda tek bir oturum açma kullanarak birden çok müşteri aboneliği arasında çalışabilmesi için daha fazla verimlilik ve ölçeklenebilirlik sağlar.

## <a name="administer-on-behalf-of-aobo"></a>Adına yönetme (AOBO)

AOLE, kiracınızda [yönetici aracı](https://docs.microsoft.com/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) rolüne sahip herhangi bir Kullanıcı, CSP programı aracılığıyla oluşturduğunuz Azure aboneliklerine YÖNELIK olarak aocektir. Müşterilerin aboneliklerine erişmesi gereken tüm kullanıcılar bu grubun bir üyesi olmalıdır. AOIMIZ, farklı müşterilerle çalışan ayrı gruplar oluşturma veya gruplar veya kullanıcılar için farklı rolleri etkinleştirme esnekliği sağlar.

![AOI kullanarak Kiracı Yönetimi](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Azure tarafından atanan temsilcinin kaynak yönetimi

Azure Temsilcili kaynak yönetimini kullanarak, aşağıdaki diyagramda gösterildiği gibi farklı müşterilere veya rollere farklı gruplar atayabilirsiniz. Kullanıcılar, Azure tarafından atanan kaynak yönetimi aracılığıyla uygun düzeyde erişime sahip olacağı için, Yönetim Aracısı rolüne sahip kullanıcı sayısını azaltabilir (ve bu nedenle tam AOBO erişimi vardır). Bu, müşterilerinizin kaynaklarına gereksiz erişimi sınırlayarak güvenliği artırmaya yardımcı olur. Ayrıca, birden çok müşteriyi ölçekli olarak yönetme konusunda daha fazla esneklik sağlar.

CSP programı aracılığıyla oluşturduğunuz bir aboneliği [eklemek, Azure 'un Temsilcili kaynak yönetimine abonelik](../how-to/onboard-customer.md)ekleme bölümünde açıklanan adımları izler. Kiracınızda yönetici Aracısı rolüne sahip olan tüm kullanıcılar bu ekleme işlemini gerçekleştirebilir.

![AOI ve Azure tarafından atanan kaynak yönetimini kullanan Kiracı Yönetimi](../media/csp-2.jpg)

## <a name="next-steps"></a>Sonraki adımlar

- [Çapraz kiracı yönetim deneyimleri](cross-tenant-management-experience.md)hakkında bilgi edinin.
- [Azure 'un Temsilcili kaynak yönetimine nasıl bir abonelik](../how-to/onboard-customer.md)ekleneceğini öğrenin.
- [Bulut çözümü sağlayıcısı programı](https://docs.microsoft.com/partner-center/csp-overview)hakkında bilgi edinin.
