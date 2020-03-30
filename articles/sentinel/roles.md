---
title: Azure Sentinel'de İzinler | Microsoft Dokümanlar
description: Bu makalede, Azure Sentinel'in kullanıcılara izin atamak için rol tabanlı erişim denetimini nasıl kullandığı açıklanmaktadır ve her rol için izin verilen eylemleri tanımlar.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: angrobe
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: yelevin
ms.openlocfilehash: e7629a53190433c6c331ce372476b0ed768fc5eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77587184"
---
# <a name="permissions-in-azure-sentinel"></a>Azure Sentinel'de İzinler

Azure Sentinel, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilecek [yerleşik roller](../role-based-access-control/built-in-roles.md) sağlamak için [Rol Tabanlı Erişim Denetimi(RBAC)](../role-based-access-control/role-assignments-portal.md)kullanır.

RBAC'ı kullanarak, Azure Sentinel'e uygun erişim sağlamak için güvenlik işlemleri ekibinizdeki rolleri kullanabilir ve oluşturabilirsiniz. Rollere bağlı olarak, Azure Sentinel erişimi olan kullanıcıların neler görebileceği üzerinde ince ayarlı denetime sahipsiniz. Azure Sentinel çalışma alanında rbac rollerini doğrudan veya çalışma alanının ait olduğu bir abonelik veya kaynak grubuna atayabilirsiniz.

Üç özel yerleşik Azure Sentinel rolü vardır.  
**Tüm Azure Sentinel yerleşik rolleri, Azure Sentinel çalışma alanınızdaki verilere okuma erişimi sağlar.**
- [Azure Sentinel okuyucu](../role-based-access-control/built-in-roles.md#azure-sentinel-reader)
- [Azure Sentinel yanıtlayıcı](../role-based-access-control/built-in-roles.md#azure-sentinel-responder)
- [Azure Sentinel katılımcısı](../role-based-access-control/built-in-roles.md#azure-sentinel-contributor)

Azure Sentinel özel RBAC rollerine ek olarak, Azure Sentinel çalışma alanınıza ve diğer kaynaklara erişimi de içeren daha geniş bir izin kümesi veren Azure ve Log Analytics RBAC rolleri de vardır:

- **Azure rolleri:** [Sahibi](../role-based-access-control/built-in-roles.md#owner), [Katkıda Bulunan](../role-based-access-control/built-in-roles.md#contributor)ve [Okuyucu](../role-based-access-control/built-in-roles.md#reader). Azure rolleri, Log Analytics çalışma alanları ve Azure Sentinel kaynakları da dahil olmak üzere tüm Azure kaynaklarınızda erişim sağlar.

-   **Log Analytics rolleri:** [Log Analytics katılımcısı](../role-based-access-control/built-in-roles.md#log-analytics-contributor), [Log Analytics okuyucu](../role-based-access-control/built-in-roles.md#log-analytics-reader). Log Analytics rolleri, tüm Log Analytics çalışma alanlarınızda erişim sağlar. 

> [!NOTE]
> Günlük Analizi rolleri ayrıca tüm Azure kaynaklarında okuma erişimi sağlar, ancak yalnızca Log Analytics kaynaklarına yazma izinleri atar.


Örneğin, **Azure Sentinel okuyucusu** ve **Azure katılımcısı** **(Azure Sentinel katılımcısı**değil) rolleri ile atanan bir kullanıcı, yalnızca **Sentinel okuyucu** izinlerine sahip olmalarına rağmen Azure Sentinel'deki verileri yeniden atabilir. Bu nedenle, yalnızca Azure Sentinel'de izin vermek istiyorsanız, başka bir kaynak için gerekli izin rolünü kırmadığınızdan emin olmak için bu kullanıcının önceki izinlerini dikkatle kaldırmanız gerekir.

> [!NOTE]
>- Azure Sentinel, otomatik tehdit yanıtı için oyun kitaplarını kullanır. Oyun kitapları Azure Mantık Uygulamalarını yararlanır ve ayrı bir Azure kaynağıdır. Güvenlik düzenleme, otomasyon ve yanıt (SOAR) işlemleri için Logic Apps'ı kullanma seçeneğiyle güvenlik operasyon ekibinizin belirli üyelerini atamak isteyebilirsiniz. Oyun kitaplarını kullanmak için açık izin atamak için [Logic App katılımcısı](../role-based-access-control/built-in-roles.md#logic-app-contributor) rolünü veya [Logic App işleci](../role-based-access-control/built-in-roles.md#logic-app-operator) rolünü kullanabilirsiniz.
>- Veri bağlayıcıları eklemek için, her bağlayıcı için gerekli roller bağlayıcı türüne göredir ve ilgili bağlayıcı sayfasında listelenir. Ayrıca, herhangi bir veri kaynağını bağlamak için Azure Sentinel çalışma alanında yazma izniniz olması gerekir.



## <a name="roles-and-allowed-actions"></a>Roller ve izin verilen eylemler

Aşağıdaki tablo, Azure Sentinel'de rolleri ve izin verilen eylemleri görüntüler. X, bu rol için eyleme izin verildiğini gösterir.

| Rol | Oyun kitapları oluşturma ve çalıştırma| Panolar, analitik kurallar ve diğer Azure Sentinel kaynakları oluşturma ve değiştirme | Olayları yönetme (görevden alma, atama, vb.) | Verileri, olayları, panoları ve diğer Azure Sentinel kaynaklarını görüntüleme |
|--- |---|---|---|---|
| Azure Sentinel okuyucu | -- | -- | -- | X |
| Azure Sentinel yanıtlayıcı|--|--| X | X |
| Azure Sentinel katılımcısı | -- | X | X | X |
| Azure Sentinel katılımcısı + Logic App katılımcısı | X | X | X | X |


> [!NOTE]
> - Kullanıcılara, görevlerini tamamlamak için gereken rolleri en alt seviyede esneklik sunacak şekilde atamanızı öneririz. Örneğin, Azure Sentinel katılımcısı rolünü yalnızca kural veya pano oluşturması gereken kullanıcılara atayın.
> - Kullanıcının aynı kaynak grubundaki tüm Azure Sentinel çalışma alanlarına erişebilmeleri için kaynak grubu kapsamında Azure Sentinel için izinayarlamanızı öneririz.
>
## <a name="building-custom-rbac-roles"></a>Özel RBAC rolleri oluşturma

Yerleşik RBAC rollerine ek olarak veya bunun yerine Azure Sentinel için özel RBAC rolleri oluşturabilirsiniz. Azure Sentinel için özel RBAC rolleri, Azure Sentinel'e ve [Azure Günlük Analizi kaynaklarına](../role-based-access-control/resource-provider-operations.md#microsoftoperationalinsights) [özel izinlere](../role-based-access-control/resource-provider-operations.md#microsoftsecurityinsights) dayalı olarak diğer [özel Azure RBAC](../role-based-access-control/custom-roles-rest.md#create-a-custom-role) rollerini oluşturduğunuz şekilde oluşturulur.

## <a name="advanced-rbac-on-the-data-you-store-in-azure-sentinel"></a>Azure Sentinel’de depoladığınız veriler üzerinde gelişmiş RBAC
  
Azure Sentinel çalışma alanınızdaki veriler arasında Log Analytics gelişmiş rol tabanlı erişim denetimini kullanabilirsiniz. Bu, hem veri türü başına rol tabanlı erişim denetimini hem de kaynak merkezli rol tabanlı erişim denetimini içerir. Günlük Analizi rolleri hakkında daha fazla bilgi için Azure [Monitor'da günlük verilerini ve çalışma alanlarını yönet](../azure-monitor/platform/manage-access.md#manage-access-using-workspace-permissions)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel kullanıcıları için rollerle nasıl çalışacağınızı ve her rolün kullanıcıların ne yapmasına olanak sağladığını öğrendiniz.

* [Azure Sentinel Blog](https://aka.ms/azuresentinelblog). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
