---
title: PıM-Azure AD 'de Azure AD rolleri için istekleri onaylama veya reddetme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rolleri için istekleri onaylamayı veya reddetmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/07/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 848723f256327190aefd3c4921784240c6c4d46a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84743193"
---
# <a name="approve-or-deny-requests-for-azure-ad-roles-in-privileged-identity-management"></a>Privileged Identity Management Azure AD rolleri için istekleri onaylama veya reddetme

Azure Active Directory (Azure AD) Privileged Identity Management (PıM) sayesinde, rolleri etkinleştirme için onay gerektirecek şekilde yapılandırabilir ve bir ya da birden çok kullanıcı ya da grubu, temsilci onaylayan olarak seçebilirsiniz. Temsilci onaylamalardan isteklerin onaylaması 24 saati vardır. İstek 24 saat içinde onaylanmamışsa, uygun Kullanıcı yeni bir isteği yeniden göndermesi gerekir. 24 saat onay zaman penceresi yapılandırılamaz.

## <a name="determine-your-version-of-pim"></a>PıM sürümünüzü belirleme

2019 Kasım 'Dan başlayarak Privileged Identity Management Azure AD rolleri bölümü, Azure rolleri deneyimleriyle eşleşen yeni bir sürüme güncelleştiriliyor. Bu, ek özellikleri [ve var olan API üzerinde yapılan değişiklikleri](azure-ad-roles-features.md#api-changes)de oluşturur. Yeni sürüm kullanıma sunulurken, bu makalede izlediğiniz yordamlar Şu anda sahip olduğunuz Privileged Identity Management sürümüne bağlıdır. Hangi Privileged Identity Management sürümünü istediğinizi öğrenmek için bu bölümdeki adımları izleyin. Privileged Identity Management Sürümünüzü öğrendikten sonra bu makaledeki sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünde olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Azure AD Privileged Identity Management**açın. Genel Bakış sayfasının üst kısmında yer alan bir başlık varsa, bu makalenin **Yeni sürüm** sekmesinde yer alan yönergeleri izleyin. Aksi takdirde, **önceki sürüm** sekmesindeki yönergeleri izleyin.

    [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

Azure AD rolleri için istekleri onaylamak veya reddetmek için bu makaledeki adımları izleyin.

# <a name="new-version"></a>[Yeni sürüm](#tab/new)

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüleme

Bir temsilci onaylayıcı olarak, Onayınızı bekleyen bir Azure AD rol isteği olduğunda bir e-posta bildirimi alırsınız. Bu bekleyen istekleri Privileged Identity Management görüntüleyebilirsiniz.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Istekleri Onayla**' yı seçin.

    ![İstekleri onaylama-Azure AD rollerini gözden geçirme isteğini gösteren sayfa](./media/azure-ad-pim-approval-workflow/resources-approve-pane.png)

    **Rol etkinleştirmeleri Için istekler** bölümünde, Onayınızı bekleyen isteklerin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onaylama

1. Onaylamak istediğiniz isteği bulun ve seçin. Onaylama veya reddetme sayfası görüntülenir.

    ![İstekleri Onayla-Ayrıntılar ve bloklama kutusu ile onaylama veya reddetme bölmesi](./media/azure-ad-pim-approval-workflow/resources-approve-pane.png)

1. **Gerekçe** kutusunda iş gerekçe ' nı girin.

1. **Onayla**seçeneğini belirleyin. Onayınız için bir Azure bildirimi alacaksınız.

    ![İsteğin onaylandığını gösteren onay bildirimi](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png))

## <a name="deny-requests"></a>Reddetme istekleri

1. Reddetmek istediğiniz isteği bulun ve seçin. Onaylama veya reddetme sayfası görüntülenir.

    ![İstekleri Onayla-Ayrıntılar ve bloklama kutusu ile onaylama veya reddetme bölmesi](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. **Gerekçe** kutusunda iş gerekçe ' nı girin.

1. **Reddet**' i seçin. Bir bildirim, geri çevirme ile birlikte görüntülenir.

## <a name="workflow-notifications"></a>İş akışı bildirimleri

Aşağıda, iş akışı bildirimleri hakkında bazı bilgiler verilmiştir:

- Bir rol isteği gözden geçirmeyi beklerken, onaylayanlara e-posta ile bildirilir. E-posta bildirimleri isteğin doğrudan bir bağlantısını, onaylayanın onaylayabileceği veya reddedebileceği bir bağlantı içerir.
- İstekler onaylayan veya reddeden ilk onaylayan tarafından çözümlenir.
- Bir onaylayan isteğe yanıt verdiğinde, tüm onaylayanlara eylem bildirilir.
- Onaylanan bir kullanıcı rolünde etkin hale geldiğinde Genel Yöneticiler ve ayrıcalıklı rol yöneticileri bilgilendirilir.

>[!NOTE]
>Onaylanan bir kullanıcının etkin olmaması gerektiğini düşündüğü genel yönetici veya ayrıcalıklı rol yöneticisi Privileged Identity Management etkin rol atamasını kaldırabilir. Bir onaylayan olmadıkları müddetçe Yöneticiler bekleyen isteklerden haberdar edilmese de, Privileged Identity Management bekleyen istekleri görüntüleyerek tüm kullanıcılar için bekleyen istekleri görüntüleyebilir ve iptal edebilirler.

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüleme

Bir temsilci onaylayıcı olarak, Onayınızı bekleyen bir Azure AD rol isteği olduğunda bir e-posta bildirimi alırsınız. Bu bekleyen istekleri Privileged Identity Management görüntüleyebilirsiniz.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ne tıklayın.

1. **Istekleri Onayla**' ya tıklayın.

    ![Azure AD rolleri-istekleri onaylama](./media/azure-ad-pim-approval-workflow/approve-requests.png)

    Onayınızı bekleyen isteklerin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onaylama

1. Onaylamak istediğiniz istekleri seçin ve ardından **Onayla** ' ya tıklayarak seçili istekleri Onayla bölmesini açın.

    ![Onaylama seçeneği vurgulanmış şekilde istekleri Onayla listesi](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. **Onay nedeni** kutusuna bir neden yazın.

    ![Seçili istekler bölmesini onay nedeni ile Onayla](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. **Onayla**'ya tıklayın.

    Durum simgesi onayınız ile güncelleştirilir.

    ![Onay düğmesine tıkladıktan sonra seçili istekleri Onayla bölmesi](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Reddetme istekleri

1. Reddetmek istediğiniz istekleri seçin ve sonra seçili istekleri Reddet bölmesini açmak için **Reddet** ' e tıklayın.

    ![Reddetme seçeneği vurgulanmış şekilde istek listesini Onayla](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. **Reddetme nedeni** kutusuna bir neden yazın.

    ![Seçili istekler bölmesini reddetme nedeni ile reddetme](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. **Reddet**' i seçin.

    Durum simgesi, geri çevirme ile güncelleştirilir.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management e-posta bildirimleri](pim-email-notifications.md)
- [Privileged Identity Management Azure Kaynak rolleri için istekleri onaylama veya reddetme](pim-resource-roles-approval-workflow.md)
