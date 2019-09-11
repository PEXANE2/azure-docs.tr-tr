---
title: PıM-Azure Active Directory 'de Azure Kaynak rolleri için istekleri onaylama veya reddetme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içindeki Azure Kaynak rolleri için istekleri onaylamayı veya reddetmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b39434f8763e44a126f74ac9a19596e4413ae9c
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804264"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>PıM 'de Azure Kaynak rolleri için istekleri onaylama veya reddetme

Azure Active Directory (Azure AD) Privileged Identity Management (PıM) sayesinde, rolleri etkinleştirme için onay gerektirecek şekilde yapılandırabilir ve bir ya da birden çok kullanıcı ya da grubu, temsilci onaylayan olarak seçebilirsiniz. Temsilci onaylamalardan isteklerin onaylaması 24 saati vardır. İstek 24 saat içinde onaylanmamışsa, uygun Kullanıcı yeni bir isteği yeniden göndermesi gerekir. 24 saat onay zaman penceresi yapılandırılamaz.

Azure Kaynak rolleri için istekleri onaylamak veya reddetmek için bu makaledeki adımları izleyin.

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüle

Bir temsilci onaylayıcı olarak, Onayınızı bekleyen bir Azure Kaynak rolü isteği olduğunda bir e-posta bildirimi alırsınız. Bu bekleyen istekleri PıM ' de görebilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Istekleri Onayla**' ya tıklayın.

    ![İstekleri onaylama-gözden geçirilecek isteği gösteren Azure kaynakları sayfası](./media/pim-resource-roles-approval-workflow/resources-approve-requests.png)

    **Rol etkinleştirmeleri Için istekler** bölümünde, Onayınızı bekleyen isteklerin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onayla

1. Onaylamak istediğiniz isteği bulun ve tıklatın. Onaylama veya reddetme bölmesi görüntülenir.

    ![İstekleri Onayla-Ayrıntılar ve bloklama kutusu ile onaylama veya reddetme bölmesi](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. **Gerekçe** kutusuna bir neden yazın.

1. **Onayla**' ya tıklayın.

    Onayınızı içeren bir bildirim görüntülenir.

    ![İsteğin onaylandığını gösteren onay bildirimi](./media/pim-resource-roles-approval-workflow/resources-approve-notification.png)

## <a name="deny-requests"></a>Reddetme istekleri

1. Reddetmek istediğiniz isteği bulun ve tıklatın. Onaylama veya reddetme bölmesi görüntülenir.

    ![İstekleri Onayla-Ayrıntılar ve bloklama kutusu ile onaylama veya reddetme bölmesi](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. **Gerekçe** kutusuna bir neden yazın.

1. **Reddet**' e tıklayın.

    Bir bildirim, geri çevirme ile birlikte görüntülenir.

## <a name="workflow-notifications"></a>İş akışı bildirimleri

Aşağıda, iş akışı bildirimleri hakkında bazı bilgiler verilmiştir:

- Bir rol isteği gözden geçirmeyi beklerken, onaylayan listesinin tüm üyelerine e-posta gönderilir. E-posta bildirimleri isteğin doğrudan bir bağlantısını, onaylayanın onaylayabileceği veya reddedebileceği bir bağlantı içerir.
- İstekler, onaylayan veya reddeden listenin ilk üyesi tarafından çözümlenir.
- Bir onaylayan isteğe yanıt verdiğinde, onaylayan listesinin tüm üyelerine eylem bildirilir.
- Onaylanan bir üye, rolünde etkin hale geldiğinde kaynak yöneticilerine bildirilir.

>[!Note]
>Onaylanan bir üyenin etkin olmaması gerektiğini düşündüğü bir kaynak yöneticisi, PıM 'de etkin rol atamasını kaldırabilir. Kaynak yöneticilerine, onaylayan listesinin üyesi olmadıkları müddetçe bekleyen istekler bildirilmese de, bekleyen istekleri PıM 'de görüntüleyerek tüm kullanıcıların bekleyen isteklerini görüntüleyebilir ve iptal edebilirler. 

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure Kaynak rollerini genişletme veya yenileme](pim-resource-roles-renew-extend.md)
- [PıM 'de e-posta bildirimleri](pim-email-notifications.md)
- [PıM 'de Azure AD rolleri için istekleri onaylama veya reddetme](azure-ad-pim-approval-workflow.md)
