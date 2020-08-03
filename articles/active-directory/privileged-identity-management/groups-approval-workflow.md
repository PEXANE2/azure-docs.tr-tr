---
title: Privileged Identity Management-Azure AD 'de Grup üyeleri ve sahipler için etkinleştirme isteklerini onaylama
description: Azure AD Privileged Identity Management (PıM) içindeki rol atanabilir gruplar için istekleri onaylamayı veya reddetmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 07/27/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: b3fbb92c2e3623f5fd9571cd94ae521a41139dd6
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87505867"
---
# <a name="approve-activation-requests-for-privileged-access-group-members-and-owners-preview"></a>Ayrıcalıklı erişim grubu üyeleri ve sahipleri için etkinleştirme isteklerini onayla (Önizleme)

Azure Active Directory (Azure AD) içinde Privileged Identity Management (PıM) ile, ayrıcalıklı erişim grubu üyelerini ve sahiplerini etkinleştirme için onay isteyecek şekilde yapılandırabilir ve Azure AD kuruluşunuzdaki kullanıcıları veya grupları, temsilci onaylayan olarak seçebilirsiniz. Ayrıcalıklı rol yöneticisi için iş yükünü azaltmak üzere her grup için iki veya daha fazla onaylayan seçmenizi öneririz. Temsilci onaylamalardan isteklerin onaylaması 24 saati vardır. İstek 24 saat içinde onaylanmamışsa, uygun Kullanıcı yeni bir isteği yeniden göndermesi gerekir. 24 saat onay zaman penceresi yapılandırılamaz.

Azure Kaynak rolleri için istekleri onaylamak veya reddetmek için bu makaledeki adımları izleyin.

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüleme

Bir temsilci onaylayıcı olarak, Onayınızı bekleyen bir Azure Kaynak rolü isteği olduğunda bir e-posta bildirimi alırsınız. Bekleyen istekleri Privileged Identity Management görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Istekleri Onayla**' yı seçin.

    ![İstekleri onaylama-gözden geçirilecek isteği gösteren Azure kaynakları sayfası](./media/groups-approval-workflow/groups-select-request.png)

    **Rol etkinleştirmeleri Için istekler** bölümünde, Onayınızı bekleyen isteklerin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onaylama

1. Onaylamak istediğiniz isteği bulun ve seçin ve **Onayla**' yı seçin.

    ![İstekleri Onayla-Ayrıntılar ve bloklama kutusu ile onaylama veya reddetme bölmesi](./media/groups-approval-workflow/groups-confirm-approval.png)

1. **Gerekçe** kutusunda iş gerekçe ' nı girin.

1. **Onayla**seçeneğini belirleyin. Onayınız tarafından bir Azure bildirimi oluşturulur.

## <a name="deny-requests"></a>Reddetme istekleri

1. Reddetmek istediğiniz isteği bulun ve seçin ve **Reddet**' i seçin.

    ![İstekleri Onayla-Ayrıntılar ve bloklama kutusu ile onaylama veya reddetme bölmesi](./media/groups-approval-workflow/groups-confirm-denial.png)

1. **Gerekçe** kutusunda iş gerekçe ' nı girin.

1. **Onayla**seçeneğini belirleyin. Bir Azure bildirimi, reddetme tarafından oluşturulur.

## <a name="workflow-notifications"></a>İş akışı bildirimleri

Aşağıda, iş akışı bildirimleri hakkında bazı bilgiler verilmiştir:

- Bir grup atama isteği gözden geçireni beklerken, onaylayanlara e-posta gönderilir. E-posta bildirimleri isteğin doğrudan bir bağlantısını, onaylayanın onaylayabileceği veya reddedebileceği bir bağlantı içerir.
- İstekler onaylayan veya reddeden ilk onaylayan tarafından çözümlenir.
- Bir onaylayan isteğe yanıt verdiğinde, tüm onaylayanlara eylem bildirilir.

>[!Note]
>Onaylanan bir kullanıcının etkin olmaması gerektiğini düşündüğü bir yönetici, Privileged Identity Management içinde etkin grup atamasını kaldırabilir. Kaynak yöneticilerine, onaylayan olmadıkları müddetçe bekleyen istekler bildirilmese de, bekleyen istekleri Privileged Identity Management görüntüleyerek tüm kullanıcılar için bekleyen istekleri görüntüleyebilir ve iptal edebilirler.

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management grup atamalarını genişletme veya yenileme](pim-resource-roles-renew-extend.md)
- [Privileged Identity Management e-posta bildirimleri](pim-email-notifications.md)
- [Privileged Identity Management Grup atamaları için istekleri onaylama veya reddetme](azure-ad-pim-approval-workflow.md)
