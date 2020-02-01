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
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/29/2020
ms.author: ryanwi
ms.custom: aaddev
ms.openlocfilehash: 4c757b52e7378bffe4a4954022b131b772a337cf
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76897690"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanıza uygulamanın kayıt bilgilerini ekleyin
Bu adımda, uygulama kimliğini projenize eklemeniz gerekir.

1.  `App.xaml.cs` açın ve `ClientId` içeren satırı ile değiştirin:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Sonraki

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
