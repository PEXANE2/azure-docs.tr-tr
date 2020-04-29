---
title: Geliştiriciler için Azure Active Directory (v 1.0) genel bakış
description: Bu makalede, Azure Active Directory v 1.0 uç noktası ve platformu kullanılarak Microsoft iş ve okul hesaplarında oturum açmaya ilişkin bir genel bakış sunulmaktadır.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80154500"
---
# <a name="azure-active-directory-for-developers-v10-overview"></a>Geliştiriciler için Azure Active Directory (v 1.0) genel bakış

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Active Directory (Azure AD), geliştiricilerin Microsoft iş veya okul hesabına sahip kullanıcıların güvenli şekilde oturumunu açan uygulamalar derlemesine olanak sağlayan bir bulut kimlik hizmetidir. Azure AD, tek kiracılı, iş kolu (LOB) uygulamalarını derleyen geliştiricileri ve çok kiracılı uygulamalar geliştirmek isteyen geliştiricileri destekler. Temel oturum açmaya ek olarak Azure AD, uygulamaların hem [Microsoft Graph](https://docs.microsoft.com/graph/overview) gibi Microsoft API’lerini hem de Azure AD platformunda derlenen özel API’leri çağırmasını sağlar. Bu belgeler, OAuth2.0 ve OpenID Connect gibi sektör standardı protokolleri kullanarak Azure AD desteğini uygulamanıza nasıl ekleyeceğinizi göstermektedir.

> [!NOTE]
> Bu sayfadaki içeriğin çoğu, yalnızca Microsoft iş veya okul hesaplarını destekleyen v 1.0 uç noktası ve platformuna odaklanır. Tüketici veya kişisel Microsoft hesaplarında oturum açmak isterseniz, [v 2.0 uç noktası ve platformu](../develop/v2-overview.md)hakkında bilgi için bkz.. V 2.0 uç noktası, tüm Microsoft kimliklerinizde oturum açmak isteyen uygulamalar için birleştirilmiş bir geliştirici deneyimi sunar.

| | |
| --- | --- |
|[Kimlik doğrulaması temel bilgileri](v1-authentication-scenarios.md) | Azure AD ile kimlik doğrulamaya giriş. |
|[Uygulama türleri](app-types.md) | Azure AD tarafından desteklenen kimlik doğrulama senaryolarına genel bakış. |
| | |

## <a name="get-started"></a>başlarken

V 1.0 hızlı başlangıç ve öğreticiler, Azure AD kimlik doğrulama kitaplığı (ADAL) SDK 'sını kullanarak tercih ettiğiniz platformda uygulama oluşturma konusunda size yol gösterir. Başlamak için [Microsoft Identity platform (geliştiriciler için Azure Active Directory)](index.yml) adresindeki **v 1.0 hızlı başlangıç** ve **v 1.0 öğreticilerine** bakın.

## <a name="how-to-guides"></a>Nasıl yapılır kılavuzları

Azure AD 'de en yaygın görevlere yönelik ayrıntılı bilgi ve yönergeler için bkz. **v 1.0 nasıl yapılır kılavuzları** .

## <a name="reference-topics"></a>Başvuru konuları

Aşağıdaki makaleler, Azure AD’de kullanılan API'ler, protokol iletileri ve terimler hakkında ayrıntılı bilgi sağlar.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Kimlik Doğrulama Kitaplıkları (ADAL)](active-directory-authentication-libraries.md)   | Azure AD tarafından sağlanan kitaplıklara ve SDK’lara genel bakış. |
| [Kod örnekleri](sample-v1-code.md)                                  | Tüm Azure AD kod örneklerinin listesi. |
| [Sözlük](../develop/developer-glossary.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)                                      | Bu belgelerde kullanılan terminoloji ve sözcük tanımları. |
|  |  |

## <a name="videos"></a>Videolar

Yeni Microsoft Identity platformuna geçiş konusunda yardım için bkz. [Azure Active Directory geliştirici platformu videoları](videos.md) .

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
