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
ms.openlocfilehash: 5b080ce3869bcdbbde9724a33433e5f0659b37e4
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703839"
---
# <a name="quickstart-build-a-net-web-api-that-integrates-with-azure-ad-for-authentication-and-authorization"></a>Hızlı başlangıç: Kimlik doğrulaması ve yetkilendirme için Azure AD ile tümleştirilen bir .NET web API'si oluşturma

[Microsoft Identity platform](v2-overview.md) , Azure Active Directory (Azure AD) geliştirici platformunun bir evmidir. Bu platform geliştiricilerin tüm Microsoft kimlikleriyle oturum açan ve Microsoft Graph veya diğer Microsoft API'leri ya da geliştiricilerin derlemiş olduğu API'lere çağrı göndermek için gerekli belirteçleri alan uygulamalar derlemesini sağlar.

[Microsoft kimlik doğrulama kitaplığı (msal)](msal-overview.md) , geliştiricilerin güvenli Web API 'lerine erişmek için Microsoft Identity platform uç noktasından belirteç almasına olanak sağlar. Active Directory Authentication Library (ADAL), MSAL Microsoft Identity platform (v 2.0) uç noktasıyla tümleştirilebilen geliştiriciler için Azure AD (v 1.0) uç noktası ile tümleşir.

Yeni Web API 'Leri için Microsoft Identity platform (v 2.0) ve MSAL kullanarak belirteçleri edinmeniz ve güvenli Web API 'Lerine erişmeniz önerilir: [hızlı başlangıç: Microsoft 'a bir ASP.NET Web uygulamasına oturum açma ekleme](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2#calling-an-aspnet-core-web-api-from-a-wpf-application-using-azure-ad-v2)
