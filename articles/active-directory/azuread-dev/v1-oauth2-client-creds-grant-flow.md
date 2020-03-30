---
title: OAuth2.0 kullanarak Hizmet Auth Için Azure AD Hizmeti | Microsoft Dokümanlar
description: Bu makalede, OAuth2.0 istemci kimlik bilgileri hibe akışını kullanarak hizmet kimlik doğrulaması uygulamak için HTTP iletileri nasıl kullanılacağı açıklanmaktadır.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 02/08/2017
ms.author: ryanwi
ms.reviewer: nacanuma
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: f2d1eaec80c8925eb7b38af848e29e944f1ebf69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154551"
---
# <a name="service-to-service-calls-using-client-credentials-shared-secret-or-certificate"></a>İstemci kimlik bilgilerini kullanarak hizmet çağrılarına hizmet (paylaşılan gizli veya sertifika)

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

OAuth 2.0 Müşteri Kimlik Bilgileri Hibe Akışı, bir web hizmetinin *(gizli istemci)* kullanıcı kimliği yerine kendi kimlik bilgilerini kullanmasına, başka bir web hizmetini ararken kimlik doğrulamasına izin verir. Bu senaryoda, istemci genellikle bir orta katman web hizmeti, bir daemon hizmeti veya web sitesidir. Azure AD, daha yüksek bir güvence düzeyi için, arama hizmetinin bir sertifikayı (paylaşılan bir sır yerine) kimlik bilgisi olarak kullanmasına da izin verir.

## <a name="client-credentials-grant-flow-diagram"></a>İstemci kimlik bilgileri hibe akış diyagramı
Aşağıdaki diyagram, Azure Etkin Dizini'nde (Azure AD) istemci kimlik bilgileri hibe akışının nasıl çalıştığını açıklar.

![OAuth2.0 Müşteri Kimlik Bilgileri Hibe Akışı](./media/v1-oauth2-client-creds-grant-flow/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. İstemci uygulaması Azure AD belirteci verme bitiş noktasına doğrular ve bir erişim belirteci ister.
2. Azure AD belirteci verme bitiş noktası erişim belirteci sorunları.
3. Erişim belirteci, güvenli kaynağa kimlik doğrulamak için kullanılır.
4. Güvenli kaynaktan gelen veriler istemci uygulamasına döndürülür.

## <a name="register-the-services-in-azure-ad"></a>Hizmetleri Azure AD'de Kaydedin
Hem arama hizmetini hem de alıcı hizmeti Azure Etkin Dizini'ne (Azure AD) kaydedin. Ayrıntılı yönergeler için [bkz.](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)

## <a name="request-an-access-token"></a>Erişim Jetonu İste
Erişim jetonu istemek için, kiracıya özgü Azure AD bitiş noktasına bir HTTP POST kullanın.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Servisten hizmete erişim jeton isteği
İstemci uygulamasının paylaşılan bir sır veya sertifika tarafından güvence altına alınıp alınmayacağına bağlı olarak iki durum vardır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durum: Paylaşılan bir sırla giriş belirteç isteği
Paylaşılan bir gizli kullanırken, servisten hizmete erişim belirteç isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |gerekli |İstenen hibe türünü belirtir. İstemci Kimlik Bilgileri Hibe akışında, değer **client_credentials**olmalıdır. |
| client_id |gerekli |Arama web hizmetinin Azure AD istemci kimliğini belirtir. Arama uygulamasının istemci kimliğini bulmak için [Azure portalında](https://portal.azure.com)Azure **Active Directory'yi**tıklatın, **Uygulama kayıtlarını**tıklatın, uygulamayı tıklatın. client_id Başvuru *Kimliği* |
| client_secret |gerekli |Azure AD'de arama web hizmeti veya daemon uygulaması için kayıtlı bir anahtar girin. Azure portalında bir anahtar oluşturmak için **Azure Active Directory'yi**tıklatın, **Uygulama kayıtlarını**tıklatın, uygulamayı tıklatın, **Ayarlar'ı**tıklatın, **Tuşlar'ı**tıklatın ve bir Anahtar ekleyin.  Url'yi sağlarken bu sırrı kodlayın. |
| kaynak |gerekli |Alıcı web hizmetinin App ID URI'sini girin. Azure portalında App ID URI'yi bulmak için **Azure Active Directory'yi**tıklatın, **Uygulama kayıtlarını**tıklatın, hizmet uygulamasını tıklatın ve ardından **Ayarlar** ve **Özellikler'i**tıklatın. |

#### <a name="example"></a>Örnek
Aşağıdaki HTTP POST, web hizmeti `https://service.contoso.com/` için bir [erişim belirteci](../develop/access-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) talep ediyor. Erişim `client_id` belirteci isteyen web hizmetini tanımlar.

```
POST /contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: Sertifikayla giriş isteği
Sertifikalı servisten hizmete erişim belirteç isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |gerekli |İstenen yanıt türünü belirtir. İstemci Kimlik Bilgileri Hibe akışında, değer **client_credentials**olmalıdır. |
| client_id |gerekli |Arama web hizmetinin Azure AD istemci kimliğini belirtir. Arama uygulamasının istemci kimliğini bulmak için [Azure portalında](https://portal.azure.com)Azure **Active Directory'yi**tıklatın, **Uygulama kayıtlarını**tıklatın, uygulamayı tıklatın. client_id Başvuru *Kimliği* |
| client_assertion_type |gerekli |Değer,`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |gerekli | Uygulamanız için kimlik bilgileri olarak kaydettiğiniz sertifikayı oluşturmanız ve imzalamanız gereken bir iddia (JSON Web Jetonu). Sertifikanızı ve belgebiçiminizi nasıl kaydedebilirsiniz öğrenmek için [sertifika kimlik bilgilerini](../develop/active-directory-certificate-credentials.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) okuyun.|
| kaynak | gerekli |Alıcı web hizmetinin App ID URI'sini girin. Azure portalında App ID URI'yi bulmak için **Azure Active Directory'yi**tıklatın, **Uygulama kayıtlarını**tıklatın, hizmet uygulamasını tıklatın ve ardından **Ayarlar** ve **Özellikler'i**tıklatın. |

Client_secret parametrenin iki parametreyle değiştirilmesi dışında, parametrenin paylaşılan gizli istek durumunda ki parametrelerle hemen hemen aynı olduğuna dikkat edin: client_assertion_type ve client_assertion.

#### <a name="example"></a>Örnek
Aşağıdaki HTTP POST, sertifikalı web `https://service.contoso.com/` hizmeti için bir erişim jetonu ister. Erişim `client_id` belirteci isteyen web hizmetini tanımlar.

```
POST /<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

resource=https%3A%2F%contoso.onmicrosoft.com%2Ffc7664b4-cdd6-43e1-9365-c2e1c4e1b3bf&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

### <a name="service-to-service-access-token-response"></a>Servise Hizmete Erişim Belirteç Yanıtı

Bir başarı yanıtı aşağıdaki parametreleri içeren bir JSON OAuth 2.0 yanıtı içerir:

| Parametre | Açıklama |
| --- | --- |
| access_token |İstenen erişim jetonu. Arama web hizmeti, alıcı web hizmetinin kimliğini doğrulamak için bu belirteci kullanabilir. |
| token_type |Belirteç türü değerini gösterir. Azure AD'nin desteklediği **Bearer**tek tür Taşıyıcı'dır. Taşıyıcı belirteçleri hakkında daha fazla bilgi için Bkz. [OAuth 2.0 Yetkilendirme Çerçevesi: Taşıyıcı Belirteç Kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| expires_in |Erişim belirteci nin ne kadar süre geçerli olduğu (saniye cinsinden). |
| expires_on |Erişim belirteci süresinin dolduğu saat. Tarih, 1970-01-01T0:0:0Z UTC'den son kullanma tarihine kadar olan saniye sayısı olarak temsil edilir. Bu değer, önbelleğe alınmış belirteçlerin kullanım ömrünü belirlemek için kullanılır. |
| not_before |Erişim belirteci kullanılabilir hale geldiği zaman. Tarih, 1970-01-01T0:0:0Z UTC'den belirteç için geçerlilik süresine kadar olan saniye sayısı olarak temsil edilir.|
| kaynak |Alıcı web hizmetinin App Id URI'si. |

#### <a name="example-of-response"></a>Yanıt örneği
Aşağıdaki örnek, bir web hizmetine erişim belirteci isteğine verilen başarı yanıtını gösterir.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD'de OAuth 2.0](v1-protocols-oauth-code.md)
* [Hizmetin C#'ından örnek](https://github.com/Azure-Samples/active-directory-dotnet-daemon) olarak servis araması ile paylaşılan bir sır ve [C# örneği ile hizmetin bir sertifika ile servis çağrısına](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)
