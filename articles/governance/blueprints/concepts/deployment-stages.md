---
title: Şema dağıtımının aşamaları
description: Bir plan ataması oluştururken Azure Blueprints hizmetlerinin geçtiği güvenlik ve yapıyla ilgili adımları öğrenin.
ms.date: 11/13/2019
ms.topic: conceptual
ms.openlocfilehash: daa7722fa37547929aa21b76b870f70143ae71ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76156633"
---
# <a name="stages-of-a-blueprint-deployment"></a>Şema dağıtımının aşamaları

Bir plan dağıtıldığında, planda tanımlanan kaynakları dağıtmak için Azure Planları hizmeti tarafından bir dizi eylem yapılır. Bu makalede, her adım ne içerir hakkında ayrıntılar sağlar.

Plan dağıtımı, bir aboneye bir plan atayarak veya [varolan bir atamayı güncelleştirerek](../how-to/update-existing-assignments.md)tetiklenir. Dağıtım sırasında Blueprints aşağıdaki üst düzey adımları alır:

> [!div class="checklist"]
> - Sahip haklarına verilen planlar
> - Plan atama nesnesi oluşturulur
> - İsteğe bağlı - Planlar **sistem le atanmış** yönetilen kimlik oluşturur
> - Yönetilen kimlik, plan yapıtlarını dağıtır
> - Plan hizmeti ve **sistem le atanmış** yönetilen kimlik hakları iptal edildi

## <a name="blueprints-granted-owner-rights"></a>Sahip haklarına verilen planlar

Azure Planları hizmet sorumlusu, [sistem tarafından atanan yönetilen kimlik yönetilen](../../../active-directory/managed-identities-azure-resources/overview.md) kimlik kullanıldığında atanan abonelik veya aboneliklerin sahibine haklar verilir. Verilen rol, Blueprints'in **sistem tarafından atanan** yönetilen kimliği oluşturmasına ve daha sonra iptal etmesine olanak tanır. **Kullanıcı tarafından atanmış** yönetilen bir kimlik kullanıyorsanız, Azure Planları hizmet sorumlusu abonelikte sahip hakları almaz ve gerekmez.

Atama portal üzerinden yapılırsa haklar otomatik olarak verilir. Ancak, atama REST API üzerinden yapılırsa, hakları verilmesi ayrı bir API çağrısı ile yapılması gerekir. Azure Blueprints AppId'dir, `f71766dc-90d9-4b7d-bd9d-4499c4331c3f`ancak hizmet ilkesi kiracıya göre değişir. Hizmet ilkesini almak için [Azure Active Directory Graph API](../../../active-directory/develop/active-directory-graph-api.md) ve REST uç nokta [hizmetMüdürlerini](/graph/api/resources/serviceprincipal) kullanın. Ardından, Azure Blueprints'e [Portal](../../../role-based-access-control/role-assignments-portal.md), [Azure CLI](../../../role-based-access-control/role-assignments-cli.md), [Azure PowerShell](../../../role-based-access-control/role-assignments-powershell.md), [REST API](../../../role-based-access-control/role-assignments-rest.md)veya [Kaynak Yöneticisi şablonu](../../../role-based-access-control/role-assignments-template.md)aracılığıyla _Sahip_ rolünü ver.

Blueprints hizmeti kaynakları doğrudan dağıtmaz.

## <a name="the-blueprint-assignment-object-is-created"></a>Plan atama nesnesi oluşturulur

Bir kullanıcı, grup veya hizmet sorumlusu bir aboneye bir plan atar. Atama nesnesi, planın atandığı abonelik düzeyinde bulunur. Dağıtım tarafından oluşturulan kaynaklar, dağıtım varlığı bağlamında yapılmaz.

Plan ataması oluşturulurken, [yönetilen kimlik](../../../active-directory/managed-identities-azure-resources/overview.md) türü seçilir. Varsayılan, **sistem tarafından atanmış** yönetilen bir kimliktir. **Kullanıcı tarafından atanan** yönetilen kimlik seçilebilir. **Kullanıcı tarafından atanan** yönetilen bir kimlik kullanırken, plan ataması oluşturulmadan önce tanımlanması ve izinverilmesi gerekir. Hem [Sahip](../../../role-based-access-control/built-in-roles.md#owner) hem de [Blueprint Operatör](../../../role-based-access-control/built-in-roles.md#blueprint-operator) yerleşik `blueprintAssignment/write` **rolleri, kullanıcı tarafından atanan** yönetilen bir kimlik kullanan bir atama oluşturmak için gerekli izine sahiptir.

## <a name="optional---blueprints-creates-system-assigned-managed-identity"></a>İsteğe bağlı - Planlar sistem le atanmış yönetilen kimlik oluşturur

Atama sırasında [sistem tarafından atanan yönetilen kimlik](../../../active-directory/managed-identities-azure-resources/overview.md) seçildiğinde, Planlar kimliği oluşturur ve yönetilen kimliğe [sahip](../../../role-based-access-control/built-in-roles.md#owner) rolü verir. Varolan bir [atama yükseltilirse,](../how-to/update-existing-assignments.md)Blueprints önceden oluşturulmuş yönetilen kimliği kullanır.

Plan ataması ile ilgili yönetilen kimlik, planda tanımlanan kaynakları dağıtmak veya yeniden dağıtmak için kullanılır. Bu tasarım, atamaların yanlışlıkla birbirine karışmamasını önler.
Bu tasarım, dağıtılan her kaynağın güvenliğini plandan denetleyerek [kaynak kilitleme](./resource-locking.md) özelliğini de destekler.

## <a name="the-managed-identity-deploys-blueprint-artifacts"></a>Yönetilen kimlik, plan yapıtlarını dağıtır

Yönetilen kimlik daha sonra tanımlanan [sıralama sırasına](./sequencing-order.md)göre plan içindeki yapıtların Kaynak Yöneticisi dağıtımlarını tetikler. Sipariş, diğer yapılara bağımlı yapılardoğru sırayla dağıtıldığından emin olmak için ayarlanabilir.

Dağıtım tarafından bir erişim hatası genellikle yönetilen kimliğe verilen erişim düzeyinin sonucudur. Blueprints hizmeti, **sistem tarafından atanan** yönetilen kimliğin güvenlik yaşam döngüsünü yönetir. Ancak, kullanıcı tarafından atanan yönetilen bir kimliğin haklarını ve yaşam döngüsünü **yönetmekten kullanıcı** sorumludur.

## <a name="blueprint-service-and-system-assigned-managed-identity-rights-are-revoked"></a>Plan hizmeti ve sistem le atanmış yönetilen kimlik hakları iptal edildi

Dağıtımlar tamamlandıktan sonra, Blueprints **sistem tarafından atanan** yönetilen kimliğin abonelikten haklarını iptal eder. Ardından, Blueprints hizmeti abonelikhaklarını iptal eder. Hakların kaldırılması, Blueprints'in abonelikte kalıcı sahibi olmasını engeller.

## <a name="next-steps"></a>Sonraki adımlar

- [Statik ve dinamik parametrelerin](parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../how-to/update-existing-assignments.md)öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.
