---
title: Web API 'sini çağıran Web API 'Lerini üretime taşıma-Microsoft Identity platform | Mavisi
description: Web API 'Lerini üretime çağıran bir Web API 'sini nasıl taşıyacağınızı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d59a5b2a74c10e36103713725113cbe8c9cc412
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965178"
---
# <a name="web-api-that-calls-web-apis---move-to-production"></a>Web API 'Lerini çağıran Web API 'SI-üretime taşı

Web API 'Lerini çağırmak için bir belirteç aldıktan sonra, uygulamanızı üretime taşıyabilirsiniz.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>Daha fazla bilgi

Web API 'inizden Web API 'Lerinin nasıl çağrılacağını öğrenmiş olduğunuza göre, Web API 'Lerini çağıran korumalı bir Web API 'SI oluşturmak için kullanılan kodu açıklayan Bu öğreticiye sahip olabilirsiniz.

| Örnek | Platform | Açıklama |
|--------|----------|-------------|
| [Active-Directory-aspnetcore-WebApi-öğretici-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | ASP.NET Core 2,2 Web API, Masaüstü (WPF) | ASP.NET Core 2,2 Web API 'sini çağıran, kendisini Microsoft Identity platformu (v 2.0) kullanarak bir WPF uygulamasından çağrılan Microsoft Graph. |
