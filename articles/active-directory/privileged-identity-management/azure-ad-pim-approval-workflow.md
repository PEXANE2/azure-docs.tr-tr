---
title: PIM'deki Azure REKLAM rolleri yle ilgili istekleri onaylama veya reddetme - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure AD rolleri için istekleri nasıl onaylayacağınızı veya reddlayacağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/07/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73ccfd1ad270072989e9b575fda538b94fd8927c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049009"
---
# <a name="approve-or-deny-requests-for-azure-ad-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rolleri için istekleri onaylama veya reddetme

Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM) ile rolleri etkinleştirme onayı gerektirecek şekilde yapılandırabilir ve bir veya birden çok kullanıcı veya grubu temsilci onaylayan olarak seçebilirsiniz. Temsilci onaylayanların istekleri onaylamak için 24 saatleri vardır. Bir istek 24 saat içinde onaylanmazsa, uygun kullanıcı yeni bir isteği yeniden göndermelidir. 24 saatlik onay süresi penceresi yapılandırılamaz.

## <a name="determine-your-version-of-pim"></a>PIM sürümünüzü belirleyin

Kasım 2019'dan itibaren, Ayrıcalıklı Kimlik Yönetimi'nin Azure AD rolleri bölümü, Azure rolleri için deneyimlerle eşleşen yeni bir sürüme güncelleniyor. Bu, [varolan API'de değişikliklerin](azure-ad-roles-features.md#api-changes)yanı sıra ek özellikler de oluşturur. Yeni sürüm kullanıma alınırken, bu makalede hangi yordamları izleyeceğiniz, şu anda sahip olduğunuz Ayrıcalıklı Kimlik Yönetimi sürümüne bağlıdır. Ayrıcalıklı Kimlik Yönetimi'nin hangi sürümüne sahip olduğunuzu belirlemek için bu bölümdeki adımları izleyin. Ayrıcalıklı Kimlik Yönetimi sürümünüzü aldıktan sonra, bu makalede bu sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünde olan bir kullanıcıyla Azure [portalında](https://portal.azure.com/) oturum açın.
1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın. Genel bakış sayfasının üst kısmında bir başlık varsa, bu makalenin **Yeni sürüm** sekmesindeki yönergeleri izleyin. Aksi takdirde, Önceki **sürüm** sekmesindeki yönergeleri izleyin.

    [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

Azure AD rolleri isteklerini onaylamak veya reddetmek için bu makaledeki adımları izleyin.

# <a name="new-version"></a>[Yeni sürüm](#tab/new)

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüleme

Temsilci onaylayıcısı olarak, Azure REKLAM rol isteği onayınızı beklerken bir e-posta bildirimi alırsınız. Bu bekleyen istekleri Ayrıcalıklı Kimlik Yönetimi'nde görüntüleyebilirsiniz.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **İstekleri Onayla'yı**seçin.

    ![İstekleri onaylayın - Azure REKLAM rollerini gözden geçirme isteğini gösteren sayfa](./media/azure-ad-pim-approval-workflow/resources-approve-pane.png)

    Rol **etkinleştirme talepleri** bölümünde, onayınızı bekleyen isteklerin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onaylama

1. Onaylamak istediğiniz isteği bulun ve seçin. Bir onay veya reddet sayfası görüntülenir.

    ![İstekleri onaylayın - ayrıntıları ve Yaslama kutusunu içeren bölmeyi onaylama veya reddetme](./media/azure-ad-pim-approval-workflow/resources-approve-pane.png)

1. **Yaslama** kutusuna, iş gerekçesini girin.

1. **Onayla'yı**seçin. Onayınıza ait bir Azure bildirimi alırsınız.

    ![İsteğin onaylandığını gösteren bildirimi onaylama](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png))

## <a name="deny-requests"></a>İstekleri reddetme

1. İnkar etmek istediğiniz isteği bulun ve seçin. Bir onay veya reddet sayfası görüntülenir.

    ![İstekleri onaylayın - ayrıntıları ve Yaslama kutusunu içeren bölmeyi onaylama veya reddetme](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. **Yaslama** kutusuna, iş gerekçesini girin.

1. **Reddet'i**seçin. Reddinizde bir bildirim görüntülenir.

## <a name="workflow-notifications"></a>İş akışı bildirimleri

İş akışı bildirimleri hakkında bazı bilgiler aşağıda veda edebilirsiniz:

- Bir rol isteği gözden geçirilmelerini beklerken onaylayanlar e-posta ile bildirilir. E-posta bildirimleri, onaylayanın onaylayacağı veya reddedebileceği isteğe doğrudan bir bağlantı içerir.
- İstekler, onaylayan veya reddeden ilk onaylayan tarafından çözülür.
- Bir onaylayıcı isteğe yanıt verdiğinde, tüm onaylayıcılar eylemden haberdar edilir.
- Onaylanan bir kullanıcı rollerinde etkin hale geldiğinde, genel yöneticiler ve Ayrıcalıklı rol yöneticileri bildirilir.

>[!NOTE]
>Onaylanmış bir kullanıcının etkin olmaması gerektiğine inanan Bir Global yönetici veya Ayrıcalıklı rol yöneticisi, Ayrıcalıklı Kimlik Yönetimi'ndeki etkin rol atamasını kaldırabilir. Yöneticiler, onaylayıcı olmadıkları sürece bekleyen istekler hakkında bilgilendirilmese ler de, Bekleyen istekleri Ayrıcalıklı Kimlik Yönetimi'nde görüntüleyerek tüm kullanıcılar için bekleyen istekleri görüntüleyebilir ve iptal edebilirler.

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüleme

Temsilci onaylayıcısı olarak, Azure REKLAM rol isteği onayınızı beklerken bir e-posta bildirimi alırsınız. Bu bekleyen istekleri Ayrıcalıklı Kimlik Yönetimi'nde görüntüleyebilirsiniz.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**tıklatın.

1. **İstekleri Onayla'yı**tıklatın.

    ![Azure AD rolleri - İstekleri onaylama](./media/azure-ad-pim-approval-workflow/approve-requests.png)

    Onayınızı bekleyen isteklerin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onaylama

1. Onaylamak istediğiniz istekleri seçin ve sonra seçili istekbölmesini onaylamak için **Onayla'yı** tıklatın.

    ![Onay seçeneği vurgulanmış olan isteklistesini onaylama](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. Onay **nedeni** kutusuna bir neden yazın.

    ![Seçili istek bölmelerini onayla nedeni ile onaylama](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. **Onayla**'ya tıklayın.

    Durum simgesi sizin onayınızla güncellenecektir.

    ![Onay düğmesi tıklandıktan sonra seçili istek bölmelerini onaylama](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>İstekleri reddetme

1. İnkar etmek istediğiniz istekleri seçin ve ardından seçili istekbölmesini açmak için **Reddet'i** tıklatın.

    ![Reddet seçeneği vurgulanmış olan isteklistesini onaylama](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. **Reddet nedeni** kutusuna bir neden yazın.

    ![Seçili istek bölmelerini reddet nedeni](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. **Reddet'i**seçin.

    Durum simgesi, reddetmenizle birlikte güncellenecektir.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetiminde E-posta Bildirimleri](pim-email-notifications.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri için istekleri onaylama veya reddetme](pim-resource-roles-approval-workflow.md)
