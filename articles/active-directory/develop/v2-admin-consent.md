---
title: Microsoft kimlik platformu yönetici onay protokolleri
description: Kapsamlar, izinler ve onay dahil olmak üzere Microsoft kimlik platformu bitiş noktasında yetkilendirme açıklaması.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 12/3/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: c9f633e0d205adaf5cefb2e3c036ce7f48253651
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886390"
---
# <a name="admin-consent-on-the-microsoft-identity-platform"></a>Microsoft kimlik platformunda yönetici onayı

Bazı izinler, kiracı içinde verilebilmeleri için yöneticinin onayvermesini gerektirir.  Yönetici onayı bitiş noktasını, tüm kiracıya izin vermek için de kullanabilirsiniz.  

## <a name="recommended-sign-the-user-into-your-app"></a>Önerilen: Kullanıcıyı uygulamanızda oturum

Genellikle, yönetici onayı bitiş noktasını kullanan bir uygulama oluşturduğunuzda, uygulamanın yöneticinin uygulamanın izinlerini onaylayacağı bir sayfaya veya görünüme ihtiyacı vardır. Bu sayfa, uygulamanın kaydolma akışının, uygulamaayarlarının bir parçası olabilir veya özel bir "bağlantı" akışı olabilir. Çoğu durumda, uygulamanın bu "bağlantı" görünümünü yalnızca bir kullanıcı bir iş veya okul Microsoft hesabıyla oturum açtındıktan sonra göstermesi mantıklıdır.

Kullanıcıyı uygulamanızda oturum açtığınızda, gerekli izinleri onaylamalarını istemeden önce yöneticinin ait olduğu kuruluşu tanımlayabilirsiniz. Kesinlikle gerekli olmasa da, kuruluş kullanıcılarınız için daha sezgisel bir deneyim oluşturmanıza yardımcı olabilir. Kullanıcıyı oturum alabilmek için [Microsoft kimlik platformu protokol eğitimlerimizi](active-directory-v2-protocols.md)uygulayın.

## <a name="request-the-permissions-from-a-directory-admin"></a>İzinleri bir dizin yöneticisinden isteme

Kuruluşunuzun yöneticisinden izin istemeye hazır olduğunuzda, kullanıcıyı Microsoft kimlik platformu *yönetici onayı bitiş noktasına*yönlendirebilirsiniz.

```
// Line breaks are for legibility only.
    GET https://login.microsoftonline.com/{tenant}/v2.0/adminconsent?
  client_id=6731de76-14a6-49ae-97bc-6eba6914391e
  &state=12345
  &redirect_uri=http://localhost/myapp/permissions
    &scope=
    https://graph.microsoft.com/calendars.read 
    https://graph.microsoft.com/mail.send
```


| Parametre     | Koşul     | Açıklama                                                                               |
|--------------:|--------------:|:-----------------------------------------------------------------------------------------:|
| `tenant` | Gerekli | İzin istemek istediğiniz dizin kiracısı. GUID veya dost ad biçiminde sağlanabilir VEYA `organizations` genel olarak örnekte görüldüğü gibi başvurulabilir. Kişisel hesaplar kiracı bağlamı dışında yönetici onayı sağlayamadığıiçin 'ortak' kullanmayın. Kiracıları yöneten kişisel hesaplarla en iyi uyumluluğu sağlamak için, mümkün olduğunda kiracı kimliğini kullanın. |
| `client_id` | Gerekli | Azure portalı - Uygulama [kayıtlarının](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atandığı **Uygulama (istemci) Kimliği.** |
| `redirect_uri` | Gerekli |UYGULAMANIZIN işlemesi için yanıtın gönderilmesini istediğiniz uri'yi yeniden yönlendirin. Uygulama kayıt portalına kaydettiğiniz yönlendirme URL'lerinden biriyle tam olarak eşleşmelidir. |
| `state` | Önerilen | Belirteç yanıtında da döndürülecek isteğe dahil edilen bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada kodlamak için durumu (örneğin, üzerinde oldukları sayfa veya görünüm) kullanın. |
|`scope`        | Gerekli      | Uygulama tarafından istenen izin kümesini tanımlar. Bu statik (/.default kullanarak) veya dinamik kapsamlar olabilir.  Bu OIDC kapsamları içerebilir`openid` `profile`( `email`, , . | 


Bu noktada, Azure AD isteği tamamlamak için bir kiracı yöneticinin oturum etmesini gerektirir. Yöneticiden `scope` parametrede istediğiniz tüm izinleri onaylaması istenir.  Statik ( )`/.default`bir değer kullandıysanız, v1.0 yönetici onayı bitiş noktası gibi çalışır ve uygulama için gerekli izinlerde bulunan tüm kapsamlar için onay ister.

### <a name="successful-response"></a>Başarılı yanıt

Yönetici uygulamanızın izinlerini onaylarsa, başarılı yanıt aşağıdaki gibi görünür:

```
http://localhost/myapp/permissions?admin_consent=True&tenant=fa00d692-e9c7-4460-a743-29f2956fd429&state=12345&scope=https%3a%2f%2fgraph.microsoft.com%2fCalendars.Read+https%3a%2f%2fgraph.microsoft.com%2fMail.Send
```

| Parametre         | Açıklama                                                                                       |
|------------------:|:-------------------------------------------------------------------------------------------------:|
| `tenant`| Başvurunuza istediği izinleri GUID biçiminde veren dizin kiracısı.|
| `state`           | İstekte yer alan ve belirteç yanıtında da döndürülecek bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada kodlamak için kullanılır(örneğin, üzerinde oldukları sayfa veya görünüm).|
| `scope`          | Uygulama için erişim izni verilen izinler kümesi.|
| `admin_consent`   | Olarak `True`ayarlanacak.|

### <a name="error-response"></a>Hata yanıtı

`http://localhost/myapp/permissions?error=consent_required&error_description=AADSTS65004%3a+The+resource+owner+or+authorization+server+denied+the+request.%0d%0aTrace+ID%3a+d320620c-3d56-42bc-bc45-4cdd85c41f00%0d%0aCorrelation+ID%3a+8478d534-5b2c-4325-8c2c-51395c342c89%0d%0aTimestamp%3a+2019-09-24+18%3a34%3a26Z&admin_consent=True&tenant=fa15d692-e9c7-4460-a743-29f2956fd429&state=12345`

Başarılı bir yanıtta görülen parametrelere eklenen hata parametreleri aşağıda görülmektedir.

| Parametre          | Açıklama                                                                                      |
|-------------------:|:-------------------------------------------------------------------------------------------------:|
| `error`            | Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi.|
| `error_description`| Bir geliştiricinin hatanın temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi.|
| `tenant`| Başvurunuza istediği izinleri GUID biçiminde veren dizin kiracısı.|
| `state`           | İstekte yer alan ve belirteç yanıtında da döndürülecek bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada kodlamak için kullanılır(örneğin, üzerinde oldukları sayfa veya görünüm).|
| `admin_consent`   | Bu yanıtın `True` yönetici onay akışında oluştuğunu belirtmek üzere ayarlanır.|

## <a name="next-steps"></a>Sonraki adımlar
- [Bir uygulamayı çok kiracı lı olarak nasıl dönüştüreceğinizi](howto-convert-app-to-be-multi-tenant.md) görün
- Yetkilendirme [kodu hibe akışı sırasında OAuth 2.0 protokol katmanında rızanın nasıl desteklenilebildiğini](v2-oauth2-auth-code-flow.md#request-an-authorization-code)öğrenin.
- [Çok kiracılı bir uygulamanın daha](active-directory-devhowto-multi-tenant-overview.md) gelişmiş çok katmanlı uygulama modellerini destekleyerek "kullanıcı" ve "yönetici" onayı uygulamak için onay çerçevesini nasıl kullanabileceğini öğrenin.
- [Azure AD uygulama onay deneyimlerini](application-consent-experience.md) anlama
