---
title: B2B işbirliği API 'SI ve özelleştirmesi-Azure Active Directory
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
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78377654"
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Azure Active Directory B2B işbirliği API 'SI ve özelleştirmesi

Birçok müşterimiz, davet sürecini kuruluşlar için en iyi şekilde çalışacak şekilde özelleştirmek istediğimizi söyledik. API 'imizde, yalnızca bunu yapabilirsiniz. [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>Davet API 'SI özellikleri

API aşağıdaki özellikleri sunar:

1. *Herhangi* bir e-posta adresine sahip bir dış kullanıcıyı davet edin.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Kullanıcılarınızın davetini kabul ettikten sonra nereye girmesini istediğinizi özelleştirin.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Standart davet e-postasını bizimle göndermek için seçin

    ```
    "sendInvitationMessage": true
    ```

   bir alıcıya, özelleştirebileceğiniz bir ileti ile

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. Ve CC: Bu ortak çalışan davet etme hakkında döngüde tutmak istediğiniz kişileri seçin.

5. Ya da Azure AD ile bildirim göndermemeyi seçerek davetinizi ve ekleme iş akışınızı tamamen özelleştirin.

    ```
    "sendInvitationMessage": false
    ```

   Bu durumda, bir e-posta şablonuna, anlık kişiye veya seçtiğiniz başka bir dağıtım yöntemine katıştıracağınız API 'den bir kullanım URL 'SI geri alırsınız.

6. Son olarak, bir yöneticisiniz, kullanıcıyı üye olarak davet etmeyi tercih edebilirsiniz.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Yetkilendirme modeli

API aşağıdaki yetkilendirme modlarında çalıştırılabilir:

### <a name="app--user-mode"></a>Uygulama + kullanıcı modu

Bu modda, API 'leri kullanarak B2B davetleri oluşturma izinlerine sahip olması gerekir.

### <a name="app-only-mode"></a>Yalnızca uygulama modu

Yalnızca uygulama bağlamında, uygulamanın kullanıcı. davet. davet için tüm kapsamın başarılı olması gerekir.

Daha fazla bilgi için bkz. https://developer.microsoft.com/graph/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell

PowerShell kullanarak dış kullanıcıları kolayca bir kuruluşa ekleyebilir ve davet edebilirsiniz. Cmdlet 'ini kullanarak bir davetiye oluşturun:

```powershell
New-AzureADMSInvitation
```

Aşağıdaki seçenekleri kullanabilirsiniz:

* -InvitedUserDisplayName
* -Davetteduseremapostaadresi
* -Sendınvitationmessage
* -InvitedUserMessageInfo

### <a name="invitation-status"></a>Davetiye durumu

Bir dış Kullanıcı daveti gönderdikten sonra, bunu kabul edip etmediğini görmek için **Get-AzureADUser** cmdlet 'ini kullanabilirsiniz. Get-AzureADUser öğesinin aşağıdaki özellikleri bir dış kullanıcıya Davetiye gönderildiğinde doldurulur:

* **UserState** , davetin **Pendingkabulünü** veya **kabul**edilip edilmediğini belirtir.
* **Userstatechangedon** , **userState** özelliğindeki en son değişikliğin zaman damgasını gösterir.

Sonuçları **userState**'e göre filtrelemek için **filtre** seçeneğini kullanabilirsiniz. Aşağıdaki örnekte, yalnızca bekleyen bir daveti olan kullanıcıları göstermek için sonuçların nasıl filtreleneceği gösterilmektedir. Örnek, görüntülenecek özellikleri belirtmenize imkan tanıyan **Format-List** seçeneğini de gösterir. 
 

```powershell
Get-AzureADUser -Filter "UserState eq 'PendingAcceptance'" | Format-List -Property DisplayName,UserPrincipalName,UserState,UserStateChangedOn
```

> [!NOTE]
> AzureAD PowerShell modülünün veya AzureADPreview PowerShell modülünün en son sürümüne sahip olduğunuzdan emin olun. 

## <a name="see-also"></a>Ayrıca bkz.

[https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)'de davet API 'sine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği davetiyesi e-postası öğeleri](invitation-email-elements.md)
- [B2B işbirliği daveti satın alma](redemption-experience.md)
- [Bir davetiye olmadan B2B işbirliği kullanıcıları ekleyin](add-user-without-invite.md)