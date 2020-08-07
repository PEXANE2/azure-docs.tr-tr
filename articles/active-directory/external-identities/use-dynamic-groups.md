---
title: Dinamik Gruplar ve B2B işbirliği-Azure Active Directory | Microsoft Docs
description: Azure Active Directory B2B işbirliğiyle Azure AD dinamik gruplarının nasıl kullanılacağını gösterir
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 02/28/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6adf7e6c5abbba3c018f9a03b5167aec7537c704
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87909951"
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dinamik Gruplar ve Azure Active Directory B2B işbirliği

## <a name="what-are-dynamic-groups"></a>Dinamik Gruplar nelerdir?
Azure Active Directory (Azure AD) için güvenlik grubu üyeliğinin dinamik yapılandırması [Azure Portal](https://portal.azure.com)kullanılabilir. Yöneticiler, Azure AD 'de oluşturulan grupları Kullanıcı özniteliklerine (userType, departmanı veya ülke/bölge gibi) göre doldurmak için kurallar ayarlayabilir. Üyeler, özniteliklerine göre otomatik olarak bir güvenlik grubuna eklenebilir veya kaldırılabilir. Bu gruplar, uygulamalara veya bulut kaynaklarına (SharePoint siteleri, belgeler) erişim sağlayabilir ve üyelere lisans atayabilir. [Azure Active Directory Içindeki adanmış gruplarda](../active-directory-accessmanagement-dedicated-groups.md)dinamik gruplar hakkında daha fazla bilgi edinin.

Dinamik gruplar oluşturmak ve kullanmak için uygun [Azure AD Premium P1 veya P2 lisansı](https://azure.microsoft.com/pricing/details/active-directory/) gerekir. [Azure Active Directory içindeki dinamik grup üyeliği için öznitelik tabanlı kurallar oluşturma](../users-groups-roles/groups-dynamic-membership.md)makalesindeki daha fazla bilgi edinin.

## <a name="creating-an-all-users-dynamic-group"></a>"Tüm kullanıcılar" dinamik grubu oluşturma
Bir üyelik kuralı kullanarak bir kiracının içindeki tüm kullanıcıları içeren bir grup oluşturabilirsiniz. Kullanıcılar gelecekte kiracıya eklendiğinde veya kiracıdan çıkarıldığında grubun üyeliği otomatik olarak ayarlanır.

1. Kiracıda genel yönetici veya Kullanıcı Yöneticisi rolünün atandığı bir hesapla [Azure Portal](https://portal.azure.com) oturum açın.
1. **Azure Active Directory**seçin.
2. **Yönet**altında **gruplar**' ı seçin ve ardından **Yeni Grup**' u seçin.
1. **Yeni Grup** sayfasında, **Grup türü**altında **güvenlik**' i seçin. Yeni grup için bir **Grup adı** ve **Grup açıklaması** girin. 
2. **Üyelik türü**altında **Dinamik Kullanıcı**' yı seçin ve ardından **Dinamik sorgu Ekle**' yi seçin. 
4. **Kural sözdizimi** metin kutusunun üstünde **Düzenle**' yi seçin. **Kural sözdizimini Düzenle** sayfasında, metin kutusuna aşağıdaki ifadeyi yazın:

   ```
   user.objectId -ne null
   ```
1. **Tamam**’ı seçin. Kural, kural sözdizimi kutusunda görünür:

   ![Tüm kullanıcılar için kural sözdizimi dinamik grubu](media/use-dynamic-groups/all-user-rule-syntax.png)

1.  **Kaydet**’i seçin. Yeni dinamik grup artık B2B Konuk kullanıcılarını ve üye kullanıcıları içerir.


1. Grubu oluşturmak için **Yeni Grup** sayfasında **Oluştur** ' u seçin.

## <a name="creating-a-group-of-members-only"></a>Yalnızca üye grubunu oluşturma

Grubunuzun Konuk kullanıcıları dışlanmasını ve yalnızca kiracınızın üyelerini içermesini istiyorsanız, yukarıda açıklandığı gibi bir dinamik grup oluşturun, ancak **kural söz dizimi** kutusunda aşağıdaki ifadeyi girin:

```
(user.objectId -ne null) and (user.userType -eq "Member")
```

Aşağıdaki görüntüde, yalnızca üyeleri içerecek ve konukları dışlayacak şekilde değiştirilen dinamik bir grubun kural sözdizimi gösterilmektedir.

![Kullanıcı türünün üye olduğu kuralı gösterir](media/use-dynamic-groups/all-member-user-rule-syntax.png)

## <a name="creating-a-group-of-guests-only"></a>Yalnızca konuk grubu oluşturuluyor

Ayrıca, yalnızca Konuk kullanıcıları içeren yeni bir dinamik grup oluşturmayı yararlı bulabilirsiniz. bu sayede ilkeleri (Azure AD koşullu erişim ilkeleri gibi) uygulayabilirsiniz. Yukarıda açıklandığı gibi bir dinamik grup oluşturun, ancak **kural söz dizimi** kutusunda aşağıdaki ifadeyi girin:

```
(user.objectId -ne null) and (user.userType -eq "Guest")
```

Aşağıdaki görüntüde, yalnızca konukları içerecek ve üye kullanıcıları dışlayacak şekilde değiştirilen dinamik bir grubun kural sözdizimi gösterilmektedir.

![Kullanıcı türünün konuğa eşit olduğu kuralı gösterir](media/use-dynamic-groups/all-guest-user-rule-syntax.png)

## <a name="next-steps"></a>Sonraki adımlar

- [B2B işbirliği kullanıcı özellikleri](user-properties.md)
- [Bir role B2B işbirliği kullanıcısı ekleme](add-guest-to-role.md)
- [B2B işbirliği kullanıcıları için koşullu erişim](conditional-access.md)

