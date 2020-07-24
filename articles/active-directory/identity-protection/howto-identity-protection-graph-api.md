---
title: Azure Active Directory Kimlik Koruması için API Microsoft Graph
description: Microsoft Graph risk algılamalarını ve ilgili bilgileri nasıl sorguleyeceğinizi öğrenin Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 07/14/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81c3715995d0d53cd72acf7277f46a279fa4cfb2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87019667"
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Azure Active Directory Kimlik Koruması ve Microsoft Graph kullanmaya başlama

Microsoft Graph, Microsoft Birleşik API uç noktası ve [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md) API 'lerinin ana adresidir. Riskli kullanıcılar ve oturum açma bilgileri sunan üç API vardır. İlk API, **riskDetection**, hem Kullanıcı hem de oturum açma ile bağlantılı risk algılamaları ve algılamayla ilgili bilgiler için Microsoft Graph sorgulamanızı sağlar. İkinci API, **Riskyusers**, risk halinde algılanan kullanıcılar kimlik koruması hakkında bilgi için Microsoft Graph sorgulamanızı sağlar. Üçüncü API, **oturum**açma, risk durumu, ayrıntı ve düzeyiyle ilgili belirli ÖZELLIKLERLE Azure AD oturum açma bilgileri için Microsoft Graph sorgulamanızı sağlar. 

Bu makale, Microsoft Graph bağlanma ve bu API 'Leri sorgulama ile çalışmaya başlamanızı sağlar. Ayrıntılı bir giriş, tam belgeler ve grafik Gezgini 'ne erişim için, bu API 'Ler için [Microsoft Graph sitesine](https://graph.microsoft.io/) veya belirli başvuru belgelerine bakın:

* [riskDetection API'si](/graph/api/resources/riskdetection?view=graph-rest-v1.0)
* [riskyUsers API'si](/graph/api/resources/riskyuser?view=graph-rest-v1.0)
* [signIn API'si](/graph/api/resources/signin?view=graph-rest-v1.0)

## <a name="connect-to-microsoft-graph"></a>Microsoft Graph 'a bağlanma

Kimlik koruma verilerine Microsoft Graph aracılığıyla erişmenin dört adımı vardır:

- [Etki alanı adınızı alın](#retrieve-your-domain-name)
- [Yeni bir uygulama kaydı oluşturun](#create-a-new-app-registration)
- [API izinlerini yapılandırma](#configure-api-permissions)
- [Geçerli bir kimlik bilgisi yapılandırın](#configure-a-valid-credential)

### <a name="retrieve-your-domain-name"></a>Etki alanı adınızı alın 

1. [Azure portalında](https://portal.azure.com) oturum açın.  
1. **Azure Active Directory**  >  **Özel etki alanı adlarına**Azure Active Directory gidin. 
1. `.onmicrosoft.com`Etki alanını bir yere göz atın, bu bilgilere sonraki bir adımda ihtiyacınız olacaktır.

### <a name="create-a-new-app-registration"></a>Yeni bir uygulama kaydı oluşturun

1. Azure Portal **Azure Active Directory**  >  **uygulama kayıtları**' e gidin.
1. **Yeni kayıt**seçeneğini belirleyin.
1. **Oluştur** sayfasında, aşağıdaki adımları uygulayın:
   1. **Ad** metin kutusuna uygulamanız için bir ad yazın (örneğin: Azure AD risk algılama API 'si).
   1. **Desteklenen hesap türleri**altında, API 'leri kullanacak hesapların türünü seçin.
   1. **Kaydet**’i seçin.
1. **Uygulama kimliğini**kopyalayın.

### <a name="configure-api-permissions"></a>API izinlerini yapılandırma

1. Oluşturduğunuz **uygulamadan** , **API izinleri**' ni seçin.
1. **Yapılandırılan izinler** sayfasında, üstteki araç çubuğundan **izin Ekle**' ye tıklayın.
1. **API erişimi ekle** sayfasında, **bir API seçin**' e tıklayın.
1. **BIR API seçin** sayfasında **Microsoft Graph**' yi seçin ve ardından **Seç**' e tıklayın.
1. **API Izinleri iste** sayfasında: 
   1. **Uygulama izinleri**' ni seçin.
   1. Ve ' nin yanındaki onay kutularını seçin `IdentityRiskEvent.Read.All` `IdentityRiskyUser.Read.All` .
   1. **Izin Ekle**' yi seçin.
1. **Etki alanı için yönetici onayı ver** ' i seçin 

### <a name="configure-a-valid-credential"></a>Geçerli bir kimlik bilgisi yapılandırın

1. Oluşturduğunuz **uygulamadan** **Sertifikalar & parolaları**' nı seçin.
1. **İstemci gizli**dizileri altında **yeni istemci parolası**' nı seçin.
   1. İstemciye gizli dizi **açıklaması** verin ve kullanım süresi sonu süresini kuruluş ilkelerinize göre ayarlayın.
   1. **Ekle**’yi seçin.

   > [!NOTE]
   > Bu anahtarı kaybederseniz, bu bölüme dönüp yeni bir anahtar oluşturmanız gerekir. Bu anahtarı gizli tut: verilerinize erişebilen herkes, verilerinize erişebilir.

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-detections-api"></a>Kimlik doğrulama Microsoft Graph ve kimlik riski algılama API 'sini sorgulama

Bu noktada, şunları yapmanız gerekir:

- Kiracınızın etki alanının adı
- Uygulama (istemci) KIMLIĞI 
- İstemci parolası veya sertifikası 

Kimlik doğrulaması yapmak için, gövdesinde aşağıdaki parametrelerle öğesine bir post isteği gönderin `https://login.microsoft.com` :

- grant_type: "**client_credentials**"
- Kaynak`https://graph.microsoft.com`
- client_id:\<your client ID\>
- client_secret:\<your key\>

Başarılı olursa, bu istek bir kimlik doğrulama belirteci döndürür.  
API 'yi çağırmak için aşağıdaki parametreyle bir üst bilgi oluşturun:

```
`Authorization`="<token_type> <access_token>"
```

Kimlik doğrulanırken, döndürülen belirteçte belirteç türünü ve erişim belirtecini bulabilirsiniz.

Bu üstbilgiyi aşağıdaki API URL 'sine bir istek olarak gönderin:`https://graph.microsoft.com/v1.0/identityProtection/riskDetections`

Başarılı olursa yanıt, bir kimlik riski algılamaları ve, uygun gördüğünüz şekilde ayrıştırılabilen ve işlenebilen OData JSON biçimindeki ilişkili veriler koleksiyonudur.

### <a name="sample"></a>Örnek

PowerShell kullanarak API 'YI doğrulamak ve çağırmak için örnek kod aşağıda verilmiştir.  
Yalnızca istemci KIMLIĞINIZI, gizli anahtarı ve kiracı etki alanını ekleyin.

```PowerShell
    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/v1.0/identityProtection/riskDetections"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 
```

## <a name="query-the-apis"></a>API 'Leri sorgulama

Bu üç API, kuruluşunuzdaki riskli kullanıcılar ve oturum açma bilgileri hakkında bilgi almak için çok sayıda fırsat sağlar. Aşağıda, bu API 'Ler ve ilişkili örnek istekler için bazı yaygın kullanım durumları verilmiştir. Bu sorguları yukarıdaki örnek kodu kullanarak veya [grafik Gezginini](https://developer.microsoft.com/graph/graph-explorer)kullanarak çalıştırabilirsiniz.

### <a name="get-all-of-the-offline-risk-detections-riskdetection-api"></a>Tüm çevrimdışı risk algılamalarını al (riskDetection API)

Kimlik koruması oturum açma risk ilkeleriyle, risk gerçek zamanlı olarak algılandığında koşullar uygulayabilirsiniz. Ancak çevrimdışı bulunan algılamalar nelerdir? Hangi algılamaların çevrimdışı olduğunu anlamak ve bu nedenle oturum açma risk ilkesini tetiklemeyeceğini anlamak için riskDetection API 'sini sorgulayabilirsiniz.

```
GET https://graph.microsoft.com/v1.0/identityProtection/riskDetections?$filter=detectionTimingType eq 'offline'
```

### <a name="get-all-of-the-users-who-successfully-passed-an-mfa-challenge-triggered-by-risky-sign-ins-policy-riskyusers-api"></a>Riskli oturum açma ilkesi (riskyUsers API) tarafından tetiklenen MFA sınamasını başarıyla geçen tüm kullanıcıları alın

Kuruluşunuzda etkili kimlik koruması risk tabanlı ilkelerin olduğunu anlamak için, riskli oturum açma ilkesi tarafından tetiklenen bir MFA sınamasını başarıyla geçen tüm kullanıcıları sorgulayabilirsiniz. Bu bilgiler, hangi kullanıcıların kimlik koruması 'nın riskli olarak ne olduğunu ve meşru kullanıcılarınızın AI tarafından riskli bir işlem gerçekleştiriyor olabileceğini anlamanıza yardımcı olabilir.

```
GET https://graph.microsoft.com/v1.0/identityProtection/riskyUsers?$filter=riskDetail eq 'userPassedMFADrivenByRiskBasedPolicy'
```

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, Microsoft Graph için ilk çağrdınız yaptınız!  
Artık kimlik riski algılamalarını sorgulayabilir ve verileri kullanabilirsiniz, ancak bunları kullanın.

Microsoft Graph ve Graph API kullanarak uygulama oluşturma hakkında daha fazla bilgi edinmek için [Microsoft Graph sitesinde](https://developer.microsoft.com/graph) [belgelere](/graph/overview) ve çok daha fazlasına göz atın. 

İlgili bilgiler için bkz.:

- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
- [Azure Active Directory Kimlik Koruması tarafından algılanan risk algılamaları türleri](../reports-monitoring/concept-risk-events.md)
- [Microsoft Graph](https://developer.microsoft.com/graph/)
- [Microsoft Graph’a genel bakış](https://developer.microsoft.com/graph/docs)
- [Azure AD Kimlik Koruması hizmeti kökü](https://docs.microsoft.com/graph/api/resources/identityprotectionroot?view=graph-rest-1.0)
