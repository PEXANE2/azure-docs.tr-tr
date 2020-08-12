---
title: Azure AD uygulama galerisine çok kiracılı uygulama ekleme
description: Özel geliştirmiş çok kiracılı uygulamanızı Azure AD uygulama galerisinde nasıl listelebileceğinizi açıklar.
services: active-directory
documentationCenter: na
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ryanwi
ms.reviewer: jeedes
ms.openlocfilehash: 37f305a7564382b9c6c5a031340d81a22cfedbf2
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88120720"
---
# <a name="add-a-multitenant-application-to-the-azure-ad-application-gallery"></a>Çok kiracılı bir uygulamayı Azure AD uygulama galerisine ekleme

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD Uygulama Galerisi nedir?

Azure Active Directory (Azure AD), bulut tabanlı bir kimlik hizmetidir. [Azure AD Uygulama Galerisi](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActiveDirectory) , tek oturum açma ve Kullanıcı sağlama için tüm uygulama bağlayıcılarının yayımlandığı Azure Marketi uygulama deposudur. Kimlik sağlayıcısı olarak Azure AD kullanan müşteriler burada yayınlanan farklı SaaS uygulaması bağlayıcılarını bulur. BT yöneticileri uygulama galerisinden bağlayıcılar ekler ve ardından çoklu oturum açma ve sağlama için bağlayıcıları yapılandırıp kullanır. Azure AD, çoklu oturum açma için SAML 2,0, OpenID Connect, OAuth ve WS-Besde dahil olmak üzere tüm önemli Federasyon protokollerini destekler. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Uygulamanız SAML veya Openıdconnect destekliyorsa
Azure AD uygulama galerisinde listelenmesini istediğiniz bir çok kiracılı uygulamanız varsa, öncelikle uygulamanızın aşağıdaki çoklu oturum açma teknolojisinden birini desteklediğinden emin olmanız gerekir:

- **OpenID Connect**: uygulamanızın listelenmesini sağlamak IÇIN Azure AD 'de çok kiracılı uygulamayı oluşturun ve uygulamanız IÇIN [Azure AD onay çerçevesini](./consent-framework.md) uygulayın. Herhangi bir müşterinin uygulamaya onay sağlayabilmesi için oturum açma isteğini ortak bir uç noktaya gönderin. Bir kullanıcının erişim belirtecini, kiracı KIMLIĞINE ve Kullanıcı belirtecine göre belirteçte alınan UPN 'yi kontrol edebilirsiniz. Uygulamanızı [Azure Active Directory Uygulama galerisinde listeleme](../azuread-dev/howto-app-gallery-listing.md)bölümünde özetlenen işlemi kullanarak gönderebilirsiniz.

- **SAML**: uygulamanız SAML 2,0 ' yi destekliyorsa, uygulama galeride listelenebilir. [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](../azuread-dev/howto-app-gallery-listing.md)içindeki yönergeleri izleyin.

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Uygulamanız SAML veya Openıdconnect 'i desteklemiyorsa
SAML veya Openıdconnect desteği olmayan uygulamalar, parola çoklu oturum açma teknolojisi aracılığıyla uygulama galerisiyle de tümleştirilebilirler.

Parola oluşturma olarak da bilinen parola çoklu oturum açma, Kimlik Federasyonu desteklemeyen Web uygulamalarına Kullanıcı erişimini ve parolalarını yönetmenizi sağlar. Ayrıca, çeşitli kullanıcıların, kuruluşunuzun sosyal medya uygulaması hesapları gibi tek bir hesabı paylaşması gereken senaryolar için de kullanışlıdır. 

Uygulamanızı bu teknolojiyle listelemek istiyorsanız:
1. [Parola çoklu oturum açmayı](../manage-apps/what-is-single-sign-on.md)YAPıLANDıRMAK için HTML oturum açma sayfasına sahip bir Web uygulaması oluşturun. 
2. [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](../azuread-dev/howto-app-gallery-listing.md)konusunda açıklandığı gibi isteği gönderir.

## <a name="escalations"></a>Yükseltmeleri

Tüm yürüyen değişiklikler için [Azure AD SSO tümleştirmesi ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) e-posta gönderin ve mümkün olan en kısa sürede size geri başlayacağız.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamanızı Azure Active Directory Uygulama galerisinde nasıl listeleyeceğinizi](../azuread-dev/howto-app-gallery-listing.md)öğrenin.