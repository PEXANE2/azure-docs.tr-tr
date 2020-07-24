---
title: Azure Danışmanı izinleri
description: Danışman izinleri ve bunların abonelikleri yapılandırma veya önerileri erteleme veya kapatma yeteneğinizi nasıl engelleyebilecekleri.
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 6b5dfdeb888b0577a40562536bd7a6dfc271cb19
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87057587"
---
# <a name="permissions-in-azure-advisor"></a>Azure Danışmanı izinleri

Azure Danışmanı, Azure kaynaklarınızın ve aboneliklerinizin kullanımını ve yapılandırmasını temel alan öneriler sağlar. Danışman, önerilere ve danışman özelliklerine erişiminizi yönetmek için [rol tabanlı Access Control](../role-based-access-control/overview.md) (RBAC) tarafından sunulan [yerleşik rolleri](../role-based-access-control/built-in-roles.md) kullanır. 

## <a name="roles-and-their-access"></a>Roller ve erişimleri

Aşağıdaki tabloda, Advisor içindeki roller ve erişim tanımları tanımlanmaktadır:

| **Role** | **Önerileri görüntüleme** | **Kuralları Düzenle** | **Abonelik yapılandırmasını düzenle** | **Kaynak grubu yapılandırmasını düzenle**| **Önerileri kapat ve ertele**|
|---|:---:|:---:|:---:|:---:|:---:|
|Abonelik sahibi|**Sayı**|**Sayı**|**Sayı**|**Sayı**|**Sayı**|
|Abonelik Katılımcısı|**Sayı**|**Sayı**|**Sayı**|**Sayı**|**Sayı**|
|Abonelik okuyucusu|**Sayı**|--|--|--|--|
|Kaynak grubu sahibi|**Sayı**|--|--|**Sayı**|**Sayı**|
|Kaynak grubu Katılımcısı|**Sayı**|--|--|**Sayı**|**Sayı**|
|Kaynak grubu okuyucusu|**Sayı**|--|--|--|--|
|Kaynak sahibi|**Sayı**|--|--|--|**Sayı**|
|Kaynak Katılımcısı|**Sayı**|--|--|--|**Sayı**|
|Kaynak okuyucu|**Sayı**|--|--|--|--|

> [!NOTE]
> Görüntüleme önerilerine erişim, önerinin etkilenen kaynağına erişime bağımlıdır.

## <a name="permissions-and-unavailable-actions"></a>İzinler ve kullanılamayan eylemler

Uygun izinlerin bulunmaması, Advisor 'daki eylemleri gerçekleştirme yeteneğinizi engelleyebilir. Yaygın olarak karşılaşılan bazı sorunlar aşağıda verilmiştir.

### <a name="unable-to-configure-subscriptions-or-resource-groups"></a>Abonelikler veya kaynak grupları yapılandırılamıyor

Danışman 'de abonelikleri veya kaynak gruplarını yapılandırmaya çalıştığınızda, dahil etme veya hariç tutma seçeneğinin devre dışı bırakıldığını görebilirsiniz. Bu durum, söz konusu kaynak grubu veya abonelik için yeterli izinlere sahip değilsiniz anlamına gelir. Bu sorunu çözmek için, [Kullanıcı erişimi verme](../role-based-access-control/quickstart-assign-role-user-portal.md)hakkında bilgi edinin.

### <a name="unable-to-postpone-or-dismiss-a-recommendation"></a>Öneri ertelenemiyor veya kapatılamıyor

Bir öneriyi ertelemeyi denerken bir hata alırsanız, yeterli izne sahip olmayabilirsiniz. Ertelediğinizden veya disemin olduğunuz önerinin etkilenen kaynağına en az katkıda bulunan erişiminiz olduğundan emin olun. Bu sorunu çözmek için, [Kullanıcı erişimi verme](../role-based-access-control/quickstart-assign-role-user-portal.md)hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Advisor 'ın Kullanıcı izinlerini denetlemek için RBAC kullanma ve genel sorunları çözme konusunda genel bakış sunulmaktadır. Advisor hakkında daha fazla bilgi edinmek için bkz.:

- [Azure Danışmanı nedir?](./advisor-overview.md)
- [Azure Advisor’ı kullanmaya başlama](./advisor-get-started.md)
