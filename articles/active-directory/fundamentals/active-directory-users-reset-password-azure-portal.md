---
title: Kullanıcıparolalarını sıfırlama - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Active Directory'yi kullanarak bir kullanıcının parolasını sıfırlama hakkında talimatlar.
services: active-directory
author: msaburnley
manager: daveba
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: b4fdbbd4d71a9c97259678413cd9e59ee8aeae6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69032667"
---
# <a name="reset-a-users-password-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak kullanıcının parolalarını sıfırlama

Yönetici olarak, parola unutulursa, kullanıcı bir aygıtın kilitlenirse veya kullanıcı hiç parola almamışsa, kullanıcının parolasını sıfırlayabilirsiniz.

>[!Note]
>Azure AD kiracınız bir kullanıcının ev dizini olmadığı sürece, parolalarını sıfırlayamazsınız. Bu, kullanıcınızın başka bir kuruluştan, microsoft hesabından veya bir Google hesabından bir hesap kullanarak kuruluşunuzda oturum açıyorsa, parolalarını sıfırlayamadığınız anlamına gelir.<br><br>Kullanıcınızın Windows Server Active Directory olarak yetki kaynağı varsa, parolayı yalnızca parola yazma sırtını açtıysanız sıfırlayabilirsiniz.<br><br>Kullanıcınızın Harici Azure REKLAMı olarak yetki kaynağı varsa, parolayı sıfırlayamazsınız. Parolayı yalnızca kullanıcı veya Harici Azure AD'deki bir yönetici sıfırlayabilir.

>[!Note]
>Yönetici değilseniz ve bunun yerine kendi iş veya okul parolanızı sıfırlama hakkında yönergeler arıyorsanız, [bkz.](../user-help/active-directory-passwords-update-your-own-password.md)

## <a name="to-reset-a-password"></a>Parolayı sıfırlamak için

1. [Azure portalında](https://portal.azure.com/) kullanıcı yöneticisi veya parola yöneticisi olarak oturum açın. Kullanılabilir roller hakkında daha fazla bilgi için Azure [Etkin Dizini'nde yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md#available-roles)

2. **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin, sıfırlamaya ihtiyacı olan kullanıcıyı arayın ve seçin ve ardından **Parolayı Sıfırla'yı**seçin.

    **Alain Charon - Profil** sayfası **Sıfırlama parolası** seçeneğiyle görüntülenir.

    ![Kullanıcının profil sayfası, parolayı sıfırla seçeneği vurgulanmış](media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. **Parolayı Sıfırla** sayfasında **parolayı sıfırla'yı**seçin.

    > [!Note]
    > Azure Etkin Dizini kullanırken, kullanıcı için otomatik olarak geçici bir parola oluşturulur. Etkin Dizin'i şirket içinde kullanırken, kullanıcı için parolayı oluşturursunuz.

4. Parolayı kopyalayın ve kullanıcıya verin. Kullanıcının bir sonraki oturum açma işlemi sırasında parolayı değiştirmesi gerekir.

    >[!Note]
    >Geçici parolanın süresi hiç dolmaz. Kullanıcı bir sonraki girişinde, geçici parola nın oluşturulmasından bu yana geçen sürene bakılmaksızın parola çalışmaya devam eder.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcınızın parolasını sıyrıkla sıyrıkla dıktan sonra aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Kullanıcı ekleme veya silme](add-users-azure-active-directory.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

Veya temsilci atama, ilkeleri kullanma ve kullanıcı hesaplarını paylaşma gibi daha karmaşık kullanıcı senaryoları gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için [Azure Etkin Dizin kullanıcı yönetimi belgelerine](../users-groups-roles/index.yml)bakın.
