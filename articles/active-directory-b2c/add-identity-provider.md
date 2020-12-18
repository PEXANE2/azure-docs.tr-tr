---
title: Kimlik sağlayıcısı ekleme-Azure Active Directory B2C | Microsoft Docs
description: Active Directory B2C kiracınıza bir kimlik sağlayıcısı eklemeyi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 12/07/2020
ms.custom: mvc
ms.topic: how-to
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: a5e8914b170f972304098bf9b4d0022164d30777
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97669100"
---
# <a name="add-an-identity-provider-to-your-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C kiracınıza bir kimlik sağlayıcısı ekleyin

Azure AD B2C, kullanıcıların uygulamanızda dış sosyal veya kurumsal kimlik sağlayıcılarından (IDP) kimlik bilgileriyle oturum açmalarına izin verecek şekilde yapılandırabilirsiniz. Azure AD B2C, Facebook, Microsoft hesabı, Google, Twitter ve OAuth 1,0, OAuth 2,0, OpenID Connect ve SAML protokollerini destekleyen herhangi bir kimlik sağlayıcısı gibi harici kimlik sağlayıcılarını destekler.

Dış kimlik sağlayıcısı Federasyonu sayesinde, tüketicilere yalnızca uygulamanız için yeni bir hesap oluşturmak zorunda kalmadan mevcut sosyal veya kurumsal hesaplarıyla oturum açma olanağı sunabilirsiniz.

Kaydolma veya oturum açma sayfasında, kullanıcının oturum açma için seçecan dış kimlik sağlayıcılarının bir listesini Azure AD B2C gösterir. Dış kimlik sağlayıcılarından birini seçtikten sonra, oturum açma işlemini tamamlaması için seçili sağlayıcının web sitesine alınır (yeniden yönlendirilir). Kullanıcı başarıyla oturum açtıktan sonra, uygulamanızdaki hesabın kimlik doğrulaması için Azure AD B2C olarak döndürülür.

![Sosyal hesap (Facebook) ile mobil oturum açma örneği](media/add-identity-provider/external-idp.png)

Azure portal kullanarak [Kullanıcı akışlarınıza](user-flow-overview.md) Azure Active Directory B2C (Azure AD B2C) tarafından desteklenen kimlik sağlayıcıları ekleyebilirsiniz. Ayrıca, [özel ilkelerinize](custom-policy-get-started.md)kimlik sağlayıcıları da ekleyebilirsiniz.

## <a name="select-an-identity-provider"></a>Bir kimlik sağlayıcısı seçin

Genellikle uygulamalarınızda yalnızca bir kimlik sağlayıcısı kullanırsınız, ancak daha fazla ekleme seçeneğiniz vardır. Aşağıdaki nasıl yapılır makalelerinde kimlik sağlayıcısı uygulamasını nasıl oluşturacağınız, kiracınıza kimlik sağlayıcısını nasıl ekleyeceğiniz ve kimlik sağlayıcısını Kullanıcı akışınıza veya özel ilkenize nasıl ekleyeceğiniz gösterilmektedir.

* [AD FS](identity-provider-adfs.md)
* [Amazon](identity-provider-amazon.md)
* [Azure AD (Tek kiracılı)](identity-provider-azure-ad-single-tenant.md)
* [Azure AD (Çok kiracılı)](identity-provider-azure-ad-multi-tenant.md)
* [Facebook](identity-provider-facebook.md)
* [Genel kimlik sağlayıcısı](identity-provider-generic-openid-connect.md)
* [GitHub](identity-provider-github.md)
* [ID.me](identity-provider-id-me.md)
* [Google](identity-provider-google.md)
* [LinkedIn](identity-provider-linkedin.md)
* [Microsoft Hesabı](identity-provider-microsoft-account.md)
* [QQ](identity-provider-qq.md)
* [Salesforce](identity-provider-salesforce.md)
* [Twitter](identity-provider-twitter.md)
* [WeChat](identity-provider-wechat.md)
* [Weibo](identity-provider-weibo.md)
