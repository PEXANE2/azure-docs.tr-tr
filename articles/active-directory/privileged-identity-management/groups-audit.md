---
title: Privileged Identity Management (PıM)-Azure AD | ' de ayrıcalıklı erişim grubu atamaları için denetim raporunu görüntüleme Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içindeki ayrıcalıklı erişim grubu atamaları için etkinlik ve denetim geçmişini görüntüleyin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: pim
ms.date: 07/27/2020
ms.author: curtand
ms.reviewer: shaunliu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94dd5c2579eb7ce96dea70c6354df8ec84125bd9
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88141678"
---
# <a name="audit-activity-history-for-privileged-access-group-assignments-preview-in-privileged-identity-management"></a>Privileged Identity Management 'da ayrıcalıklı erişim grubu atamaları (Önizleme) için denetim etkinliği geçmişi

Privileged Identity Management (PıM) ile, Azure Active Directory (Azure AD) kuruluşunuzdaki Azure ayrıcalıklı erişim grubu üyeleri ve sahipleri için etkinlik, etkinleştirmeler ve denetim geçmişini görüntüleyebilirsiniz.

> [!NOTE]
> Kuruluşunuzun, [Azure tarafından yetkilendirilen kaynak yönetimi](../../lighthouse/concepts/azure-delegated-resource-management.md)kullanan bir hizmet sağlayıcısına dış kaynaklı yönetim işlevleri varsa, bu hizmet sağlayıcısı tarafından yetkilendirilen rol atamaları burada gösterilmez.

Ayrıcalıklı erişim gruplarının denetim geçmişini görüntülemek için bu adımları izleyin.

## <a name="view-resource-audit-history"></a>Kaynak denetim geçmişini görüntüle

**Kaynak denetimi** , ayrıcalıklı erişim gruplarınız ile ilişkili tüm etkinliklerin bir görünümünü sağlar.

1. **Azure AD Privileged Identity Management**açın.

1. **Ayrıcalıklı erişim grupları (Önizleme)** seçeneğini belirleyin.

1. Denetim geçmişini görüntülemek istediğiniz ayrıcalıklı erişim grubunu seçin.

1. **Etkinlik**' ın altında **kaynak denetimi**' ni seçin.

1. Geçmişi önceden tanımlanmış bir tarih veya özel Aralık kullanarak filtreleyin.

    ![Filtrelerle kaynak denetim listesi](media/groups-audit/groups-resource-audit.png)

## <a name="view-my-audit"></a>Denetmi görüntüle

**My Audit** , ayrıcalıklı bir erişim grubu için kişisel rol etkinliğinizi görüntülemenize olanak sağlar.

1. **Azure AD Privileged Identity Management**açın.

1. **Ayrıcalıklı erişim grupları (Önizleme)** seçeneğini belirleyin.

1. Denetim geçmişini görüntülemek istediğiniz ayrıcalıklı erişim grubunu seçin.

1. **Etkinlik**' ın altında **Hesabım**' ı seçin.

1. Geçmişi önceden tanımlanmış bir tarih veya özel Aralık kullanarak filtreleyin.

    ![Geçerli Kullanıcı için denetim listesi](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management grup üyeliği ve sahipliği atama](groups-assign-member-owner.md)
- [PıM 'de ayrıcalıklı erişim grupları için istekleri onaylama veya reddetme](groups-approval-workflow.md)
- [PıM 'de Azure AD rollerinin denetim geçmişini görüntüleme](groups-audit.md)
