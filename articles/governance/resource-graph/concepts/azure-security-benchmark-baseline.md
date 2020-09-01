---
title: Azure Güvenlik kıyaslaması için Azure Kaynak Grafiği güvenlik temeli
description: Azure Kaynak Grafiği güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: resource-graph
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: a85fdf1e1e27b1ce5abb0c7890b717d48ba3bd3f
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89230458"
---
# <a name="azure-resource-graph-security-baseline-for-azure-security-benchmark"></a>Azure Güvenlik kıyaslaması için Azure Kaynak Grafiği güvenlik temeli

Bu güvenlik temeli [Azure Güvenlik kıyaslayıcılarından](../../../security/benchmarks/overview.md) Azure Kaynak grafiğine kılavuzluk uygular. Azure Güvenlik kıyaslaması, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve Azure Kaynak grafı için geçerli olan ilgili kılavuza göre gruplandırılır. Azure Kaynak Grafiği için geçerli olmayan **denetimler** dışlandı. Azure Kaynak grafiğinin Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure sanal ağ güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.



## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](../../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Kılavuz**: Azure Kaynak Grafiği, Azure rol tabanlı erişim denetimi (Azure RBAC) temelinde kaynak türlerine ve özelliklere erişim sağlar. Sorguların uygun kaynaklar için sonuçlar döndürmesini sağlamak üzere güvenlik ilkelerine (kullanıcılar, gruplar ve hizmet hesapları) verilen erişimi denetleyin ve gözden geçirin.

* [Azure Kaynak Grafiği’nde izinler](../overview.md#permissions-in-azure-resource-graph)

* [Azure kimlik erişimi Incelemelerini kullanma](../../../active-directory/governance/access-reviews-overview.md)


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](../../../security/benchmarks/security-control-data-protection.md).*

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: veri ve kaynaklara erişimi denetlemek için Azure rol tabanlı erişim denetimi (Azure RBAC) kullanın. Azure Kaynak grafiğini kullanmak için, sorgulamak istediğiniz kaynaklara uygun erişiminizin olması gerekir. Bu erişimin kapsamı salt okunurdur ve yalnızca gerekli personele verilmelidir.

* [Azure Kaynak Grafiği’nde izinler](../overview.md#permissions-in-azure-resource-graph)

* [Azure RBAC 'yi yapılandırma](../../../role-based-access-control/role-assignments-rest.md)


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](../../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: abonelikleriniz, yönetim grupları ve kiracılar içindeki tüm desteklenen kaynakları sorgulamak ve saptamak Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun izinlere sahip olduğunuzdan ve aboneliklerinizin içindeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

* [Azure Kaynak Graf ile sorgu oluşturma](../first-query-portal.md)

* [Azure RBAC 'yi anlama](../../../role-based-access-control/overview.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynaklarının envanterini tanımlayın ve saklayın

**Rehberlik**: Kurumsal gereksinimlerinize göre işlem kaynakları Için onaylanan Azure kaynakları ve onaylanan yazılım envanterini oluşturun. Azure Kaynak Grafiği 'ni kullanarak, anlık görüntüleri gözden geçirip nelerin değiştiğini görmek için onaylanan Azure kaynaklarını ve değişiklik geçmişini (Önizleme) sorgulayın.

* [Azure Resource Graph ile Azure kaynaklarını sorgulama](../first-query-portal.md)

* [Kaynak değişikliklerini alma](../how-to/get-resource-changes.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aboneliklerinizde, yönetim gruplarında ve kiracılarınızda kaynakları sorgulamak ve saptamak Için Azure Kaynak grafiğini kullanın. Ortamdaki tüm Azure kaynaklarının onaylandığından emin olun.

* [Azure Resource Graph ile Azure kaynaklarını sorgulama](../first-query-portal.md)

* [Örnekler: Azure Kaynak Grafiği için başlangıç sorguları](../samples/starter.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../../../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../../../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin
