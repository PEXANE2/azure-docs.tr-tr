---
title: FHıR proxy üzerinde akıllı Azure Active Directory
description: Bu öğreticide, FHıR için Azure API 'SI ile FHıR uygulamalarında akıllı etkinleştirme için bir proxy 'nin nasıl kullanılacağı açıklanmaktadır.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: dseven
ms.author: matjazl
author: matjazl
ms.date: 04/02/2019
ms.openlocfilehash: 2e13a9fc32964781dda07e5534e5cab79868ddf0
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87846985"
---
# <a name="tutorial-azure-active-directory-smart-on-fhir-proxy"></a>Öğretici: FHıR proxy üzerinde akıllı Azure Active Directory

[Fhır 'de akıllı](https://docs.smarthealthit.org/) , fhır sunucularıyla iş ortağı uygulamalarını ve fhır arabirimleri olan elektronik tıbbi kayıt sistemlerini tümleştirmeye yönelik bir dizi açık belirtimdir. Belirtimlerin başlıca amaçlarından biri, bir uygulamanın bir FHıR sunucusu için kimlik doğrulama uç noktalarını bulmasını ve bir kimlik doğrulama sırasını başlatmasını açıklamaktadır. 

Kimlik doğrulaması OAuth2 tabanlıdır. Ancak FHıR üzerinde akıllı, Azure Active Directory (Azure AD) ile hemen uyumlu olmayan parametre adlandırma kurallarını kullandığından, FHıR için Azure API 'sinin, fhır başlatma dizileri için akıllı bir alt kümesini sağlayan yerleşik bir Azure AD akıllı sunucusu vardır. Özellikle, proxy, [EHR başlatma sırasını](https://hl7.org/fhir/smart-app-launch/#ehr-launch-sequence)sunar.

Bu öğreticide, FHıR için Azure API 'SI ile FHıR uygulamalarında akıllı etkinleştirme için proxy 'nin nasıl kullanılacağı açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

- FHıR için Azure API örneği
- [.NET Core 2,2](https://dotnet.microsoft.com/download/dotnet-core/2.2)

## <a name="configure-azure-ad-registrations"></a>Azure AD kayıtlarını yapılandırma

FHıR üzerinde akıllı, `Audience` fhır HIZMETININ URI 'sine eşit bir tanımlayıcı URI 'si gerektirir. FHıR için Azure API 'sinin standart yapılandırması bir `Audience` değeri kullanır `https://azurehealthcareapis.com` . Bununla birlikte, FHıR hizmetinizin belirli URL 'siyle eşleşen bir değer de belirleyebilirsiniz (örneğin `https://MYFHIRAPI.azurehealthcareapis.com` ). Bu, FHıR proxy üzerinde akıllı ile çalışırken gereklidir.

Ayrıca, bir istemci uygulaması kaydına de ihtiyacınız olacaktır. FHıR uygulamalarında çoğu akıllı tek sayfalı JavaScript uygulamalardır. Bu nedenle, [genel bir Azure AD istemci uygulamasını](register-public-azure-ad-client-app.md)yapılandırma yönergelerini izlemeniz gerekir.

Bu adımları tamamladıktan sonra, şunları yapmanız gerekir:

- Rge hedef kitlesi olarak ayarlanan bir FHıR sunucusu `https://MYFHIRAPI.azurehealthcareapis.com` , `MYFHIRAPI` fhir örneği IÇIN Azure API 'nizin adıdır.
- Ortak bir istemci uygulama kaydı. Bu istemci uygulaması için uygulama KIMLIĞINI bir yere unutmayın.

## <a name="enable-the-smart-on-fhir-proxy"></a>FHıR proxy üzerinde akıllı etkinleştirme

Fhır **proxy 'sine yönelik akıllı ara sunucu** onay kutusunu seçerek fhır örneği IÇIN Azure API 'Niz Için **kimlik doğrulama** ayarları 'ndaki akıllı ' i etkinleştirin:

![FHıR proxy üzerinde akıllı etkinleştirme seçimleri](media/tutorial-smart-on-fhir/enable-smart-on-fhir-proxy.png)

## <a name="enable-cors"></a>CORS'yi etkinleştirme

FHıR uygulamalarında çoğu akıllı tek sayfalı JavaScript uygulamaları olduğundan, FHıR için Azure API 'SI için [çıkış noktaları arası kaynak paylaşımını (CORS) etkinleştirmeniz](configure-cross-origin-resource-sharing.md) gerekir:

![CORS 'yi etkinleştirme seçimleri](media/tutorial-smart-on-fhir/enable-cors.png)

## <a name="configure-the-reply-url"></a>Yanıt URL 'sini yapılandırma

FHıR proxy üzerinde akıllı, FHıR uygulaması ile Azure AD arasında bir aracı gibi davranır. Kimlik doğrulama yanıtı (kimlik doğrulama kodu), uygulamanın kendisi yerine FHıR proxy 'sine gitmelidir. Ardından Proxy, yanıtı uygulamaya iletir. 

Kimlik doğrulama kodunun bu iki aşamalı geçişi nedeniyle, Azure AD İstemci uygulamanızın yanıt URL 'sini (geri çağırma), akıllı FHıR proxy 'sine yönelik yanıt URL 'si ve akıllı on FHıR uygulamasının yanıt URL 'si birleşimi olan bir URL 'ye ayarlamanız gerekir. Birleşik yanıt URL 'SI şu biçimdedir:

```http
https://MYFHIRAPI.azurehealthcareapis.com/AadSmartOnFhirProxy/callback/aHR0cHM6Ly9sb2NhbGhvc3Q6NTAwMS9zYW1wbGVhcHAvaW5kZXguaHRtbA
```

Bu yanıtta, `aHR0cHM6Ly9sb2NhbGhvc3Q6NTAwMS9zYW1wbGVhcHAvaW5kZXguaHRtbA` fhır UYGULAMASıNıN Smart On yanıt URL 'SININ URL güvenli, Base64 ile kodlanmış bir sürümüdür. FHıR uygulama başlatıcısı için, uygulama yerel olarak çalıştığında, yanıt URL 'SI olur `https://localhost:5001/sampleapp/index.html` . 

Aşağıdaki gibi bir komut dosyası kullanarak Birleşik yanıt URL 'sini oluşturabilirsiniz:

```PowerShell
$replyUrl = "https://localhost:5001/sampleapp/index.html"
$fhirServerUrl = "https://MYFHIRAPI.azurewebsites.net"
$bytes = [System.Text.Encoding]::UTF8.GetBytes($ReplyUrl)
$encodedText = [Convert]::ToBase64String($bytes)
$encodedText = $encodedText.TrimEnd('=');
$encodedText = $encodedText.Replace('/','_');
$encodedText = $encodedText.Replace('+','-');

$newReplyUrl = $FhirServerUrl.TrimEnd('/') + "/AadSmartOnFhirProxy/callback/" + $encodedText
```

Yanıt URL 'sini daha önce Azure AD için oluşturduğunuz ortak istemci uygulamasına ekleyin:

![Ortak istemci için yapılandırılmış yanıt URL 'SI](media/tutorial-smart-on-fhir/configure-reply-url.png)

## <a name="get-a-test-patient"></a>Sınama hasta 'i alın

FHAR için Azure API 'sini ve FHıR proxy 'sini test etmek için veritabanında en az bir hasta olması gerekir. Henüz API ile etkileşim yüklemediyseniz ve veritabanında veri yoksa, hasta yüklemek için [Fhır API Postman öğreticisini](access-fhir-postman-tutorial.md) takip edin. Belirli bir hasta 'in KIMLIĞINI bir yere göz önünde bir şekilde oluşturun.

## <a name="download-the-smart-on-fhir-app-launcher"></a>FHıR uygulama başlatıcısı 'nı akıllı indirin

[Azure deposu için açık kaynaklı Fhır sunucusu](https://github.com/Microsoft/fhir-server) , basıt bir akıllı on fhır uygulama başlatıcısı ve BIR örnek Smart On fhır uygulaması içerir. Bu öğreticide, kurulumu test etmek için bu akıllı on FHıR Başlatıcısı ' nı yerel olarak kullanın.

GitHub deposunu kopyalayabilir ve şu komutları kullanarak uygulamaya gidebilirsiniz:

```PowerShell
git clone https://github.com/Microsoft/fhir-server
cd fhir-server/samples/apps/SmartLauncher
```

Uygulamanın, içinde ayarlayabileceğiniz birkaç yapılandırma ayarı olması gerekir `appsettings.json` :

```json
{
    "FhirServerUrl": "https://MYFHIRAPI.azurehealthcareapis.com",
    "ClientId": "APP-ID",
    "DefaultSmartAppUrl": "/sampleapp/launch.html"
}
```

Özelliğini kullanmanızı öneririz `dotnet user-secrets` :

```PowerShell
dotnet user-secrets set FhirServerUrl https://MYFHIRAPI.azurehealthcareapis.com
dotnet user-secrets set ClientId <APP-ID>
```

Uygulamayı çalıştırmak için şu komutu kullanın:

```PowerShell
dotnet run
```

## <a name="test-the-smart-on-fhir-proxy"></a>FHıR proxy üzerinde akıllı sınama

FHıR uygulama başlatıcısı üzerinde akıllı başlattıktan sonra, `https://localhost:5001` aşağıdaki ekranı görmeniz gereken yere tarayıcınızı işaret edebilirsiniz:

![FHıR uygulama başlatıcısı üzerinde akıllı](media/tutorial-smart-on-fhir/smart-on-fhir-app-launcher.png)

**Hasta**, **karşılaştığı**veya **uygulayıcı** bilgilerini girdiğinizde, **başlatma bağlamının** güncelleştirildiğini fark edersiniz. FHıR için Azure API 'yi kullanırken, başlatma bağlamı yalnızca hasta, uygulayıcı ve daha fazlası hakkında bilgi içeren bir JSON belgesidir. Bu başlatma bağlamı Base64 olarak kodlanır ve sorgu parametresi olarak SMART on FHıR uygulamasına geçirilir `launch` . FHıR belirtimine göre, bu değişken, FHıR uygulamasında akıllı olarak anlaşılmaz ve kimlik sağlayıcısına geçirilir. 

FHıR proxy üzerinde akıllı bu bilgileri, belirteç yanıtındaki alanları doldurmak için kullanır. FHıR uygulamasında akıllı uygulama, bu alanları, hangi hasta için veri isteğinde olduğunu ve ekrandaki uygulamayı nasıl işlediğini denetlemek *için kullanabilir.* FHıR proxy üzerinde akıllı, aşağıdaki alanları destekler:

* `patient`
* `encounter`
* `practitioner`
* `need_patient_banner`
* `smart_style_url`

Bu alanlar uygulama için rehberlik sağlamaktır, ancak herhangi bir güvenlik bilgisi iletmazlar. FHıR uygulamasında akıllı bir akıllı uygulama bunları yok sayabilir.

FHıR uygulama başlatıcısı 'ndaki akıllı öğesinin, sayfanın en altındaki **başlatma URL** bilgilerini güncelleştirdiğine dikkat edin. Örnek uygulamayı başlatmak için **Başlat** ' ı seçin ve bu örneğe benzer bir şey görmeniz gerekir:

![FHıR uygulamasında akıllı](media/tutorial-smart-on-fhir/smart-on-fhir-app.png)

Başlatma bağlam alanlarının uygulamaya nasıl geçtiğini görmek için belirteç yanıtını inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, FHıR proxy üzerinde akıllı Azure Active Directory yapılandırdınız. FHıR için Azure API 'SI ve Azure için açık kaynaklı FHıR sunucusu ile akıllı on FHıR uygulamalarının kullanımını araştırmak için GitHub 'daki FHıR sunucu örnekleri deposuna gidin:

>[!div class="nextstepaction"]
>[FHıR sunucu örnekleri](https://github.com/Microsoft/fhir-server-samples)
