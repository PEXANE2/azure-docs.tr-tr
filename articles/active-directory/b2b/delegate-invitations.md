---
title: B2B dış işbirliği ayarlarını etkinleştirin - Azure AD
description: Active Directory B2B dış işbirliğini nasıl etkinleştirebileceğinizi ve konuk kullanıcıları kimlere davet edebileceğinizi nasıl yönetebileceğinizi öğrenin. Davetleri devretmek için Konuk Davetçi rolünü kullanın.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fb10863334392b207c7cfd2172dc9260cf15e2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74272893"
---
# <a name="enable-b2b-external-collaboration-and-manage-who-can-invite-guests"></a>B2B dış işbirliğini etkinleştirin ve konukları kimleri davet edebilirsiniz

Bu makalede, Azure Etkin Dizin (Azure AD) B2B işbirliğinin nasıl etkinleştirilen ve konukları kimlertarafından davet edilebildiğini belirlemek açıklanmaktadır. Varsayılan olarak, dizininizdeki tüm kullanıcılar ve konuklar, yönetici rolüne atanmamış olsalar bile konukları davet edebilir. Dış işbirliği ayarları, kuruluşunuzdaki farklı kullanıcı türleri için konuk davetlerini açmanızı veya kapatmanızı sağlar. Ayrıca, konukları davet etmelerine izin veren roller atayarak tek tek kullanıcılara davet ler verebilirsiniz.

## <a name="configure-b2b-external-collaboration-settings"></a>B2B dış işbirliği ayarlarını yapılandırma

Azure AD B2B işbirliği ile kiracı yönetici aşağıdaki davet ilkelerini ayarlayabilir:

- Davetiyeleri kapatma
- Yalnızca Konuk Davetli rolündeki yöneticiler ve kullanıcılar
- Yöneticiler, Konuk Davetçi rolü ve üyeler
- Konuklar da dahil olmak üzere tüm kullanıcılar,

Varsayılan olarak, misafirler de dahil olmak üzere tüm kullanıcılar konuk kullanıcıları davet edebilir.

### <a name="to-configure-external-collaboration-settings"></a>Dış işbirliği ayarlarını yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) kiracı yönetici olarak oturum açın.
2. **Azure Etkin Dizin** > **Kullanıcıları** > **Kullanıcı ayarlarını**seçin.
3. **Dış kullanıcılar**altında, dış **işbirliği ayarlarını yönet'i**seçin.
   > [!NOTE]
   > **Dış işbirliği ayarları,** Kuruluş **ilişkileri** sayfasından da kullanılabilir. Azure Etkin Dizini'nde, **Yönet**altında, **Kurumsal ilişkiler** > **Ayarları'na**gidin.
4. Dış **işbirliği ayarları** sayfasında, etkinleştirmek istediğiniz ilkeleri seçin.

   ![Dış işbirliği ayarları](./media/delegate-invitations/control-who-to-invite.png)

  - **Konuk kullanıcı izinleri sınırlıdır**: Bu politika, dizininizdeki misafirlerin izinlerini belirler. Kullanıcıları, grupları veya diğer dizin kaynaklarını sayısallandırma gibi belirli dizin görevlerinden misafirleri engellemek için **Evet'i** seçin. Misafirlere dizininizdeki normal kullanıcılarla aynı dizin verilerine erişim sağlamak için **Hayır'ı** seçin.
   - **Konuk davetli rolündeki yöneticiler ve kullanıcılar davet edebilir:**"Konuk Davetli" rolündeki yöneticilerin ve kullanıcıların davet etmesine izin vermek için bu politikayı **Evet**olarak ayarlayın.
   - **Üyeler davet edebilir**: Dizininizin yönetici olmayan üyelerinin konukları davet etmesine izin vermek için bu politikayı **Evet**olarak ayarlayın.
   - **Konuklar davet edebilir**: Misafirlerin diğer konukları davet etmesine izin vermek için bu politikayı **Evet**olarak ayarlayın.
   - **Konuklar için E-posta Tek Seferlik Şifreyi Etkinleştir (Önizleme)**: Tek seferlik parola özelliği hakkında daha fazla bilgi için [e-posta nın tek seferlik parola kimlik doğrulaması (önizleme)](one-time-passcode.md)adresine bakın.
   - **İşbirliği kısıtlamaları**: Belirli etki alanlarındaki davetlere izin verme veya engelleme hakkında daha fazla bilgi için [bkz.](allow-deny-list.md)

## <a name="assign-the-guest-inviter-role-to-a-user"></a>Konuk Davetçi rolünü bir kullanıcıya atama

Konuk Davetçi rolüyle, bireysel kullanıcılara genel bir yönetici veya başka bir yönetici rolü atamadan konukları davet etme olanağı verebilirsiniz. Konuk davetli rolünü bireylere atayın. Ardından, Konuk **davetli rolündeki Yöneticileri ve kullanıcıları** **Evet'e**davet edebileceğinden emin olun.

Aşağıda, Konuk Davetli rolüne bir kullanıcı eklemek için PowerShell'in nasıl kullanılacağını gösteren bir örnek verilmiştir:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği konuk kullanıcılarını davetolmadan ekleme](add-user-without-invite.md)
- [Bir role B2B işbirliği kullanıcısı ekleme](add-guest-to-role.md)


