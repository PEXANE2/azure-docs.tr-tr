---
title: Web uygulaması öğreticisi-FHıR için Azure API 'sine bağlanmayı sınama
description: Bu öğreticiler basit bir Web uygulaması dağıtma örneği hakkında yol gösterir. Öğreticinin bu bölümü, FHıR sunucusuna Postman ile bağlanma sınamasını açıklar
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 01/03/2020
ms.openlocfilehash: 1c64468a2e420734ca51a5b9308bb52e13712c51
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852931"
---
# <a name="testing-the-fhir-api"></a>FHıR API 'sini test etme
Önceki iki adımda, FHıR için Azure API 'yi dağıttınız ve istemci uygulamanızı kaydettiniz. Artık, FHıR için Azure API 'nizin istemci uygulamanızla birlikte ayarlanmış olduğunu test etmeye hazırsınız. 

## <a name="retrieve-capability-statement"></a>Yetenek bildirisini al
İlk olarak, FHıR için Azure API 'niz için yetenek beyanını alacaksınız. 
1. Postman 'ı aç
1. GET https://. azurehealthcareapis.com/metadata yaparak yetenek ifadesini alın \<FHIR-SERVER-NAME> . Aşağıdaki görüntüde FHıR sunucu adı **fhirserver**' dır.

![Capability ekstresi](media/tutorial-web-app/postman-capability-statement.png)

Bir sonraki adımda, hasta alma girişiminde bulunuldu. Hasta almak için GET https:// \<FHIR-SERVER-NAME> . azurehealthcareapis.com/Patient girin. 401 Yetkisiz bir hata alacaksınız. Bu hata, hasta verilerine erişiminizin olması gerektiğini kanıtlamadınız.

## <a name="get-patient-from-fhir-server"></a>FHıR sunucusundan hasta alın
![Başarısız hasta](media/tutorial-web-app/postman-patient-authorization-failed.png)

Erişim kazanmak için bir erişim belirtecine ihtiyacınız vardır.
1. Postman 'da **Yetkilendirme** ' yi seçin ve türü **OAuth 2.0** olarak ayarlayın
1. **Yeni erişim belirteci al** ' ı seçin
1. Alanları doldurup **Istek belirteci**' ni seçin. Bu öğreticide her bir alan için değerleri görebilirsiniz.

|Alan                |Değer                                                               |
|---------------------|--------------------------------------------------------------------|
|Belirteç Adı           |Belirteciniz için bir ad                                               |
|Veriliş Türü           |Yetkilendirme kodu                                                  |
|Geri Çağırma URL’si         |https://www.getpostman.com/oauth2/callback                          |
|Kimlik Doğrulama URL’si             |https://login.microsoftonline.com/\<AZURE-AD-TENANT-ID>/OAuth2/? kaynağı = https:// \<FHIR-SERVER-NAME> . azurehealthcareapis.com|
|Erişim Belirteci URL’si     |https://login.microsoftonline.com/\<AZURE-AD-TENANT-ID>/OAuth2/Token|
|İstemci Kimliği            |Önceki adımlarda kopyaladığınız istemci KIMLIĞI             |
|İstemci Gizli Anahtarı        |\<BLANK>                                                            |
|Kapsam                |\<BLANK>                                                            |
|Durum                |1234                                                                |
|İstemci Kimlik Doğrulaması|İstemci kimlik bilgilerini gövdesinde gönder                                     |

4. Kimlik bilgilerinizle oturum açın ve **kabul et** ' i seçin
1. Sonucu aşağı kaydırın ve **belirteci kullan** ' ı seçin.
1. En üstte **Gönder** ' i ve bu kez bir sonuç elde etmeniz gerekir ![](media/tutorial-web-app/postman-patient-authorization-success.png)

## <a name="post-patient-into-fhir-server"></a>FHıR sunucusuna hasta gönder
Artık erişiminiz var, yeni bir hasta oluşturabilirsiniz. FHıR sunucunuza ekleyebileceğiniz basit bir hasta örneği aşağıda verilmiştir. Aşağıdaki kodu Postman 'nın **gövde** bölümüne girin.

``` json
    {
        "resourceType": "Patient",
        "active": true,
        "name": [
            {
                "use": "official",
                "family": "Kirk",
                "given": [
                    "James",
                    "Tiberious"
                ]
            },
            {
                "use": "usual",
                "given": [
                    "Jim"
                ]
            }
        ],
        "gender": "male",
        "birthDate": "1960-12-25"
    }
```
Bu GÖNDERI, FHıR sunucunuzda James Tiberemkirk adında yeni bir hasta oluşturacak.
![Hasta gönder](media/tutorial-web-app/postman-post-patient.png)

Bir hasta almak için yukarıdaki alma adımını yaparsanız, çıktıda James Tiber, Kirk listelendiğini görürsünüz.

## <a name="troubleshooting-access-issues"></a>Erişim sorunlarını giderme
Bu adımların herhangi biri sırasında sorunlarla karşılaşırsanız, Azure Active Directory birlikte yerleştirdiğimiz belgeleri ve FHıR için Azure API 'sini gözden geçirin. 

* [Fhır Için Azure AD ve Azure API 'si](azure-ad-hcapi.md) -bu belge, Azure Active Directory temel prensiplerin ve fhır 'nın Azure API 'siyle nasıl etkileşime gireceğini özetler.
* [Erişim belirteci doğrulaması](azure-ad-hcapi-token-validation.md) -bu nasıl yapılır Kılavuzu, erişim belirteci doğrulaması ve erişim sorunlarını çözmek için gerçekleştirilecek adımlar hakkında daha ayrıntılı bilgiler sunar.

## <a name="next-steps"></a>Sonraki Adımlar
İstemci uygulamanıza başarıyla bağlandığınıza göre, Web uygulamanızı yazmaya hazırsınız.

>[!div class="nextstepaction"]
>[Web uygulaması yazma](tutorial-web-app-write-web-app.md)



