---
title: Bir kuruluşu konuk kullanıcı olarak bırakın - Azure Etkin Dizin
description: Azure AD B2B konuk kullanıcısının Access Panelini kullanarak kuruluştan nasıl ayrılabileceğini gösterir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74272488"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Bir kuruluşu konuk kullanıcı olarak bırakma

Azure Etkin Dizin (Azure AD) B2B konuk kullanıcısı, artık o kuruluştaki uygulamaları kullanmaları veya herhangi bir ilişkilendirme sürdürmeleri gerekmiyorsa, herhangi bir zamanda kuruluştan ayrılmaya karar verebilir. Kullanıcı, bir yöneticiyle iletişim kurmak zorunda kalmadan bir kuruluştan kendi başına ayrılabilir.

> [!NOTE]
> Konuk kullanıcı, hesabı ev kiracısında veya kaynak kiracısında devre dışı bırakılmışsa kuruluştan ayrılamaz. Hesapları devre dışı bırakılırsa, konuk kullanıcının konuk hesabı silebilen veya konuk hesabı etkinleştirebilen kiracı yöneticiyle iletişime geçmesi gerekir.

## <a name="leave-an-organization"></a>Kuruluştan ayrılma

Kuruluştan ayrılmak için aşağıdaki adımları izleyin.

1. Aşağıdaki adımlardan birini yaparak Access Panel Profili sayfanıza gidin:
   
   - Azure [portalında,](https://portal.azure.com)sağ üstteki adınızı tıklatın ve **Hesabı Görüntüle'yi**seçin.
   - Erişim [Panelinizi](https://myapps.microsoft.com)açın, sağ üstteki adınızı tıklatın ve **Kuruluşların**yanında ayarlar simgesini (vites) seçin.
 
   ![Access Panel'de kullanıcı ayarlarını gösteren ekran görüntüsü](media/leave-the-organization/UserSettings.png) 

   > [!NOTE]
   > Ayrılmak istediğiniz kuruluşta zaten oturum açmadıysanız, **Kuruluşlar**altında, kuruluşun adının yanındaki kuruluş bağlantısını **bırakmak için Oturum Aç'ı** tıklatın. Oturum girdikten sonra, sağ üstte ve **Kuruluşların**yanında adınızı yeniden tıklatın, ayarlar simgesini (vites) seçin.

3. **Kuruluşlar**altında, ayrılmak istediğiniz kuruluşu bulun ve **kuruluşu bırak'ı**seçin.

   ![Kullanıcı arabiriminde organizasyon bırak seçeneğini gösteren ekran görüntüsü](media/leave-the-organization/LeaveOrg.png)

4. Onaylamam istendiğinde **Bırak'ı**seçin. 

## <a name="account-removal"></a>Hesap kaldırma

Bir kullanıcı bir kuruluşayrıldığında, kullanıcı hesabı dizinde "yumuşak silinir". Varsayılan olarak, kullanıcı nesnesi Azure AD'deki **Silinmiş kullanıcılar** alanına taşınır, ancak 30 gün boyunca kalıcı olarak silinmez. Bu yumuşak silme, kullanıcı nın hesabı 30 günlük süre içinde geri yükleme sevesi getirmesi durumunda, yöneticinin kullanıcı hesabını (gruplar ve izinler dahil) geri yüklemesine olanak tanır.

İstenirse, kiracı yönetici 30 günlük süre boyunca herhangi bir zamanda hesabı kalıcı olarak silebilir. Bunu yapmak için:

1. Azure [portalında](https://portal.azure.com) **Azure Etkin Dizin'i**seçin.
2. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
3. **Silinen kullanıcıları**seçin.
4. Silinen bir kullanıcının yanındaki onay kutusunu seçin ve ardından **kalıcı olarak Sil'i**seçin.

Bir kullanıcıyı kalıcı olarak silerseniz, bu eylem geri alınamaz.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD B2B'ye genel bakış için [bkz.](what-is-b2b.md)



