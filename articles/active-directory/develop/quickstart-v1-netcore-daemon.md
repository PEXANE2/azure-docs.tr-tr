---
title: Microsoft Identity platform .NET Core Daemon | Microsoft Docs
description: Azure AD ile tümleştirilen bir .NET Daemon uygulamasının nasıl oluşturulduğunu, OAuth 2,0 kullanarak Azure AD korumalı API 'Leri çağırır hakkında bilgi edinin.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: jmprieur
ms.reviewer: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f56f3f65919c4d1d9b59f7d0f67522a935579d64
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68327188"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-console-app-using-an-apps-identity"></a>Hızlı Başlangıç: Uygulamanın kimliğini kullanarak bir konsol uygulamasından belirteç alma ve Microsoft Graph API 'SI çağırma

[Microsoft Identity platform](v2-overview.md) , Azure Active Directory (Azure AD) geliştirici platformunun bir evmidir. Bu platform geliştiricilerin tüm Microsoft kimlikleriyle oturum açan ve Microsoft Graph veya diğer Microsoft API'leri ya da geliştiricilerin derlemiş olduğu API'lere çağrı göndermek için gerekli belirteçleri alan uygulamalar derlemesini sağlar.

[Microsoft kimlik doğrulama kitaplığı (msal)](msal-overview.md) , geliştiricilerin güvenli Web API 'lerine erişmek için Microsoft Identity platform uç noktasından belirteç almasına olanak sağlar. Active Directory Authentication Library (ADAL), MSAL Microsoft Identity platform (v 2.0) uç noktasıyla tümleştirilebilen geliştiriciler için Azure AD (v 1.0) uç noktası ile tümleşir.

Yeni .NET Daemon uygulamaları için Microsoft Identity platform (v 2.0) ve MSAL kullanarak belirteçleri edinmeniz ve güvenli Web API 'Lerine erişmeniz önerilir: [Hızlı Başlangıç: Bir belirteç alın ve uygulamanın kimliğini](quickstart-v2-netcore-daemon.md)kullanarak konsol uygulamasından Microsoft Graph API 'si çağırın.
