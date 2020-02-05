---
title: PıM-Azure AD 'de Azure AD rolleri için denetim raporunu görüntüleme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rollerinin denetim geçmişini görüntülemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/13/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d46036efa04b4e0225cad6e8a70cd31ad3c10bd
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024186"
---
# <a name="view-audit-history-for-azure-ad-roles-in-pim"></a>PıM 'de Azure AD rollerinin denetim geçmişini görüntüleme

Tüm ayrıcalıklı roller için son 30 gün içinde tüm rol atamalarını ve etkinleştirmeleri görmek üzere Privileged Identity Management (PıM) denetim geçmişini kullanabilirsiniz. Yönetici, son kullanıcı ve eşitleme etkinliği dahil olmak üzere Azure Active Directory (Azure AD) kuruluşunuzda etkinliğin tam denetim geçmişini görmek isterseniz, [Azure Active Directory güvenlik ve etkinlik raporlarını](../reports-monitoring/overview-reports.md)kullanabilirsiniz.

## <a name="determine-your-version-of-pim"></a>PıM sürümünüzü belirleme

2019 Kasım 'Dan başlayarak Privileged Identity Management Azure AD rolleri bölümü, Azure Kaynak rolleri deneyimleriyle eşleşen yeni bir sürüme güncelleştiriliyor. Bu, ek özellikleri [ve var olan API üzerinde yapılan değişiklikleri](azure-ad-roles-features.md#api-changes)de oluşturur. Yeni sürüm kullanıma sunulurken, bu makalede izlediğiniz yordamlar Şu anda sahip olduğunuz Privileged Identity Management sürümüne bağlıdır. Hangi Privileged Identity Management sürümünü istediğinizi öğrenmek için bu bölümdeki adımları izleyin. Privileged Identity Management Sürümünüzü öğrendikten sonra bu makaledeki sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünde olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Azure AD Privileged Identity Management**açın. Genel Bakış sayfasının üst kısmında yer alan bir başlık varsa, bu makalenin **Yeni sürüm** sekmesinde yer alan yönergeleri izleyin. Aksi takdirde, **önceki sürüm** sekmesindeki yönergeleri izleyin.

    ![Azure AD rolleri yeni sürüm](./media/pim-how-to-add-role-to-user/pim-new-version.png)

# <a name="new-versiontabnew"></a>[Yeni sürüm](#tab/new)

Azure AD rollerinin denetim geçmişini görüntülemek için bu adımları izleyin.

## <a name="view-resource-audit-history"></a>Kaynak denetim geçmişini görüntüle

Kaynak denetimi size Azure AD rolleriniz ile ilişkili tüm etkinliklerin bir görünümünü sunar.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ni seçin.

1. **Kaynak denetimi**' ni seçin.

1. Geçmişi önceden tanımlanmış bir tarih veya özel Aralık kullanarak filtreleyin.

    ![Filtrelerle kaynak denetim listesi](media/azure-pim-resource-rbac/rbac-resource-audit.png)

## <a name="view-my-audit"></a>Denetmi görüntüle

My Audit, kişisel rol etkinliğinizi görüntülemenize olanak sağlar.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ni seçin.

1. Denetim geçmişini görüntülemek istediğiniz kaynağı seçin.

1. **Denetmi**seçin.

1. Geçmişi önceden tanımlanmış bir tarih veya özel Aralık kullanarak filtreleyin.

    ![Geçerli Kullanıcı için denetim listesi](media/azure-pim-resource-rbac/my-audit-time.png)

# <a name="previous-versiontabprevious"></a>[Önceki sürüm](#tab/previous)

## <a name="view-audit-history"></a>Denetim geçmişini görüntüleme

Azure AD rollerinin denetim geçmişini görüntülemek için bu adımları izleyin.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünün üyesi olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ni seçin.

1. **Dizin rolleri denetim geçmişi**' ni seçin.

    Denetim geçmişinize bağlı olarak, toplam etkinleştirmeler, günde maksimum etkinleştirme ve günlük ortalama etkinleştirme ile birlikte bir sütun grafiği görüntülenir.

    ![Dizin rolleri denetim geçmişi](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

    Sayfanın en altında, kullanılabilir denetim geçmişinde her eylem hakkında bilgi içeren bir tablo görüntülenir. Sütunlar aşağıdaki anlamlara sahiptir:

    | Sütun | Açıklama |
    | --- | --- |
    | Zaman | Eylem oluştuğunda. |
    | Sahibinin | Rol etkinleştirmeyi veya değiştirmeyi isteyen kullanıcı. Değer **Azure sistemise**daha fazla bilgi için Azure denetim geçmişini kontrol edin. |
    | Eylem | İstek sahibi tarafından gerçekleştirilen eylemler. Eylemler, atama, atamayı kaldırma, etkinleştirme, devre dışı bırakma veya AddedOutsidePIM içerebilir. |
    | Üye | Rolü etkinleştiren veya bir role atanan kullanıcı. |
    | Rol | Kullanıcı tarafından atanan veya etkinleştirilen rol. |
    | Mantık yürütme | Etkinleştirme sırasında neden alanına girilen metin. |
    | Dolmadan | Etkinleştirilen bir rolün süresi dolar. Yalnızca uygun rol atamaları için geçerlidir. |

1. Denetim geçmişini sıralamak için **saat**, **eylem**ve **rol** düğmelerine tıklayın.

## <a name="filter-audit-history"></a>Denetim geçmişini filtrele

1. Denetim geçmişi sayfasının en üstünde bulunan **filtre** düğmesine tıklayın.

    **Grafik parametrelerini Güncelleştir** bölmesi görüntülenir.

1. **Zaman aralığı**' nda bir zaman aralığı seçin.

1. **Roller**' de, görüntülemek istediğiniz rolleri belirtmek için onay kutularını seçin.

    ![Grafik parametreleri bölmesini Güncelleştir](media/pim-how-to-use-audit-log/update-chart-parameters.png)

1. Filtrelenmiş denetim geçmişini görüntülemek için **bitti** ' yi seçin.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure Kaynak rolleri için etkinlik ve denetim geçmişini görüntüleme](azure-pim-resource-rbac.md)
