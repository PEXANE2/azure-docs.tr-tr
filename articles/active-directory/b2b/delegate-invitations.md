---
title: B2B dış işbirliği ayarlarını etkinleştirme-Azure AD
description: B2B dış işbirliğini Active Directory etkinleştirmeyi ve konuk kullanıcıları kimlerin davet edebileceğini yönetmeyi öğrenin. Davetleri devretmek için konuk davet eden rolünü kullanın.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 05/11/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6a2c1a9b908503ee5afc2687ebef473ffed626a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85387191"
---
# <a name="enable-b2b-external-collaboration-and-manage-who-can-invite-guests"></a>B2B dış işbirliğini etkinleştirme ve konuk davet edebilecek kişileri yönetme

Bu makalede Azure Active Directory (Azure AD) B2B işbirliğinin nasıl etkinleştirileceği ve konukları kimlerin davet edebileceğinizi belirleme açıklanmaktadır. Varsayılan olarak, dizininizdeki tüm kullanıcılar ve konuklar bir yönetici rolüne atanmasa bile konukları davet edebilir. Dış işbirliği ayarları, kuruluşunuzdaki farklı türlerdeki kullanıcılar için konuk davetlerini açmanıza veya kapaetmenize olanak tanır. Ayrıca, konukların konuk davet etmesini sağlayan roller atayarak bireysel kullanıcılara davetiye da atayabilirsiniz.

## <a name="configure-b2b-external-collaboration-settings"></a>B2B dış işbirliği ayarlarını yapılandırma

Azure AD B2B işbirliği ile bir kiracı yöneticisi aşağıdaki davet ilkelerini ayarlayabilir:

- Davetleri kapat
- Yalnızca konuk davet eden rolündeki Yöneticiler ve kullanıcılar davet edebilir
- Yöneticiler, konuk davet eden rolü ve Üyeler davet edebilir
- Konuklar dahil tüm kullanıcılar davet edebilir

Varsayılan olarak, konukları dahil tüm kullanıcılar Konuk kullanıcıları davet edebilir.

### <a name="to-configure-external-collaboration-settings"></a>Dış işbirliği ayarlarını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) kiracı yöneticisi olarak oturum açın.
2. **Azure Active Directory**seçin.
3. **Dış kimlikler**  >  **dış işbirliği ayarları**' nı seçin.
6. **Dış işbirliği ayarları** sayfasında, etkinleştirmek istediğiniz ilkeleri seçin.

   ![Dış işbirliği ayarları](./media/delegate-invitations/control-who-to-invite.png)

  - **Konuk kullanıcılar izinleri sınırlıdır**: Bu ilke, dizininizdeki konuklara ilişkin izinleri belirler. Kullanıcıları, grupları veya diğer dizin kaynaklarını numaralandırma gibi belirli dizin görevlerinin konukları engellemek için **Evet** ' i seçin. Konuklara, dizininizdeki normal kullanıcılar olarak dizin verilerine aynı erişimi vermek için **Hayır** ' ı seçin.
   - **Konuk davet eden rolündeki Yöneticiler ve kullanıcılar davet edebilir**: yöneticilerin ve kullanıcıların "konuk davet eden" rolünde konukları davet etmesini sağlamak için, bu ilkeyi **Evet**olarak ayarlayın.
   - **Üyeler davet edebilir**: dizininizin yönetici olmayan üyelerinin konukları davet etmesini sağlamak için, bu ilkeyi **Evet**olarak ayarlayın.
   - **Konuklar davet edebilir**: konukların diğer konukları davet etmesini sağlamak için bu ilkeyi **Evet**olarak ayarlayın.
   - **Konuklar Için tek seferlik geçiş kodunu etkinleştir (Önizleme)**: tek seferlik geçiş kodu özelliği hakkında daha fazla bilgi için bkz. [e-posta bir kerelik geçiş kodu doğrulaması (Önizleme)](one-time-passcode.md).
   - **İşbirliği kısıtlamaları**: belirli etki alanlarına davetlere izin verme veya bunları engelleme hakkında daha fazla bilgi için bkz. [belirli kuruluşlardan B2B kullanıcılarına izin verme veya bu kullanıcıları engelleme](allow-deny-list.md).
   
   > [!NOTE]
   > **Üyeler davet** etmek için **Hayır** ve **Yöneticiler ve konuk davet eden rolündeki kullanıcılar davet** **olarak ayarlanırsa,** **konuk davet** eden rolündeki kullanıcılar konukları davet edebilir.

## <a name="assign-the-guest-inviter-role-to-a-user"></a>Konuk davet eden rolünü bir kullanıcıya atama

Konuk davet eden rolü sayesinde, bireysel kullanıcılara bir küresel yönetici veya başka yönetici rolü atamadan konukları davet etme yeteneği verebilirsiniz. Konuk davet eden rolünü kişilere atayın. Daha sonra **, yöneticileri ve konuk davet eden rolünde bulunan kullanıcıları Evet 'e davet edediğinizden** emin **Yes**olun.

Konuk davet eden rolüne kullanıcı eklemek için PowerShell 'in nasıl kullanılacağını gösteren bir örnek aşağıda verilmiştir:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Bir davetiye olmadan B2B işbirliği Konuk kullanıcıları ekleme](add-user-without-invite.md)
- [Bir role B2B işbirliği kullanıcısı ekleme](add-guest-to-role.md)

