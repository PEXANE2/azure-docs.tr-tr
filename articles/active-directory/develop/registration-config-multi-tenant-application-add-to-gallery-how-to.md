---
title: Azure AD uygulama galerisine çok kiracılı bir uygulama ekleme | Microsoft Docs
description: Özel geliştirmiş çok kiracılı uygulamanızı Azure AD uygulama galerisinde nasıl listelebileceğinizi açıklar.
services: active-directory
documentationCenter: na
author: rwike77
manager: CelesteDG
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ryanwi
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8024130b986d111abe3b470d6b06e86ed6419dc4
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320926"
---
# <a name="add-a-multitenant-application-to-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisine çok kiracılı bir uygulama ekleme

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD Uygulama Galerisi nedir?

Azure Active Directory (Azure AD), bulut tabanlı bir kimlik hizmetidir. [Azure AD Uygulama Galerisi](https://azure.microsoft.com/marketplace/active-directory/all/) , tek oturum açma ve Kullanıcı sağlama için tüm uygulama bağlayıcılarının yayımlandığı Azure Marketi uygulama deposudur. Kimlik sağlayıcısı olarak Azure AD kullanan müşteriler burada yayınlanan farklı SaaS uygulaması bağlayıcılarını bulur. BT yöneticileri uygulama galerisinden bağlayıcılar ekler ve ardından çoklu oturum açma ve sağlama için bağlayıcıları yapılandırıp kullanır. Azure AD, çoklu oturum açma için SAML 2,0, OpenID Connect, OAuth ve WS-Besde dahil olmak üzere tüm önemli Federasyon protokollerini destekler. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Uygulamanız SAML veya Openıdconnect destekliyorsa
Azure AD uygulama galerisinde listelenmesini istediğiniz bir çok kiracılı uygulamanız varsa, öncelikle uygulamanızın aşağıdaki çoklu oturum açma teknolojisinden birini desteklediğinden emin olmanız gerekir:

- **OpenID Connect**: Uygulamanızın listelenmesini sağlamak için Azure AD 'de çok kiracılı uygulamayı oluşturun ve uygulamanız için [Azure AD onay çerçevesini](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) uygulayın. Herhangi bir müşterinin uygulamaya onay sağlayabilmesi için oturum açma isteğini ortak bir uç noktaya gönderin. Bir kullanıcının erişim belirtecini, kiracı KIMLIĞINE ve Kullanıcı belirtecine göre belirteçte alınan UPN 'yi kontrol edebilirsiniz. Uygulamanızı [Azure Active Directory Uygulama galerisinde listeleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)bölümünde özetlenen işlemi kullanarak gönderebilirsiniz.

- **SAML**: Uygulamanız SAML 2,0 ' yi destekliyorsa, uygulama galeride listelenebilir. [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)içindeki yönergeleri izleyin.

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Uygulamanız SAML veya Openıdconnect 'i desteklemiyorsa
SAML veya Openıdconnect desteği olmayan uygulamalar, parola çoklu oturum açma teknolojisi aracılığıyla uygulama galerisiyle de tümleştirilebilirler.

Parola oluşturma olarak da bilinen parola çoklu oturum açma, Kimlik Federasyonu desteklemeyen Web uygulamalarına Kullanıcı erişimini ve parolalarını yönetmenizi sağlar. Ayrıca, çeşitli kullanıcıların, kuruluşunuzun sosyal medya uygulaması hesapları gibi tek bir hesabı paylaşması gereken senaryolar için de kullanışlıdır. 

Uygulamanızı bu teknolojiyle listelemek istiyorsanız:
1. [Parola çoklu oturum açmayı](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)YAPıLANDıRMAK için HTML oturum açma sayfasına sahip bir Web uygulaması oluşturun. 
2. [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)konusunda açıklandığı gibi isteği gönderir.

## <a name="escalations"></a>Yükseltmeleri

Tüm yürüyen değişiklikler için [Azure AD SSO tümleştirmesi ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) e-posta gönderin ve mümkün olan en kısa sürede size geri başlayacağız.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamanızı Azure Active Directory Uygulama galerisinde](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)nasıl listeleyeceğinizi öğrenin.
