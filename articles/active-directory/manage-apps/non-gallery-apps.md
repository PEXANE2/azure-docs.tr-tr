---
title: Uygulama galerisinde listelenmeyen uygulamalar için Azure AD kullanma
description: Azure AD galerisinde listelenmeyen uygulamaların nasıl tümleştirileceğini anlayın.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 07/27/2020
ms.author: kenwith
ms.reviewer: arvinh,luleon
ms.openlocfilehash: 262c3a7c8a668ef25d092b5d523743e0ece89cc3
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88641152"
---
# <a name="using-azure-ad-for-applications-not-listed-in-the-app-gallery"></a>Uygulama galerisinde listelenmeyen uygulamalar için Azure AD kullanma

[Uygulama ekleme](add-application-portal.md) hızlı başlangıcı ' nda, Azure AD kiracınıza uygulama eklemeyi öğreneceksiniz.

[Azure AD uygulama galerisindeki](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/)seçeneklere ek olarak, **Galeri dışı bir uygulama**ekleme seçeneğiniz vardır. 

## <a name="capabilities-for-apps-not-listed-in-the-azure-ad-gallery"></a>Azure AD galerisinde listelenmeyen uygulamalar için yetenekler

Kuruluşunuzda zaten var olan herhangi bir uygulamayı veya Azure AD Galerisi 'nin parçası olmayan bir satıcıdan herhangi bir üçüncü taraf uygulamayı ekleyebilirsiniz. [Lisans sözleşmenize](https://azure.microsoft.com/pricing/details/active-directory/)bağlı olarak aşağıdaki yetenekler mevcuttur:

- [Security assertion Markup Language (SAML) 2,0](https://wikipedia.org/wiki/SAML_2.0) kimlik sağlayıcılarını destekleyen tüm uygulamaların self servis TÜMLEŞTIRMESI (SP tarafından başlatılan veya IDP-başlatıldı)
- [Parola tabanlı SSO](sso-options.md#password-based-sso) kullanarak HTML tabanlı bir oturum açma sayfasına sahip herhangi bir Web uygulamasının self servis tümleştirmesi
- [Kullanıcı sağlaması Için etki alanları arası kimlik yönetimi (SCıM) protokolü Için sistemi](../app-provisioning/use-scim-to-provision-users-and-groups.md) kullanan uygulamaların Self Servis bağlantısı
- [Office 365 uygulama başlatıcısı](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) veya [uygulamalarınızda](sso-options.md#linked-sign-on) herhangi bir uygulamaya bağlantı ekleme özelliği

Özel uygulamaları Azure AD ile tümleştirme hakkında Geliştirici Kılavuzu arıyorsanız bkz. [Azure AD Için kimlik doğrulama senaryoları](../develop/authentication-scenarios.md). Kullanıcıların kimliğini doğrulamak için [OpenID Connect/OAuth](../develop/active-directory-v2-protocols.md) gibi modern bir protokol kullanan bir uygulama geliştirirken, Azure Portal [uygulama kayıtları](../develop/quickstart-register-app.md) deneyimini kullanarak Microsoft Identity platformuna kaydedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama yönetiminde hızlı başlangıç serisi](view-applications-portal.md)

