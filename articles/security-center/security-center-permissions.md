---
title: Azure Güvenlik Merkezi'ndeki izinler | Microsoft Dokümanlar
description: Bu makalede, Azure Güvenlik Merkezi'nin kullanıcılara izin atamak için rol tabanlı erişim denetimini nasıl kullandığı açıklanmaktadır ve her rol için izin verilen eylemleri tanımlar.
services: security-center
cloud: na
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: 0e60e782fa65cd5868bebe081673f9a158e07799
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921326"
---
# <a name="permissions-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde İzinler

Azure Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../role-based-access-control/built-in-roles.md) sağlayan [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../role-based-access-control/role-assignments-portal.md) kullanır.

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını tanımlamak için kaynaklarınızın yapılandırmasını değerlendirir. Güvenlik Merkezi'nde, yalnızca kaynağın ait olduğu abonelik veya kaynak grubu için Sahip, Katılımcı veya Okuyucu rolü atandığında kaynakla ilgili bilgileri görürsünüz.

Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

* **Güvenlik Okuyucu :** Bu role ait bir kullanıcı güvenlik merkezinin görüntüleme haklarına sahiptir. Kullanıcı önerileri, uyarıları, güvenlik ilkesini ve güvenlik durumlarını görüntüleyebilir, ancak değişiklik yapamaz.
* **Güvenlik Yöneticisi**: Bu role ait bir kullanıcı Güvenlik Okuyucusu ile aynı haklara sahiptir ve güvenlik ilkesini güncelleyebilir ve uyarıları ve önerileri reddedebilir.

> [!NOTE]
> Güvenlik Yöneticisi ve Güvenlik Yöneticisi rolleri yalnızca Güvenlik Merkezi'nde erişebilir. Güvenlik rollerinin Azure'un Depolama, Web & Mobil veya Nesnelerin İnterneti gibi diğer hizmet alanlarına erişimi yoktur.
>
>

## <a name="roles-and-allowed-actions"></a>Roller ve izin verilen eylemler

Aşağıdaki tablo, Güvenlik Merkezi'nde rolleri ve izin verilen eylemleri görüntüler.

| Rol | Güvenlik ilkesini edin | Kaynak için güvenlik önerileri uygulama</br> ('Hızlı Düzeltme dahil!') | Uyarıları ve önerileri reddetme | Uyarıları ve önerileri görüntüleme |
|:--- |:---:|:---:|:---:|:---:|
| Abonelik Sahibi | ✔ | ✔ | ✔ | ✔ |
| Abonelik Katılımcısı | -- | ✔ | ✔ | ✔ |
| Kaynak Grubu Sahibi | -- | ✔ | -- | ✔ |
| Kaynak Grubu Katılımcısı | -- | ✔ | -- | ✔ |
| Okuyucu | -- | -- | -- | ✔ |
| Güvenlik Yöneticisi | ✔ | -- | ✔ | ✔ |
| Güvenlik Okuyucu | -- | -- | -- | ✔ |

> [!NOTE]
> Kullanıcılara, görevlerini tamamlamak için gereken rolleri en alt seviyede esneklik sunacak şekilde atamanızı öneririz. Örneğin, Okuyucu rolünü yalnızca bir kaynağın güvenlik durumu yla ilgili bilgileri görüntülemesi gereken ancak öneriler uygulama veya ilkeleri düzenleme gibi eylemde bulunmayan kullanıcılara atayın.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nin kullanıcılara izin atamak için RBAC'ı nasıl kullandığı açıklanmıştır ve her rol için izin verilen eylemleri tanımlamıştır. Artık aboneliğinizin güvenlik durumunu izlemek, güvenlik ilkelerini yeniden dizin ve öneriler uygulamak için gereken rol atamalarına aşina olduğunuza göre, nasıl yapılacağını öğrenin:

- [Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md)
- [Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md)
- [Azure kaynaklarınızın güvenlik durumunu izleme](security-center-monitoring.md)
- [Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
- [İş ortağı güvenlik çözümlerini izleyin](security-center-partner-solutions.md)
