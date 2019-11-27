---
title: Kaynak sahibi parola kimlik bilgilerini (ROPC) kullanarak kullanıcıların oturumunu açmak için Microsoft Identity platform 'ı kullanın | Mavisi
description: Kaynak sahibi parola kimlik bilgisi verme kullanarak tarayıcı tarafından daha az kimlik doğrulama akışlarını destekler.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4504a1ae60aaac790ca15c120433159c2ff78fa
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74207777"
---
# <a name="microsoft-identity-platform-and-the-oauth-20-resource-owner-password-credentials"></a>Microsoft Identity platformu ve OAuth 2,0 kaynak sahibi parolası kimlik bilgileri

Microsoft Identity platform, bir uygulamanın kullanıcının parolasını doğrudan işleyerek oturum açmasına izin veren [OAuth 2,0 kaynak sahibi parola kimlik bilgileri (ROPC) vermeyi](https://tools.ietf.org/html/rfc6749#section-4.3)destekler.  Bu makalede, uygulamanızdaki protokolde doğrudan programlanın nasıl yapılacağı açıklanır.  Mümkün olduğunda, [belirteçleri edinmek ve güvenli Web API 'lerini çağırmak](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)Için desteklenen Microsoft kimlik doğrulama KITAPLıKLARıNı (msal) kullanmanızı öneririz.  Ayrıca [, msal kullanan örnek uygulamalara](sample-v2-code.md)göz atın.

> [!WARNING]
> Microsoft, ROPC _akışını kullanmanıza tavsiye_ eder. Çoğu senaryoda, daha güvenli alternatifler kullanılabilir ve önerilir. Bu akış uygulamada çok yüksek düzeyde güven gerektirir ve diğer akışlarda bulunmayan riskleri taşır. Bu akışı yalnızca daha fazla güvenli akış kullanılmıyorsa kullanmanız gerekir.

> [!IMPORTANT]
>
> * Microsoft Identity platform uç noktası, kişisel hesaplara değil yalnızca Azure AD kiracılar için ROPC 'yi destekler. Bu, kiracıya özgü bir uç nokta (`https://login.microsoftonline.com/{TenantId_or_Name}`) veya `organizations` uç noktası kullanmanız gerektiği anlamına gelir.
> * Bir Azure AD kiracısına davet edilen kişisel hesaplar ROPC kullanamaz.
> * Parolası olmayan hesaplar ROPC aracılığıyla oturum açamaz. Bu senaryo için, bunun yerine uygulamanız için farklı bir akış kullanmanızı öneririz.
> * Kullanıcıların uygulamada oturum açmak için Multi-Factor Authentication (MFA) kullanması gerekiyorsa, bunun yerine engellenecektir.
> * ROPC, [karma kimlik Federasyonu](/azure/active-directory/hybrid/whatis-fed) senaryolarında (örneğin, Azure AD ve şirket içi hesapların kimliğini doğrulamak IÇIN kullanılan ADFS) desteklenmez. Kullanıcılar, şirket içi kimlik sağlayıcılarına tam sayfa yönlendirirse, Azure AD bu kimlik sağlayıcısına karşı Kullanıcı adını ve parolayı test edemeyebilir. Öte yandan [doğrudan kimlik doğrulaması](/azure/active-directory/hybrid/how-to-connect-pta) , ropc ile desteklenir.

## <a name="protocol-diagram"></a>Protokol diyagramı

Aşağıdaki diyagramda ROPC akışı gösterilmektedir.

![Kaynak sahibi parola kimlik bilgisi akışını gösteren diyagram](./media/v2-oauth2-ropc/v2-oauth-ropc.svg)

## <a name="authorization-request"></a>Yetkilendirme isteği

ROPC Flow tek bir istek: istemci kimliğini ve kullanıcının kimlik bilgilerini ıDP 'ye gönderir ve ardından dönüşte belirteçleri alır. İstemci, bunu yapmadan önce kullanıcının e-posta adresini (UPN) ve parolasını istemelidir. Başarılı bir istekten hemen sonra istemci, kullanıcının kimlik bilgilerini bellekten güvenli bir şekilde serbest bırakmalıdır. Onları hiçbir şekilde kaydetmemesi gerekir.

> [!TIP]
> Bu isteği Postman 'da yürütmeyi deneyin!
> [![bu isteği Postman 'da çalıştırmayı deneyin](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)


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
| `tenant` | Gerekli | Kullanıcının oturum açmasını istediğiniz dizin kiracısı. Bu, GUID veya kolay ad biçiminde olabilir. Bu parametre `common` veya `consumers`olarak ayarlanamaz, ancak `organizations`olarak ayarlanabilir. |
| `client_id` | Gerekli | [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasının uygulamanıza atadığı uygulama (ISTEMCI) kimliği. | 
| `grant_type` | Gerekli | `password`olarak ayarlanmalıdır. |
| `username` | Gerekli | Kullanıcının e-posta adresi. |
| `password` | Gerekli | Kullanıcının parolası. |
| `scope` | Önerilen | Uygulamanın gerektirdiği, [kapsam](v2-permissions-and-consent.md)veya izinlerin boşlukla ayrılmış bir listesi. Etkileşimli bir akışta yönetici veya Kullanıcı bu kapsamları daha önce kabul etmelidir. |
| `client_secret`| Bazen gerekli | Uygulamanız ortak bir istemcise `client_secret` veya `client_assertion` dahil edilemez.  Uygulama gizli bir istemcise, dahil edilmiş olmalıdır. | 
| `client_assertion` | Bazen gerekli | Sertifika kullanılarak oluşturulan, farklı bir `client_secret`biçimi.  Daha fazla ayrıntı için bkz. [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) . | 

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
| `token_type` | Dize | Her zaman `Bearer`olarak ayarlayın. |
| `scope` | Boşlukla ayrılmış dizeler | Erişim belirteci döndürülürse, bu parametre erişim belirtecinin geçerli olduğu kapsamları listeler. |
| `expires_in`| int | Dahil edilen erişim belirtecinin geçerli olduğu saniye sayısı. |
| `access_token`| Donuk dize | İstenen [kapsamlar](v2-permissions-and-consent.md) için verildi. |
| `id_token` | JWT | Özgün `scope` parametresi `openid` kapsamını içeriyorsa verildi. |
| `refresh_token` | Donuk dize | Özgün `scope` parametresi `offline_access`dahil edilmek için verilir. |

Yenileme belirtecini, [OAuth kod akışı belgelerinde](v2-oauth2-auth-code-flow.md#refresh-the-access-token)açıklanan akışı kullanarak yeni erişim belirteçleri elde etmek ve belirteçleri yenilemek için kullanabilirsiniz.

### <a name="error-response"></a>Hata yanıtı

Kullanıcı doğru Kullanıcı adını veya parolayı sağlamadıysa veya istemci istenen onayı almadıysanız, kimlik doğrulaması başarısız olur.

| Hata | Açıklama | İstemci eylemi |
|------ | ----------- | -------------|
| `invalid_grant` | Kimlik doğrulaması başarısız oldu | Kimlik bilgileri hatalıydı veya istemcinin istenen kapsamlar için izni yok. Kapsamlar verilmezse `consent_required` bir hata döndürülür. Bu durum oluşursa, istemci bir Web görünümü veya tarayıcı kullanarak kullanıcıyı etkileşimli bir istemde göndermelidir. |
| `invalid_request` | İstek yanlış oluşturulmuş | İzin türü `/common` veya `/consumers` kimlik doğrulama bağlamlarında desteklenmiyor.  Bunun yerine `/organizations` veya kiracı KIMLIĞINI kullanın. |

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Örnek konsol uygulamasını](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2)kullanarak ropc 'nizi kendiniz deneyin.
* V 2.0 uç noktasını kullanmanız gerekip gerekmediğini öğrenmek için [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md)hakkında bilgi edinin.
