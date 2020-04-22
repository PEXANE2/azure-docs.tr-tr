---
title: Dahili kullanıcıları B2B işbirliğine davet edin - Azure AD
description: İş ortakları, distribütörler, tedarikçiler, satıcılar ve diğer konuklar için dahili kullanıcı hesaplarınız varsa, onları kendi dış kimlik bilgileriyle oturum açmaya veya oturum açmaya davet ederek Azure AD B2B işbirliğine geçiş yapabilirsiniz. PowerShell veya Microsoft Graph davet API'sini kullanın.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/12/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 783fc0fa6f6c4e6c918fa3ff5fe0b53a71fa0178
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680169"
---
# <a name="invite-internal-users-to-b2b-collaboration"></a>Dahili kullanıcıları B2B işbirliğine davet etme

|     |
| --- |
| Dahili kullanıcıları B2B işbirliğini kullanmaya davet etmek Azure Active Directory'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. |
|     |

Azure AD B2B işbirliğinin kullanılabilirliğinden önce, kuruluşlar distribütörler, tedarikçiler, satıcılar ve diğer konuk kullanıcılarla iç kimlik bilgileri oluşturarak işbirliği yapabilir. Bunun gibi dahili konuk kullanıcılarınız varsa, Azure AD B2B avantajlarından yararlanabilmeniz için onları B2B işbirliğini kullanmaya davet edebilirsiniz. B2B konuk kullanıcılarınız oturum açabilmek için kendi kimliklerini ve kimlik bilgilerini kullanabilecek ve parolaları korumanız veya hesap yaşam döngülerini yönetmeniz gerekmez.

Varolan bir dahili hesaba davet göndermek, kullanıcının nesne kimliğini, UPN'sini, grup üyeliklerini ve uygulama atamalarını korumanızı sağlar. Kullanıcıyı el ile silmeniz ve yeniden davet etmeniz veya kaynakları yeniden atamanız gerekmez. Kullanıcıyı davet etmek için, davet API'sini hem dahili kullanıcı nesnesini hem de konuk kullanıcının e-posta adresini davetle birlikte geçirmek için kullanırsınız. Kullanıcı daveti kabul ettiğinde, B2B hizmeti varolan iç kullanıcı nesnesini bir B2B kullanıcısıyla değiştirir. İleriye dönük olarak, kullanıcıb 2B kimlik bilgilerini kullanarak bulut kaynakları hizmetlerinde oturum açmalıdır. Şirket içi kaynaklara erişmek için dahili kimlik bilgilerini kullanmaya devam edebilirler, ancak iç hesaptaki parolayı sıfırlayarak veya değiştirerek bunu önleyebilirsiniz.

> [!NOTE]
> Davet tek yönlüdür. Dahili kullanıcıları B2B işbirliğini kullanmaya davet edebilirsiniz, ancak eklendikten sonra B2B kimlik bilgilerini kaldıramazsınız. Kullanıcıyı yalnızca dahili bir kullanıcıya geri getirmek için, kullanıcı nesnesini silmeniz ve yeni bir kullanıcı oluşturmanız gerekir.

Genel önizlemede yken, bu makalede iç kullanıcıları B2B işbirliğine davet etmek için açıklanan yöntem şu durumlarda kullanılamaz:

- Dahili kullanıcıya zaten bir Exchange lisansı atandı.
- Kullanıcı, dizininizde doğrudan federasyon için ayarlanmış bir etki alanından gelir.
- Dahili kullanıcı yalnızca buluta açık bir hesaptır ve ana hesabı Azure AD'de değildir.

Bu gibi durumlarda, dahili kullanıcının bir B2B kullanıcısı olarak değiştirilmesi gerekiyorsa, dahili hesabı silmeli ve kullanıcıya B2B işbirliği için bir davet göndermeniz gerekir.

**Şirket içi senkronize kullanıcılar**: Şirket içi ve bulut arasında senkronize edilen kullanıcı hesapları için, şirket içi dizin B2B işbirliğini kullanmaya davet edildikten sonra yetki kaynağı olmaya devam eder. Şirket içi hesapta yaptığınız değişiklikler, hesabı devre dışı bırakmak veya silmesi de dahil olmak üzere bulut hesabıyla eşitlenir. Bu nedenle, yalnızca şirket içi hesabı silerek kullanıcının şirket içi hesabında oturum açmasını engelleyemezsiniz. Bunun yerine, şirket içi hesap parolasını rasgele bir GUID veya bilinmeyen başka bir değere ayarlayabilirsiniz.

## <a name="how-to-invite-internal-users-to-b2b-collaboration"></a>Dahili kullanıcıları B2B işbirliğine nasıl davet edilir?

Dahili kullanıcıya B2B daveti göndermek için PowerShell'i veya davet API'sini kullanabilirsiniz. Davet için kullanmak istediğiniz e-posta adresinin dahili kullanıcı nesnesindeki dış e-posta adresi olarak ayarlandığınızdan emin olun.

- Yalnızca bulutkullanıcıiçin, davet için User.OtherMails özelliğindeki e-posta adresini kullanın.
- Şirket içi senkronize edilmiş bir kullanıcı için, davet için User.Mail özelliğindeki değeri kullanmanız gerekir.
- Kullanıcının Posta özelliğindeki etki alanının oturum açmada kullandıkları hesapla eşleşmesi gerekir. Aksi takdirde, Takımlar gibi bazı hizmetler kullanıcının kimliğini doğrulayamaz.

Varsayılan olarak, davet kullanıcıya davet edildiklerini bildiren bir e-posta gönderir, ancak bu e-postayı bastırabilir ve bunun yerine kendi e-postanızı gönderebilirsiniz.

> [!NOTE]
> Kendi e-postanızı veya diğer iletişiminizi göndermek için, kullanıcıları sessizce davet etmek ve sonra kendi e-posta iletinizi dönüştürülmüş kullanıcıya göndermek için -SendInvitationMessage:$false ile Yeni AzureADMSInvitation'u kullanabilirsiniz. Bkz. [Azure AD B2B işbirliği API'si ve özelleştirme.](customize-invitation-api.md)

## <a name="use-powershell-to-send-a-b2b-invitation"></a>B2B davetiyesi göndermek için PowerShell'i kullanın

Kullanıcıyı B2B işbirliğine davet etmek için aşağıdaki komutu kullanın:

```powershell
Uninstall-Module AzureADPreview
Install-Module AzureADPreview
$ADGraphUser = Get-AzureADUser -searchstring "<<external email>>"
$msGraphUser = New-Object Microsoft.Open.MSGraph.Model.User -ArgumentList $ADGraphUser.ObjectId
New-AzureADMSInvitation -InvitedUserEmailAddress <<external email>> -SendInvitationMessage $True -InviteRedirectUrl "http://myapps.microsoft.com" -InvitedUser $msGraphUser
```

## <a name="use-the-invitation-api-to-send-a-b2b-invitation"></a>B2B daveti göndermek için davet API'sini kullanma

Aşağıdaki örnekte, bir b2B kullanıcısı olarak dahili bir kullanıcıyı davet etmek için davet API'sinin nasıl çağrılması gösteriş verilmiştir.

```json
POST https://graph.microsoft.com/v1.0/invitations
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
        "id": "<<ID for the user you want to convert>>"
    }
}
```

API'ye verilen yanıt, yeni bir konuk kullanıcıyı dizine davet ettiğinizde aldığınız yanıtla aynıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [B2B işbirliği daveti itfa](redemption-experience.md)
