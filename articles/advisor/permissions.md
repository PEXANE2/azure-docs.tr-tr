---
title: Azure Danışmanı'nda izinler
description: Danışman izinleri ve abonelikleri yapılandırma veya önerileri erteleme veya reddetme yeteneğinizi nasıl engelleyebilecekleri.
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: c850d757044066d5c4a793e076436906d715833c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422312"
---
# <a name="permissions-in-azure-advisor"></a>Azure Danışmanı'nda izinler

Azure Danışmanı, Azure kaynaklarınızın ve aboneliklerinizin kullanımına ve yapılandırmasını temel alan öneriler sunar. Danışman, önerilere ve Danışman özelliklerine erişiminizi yönetmek için [Role Tabanlı Erişim Denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview) (RBAC) tarafından sağlanan yerleşik [rolleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) kullanır. 

## <a name="roles-and-their-access"></a>Roller ve erişim

Aşağıdaki tablo, Advisor içindeki rolleri ve erişimlerini tanımlar:

| **Rol** | **Önerileri görüntüleme** | **Kuralları edin** | **Abonelik yapılandırması düzenleme** | **Kaynak grubu yapılandırması düzenleme**| **Önerileri reddetme ve erteleme**|
|---|:---:|:---:|:---:|:---:|:---:|
|Abonelik Sahibi|**X**|**X**|**X**|**X**|**X**|
|Abonelik Katılımcısı|**X**|**X**|**X**|**X**|**X**|
|Abonelik Okuyucu|**X**|--|--|--|--|
|Kaynak grubu Sahibi|**X**|--|--|**X**|**X**|
|Kaynak grubu Katılımcı|**X**|--|--|**X**|**X**|
|Kaynak grubu Okuyucu|**X**|--|--|--|--|
|Kaynak Sahibi|**X**|--|--|--|**X**|
|Kaynak Katkıda Bulunan|**X**|--|--|--|**X**|
|Kaynak Okuyucu|**X**|--|--|--|--|

> [!NOTE]
> Görüntüleme önerilerine erişim, önerinin etkilenen kaynağına erişiminize bağlıdır.

## <a name="permissions-and-unavailable-actions"></a>İzinler ve kullanılamayan eylemler

Uygun izinlerin olmaması, Advisor'da eylem gerçekleştirme yeteneğinizi engelleyebilir. Aşağıda bazı sık karşılaşılan sorunlar vardır.

### <a name="unable-to-configure-subscriptions-or-resource-groups"></a>Abonelikleri veya kaynak gruplarını yapılandıramıyor

Danışman'da abonelikleri veya kaynak gruplarını yapılandırmaya çalıştığınızda, ekleme veya hariç tutma seçeneğinin devre dışı bırakıldığını görebilirsiniz. Bu durum, bu kaynak grubu veya abonelik için yeterli izin düzeyine sahip olmadığını gösterir. Bu sorunu gidermek için, bir kullanıcıya nasıl [erişim veriş verişeceğinizi](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)öğrenin.

### <a name="unable-to-postpone-or-dismiss-a-recommendation"></a>Bir öneriyi erteleyemiyor veya görevden alamıyor

Bir öneriyi ertelemeye veya reddetmeye çalışırken bir hata alırsanız, yeterli izniniz olmayabilir. Ertelediğiniz veya iptal ettiğiniz tavsiyenin etkilenen kaynağına en az katkıda bulunana erişebildiğinizden emin olun. Bu sorunu gidermek için, bir kullanıcıya nasıl [erişim veriş verişeceğinizi](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Danışmanın kullanıcı izinlerini denetlemek için RBAC'ı nasıl kullandığı ve sık karşılaşılan sorunları nasıl çözeceği hakkında genel bir bakış vermiştir. Danışman hakkında daha fazla bilgi edinmek için bkz:

- [Azure Danışmanı nedir?](https://docs.microsoft.com/azure/advisor/advisor-overview)
- [Azure Advisor’ı kullanmaya başlama](https://docs.microsoft.com/azure/advisor/advisor-get-started)
