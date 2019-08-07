---
title: Ulusal bulutlarda Azure Active Directory kullanarak kimlik doğrulaması
description: Ulusal bulutlar için uygulama kaydı ve kimlik doğrulama uç noktaları hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: negoe,CelesteDG
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: abaf8c910710000930f4e0ab6200d0ebd75b8d46
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68835201"
---
# <a name="national-clouds"></a>Ulusal bulutlar

Ulusal bulutlar, Azure 'un fiziksel olarak yalıtılmış örnekleridir. Azure 'un bu bölgeleri, verilerin yer aldığı, egemenlik ve uyumluluk gereksinimlerinin coğrafi sınırlar içinde kabul edilen şekilde sağlandığından emin olmak için tasarlanmıştır.

Genel bulut dahil, Azure Active Directory (Azure AD) aşağıdaki Ulusal bulutlara dağıtılır:  

- Azure Kamu
- Azure Almanya
- Azure Çin 21Vianet

Ulusal bulutlar benzersizdir ve Azure Global 'in ayrı bir ortamıdır. Uygulamanızı bu ortamlar için geliştirirken önemli farklılıkları bilmeniz önemlidir. Farklı uygulamalar kaydetme, belirteçleri alma ve uç noktaların yapılandırılması dahildir.

## <a name="app-registration-endpoints"></a>Uygulama kayıt uç noktaları

Ulusal bulutların her biri için ayrı bir Azure portal vardır. Uygulamaları ulusal bir bulutta Microsoft Identity platformu ile bütünleştirmek için, uygulamanızı ortama özgü her bir Azure portal ayrı olarak kaydetmeniz gerekir.

Aşağıdaki tabloda, her bir ulusal bulut için bir uygulamayı kaydetmek üzere kullanılan Azure AD uç noktaları için temel URL 'Ler listelenmektedir.

| Ulusal bulut | Azure AD Portal uç noktası |
|----------------|--------------------------|
| ABD kamu için Azure AD | `https://portal.azure.us` |
| Azure AD Almanya | `https://portal.microsoftazure.de` |
| 21Vianet tarafından çalıştırılan Azure AD Çin | `https://portal.azure.cn` |
| Azure AD (küresel hizmet) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Azure AD kimlik doğrulama uç noktaları

Tüm ulusal bulutlar, kullanıcıların kimliklerini her ortamda ayrı olarak doğrular ve ayrı kimlik doğrulama uç noktaları vardır.

Aşağıdaki tabloda, her bir ulusal bulut için belirteçleri elde etmek üzere kullanılan Azure AD uç noktaları için temel URL 'Ler listelenmektedir.

| Ulusal bulut | Azure AD kimlik doğrulama uç noktası |
|----------------|-------------------------|
| ABD kamu için Azure AD | `https://login.microsoftonline.us` |
| Azure AD Almanya| `https://login.microsoftonline.de` |
| 21Vianet tarafından çalıştırılan Azure AD Çin | `https://login.chinacloudapi.cn` |
| Azure AD (küresel hizmet)| `https://login.microsoftonline.com` |

Uygun bölgeye özgü temel URL 'YI kullanarak Azure AD yetkilendirmesi veya belirteç uç noktalarına istek oluşturabilirsiniz. Örneğin, Azure Almanya için:

  - Yetkilendirme ortak uç noktası `https://login.microsoftonline.de/common/oauth2/authorize`.
  - Belirteç ortak uç noktası `https://login.microsoftonline.de/common/oauth2/token`.

Tek kiracılı uygulamalarda, önceki URL 'Lerdeki "ortak" i kiracı KIMLIĞINIZLE veya adınızla değiştirin. `https://login.microsoftonline.de/contoso.com` bunun bir örneğidir.

> [!NOTE]
> [Azure AD v 2.0 yetkilendirme]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) ve belirteç uç noktaları yalnızca küresel hizmet için kullanılabilir. Ulusal bulut dağıtımları için desteklenmez.

## <a name="microsoft-graph-api"></a>Microsoft Graph API 'SI

Ulusal bir bulut ortamında Microsoft Graph API 'Lerini nasıl çağıracağınızı öğrenmek için, [Ulusal bulut dağıtımlarında Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/deployments)' a gidin.

> [!IMPORTANT]
> Küresel hizmetin belirli bölgelerinde bulunan belirli hizmetler ve özellikler, tüm ulusal bulutlarda kullanılamayabilir. Hangi hizmetlerin kullanılabilir olduğunu öğrenmek için [bölgeye göre kullanılabilir ürünlere](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast)gidin.

Microsoft Identity platformunu kullanarak bir uygulama oluşturmayı öğrenmek için, [Microsoft kimlik doğrulama kitaplığı (msal) öğreticisini](msal-national-cloud.md)izleyin. Özellikle, bu uygulama bir kullanıcıya oturum açacaktır ve Microsoft Graph API 'sini çağırmak için bir erişim belirteci alır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi:

- [Azure Devlet Kurumları](https://docs.microsoft.com/azure/azure-government/)
- [Azure Çin 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Almanya](https://docs.microsoft.com/azure/germany/)
- [Azure AD kimlik doğrulaması temelleri](authentication-scenarios.md)
