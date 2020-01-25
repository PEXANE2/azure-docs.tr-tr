---
title: Azure AD v2 Windows Masaüstü başlangıç-yapılandırma | Microsoft Docs
description: Bir Windows Masaüstü .NET (XAML) uygulaması, bir erişim belirteci alabilir ve Azure Active Directory v2 uç noktası tarafından korunan bir API 'YI çağırabilir.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: ryanwi
ms.custom: aaddev
ms.openlocfilehash: e84d99d18d999a7b629f65f9d18355321f04e538
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76712507"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanıza uygulamanın kayıt bilgilerini ekleyin
Bu adımda, uygulama kimliğini projenize eklemeniz gerekir.

1.  `App.xaml.cs` açın ve `ClientId` içeren satırı ile değiştirin:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Sonraki

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
