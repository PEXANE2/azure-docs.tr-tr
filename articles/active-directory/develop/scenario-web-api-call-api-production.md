---
title: Web API'yi üretime çağıran web API'sini üretime taşıyın - Microsoft kimlik platformu | Azure
description: Web API'lerini üretime çağıran bir web API'sini nasıl taşıyacağım hakkında bilgi edinin.
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
ms.openlocfilehash: 474f771f007666179295f4502108acee88d1dc33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76701731"
---
# <a name="a-web-api-that-calls-web-apis-move-to-production"></a>Web API'lerini çağıran bir web API'sı: Üretime geç

Web API'lerini aramak için bir belirteç edindikten sonra, uygulamanızı üretime taşıyabilirsiniz.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>Daha fazla bilgi edinin

Artık kendi web API'nizden web API'lerini aramanın temellerini bildiğinize göre, web API'lerini çağıran korumalı bir web API'sı oluşturmak için kullanılan kodu açıklayan aşağıdaki öğretici ilginizi çekebilir.

| Örnek | Platform | Açıklama |
|--------|----------|-------------|
| [aktif-dizin-aspnetcore-webapi-öğretici-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | ASP.NET Core 2.2 Web API, Masaüstü (WPF) | ASP.NET Core 2.2 Web API'si, WPF uygulamasından Microsoft kimlik platformu (v2.0) kullanarak çağırdığınız Microsoft Graph'ı çağırır. |
