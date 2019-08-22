---
title: Portalda Azure yönetim erişimi için RBAC rolleri ayarlama-Azure Search
description: Azure Search yönetimi için yönetim görevlerini denetlemek ve bunların yetkisini vermek üzere Azure portal rol tabanlı yönetim denetimi (RBAC).
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 924d2529e3477c299d4a90c076fe9e6c8faf11f3
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647402"
---
# <a name="set-rbac-roles-for-administrative-access"></a>Yönetim erişimi için RBAC rollerini ayarlama

Azure, portal veya Kaynak Yöneticisi API 'Leri aracılığıyla yönetilen tüm hizmetler için [Genel rol tabanlı bir yetkilendirme modeli](../role-based-access-control/role-assignments-portal.md) sağlar. Sahip, katkıda bulunan ve okuyucu rolleri, her role atanan Active Directory Kullanıcılar, gruplar ve güvenlik sorumluları için *hizmet yönetiminin* düzeyini belirlemektir. 

> [!Note]
> Bir dizinin veya bir belge alt kümesinin bölümlerinin güvenliğini sağlamak için rol tabanlı erişim denetimi yoktur. Arama sonuçları üzerinden kimlik tabanlı erişim için, sonuçları kimliğe göre kırpmak, istek sahibinin erişimi olmaması gereken belgeleri kaldırmak için güvenlik filtreleri oluşturabilirsiniz. Daha fazla bilgi için bkz. [Güvenlik filtreleri](search-security-trimming-for-azure-search.md) ve [Active Directory Ile güvenli hale getirme](search-security-trimming-for-azure-search-with-aad.md).

## <a name="management-tasks-by-role"></a>Role göre yönetim görevleri

Azure Search için roller, aşağıdaki yönetim görevlerini destekleyen izin düzeyleriyle ilişkilendirilir:

| Role | Görev |
| --- | --- |
| Sahip |API anahtarları, dizinler, Dizin oluşturucular, Dizin Oluşturucu veri kaynakları ve Dizin Oluşturucu zamanlamaları dahil hizmet veya hizmette herhangi bir nesne oluşturun veya silin.<p>Sayılar ve depolama boyutu dahil olmak üzere hizmet durumunu görüntüleyin.<p>Rol üyeliği ekleme veya silme (yalnızca sahip, rol üyeliğini yönetebilir).<p>Abonelik yöneticileri ve hizmet sahipleri, sahipler rolünde otomatik üyeliğe sahiptir. |
| Katılımcı |Sahibe göre aynı erişim düzeyi, eksi RBAC rol yönetimi. Örneğin, katkıda bulunan, nesne oluşturabilir veya silebilir ya da [API anahtarlarını](search-security-api-keys.md)görüntüleyebilir ve yeniden oluşturabilir, ancak rol üyeliklerini değiştiremezler. |
| [Arama Hizmeti katkıda bulunan yerleşik rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor) | Katkıda bulunan rolüne eşdeğerdir. |
| Okuyucu |Hizmet temellerini ve ölçümlerini görüntüleyin. Bu rolün üyeleri dizin, Dizin Oluşturucu, veri kaynağı veya anahtar bilgilerini görüntüleyemez.  |

Roller, hizmet uç noktasına erişim hakkı vermez. Dizin Yönetimi, dizin oluşturma ve arama verilerinde sorgular gibi arama hizmeti işlemleri, rol değil API anahtarları aracılığıyla denetlenir. Daha fazla bilgi için bkz. [API anahtarlarını yönetme](search-security-api-keys.md).

## <a name="see-also"></a>Ayrıca bkz.

+ [PowerShell’i kullanarak yönetme](search-manage-powershell.md) 
+ [Azure Search performans ve iyileştirme](search-performance-optimization.md)
+ [Azure Portal rol tabanlı Access Control kullanmaya](../role-based-access-control/overview.md)başlayın.