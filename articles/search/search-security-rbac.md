---
title: Azure yönetim erişimi için RBAC rollerini ayarlama
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama yönetimi için yönetim görevlerini denetlemek ve devredetgetirmek için Azure portalında rol tabanlı yönetim denetimi (RBAC).
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 9262d01e35bd03a9116a30b070b023f578f0b15a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74112565"
---
# <a name="set-rbac-roles-for-administrative-access-to-azure-cognitive-search"></a>Azure Bilişsel Arama'ya yönetimsel erişim için RBAC rollerini ayarlama

Azure, portal veya Kaynak Yöneticisi API'leri aracılığıyla yönetilen tüm hizmetler için [genel bir rol tabanlı yetkilendirme modeli](../role-based-access-control/role-assignments-portal.md) sağlar. Sahip, Katılımcı ve Okuyucu rolleri, her role atanan Etkin Dizin kullanıcıları, grupları ve güvenlik ilkeleri için *hizmet yönetimi* düzeyini belirler. 

> [!Note]
> Bir dizin veya belgelerin bir alt kümesinin bölümlerini güvence altına almak için rol tabanlı erişim denetimleri yoktur. Arama sonuçlarına göre kimlik tabanlı erişim için, isteklerin erişmemesi gereken belgeleri kaldırarak sonuçları kimlikle kırpmak için güvenlik filtreleri oluşturabilirsiniz. Daha fazla bilgi için [Bkz. Güvenlik filtreleri](search-security-trimming-for-azure-search.md) ve [Active Directory ile Güvenli.](search-security-trimming-for-azure-search-with-aad.md)

## <a name="management-tasks-by-role"></a>Role göre yönetim görevleri

Azure Bilişsel Arama için roller, aşağıdaki yönetim görevlerini destekleyen izin düzeyleriyle ilişkilidir:

| Rol | Görev |
| --- | --- |
| Sahip |Api tuşları, dizin ler, dizin oluşturucular, dizin oluşturucu veri kaynakları ve dizin oluşturucu zamanlamaları da dahil olmak üzere hizmeti veya hizmetteki herhangi bir nesneyi oluşturun veya silin.<p>Sayımlar ve depolama boyutu da dahil olmak üzere hizmet durumunu görüntüleyin.<p>Rol üyeliği ekleme veya silme (yalnızca bir Sahip rol üyeliğini yönetebilir).<p>Abonelik yöneticileri ve hizmet sahipleri, Sahipler rolünde otomatik üyeliğe sahiptir. |
| Katılımcı |Sahibi, eksi RBAC rol yönetimi ile aynı erişim düzeyi. Örneğin, Bir Katılımcı nesneleri oluşturabilir veya silebilir veya [api anahtarlarını](search-security-api-keys.md)görüntüleyebilir ve yeniden oluşturabilir, ancak rol üyeliklerini değiştiremez. |
| [Arama Hizmeti Katılımcısı yerleşik rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor) | Katılımcı rolüne eşdeğerdir. |
| Okuyucu |Hizmet temellerini ve ölçümlerini görüntüleyin. Bu rolün üyeleri dizin, dizinleyici, veri kaynağı veya anahtar bilgileri görüntüleyemez.  |

Roller, hizmet bitiş noktasına erişim hakkı vermez. Dizin yönetimi, dizin popülasyonu ve arama verilerindeki sorgular gibi arama hizmeti işlemleri, roller le değil, api anahtarları yla denetlenir. Daha fazla bilgi için [bkz.](search-security-api-keys.md)

## <a name="see-also"></a>Ayrıca bkz.

+ [PowerShell kullanarak yönetme](search-manage-powershell.md) 
+ [Azure Bilişsel Arama'da performans ve optimizasyon](search-performance-optimization.md)
+ [Azure portalında Rol Tabanlı Erişim Denetimi'ne başlayın.](../role-based-access-control/overview.md)