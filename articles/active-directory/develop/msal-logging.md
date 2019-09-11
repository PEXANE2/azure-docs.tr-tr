---
title: MSAL uygulamalarında oturum açma | Mavisi
description: Microsoft kimlik doğrulama kitaplığı (MSAL) uygulamalarında günlüğe kaydetme hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/22/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02c093375ba2dc5c851a2deb35bdea28338ee982
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70135778"
---
# <a name="logging"></a>Günlüğe Kaydetme
Sorunları tanılamanıza ve ayrıntı sağlamanıza yardımcı olabilecek günlük iletileri oluşturmak için Microsoft kimlik doğrulama kitaplığı (MSAL) uygulamaları. Bir uygulama, birkaç satırlık kodla günlüğe kaydetmeyi yapılandırabilir ve ayrıntı düzeyi üzerinde özel denetime sahip olabilir ve kişisel ve kurumsal verilerin günlüğe kaydedilip kaydedilmeyeceğini belirtir. Bir MSAL günlüğe kaydetme geri çağırması ayarlamanız ve kullanıcıların kimlik doğrulama sorunlarıyla karşılaştıklarında günlükleri göndermesi için bir yol sağlamanız önerilir.

## <a name="logging-levels"></a>Günlüğe kaydetme düzeyleri

MSAL 'nin günlükçüsü, birkaç ayrıntı düzeyinin yakalamaya izin verir:

- Hata: Bir şeyin yanlış geçmiş olduğunu ve bir hatanın oluşturulduğunu gösterir. Hata ayıklama ve sorunları tanımlama için kullanın.
- Uyarı: Soru ve uygulama için daha fazla bilgi gerekiyor. Bir hata veya hata olması gerekmez, ancak tanılama ve işaret eden sorun sorunları için tasarlanmıştır.
- Bilgisine MSAL, bilgilendirici amaçlar için tasarlanan olayları günlüğe kaydetmek için gerekli değildir.
- Seçeneini Varsayılan. MSAL, büyük miktarda bilgiyi günlüğe kaydeder ve hangi kitaplık davranışının tüm ayrıntılarını verir.

## <a name="personal-and-organizational-data"></a>Kişisel ve Kurumsal veriler
Varsayılan olarak, MSAL günlükçüsü, son derece hassas kişisel veya kurumsal verileri yakalamaz. Kitaplığı, bunu yapmayı seçerseniz kişisel ve kurumsal verileri günlüğe kaydetmeyi etkinleştirme seçeneğini sağlar.

## <a name="logging-in-msalnet"></a>MSAL.NET içinde oturum açma

 > [!NOTE]
 > MSAL.NET hakkında daha fazla bilgi için [msal.net wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)'yi inceleyin. MSAL.NET günlük ve daha fazlasını örnekler alın. 
 
MSAL 3. x içinde, günlük kaydı `.WithLogging` Oluşturucu değiştiricisini kullanarak uygulama oluşturma sırasında uygulama başına ayarlanır. Bu yöntem isteğe bağlı parametreleri alır:

- *Düzey* , hangi günlük kayıt düzeyini istediğinize karar vermenize olanak sağlar. Hatalara ayarlandığında yalnızca hatalar alınır
- *Ilookupggingenabled* , true olarak ayarlandıysa kişisel ve kurumsal verileri günlüğe almanıza olanak sağlar. Bu, varsayılan olarak false değerine ayarlanır, böylece uygulamanız kişisel verileri günlüğe eklemez.
- *LogCallback* , günlüğü yapan bir temsilciye ayarlanır. *Pııvlıggingenabled* değeri true ise, bu yöntem iletileri iki kez alır: *containspii* parametresine sahip bir kez false ve kişisel veriler olmadan Ileti ve *containspıı* parametresi ile ikinci bir kez true değerine eşit ve ileti kişisel veri içeriyor olabilir. Bazı durumlarda (ileti kişisel veriler içermiyorsa), ileti aynı olur.
- *Defaultloggingenabled* , platform için varsayılan günlüğü etkinleştir. Varsayılan olarak false 'tur. Bunu true olarak ayarlarsanız, masaüstü/UWP uygulamalarında olay Izlemeyi kullanır, iOS üzerinde NSLog ve Android üzerinde Logcat.

```csharp
class Program
 {
  private static void Log(LogLevel level, string message, bool containsPii)
  {
     if (containsPii)
     {
        Console.ForegroundColor = ConsoleColor.Red;
     }
     Console.WriteLine($"{level} {message}");
     Console.ResetColor();
  }

  static void Main(string[] args)
  {
    var scopes = new string[] { "User.Read" };

    var application = PublicClientApplicationBuilder.Create("<clientID>")
                      .WithLogging(Log, LogLevel.Info, true)
                      .Build();

    AuthenticationResult result = application.AcquireTokenInteractive(scopes)
                                             .ExecuteAsync().Result;
  }
 }
 ```

 ## <a name="logging-in-msaljs"></a>MSAL. js ' de günlüğe kaydetme

 `UserAgentApplication` Örnek oluşturmak için yapılandırma sırasında bir günlükçü nesnesi geçirerek msal. js ' de günlük kaydını etkinleştirebilirsiniz. Bu günlükçü nesnesi aşağıdaki özelliklere sahiptir:

- *Localcallback*: geliştirici tarafından günlükleri özel bir biçimde tüketmek ve yayımlamak için kullanılabilecek bir geri çağırma örneği. Günlükleri yeniden yönlendirmek istediğiniz yönteme bağlı olarak localCallback yöntemini uygulayın.

- *düzey* (isteğe bağlı): yapılandırılabilir günlük düzeyi. Desteklenen günlük düzeyleri şunlardır: Hata, uyarı, bilgi, ayrıntılı. Varsayılan değer Info ' dır.

- *ılookupggingenabled* (isteğe bağlı): true olarak ayarlandıysa kişisel ve kurumsal verileri günlüğe almanıza olanak sağlar. Bu, varsayılan olarak false olarak ayarlanır, böylece uygulamanız kişisel verileri günlüğe eklemez. Kişisel veri günlükleri hiçbir şekilde konsol, Logcat veya NSLog gibi varsayılan çıkışlara yazılmaz. Varsayılan değer false olarak ayarlanmıştır.

- *CorrelationId* (isteğe bağlı): isteği hata ayıklama amacıyla Yanıtla eşlemek için kullanılan benzersiz bir tanımlayıcı. Varsayılan olarak RFC4122 sürüm 4 GUID (128 bit) olur.

```javascript

function loggerCallback(logLevel, message, containsPii) {
   console.log(message);
}

var msalConfig = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”,
    },
     system: {
             logger: new Msal.Logger(
                                loggerCallback ,{
                                     level: Msal.LogLevel.Verbose,
                                     piiLoggingEnabled: false,
                                     correlationId: '1234'
                                }
                        )
     }
}

var UserAgentApplication = new Msal.UserAgentApplication(msalConfig);
```
