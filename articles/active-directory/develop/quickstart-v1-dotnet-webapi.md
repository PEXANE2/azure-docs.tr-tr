---
title: Kimlik doğrulama & yetkilendirmesi için bir Azure AD .NET Web API 'SI oluşturma | Microsoft Docs
description: Kimlik doğrulaması ve yetkilendirme için Azure AD ile tümleştirilen bir .NET MVC web API'si nasıl oluşturulur?
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: abc4c05aab6efcfc55d5e4dc812ab41a3e3fc8aa
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74963282"
---
# <a name="quickstart-build-a-net-web-api-that-integrates-with-azure-ad-for-authentication-and-authorization"></a>Hızlı başlangıç: Kimlik doğrulaması ve yetkilendirme için Azure AD ile tümleştirilen bir .NET web API'si oluşturma

[Microsoft Identity platform](v2-overview.md) , Azure Active Directory (Azure AD) geliştirici platformunun bir evmidir. Bu platform geliştiricilerin tüm Microsoft kimlikleriyle oturum açan ve Microsoft Graph veya diğer Microsoft API'leri ya da geliştiricilerin derlemiş olduğu API'lere çağrı göndermek için gerekli belirteçleri alan uygulamalar derlemesini sağlar.

[Microsoft kimlik doğrulama kitaplığı (msal)](msal-overview.md) , geliştiricilerin güvenli Web API 'lerine erişmek için Microsoft Identity platform uç noktasından belirteç almasına olanak sağlar. Active Directory Authentication Library (ADAL), MSAL Microsoft Identity platform (v 2.0) uç noktasıyla tümleştirilebilen geliştiriciler için Azure AD (v 1.0) uç noktası ile tümleşir.

Yeni Web API 'Leri için Microsoft Identity platform (v 2.0) ve MSAL kullanarak belirteçleri edinmeniz ve güvenli Web API 'Lerine erişmeniz önerilir: [hızlı başlangıç: Microsoft 'a bir ASP.NET Web uygulamasına oturum açma ekleme](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2#calling-an-aspnet-core-web-api-from-a-wpf-application-using-azure-ad-v2)
