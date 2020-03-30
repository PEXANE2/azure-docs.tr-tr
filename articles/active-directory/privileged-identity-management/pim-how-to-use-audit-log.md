---
title: Azure AD PIM'deki Azure REKLAM rolleri için denetim günlüğü raporunu görüntüleyin | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure AD rolleri için denetim günlüğü geçmişini nasıl görüntüleyebilirsiniz öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 01/07/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4b8aef68e0f61e6ca995fc2bb362d59aba73ead2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78329535"
---
# <a name="view-audit-history-for-azure-ad-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rolleri için denetim geçmişini görüntüleme

Tüm ayrıcalıklı roller için son 30 gün içindeki tüm rol atamalarını ve etkinleştirmeleri görmek için Ayrıcalıklı Kimlik Yönetimi (PIM) denetim geçmişini kullanabilirsiniz. Yönetici, son kullanıcı ve eşitleme etkinliği de dahil olmak üzere Azure Active Directory (Azure AD) kuruluşunuzdaki etkinliğin tam denetim geçmişini görmek istiyorsanız, [Azure Etkin Dizin güvenlik ve etkinlik raporlarını](../reports-monitoring/overview-reports.md)kullanabilirsiniz.

## <a name="determine-your-version-of-pim"></a>PIM sürümünüzü belirleyin

Kasım 2019'dan itibaren, Ayrıcalıklı Kimlik Yönetimi'nin Azure AD rolleri bölümü, Azure kaynak rollerinin deneyimleriyle eşleşen yeni bir sürüme güncelleniyor. Bu, [varolan API'de değişikliklerin](azure-ad-roles-features.md#api-changes)yanı sıra ek özellikler de oluşturur. Yeni sürüm kullanıma alınırken, bu makalede hangi yordamları izleyeceğiniz, şu anda sahip olduğunuz Ayrıcalıklı Kimlik Yönetimi sürümüne bağlıdır. Ayrıcalıklı Kimlik Yönetimi'nin hangi sürümüne sahip olduğunuzu belirlemek için bu bölümdeki adımları izleyin. Ayrıcalıklı Kimlik Yönetimi sürümünüzü aldıktan sonra, bu makalede bu sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünde olan bir kullanıcıyla Azure [portalında](https://portal.azure.com/) oturum açın.
1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın. Genel bakış sayfasının üst kısmında bir başlık varsa, bu makalenin **Yeni sürüm** sekmesindeki yönergeleri izleyin. Aksi takdirde, Önceki **sürüm** sekmesindeki yönergeleri izleyin.

    [![Azure AD rolleri yeni sürüm](media/pim-how-to-use-audit-log/directory-roles-audit-history.png "Sürümünüz için sekmeyi seçin")](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

# <a name="new-version"></a>[Yeni sürüm](#tab/new)

Azure AD rollerinin denetim geçmişini görüntülemek için aşağıdaki adımları izleyin.

## <a name="view-resource-audit-history"></a>Kaynak denetim geçmişini görüntüleme

Kaynak denetimi, Azure REKLAM rollerinizle ilişkili tüm etkinliklerin görünümünü sağlar.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

1. **Kaynak denetimi'ni**seçin.

1. Önceden tanımlanmış bir tarih veya özel aralık kullanarak geçmişi filtreleyin.

    ![Filtrelerle kaynak denetim listesi](media/azure-pim-resource-rbac/rbac-resource-audit.png)

## <a name="view-my-audit"></a>Denetimimi görüntüleme

Denetimim kişisel rol aktivitenizi görüntülemenizi sağlar.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

1. Denetim geçmişini görüntülemek istediğiniz kaynağı seçin.

1. **Denetimimi**seçin.

1. Önceden tanımlanmış bir tarih veya özel aralık kullanarak geçmişi filtreleyin.

    ![Geçerli kullanıcı için denetim listesi](media/azure-pim-resource-rbac/my-audit-time.png)

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

## <a name="view-audit-history"></a>Denetim geçmişini görüntüleme

Azure AD rollerinin denetim geçmişini görüntülemek için aşağıdaki adımları izleyin.

1. [Ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünün bir üyesi olan bir kullanıcıyla Azure [portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

1. **Dizin rolleri denetim geçmişini**seçin.

    Denetim geçmişinize bağlı olarak, bir sütun grafiği toplam etkinleştirmeler, günlük maksimum etkinleştirmeler ve günlük ortalama etkinleştirmelerle birlikte görüntülenir.

    [![Azure AD rolleri yeni sürüm](media/pim-how-to-use-audit-log/directory-roles-audit-history.png "Dizin rolleri denetim geçmişini görüntüleme")](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

    Sayfanın alt kısmında, kullanılabilir denetim geçmişindeki her eylem hakkında bilgi içeren bir tablo görüntülenir. Sütunların şu anlamları vardır:

    | Sütun | Açıklama |
    | --- | --- |
    | Zaman | Eylem oluştuğunda. |
    | Requestor | Rol etkinleştirme veya değiştirme talebinde bulunan kullanıcı. Değer Azure **Sistemi**ise, daha fazla bilgi için Azure denetim geçmişini kontrol edin. |
    | Eylem | İstekli tarafından yapılan işlemler. Eylemler Atama, Atama, Etkinleştirme, Devre Dışı Bırakma veya AddedOutsidePIM'i içerebilir. |
    | Üye | Etkinleştiren veya bir role atanan kullanıcı. |
    | Rol | Kullanıcı tarafından atanan veya etkinleştirilen rol. |
    | Akıl | Etkinleştirme sırasında neden alanına girilen metin. |
    | Bitiş tarihi | Etkinleştirilen bir rolün süresi dolduğunda. Yalnızca uygun rol atamaları için geçerlidir. |

1. Denetim geçmişini sıralamak için **Zaman,** **Eylem**ve **Rol** düğmelerini tıklatın.

## <a name="filter-audit-history"></a>Filtre denetim geçmişi

1. Denetim geçmişi sayfasının üst kısmında Filtre **düğmesini** tıklatın.

    **Grafik parametrelerini güncelleştir** bölmesi görüntülenir.

1. **Zaman aralığında,** bir zaman aralığı seçin.

1. **Roller'de,** görüntülemek istediğiniz rolleri belirtmek için onay kutularını seçin.

    ![Grafik parametreleri bölmesi güncelleştirme](media/pim-how-to-use-audit-log/update-chart-parameters.png)

1. Filtre uygulanmış denetim geçmişini görüntülemek için **Bitti'yi** seçin.

## <a name="get-reason-approver-and-ticket-number-for-approval-events"></a>Onay etkinlikleri için neden, onaylayıcı ve bilet numarası alma

1. Ayrıcalıklı Rol yöneticisi rol izinleriyle [Azure portalında](https://aad.portal.azure.com) oturum açın ve Azure AD'yi açın.
1. **Denetim günlükleri**'ni seçin.
1. Yalnızca Ayrıcalıklı kimlik yönetimi hizmeti için denetim olaylarını görüntülemek için **Hizmet** filtresini kullanın. Denetim **günlükleri** sayfasında şunları yapabilirsiniz:

    - **Durum nedeni** sütununda bir denetim olayının nedenini görün.
    - "Rolün onaylanmasına üye ekle" olayı için **Başlatılan (aktör)** sütunundaki onaylayıcıya bakın.

    [![Azure AD rolleri yeni sürüm](media/pim-how-to-use-audit-log/filter-audit-logs.png "PIM hizmeti için denetim günlüğünü filtreleyin")](media/pim-how-to-use-audit-log/filter-audit-logs.png)

1. **Ayrıntılar** bölmesinin **Etkinlik** sekmesinde bilet numarasını görmek için bir denetim günlüğü olayı seçin.
  
    [![Azure AD rolleri yeni sürüm](media/pim-how-to-use-audit-log/audit-event-ticket-number.png "Denetim etkinliği için bilet numarasını kontrol edin")](media/pim-how-to-use-audit-log/audit-event-ticket-number.png)

1. Bir denetim olayı için **Ayrıntılar** bölmesinin **Hedefler** sekmesinde istekte bulundurucuyu (rolü etkinleştiren kişi) görüntüleyebilirsiniz. Azure AD rolleri için iki hedef türü vardır:

    - Rol (**Type** = Role)
    - İstekli (**Type** = Kullanıcı)

Genellikle, onay olayının hemen üstündeki denetim günlüğü olayı, **başlatılan (aktör)** tarafından başlatılan "Üyeyi tamamlanan role ekle" olayıdır. Çoğu durumda, denetim açısından onay isteğinde istekte bulunanı bulmanız gerekmez.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri için etkinlik ve denetim geçmişini görüntüleme](azure-pim-resource-rbac.md)
