---
title: Azure AD v2 Windows Masaüstü başlangıç-yapılandırma
description: Bir Windows Masaüstü .NET (XAML) uygulaması, bir erişim belirteci alabilir ve Azure Active Directory v2 uç noktası tarafından korunan bir API 'YI çağırabilir.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/29/2020
ms.author: ryanwi
ms.custom: aaddev
ms.openlocfilehash: d82f9beecb1b558fca094c31f8c6718c990debd1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80885778"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanıza uygulamanın kayıt bilgilerini ekleyin
Bu adımda, uygulama kimliğini projenize eklemeniz gerekir.

1.  Öğesini açın `App.xaml.cs` ve ile içeren satırı değiştirin `ClientId` :

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Sonraki

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
