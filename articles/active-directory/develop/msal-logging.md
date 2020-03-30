---
title: MSAL uygulamalarında oturum açma | Azure
titleSuffix: Microsoft identity platform
description: Microsoft Kimlik Doğrulama Kitaplığı (MSAL) uygulamalarında günlüğe kaydetme hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/11/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 58697cc535357710c6889f05060b5e04e129ae7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084880"
---
# <a name="logging-in-msal-applications"></a>MSAL uygulamalarında oturum açma

Microsoft Kimlik Doğrulama Kitaplığı (MSAL) uygulamaları, sorunları tanılamaya yardımcı olabilecek günlük iletileri oluşturur. Bir uygulama, birkaç kod satırıyla günlüğe kaydetmeyi yapılandırabilir ve ayrıntı düzeyi ve kişisel ve kuruluş verilerinin günlüğe kaydedilip günlüğe kaydedilmediği üzerinde özel denetime sahip olabilir. Bir MSAL günlük geri araması oluşturmanızı ve kullanıcıların kimlik doğrulama sorunları olduğunda günlük göndermeleri için bir yol sağlamanızı öneririz.

## <a name="logging-levels"></a>Günlük seviyeleri

MSAL, birkaç günlük ayrıntıdüzeyi sağlar:

- Hata: Bir şeylerin yanlış gittiğini ve bir hata oluşturulduğunu gösterir. Hata ayıklama ve sorunları tanımlamak için kullanın.
- Uyarı: Mutlaka bir hata veya hata olmadı, ancak tanılama ve sorunları saptamak için tasarlanmıştır.
- Bilgi: MSAL, hata ayıklama için amaçlanmayan bilgilendirme amaçlı olayları günlüğe kaydeder.
- Verbose: Varsayılan. MSAL kitaplık davranışının tüm ayrıntılarını kaydeder.

## <a name="personal-and-organizational-data"></a>Kişisel ve organizasyonel veriler

Varsayılan olarak, MSAL logger herhangi bir son derece hassas kişisel veya kuruluş verileri yakalamak değildir. Kitaplık, bunu yapmaya karar verirseniz kişisel ve kuruluş verilerini günlüğe kaydetme seçeneği sunar.

Belirli bir dilde MSAL günlüğe kaydetme hakkında daha fazla bilgi için, dilinizile eşleşen sekmeyi seçin:

## <a name="net"></a>[.NET](#tab/dotnet)

## <a name="logging-in-msalnet"></a>MSAL.NET giriş

 > [!NOTE]
 > MSAL.NET günlük ve daha fazlası örnekleri için [MSAL.NET wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) bakın.

MSAL 3.x'te, oturum açma, `.WithLogging` uygulama oluşturma da oluşturucu değiştirici kullanılarak ayarlanır. Bu yöntem isteğe bağlı parametreleri alır:

- `Level`istediğiniz günlüğe kaydetme düzeyine karar vermenizi sağlar. Hatalara ayarlanması yalnızca hata alır
- `PiiLoggingEnabled`doğru ayarlanmışsa kişisel ve kuruluş verilerini günlüğe kaydetmenizi sağlar. Varsayılan olarak bu, uygulamanızın kişisel verileri günlüğe kaydetmemesi için yanlış olarak ayarlanır.
- `LogCallback`günlüğe kaydetmeyi yapan bir temsilciye ayarlanır. Doğruysa, `PiiLoggingEnabled` bu yöntem iletileri iki kez alır: `containsPii` bir kez parametre ile yanlış ve kişisel veri `containsPii` olmadan ileti eşittir ve parametre ile ikinci kez doğru eşittir ve ileti kişisel veri içerebilir. Bazı durumlarda (ileti kişisel veri içermiyorsa), ileti aynı olacaktır.
- `DefaultLoggingEnabled`platform için varsayılan günlüğe kaydetmeyi sağlar. Varsayılan olarak yanlış. Masaüstü/UWP uygulamalarında Olay İzleme, iOS'ta NSLog ve Android'de logcat kullanıyor.

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

## <a name="android"></a>[Android](#tab/android)

## <a name="logging-in-msal-for-android-using-java"></a>Java kullanarak Android için MSAL'da oturum açma

Günlük geri araması oluşturarak uygulama oluşturmada oturum açmayı açın. Geri arama şu parametreleri alır:

- `tag`kitaplık tarafından geri arama geçirilen bir dizedir. Günlük girişiyle ilişkilidir ve günlük iletilerini sıralamak için kullanılabilir.
- `logLevel`istediğiniz günlüğe kaydetme düzeyine karar vermenizi sağlar. Desteklenen günlük düzeyleri `Error`şunlardır: , `Warning`, `Info`, ve `Verbose`.
- `message`giriş girişinin içeriğidir.
- `containsPII`kişisel veri içeren iletilerin veya kuruluş verilerinin günlüğe kaydedilip günlüğe kaydedilmeyeceğini belirtir. Varsayılan olarak, uygulamanızın kişisel verileri günlüğe kaydetmemesi için bu yanlış olarak ayarlanır. `containsPII` Ise, `true`bu yöntem iki kez iletileri alırsınız: bir kez `containsPII` parametre `false` ayarlanmış ve kişisel veri `message` olmadan, `containsPii` `true` ve parametre için ayarlanmış ve ileti kişisel veri içerebilir ikinci kez. Bazı durumlarda (ileti kişisel veri içermiyorsa), ileti aynı olacaktır.

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

Varsayılan olarak, MSAL kaydedici herhangi bir kişisel tanımlanabilir bilgi veya kuruluş tanımlayıcı bilgileri yakalamaz.
Kişisel tanımlanabilir bilgilerin veya kuruluş tarafından tanımlanabilir bilgilerin günlüğe kaydedilmesini etkinleştirmek için:

```java
Logger.getInstance().setEnablePII(true);
```

Kişisel verileri ve kuruluş verilerini günlüğe kaydetmeyi devre dışı kakmak için:

```java
Logger.getInstance().setEnablePII(false);
```

Varsayılan olarak logcat için günlük devre dışı bırakılır. Etkinleştirmek için:

```java
Logger.getInstance().setEnableLogcatLog(true);
```

## <a name="javascript"></a>[Javascript](#tab/javascript)

 `UserAgentApplication` Bir örnek oluşturmak için yapılandırma sırasında bir logger nesnesi geçerek MSAL.js (JavaScript) oturum açma etkinleştirin. Bu kaydedici nesne aşağıdaki özelliklere sahiptir:

- `localCallback`: geliştirici tarafından günlükleri özel bir şekilde tüketmek ve yayımlamak için sağlanabilecek bir Geri Arama örneği. Günlükleri nasıl yeniden yönlendirmek istediğinize bağlı olarak yerel Callback yöntemini uygulayın.
- `level`(isteğe bağlı): yapılandırılabilir günlük düzeyi. Desteklenen günlük düzeyleri `Error`şunlardır: , `Warning`, `Info`, ve `Verbose`. Varsayılan değer: `Info`.
- `piiLoggingEnabled`(isteğe bağlı): doğru olarak ayarlanmışsa, kişisel ve kuruluş verilerini kaydeder. Varsayılan olarak bu, uygulamanızın kişisel verileri günlüğe kaydetmemesi için yanlıştır. Kişisel veri günlükleri konsol, Logcat veya NSLog gibi varsayılan çıktılara asla yazılmaz.
- `correlationId`(isteğe bağlı): hata ayıklama amacıyla yanıtla isteği eşlemek için kullanılan benzersiz bir tanımlayıcı. Varsayılanr rFC4122 sürüm 4 guid (128 bit).

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

## <a name="objective-c"></a>[Objective-C](#tab/objc)

## <a name="msal-for-ios-and-macos-logging-objc"></a>iOS ve macOS oturum açma-ObjC için MSAL

MSAL günlüğe kaydetmeyi yakalamak için bir geri arama ayarlayın ve bunu kendi uygulamanızın günlüğe kaydetmesine dahil edin. Geri arama nın imzası aşağıdaki gibi görünür:

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

Örnek:

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

### <a name="personal-data"></a>Kişisel veriler

Varsayılan olarak, MSAL herhangi bir kişisel veri (PII) yakalamaz veya günlüğe kaydetmez. Kitaplık, uygulama geliştiricilerin bunu MSALLogger sınıfındaki bir özellik aracılığıyla açmalarına olanak tanır. Uygulama `pii.Enabled`açarak, son derece hassas verileri güvenli bir şekilde işlemek ve yasal gereklilikleri takip etmek için sorumluluk alır.

```objc
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = YES;

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = NO;
```

### <a name="logging-levels"></a>Günlük seviyeleri

iOS ve macOS için MSAL'ı kullanarak oturum açtığınızda günlüğe kaydetme düzeyini ayarlamak için aşağıdaki değerlerden birini kullanın:

|Düzey  |Açıklama |
|---------|---------|
| `MSALLogLevelNothing`| Tüm günlüğe kaydetmeyi devre dışı |
| `MSALLogLevelError` | Varsayılan düzey, yalnızca hatalar oluştuğunda bilgileri yazdırır |
| `MSALLogLevelWarning` | Uyarılar |
| `MSALLogLevelInfo` |  Kütüphane giriş noktaları, parametreler ve çeşitli anahtarlık işlemleri ile |
|`MSALLogLevelVerbose`     |  API izleme |

Örnek:

```objc
MSALGlobalConfig.loggerConfig.logLevel = MSALLogLevelVerbose;
 ```

 ### <a name="log-message-format"></a>Günlük ileti biçimi

MSAL günlük iletilerinin ileti bölümü`TID = <thread_id> MSAL <sdk_ver> <OS> <OS_ver> [timestamp - correlation_id] message`

Örnek:

`TID = 551563 MSAL 0.2.0 iOS Sim 12.0 [2018-09-24 00:36:38 - 36764181-EF53-4E4E-B3E5-16FE362CFC44] acquireToken returning with error: (MSALErrorDomain, -42400) User cancelled the authorization session.`

Korelasyon tonları ve zaman damgaları sağlamak sorunları izlemek için yararlıdır. Zaman damgası ve korelasyon kimliği bilgileri günlük iletisinde kullanılabilir. Bunları almak için tek güvenilir yer MSAL günlük iletileri dir.

## <a name="swift"></a>[Swift](#tab/swift)

## <a name="msal-for-ios-and-macos-logging-swift"></a>iOS ve macOS oturum açma-Swift için MSAL

MSAL günlüğe kaydetmeyi yakalamak için bir geri arama ayarlayın ve bunu kendi uygulamanızın günlüğe kaydetmesine dahil edin. Geri arama için imza (Objective-C'de temsil edilir) aşağıdaki gibi görünür:

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

Örnek:

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

Varsayılan olarak, MSAL herhangi bir kişisel veri (PII) yakalamaz veya günlüğe kaydetmez. Kitaplık, uygulama geliştiricilerin bunu MSALLogger sınıfındaki bir özellik aracılığıyla açmalarına olanak tanır. Uygulama `pii.Enabled`açarak, son derece hassas verileri güvenli bir şekilde işlemek ve yasal gereklilikleri takip etmek için sorumluluk alır.

```swift
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = true

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = false
```

### <a name="logging-levels"></a>Günlük seviyeleri

iOS ve macOS için MSAL'ı kullanarak oturum açtığınızda günlüğe kaydetme düzeyini ayarlamak için aşağıdaki değerlerden birini kullanın:

|Düzey  |Açıklama |
|---------|---------|
| `MSALLogLevelNothing`| Tüm günlüğe kaydetmeyi devre dışı |
| `MSALLogLevelError` | Varsayılan düzey, yalnızca hatalar oluştuğunda bilgileri yazdırır |
| `MSALLogLevelWarning` | Uyarılar |
| `MSALLogLevelInfo` |  Kütüphane giriş noktaları, parametreler ve çeşitli anahtarlık işlemleri ile |
|`MSALLogLevelVerbose`     |  API izleme |

Örnek:

```swift
MSALGlobalConfig.loggerConfig.logLevel = .verbose
 ```

### <a name="log-message-format"></a>Günlük ileti biçimi

MSAL günlük iletilerinin ileti bölümü`TID = <thread_id> MSAL <sdk_ver> <OS> <OS_ver> [timestamp - correlation_id] message`

Örnek:

`TID = 551563 MSAL 0.2.0 iOS Sim 12.0 [2018-09-24 00:36:38 - 36764181-EF53-4E4E-B3E5-16FE362CFC44] acquireToken returning with error: (MSALErrorDomain, -42400) User cancelled the authorization session.`

Korelasyon tonları ve zaman damgaları sağlamak sorunları izlemek için yararlıdır. Zaman damgası ve korelasyon kimliği bilgileri günlük iletisinde kullanılabilir. Bunları almak için tek güvenilir yer MSAL günlük iletileri dir.

## <a name="java"></a>[Java](#tab/java)

## <a name="msal-for-java-logging"></a>Java günlük için MSAL

Java için MSAL, SLF4J ile uyumlu olduğu sürece uygulamanızla zaten kullanmakta olduğunuz günlük kitaplığını kullanmanıza olanak tanır. Java için MSAL java [için Basit Günlük Facade](http://www.slf4j.org/) kullanır (SLF4J) [java.util.log,](https://docs.oracle.com/javase/7/docs/api/java/util/logging/package-summary.html) [Logback](http://logback.qos.ch/) ve [Log4j](https://logging.apache.org/log4j/2.x/)gibi çeşitli günlük çerçeveleri için basit bir cephe veya soyutlama olarak . SLF4J, kullanıcının dağıtım sırasında istenen günlük çerçevesini takmasını sağlar.

Örneğin, uygulamanızda günlük çerçevesi olarak Giriş Yap'ı kullanmak için, uygulamanız için Maven pom dosyasına Giriş Yap bağımlılığını ekleyin:

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
```

Ardından Logback yapılandırma dosyasını ekleyin:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">

</configuration>
```

SLF4J dağıtım zamanında otomatik olarak Logback'e bağlanır. MSAL günlükleri konsola yazılacaktır.

Diğer günlük çerçevelerine nasıl bağlanacağına ilişkin talimatlar için [SLF4J kılavuzuna](http://www.slf4j.org/manual.html)bakın.

### <a name="personal-and-organization-information"></a>Kişisel ve organizasyon bilgileri

Varsayılan olarak, MSAL günlük herhangi bir kişisel veya kuruluş verilerini yakalamaz veya günlüğe kaydetmez. Aşağıdaki örnekte, kişisel veya kuruluş verilerini günlüğe kaydetme varsayılan olarak kapalıdır:

```java
    PublicClientApplication app2 = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
            .authority(AUTHORITY)
            .build();
```

İstemci uygulama oluşturucuyu `logPii()` ayarlayarak kişisel ve kurumsal veri günlüğe kaydetmeyi açın. Kişisel veya kuruluş el işlemlerini açarsanız, uygulamanız son derece hassas verileri güvenli bir şekilde işlemek ve tüm yasal gerekliliklere uymak için sorumluluk almalıdır.

Aşağıdaki örnekte, kişisel veya kuruluş verilerini günlüğe kaydetme etkinleştirilir:

```java
PublicClientApplication app2 = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
        .authority(AUTHORITY)
        .logPii(true)
        .build();
```

## <a name="python"></a>[Python](#tab/python)

## <a name="msal-for-python-logging"></a>Python günlüğü için MSAL

MSAL Python'da günlüğe kaydetme standart `logging.info("msg")` Python günlüğe kaydetme mekanizmasını kullanır, örneğin MSAL günlüğe kaydetmeyi aşağıdaki gibi yapılandırabilirsiniz (ve [username_password_sample](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.0.0/sample/username_password_sample.py#L31L32)iş başında görebilirsiniz):

### <a name="enable-debug-logging-for-all-modules"></a>Tüm modüller için hata ayıklama günlüğe kaydetmeyi etkinleştirme

Varsayılan olarak, herhangi bir Python komut dosyasında günlüğe kaydetme kapatılır. Tüm Python komut dosyanızdaki tüm modüller için hata ayıklama günlüğe kaydetmeyi etkinleştirmek istiyorsanız, aşağıdakileri kullanın:

```python
logging.basicConfig(level=logging.DEBUG)
```

### <a name="silence-only-msal-logging"></a>Yalnızca MSAL günlüğe kaydetme

Python komut dosyanızdaki diğer tüm modüllerde hata ayıklama günlüğe kaydetmeyi etkinleştirirken yalnızca MSAL kitaplığı günlüğe kaydetmeyi sessize almak için, MSAL Python tarafından kullanılan logger'ı kapatın:

```Python
logging.getLogger("msal").setLevel(logging.WARN)
```

### <a name="personal-and-organizational-data-in-python"></a>Python'da kişisel ve örgütsel veriler

Python için MSAL kişisel verileri veya kuruluş verilerini kaydetmez. Kişisel veya kuruluş verilerinin oturum açmasını veya kapatmasını açmak için hiçbir özellik yoktur.

İstediğiniz her şeyi günlüğe kaydetmek için standart Python günlüğünü kullanabilirsiniz, ancak hassas verileri güvenli bir şekilde işlemek ve yasal gereklilikleri takip etmek sizin sorumluluğunuzdadır.

Python'da günlüğe kaydetme hakkında daha fazla bilgi için lütfen Python'un [Günlük HOWTO'suna](https://docs.python.org/3/howto/logging.html#logging-basic-tutorial)bakın.

---
