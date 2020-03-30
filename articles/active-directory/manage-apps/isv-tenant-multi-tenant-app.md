---
title: Çok kiracılı bir uygulama için Azure kiracısı oluşturma
description: Azure Active Directory ile tümleştirme konusunda bağımsız yazılım satıcıları için kılavuz
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 637adba89445e6974e83486f0641576225ccd268
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70812619"
---
# <a name="create-an-azure-tenant-for-a-multi-tenant-application"></a>Çok kiracılı bir uygulama için Azure kiracısı oluşturma  

Çok kiracılı uygulamanıza erişim sağlamak için, uygulamayı kaydetmek ve müşterinizin kimliklerinin federasyonuna olanak sağlamak için bir Azure Etkin Dizin kiracısı oluşturmanız gerekir. Bkz. [Çok kiracılı uygulamanız için doğru federasyon protokolünü seçme.](isv-choose-multi-tenant-federation.md) Bu kiracı, uygulamanızı ve federasyonu müşterilerinize benzeyen bir ortamda test etmenize olanak tanır.

## <a name="costs-of-hosting-a-multi-tenant-application"></a>Çok kiracılı bir uygulamayı barındırmanın maliyetleri

Azure Active Directory birden çok sürümde kullanılabilir. [Ayrıntılı özellik karşılaştırmasını görün.](https://azure.microsoft.com/pricing/details/active-directory/)

Azure aboneliğinizi ve Azure etkin dizininizi ücretsiz olarak oluşturabilir ve temel özellikleri kullanabilirsiniz.

## <a name="create-your-tenant"></a>Kiracınızı oluşturun

1. Kiracınızı oluşturun. Bkz. [Bir dev ortamı ayarlama](../develop/quickstart-create-new-tenant.md).

2. Uygulamanıza tek oturum açma erişimini etkinleştirin ve test edin,

   a. **OIDC veya Yemin başvuruları için,** başvurunuzu çok kiracılı bir uygulama olarak [kaydedin.](../develop/quickstart-register-app.md) Desteklenen Hesap türlerinde herhangi bir kuruluş dizininde ve kişisel Microsoft hesapları seçeneğinde Hesapları seçin

   b. **SAML ve WS-Fed tabanlı uygulamalar**için, Azure AD'de genel bir SAML şablonu kullanarak SAML tabanlı Tek oturum açma uygulamalarını [yapılandırırsınız.](configure-single-sign-on-non-gallery-applications.md)

Gerekirse [tek kiracılı bir uygulamayı birden çok kiracıya da dönüştürebilirsiniz.](../develop/howto-convert-app-to-be-multi-tenant.md)

## <a name="next-steps"></a>Sonraki Adımlar

[Başvurunuza SSO'yu entegre edin](isv-sso-content.md)
