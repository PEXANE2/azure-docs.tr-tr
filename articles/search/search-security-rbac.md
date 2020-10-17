---
title: Azure yönetim erişimi için Azure rollerini ayarlama
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama Yönetimi için yönetim görevlerini denetlemek ve bunların yetkisini vermek üzere Azure portal rol tabanlı erişim denetimi (RBAC).
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/16/2020
ms.openlocfilehash: 2f9f979e5871a4888978ff14362a7fb0082917d5
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92151196"
---
# <a name="set-azure-roles-for-administrative-access-to-azure-cognitive-search"></a>Azure 'a yönetici erişimi için Azure rolleri ayarlama Bilişsel Arama

Azure, portal veya Kaynak Yöneticisi API 'Leri aracılığıyla yönetilen tüm hizmetler için [Genel rol tabanlı bir yetkilendirme modeli](../role-based-access-control/role-assignments-portal.md) sağlar. Sahip, katkıda bulunan ve okuyucu rolleri, her role atanan Active Directory Kullanıcılar, gruplar ve güvenlik sorumluları için *hizmet yönetiminin* düzeyini belirlemektir. 

> [!Note]
> Hizmetin içeriğini güvenli hale getirmek için rol tabanlı erişim denetimi (RBAC) yoktur. Hizmetin kendisi için bir yönetici API anahtarı ya da sorgu API anahtarı kullanacaksınız. Arama sonuçları üzerinden kimlik tabanlı erişim için, sonuçları kimliğe göre kırpmak, istek sahibinin erişimi olmaması gereken belgeleri kaldırmak için güvenlik filtreleri oluşturabilirsiniz. Daha fazla bilgi için bkz. [Güvenlik filtreleri](search-security-trimming-for-azure-search.md).

## <a name="management-tasks-by-role"></a>Role göre yönetim görevleri

Azure Bilişsel Arama için roller, aşağıdaki yönetim görevlerini destekleyen izin düzeyleriyle ilişkilendirilir:

| Rol | Görev |
| --- | --- |
| Sahip |API anahtarları, dizinler, Dizin oluşturucular, Dizin Oluşturucu veri kaynakları ve Dizin Oluşturucu zamanlamaları dahil hizmet veya hizmette herhangi bir nesne oluşturun veya silin.<p>Sayılar ve depolama boyutu dahil olmak üzere hizmet durumunu görüntüleyin.<p>Rol üyeliği ekleme veya silme (yalnızca sahip, rol üyeliğini yönetebilir).<p>Abonelik yöneticileri ve hizmet sahipleri, sahipler rolünde otomatik üyeliğe sahiptir. |
| Katılımcı | Sahibe göre aynı erişim düzeyi, eksi Azure rol yönetimi. Örneğin, katkıda bulunan, nesne oluşturabilir veya silebilir ya da [API anahtarlarını](search-security-api-keys.md)görüntüleyebilir ve yeniden oluşturabilir, ancak rol üyeliklerini değiştiremezler.<br><br>[Arama hizmeti katkıda](../role-based-access-control/built-in-roles.md#search-service-contributor) bulunan, genel katkıda bulunan yerleşik rolü ile eşdeğerdir. |
| Okuyucu |Hizmet uç noktası, abonelik, kaynak grubu, bölge, katman ve kapasite gibi hizmet temellerini görüntüleyin. Izleme sekmesinde, saniye başına ortalama sorgu gibi hizmet ölçümlerini da görüntüleyebilirsiniz. Bu rolün üyeleri dizin, Dizin Oluşturucu, veri kaynağı veya beceri bilgilerini görüntüleyemez. Bu, hizmette kaç Dizin bulunduğu gibi bu nesneler için kullanım verilerini içerir. |

Roller, hizmet uç noktasına erişim hakkı vermez. Dizin Yönetimi, dizin oluşturma ve arama verilerinde sorgular gibi arama hizmeti işlemleri, rol değil API anahtarları aracılığıyla denetlenir. Daha fazla bilgi için bkz. [API anahtarlarını yönetme](search-security-api-keys.md).

## <a name="permissions-table"></a>İzinler tablosu

Aşağıdaki tabloda, Azure Bilişsel Arama 'de izin verilen işlemler özetlenmektedir ve hangi anahtarın belirli bir işleme erişim kilidi vardır.

RBAC izinleri Portal işlemleri ve hizmet yönetimi için geçerlidir (hizmet veya API anahtarlarını oluşturma, silme veya değiştirme). API anahtarları bir hizmet mevcut olduktan sonra oluşturulur ve hizmette içerik işlemlerine uygulanır. Ayrıca, portaldaki içerikle ilgili işlemler (örneğin, nesne oluşturma veya silme gibi), bir RBAC sahibi veya katkıda bulunan hizmeti, kapsanan bir yönetici API anahtarı ile birlikte etkileşime geçin.

| İşlem | Denetleyen |
|-----------|-------------------------|
| Hizmet oluşturma | RBAC izinleri: sahip veya katkıda bulunan |
| Hizmeti ölçeklendirme | RBAC izinleri: sahip veya katkıda bulunan|
| Hizmet silme | RBAC izinleri: sahip veya katkıda bulunan |
| Yönetici veya sorgu anahtarlarını yönetme | RBAC izinleri: sahip veya katkıda bulunan|
| Portalda veya yönetim API 'sindeki hizmet bilgilerini görüntüleme | RBAC izinleri: sahip, katkıda bulunan veya okuyucu  |
| Portalda veya yönetim API 'sindeki nesne bilgilerini ve ölçümlerini görüntüleme | RBAC izinleri: sahip veya katkıda bulunan |
| Hizmette nesne oluşturun, değiştirin, silin: <br>Dizinler ve bileşen bölümleri (çözümleyici tanımları, Puanlama profilleri, CORS seçenekleri dahil), Dizin oluşturucular, veri kaynakları, eş anlamlılar, öneri araçları | Bir API kullanılıyorsa yönetici anahtarı, Portal kullanılıyorsa RBAC sahibi veya katkıda bulunan |
| Dizin sorgulama | Bir API kullanılıyorsa yönetici veya sorgu anahtarı, Portal kullanılıyorsa RBAC sahibi veya katkıda bulunan |
| Nesneler hakkında, istatistik, sayı ve nesne listesi döndürme gibi sistem bilgilerini sorgulama | Bir API kullanılıyorsa yönetici anahtarı, Portal kullanılıyorsa RBAC sahibi veya katkıda bulunan |

## <a name="next-steps"></a>Sonraki adımlar

+ [PowerShell kullanarak yönetme](search-manage-powershell.md) 
+ [Azure Bilişsel Arama performans ve iyileştirme](search-performance-optimization.md)
+ [Azure portal Role-Based Access Control kullanmaya](../role-based-access-control/overview.md)başlayın.
