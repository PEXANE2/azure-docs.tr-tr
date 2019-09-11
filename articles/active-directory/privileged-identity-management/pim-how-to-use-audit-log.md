---
title: PıM-Azure Active Directory 'de Azure AD rollerinin denetim geçmişini görüntüleme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rollerinin denetim geçmişini görüntülemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/10/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3547ff218e9a15151e4abe2ceff53292d3f01ac0
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804315"
---
# <a name="view-audit-history-for-azure-ad-roles-in-pim"></a>PıM 'de Azure AD rollerinin denetim geçmişini görüntüleme

Tüm ayrıcalıklı roller için son 30 gün içinde tüm rol atamalarını ve etkinleştirmeleri görmek üzere Azure Active Directory (Azure AD) Privileged Identity Management (PıM) denetim geçmişini kullanabilirsiniz. Dizininizde, yönetici, son kullanıcı ve eşitleme etkinliği dahil olmak üzere etkinliğin tam denetim geçmişini görmek isterseniz, [Azure Active Directory güvenlik ve etkinlik raporlarını](../reports-monitoring/overview-reports.md)kullanabilirsiniz.

## <a name="view-audit-history"></a>Denetim geçmişini görüntüleme

Azure AD rollerinin denetim geçmişini görüntülemek için bu adımları izleyin.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünün üyesi olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ne tıklayın.

1. **Dizin rolleri denetim geçmişi**' ne tıklayın.

    Denetim geçmişinize bağlı olarak, toplam etkinleştirmeler, günde maksimum etkinleştirme ve günlük ortalama etkinleştirme ile birlikte bir sütun grafiği görüntülenir.

    ![Dizin rolleri denetim geçmişi](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

    Sayfanın en altında, kullanılabilir denetim geçmişinde her eylem hakkında bilgi içeren bir tablo görüntülenir. Sütunlar aşağıdaki anlamlara sahiptir:

    | Sütun | Açıklama |
    | --- | --- |
    | Time | Eylem oluştuğunda. |
    | İstek sahibi | Rol etkinleştirmeyi veya değiştirmeyi isteyen kullanıcı. Değer **Azure sistemise**daha fazla bilgi için Azure denetim geçmişini kontrol edin. |
    | Action | İstek sahibi tarafından gerçekleştirilen eylemler. Eylemler, atama, atamayı kaldırma, etkinleştirme, devre dışı bırakma veya AddedOutsidePIM içerebilir. |
    | Üyesi | Rolü etkinleştiren veya bir role atanan kullanıcı. |
    | Role | Kullanıcı tarafından atanan veya etkinleştirilen rol. |
    | Neden | Etkinleştirme sırasında neden alanına girilen metin. |
    | Süre sonu | Etkinleştirilen bir rolün süresi dolar. Yalnızca uygun rol atamaları için geçerlidir. |

1. Denetim geçmişini sıralamak için **saat**, **eylem**ve **rol** düğmelerine tıklayın.

## <a name="filter-audit-history"></a>Denetim geçmişini filtrele

1. Denetim geçmişi sayfasının en üstünde bulunan **filtre** düğmesine tıklayın.

    **Grafik parametrelerini Güncelleştir** bölmesi görüntülenir.

1. **Zaman aralığı**' nda bir zaman aralığı seçin.

1. **Roller**' de, görüntülemek istediğiniz roller için onay işaretleri ekleyin.

    ![Grafik parametreleri bölmesini Güncelleştir](media/pim-how-to-use-audit-log/update-chart-parameters.png)

1. Filtrelenmiş denetim geçmişini görüntülemek için **bitti** ' ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure Kaynak rolleri için etkinlik ve denetim geçmişini görüntüleme](azure-pim-resource-rbac.md)
