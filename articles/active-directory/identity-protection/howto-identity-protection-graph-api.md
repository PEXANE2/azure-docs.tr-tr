---
title: Azure Active Directory Identity Protection için Microsoft Grafik API'si
description: Azure Active Directory'den Microsoft Graph risk algılamalarını ve ilişkili bilgileri nasıl sorgulayabilirsiniz öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: reference
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6fd62ccab4d6e32b23835d280732797e133ada8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671615"
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Azure Active Directory Identity Protection ve Microsoft Graph ile başlayın

Microsoft Graph, Microsoft birleşik API bitiş noktası ve [Azure Active Directory Identity Protection](../active-directory-identityprotection.md) API'lerinin yuvasıdır. Riskli kullanıcılar ve oturum açma bilgileri ortaya çıkaran dört API vardır. İlk API, **riskDetection**, hem kullanıcı ve oturum açma bağlantılı risk algılamaları ve algılama hakkında ilişkili bilgilerin bir listesi için Microsoft Graph sorgulamak için izin verir. İkinci API, **riskli Kullanıcılar,** risk olarak algılanan kullanıcılar Kimlik Koruması hakkında bilgi için Microsoft Graph sorgulamanızı sağlar. **SignIn'** in üçüncü API'si, risk durumu, ayrıntı ve düzeyle ilgili belirli özelliklere sahip Azure AD oturum açmabilgileri için Microsoft Graph'ı sorgulamanıza olanak tanır. Dördüncü API, **identityRiskEvents**, [risk algılamaları](../reports-monitoring/concept-risk-events.md) ve ilişkili bilgilerin bir listesi için Microsoft Graph sorgulamanızı sağlar. KimlikRiskEvents API 10 Ocak 2020 tarihinde amortismana sunulacak; bunun yerine **riskDetections** API kullanmanızı öneririz. Bu makale, Microsoft Graph'a bağlanmave bu API'leri sorgulama ile başlamanızı sağlar. Grafik Gezgini'ne derinlemesine giriş, tam dokümantasyon ve grafik gezginine erişim için [Microsoft Graph sitesine](https://graph.microsoft.io/) veya bu API'ler için özel başvuru belgelerine bakın:

* [riskAlgılama API'si](/graph/api/resources/riskdetection?view=graph-rest-beta)
* [riskyUsers API](/graph/api/resources/riskyuser?view=graph-rest-beta)
* [signIn API](/graph/api/resources/signin?view=graph-rest-beta)
* [identityRiskEvents API](/graph/api/resources/identityriskevent?view=graph-rest-beta) *10 Ocak 2020'de amortismana alınacaktır*

## <a name="connect-to-microsoft-graph"></a>Microsoft grafiğine bağlanma

Microsoft Graph aracılığıyla Kimlik Koruması verilerine erişmek için dört adım vardır:

1. Etki alanı adınızı alın.
2. Yeni bir uygulama kaydı oluşturun. 
3. Kimlik doğrulama belirteci aldığınız Microsoft Graph'a kimlik doğrulaması yapmak için bu sırrı ve birkaç başka bilgi parçasını kullanın. 
4. API bitiş noktasına istekte bulunmak ve Kimlik Koruması verilerini geri almak için bu belirteci kullanın.

Başlamadan önce şunları yapmanız gerekir:

* Azure AD'de uygulama oluşturmak için yönetici ayrıcalıkları
* Kiracınızın etki alanının adı (örneğin, contoso.onmicrosoft.com)

## <a name="retrieve-your-domain-name"></a>Etki alanı adınızı alma 

1. Azure portalınızda yönetici olarak [oturum açın.](https://portal.azure.com) 
1. Sol gezinti bölmesinde **Active Directory'yi**tıklatın. 

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/41.png)

1. **Yönet** **bölümünde, Özellikler'i**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/42.png)

1. Etki alanı adınızı kopyalayın.

## <a name="create-a-new-app-registration"></a>Yeni bir uygulama kaydı oluşturma

1. Active **Directory** sayfasında, **Yönet** **bölümünde, Uygulama kayıtlarını**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/42.png)

1. Üstteki menüde Yeni **uygulama kaydını**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/43.png)

1. **Oluştur** sayfasında aşağıdaki adımları gerçekleştirin:

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/44.png)

   1. **Ad** metin kutusunda, uygulamanız için bir ad yazın (örneğin: Azure AD Risk Algılama API Uygulaması).

   1. **Türü**olarak, **Web Application And / Veya Web API**seçin.

   1. Oturum **Açma URL** metin kutusuna `http://localhost`.

   1. **Oluştur'u**tıklatın.
1. Uygulamalar listesinde **ayarlar** sayfasını açmak için yeni oluşturulan uygulama kaydınızı tıklatın. 
1. Başvuru **Kimliğini**kopyalayın.

## <a name="grant-your-application-permission-to-use-the-api"></a>API'yi kullanmak için başvuru izni ver

1. **Ayarlar** sayfasında Gerekli **İzinler'i**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/15.png)

1. Gerekli **İzinler** sayfasında, üstteki araç çubuğunda **Ekle'yi**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/16.png)

1. **API ekle erişim** **sayfasında, API seç'i**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/17.png)

1. **API seç** sayfasında Microsoft **Graph'ı**seçin ve sonra **Seç'i**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/18.png)

1. **API ekle erişim** **sayfasında, İzinleri Seç'i**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/19.png)

1. Erişimi **Etkinleştir** sayfasında, **tüm kimlik risk bilgilerini oku'yu**tıklatın ve sonra **Seç'i**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/20.png)

1. **API ekle erişim** sayfasında **Bitti'yi**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/21.png)

1. Gerekli **İzinler** sayfasında, **İzin ver'i**tıklatın ve ardından **Evet'i**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/22.png)

## <a name="get-an-access-key"></a>Bir erişim anahtarı alma

1. **Ayarlar** sayfasında **Tuşlar'ı**tıklatın.

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/23.png)

1. **Keys** sayfasında aşağıdaki adımları gerçekleştirin:

   ![Uygulama oluşturma](./media/howto-identity-protection-graph-api/24.png)

   1. Anahtar **açıklaması** metin kutusunda bir açıklama yazın (örneğin, *Azure AD Risk Algılama).*
   1. **Süre**olarak , **1 yıl içinde**seçin.
   1. **Kaydet**'e tıklayın.
   1. Anahtar değerini kopyalayın ve güvenli bir konuma yapıştırın.   
   
   > [!NOTE]
   > Bu anahtarı kaybederseniz, bu bölüme dönüp yeni bir anahtar oluşturmanız gerekir. Bu anahtarı gizli tutun: Bu anahtara sahip olan herkes verilerinize erişebilir.
   > 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-detections-api"></a>Microsoft Graph'a kimlik doğrulaması ve Kimlik Risk Algılamaları API'sini sorgula

Bu noktada, şunları olmalıdır:

- Kiracınızın etki alanının adı
- İstemci kimliği 
- Anahtar 

Kimlik doğrulaması yapmak için, `https://login.microsoft.com` gövdede aşağıdaki parametrelerle bir gönderi isteği gönderin:

- grant_type: "**client_credentials**"
- Kaynak:`https://graph.microsoft.com`
- client_id: \<müşteri kimliğiniz\>
- client_secret: \<anahtarınız\>

Başarılı olursa, bu bir kimlik doğrulama belirteci döndürür.  
API'yi aramak için aşağıdaki parametreye sahip bir üstbilgi oluşturun:

```
`Authorization`="<token_type> <access_token>"
```

Kimlik doğrulaması yaparken, iade edilen belirteçte belirteç türünü ve erişim belirtecinizi bulabilirsiniz.

Bu üstbilgiyi istek olarak aşağıdaki API URL'sine gönderin:`https://graph.microsoft.com/beta/identityRiskEvents`

Yanıt, başarılı olursa, kimlik riski algılamaları ve Uygun gördüğünüz gibi ayrıştırılabilir ve işlenebilir OData JSON biçiminde ilişkili verilerin bir koleksiyondur.

PowerShell'i kullanarak API'nin kimliğini doğrulamak ve aramak için örnek kod burada.  
İstemci kimliğinizi, gizli anahtarınızı ve kiracı etki alanını eklemeniz.

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

## <a name="query-the-apis"></a>API'leri sorgula

Bu üç API, kuruluşunuzdaki riskli kullanıcılar ve oturum açma lar hakkında bilgi almak için çok sayıda fırsat sağlar. Aşağıda, bu API'ler ve ilişkili örnek istekleri için bazı yaygın kullanım örnekleri verilmiştir. Bu sorguları yukarıdaki örnek kodu kullanarak veya [Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer)kullanarak çalıştırabilirsiniz.

### <a name="get-all-of-the-offline-risk-detections-riskdetection-api"></a>Tüm çevrimdışı risk algılamalarını (riskAlgılama API'si) alın

Kimlik Koruması oturum açma risk ilkeleriyle, risk gerçek zamanlı olarak algılandığında koşulları uygulayabilirsiniz. Peki ya çevrimdışı olarak keşfedilen algılamalar? Hangi algılamaların çevrimdışı gerçekleştiğini ve böylece oturum açma risk ilkesini tetiklemeyeceğini anlamak için risk Algılama API'sını sorgulayabilirsiniz.

```
GET https://graph.microsoft.com/beta/riskDetections?$filter=detectionTimingType eq 'offline'
```

### <a name="get-all-of-the-users-who-successfully-passed-an-mfa-challenge-triggered-by-risky-sign-ins-policy-riskyusers-api"></a>Riskli oturum açma ilkesi (riskli Kullanıcılar API' si) tarafından tetiklenen bir MFA meydan okumasını başarıyla geçen tüm kullanıcıları elde edin

Kimlik Koruması risk tabanlı ilkelerin kuruluşunuz üzerindeki etkisini anlamak için, riskli bir oturum açma ilkesi tarafından tetiklenen bir MFA meydan okumasını başarıyla geçen tüm kullanıcıları sorgulayabilirsiniz. Bu bilgiler, kimlik korumasının hangi kullanıcıların yanlış bir şekilde risk olarak algıladığını ve yasal kullanıcılarınızdan hangilerinin AI'nin riskli gördüğü eylemleri gerçekleştirebileceğini anlamanıza yardımcı olabilir.

```
GET https://graph.microsoft.com/beta/riskyUsers?$filter=riskDetail eq 'userPassedMFADrivenByRiskBasedPolicy'
```

### <a name="get-all-the-risky-sign-ins-for-a-specific-user-signin-api"></a>Belirli bir kullanıcı için tüm riskli oturum açma ları alın (signIn API)

Bir kullanıcının gizliliğinin ihlal edildiğini düşünüyorsanız, tüm riskli oturum açmalarını alarak risklerinin durumunu daha iyi anlayabilirsiniz. 

```
https://graph.microsoft.com/beta/identityRiskEvents?`$filter=userID eq '<userID>' and riskState eq 'atRisk'
```
## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, microsoft graph için ilk arama yaptı!  
Artık kimlik risk algılamalarını sorgulayabilir ve verileri uygun gördüğünüz şekilde kullanabilirsiniz.

Microsoft Graph ve Graph API'yi kullanarak uygulamaların nasıl oluşturulabildiğini öğrenmek için, Microsoft [Graph sitesindeki](https://developer.microsoft.com/graph) [belgelere](/graph/overview) ve çok daha fazlasına göz atın. 

İlgili bilgiler için bkz:

- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
- [Azure Active Directory Identity Protection tarafından algılanan risk algılama türleri](../reports-monitoring/concept-risk-events.md)
- [Microsoft Graph](https://developer.microsoft.com/graph/)
- [Microsoft Graph’a genel bakış](https://developer.microsoft.com/graph/docs)
- [Azure AD Kimlik Koruma Hizmeti Kökü](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityprotection_root)
