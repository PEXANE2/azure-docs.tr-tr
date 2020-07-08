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
ms.openlocfilehash: 2284d015b451872753dd0855cac42e6f1926545c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83712171"
---
# <a name="identity-providers-for-external-identities"></a>Dış kimlikler için kimlik sağlayıcıları

Kimlik *sağlayıcısı* , uygulamalara kimlik doğrulama hizmetleri sağlarken kimlik bilgilerini oluşturur, korur ve yönetir. Uygulamalarınızı ve kaynaklarınızı dış kullanıcılarla paylaşırken Azure AD, paylaşım için varsayılan kimlik sağlayıcıdır. Bu, zaten bir Azure AD veya Microsoft hesabı bulunan dış kullanıcıları davet ettiğinizde, sizin bölümlemeden daha fazla yapılandırma olmadan otomatik olarak oturum açabilirler.

Bununla birlikte, kullanıcıların çeşitli kimlik sağlayıcılarıyla oturum açmasını sağlayabilirsiniz.

- **Google**: Google Federasyonu, dış kullanıcıların kendi Gmail hesaplarıyla uygulamalarınızda oturum açarak davetlerini sizin tarafınızdan kullanmasına izin verir. Google Federation, self servis kaydolma Kullanıcı akışlarınızda da kullanılabilir.
   > [!NOTE]
   > Geçerli self servis kaydolma önizlemesinde, bir Kullanıcı akışı bir uygulamayla ilişkiliyse ve bu uygulamaya bir Kullanıcı daveti gönderirseniz, Kullanıcı daveti kullanmak için bir Gmail hesabı kullanamaz. Geçici bir çözüm olarak, Kullanıcı self servis kaydolma işlemini gerçekleştirebilir. Ya da, farklı bir uygulamaya erişerek veya ' de My Apps Portal ' ı kullanarak daveti kullanabilirler https://myapps.microsoft.com .

- **Facebook**: bir uygulama oluştururken, self servis kaydolma 'yı yapılandırabilir ve Facebook Federasyonu 'ni etkinleştirerek kullanıcıların kendi Facebook hesaplarını kullanarak uygulamanıza kaydolmasını sağlayabilirsiniz. Facebook yalnızca self servis kaydolma Kullanıcı akışları için kullanılabilir ve kullanıcılar sizi davet etmek için bir oturum açma seçeneği olarak kullanılamaz.

- **Doğrudan Federasyon**: SAML veya WS-beslikli protokolleri destekleyen herhangi bir dış kimlik sağlayıcısı ile doğrudan Federasyonu de ayarlayabilirsiniz. Doğrudan Federasyon, dış kullanıcıların uygulamalarınızı mevcut sosyal veya kurumsal hesaplarıyla oturum açarak sizin tarafınızdan davet etmesine olanak tanır. 
   > [!NOTE]
   > Self Servis kaydolma Kullanıcı akışlarınızda doğrudan Federasyon kimlik sağlayıcıları kullanılamaz.


## <a name="how-it-works"></a>Nasıl çalışır?

Azure AD dış kimlikleri self servis kaydolma özelliği, kullanıcıların Azure AD, Google veya Facebook hesabıyla kaydolmalarına olanak tanır. Azure AD kiracınızda sosyal kimlik sağlayıcılarını ayarlamak için her bir kimlik sağlayıcısında bir uygulama oluşturacak ve kimlik bilgilerini yapılandıracaksınız. Daha sonra Azure AD kiracınıza ekleyebileceğiniz bir istemci veya uygulama KIMLIĞI ve bir istemci ya da uygulama gizli anahtarı elde edersiniz.

Azure AD kiracınıza bir kimlik sağlayıcısı ekledikten sonra:

- Bir dış kullanıcıyı kuruluşunuzdaki uygulamalara veya kaynaklara davet ettiğinizde, dış Kullanıcı bu kimlik sağlayıcısıyla kendi hesabını kullanarak oturum açabilir.
- Uygulamalarınız için [self servis kaydolma](self-service-sign-up-overview.md) özelliğini etkinleştirdiğinizde, dış kullanıcılar, eklediğiniz kimlik sağlayıcıları ile kendi hesaplarını kullanarak uygulamalarınıza kaydolabilir.

> [!NOTE]
> Azure AD, self servis kaydolma için varsayılan olarak etkindir, bu nedenle kullanıcılar her zaman bir Azure AD hesabı kullanarak kaydolma seçeneğine sahip olur.

Davetinizi benimseyeniz veya uygulamanıza kaydolduktan sonra dış Kullanıcı oturum açma ve sosyal kimlik sağlayıcısı ile kimlik doğrulama seçeneğine sahiptir:

![Google ve Facebook seçenekleriyle oturum açma ekranını gösteren ekran görüntüsü](media/identity-providers/sign-in-with-social-identity.png)

En iyi oturum açma deneyimi sayesinde, davet eden konuklarına uygulamalarınıza erişirken sorunsuz bir oturum açma deneyimi sağlamak için mümkün olduğunda kimlik sağlayıcılarıyla yararlanın.  

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınıza oturum açma için kimlik sağlayıcıları eklemeyi öğrenmek için aşağıdaki makalelere bakın:

- Sosyal kimlik sağlayıcıları listenize [Google ekleyin](google-federation.md)
- Sosyal kimlik sağlayıcıları listenize [Facebook ekleyin](facebook-federation.md)
- Kimlik sağlayıcısı SAML 2,0 veya WS-beslenir protokolünü destekleyen herhangi bir kuruluşla [doğrudan Federasyon ayarlayın](direct-federation.md) . Doğrudan Federasyonun self servis kaydolma Kullanıcı akışları için bir seçenek olmadığına unutmayın.
