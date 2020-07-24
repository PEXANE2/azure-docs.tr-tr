---
title: Azure Sentinel izinleri | Microsoft Docs
description: Bu makalede, Azure Sentinel 'in kullanıcılara izin atamak için rol tabanlı erişim denetimi nasıl kullandığı ve her bir rol için izin verilen eylemleri tanımladığı açıklanır.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2020
ms.author: yelevin
ms.openlocfilehash: 9f9a84726b54569d612a94f183531567b2242ff5
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87127171"
---
# <a name="permissions-in-azure-sentinel"></a>Azure Sentinel izinleri

Azure Sentinel, Azure 'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../role-based-access-control/built-in-roles.md)sağlamak Için [rol tabanlı Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md) kullanır   .

Azure Sentinel 'e uygun erişim sağlamak için güvenlik işlemleri ekibiniz dahilinde roller oluşturup atamak üzere RBAC 'yi kullanın. Farklı roller, Azure Sentinel 'in kullanıcılarının görebileceği ve yapabilecekleri ayrıntılı denetim sağlar. RBAC rolleri doğrudan Azure Sentinel çalışma alanına atanabilir (aşağıdaki nota bakın) veya çalışma alanının ait olduğu bir abonelikte veya kaynak grubunda Azure Sentinel 'in devralması gerekir.

## <a name="roles-for-working-in-azure-sentinel"></a>Azure Sentinel 'de çalışmak için roller

### <a name="azure-sentinel-specific-roles"></a>Azure Sentinel 'e özgü roller

Üç adanmış yerleşik Azure Sentinel rolü vardır.

**Tüm Azure Sentinel yerleşik rolleri, Azure Sentinel çalışma alanınızdaki verilere yönelik okuma erişimi verir.**

- [Azure Sentinel okuyucusu](../role-based-access-control/built-in-roles.md#azure-sentinel-reader) verileri, olayları, çalışma kitaplarını ve diğer Azure Sentinel kaynaklarını görüntüleyebilir.

- [Azure Sentinel Yanıtlayıcı](../role-based-access-control/built-in-roles.md#azure-sentinel-responder) , yukarıdaki, olayları yönetme (ata, kapat vb.)

- [Azure Sentinel katılımcısı](../role-based-access-control/built-in-roles.md#azure-sentinel-contributor) , yukarıdaki ' a ek olarak, çalışma kitapları, analiz kuralları ve diğer Azure Sentinel kaynakları oluşturabilir ve düzenleyebilir.

> [!NOTE]
>
> - En iyi sonuçlar için, bu rollerin Azure Sentinel çalışma alanını içeren **kaynak grubunda** atanması gerekir. Bu şekilde, bu kaynaklar aynı kaynak grubuna yerleştirilmesi gerektiği için, roller Azure Sentinel 'i desteklemek için dağıtılan tüm kaynaklara uygulanır.
>
> - Diğer bir seçenek de rolleri doğrudan Azure Sentinel **çalışma alanına** atamanız olur. Bunu yaparsanız, bu çalışma alanındaki Securityınsights **çözüm kaynağında** aynı rolleri de atamanız gerekir. Bunları diğer kaynaklara da atamanız gerekebilir ve kaynaklarda rol atamalarını sürekli olarak yönetmeniz gerekir.

### <a name="additional-roles-and-permissions"></a>Ek roller ve izinler

Belirli iş gereksinimlerine sahip kullanıcılara görevlerini gerçekleştirmek için ek roller veya belirli izinler atanması gerekebilir.

- Tehditlere yönelik yanıtları otomatik hale getirmek için PlayBook 'lar ile çalışma

    Azure Sentinel otomatik tehdit yanıtı için **PlayBook 'ları** kullanır. Playbooks **Azure Logic Apps**kurulmuştur ve ayrı bir Azure kaynağıdır. Güvenlik işlemleri takımınızın belirli üyelerine, güvenlik düzenleme, otomasyon ve yanıt (SOAR) işlemleri için Logic Apps kullanma yeteneği atamak isteyebilirsiniz. PlayBook 'ları kullanmaya açık izin atamak için [Logic App katılımcısı](../role-based-access-control/built-in-roles.md#logic-app-contributor) rolünü veya [mantıksal uygulama işletmeni](../role-based-access-control/built-in-roles.md#logic-app-operator) rolünü kullanabilirsiniz.

- Veri kaynaklarını Azure Sentinel 'e bağlama

    Bir kullanıcının **veri bağlayıcıları**eklemesi Için, Azure Sentinel çalışma alanında Kullanıcı yazma izinlerini atamanız gerekir. Ayrıca, ilgili bağlayıcı sayfasında listelendiği gibi her bağlayıcı için gerekli ek izinleri göz önünde bulabilirsiniz.

Yan yana karşılaştırma için [aşağıdaki tabloya](#roles-and-allowed-actions)bakın.

### <a name="other-roles-you-might-see-assigned"></a>Atanmış olarak görebileceğiniz diğer roller

Azure Sentinel 'e özgü RBAC rolleri atarken, diğer Azure ve diğer amaçlar için kullanıcılara atanmış olan RBAC rollerinin Log Analytics olabilirsiniz. Bu rollerin Azure Sentinel çalışma alanınıza ve diğer kaynaklara erişim de dahil olmak üzere daha geniş bir izin kümesi verildiğini bilmeniz gerekir:

- **Azure rolleri:** [sahip](../role-based-access-control/built-in-roles.md#owner), [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor)ve [okuyucu](../role-based-access-control/built-in-roles.md#reader). Azure rolleri, Log Analytics çalışma alanları ve Azure Sentinel kaynakları dahil olmak üzere tüm Azure kaynaklarınız genelinde erişim izni verir.

- **Log Analytics roller:** [katkıda bulunan](../role-based-access-control/built-in-roles.md#log-analytics-contributor) ve [Log Analytics okuyucu](../role-based-access-control/built-in-roles.md#log-analytics-reader)Log Analytics. Log Analytics roller Log Analytics çalışma alanlarınızın erişimine izin verir. 

Örneğin, Azure Sentinel **Reader** rolünü atayan, ancak Azure **Sentinel katkıda** bulunan rolüne sahip olmayan bir Kullanıcı, Azure düzeyinde **katkıda** bulunan rolü atanmışsa Azure Sentinel 'de öğeleri düzenleyebilecektir. Bu nedenle, yalnızca Azure Sentinel 'de bir kullanıcıya izin vermek istiyorsanız, bu kullanıcının önceki izinlerini dikkatle kaldırmanız gerekir ve bu, başka bir kaynağa gereken erişimi bozmayın.

## <a name="roles-and-allowed-actions"></a>Roller ve izin verilen eylemler

Aşağıdaki tabloda, Azure Sentinel 'de roller ve izin verilen eylemler özetlenmektedir. 

| Rol | PlayBook 'lar oluşturma ve çalıştırma| Çalışma kitaplarını, analitik kuralları ve diğer Azure Sentinel kaynaklarını oluşturma ve düzenleme | Olayları yönetme (Kapat, ata, vb.) | Verileri, olayları, çalışma kitaplarını ve diğer Azure Sentinel kaynaklarını görüntüleyin |
|---|---|---|---|---|
| Azure Sentinel Okuyucusu | -- | -- | -- | &#10003; |
| Azure Sentinel Yanıtlayıcısı | -- | -- | &#10003; | &#10003; |
| Azure Sentinel Katkıda Bulunanı | -- | &#10003; | &#10003; | &#10003; |
| Azure Sentinel katkıda bulunan + Logic App Katılımcısı | &#10003; | &#10003; | &#10003; | &#10003; |

## <a name="custom-roles-and-advanced-rbac"></a>Özel roller ve gelişmiş RBAC

- Ya da yerine, yerleşik RBAC rollerini kullanarak Azure Sentinel için Azure özel rolleri oluşturabilirsiniz. Azure Sentinel için Azure özel rolleri, [Azure Sentinel 'e](../role-based-access-control/resource-provider-operations.md#microsoftsecurityinsights) ve [Azure Log Analytics kaynaklarına](../role-based-access-control/resource-provider-operations.md#microsoftoperationalinsights)özel izinler temel ALıNARAK diğer [özel Azure RBAC](../role-based-access-control/custom-roles-rest.md#create-a-custom-role) rollerini oluşturduğunuz şekilde oluşturulmuştur.

- Log Analytics gelişmiş rol tabanlı erişim denetimini Azure Sentinel çalışma alanınızdaki veriler arasında kullanabilirsiniz. Bu hem veri türü tabanlı RBAC hem de kaynak merkezli RBAC içerir. Log Analytics rolleri hakkında daha fazla bilgi için bkz. [Azure izleyici 'de günlük verilerini ve çalışma alanlarını yönetme](../azure-monitor/platform/manage-access.md#manage-access-using-workspace-permissions).

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Sentinel kullanıcıları için rollerle çalışmayı ve her rolün kullanıcılara ne yaptığını öğrendiniz.

* [Azure Sentinel blogu](https://aka.ms/azuresentinelblog). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
