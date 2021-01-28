---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 75650d7ff0ac647aeb6dace76c270680b1b89347
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98954977"
---
## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Hata ayıklama ve kimlik doğrulama sırasında hata giderme senaryolarında, Microsoft kimlik doğrulama kitaplığı yerleşik günlük desteği sağlar. Günlüğe kaydetme her bir kitaplık aşağıdaki makalelerde ele alınmıştır:

:::row:::
    :::column:::
        - [MSAL.NET’te oturum açma](../articles/active-directory/develop/msal-logging-dotnet.md)
        - [Android için MSAL’de oturum açma](../articles/active-directory/develop/msal-logging-android.md)
        - [MSAL.js’de oturum açma](../articles/active-directory/develop/msal-logging-js.md)
    :::column-end:::
    :::column:::
        - [iOS/macOS için MSAL’de oturum açma](../articles/active-directory/develop/msal-logging-ios.md)
        - [Java için MSAL’de oturum açma](../articles/active-directory/develop/msal-logging-java.md)
        - [Python için MSAL’de oturum açma](../articles/active-directory/develop/msal-logging-python.md)
    :::column-end:::
:::row-end:::

Veri toplama için bazı öneriler aşağıda verilmiştir:

- Kullanıcılar sorun yaşadıklarında yardım isteyebilir. Günlükleri yakalamak ve geçici olarak depolamak en iyi uygulamadır. Kullanıcıların günlükleri karşıya yükleyebilecekleri bir konum belirtin. MSAL, kimlik doğrulamasıyla ilgili ayrıntılı bilgileri yakalamak için günlüğe kaydetme uzantıları sağlar.

- Telemetri varsa, kullanıcıların uygulamanızda oturum açma hakkında veri toplamak için MSAL aracılığıyla etkinleştirin.


## <a name="validate-your-integration"></a>Tümleştirmenizi doğrulama

[Microsoft Identity platform tümleştirme denetim listesini](../articles/active-directory/develop/identity-platform-integration-checklist.md)izleyerek tümleştirmenizi test edin.
