---
title: MSAL uygulamalarında oturum açma | Microsoft Identity platformu
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
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4dad8a276cd40b1ff04bbced833b5d70cec4fc87
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268595"
---
# <a name="logging-in-msal-applications"></a>MSAL uygulamalarında oturum açma

Microsoft kimlik doğrulama kitaplığı (MSAL) uygulamaları, sorunların tanılanmasına yardımcı olabilecek günlük iletileri oluşturur. Bir uygulama, birkaç satırlık kodla günlüğe kaydetmeyi yapılandırabilir ve ayrıntı düzeyi üzerinde özel denetime sahip olabilir ve kişisel ve kurumsal verilerin günlüğe kaydedilip kaydedilmeyeceğini belirtir. Bir MSAL günlüğü geri araması oluşturmanızı ve kullanıcıların, kimlik doğrulama sorunları olduğunda günlükleri göndermesi için bir yol sağlamanızı öneririz.

## <a name="logging-levels"></a>Günlüğe kaydetme düzeyleri

MSAL çeşitli düzeylerde günlük ayrıntısı sağlar:

- Hata: Bir şeyin yanlış geçmiş olduğunu ve bir hatanın oluşturulduğunu gösterir. Hata ayıklama ve sorunları tanımlama için kullanın.
- Uyarı: Bir hata veya hata olması gerekmez, ancak tanılama ve işaret eden sorun sorunları için tasarlanmıştır.
- Bilgisine MSAL, bilgilendirici amaçlar için tasarlanan olayları günlüğe kaydetmek için gerekli değildir.
- seçeneini Varsayılan. MSAL, kitaplık davranışının tüm ayrıntılarını günlüğe kaydeder.

## <a name="personal-and-organizational-data"></a>Kişisel ve Kurumsal veriler

Varsayılan olarak, MSAL günlükçüsü son derece hassas kişisel veya kurumsal verileri yakalamaz. Kitaplığı, bunu yapmayı seçerseniz kişisel ve kurumsal verileri günlüğe kaydetmeyi etkinleştirme seçeneğini sağlar.

## <a name="logging-in-msalnet"></a>MSAL.NET içinde oturum açma

 > [!NOTE]
 > MSAL.NET hakkında daha fazla bilgi için [msal.net wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)'yi inceleyin. MSAL.NET günlük ve daha fazlasını örnekler alın.
 
MSAL 3. x içinde, günlük kaydı `.WithLogging` Oluşturucu değiştiricisini kullanarak uygulama oluşturma sırasında uygulama başına ayarlanır. Bu yöntem isteğe bağlı parametreleri alır:

- *Düzey* , hangi günlük kayıt düzeyini istediğinize karar vermenize olanak sağlar. Hatalara ayarlandığında yalnızca hatalar alınır
- *Ilookupggingenabled* , true olarak ayarlandıysa kişisel ve kurumsal verileri günlüğe almanıza olanak sağlar. Bu, varsayılan olarak false değerine ayarlanır, böylece uygulamanız kişisel verileri günlüğe kaydetmez.
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

- `localCallback`: geliştirici tarafından günlükleri özel bir biçimde tüketmek ve yayımlamak için kullanılabilecek bir geri çağırma örneği. Günlükleri yeniden yönlendirmek istediğiniz yönteme bağlı olarak localCallback yöntemini uygulayın.

- `level`(isteğe bağlı): yapılandırılabilir günlük düzeyi. Desteklenen günlük düzeyleri şunlardır: Hata, uyarı, bilgi, ayrıntılı. Varsayılan değer Info ' dır.

- `piiLoggingEnabled`(isteğe bağlı): true olarak ayarlandıysa kişisel ve kurumsal verileri günlüğe almanıza olanak sağlar. Varsayılan olarak, uygulamanızın kişisel verileri günlüğe almamasını sağlamak için bu false olarak ayarlanır. Kişisel veri günlükleri hiçbir şekilde konsol, Logcat veya NSLog gibi varsayılan çıkışlara yazılmaz. Varsayılan değer false olarak ayarlanmıştır.

- `correlationId`(isteğe bağlı): isteği hata ayıklama amacıyla Yanıtla eşlemek için kullanılan benzersiz bir tanımlayıcı. Varsayılan olarak RFC4122 sürüm 4 GUID (128 bit) olur.

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

## <a name="logging-in-msal-for-ios-and-macos"></a>İOS ve macOS için MSAL 'da oturum açma

MSAL günlüğünü yakalamak ve kendi uygulamanızın günlüğüne eklemek için bir geri çağırma ayarlayın. Geri arama imzası şöyle görünür:

```objc
/*!
    The LogCallback block for the MSAL logger
 
    @param  level           The level of the log message
    @param  message         The message being logged
    @param  containsPII     If the message might contain Personally Identifiable Information (PII)
                            this will be true. Log messages possibly containing PII will not be
                            sent to the callback unless PIllLoggingEnabled is set to YES on the
                            logger.

 */
typedef void (^MSALLogCallback)(MSALLogLevel level, NSString *message, BOOL containsPII);
```

Örneğin:

Objective-C
```objc
[MSALGlobalConfig.loggerConfig setLogCallback:^(MSALLogLevel level, NSString *message, BOOL containsPII)
    {
        if (!containsPII)
        {
#if DEBUG
            // IMPORTANT: MSAL logs may contain sensitive information. Never output MSAL logs with NSLog, or print, directly unless you're running your application in debug mode. If you're writing MSAL logs to file, you must store the file securely.
            NSLog(@"MSAL log: %@", message);
#endif
        }
    }];
```

Swift
```swift
MSALGlobalConfig.loggerConfig.setLogCallback { (level, message, containsPII) in
    if let message = message, !containsPII
    {
#if DEBUG
    // IMPORTANT: MSAL logs may contain sensitive information. Never output MSAL logs with NSLog, or print, directly unless you're running your application in debug mode. If you're writing MSAL logs to file, you must store the file securely.
    print("MSAL log: \(message)")
#endif
    }
}
```

### <a name="personal-identifiable-information-pii"></a>Kişisel olarak tanımlanabilen bilgiler (PII)

Varsayılan olarak, MSAL hiçbir PII yakalamaz veya günlüğe kaydetmez. Kitaplık, uygulama geliştiricilerinin Msalgünlükçü sınıfındaki bir özellik aracılığıyla bunu açmasına olanak sağlar. PII açılarak uygulama, yüksek düzeyde hassas verileri güvenli bir şekilde işlemek ve yasal gereksinimleri takip etmek için sorumluluk kazanır.

Objective-C
```objc
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = YES;

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = NO;
```

Swift
```swift
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = true

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = false
```

### <a name="logging-levels"></a>Günlüğe kaydetme düzeyleri

İOS ve macOS için MSAL kullanarak günlüğe kaydetme düzeyini ayarlamak için aşağıdaki değerlerden birini kullanın:

|Düzey  |Açıklama |
|---------|---------|
| `MSALLogLevelNothing`| Tüm günlüğe kaydetmeyi devre dışı bırak |
| `MSALLogLevelError` | Varsayılan düzey, yalnızca hata oluştuğunda bilgileri yazdırır |
| `MSALLogLevelWarning` | Uyarılar |
| `MSALLogLevelInfo` |  Kitaplık giriş noktaları, parametreler ve çeşitli Anahtarlık işlemleri |
|`MSALLogLevelVerbose`     |  API izleme       |

Örneğin:

Objective-C
```objc
MSALGlobalConfig.loggerConfig.logLevel = MSALLogLevelVerbose;
 ```
 
 Swift
```swift
MSALGlobalConfig.loggerConfig.logLevel = .verbose
 ```

### <a name="log-message-format"></a>Günlük iletisi biçimi

MSAL günlük iletilerinin ileti kısmı şu biçimdedir`TID = <thread_id> MSAL <sdk_ver> <OS> <OS_ver> [timestamp - correlation_id] message`

Örneğin:

`TID = 551563 MSAL 0.2.0 iOS Sim 12.0 [2018-09-24 00:36:38 - 36764181-EF53-4E4E-B3E5-16FE362CFC44] acquireToken returning with error: (MSALErrorDomain, -42400) User cancelled the authorization session.`

Bağıntı kimlikleri ve zaman damgaları sağlamak, sorunları izlemek için yararlıdır. Zaman damgası ve bağıntı KIMLIĞI bilgileri günlük iletisinde bulunabilir. Bu dosyaları almak için tek güvenilir yer MSAL Logging iletilerinden alınır.
