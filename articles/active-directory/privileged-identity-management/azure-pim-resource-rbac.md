---
title: Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynak rolleri için denetim raporunu görüntüleyin - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynak rolleri için etkinlik ve denetim geçmişini görüntüleyin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: pim
ms.date: 01/10/2020
ms.author: curtand
ms.reviewer: shaunliu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4bf65ad595fb1ab70eb6613b6d54ac2a4f69141e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78329645"
---
# <a name="view-activity-and-audit-history-for-azure-resource-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri için etkinlik ve denetim geçmişini görüntüleme

Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM) ile, kuruluşunuzdaki Azure kaynakları rolleri için etkinliği, etkinleştirmeleri ve denetim geçmişini görüntüleyebilirsiniz. Buna abonelikler, kaynak grupları ve hatta sanal makineler dahildir. Azure portalında, Azure rol tabanlı erişim denetimi işlevinden yararlanan tüm kaynaklar, Ayrıcalıklı Kimlik Yönetimi'ndeki güvenlik ve yaşam döngüsü yönetimi özelliklerinden yararlanabilir.

> [!NOTE]
> Kuruluşunuz, [Azure temsilcili kaynak yönetimini](../../lighthouse/concepts/azure-delegated-resource-management.md)kullanan bir hizmet sağlayıcısına dış kaynak yönetim işlevleri veriyorsa, bu hizmet sağlayıcısı tarafından yetkilendirilen rol atamaları burada gösterilmez.

## <a name="view-activity-and-activations"></a>Etkinliği ve etkinleştirmeleri görüntüleme

Belirli bir kullanıcının çeşitli kaynaklarda hangi eylemleri gerçekleştirdiğini görmek için, belirli bir etkinleştirme dönemiyle ilişkili Azure kaynak etkinliğini görüntüleyebilirsiniz.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure kaynaklarını**seçin.

1. Etkinliği ve etkinleştirmeleri görüntülemek istediğiniz kaynağı seçin.

1. **Roller** veya **Üyeler'i**seçin.

1. Bir kullanıcı seçin.

    Kullanıcının Azure kaynaklarındaki eylemlerinin bir özetini tarihe göre görürsünüz. Ayrıca, aynı zaman diliminde son rol etkinleştirmelerini de gösterir.

    ![Kaynak etkinlik özeti ve rol etkinleştirmeleri ile kullanıcı ayrıntıları](media/azure-pim-resource-rbac/rbac-user-details.png)

1. Ayrıntıları ve kullanıcı etkinken oluşan ilgili Azure kaynak etkinliğini görmek için belirli bir rol etkinleştirme seçin.

    [![Rol etkinleştirme seçili ve etkinlik ayrıntıları](media/azure-pim-resource-rbac/export-membership.png "Rol etkinleştirme seçili ve etkinlik ayrıntıları")](media/azure-pim-resource-rbac/export-membership.png)

## <a name="export-role-assignments-with-children"></a>Çocuklarla rol atamalarını dışa aktarma

Denetçilere rol atamalarının tam listesini sağlamanız gereken bir uyumluluk gereksiniminolabilir. Ayrıcalıklı Kimlik Yönetimi, tüm alt kaynaklar için rol atamaları içeren belirli bir kaynakta rol atamaları sorgulamanızı sağlar. Daha önce, yöneticilerin bir abonelik için rol atamalarının tam listesini almak zordu ve her bir kaynak için rol atamaları dışa aktarmak zorundaydılar. Ayrıcalıklı Kimlik Yönetimi'ni kullanarak, tüm kaynak grupları ve kaynaklar için rol atamaları da dahil olmak üzere bir abonelikteki tüm etkin ve uygun rol atamaları için sorgu yapabilirsiniz.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure kaynaklarını**seçin.

1. Abonelik gibi rol atamaları için dışa aktarmak istediğiniz kaynağı seçin.

1. **Üyeler**’i seçin.

1. Dışa Aktarma üyelik bölmesini açmak için **Dışa** Aktarma'yı seçin.

    [![Tüm üyeleri dışa aktarmak için üyelik bölmesini dışa aktarma](media/azure-pim-resource-rbac/export-membership.png "Tüm üyeleri dışa aktarmak için üyelik sayfasını dışa aktarma")](media/azure-pim-resource-rbac/export-membership.png)

1. CSV dosyasındaki tüm rol atamalarını dışa aktarmak için **tüm üyeleri dışa** aktar'ı seçin.

    ![CSV dosyasındaki rol atamaları Excel'de görüntü olarak dışa aktar](media/azure-pim-resource-rbac/export-csv.png)

## <a name="view-resource-audit-history"></a>Kaynak denetim geçmişini görüntüleme

Kaynak denetimi, bir kaynak için tüm rol etkinliğinin görünümünü sağlar.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure kaynaklarını**seçin.

1. Denetim geçmişini görüntülemek istediğiniz kaynağı seçin.

1. **Kaynak denetimi'ni**seçin.

1. Önceden tanımlanmış bir tarih veya özel aralık kullanarak geçmişi filtreleyin.

    [![Filtrelerle kaynak denetim listesi](media/azure-pim-resource-rbac/rbac-resource-audit.png "Filtrelerle kaynak denetim listesi")](media/azure-pim-resource-rbac/rbac-resource-audit.png)

1. **Denetim türü için**Etkinleştir **(Atanmış + Etkin)** seçeneğini belirleyin.

    Denetim [![türünü etkinleştirme](media/azure-pim-resource-rbac/rbac-audit-activity.png "Etkinleştir'e göre filtrelenen kaynak denetim listesi")](media/azure-pim-resource-rbac/rbac-audit-activity.png) ![denetim türüne göre filtrelenen kaynak denetim listesi](media/azure-pim-resource-rbac/rbac-audit-activity.png)

1. **Eylem**altında, bir kullanıcının bu kullanıcının etkinlik ayrıntılarını Azure kaynaklarında görmesi için **(etkinlik)** tıklatın.

    ![Belirli bir eylem için kullanıcı etkinliği ayrıntıları](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

## <a name="view-my-audit"></a>Denetimimi görüntüleme

Denetimim kişisel rol aktivitenizi görüntülemenizi sağlar.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure kaynaklarını**seçin.

1. Denetim geçmişini görüntülemek istediğiniz kaynağı seçin.

1. **Denetimimi**seçin.

1. Önceden tanımlanmış bir tarih veya özel aralık kullanarak geçmişi filtreleyin.

    [![Geçerli kullanıcı için denetim listesi](media/azure-pim-resource-rbac/my-audit-time.png "Geçerli kullanıcı için denetim listesi")](media/azure-pim-resource-rbac/my-audit-time.png)

> [!NOTE]
> Denetim geçmişine erişim için Global Administrator veya Ayrıcalıklı Rol Yöneticisi rolü gerektirir.

## <a name="get-reason-approver-and-ticket-number-for-approval-events"></a>Onay etkinlikleri için neden, onaylayıcı ve bilet numarası alma

1. Ayrıcalıklı Rol yöneticisi rol izinleriyle [Azure portalında](https://aad.portal.azure.com) oturum açın ve Azure AD'yi açın.
1. **Denetim günlükleri**'ni seçin.
1. Yalnızca Ayrıcalıklı kimlik yönetimi hizmeti için denetim olaylarını görüntülemek için **Hizmet** filtresini kullanın. Denetim **günlükleri** sayfasında şunları yapabilirsiniz:

    - **Durum nedeni** sütununda bir denetim olayının nedenini görün.
    - "Rolün onaylanmasına üye ekle" olayı için **Başlatılan (aktör)** sütunundaki onaylayıcıya bakın.

    [![PIM hizmeti için denetim günlüğünü filtreleyin](media/azure-pim-resource-rbac/filter-audit-logs.png "PIM hizmeti için denetim günlüğünü filtreleyin")](media/azure-pim-resource-rbac/filter-audit-logs.png)

1. **Ayrıntılar** bölmesinin **Etkinlik** sekmesinde bilet numarasını görmek için bir denetim günlüğü olayı seçin.
  
    [![Denetim olayı için bilet numarasını kontrol edin](media/azure-pim-resource-rbac/audit-event-ticket-number.png "Denetim etkinliği için bilet numarasını kontrol edin")](media/azure-pim-resource-rbac/audit-event-ticket-number.png)]

1. Bir denetim olayı için **Ayrıntılar** bölmesinin **Hedefler** sekmesinde istekte bulundurucuyu (rolü etkinleştiren kişi) görüntüleyebilirsiniz. Azure kaynak rolleri için üç hedef türü vardır:

    - Rol (**Type** = Role)
    - İstekli (**Type** = Other)
    - Onaylayan (**Type** = Kullanıcı)

    [![Hedef türünü kontrol edin](media/azure-pim-resource-rbac/audit-event-target-type.png "Hedef türünü kontrol edin")](media/azure-pim-resource-rbac/audit-event-target-type.png)

Genellikle, onay olayının hemen üstündeki günlük olayı, **başlatılan (aktör)** tarafından başlatılan "Üyeyi tamamlanan role ekle" olayıdır. Çoğu durumda, denetim açısından onay isteğinde istekte bulunanı bulmanız gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerini atama](pim-resource-roles-assign-roles.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri için istekleri onaylama veya reddetme](pim-resource-roles-approval-workflow.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rolleri için denetim geçmişini görüntüleme](pim-how-to-use-audit-log.md)
