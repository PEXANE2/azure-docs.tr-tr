---
title: Microsoft Identity Platform Yöneticisi onay protokolleri
description: Microsoft Identity platform uç noktasındaki kapsamlar, izinler ve onay dahil olmak üzere yetkilendirme açıklaması.
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
ms.openlocfilehash: cb629b80958ed2897f76eb099f738c33b48c3696
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88119615"
---
# <a name="admin-consent-on-the-microsoft-identity-platform"></a>Microsoft Identity platformunda yönetici onayı

Bazı izinler, bir kiracı içinde verilebilmesi için önce bir yöneticiden izin gerektirir.  Bir kiracının tamamına izin vermek için yönetici onay uç noktasını da kullanabilirsiniz.

## <a name="recommended-sign-the-user-into-your-app"></a>Önerilir: Kullanıcı uygulamanızda Imzalanın

Genellikle, yönetici onay uç noktasını kullanan bir uygulama oluşturduğunuzda, uygulamanın, yöneticinin uygulamanın izinlerini onaylayabileceği bir sayfa veya görünüm gerekir. Bu sayfa, uygulamanın kaydolma akışının bir parçası, uygulamanın ayarlarının bir parçası olabilir veya adanmış bir "Connect" akışı olabilir. Çoğu durumda, uygulamanın bu "Bağlan" görünümünü yalnızca bir kullanıcı iş veya okul Microsoft hesabı oturum açtıktan sonra göstermesini mantıklı hale getirir.

Kullanıcıyı uygulamanıza imzaladığınızda, yöneticinin gerekli izinleri onaylamasını istemeden önce sahip olduğu kuruluşu belirleyebilirsiniz. Kesinlikle gerekli olmasa da, kurumsal kullanıcılarınız için daha sezgisel bir deneyim oluşturmanıza yardımcı olabilir. Kullanıcı oturumu açmak için [Microsoft Identity platform protokol öğreticilerimizi](active-directory-v2-protocols.md)izleyin.

## <a name="request-the-permissions-from-a-directory-admin"></a>Dizin yöneticisinden izinleri isteme

Kuruluşunuzun yöneticisinden izin istemek için hazırsanız, kullanıcıyı Microsoft Identity Platform *Yöneticisi onay uç noktasına*yönlendirebilirsiniz.

```HTTP
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
| `tenant` | Gerekli | İzin istemek istediğiniz dizin kiracısı. , Örnekte görüldüğü gibi, GUID veya kolay ad biçiminde veya genel olarak başvuruda bulunulan `organizations` şekilde belirtilebilir. Kişisel hesaplar kiracı bağlamı haricinde yönetici onayı sağlayamadığından ' Common ' kullanmayın. Kiracıların yönetiminde kişisel hesaplarla en iyi uyumluluğu sağlamak için, mümkün olduğunda kiracı KIMLIĞINI kullanın. |
| `client_id` | Gerekli | [Azure Portal – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminin uygulamanıza atandığı **uygulama (istemci) kimliği** . |
| `redirect_uri` | Gerekli |Uygulamanızın işlenmesi için yanıtın gönderilmesini istediğiniz yeniden yönlendirme URI 'SI. Uygulama kayıt portalı 'nda kaydettiğiniz yeniden yönlendirme URI 'lerinden biriyle tam olarak eşleşmesi gerekir. |
| `state` | Önerilen | İsteğin belirteç yanıtında de döndürülecek bir değer. İstediğiniz herhangi bir içerik dizesi olabilir. Kullanıcının uygulamadaki durumuyla ilgili bilgileri, uygulamanın bulunduğu sayfa veya görünüm gibi kimlik doğrulama isteği olmadan önce kodlamak için bu durumu kullanın. |
|`scope`        | Gerekli      | Uygulama tarafından istenen izin kümesini tanımlar. Bu, statik (//varsayılan kullanılarak) veya dinamik kapsamlar olabilir.  Bu, OıDC kapsamlarını ( `openid` , `profile` ,) içerebilir `email` . |


Bu noktada, Azure AD 'nin isteği tamamlaması için bir kiracı yöneticisinin oturum açması gerekir. Yöneticinin, parametresinde istediğiniz tüm izinleri onaylaması istenir `scope` .  Statik ( `/.default` ) değeri kullandıysanız, bu, uygulama için gerekli izinlerde bulunan tüm kapsamlar için v 1.0 Yönetici onay uç noktası ve istek onayı gibi çalışır.

### <a name="successful-response"></a>Başarılı yanıt

Yönetici, uygulamanız için izinleri onayladığında, başarılı yanıt şöyle görünür:

```
http://localhost/myapp/permissions?admin_consent=True&tenant=fa00d692-e9c7-4460-a743-29f2956fd429&state=12345&scope=https%3a%2f%2fgraph.microsoft.com%2fCalendars.Read+https%3a%2f%2fgraph.microsoft.com%2fMail.Send
```

| Parametre         | Açıklama                                                                                       |
|------------------:|:-------------------------------------------------------------------------------------------------:|
| `tenant`| Uygulamanıza istenen izinleri (GUID biçiminde) veren dizin kiracısı.|
| `state`           | İstekte bulunan ve belirteç yanıtında de döndürülen bir değer. İstediğiniz herhangi bir içerik dizesi olabilir. Durum, kullanıcının uygulamadaki durumu hakkında bilgi kodlamak için kullanılır; Örneğin, bulunan sayfa veya görünüm gibi kimlik doğrulama isteği gerçekleştirilmeden önce.|
| `scope`          | Uygulamasına erişim izni verilen izinler kümesi.|
| `admin_consent`   | , Olarak ayarlanır `True` .|

### <a name="error-response"></a>Hata yanıtı

`http://localhost/myapp/permissions?error=consent_required&error_description=AADSTS65004%3a+The+resource+owner+or+authorization+server+denied+the+request.%0d%0aTrace+ID%3a+d320620c-3d56-42bc-bc45-4cdd85c41f00%0d%0aCorrelation+ID%3a+8478d534-5b2c-4325-8c2c-51395c342c89%0d%0aTimestamp%3a+2019-09-24+18%3a34%3a26Z&admin_consent=True&tenant=fa15d692-e9c7-4460-a743-29f2956fd429&state=12345`

Başarılı bir yanıtta görülen parametrelere ekleme, hata parametreleri aşağıda gösterildiği gibi görülür.

| Parametre          | Açıklama                                                                                      |
|-------------------:|:-------------------------------------------------------------------------------------------------:|
| `error`            | Oluşan hata türlerini sınıflandırmak için kullanılabilen ve hatalara yanıt vermek için kullanılabilen bir hata kodu dizesi.|
| `error_description`| Bir geliştiricinin hatanın kök nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi.|
| `tenant`| Uygulamanıza istenen izinleri (GUID biçiminde) veren dizin kiracısı.|
| `state`           | İstekte bulunan ve belirteç yanıtında de döndürülen bir değer. İstediğiniz herhangi bir içerik dizesi olabilir. Durum, kullanıcının uygulamadaki durumu hakkında bilgi kodlamak için kullanılır; Örneğin, bulunan sayfa veya görünüm gibi kimlik doğrulama isteği gerçekleştirilmeden önce.|
| `admin_consent`   | , `True` Bu yanıtın yönetici onay akışında oluştuğunu göstermek için olarak ayarlanır.|

## <a name="next-steps"></a>Sonraki adımlar
- Bkz. [bir uygulamayı çok kiracılı olarak dönüştürme](howto-convert-app-to-be-multi-tenant.md)
- [Yetkilendirme kodu verme akışı sırasında OAuth 2,0 protokol katmanında izin nasıl desteklendiğini](v2-oauth2-auth-code-flow.md#request-an-authorization-code)öğrenin.
- Çok kiracılı bir uygulamanın "Kullanıcı" ve "Yönetici" onayını uygulamak için, daha gelişmiş çok katmanlı uygulama düzenlerini desteklemek için [izin çerçevesini nasıl kullanabileceği hakkında](./howto-convert-app-to-be-multi-tenant.md) bilgi edinin.
- [Azure AD uygulama onayı deneyimlerini](application-consent-experience.md) anlama