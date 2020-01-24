---
title: .NET Desktop 'tan (WPF) Microsoft Graph API 'Leri çağırma & kullanıcılar oturum açma | Mavisi
description: Oturum açmak için Azure AD ile tümleştirilen ve OAuth 2,0 kullanarak Azure AD korumalı API 'Leri çağıran bir .NET Windows masaüstü uygulaması oluşturmayı öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c35e44dff321c4a2f568855dacd827001a57f64a
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703822"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-a-net-desktop-wpf-app"></a>Hızlı başlangıç: kullanıcılarda oturum açın ve bir .NET Masaüstü (WPF) uygulamasından Microsoft Graph API 'sini çağırın

[Microsoft Identity platform](v2-overview.md) , Azure Active Directory (Azure AD) geliştirici platformunun bir evmidir. Bu platform geliştiricilerin tüm Microsoft kimlikleriyle oturum açan ve Microsoft Graph veya diğer Microsoft API'leri ya da geliştiricilerin derlemiş olduğu API'lere çağrı göndermek için gerekli belirteçleri alan uygulamalar derlemesini sağlar.

[Microsoft kimlik doğrulama kitaplığı (msal)](msal-overview.md) , geliştiricilerin güvenli Web API 'lerine erişmek için Microsoft Identity platform uç noktasından belirteç almasına olanak sağlar. Active Directory Authentication Library (ADAL), MSAL Microsoft Identity platform (v 2.0) uç noktasıyla tümleştirilebilen geliştiriciler için Azure AD (v 1.0) uç noktası ile tümleşir.

Yeni masaüstü uygulamalarında, belirteçleri edinmek ve güvenli Web API 'Lerine erişmek için Microsoft Identity platform (v 2.0) ve MSAL kullanmanızı öneririz: [hızlı başlangıç: bir belirteç alma ve Windows masaüstü uygulamasından MICROSOFT Graph API çağrısı](quickstart-v2-windows-desktop.md) .
