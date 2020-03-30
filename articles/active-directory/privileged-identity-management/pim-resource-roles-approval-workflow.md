---
title: PIM'de Azure kaynak rolleri için istekleri onaylama - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynak rolleri için istekleri nasıl onaylayacağınızı veya reddlayacağınızı öğrenin.
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
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e779f633efccf7b594c193e165a584d22b1d653b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74021981"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri için istekleri onaylama veya reddetme

Azure Etkin Dizini'ndeki (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM) ile rolleri etkinleştirme onayı gerektirecek şekilde yapılandırabilir ve Azure AD kuruluşunuzdaki kullanıcıları veya grupları temsilci onaylayanlar olarak seçebilirsiniz. Ayrıcalıklı rol yöneticisinin iş yükünü azaltmak için her rol için iki veya daha fazla onaylayıcı seçmenizi öneririz. Temsilci onaylayanların istekleri onaylamak için 24 saatleri vardır. Bir istek 24 saat içinde onaylanmazsa, uygun kullanıcı yeni bir isteği yeniden göndermelidir. 24 saatlik onay süresi penceresi yapılandırılamaz.

Azure kaynak rolleri isteklerini onaylamak veya reddetmek için bu makaledeki adımları izleyin.

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüleme

Temsilci onaylayıcısı olarak, Azure kaynak rolü isteği onayınızı beklerken bir e-posta bildirimi alırsınız. Bu bekleyen istekleri Ayrıcalıklı Kimlik Yönetimi'nde görüntüleyebilirsiniz.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **İstekleri Onayla'yı**seçin.

    ![İstekleri onayla - Gözden geçirme isteğini gösteren Azure kaynakları sayfası](./media/pim-resource-roles-approval-workflow/resources-approve-requests.png)

    Rol **etkinleştirme talepleri** bölümünde, onayınızı bekleyen isteklerin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onaylama

1. Onaylamak istediğiniz isteği bulun ve seçin. Bir onay veya reddet sayfası görüntülenir.

    ![İstekleri onaylayın - ayrıntıları ve Yaslama kutusunu içeren bölmeyi onaylama veya reddetme](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. **Yaslama** kutusuna, iş gerekçesini girin.

1. **Onayla'yı**seçin. Onayınıza ait bir Azure bildirimi alırsınız.

    ![İsteğin onaylandığını gösteren bildirimi onaylama](./media/pim-resource-roles-approval-workflow/resources-approve-notification.png)

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
- Onaylı bir kullanıcı rollerinde etkin hale geldiğinde kaynak yöneticileri bildirilir.

>[!Note]
>Onaylanmış bir kullanıcının etkin olmaması gerektiğine inanan bir kaynak yöneticisi, Ayrıcalıklı Kimlik Yönetimi'ndeki etkin rol atamasını kaldırabilir. Kaynak yöneticileri, onaylayıcı olmadıkları sürece bekleyen istekler hakkında bilgilendirilmese ler de, Bekleyen Istekleri Ayrıcalıklı Kimlik Yönetimi'nde görüntüleyerek tüm kullanıcılar için bekleyen istekleri görüntüleyebilir ve iptal edebilirler.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerini genişletme veya yenileme](pim-resource-roles-renew-extend.md)
- [Ayrıcalıklı Kimlik Yönetiminde E-posta Bildirimleri](pim-email-notifications.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rolleri için istekleri onaylama veya reddetme](azure-ad-pim-approval-workflow.md)
