---
title: Azure AD v2 Windows Masaüstü Başlıyor - Config
description: Bir Windows Desktop .NET (XAML) uygulaması nın erişim jetonunu nasıl elde edebileceği ve Azure Active Directory v2 bitiş noktası tarafından korunan bir API'yi nasıl arayabilirsiniz?
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
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885778"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgilerini uygulamanız için ekleyin
Bu adımda, Projenize Uygulama Kimliği eklemeniz gerekir.

1.  Aşağıdakileri içeren satırı `App.xaml.cs` `ClientId` açın ve değiştirin:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Sonraki Nedir

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
