---
title: Azure AD uygulama galerisine çok kiracılı uygulama ekleme | Microsoft Dokümanlar
description: Azure AD uygulama galerisinde özel olarak geliştirilmiş çok kiracılı uygulamanızı nasıl listelediğinizi açıklar.
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
ms.openlocfilehash: be660ad42c1336d479f1793b20d2994682db1225
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76702768"
---
# <a name="add-a-multitenant-application-to-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisine çok kiracılı bir uygulama ekleme

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisi nedir?

Azure Etkin Dizin (Azure AD) bulut tabanlı bir kimlik hizmetidir. [Azure AD uygulama galerisi,](https://azure.microsoft.com/marketplace/active-directory/all/) tüm uygulama bağlayıcılarının tek oturum açma ve kullanıcı sağlama için yayımlandığı Azure Marketi uygulama mağazasında yer almaktadır. Azure AD'yi kimlik sağlayıcısı olarak kullanan müşteriler, burada yayınlanan farklı SaaS uygulama bağlayıcılarını bulur. BT yöneticileri uygulama galerisinden bağlayıcılar ekler ve ardından tek oturum açma ve sağlama için bağlayıcıları yapılandırıp kullanır. Azure AD, SAML 2.0, OpenID Connect, OAuth ve WS-Fed dahil olmak üzere tüm büyük federasyon protokollerini tek oturum açmak için destekler. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Uygulamanız SAML veya OpenIDConnect'i destekliyorsa
Azure AD uygulama galerisinde listelenmesini istediğiniz çok kiracılı bir uygulamanız varsa, öncelikle uygulamanızın aşağıdaki tek oturum açma teknolojilerinden birini desteklediğinden emin olmalısınız:

- **OpenID Connect**: Uygulamanızın listelenmiş olması için Azure AD'da çok kiracılı bir uygulama oluşturun ve uygulamanız için [Azure AD onay çerçevesini](https://docs.microsoft.com/azure/active-directory/develop/consent-framework) uygulayın. Herhangi bir müşterinin uygulamaya onay verebilmesi için giriş isteğini ortak bir bitiş noktasına gönderin. Bir kullanıcının erişimini kiracı kimliğine ve kullanıcının belirteçte alınan UPN'ine göre denetleyebilirsiniz. [Uygulamanızı Azure Etkin Dizin uygulama galerisinde listeleme'de](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)özetlenen işlemi kullanarak uygulamayı gönderin.

- **SAML**: Uygulamanız SAML 2.0'ı destekliyorsa, uygulama galeride listelenebilir. Azure Etkin [Dizin uygulama galerisinde uygulamanızı listeleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)deki yönergeleri izleyin.

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Uygulamanız SAML veya OpenIDConnect'i desteklemiyorsa
SAML veya OpenIDConnect'i desteklemeyen uygulamalar, parola tek oturum açma teknolojisi yle uygulama galerisine entegre edilebilir.

Parola atlama olarak da adlandırılan parola tek oturum açma, kimlik federasyonunu desteklemeyen web uygulamalarına kullanıcı erişimini ve parolaları yönetmenize olanak tanır. Ayrıca, kuruluşunuzun sosyal medya uygulama hesapları gibi birden çok kullanıcının tek bir hesabı paylaşması gereken senaryolar için de yararlıdır. 

Uygulamanızı bu teknolojiyle listelemek istiyorsanız:
1. Parola tek [oturum](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)açma yapılandırmak için HTML oturum açma sayfası olan bir web uygulaması oluşturun. 
2. Talebi, [uygulamanızı Azure Etkin Dizin uygulama galerisinde listeleme'de](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)açıklandığı şekilde gönderin.

## <a name="escalations"></a>Escalations

Herhangi bir artış için [Azure AD SSO Entegrasyon Ekibi'ne](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) e-posta gönderin ve en kısa sürede size geri dönelim.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamanızı Azure Etkin Dizin uygulama galerisinde nasıl listeleysüreceğinizi](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)öğrenin.
