---
title: Azure AD v2 Windows Masaüstü Başlıyor - Config | Microsoft Dokümanlar
description: Bir Windows Desktop .NET (XAML) uygulaması nın erişim jetonunu nasıl elde edebileceği ve Azure Active Directory v2 bitiş noktası tarafından korunan bir API'yi nasıl arayabilirsiniz?
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76897690"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgilerini uygulamanız için ekleyin
Bu adımda, Projenize Uygulama Kimliği eklemeniz gerekir.

1.  Aşağıdakileri içeren satırı `App.xaml.cs` `ClientId` açın ve değiştirin:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Sonraki Nedir

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
