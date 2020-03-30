---
title: Azure AD kimlik doğrulaması & ulusal bulutlar | Azure
titleSuffix: Microsoft identity platform
description: Ulusal bulutlar için uygulama kaydı ve kimlik doğrulama bitiş noktaları hakkında bilgi edinin.
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: negoe
ms.reviewer: negoe,celested
ms.custom: aaddev
ms.openlocfilehash: 20a053369149dc29d6485c49bb091a75bb9fb591
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263003"
---
# <a name="national-clouds"></a>Ulusal bulutlar

Ulusal bulutlar, fiziksel olarak yalıtılmış Azure örnekleridir. Azure'un bu bölgeleri, veri ikamet, egemenlik ve uyumluluk gereksinimlerinin coğrafi sınırlar içinde yerine getirilmesini sağlamak için tasarlanmıştır.

Genel bulut da dahil olmak üzere, Azure Etkin Dizin (Azure AD) aşağıdaki ulusal bulutlarda dağıtılır:  

- Azure Kamu
- Azure Almanya
- Azure Çin 21Vianet

Ulusal bulutlar benzersizdir ve Azure global'den ayrı bir ortamdır. Bu ortamlar için uygulamanızı geliştirirken önemli farklılıkların farkında olmak önemlidir. Farklar, uygulamaları kaydetme, belirteçleri edinme ve uç noktaları yapılandırmayı içerir.

## <a name="app-registration-endpoints"></a>Uygulama kayıt bitiş noktaları

Ulusal bulutların her biri için ayrı bir Azure portalı vardır. Uygulamaları ulusal bir bulutta Microsoft kimlik platformuyla tümleştirmek için, uygulamanızı ortama özel her Azure portalına ayrı olarak kaydetmeniz gerekir.

Aşağıdaki tabloda, her ulusal bulut için bir uygulama kaydetmek için kullanılan Azure AD uç noktalarının temel URL'leri listelanmaktadır.

| Ulusal bulut | Azure AD portalı bitiş noktası |
|----------------|--------------------------|
| ABD Hükümeti için Azure REKLAM | `https://portal.azure.us` |
| Azure AD Almanya | `https://portal.microsoftazure.de` |
| Azure AD Çin 21Vianet tarafından işletilen | `https://portal.azure.cn` |
| Azure AD (genel hizmet) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Azure AD kimlik doğrulama bitiş noktaları

Tüm ulusal bulutlar her ortamda kullanıcıları ayrı ayrı doğrular ve ayrı kimlik doğrulama bitiş noktalarına sahiptir.

Aşağıdaki tabloda, her ulusal bulut için belirteçler elde etmek için kullanılan Azure AD uç noktalarının temel URL'leri listelanmaktadır.

| Ulusal bulut | Azure AD kimlik doğrulama bitiş noktası |
|----------------|-------------------------|
| ABD Hükümeti için Azure REKLAM | `https://login.microsoftonline.us` |
| Azure AD Almanya| `https://login.microsoftonline.de` |
| Azure AD Çin 21Vianet tarafından işletilen | `https://login.chinacloudapi.cn` |
| Azure AD (genel hizmet)| `https://login.microsoftonline.com` |

Uygun bölgeye özgü temel URL'yi kullanarak Azure AD yetkilendirmesine veya belirteç uç noktalarına istekler oluşturabilirsiniz. Örneğin, Azure Almanya için:

  - Yetkilendirme ortak bitiş `https://login.microsoftonline.de/common/oauth2/authorize`noktasıdır.
  - Belirteç ortak `https://login.microsoftonline.de/common/oauth2/token`bitiş noktasıdır.

Tek kiracılı uygulamalar da önceki URL'lerde "ortak" olarak değiştirin ve kiracı kimliğiniz veya adınız ile değiştirin. `https://login.microsoftonline.de/contoso.com` bunun bir örneğidir.

## <a name="microsoft-graph-api"></a>Microsoft Graph API'si

Microsoft Graph API'lerini ulusal bulut ortamında nasıl arayacağınızı öğrenmek için, [ulusal bulut dağıtımlarında Microsoft Graph'a](https://developer.microsoft.com/graph/docs/concepts/deployments)gidin.

> [!IMPORTANT]
> Genel hizmetin belirli bölgelerinde bulunan belirli hizmetler ve özellikler tüm ulusal bulutlarda kullanılamayabilir. Hangi hizmetlerin mevcut olduğunu öğrenmek için [bölgeye göre kullanılabilen Ürünler'e](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast)gidin.

Microsoft kimlik platformunu kullanarak bir uygulamayı nasıl oluşturacağımı öğrenmek için [Microsoft Kimlik Doğrulama Kitaplığı (MSAL) öğreticisini](msal-national-cloud.md)izleyin. Özellikle, bu uygulama bir kullanıcı oturum açacak ve Microsoft Graph API aramak için bir erişim belirteci olsun.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Azure Kamu](https://docs.microsoft.com/azure/azure-government/)
- [Azure Çin 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Almanya](https://docs.microsoft.com/azure/germany/)
- [Azure AD kimlik doğrulama temelleri](authentication-scenarios.md)
