---
title: Grubu silme-Azure Active Directory | Microsoft Docs
description: Azure Active Directory kullanarak bir grubun nasıl silineceği hakkında yönergeler.
services: active-directory
author: ajburnle
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
ms.openlocfilehash: 9aed5f282ff7b5d573a6f8511f2fc4dbfd27135d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92371826"
---
# <a name="delete-a-group-using-azure-active-directory"></a>Azure Active Directory kullanarak bir grubu silme
Herhangi bir sayıda nedenden dolayı Azure Active Directory (Azure AD) grubunu silebilirsiniz, ancak genellikle şunlar olur:

- **Grup türü** yanlış seçeneğe yanlış ayarlandı.

- Yanlışlıkla yanlış veya yinelenen bir grup oluşturuldu. 

- Artık gruba gerek yok.

## <a name="to-delete-a-group"></a>Bir grubu silmek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Active Directory**' yi seçin ve ardından **gruplar**' ı seçin.

3. **Gruplar-tüm gruplar** sayfasında, silmek istediğiniz grubu arayıp seçin. Bu adımlar için **MDM ilkesi-Doğu** kullanacağız.

    ![Groups-All grupları sayfası, Grup adı vurgulanmış](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. **MDM ilkesi-Doğu genel bakış** sayfasında, **Sil**' i seçin.

    Grup, Azure Active Directory kiracınızdan silinir.

    ![MDM ilkesi-Doğu genel bakış sayfası, silme seçeneği vurgulanmış](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bir grubu yanlışlıkla silerseniz, yeniden oluşturabilirsiniz. Daha fazla bilgi için bkz. [temel Grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md).

- Bir Microsoft 365 grubunu yanlışlıkla silerseniz, geri yükleyebilirsiniz. Daha fazla bilgi için bkz. [silinen Office 365 grubunu geri yükleme](../enterprise-users/groups-restore-deleted.md).
