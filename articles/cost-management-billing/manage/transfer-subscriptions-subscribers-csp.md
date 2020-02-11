---
title: Azure aboneliklerini aboneler ile CSP'ler arasında aktarma
description: Azure aboneliklerini aboneler ile CSP'ler arasında nasıl aktarabileceğinizi öğrenin.
services: billing
author: bandersmsft
ms.reviewer: dhgandhi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: banders
ms.openlocfilehash: 08aeac9a8cd21e6f13bab0ee10ba4d212d96c5b0
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76775423"
---
# <a name="transfer-azure-subscriptions-between-subscribers-and-csps"></a>Azure aboneliklerini aboneler ile CSP'ler arasında aktarma

Bu makalede Azure aboneliklerini Bulut Çözümü Sağlayıcıları (CSP) iş ortakları ile müşterileri arasında aktarmayla ilgili üst düzey adımlara yer verilmiştir.

## <a name="transfer-ea-subscriptions"></a>EA aboneliklerini aktarma

[Azure Uzman Yönetilen Hizmetler Sağlayıcısı (MSP)](https://partner.microsoft.com/membership/azure-expert-msp) sertifikalı CSP doğrudan faturalandırma iş ortakları, Doğrudan Kurumsal Anlaşma (EA) sahibi olan müşterilerinin Azure aboneliklerinin aktarılması isteğinde bulunabilirler. Abonelik aktarma işlemleri yalnızca kabul edilen bir Microsoft Müşteri Sözleşmesi'ne (MCA) sahip olan ve bir Azure planı satın almış olan müşteriler tarafından gerçekleştirilebilir.

İstek onaylandıktan sonra CSP, müşterilerine birleştirilmiş fatura sunabilir. CSP'lerin abonelikleri aktarması hakkında daha fazla bilgi için bkz. [MPA hesabınız için Azure aboneliklerinin fatura sahipliğini alma](mpa-request-ownership.md).

## <a name="other-subscription-transfers-to-a-csp-partner"></a>CSP iş ortağına diğer aboneliklerin aktarılması

Diğer Azure aboneliklerini CSP iş ortağına aktarmak isteyen abonenin kaynak aboneliklerindeki kaynakları CSP aboneliklerine taşıması gerekir. Kaynakları abonelikler arasında taşımak için aşağıdaki yönergeleri izleyin.

1. CSP iş ortağınızla birlikte çalışarak hedef Azure CSP aboneliklerini oluşturun.
1. Kaynak ve hedef CSP aboneliklerinin aynı Azure Active Directory (Azure AD) kiracısında olduğundan emin olun.  
    Azure CSP aboneliğinin Azure AD kiracısını değiştiremezsiniz. Bunun yerine kaynak aboneliği CSP Azure AD kiracısına eklemeniz veya onunla ilişkilendirmeniz gerekir. Daha fazla bilgi için bkz. [Azure Active Directory kiracınıza bir Azure aboneliğini ekleme veya ilişkilendirme](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).
    > [!IMPORTANT]
    > - Bir aboneliği farklı bir Azure AD dizini ile ilişkilendirdiğinizde [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/role-assignments-portal.md) ile atanmış rollere sahip olan kullanıcılar erişimlerini kaybeder. Hizmet Yöneticisi ve Ortak Yöneticiler dahil olmak üzere klasik abonelik yöneticileri de erişimi kaybeder. 
    > - Abonelik farklı bir dizinle ilişkilendirildiğinde İlke Atamaları da abonelikten kaldırılır.
1. Aktarımı gerçekleştirmek için kullandığınız kullanıcı hesabının iki abonelikte de [RBAC](add-change-subscription-administrator.md) sahip erişimine sahip olması gerekir.
1. Başlamadan önce tüm Azure kaynaklarının kaynak abonelikten hedef aboneliğe taşınabileceğini [doğrulayın](/rest/api/resources/resources/validatemoveresources).  
    Bazı Azure kaynakları farklı aboneliklere taşınamaz. Taşınabilecek Azure kaynaklarının tam listesini görüntülemek için bkz. [Kaynaklar için taşıma işlemi desteği](../../azure-resource-manager/management/move-support-resources.md).
    > [!IMPORTANT]
    >  - Azure CSP yalnızca Azure Resource Manager kaynaklarını destekler. Kaynak abonelikte Azure klasik dağıtım modeli kullanılarak oluşturulan Azure kaynağı varsa geçişten önce [Azure Resource Manager](https://docs.microsoft.com/azure/cloud-solution-provider/migration/ea-payg-to-azure-csp/ea-open-direct-asm-to-arm)'a geçirilmesi gerekir. Web sayfasını görüntülemek için iş ortağı olmanız gerekir.

1. Kaynak abonelik hizmetlerinin tümünün Azure Resource Manager modelini kullandığını doğrulayın. Daha sonra [Azure Kaynağını Taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md) bölümünü kullanarak kaynak abonelikteki kaynakları hedef aboneliğe aktarın.
    > [!IMPORTANT]
    >  - Azure kaynaklarını farklı aboneliklere taşımak, aboneliklerinizdeki kaynaklara göre hizmette kesintiye neden olabilir.

## <a name="all-subscription-transfers-from-a-csp-partner"></a>CSP iş ortağındaki tüm aboneliklerin aktarılması

CSP iş ortağındaki diğer abonelikleri başka bir Azure teklifine taşımak için abonenin kaynak CSP aboneliklerindeki kaynakları hedef aboneliklere taşıması gerekir.

1. Hedef Azure aboneliklerini oluşturun.
1. Kaynak ve hedef aboneliklerin aynı Azure Active Directory (Azure AD) kiracısında olduğundan emin olun. Azure AD kiracısı değiştirme hakkında daha fazla bilgi için bkz. [Azure Active Directory kiracınıza bir Azure aboneliğini ekleme veya ilişkilendirme](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

    > [!IMPORTANT]
    >  - Bir aboneliği farklı bir dizin ile ilişkilendirdiğinizde [RBAC](../../role-based-access-control/role-assignments-portal.md) ile atanmış rollere sahip olan kullanıcılar erişimlerini kaybeder. Hizmet Yöneticisi ve Ortak Yöneticiler dahil olmak üzere klasik abonelik yöneticileri de erişimi kaybeder.
    >  - Abonelik farklı bir dizinle ilişkilendirildiğinde İlke Atamaları da abonelikten kaldırılır.

1. Aktarımı gerçekleştirmek için kullandığınız kullanıcı hesabının iki abonelikte de [RBAC](add-change-subscription-administrator.md) sahip erişimine sahip olması gerekir.
1. Başlamadan önce tüm Azure kaynaklarının kaynak abonelikten hedef aboneliğe taşınabileceğini [doğrulayın](/rest/api/resources/resources/validatemoveresources).
    > [!IMPORTANT]
    >  - Bazı Azure kaynakları farklı aboneliklere taşınamaz. Taşınabilecek Azure kaynaklarının tam listesini görüntülemek için bkz. [Kaynaklar için taşıma işlemi desteği](../../azure-resource-manager/management/move-support-resources.md).

1. [Azure Kaynağını Taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md) bölümünü kullanarak kaynak abonelikteki kaynakları hedef aboneliğe aktarın.
    > [!IMPORTANT]
    >  - Azure kaynaklarını farklı aboneliklere taşımak, aboneliklerinizdeki kaynaklara göre hizmette kesintiye neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure aboneliklerinin fatura sahipliğini MPA hesabınıza alma](mpa-request-ownership.md).
- Bkz. [Azure Faturalandırma ile hesapları ve abonelikleri yönetme](index.yml).
