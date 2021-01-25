---
title: Azure Güvenlik Merkezi için yerleşik ilke tanımları
description: Azure Güvenlik Merkezi için Azure Ilkesi yerleşik ilke tanımlarını listeler. Bu yerleşik ilke tanımları, Azure kaynaklarınızı yönetmek için yaygın yaklaşımlar sağlar.
ms.date: 01/24/2021
ms.topic: reference
author: memildin
ms.author: memildin
ms.service: security-center
ms.custom: subject-policy-reference
ms.openlocfilehash: 1192e765dc3cc001f8d1a1c6a28fe1a5cf4c9ecc
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98757280"
---
# <a name="azure-policy-built-in-definitions-for-azure-security-center"></a>Azure Güvenlik Merkezi için Azure İlkesi yerleşik tanımları

Bu sayfa, Azure Güvenlik Merkezi ile ilgili [Azure ilkesi](../governance/policy/overview.md) yerleşik ilke tanımlarının bir dizinidir. Aşağıdaki ilke tanımlarının gruplandırmaları kullanılabilir:

- [Girişimler](#azure-security-center-initiatives) grubu, ' Güvenlik Merkezi ' kategorisindeki Azure ilke girişim tanımlarını listeler.
- [Varsayılan girişim](#azure-security-center-initiatives) grubu, güvenlik merkezi 'nin varsayılan girişim, [Azure Güvenlik kıyaslamalarından](../security/benchmarks/introduction.md)oluşan tüm Azure ilke tanımlarını listeler. Bu Microsoft tarafından yazılmış, yaygın olarak kullanılan kıyaslanmalar, [Internet güvenliği (CIS) Için merkezden](https://www.cisecurity.org/benchmark/azure/) ve [ulusal standartlar ve teknolojı Enstitüsü (NIST)](https://www.nist.gov/) ile bulut merkezli güvenliğe odaklanarak yapılar oluşturur.
- [Kategori](#azure-security-center-category) grubu ' Güvenlik Merkezi ' kategorisindeki tüm Azure ilke tanımlarını listeler.

Güvenlik ilkeleri hakkında daha fazla bilgi için bkz. [güvenlik Ilkeleriyle çalışma](./tutorial-security-policy.md). Diğer hizmetlere yönelik diğer Azure Ilkesi için yerleşik olarak [bulunan Azure ilkesi yerleşik tanımları](../governance/policy/samples/built-in-policies.md)bölümüne bakın.

Her yerleşik ilke tanımının adı Azure portal ilke tanımına bağlanır. Kaynağı [Azure Policy GitHub](https://github.com/Azure/azure-policy)deposunda görüntülemek için **Sürüm** sütunundaki bağlantıyı kullanın.

## <a name="azure-security-center-initiatives"></a>Azure Güvenlik Merkezi girişimleri

Güvenlik Merkezi tarafından izlenen yerleşik girişimler hakkında bilgi edinmek için aşağıdaki tabloya bakın:

[!INCLUDE [azure-policy-reference-policyset-security-center](../../includes/policy/reference/bycat/policysets-security-center.md)]

## <a name="azure-security-center-default-initiative"></a>Azure Güvenlik Merkezi varsayılan girişimi

Güvenlik Merkezi tarafından izlenen yerleşik ilkeler hakkında bilgi edinmek için aşağıdaki tabloya bakın:

[!INCLUDE [azure-policy-reference-init-asc](../../includes/policy/reference/custom/init-asc.md)]

## <a name="azure-security-center-category"></a>Azure Güvenlik Merkezi kategorisi

[!INCLUDE [azure-policy-reference-category-securitycenter](../../includes/policy/reference/bycat/policies-security-center.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, güvenlik merkezi 'nde Azure Ilkesi güvenlik ilkesi tanımları hakkında bilgi edindiniz. Daha fazla bilgi için aşağıdaki makalelere bakın.

- Yerleşik ilkeleri görmek için [Azure İlkesi GitHub deposuna](https://github.com/Azure/azure-policy) gidin.
- [Azure İlkesi tanımı yapısını](../governance/policy/concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../governance/policy/concepts/effects.md) konusunu gözden geçirin.
- [Azure Güvenlik Merkezi planlama ve işlemler Kılavuzu](./security-center-planning-and-operations-guide.md): Azure Güvenlik Merkezi 'nde tasarım konularını planlama ve anlama hakkında bilgi edinin.
- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](./security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](./security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve ele alma hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](./security-center-partner-integration.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure ilkesi](../governance/policy/overview.md): Azure kaynaklarınızı denetleme ve yönetme hakkında bilgi edinin.
