---
title: Dinamik gruplar ve B2B işbirliği - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory B2B işbirliği ile Azure AD dinamik gruplarının nasıl kullanılacağını gösterir
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 41e8b81bc3594c6a378757636f70058510a38cc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78226894"
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dinamik gruplar ve Azure Active Directory B2B işbirliği

## <a name="what-are-dynamic-groups"></a>Dinamik gruplar nelerdir?
Azure Active Directory (Azure AD) için güvenlik grubu üyeliğinin dinamik [yapılandırması Azure portalında](https://portal.azure.com)kullanılabilir. Yöneticiler, kullanıcı özelliklerine (userType, department veya country/region gibi) göre Azure AD'de oluşturulan grupları doldurmak için kurallar belirleyebilir. Üyeler, özniteliklerine göre bir güvenlik grubuna otomatik olarak eklenebilir veya kaldırılabilir. Bu gruplar uygulamalara veya bulut kaynaklarına (SharePoint siteleri, belgeler) erişim sağlayabilir ve üyelere lisans atayabilir. [Azure Etkin Dizini'nde Özel gruplardaki](../active-directory-accessmanagement-dedicated-groups.md)dinamik gruplar hakkında daha fazla bilgi edinin.

Dinamik gruplar oluşturmak ve kullanmak için uygun [Azure AD Premium P1 veya P2 lisanslama](https://azure.microsoft.com/pricing/details/active-directory/) gereklidir. Makalede daha fazla bilgi Edinin [Azure Active Directory dinamik grup üyeliği için öznitelik tabanlı kurallar oluşturun.](../users-groups-roles/groups-dynamic-membership.md)

## <a name="creating-an-all-users-dynamic-group"></a>"Tüm kullanıcılar" dinamik grubu oluşturma
Üyelik kuralını kullanarak kiracı içindeki tüm kullanıcıları içeren bir grup oluşturabilirsiniz. Kullanıcılar gelecekte kiracıya eklendiğinde veya kaldırıldığında, grubun üyeliği otomatik olarak ayarlanır.

1. [Azure portalında,](https://portal.azure.com) kiracıda Global yönetici veya Kullanıcı yöneticisi rolüne atanmış bir hesapla oturum açın.
1. **Azure Etkin Dizini'ni**seçin.
2. **Yönet**altında, **Gruplar'ı**seçin ve ardından **Yeni grubu**seçin.
1. Yeni **Grup** sayfasında, **Grup türü**altında **Güvenlik'i**seçin. Yeni grup için bir **Grup adı** ve **Grup açıklaması** girin. 
2. **Üyelik türü altında**Dinamik **Kullanıcı'yı**seçin ve ardından **dinamik sorgu ekle'yi**seçin. 
4. Kural **sözdizimi** metin kutusunun **üzerinde, Edit'i**seçin. **Edit kuralı sözdizimi** sayfasında, metin kutusuna aşağıdaki ifadeyi yazın:

   ```
   user.objectId -ne null
   ```
1. **Tamam'ı**seçin. Kural, Kural sözdizimi kutusunda görünür:

   ![Tüm kullanıcılar dinamik grup için kural sözdizimi](media/use-dynamic-groups/all-user-rule-syntax.png)

1.  **Kaydet'i**seçin. Yeni dinamik grup artık B2B konuk kullanıcılarının yanı sıra üye kullanıcıları da kapsayacak.


1. Grubu oluşturmak için **Yeni grup** sayfasında **Oluştur'u** seçin.

## <a name="creating-a-group-of-members-only"></a>Yalnızca bir üye grubu oluşturma

Grubunuzun konuk kullanıcıları hariç tutmasını ve yalnızca kiracınızın üyelerini içermesini istiyorsanız, yukarıda açıklandığı gibi dinamik bir grup oluşturun, ancak **Kural sözdizimi** kutusuna aşağıdaki ifadeyi girin:

```
(user.objectId -ne null) and (user.userType -eq "Member")
```

Aşağıdaki resimde, yalnızca üyeleri içerecek ve konukları hariç tutmak üzere değiştirilmiş dinamik bir grup için kural sözdizimini gösterilmektedir.

![Kullanıcı türünün üyeye eşit olduğu kuralı gösterir](media/use-dynamic-groups/all-member-user-rule-syntax.png)

## <a name="creating-a-group-of-guests-only"></a>Yalnızca bir konuk grubu oluşturma

İlkeleri (Azure AD Koşullu Erişim ilkeleri gibi) uygulayabilmeniz için yalnızca konuk kullanıcıları içeren yeni bir dinamik grup oluşturmayı da yararlı bulabilirsiniz. Yukarıda açıklandığı gibi dinamik bir grup oluşturun, ancak **Kural sözdizimi** kutusuna aşağıdaki ifadeyi girin:

```
(user.objectId -ne null) and (user.userType -eq "Guest")
```

Aşağıdaki resimde, yalnızca konukları içerecek ve üye kullanıcıları hariç tutmak üzere değiştirilmiş dinamik bir grup için kural sözdizimini gösterilmektedir.

![Kullanıcı türünün konuk eşit olduğu kuralı gösterir](media/use-dynamic-groups/all-guest-user-rule-syntax.png)

## <a name="next-steps"></a>Sonraki adımlar

- [B2B işbirliği kullanıcı özellikleri](user-properties.md)
- [Bir role B2B işbirliği kullanıcısı ekleme](add-guest-to-role.md)
- [B2B işbirliği kullanıcıları için Koşullu Erişim](conditional-access.md)

