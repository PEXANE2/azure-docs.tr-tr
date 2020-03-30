---
title: B2B işbirliği API ve özelleştirme - Azure Active Directory
description: Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: reference
ms.date: 04/11/2017
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a090ee3f9588ff6bff01e12db469bf04407a7fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263471"
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Azure Active Directory B2B işbirliği API ve özelleştirme

Birçok müşterimiz, davet işlemini organizasyonları için en uygun şekilde özelleştirmek istediklerini söyledi. ApI'mizle bunu yapabilirsin. [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>Davet API'sinin yetenekleri

API aşağıdaki yetenekleri sunar:

1. *Herhangi* bir e-posta adresi olan harici bir kullanıcıyı davet edin.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Kullanıcılarınızın davetlerini kabul ettikten sonra nereye inmelerini istediğinizi özelleştirin.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Standart davetiye postasını bizim aracılığımızüzerinden göndermeyi seçin

    ```
    "sendInvitationMessage": true
    ```

   özelleştirebileceğiniz bir ileti ile alıcıya

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. Ve cc seçin: bu işbirlikçi davet hakkında döngü içinde tutmak istediğiniz kişiler.

5. Veya Azure AD üzerinden bildirim göndermemeyi seçerek davetinizi ve iş akışınızı tamamen özelleştirin.

    ```
    "sendInvitationMessage": false
    ```

   Bu durumda, API'den e-posta şablonuna, IM'ye veya seçtiğiniz diğer dağıtım yöntemine gömebileceğiniz bir kullanım URL'si alırsınız.

6. Son olarak, yöneticiyseniz, kullanıcıyı üye olarak davet etmeyi seçebilirsiniz.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Yetkilendirme modeli

API aşağıdaki yetkilendirme modlarında çalıştırılabilir:

### <a name="app--user-mode"></a>Uygulama + Kullanıcı modu

Bu modda, API'yi kim kullanıyorsa B2B davetiyeleri oluşturmak için izinlere sahip olması gerekir.

### <a name="app-only-mode"></a>Uygulama yalnızca modu

Uygulama nın yalnızca bağlamında, uygulamanın başarılı olması için User.Invite.All scope'a ihtiyacı vardır.

Daha fazla bilgi için:https://developer.microsoft.com/graph/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell

Harici kullanıcıları kolayca bir kuruluşa eklemek ve davet etmek için PowerShell'i kullanabilirsiniz. Cmdlet kullanarak davetiye oluşturun:

```powershell
New-AzureADMSInvitation
```

Aşağıdaki seçenekleri kullanabilirsiniz:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -Davetiye Mesajı Gönder
* -InvitedUserMessageInfo

### <a name="invitation-status"></a>Davet durumu

Harici bir kullanıcıya davetiye gönderdikten sonra, kabul edip etmediklerini görmek için **AzureADUser** cmdlet'ini kullanabilirsiniz. AzureADUser'ın aşağıdaki özellikleri, harici bir kullanıcıya davet gönderildiğinde doldurulur:

* **UserState,** davetin **Beklemeli Kabul** veya **Kabul**olup olmadığını gösterir.
* **UserStateChangedOn,** **UserState** özelliğindeki en son değişikliğin zaman damgasını gösterir.

Sonuçları **UserState'e**göre filtrelemek için **Filtre** seçeneğini kullanabilirsiniz. Aşağıdaki örnekte, yalnızca bekleyen bir daveti olan kullanıcıları göstermek için sonuçları nasıl filtreleneniz gösterilmektedir. Örnekte, görüntülenecek özellikleri belirtmenize olanak tanıyan **Biçimlendirme seçeneği** de gösterilmektedir. 
 

```powershell
Get-AzureADUser -Filter "UserState eq 'PendingAcceptance'" | Format-List -Property DisplayName,UserPrincipalName,UserState,UserStateChangedOn
```

> [!NOTE]
> AzureAD PowerShell modülünün veya AzureADPreview PowerShell modülünün en son sürümüne sahip olduğundan emin olun. 

## <a name="see-also"></a>Ayrıca bkz.

'deki davet API [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)başvurusuna göz atın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği daveti e-posta unsurları](invitation-email-elements.md)
- [B2B işbirliği daveti itfa](redemption-experience.md)
- [B2B işbirliği kullanıcılarını davetsiz ekleme](add-user-without-invite.md)