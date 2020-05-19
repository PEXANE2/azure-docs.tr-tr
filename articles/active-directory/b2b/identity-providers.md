---
title: Dış kimlikler için kimlik sağlayıcıları-Azure AD
description: Azure Active Directory B2B işbirliği, kurumsal uygulamalarınıza seçmeli erişim için Multi-Factor Authentication 'ı (MFA) destekler
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 395473e70137ead2b7185d54d165f82e9b3f1f04
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83598153"
---
# <a name="identity-providers-for-external-identities"></a>Dış kimlikler için kimlik sağlayıcıları

Kimlik *sağlayıcısı* , uygulamalara kimlik doğrulama hizmetleri sağlarken kimlik bilgilerini oluşturur, korur ve yönetir. Uygulamalarınızı ve kaynaklarınızı dış kullanıcılarla paylaşırken Azure AD, paylaşım için varsayılan kimlik sağlayıcıdır. Bu, zaten bir Azure AD veya Microsoft hesabı bulunan dış kullanıcıları davet ettiğinizde, sizin bölümlemeden daha fazla yapılandırma olmadan otomatik olarak oturum açabilirler.

Bununla birlikte, kullanıcıların çeşitli kimlik sağlayıcılarıyla oturum açmasını sağlayabilirsiniz. Örneğin, Google ve Facebook dahil olmak üzere Azure AD tarafından desteklenen sosyal kimlik sağlayıcılarıyla Federasyon oluşturabilirsiniz. SAML veya WS-Beslikli protokolleri destekleyen herhangi bir dış kimlik sağlayıcısıyla de federasyona ekleyebilirsiniz. Dış kimlik sağlayıcısı Federasyonu sayesinde, dış kullanıcılara mevcut sosyal veya kurumsal hesaplarıyla uygulamalarınızda oturum açabilme olanağı sunabilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure AD dış kimlikleri, Google ve Facebook ile Federasyon için önceden yapılandırılmıştır. Azure AD kiracınızda bu kimlik sağlayıcılarını ayarlamak için her bir kimlik sağlayıcısında bir uygulama oluşturacak ve kimlik bilgilerini yapılandıracaksınız. Daha sonra Azure AD kiracınıza ekleyebileceğiniz bir istemci veya uygulama KIMLIĞI ve bir istemci ya da uygulama gizli anahtarı elde edersiniz.

Azure AD kiracınıza bir kimlik sağlayıcısı ekledikten sonra:

- Bir dış kullanıcıyı kuruluşunuzdaki uygulamalara veya kaynaklara davet ettiğinizde, dış Kullanıcı bu kimlik sağlayıcısıyla kendi hesabını kullanarak oturum açabilir.
- Uygulamalarınız için [self servis kaydolma](self-service-sign-up-overview.md) özelliğini etkinleştirdiğinizde, dış kullanıcılar, eklediğiniz kimlik sağlayıcıları ile kendi hesaplarını kullanarak uygulamalarınıza kaydolabilir. 

Davetinizi benimseyeniz veya uygulamanıza kaydolduktan sonra dış Kullanıcı oturum açma ve sosyal kimlik sağlayıcısı ile kimlik doğrulama seçeneğine sahiptir:

![Google ve Facebook seçenekleriyle oturum açma ekranını gösteren ekran görüntüsü](media/identity-providers/sign-in-with-social-identity.png)

En iyi oturum açma deneyimi sayesinde, davet eden konuklarına uygulamalarınıza erişirken sorunsuz bir oturum açma deneyimi sağlamak için mümkün olduğunda kimlik sağlayıcılarıyla yararlanın.  

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınıza oturum açma için kimlik sağlayıcıları eklemeyi öğrenmek için aşağıdaki makalelere bakın:

- Sosyal kimlik sağlayıcıları listenize [Google ekleyin](google-federation.md)
- Sosyal kimlik sağlayıcıları listenize [Facebook ekleyin](facebook-federation.md)
- Kimlik sağlayıcısı SAML 2,0 veya WS-beslenir protokolünü destekleyen herhangi bir kuruluşla [doğrudan Federasyon ayarlayın](direct-federation.md) . Doğrudan Federasyonun self servis kaydolma Kullanıcı akışları için bir seçenek olmadığına unutmayın.
