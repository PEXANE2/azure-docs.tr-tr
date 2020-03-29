---
title: Grubu silme - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizini'ni kullanarak bir grubun nasıl silinene ilişkin yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdba55e0655a13e65e403f5da73fcb69db5dbca5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68561906"
---
# <a name="delete-a-group-using-azure-active-directory"></a>Azure Etkin Dizini'ni kullanarak grubu silme
Bir Azure Etkin Dizin (Azure AD) grubunu birçok nedenden dolayı silebilirsiniz, ancak genellikle sizin için olacaktır:

- **Grup türünü** yanlış bir seçeneğin olarak ayarlayın.

- Yanlışlıkla yanlış veya yinelenen bir grup oluşturuldu. 

- Artık gruba ihtiyacım yok.

## <a name="to-delete-a-group"></a>Bir grubu silmek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Etkin Dizini'ni**seçin ve ardından **Gruplar'ı**seçin.

3. **Gruplardan - Tüm gruplar** sayfasında, silmek istediğiniz grubu arayın ve seçin. Bu adımlar için **MDM politikasını**kullanacağız - Doğu .

    ![Gruplar-Tüm gruplar sayfası, grup adı vurgulanır](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. **MDM ilkesinde - Doğu Genel Bakış** sayfasında ve sonra **Sil'i**seçin.

    Grup, Azure Etkin Dizin kiracınızdan silinir.

    ![MDM ilkesi - Doğu Genel Bakış sayfası, silme seçeneği vurgulanır](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bir grubu yanlışlıkla silerseniz, yeniden oluşturabilirsiniz. Daha fazla bilgi için [temel bir grup oluşturma ve üye ekleme hakkında](active-directory-groups-create-azure-portal.md)bilgi alabiliyorum.

- Bir Office 365 grubunu yanlışlıkla silerseniz, geri yükleyebilirsiniz. Daha fazla bilgi için [silinmiş bir Office 365 grubunu geri yükle'ye](../users-groups-roles/groups-restore-deleted.md)bakın.
