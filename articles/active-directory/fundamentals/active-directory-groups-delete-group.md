---
title: Grubu silme-Azure Active Directory | Microsoft Docs
description: Azure Active Directory kullanarak bir grubun nasıl silineceği hakkında yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 08/29/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: cf309c10dc5924374d8c3a191f6fd11cceb2ac49
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85604293"
---
# <a name="delete-a-group-using-azure-active-directory"></a>Azure Active Directory kullanarak bir grubu silme
Herhangi bir sayıda nedenden dolayı Azure Active Directory (Azure AD) grubunu silebilirsiniz, ancak genellikle şunlar olur:

- **Grup türü** yanlış seçeneğe yanlış ayarlandı.

- Yanlışlıkla yanlış veya yinelenen bir grup oluşturuldu. 

- Artık gruba gerek yok.

## <a name="to-delete-a-group"></a>Bir grubu silmek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Active Directory**' yi seçin ve ardından **gruplar**' ı seçin.

3. **Gruplar-tüm gruplar** sayfasında, silmek istediğiniz grubu arayıp seçin. Bu adımlar için **MDM ilkesi-Doğu**kullanacağız.

    ![Gruplar-tüm gruplar sayfası, Grup adı vurgulanmış](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. **MDM ilkesi-Doğu genel bakış** sayfasında, **Sil**' i seçin.

    Grup, Azure Active Directory kiracınızdan silinir.

    ![MDM ilkesi-Doğu genel bakış sayfası, silme seçeneği vurgulanmış](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bir grubu yanlışlıkla silerseniz, yeniden oluşturabilirsiniz. Daha fazla bilgi için bkz. [temel Grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md).

- Bir Office 365 grubunu yanlışlıkla silerseniz, geri yükleyebilirsiniz. Daha fazla bilgi için bkz. [silinen Office 365 grubunu geri yükleme](../users-groups-roles/groups-restore-deleted.md).
