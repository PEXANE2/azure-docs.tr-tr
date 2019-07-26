---
title: Azure Active Directory Kimlik Koruması için Microsoft Graph | Microsoft Docs
description: Azure Active Directory bir risk olayları listesi ve ilgili bilgiler için Microsoft Graph sorgulama hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: reference
ms.date: 01/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f939811bec312baa1f4c37f0f915d2e881121af
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334085"
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Azure Active Directory Kimlik Koruması ve Microsoft Graph kullanmaya başlama

Microsoft Graph, Microsoft Birleşik API uç noktası ve [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md) API 'lerinin ana adresidir. Riskli kullanıcılar ve oturum açma bilgileri sunan üç API vardır. Birinci API, **ıdentityriskevents**, [risk olaylarının](../reports-monitoring/concept-risk-events.md) listesi ve ilgili bilgiler için Microsoft Graph sorgulamanızı sağlar. İkinci API, **Riskyusers**, risk halinde algılanan kullanıcılar kimlik koruması hakkında bilgi için Microsoft Graph sorgulamanızı sağlar. Üçüncü API, **oturum**açma, risk durumu, ayrıntı ve düzeyiyle ilgili belirli ÖZELLIKLERLE Azure AD oturum açma bilgileri için Microsoft Graph sorgulamanızı sağlar. Bu makale, Microsoft Graph bağlanma ve bu API 'Leri sorgulama ile çalışmaya başlamanızı sağlar. Ayrıntılı bir giriş, tam belgeler ve grafik Gezgini 'ne erişim için, bu API 'Ler için [Microsoft Graph sitesine](https://graph.microsoft.io/) veya belirli başvuru belgelerine bakın:

* [ıdentityriskevents API 'SI](https://docs.microsoft.com/graph/api/resources/identityriskevent?view=graph-rest-beta)
* [riskyUsers API 'SI](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta)
* [Oturum açma API 'SI](https://docs.microsoft.com/graph/api/resources/signin?view=graph-rest-beta)

## <a name="connect-to-microsoft-graph"></a>Microsoft Graph 'a bağlanma

Kimlik koruma verilerine Microsoft Graph aracılığıyla erişmenin dört adımı vardır:

1. Etki alanı adınızı alın.
2. Yeni bir uygulama kaydı oluşturun. 
3. Kimlik doğrulama belirteci aldığınız Microsoft Graph kimlik doğrulaması yapmak için bu parolayı ve diğer birkaç bilgiyi kullanın. 
4. API uç noktasına istek yapmak ve kimlik koruma verilerini geri almak için bu belirteci kullanın.

Başlamadan önce şunları yapmanız gerekir:

* Azure AD 'de uygulamayı oluşturmak için yönetici ayrıcalıkları
* Kiracınızın etki alanının adı (örneğin, contoso.onmicrosoft.com)

## <a name="retrieve-your-domain-name"></a>Etki alanı adınızı alın 

1. Azure portal yönetici olarak [oturum açın](https://portal.azure.com) . 
1. Sol gezinti bölmesinde **Active Directory**' ye tıklayın. 

   ![Uygulama oluşturma](./media/graph-get-started/41.png)

1. **Yönet** bölümünde, **Özellikler**' e tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/42.png)

1. Etki alanı adınızı kopyalayın.

## <a name="create-a-new-app-registration"></a>Yeni bir uygulama kaydı oluşturma

1. **Active Directory** sayfasında, **yönet** bölümünde, **uygulama kayıtları**' a tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/42.png)

1. Üstteki menüde **Yeni uygulama kaydı**' na tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/43.png)

1. **Oluştur** sayfasında, aşağıdaki adımları uygulayın:

   ![Uygulama oluşturma](./media/graph-get-started/44.png)

   1. **Ad** metin kutusuna uygulamanız için bir ad yazın (örneğin: AADıP risk olayı API uygulaması).

   1. **Tür**olarak, **Web uygulaması ve/veya Web API 'si**' ni seçin.

   1. **Oturum açma URL 'si** metin kutusuna yazın `http://localhost`.

   1.           **Oluştur**'a tıklayın.
1. **Ayarlar** sayfasını açmak için, uygulamalar listesinde, yeni oluşturduğunuz uygulama kaydına tıklayın. 
1. **Uygulama kimliğini**kopyalayın.

## <a name="grant-your-application-permission-to-use-the-api"></a>API 'YI kullanmak için uygulamanıza izin verin

1. **Ayarlar** sayfasında, **gerekli izinler**' e tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/15.png)

1. **Gerekli izinler** sayfasında, üstteki araç çubuğundan **Ekle**' ye tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/16.png)

1. **API erişimi ekle** sayfasında, **bir API seçin**' e tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/17.png)

1. **BIR API seçin** sayfasında **Microsoft Graph**' yi seçin ve ardından **Seç**' e tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/18.png)

1. **API erişimi ekle** sayfasında, **izinleri seç**' e tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/19.png)

1. **Erişimi etkinleştir** sayfasında, **tüm kimlik risk bilgilerini oku**' na tıklayın ve ardından **Seç**' e tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/20.png)

1. **API erişimi ekle** sayfasında **bitti**' ye tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/21.png)

1. **Gerekli izinler** sayfasında, **izin ver**' e tıklayın ve ardından **Evet**' e tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/22.png)

## <a name="get-an-access-key"></a>Bir erişim anahtarı alma

1. **Ayarlar** sayfasında **anahtarlar**' a tıklayın.

   ![Uygulama oluşturma](./media/graph-get-started/23.png)

1. **Anahtarlar** sayfasında, aşağıdaki adımları gerçekleştirin:

   ![Uygulama oluşturma](./media/graph-get-started/24.png)

   1. **Anahtar açıklaması** metin kutusuna bir açıklama yazın (örneğin, *Aadıp risk olayı*).
   1. **Süre**olarak **1 yıl içinde**öğesini seçin.
   1. **Kaydet**’e tıklayın.
   1. Anahtar değerini kopyalayın ve ardından güvenli bir konuma yapıştırın.   
   
   > [!NOTE]
   > Bu anahtarı kaybederseniz, bu bölüme dönüp yeni bir anahtar oluşturmanız gerekir. Bu anahtarı gizli tut: verilerinize erişebilen herkes, verilerinize erişebilir.
   > 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Kimlik Microsoft Graph doğrulama ve kimlik risk olayları API 'sini sorgulama

Bu noktada, şunları yapmanız gerekir:

- Kiracınızın etki alanının adı
- İstemci KIMLIĞI 
- Anahtar 

Kimlik doğrulaması yapmak için, gövdesinde aşağıdaki parametrelerle `https://login.microsoft.com` öğesine bir post isteği gönderin:

- grant_type: "**client_credentials**"
- Kaynak`https://graph.microsoft.com`
- client_id: \<istemci kimliğiniz\>
- client_secret: \<anahtarınız\>

Başarılı olursa, bir kimlik doğrulama belirteci döndürür.  
API 'yi çağırmak için aşağıdaki parametreyle bir üst bilgi oluşturun:

```
`Authorization`=”<token_type> <access_token>"
```

Kimlik doğrulanırken, döndürülen belirteçte belirteç türünü ve erişim belirtecini bulabilirsiniz.

Bu üstbilgiyi aşağıdaki API URL 'sine bir istek olarak gönderin:`https://graph.microsoft.com/beta/identityRiskEvents`

Başarılı olursa yanıt, bir kimlik riski olayları ve OData JSON biçimindeki ilişkili verilerin bir koleksiyonudur ve uygun gördüğünüz şekilde ayrıştırılabilir ve işlenebilir.

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

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
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

### <a name="get-the-high-risk-and-medium-risk-events-identityriskevents-api"></a>Yüksek riskli ve orta riskli etkinlikleri edinme (ıdentityriskevents API 'SI)

Orta ve yüksek riskli olaylar, kimlik koruması oturum açma veya Kullanıcı risk ilkelerini tetikleme yeteneğine sahip olabilecek olanları temsil eder. Kullanıcının oturum açmaya çalışan Kullanıcı meşru kimlik sahibi olmadığından, orta veya yüksek bir olasılık olduğundan, bu olayların yapılması bir öncelik olmalıdır. 

```
GET https://graph.microsoft.com/beta/identityRiskEvents?`$filter=riskLevel eq 'high' or riskLevel eq 'medium'" 
```

### <a name="get-all-of-the-users-who-successfully-passed-an-mfa-challenge-triggered-by-risky-sign-ins-policy-riskyusers-api"></a>Riskli oturum açma ilkesi (riskyUsers API) tarafından tetiklenen MFA sınamasını başarıyla geçen tüm kullanıcıları alın

Bir etki alanı risk tabanlı ilkeleri kuruluşunuzda bulunan etkili kimlik koruması hakkında anlamak için, riskli oturum açma ilkesi tarafından tetiklenen bir MFA sınamasını başarıyla geçen tüm kullanıcıları sorgulayabilirsiniz. Bu bilgiler, hangi kullanıcıların kimlik koruması 'nın riskli olarak ne olduğunu ve meşru kullanıcılarınızın AI tarafından riskli bir işlem gerçekleştiriyor olabileceğini anlamanıza yardımcı olabilir.

```
GET https://graph.microsoft.com/beta/riskyUsers?$filter=riskDetail eq 'userPassedMFADrivenByRiskBasedPolicy'
```

### <a name="get-all-the-risky-sign-ins-for-a-specific-user-signin-api"></a>Belirli bir kullanıcı için tüm riskli oturum açma işlemlerini (oturum açma API 'SI) edinme

Bir kullanıcının tehlikede olduğunu düşünüyorsanız, riskli oturum açma işlemlerinin tümünü alarak risk durumunu daha iyi anlayabilirsiniz. 

```
https://graph.microsoft.com/beta/identityRiskEvents?`$filter=userID eq '<userID>' and riskState eq 'atRisk'
```
## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, Microsoft Graph için ilk çağrdınız yaptınız!  
Artık, kimlik riski olaylarını sorgulayabilir ve verileri kullanabilirsiniz, ancak uygun gördüğünüz şekilde

Microsoft Graph ve Graph API kullanarak uygulama oluşturma hakkında daha fazla bilgi edinmek için [Microsoft Graph sitesinde](https://developer.microsoft.com/graph) [belgelere](https://docs.microsoft.com/graph/overview) ve çok daha fazlasına göz atın. 

İlgili bilgiler için bkz.:

- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
- [Azure Active Directory Kimlik Koruması tarafından algılanan risk olayı türleri](../reports-monitoring/concept-risk-events.md)
- [Microsoft Graph](https://developer.microsoft.com/graph/)
- [Microsoft Graph’a genel bakış](https://developer.microsoft.com/graph/docs)
- [Azure AD Kimlik Koruması hizmeti kökü](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityprotection_root)
