---
title: Microsoft kimlik doğrulama kitaplığı (MSAL) uygulamalarında oturum açma
titleSuffix: Microsoft identity platform
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
ms.date: 10/31/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9045fd6c1f5dcc4587b6ff85d567584f02421ba
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2019
ms.locfileid: "73902912"
---
# <a name="logging-in-msal-applications"></a>MSAL uygulamalarında oturum açma

Microsoft kimlik doğrulama kitaplığı (MSAL) uygulamaları, sorunların tanılanmasına yardımcı olabilecek günlük iletileri oluşturur. Bir uygulama, birkaç satırlık kodla günlüğe kaydetmeyi yapılandırabilir ve ayrıntı düzeyi üzerinde özel denetime sahip olabilir ve kişisel ve kurumsal verilerin günlüğe kaydedilip kaydedilmeyeceğini belirtir. Bir MSAL günlüğü geri araması oluşturmanızı ve kullanıcıların, kimlik doğrulama sorunları olduğunda günlükleri göndermesi için bir yol sağlamanızı öneririz.

## <a name="logging-levels"></a>Günlüğe kaydetme düzeyleri

MSAL çeşitli düzeylerde günlük ayrıntısı sağlar:

- Hata: bir şeyin yanlış geçmiş olduğunu ve bir hatanın oluşturulduğunu gösterir. Hata ayıklama ve sorunları tanımlama için kullanın.
- Uyarı: bir hata veya hata olması gerekmez, ancak tanılama ve işaret noktası sorunlarına yöneliktir.
- Bilgi: MSAL, hata ayıklama amaçlı olarak değil bilgilendirici amaçlar için tasarlanan olayları günlüğe kaydeder.
- Verbose: varsayılan. MSAL, kitaplık davranışının tüm ayrıntılarını günlüğe kaydeder.

## <a name="personal-and-organizational-data"></a>Kişisel ve Kurumsal veriler

Varsayılan olarak, MSAL günlükçüsü son derece hassas kişisel veya kurumsal verileri yakalamaz. Kitaplığı, bunu yapmayı seçerseniz kişisel ve kurumsal verileri günlüğe kaydetmeyi etkinleştirme seçeneğini sağlar.

## <a name="logging-in-msalnet"></a>MSAL.NET içinde oturum açma

 > [!NOTE]
 > MSAL.NET Logging örnekleri ve daha fazlası için bkz. [msal.net wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) .

MSAL 3. x içinde, günlük kaydı `.WithLogging` Oluşturucu değiştiricisi kullanılarak uygulama oluşturma sırasında ayarlanır. Bu yöntem isteğe bağlı parametreleri alır:

- `Level`, hangi günlük kayıt düzeyini istediğinize karar vermenize olanak sağlar. Hatalara ayarlandığında yalnızca hatalar alınır
- `PiiLoggingEnabled`, doğru olarak ayarlandıysa kişisel ve kurumsal verileri günlüğe almanıza olanak sağlar. Bu, varsayılan olarak false değerine ayarlanır, böylece uygulamanız kişisel verileri günlüğe eklemez.
- `LogCallback`, günlüğü yapan bir temsilciye ayarlanır. `PiiLoggingEnabled` true ise, bu yöntem iki kez iletileri alır: `containsPii` parametresine sahip bir kez false ve kişisel veriler olmadan ileti ve `containsPii` parametresine sahip ikinci bir kez doğru ve ileti kişisel veriler içeriyor olabilir. Bazı durumlarda (ileti kişisel veriler içermiyorsa), ileti aynı olur.
- `DefaultLoggingEnabled` platform için varsayılan günlüğe kaydetmeyi mümkün. Varsayılan olarak false 'tur. Bunu true olarak ayarlarsanız, masaüstü/UWP uygulamalarında olay Izlemeyi kullanır, iOS üzerinde NSLog ve Android üzerinde Logcat.

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

## <a name="logging-in-msal-for-android-using-java"></a>Java kullanarak Android için MSAL 'de oturum açma

Günlüğe kaydetme geri araması oluşturarak uygulama oluşturma sırasında oturum açmayı açın. Geri çağırma bu parametreleri alır:

- `tag`, kitaplık tarafından geri çağırmaya geçirilen bir dizedir. Günlük girdisiyle ilişkilendirilir ve günlük iletilerini sıralamak için kullanılabilir.
- `logLevel`, hangi günlük kayıt düzeyini istediğinize karar vermenize olanak sağlar. Desteklenen günlük düzeyleri şunlardır: `Error`, `Warning`, `Info`ve `Verbose`.
- `message` günlük girdisinin içeridir.
- `containsPII` kişisel verileri içeren iletilerin veya kurumsal verilerin günlüğe kaydedilip kaydedilmeyeceğini belirtir. Varsayılan olarak, uygulamanızın kişisel verileri günlüğe almamasını sağlamak için bu false olarak ayarlanır. `containsPII` `true`ise, bu yöntem iletileri iki kez alır: `containsPII` parametresi bir kez `false` ve kişisel veriler olmadan `message` ve `containsPii` parametresi `true` olarak ayarlanmış ve ileti kişisel verileri içerebilir. Bazı durumlarda (ileti kişisel veriler içermiyorsa), ileti aynı olur.

```java
private StringBuilder mLogs;

mLogs = new StringBuilder();
Logger.getInstance().setExternalLogger(new ILoggerCallback()
{
   @Override
   public void log(String tag, Logger.LogLevel logLevel, String message, boolean containsPII)
   {
      mLogs.append(message).append('\n');
   }
});
```

Varsayılan olarak, MSAL günlükçüsü herhangi bir kişisel bilgi veya kuruluş tarafından tanımlanabilen bilgileri yakalamaz.
Kişisel olarak tanımlanabilen bilgilerin veya kurumsal olarak tanımlanabilen bilgilerin günlüğe kaydedilmesini etkinleştirmek için:

```java
Logger.getInstance().setEnablePII(true);
```

Kişisel verileri ve kuruluş verilerini günlüğe kaydetmeyi devre dışı bırakmak için:

```java
Logger.getInstance().setEnablePII(false);
```

Günlüğe kaydetme varsayılan olarak Logcat devre dışıdır. Şunları etkinleştirmek için:

```java
Logger.getInstance().setEnableLogcatLog(true);
```

## <a name="logging-in-msaljs"></a>MSAL. js ' de günlüğe kaydetme

 Bir `UserAgentApplication` örneği oluşturmak için yapılandırma sırasında bir günlükçü nesnesi geçirerek MSAL. js ' de (JavaScript) günlük kaydını etkinleştirin. Bu günlükçü nesnesi aşağıdaki özelliklere sahiptir:

- `localCallback`: geliştirici tarafından günlükleri özel bir biçimde tüketmek ve yayımlamak için kullanılabilecek bir geri çağırma örneği. Günlükleri yeniden yönlendirmek istediğiniz yönteme bağlı olarak localCallback yöntemini uygulayın.
- `level` (isteğe bağlı): yapılandırılabilir günlük düzeyi. Desteklenen günlük düzeyleri şunlardır: `Error`, `Warning`, `Info`ve `Verbose`. Varsayılan değer: `Info`.
- `piiLoggingEnabled` (isteğe bağlı): true olarak ayarlanırsa, kişisel ve kurumsal verileri günlüğe kaydeder. Bu, varsayılan olarak, uygulamanızın kişisel verileri günlüğe almamasını sağlamak için false 'tur. Kişisel veri günlükleri hiçbir şekilde konsol, Logcat veya NSLog gibi varsayılan çıkışlara yazılmaz.
- `correlationId` (isteğe bağlı): isteği hata ayıklama amacıyla Yanıtla eşlemek için kullanılan benzersiz bir tanımlayıcı. Varsayılan olarak RFC4122 sürüm 4 GUID (128 bit) olur.

```javascript
function loggerCallback(logLevel, message, containsPii) {
   console.log(message);
}

var msalConfig = {
    auth: {
        clientId: "<Enter your client id>",
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

### <a name="personal-data"></a>Kişisel veriler

Varsayılan olarak, MSAL kişisel verileri (PII) yakalamaz veya günlüğe kaydetmez. Kitaplık, uygulama geliştiricilerinin Msalgünlükçü sınıfındaki bir özellik aracılığıyla bunu açmasına olanak sağlar. `pii.Enabled`etkinleştirerek uygulama, yüksek düzeyde hassas verileri güvenli bir şekilde işlemek ve yasal gereksinimleri takip etmek için sorumluluk kazanır.

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
| `MSALLogLevelWarning` | Uyarılarına |
| `MSALLogLevelInfo` |  Kitaplık giriş noktaları, parametreler ve çeşitli Anahtarlık işlemleri |
|`MSALLogLevelVerbose`     |  API izleme |

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

MSAL günlük iletilerinin ileti kısmı `TID = <thread_id> MSAL <sdk_ver> <OS> <OS_ver> [timestamp - correlation_id] message` biçimindedir

Örneğin:

`TID = 551563 MSAL 0.2.0 iOS Sim 12.0 [2018-09-24 00:36:38 - 36764181-EF53-4E4E-B3E5-16FE362CFC44] acquireToken returning with error: (MSALErrorDomain, -42400) User cancelled the authorization session.`

Bağıntı kimlikleri ve zaman damgaları sağlamak, sorunları izlemek için yararlıdır. Zaman damgası ve bağıntı KIMLIĞI bilgileri günlük iletisinde bulunabilir. Bu dosyaları almak için tek güvenilir yer MSAL Logging iletilerinden alınır.

## <a name="logging-in-msal-for-java"></a>Java için MSAL 'de oturum açma

Java için MSAL (MSAL4J), DOLAYıSıYLA SLF4J ile uyumlu olduğu sürece uygulamanızla zaten kullandığınız günlük kitaplığını kullanmanıza olanak sağlar. MSAL4j Java [. util. Logging](https://docs.oracle.com/javase/7/docs/api/java/util/logging/package-summary.html), [Logback](http://logback.qos.ch/) ve [Log4J](https://logging.apache.org/log4j/2.x/)gibi çeşitli günlük çerçeveleri için basit bir façlade veya soyutlama olarak Java (dolayısıyla slf4j) [için basit günlük kaydı](http://www.slf4j.org/) kullanır. DOLAYıSıYLA SLF4J, son kullanıcının dağıtım zamanında istenen günlük çerçevesini eklemesine izin verir.

Örneğin, uygulamanızdaki günlüğe kaydetme çerçevesi olarak Logback kullanmak için, uygulamanız için Maven Pod dosyasına Logback bağımlılığını ekleyin:

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
```

Sonra Logback yapılandırma dosyasını ekleyin:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">

</configuration>
```

DOLAYıSıYLA SLF4J, dağıtım zamanında otomatik olarak Logback 'a bağlanır. MSAL Günlükler konsola yazılacak.

Diğer günlük çerçevelerine nasıl bağlayacağınız hakkında yönergeler için bkz. [dolayısıyla slf4j Manual](http://www.slf4j.org/manual.html).

### <a name="personal-and-organization-information"></a>Kişisel ve kuruluş bilgileri

Varsayılan olarak, MSAL Logging kişisel veya kurumsal verileri yakalamaz veya günlüğe eklemez. Aşağıdaki örnekte, kişisel veya kurumsal verilerin günlüğe kaydedilmesi varsayılan olarak kapalıdır:

```java
    PublicClientApplication app2 = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
            .authority(AUTHORITY)
            .build();
```

İstemci uygulama tasarımcısında `logPii()` ayarlayarak kişisel ve kurumsal veri günlüğünü açın. Kişisel veya kurumsal veri günlüğü 'nü açarsanız, uygulamanızın yüksek oranda duyarlı verileri güvenli bir şekilde işlemek ve tüm düzenleme gereksinimleriyle uyumlu olması için sorumluluk olması gerekir.

Aşağıdaki örnekte, kişisel veya kurumsal verilerin günlüğe kaydedilmesi etkindir:

```java
PublicClientApplication app2 = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
        .authority(AUTHORITY)
        .logPii(true)
        .build();
```
