---
title: Bir kuruluştan Konuk Kullanıcı olarak ayrılın Azure Active Directory
description: Bir Azure AD B2B Konuk kullanıcısının, erişim panelini kullanarak bir kuruluştan nasıl çıkabileceğiniz gösterilmektedir.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8bce67c81b924d768826402b707c41c085b7767b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74272488"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Bir kuruluştan Konuk Kullanıcı olarak ayrılın

Azure Active Directory (Azure AD) B2B Konuk kullanıcısı, artık bu kuruluştan uygulama kullanmaya gerek kalmadığında veya herhangi bir ilişkilendirmeyi koruduklarında, bir kuruluştan herhangi bir zamanda ayrılmaya karar verebilir. Bir Kullanıcı bir yönetici ile iletişim kurmak zorunda kalmadan bir kuruluştan kendi kendine bırakabilir.

> [!NOTE]
> Bir Konuk Kullanıcı, hesabı ev kiracısında veya kaynak kiracısında devre dışı bırakılmışsa bir kuruluştan çıkamaz. Hesabı devre dışıysa, Konuk kullanıcının Kiracı Yöneticisi ile iletişim kurabilmesi gerekir, bu da Konuk hesabını silebilir veya kullanıcı kuruluştan ayrıduyabilmesi için konuk hesabını etkinleştirebilir.

## <a name="leave-an-organization"></a>Kuruluştan ayrılma

Bir kuruluştan ayrılmak için aşağıdaki adımları izleyin.

1. Aşağıdaki adımlardan birini gerçekleştirerek erişim paneli profil sayfanıza gidin:
   
   - [Azure Portal](https://portal.azure.com), sağ üst köşedeki ad ' a tıklayın ve **hesabı görüntüle**' yi seçin.
   - [Erişim masasını](https://myapps.microsoft.com)açın, sağ üst köşedeki adınızı ve ardından **kuruluşlar**' ın yanındaki Ayarlar simgesini (dişli) seçin.
 
   ![Erişim panelinde Kullanıcı ayarlarını gösteren ekran görüntüsü](media/leave-the-organization/UserSettings.png) 

   > [!NOTE]
   > Ayrılmak istediğiniz kuruluşta oturum açmadıysanız, **kuruluşlar**' ın altında kuruluş adının yanında **kuruluştan çıkmak için oturum aç** bağlantısını tıklatın. Oturum açtıktan sonra, sağ üst köşedeki ve **kuruluşlar**' ın yanındaki ad ' a tıklayın, ayarlar simgesini (dişli) seçin.

3. **Kuruluşlar**' ın altında, bırakmak istediğiniz kuruluşu bulun ve **kuruluştan ayrıl**' ı seçin.

   ![Kullanıcı arabirimindeki organizasyonu bırak seçeneğini gösteren ekran görüntüsü](media/leave-the-organization/LeaveOrg.png)

4. Onaylamanız istendiğinde, **bırak**' ı seçin. 

## <a name="account-removal"></a>Hesap kaldırma

Bir kullanıcı kuruluştan ayrıldığında Kullanıcı hesabı, dizinde "geçici olarak silindi" olur. Varsayılan olarak, Kullanıcı nesnesi Azure AD 'deki **silinen kullanıcılar** alanına gider, ancak 30 gün boyunca kalıcı olarak silinmez. Bu geçici silme, kullanıcının hesabı 30 günlük süre içinde geri yükleme isteğinde bulunursa, yöneticinin kullanıcı hesabını (gruplar ve izinler dahil) geri yüklemesini sağlar.

İsterseniz, bir kiracı yöneticisi, hesabı 30 günlük süre boyunca dilediğiniz zaman kalıcı olarak silebilir. Bunu yapmak için:

1. [Azure portal](https://portal.azure.com) **Azure Active Directory**' ni seçin.
2. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
3. **Silinen kullanıcılar**' ı seçin.
4. Silinen bir kullanıcının yanındaki onay kutusunu işaretleyin ve **kalıcı olarak sil**' i seçin.

Bir kullanıcıyı kalıcı olarak silerseniz, bu eylem geri alınamaz.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD B2B 'ye genel bakış için bkz. [Azure AD B2B işbirliği nedir?](what-is-b2b.md)



