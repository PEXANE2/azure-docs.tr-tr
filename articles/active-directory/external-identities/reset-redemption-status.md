---
title: Konuk kullanıcının kullanım durumunu sıfırlama-Azure AD
description: Azure AD dış kimliklerinizde Azure Active Directory B2B Konuk kullanıcıları için davet kullanım durumunu sıfırlama hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 02/03/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc51a8af85b85db28265d1fca420a5b9b368b078
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99576937"
---
# <a name="reset-redemption-status-for-a-guest-user"></a>Konuk Kullanıcı için kullanım durumunu sıfırlama

Bir Konuk Kullanıcı, B2B işbirliği davetinizi tamamladıktan sonra, oturum açma bilgilerini güncelleştirmeniz gereken zamanlar olabilir, örneğin:

- Kullanıcı farklı bir e-posta ve kimlik sağlayıcısı kullanarak oturum açmak istiyor
- Ana kiracısındaki kullanıcının hesabı silindi ve yeniden oluşturuldu
- Kullanıcı farklı bir şirkete taşınmış, ancak yine de kaynaklarınızla aynı erişime ihtiyaç duyuyor
- Kullanıcının sorumlulukları başka bir kullanıcıya geçirilmiş

Daha önce bu senaryoları yönetmek için, Konuk kullanıcının hesabını dizininizden el ile silip kullanıcıyı yeniden davet etmeniz gerekiyordu. Artık Kullanıcı kullanım durumunu sıfırlamak ve kullanıcının nesne KIMLIĞINI, grup üyeliklerini ve uygulama atamalarını korurken kullanıcıyı yeniden davet etmek için PowerShell veya Microsoft Graph davet API 'sini kullanabilirsiniz. Kullanıcı yeni daveti etkin hale geldiğinde, yeni e-posta adresi kullanıcının UPN 'si olur. Kullanıcı daha sonra yeni e-posta veya Kullanıcı nesnesinin özelliğine eklemiş olduğunuz bir e-posta kullanarak oturum açabilir `otherMails` .

## <a name="use-powershell-to-reset-redemption-status"></a>Kullanım durumunu sıfırlamak için PowerShell 'i kullanma

En son AzureADPreview PowerShell modülünü kurun ve `InvitedUserEMailAddress` Yeni e-posta adresine ayarlı yeni bir davet oluşturun ve `ResetRedemption` olarak ayarlayın `true` .

```powershell  
Uninstall-Module AzureADPreview 
Install-Module AzureADPreview 
Connect-AzureAD 
$ADGraphUser = Get-AzureADUser -objectID "UPN of User to Reset"  
$msGraphUser = New-Object Microsoft.Open.MSGraph.Model.User -ArgumentList $ADGraphUser.ObjectId 
New-AzureADMSInvitation -InvitedUserEmailAddress <<external email>> -SendInvitationMessage $True -InviteRedirectUrl "http://myapps.microsoft.com" -InvitedUser $msGraphUser -ResetRedemption $True 
```

## <a name="use-microsoft-graph-api-to-reset-redemption-status"></a>Kullanım durumunu sıfırlamak için Microsoft Graph API kullanma

[Microsoft Graph davet API](/graph/api/resources/invitation?view=graph-rest-1.0)'sini kullanarak `resetRedemption` özelliğini olarak ayarlayın `true` ve özelliğindeki yeni e-posta adresini belirtin `invitedUserEmailAddress` .

```json
POST https://graph.microsoft.com/beta/invitations  
Authorization: Bearer eyJ0eX...  
ContentType: application/json  
{  
   "invitedUserEmailAddress": "<<external email>>",  
   "sendInvitationMessage": true,  
   "invitedUserMessageInfo": {  
      "messageLanguage": "en-US",  
      "ccRecipients": [  
         {  
            "emailAddress": {  
               "name": null,  
               "address": "<<optional additional notification email>>"  
            }  
         } 
      ],  
      "customizedMessageBody": "<<custom message>>"  
},  
"inviteRedirectUrl": "https://myapps.microsoft.com?tenantId=",  
"invitedUser": {  
   "id": "<<ID for the user you want to reset>>"  
}, 
"resetRedemption": true 
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell kullanarak Azure Active Directory B2B işbirliği kullanıcıları ekleme](customize-invitation-api.md#powershell)
- [Azure AD B2B Konuk kullanıcısının özellikleri](user-properties.md)
