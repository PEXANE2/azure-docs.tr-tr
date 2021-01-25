---
title: MSAL.NET 'de hataları ve özel durumları günlüğe kaydetme
titleSuffix: Microsoft identity platform
description: MSAL.NET 'te hataların ve özel durumların nasıl günlüğe alınacağını öğrenin
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/25/2021
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 47576265c9b1a20f801231abe2fb37a929d5a27c
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98763584"
---
# <a name="logging-in-msalnet"></a>MSAL.NET’te oturum açma

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="configure-logging-in-msalnet"></a>MSAL.NET 'de günlüğe kaydetmeyi yapılandırma

MSAL 3. x içinde, günlük kaydı Oluşturucu değiştiricisini kullanarak uygulama oluşturma sırasında uygulama başına ayarlanır `.WithLogging` . Bu yöntem isteğe bağlı parametreleri alır:

- `Level` hangi günlük kayıt düzeyini istediğinize karar vermenize olanak sağlar. Hatalara ayarlandığında yalnızca hatalar alınır
- `PiiLoggingEnabled` true olarak ayarlanırsa kişisel ve kurumsal verileri günlüğe almanıza olanak sağlar. Bu, varsayılan olarak false değerine ayarlanır, böylece uygulamanız kişisel verileri günlüğe eklemez.
- `LogCallback` günlüğe kaydetmeyi yapan bir temsilciye ayarlanır. Eğer `PiiLoggingEnabled` true ise, bu yöntem iki kez iletileri alır: `containsPii` parametre false değerine eşitse ve kişisel veriler olmadan ileti ve bir ikinci kez `containsPii` parametresi true değerine eşitse ve ileti kişisel veriler içeriyorsa. Bazı durumlarda (ileti kişisel veriler içermiyorsa), ileti aynı olur.
- `DefaultLoggingEnabled` platformun varsayılan günlük kaydını etkinleştir. Varsayılan olarak false 'tur. Bunu true olarak ayarlarsanız, masaüstü/UWP uygulamalarında olay Izlemeyi kullanır, iOS üzerinde NSLog ve Android üzerinde Logcat.

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

> [!TIP]
 > MSAL.NET Logging örnekleri ve daha fazlası için bkz. [msal.net wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) .

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla kod örneği için [Microsoft Identity platform kodu örneklerine](sample-v2-code.md)bakın.