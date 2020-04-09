---
title: Kaynak sahibi şifre kimlik bilgileri hibesi ile oturum açın | Azure
titleSuffix: Microsoft identity platform
description: Kaynak sahibi parola kimlik bilgisi (ROPC) hibesini kullanarak tarayıcısız kimlik doğrulama akışlarını destekleyin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 173f632a6baed0179b7b4facd2962f9c9764af47
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886339"
---
# <a name="microsoft-identity-platform-and-oauth-20-resource-owner-password-credentials"></a>Microsoft kimlik platformu ve OAuth 2.0 Kaynak Sahibi Parola Kimlik Bilgileri

Microsoft kimlik platformu, bir uygulamanın parolalarını doğrudan işleyerek kullanıcıda oturum açmasına olanak tanıyan [OAuth 2.0 Kaynak Sahibi Parola Kimlik Bilgileri (ROPC) hibesini](https://tools.ietf.org/html/rfc6749#section-4.3)destekler.  Bu makalede, uygulamanızdaki protokole karşı doğrudan programlama nın nasıl yapılacağını açıklanmaktadır.  Mümkün olduğunda, [belirteçleri elde etmek ve güvenli web API'lerini aramak](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)yerine desteklenen Microsoft Kimlik Doğrulama Kitaplıklarını (MSAL) kullanmanızı öneririz.  Ayrıca [MSAL kullanan örnek uygulamalara](sample-v2-code.md)da göz atın.

> [!WARNING]
> Microsoft, ROPC _not_ akışını kullanmamanızı önerir. Çoğu senaryoda, daha güvenli alternatifler kullanılabilir ve önerilir. Bu akış, uygulamaya çok yüksek derecede güven gerektirir ve diğer akışlarda bulunmayan riskleri taşır. Bu akışı yalnızca diğer daha güvenli akışlar kullanılamıyorduğunda kullanmalısınız.

> [!IMPORTANT]
>
> * Microsoft kimlik platformu bitiş noktası, kişisel hesaplar için değil, yalnızca Azure AD kiracıları için ROPC'yi destekler. Bu, kiracıya özgü bir bitiş noktası`https://login.microsoftonline.com/{TenantId_or_Name}`() `organizations` veya bitiş noktası kullanmanız gerektiği anlamına gelir.
> * Azure AD kiracısına davet edilen kişisel hesaplar ROPC kullanamaz.
> * Parolası olmayan hesaplar ROPC üzerinden oturum açabiliyor. Bu senaryo için, uygulamanız için farklı bir akış kullanmanızı öneririz.
> * Kullanıcıların uygulamada oturum açmak için çok faktörlü kimlik doğrulama (MFA) kullanması gerekiyorsa, bunun yerine engellenir.
> * ROPC karma kimlik [federasyonu](/azure/active-directory/hybrid/whatis-fed) senaryolarında desteklenmez (örneğin, Azure AD ve ADFS şirket içi hesapların kimliğini doğrulamak için kullanılır). Kullanıcılar tam sayfa şirket içi kimlik sağlayıcılarına yönlendiriliyorsa, Azure AD kullanıcı adını ve parolayı bu kimlik sağlayıcısıyla karşısına tam olarak test edemez. [Ancak, geçiş kimlik doğrulaması](/azure/active-directory/hybrid/how-to-connect-pta) ROPC ile desteklenir.

## <a name="protocol-diagram"></a>Protokol diyagramı

Aşağıdaki diyagram ROPC akışını gösterir.

![Kaynak sahibinin parola kimlik bilgi akışını gösteren diyagram](./media/v2-oauth2-ropc/v2-oauth-ropc.svg)

## <a name="authorization-request"></a>Yetkilendirme isteği

ROPC akışı tek bir istektir: istemci kimliğini ve kullanıcı kimlik bilgilerini IDP'ye gönderir ve karşılığında belirteçleri alır. İstemci bunu yapmadan önce kullanıcının e-posta adresini (UPN) ve şifresini talep etmelidir. Başarılı bir istekten hemen sonra, istemci kullanıcının kimlik bilgilerini bellekten güvenli bir şekilde serbest bırakmalıdır. Onları asla kurtarmamalı.

> [!TIP]
> Postacı bu isteği yürütmeyi deneyin!
> [![Postacı'da bu isteği çalıştırmayı deneyin](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)


```
// Line breaks and spaces are for legibility only.  This is a public client, so no secret is required. 

POST {tenant}/oauth2/v2.0/token
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=user.read%20openid%20profile%20offline_access
&username=MyUsername@myTenant.com
&password=SuperS3cret
&grant_type=password
```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | Kullanıcıyı oturum açmak istediğiniz dizin kiracı. Bu GUID veya dostu ad biçiminde olabilir. Bu parametre ayarlanamıyor `common` veya `consumers`, ama ' `organizations`olarak ayarlanabilir . |
| `client_id` | Gerekli | Uygulama (istemci) kimliği, Uygulamanıza atanan [Azure portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasıdır. | 
| `grant_type` | Gerekli | Ayarlanmış `password`olmalı. |
| `username` | Gerekli | Kullanıcının e-posta adresi. |
| `password` | Gerekli | Kullanıcının parolası. |
| `scope` | Önerilen | Uygulamanın gerektirdiği [kapsamların](v2-permissions-and-consent.md)veya izinlerin yer le ayrılmış bir listesi. Etkileşimli bir akışta, yönetici veya kullanıcı bu kapsamları önceden kabul etmelidir. |
| `client_secret`| Bazen gerekli | Uygulamanız genel bir istemciyse, `client_secret` `client_assertion` uygulama dahil edilemez veya dahil edilemez.  Uygulama gizli bir istemciyse, eklenmelidir. | 
| `client_assertion` | Bazen gerekli | Farklı bir `client_secret`form , bir sertifika kullanılarak oluşturulan.  Daha fazla ayrıntı için [sertifika kimlik bilgilerine](active-directory-certificate-credentials.md) bakın. | 

### <a name="successful-authentication-response"></a>Başarılı kimlik doğrulama yanıtı

Aşağıdaki örnekte başarılı bir belirteç yanıtı gösterilmektedir:

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| Parametre | Biçimlendir | Açıklama |
| --------- | ------ | ----------- |
| `token_type` | Dize | Her zaman `Bearer`. |
| `scope` | Boşluk ayrılmış dizeleri | Bir erişim belirteci döndürüldüyse, bu parametre erişim belirteci için geçerli olan kapsamları listeler. |
| `expires_in`| int | Dahil edilen erişim jetonunun geçerli olduğu saniye sayısı. |
| `access_token`| Opak dize | İstenen [kapsamlar](v2-permissions-and-consent.md) için verilir. |
| `id_token` | Jwt | Özgün `scope` parametre `openid` kapsamı dahil ise verilir. |
| `refresh_token` | Opak dize | Orijinal `scope` parametre dahil sayılsa `offline_access`verilir. |

Yeni erişim belirteçleri elde etmek ve [OAuth Code akış belgelerinde](v2-oauth2-auth-code-flow.md#refresh-the-access-token)açıklanan aynı akışı kullanarak belirteçleri yenilemek için yenileme belirteci kullanabilirsiniz.

### <a name="error-response"></a>Hata yanıtı

Kullanıcı doğru kullanıcı adını veya parolayı sağlamamışsa veya istemci istenen onayı almadıysa, kimlik doğrulama başarısız olur.

| Hata | Açıklama | İstemci eylemi |
|------ | ----------- | -------------|
| `invalid_grant` | Kimlik doğrulama başarısız oldu | Kimlik bilgileri yanlıştı veya istemcinin istenen kapsamlar için izni yok. Kapsamlar verilmezse, bir `consent_required` hata döndürülür. Bu durumda, istemci bir web görünümü veya tarayıcı kullanarak etkileşimli bir komut istemi kullanıcı göndermesi gerekir. |
| `invalid_request` | İstek yanlış yapılmış. | Hibe türü `/common` veya `/consumers` kimlik doğrulama bağlamlarında desteklenmez.  Bunun `/organizations` yerine bir kiracı kimliği kullanın. |

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Örnek konsol uygulamasını](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2)kullanarak ROPC'yi kendiniz deneyin.
* v2.0 bitiş noktasını kullanıp kullanmadığınızı belirlemek için [Microsoft kimlik platformu sınırlamaları](active-directory-v2-limitations.md)hakkında bilgi edinin.
