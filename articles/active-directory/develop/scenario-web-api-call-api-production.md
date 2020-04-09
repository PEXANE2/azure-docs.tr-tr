---
title: Web API'yi üretime çağıran web API'sini üretime taşıyın - Microsoft kimlik platformu | Azure
description: Web API'lerini üretime çağıran bir web API'sini nasıl taşıyacağım hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: d557c70c09824f3aead41a654dc92c4ec46e9238
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885064"
---
# <a name="a-web-api-that-calls-web-apis-move-to-production"></a>Web API'lerini çağıran bir web API'sı: Üretime geç

Web API'lerini aramak için bir belirteç edindikten sonra, uygulamanızı üretime taşıyabilirsiniz.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>Daha fazla bilgi edinin

Artık kendi web API'nizden web API'lerini aramanın temellerini bildiğinize göre, web API'lerini çağıran korumalı bir web API'sı oluşturmak için kullanılan kodu açıklayan aşağıdaki öğretici ilginizi çekebilir.

| Örnek | Platform | Açıklama |
|--------|----------|-------------|
| [aktif-dizin-aspnetcore-webapi-öğretici-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | ASP.NET Core 2.2 Web API, Masaüstü (WPF) | ASP.NET Core 2.2 Web API'si, WPF uygulamasından Microsoft kimlik platformu (v2.0) kullanarak çağırdığınız Microsoft Graph'ı çağırır. |
