---
title: Geliştiriciler için Azure Etkin Dizini (v1.0) genel bakış
description: Bu makalede, Azure Active Directory v1.0 bitiş noktası ve platform'u kullanarak Microsoft iş ve okul hesaplarında oturum açma genel bir bakış sağlar.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: conceptual
ms.workload: identity
ms.date: 10/24/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: ade350c91ebd2f3a68b52011e598f739a14c220f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154500"
---
# <a name="azure-active-directory-for-developers-v10-overview"></a>Geliştiriciler için Azure Etkin Dizini (v1.0) genel bakış

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Active Directory (Azure AD), geliştiricilerin Microsoft iş veya okul hesabına sahip kullanıcıların güvenli şekilde oturumunu açan uygulamalar derlemesine olanak sağlayan bir bulut kimlik hizmetidir. Azure AD, tek kiracılı, iş kolu (LOB) uygulamalarını derleyen geliştiricileri ve çok kiracılı uygulamalar geliştirmek isteyen geliştiricileri destekler. Temel oturum açmaya ek olarak Azure AD, uygulamaların hem [Microsoft Graph](https://docs.microsoft.com/graph/overview) gibi Microsoft API’lerini hem de Azure AD platformunda derlenen özel API’leri çağırmasını sağlar. Bu belgeler, OAuth2.0 ve OpenID Connect gibi sektör standardı protokolleri kullanarak Azure AD desteğini uygulamanıza nasıl ekleyeceğinizi göstermektedir.

> [!NOTE]
> Bu sayfadaki içeriğin çoğu, yalnızca Microsoft iş veya okul hesaplarını destekleyen v1.0 bitiş noktası ve platformuna odaklanır. Tüketici veya kişisel Microsoft hesaplarını oturum etmek istiyorsanız, [v2.0 bitiş noktası ve platformundaki](../develop/v2-overview.md)bilgilere bakın. v2.0 bitiş noktası, tüm Microsoft kimliklerinde oturum imzalamak isteyen uygulamalar için birleşik bir geliştirici deneyimi sunar.

| | |
| --- | --- |
|[Kimlik doğrulaması temel bilgileri](v1-authentication-scenarios.md) | Azure AD ile kimlik doğrulamaya giriş. |
|[Uygulama türleri](app-types.md) | Azure AD tarafından desteklenen kimlik doğrulama senaryolarına genel bakış. |
| | |

## <a name="get-started"></a>Kullanmaya başlayın

V1.0 hızlı başlangıçlar ve öğreticiler, Azure AD Kimlik Doğrulama Kitaplığı (ADAL) SDK'yı kullanarak tercih ettiğiniz platformda bir uygulama oluşturmanızda size yol sunar. Başlamak için [Microsoft kimlik platformundaki (geliştiriciler için Azure Active Directory)](index.yml) **v1.0 Quickstarts** ve **v1.0 Öğreticilerine** bakın.

## <a name="how-to-guides"></a>Nasıl yapılır kılavuzları

Azure AD'deki en yaygın görevlerin ayrıntılı bilgi ve izthroughleri için **v1.0 Nasıl Yapılulu kılavuzlarına** bakın.

## <a name="reference-topics"></a>Başvuru konuları

Aşağıdaki makaleler, Azure AD’de kullanılan API'ler, protokol iletileri ve terimler hakkında ayrıntılı bilgi sağlar.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Kimlik Doğrulama Kitaplıkları (ADAL)](active-directory-authentication-libraries.md)   | Azure AD tarafından sağlanan kitaplıklara ve SDK’lara genel bakış. |
| [Kod örnekleri](sample-v1-code.md)                                  | Tüm Azure AD kod örneklerinin listesi. |
| [Sözlük](../develop/developer-glossary.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)                                      | Bu belgelerde kullanılan terminoloji ve sözcük tanımları. |
|  |  |

## <a name="videos"></a>Videolar

Yeni Microsoft kimlik platformuna geçiş edersiniz yardımcı olmak için [Azure Active Directory geliştirici platformu videolarına](videos.md) bakın.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
