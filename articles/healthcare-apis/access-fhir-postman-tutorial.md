---
title: Azure 'da Postman FHıR sunucusu-FHıR için Azure API
description: Bu öğreticide, bir FHıR sunucusuna erişmek için Postman 'ı kullanmak için gereken adımları adım adım göstereceğiz. Postman, API 'Lere erişen uygulamalarda hata ayıklama için yararlıdır.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: dseven
ms.author: matjazl
author: matjazl
ms.date: 02/07/2019
ms.openlocfilehash: f8b5e344fc963d466571e75ff16f17367dc32971
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87844856"
---
# <a name="access-azure-api-for-fhir-with-postman"></a>Postman ile FHıR için Azure API 'sine erişme

İstemci uygulaması bir [REST API](https://www.hl7.org/fhir/http.html)aracılığıyla bir FHıR API 'sine erişir. Ayrıca, uygulamalar oluştururken, örneğin hata ayıklama amacıyla, FHıR sunucusuyla doğrudan etkileşim kurmak isteyebilirsiniz. Bu öğreticide, bir FHıR sunucusuna erişmek için [Postman](https://www.getpostman.com/) 'ı kullanmak için gereken adımları adım adım göstereceğiz. Postman, API 'Lere erişen uygulamalar oluştururken genellikle hata ayıklama için kullanılan bir araçtır.

## <a name="prerequisites"></a>Önkoşullar

- Azure 'da bir FHıR uç noktası. Azure için, FHıR için yönetilen Azure API 'sini veya açık kaynaklı FHıR sunucusunu kullanarak bu ayarı yapabilirsiniz. [Azure Portal](fhir-paas-portal-quickstart.md), [POWERSHELL](fhir-paas-powershell-quickstart.md)veya [Azure CLI](fhir-paas-cli-quickstart.md)kullanarak FHıR için yönetilen Azure API 'sini ayarlayın.
- FHıR hizmetine erişmek için kullanacağınız bir [istemci uygulaması](register-confidential-azure-ad-client-app.md)
- Postman yüklendi. Buradan edinebilirsiniz[https://www.getpostman.com](https://www.getpostman.com)

## <a name="fhir-server-and-authentication-details"></a>FHıR sunucusu ve kimlik doğrulaması ayrıntıları

Postman kullanabilmek için aşağıdaki ayrıntılar gereklidir:

- FHıR sunucu URL 'niz, örneğin`https://MYACCOUNT.azurehealthcareapis.com`
- `Authority`Fhır sunucunuzun kimlik sağlayıcısı, örneğin,`https://login.microsoftonline.com/{TENANT-ID}`
- Yapılandırıldı `audience` . Bu genellikle FHıR sunucusunun URL 'sidir. Örneğin, `https://MYACCOUNT.azurehealthcareapis.com` yalnızca `https://azurehealthcareapis.com` .
- `client_id`Fhır hizmetine erişmek için kullanacağınız [istemci uygulamasının](register-confidential-azure-ad-client-app.md) (veya uygulama kimliği).
- `client_secret`İstemci uygulamasının (veya uygulama gizli dizisi).

Son olarak, bunun `https://www.getpostman.com/oauth2/callback` istemci uygulamanız için kayıtlı bir yanıt URL 'si olduğunu denetlemeniz gerekir.

## <a name="connect-to-fhir-server"></a>FHıR sunucusuna bağlan

Postman kullanarak şunları yapmak `GET` için bir istek yapın `https://fhir-server-url/metadata` :

![Postman meta verileri yetenek beyanı](media/tutorial-postman/postman-metadata.png)

FHıR için Azure API 'nin meta veri URL 'SI `https://MYACCOUNT.azurehealthcareapis.com/metadata` . Bu örnekte, FHıR sunucu URL 'SI `https://fhirdocsmsft.azurewebsites.net` ve sunucunun yetenek beyanı ' nda bulunabilir `https://fhirdocsmsft.azurewebsites.net/metadata` . Bu uç noktaya kimlik doğrulaması olmadan erişilebilir olmalıdır.

Kısıtlanmış kaynaklara erişmeyi denerseniz, "kimlik doğrulama başarısız oldu" yanıtını almanız gerekir:

![Kimlik doğrulama başarısız](media/tutorial-postman/postman-authentication-failed.png)

## <a name="obtaining-an-access-token"></a>Erişim belirteci alma

Geçerli bir erişim belirteci almak için "yetkilendirme" ve "OAuth 2,0" tür seçin:

![OAuth 2,0 ayarla](media/tutorial-postman/postman-select-oauth2.png)

"Yeni erişim belirteci al" düğmesine basın ve bir iletişim kutusu görünür:

![Yeni erişim belirteci iste](media/tutorial-postman/postman-request-token.png)

Bazı ayrıntılara ihtiyacınız olacak:

| Alan                 | Örnek Değer                                                                                                   | Yorum                    |
|-----------------------|-----------------------------------------------------------------------------------------------------------------|----------------------------|
| Belirteç Adı            | MYTOKEN                                                                                                         | Seçtiğiniz bir ad          |
| Veriliş Türü            | Yetkilendirme kodu                                                                                              |                            |
| Geri Çağırma URL’si          | `https://www.getpostman.com/oauth2/callback`                                                                      |                            |
| Kimlik Doğrulama URL’si              | `https://login.microsoftonline.com/{TENANT-ID}/oauth2/authorize?resource=<audience>` | `audience``https://MYACCOUNT.azurehealthcareapis.com`fhır Için Azure API 'si içindir |
| Erişim Belirteci URL’si      | `https://login.microsoftonline.com/{TENANT ID}/oauth2/token`                                                      |                            |
| İstemci Kimliği             | `XXXXXXXX-XXX-XXXX-XXXX-XXXXXXXXXXXX`                                                                            | Uygulama Kimliği             |
| İstemci Gizli Anahtarı         | `XXXXXXXX`                                                                                                        | Gizli anahtar istemci anahtarı          |
| Kapsam | `<Leave Blank>` |
| Durum                 | `1234`                                                                                                            |                            |
| İstemci Kimlik Doğrulaması | İstemci kimlik bilgilerini gövdesinde gönder                                                                                 |                 

"Istek belirteci" ' ne gidin ve Azure Active Directory kimlik doğrulama akışı üzerinden size kılavuzluk eder ve Postman 'ya bir belirteç döndürülür. Sorunlarla karşılaşırsanız, Postman konsolunu açın ("görünüm->göster Postman konsolundan" menü öğesi).

Döndürülen belirteç ekranında aşağı kaydırın ve "belirteç kullan" düğmesine basın:

![Belirteç kullan](media/tutorial-postman/postman-use-token.png)

Belirtecin şimdi "erişim belirteci" alanında doldurulması gerekir ve "kullanılabilir belirteçlerden" belirteçleri seçebilirsiniz. Kaynak aramasını tekrarlamak için "gönderirseniz" `Patient` bir durum almalısınız `200 OK` :

![200 TAMAM](media/tutorial-postman/postman-200-OK.png)

Bu durumda, veritabanında hastalar yoktur ve arama boştur.

Erişim belirtecini gibi bir araçla [https://jwt.ms](https://jwt.ms) karşılaşırsanız, şunun gibi bir içerik görmeniz gerekir:

```jsonc
{
  "aud": "https://MYACCOUNT.azurehealthcareapis.com",
  "iss": "https://sts.windows.net/{TENANT-ID}/",
  "iat": 1545343803,
  "nbf": 1545343803,
  "exp": 1545347703,
  "acr": "1",
  "aio": "AUQAu/8JXXXXXXXXXdQxcxn1eis459j70Kf9DwcUjlKY3I2G/9aOnSbw==",
  "amr": [
    "pwd"
  ],
  "appid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "oid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "appidacr": "1",

  ...// Truncated
}
```

Sorun giderme durumlarında, doğru hedef kitleye ( `aud` talep) başlamak için iyi bir yerdir. Belirteciniz doğru sertifika verenden ( `iss` talep) ve doğru hedef kitleye ( `aud` talep) sahipse, ancak yıne de FHıR API 'sine erişemiyorsanız, Kullanıcı veya hizmet sorumlusu ( `oid` talep) fhır veri düzlemine erişemez. Kullanıcılara veri düzlemi rolleri atamak için [Azure rol tabanlı erişim denetimi (Azure RBAC) kullanmanızı](configure-azure-rbac.md) öneririz. Veri düzleiniz için harici, ikincil bir Azure Active Directory kiracısı kullanıyorsanız, [Yerel RBAC atamalarını yapılandırmanız](configure-local-rbac.md)gerekecektir.

[Azure CLI kullanarak fhır Için Azure API için bir belirteç almak](get-healthcare-apis-access-token-cli.md)da mümkündür. Azure CLı ile alınan bir belirteç kullanıyorsanız, "taşıyıcı belirteci" Yetkilendirme türünü kullanmanız ve belirteci doğrudan yapıştırmanız gerekir.

## <a name="inserting-a-patient"></a>Hasta ekleme

Artık geçerli bir erişim belirteciniz olduğuna göre. Yeni bir hasta ekleyebilirsiniz. "POST" yöntemine geçin ve isteğin gövdesine aşağıdaki JSON belgesini ekleyin:

[!code-json[](samples/sample-patient.json)]

"Gönder" düğmesine basın ve hasta 'in başarıyla oluşturulduğunu görmeniz gerekir:

![Hasta oluşturuldu](media/tutorial-postman/postman-patient-created.png)

Hasta aramasını yinelediğiniz takdirde hasta kaydını görmeniz gerekir:

![Hasta oluşturuldu](media/tutorial-postman/postman-patient-found.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Postman kullanarak bir FHıR API 'sine eriştiniz. Desteklenen özellikler bölümümüzde desteklenen API özellikleri hakkında bilgi edinin.
 
>[!div class="nextstepaction"]
>[Desteklenen özellikler](fhir-features-supported.md)
