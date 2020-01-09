---
title: Azure Danışmanı izinleri
description: Danışman izinleri ve bunların abonelikleri yapılandırma veya önerileri erteleme veya kapatma yeteneğinizi nasıl engelleyebilecekleri.
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: c850d757044066d5c4a793e076436906d715833c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75422312"
---
# <a name="permissions-in-azure-advisor"></a>Azure Danışmanı izinleri

Azure Danışmanı, Azure kaynaklarınızın ve aboneliklerinizin kullanımını ve yapılandırmasını temel alan öneriler sağlar. Danışman, önerilere ve danışman özelliklerine erişiminizi yönetmek için [rol tabanlı Access Control](https://docs.microsoft.com/azure/role-based-access-control/overview) (RBAC) tarafından sunulan [yerleşik rolleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) kullanır. 

## <a name="roles-and-their-access"></a>Roller ve erişimleri

Aşağıdaki tabloda, Advisor içindeki roller ve erişim tanımları tanımlanmaktadır:

| **Rol** | **Önerileri görüntüle** | **Kuralları Düzenle** | **Abonelik yapılandırmasını düzenle** | **Kaynak grubu yapılandırmasını düzenle**| **Önerileri kapat ve ertele**|
|---|:---:|:---:|:---:|:---:|:---:|
|Abonelik sahibi|**X**|**X**|**X**|**X**|**X**|
|Abonelik Katılımcısı|**X**|**X**|**X**|**X**|**X**|
|Abonelik okuyucusu|**X**|--|--|--|--|
|Kaynak grubu sahibi|**X**|--|--|**X**|**X**|
|Kaynak grubu Katılımcısı|**X**|--|--|**X**|**X**|
|Kaynak grubu okuyucusu|**X**|--|--|--|--|
|Kaynak sahibi|**X**|--|--|--|**X**|
|Kaynak Katılımcısı|**X**|--|--|--|**X**|
|Kaynak okuyucu|**X**|--|--|--|--|

> [!NOTE]
> Görüntüleme önerilerine erişim, önerinin etkilenen kaynağına erişime bağımlıdır.

## <a name="permissions-and-unavailable-actions"></a>İzinler ve kullanılamayan eylemler

Uygun izinlerin bulunmaması, Advisor 'daki eylemleri gerçekleştirme yeteneğinizi engelleyebilir. Yaygın olarak karşılaşılan bazı sorunlar aşağıda verilmiştir.

### <a name="unable-to-configure-subscriptions-or-resource-groups"></a>Abonelikler veya kaynak grupları yapılandırılamıyor

Danışman 'de abonelikleri veya kaynak gruplarını yapılandırmaya çalıştığınızda, dahil etme veya hariç tutma seçeneğinin devre dışı bırakıldığını görebilirsiniz. Bu durum, söz konusu kaynak grubu veya abonelik için yeterli izinlere sahip değilsiniz anlamına gelir. Bu sorunu çözmek için, [Kullanıcı erişimi verme](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)hakkında bilgi edinin.

### <a name="unable-to-postpone-or-dismiss-a-recommendation"></a>Öneri ertelenemiyor veya kapatılamıyor

Bir öneriyi ertelemeyi denerken bir hata alırsanız, yeterli izne sahip olmayabilirsiniz. Ertelediğinizden veya disemin olduğunuz önerinin etkilenen kaynağına en az katkıda bulunan erişiminiz olduğundan emin olun. Bu sorunu çözmek için, [Kullanıcı erişimi verme](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Advisor 'ın Kullanıcı izinlerini denetlemek için RBAC kullanma ve genel sorunları çözme konusunda genel bakış sunulmaktadır. Advisor hakkında daha fazla bilgi edinmek için bkz.:

- [Azure Advisor nedir?](https://docs.microsoft.com/azure/advisor/advisor-overview)
- [Azure Advisor 'ı kullanmaya başlama](https://docs.microsoft.com/azure/advisor/advisor-get-started)
