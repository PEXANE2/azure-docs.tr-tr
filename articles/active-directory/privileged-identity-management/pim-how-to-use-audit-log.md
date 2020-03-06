---
title: Azure AD PıM 'de Azure AD rolleri için denetim günlüğü raporunu görüntüleme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rolleri için denetim günlüğü geçmişini görüntülemeyi öğrenin.
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
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78329535"
---
# <a name="view-audit-history-for-azure-ad-roles-in-privileged-identity-management"></a>Privileged Identity Management Azure AD rolleri için denetim geçmişini görüntüleme

Tüm ayrıcalıklı roller için son 30 gün içinde tüm rol atamalarını ve etkinleştirmeleri görmek üzere Privileged Identity Management (PıM) denetim geçmişini kullanabilirsiniz. Yönetici, son kullanıcı ve eşitleme etkinliği dahil olmak üzere Azure Active Directory (Azure AD) kuruluşunuzda etkinliğin tam denetim geçmişini görmek isterseniz, [Azure Active Directory güvenlik ve etkinlik raporlarını](../reports-monitoring/overview-reports.md)kullanabilirsiniz.

## <a name="determine-your-version-of-pim"></a>PıM sürümünüzü belirleme

2019 Kasım 'Dan başlayarak Privileged Identity Management Azure AD rolleri bölümü, Azure Kaynak rolleri deneyimleriyle eşleşen yeni bir sürüme güncelleştiriliyor. Bu, ek özellikleri [ve var olan API üzerinde yapılan değişiklikleri](azure-ad-roles-features.md#api-changes)de oluşturur. Yeni sürüm kullanıma sunulurken, bu makalede izlediğiniz yordamlar Şu anda sahip olduğunuz Privileged Identity Management sürümüne bağlıdır. Hangi Privileged Identity Management sürümünü istediğinizi öğrenmek için bu bölümdeki adımları izleyin. Privileged Identity Management Sürümünüzü öğrendikten sonra bu makaledeki sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünde olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Azure AD Privileged Identity Management**açın. Genel Bakış sayfasının üst kısmında yer alan bir başlık varsa, bu makalenin **Yeni sürüm** sekmesinde yer alan yönergeleri izleyin. Aksi takdirde, **önceki sürüm** sekmesindeki yönergeleri izleyin.

    [![Azure AD rolleri yeni sürüm](media/pim-how-to-use-audit-log/directory-roles-audit-history.png "Sürümünüz için sekmeyi seçin")](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

# <a name="new-version"></a>[Yeni sürüm](#tab/new)

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

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

## <a name="view-audit-history"></a>Denetim geçmişini görüntüleme

Azure AD rollerinin denetim geçmişini görüntülemek için bu adımları izleyin.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünün üyesi olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ni seçin.

1. **Dizin rolleri denetim geçmişi**' ni seçin.

    Denetim geçmişinize bağlı olarak, toplam etkinleştirmeler, günde maksimum etkinleştirme ve günlük ortalama etkinleştirme ile birlikte bir sütun grafiği görüntülenir.

    [![Azure AD rolleri yeni sürüm](media/pim-how-to-use-audit-log/directory-roles-audit-history.png "Dizin rollerinin denetim geçmişini görüntüle")](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

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

## <a name="get-reason-approver-and-ticket-number-for-approval-events"></a>Onay olayları için neden, onaylayan ve bilet numarası alın

1. Ayrıcalıklı rol yönetici rolü izinleriyle [Azure Portal](https://aad.portal.azure.com) oturum açın ve Azure AD ' yi açın.
1. **Denetim günlükleri**' ni seçin.
1. Yalnızca ayrıcalıklı kimlik yönetimi hizmeti için denetim olaylarını göstermek üzere **hizmet** filtresini kullanın. **Denetim günlükleri** sayfasında şunları yapabilirsiniz:

    - **Durum nedeni** sütununda bir denetim olayının nedenine bakın.
    - "Rol isteği için üye Ekle" olayı için **başlatan (aktör)** sütunundaki onaylayana bakın.

    [![Azure AD rolleri yeni sürüm](media/pim-how-to-use-audit-log/filter-audit-logs.png "PıM hizmeti için denetim günlüğünü filtrele")](media/pim-how-to-use-audit-log/filter-audit-logs.png)

1. **Ayrıntılar** bölmesinin **etkinlik** sekmesinde bilet numarasını görmek için bir denetim günlüğü olayı seçin.
  
    [![Azure AD rolleri yeni sürüm](media/pim-how-to-use-audit-log/audit-event-ticket-number.png "Denetim olayının bilet numarasını denetleyin")](media/pim-how-to-use-audit-log/audit-event-ticket-number.png)

1. Bir denetim olayının **Ayrıntılar** bölmesinin **hedefler** sekmesinde istek sahibine (rolü etkinleştiren kişi) görüntüleyebilirsiniz. Azure AD rolleri için iki hedef türü vardır:

    - Rol (**Type** = Role)
    - İstek sahibi (**tür** = Kullanıcı)

Genellikle, onay olayının hemen üzerindeki denetim günlüğü olayı, **başlatılan (aktör)** istek sahibi olduğu "role üye ekleme tamamlandı" için bir olaydır. Çoğu durumda, istek sahibine bir denetim perspektifinden onay isteğinde bulmak zorunda kalmazsınız.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure Kaynak rolleri için etkinlik ve denetim geçmişini görüntüleme](azure-pim-resource-rbac.md)
