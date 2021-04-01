---
title: Dış kimlikler için kimlik sağlayıcıları-Azure AD
description: Azure AD 'yi, dış kullanıcılarla paylaşmak üzere varsayılan kimlik sağlayıcınız olarak nasıl kullanacağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdef929b27c636b3908dd7a88eb93adc2382a53f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101687756"
---
# <a name="identity-providers-for-external-identities"></a>Dış kimlikler için kimlik sağlayıcıları

> [!NOTE]
> Bu makalede bahsedilen özelliklerden bazıları Azure Active Directory genel önizleme özellikleridir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Kimlik *sağlayıcısı* , uygulamalara kimlik doğrulama hizmetleri sağlarken kimlik bilgilerini oluşturur, korur ve yönetir. Uygulamalarınızı ve kaynaklarınızı dış kullanıcılarla paylaşırken Azure AD, paylaşım için varsayılan kimlik sağlayıcıdır. Bu, zaten bir Azure AD veya Microsoft hesabı bulunan dış kullanıcıları davet ettiğinizde, sizin bölümlemeden daha fazla yapılandırma olmadan otomatik olarak oturum açabilirler.

Dış kimlikler, Azure AD hesaplarına ek olarak çeşitli kimlik sağlayıcıları da sunar.

- **Microsoft hesapları** (Önizleme): Konuk KULLANıCıLAR, B2B işbirliği davetlerinizi kullanmak için kendi kişisel MICROSOFT HESABı (MSA) kullanabilir. Self Servis kaydolma Kullanıcı akışı ayarlarken, izin verilen kimlik sağlayıcılarından biri olarak [Microsoft hesabı (Önizleme)](microsoft-account.md) ekleyebilirsiniz. Bu kimlik sağlayıcısını Kullanıcı akışları için kullanılabilir hale getirmek için ek yapılandırma gerekmez.

- **E-posta bir kerelik geçiş kodu** (Önizleme): bir daveti benimseme veya paylaşılan bir kaynağa erişme yaparken, Konuk Kullanıcı, e-posta adreslerine gönderilen geçici bir kod talep edebilir. Sonra oturum açmaya devam etmek için bu kodu girer. E-posta bir kerelik geçiş kodu özelliği, diğer yollarla kimlik doğrulamasından geçmediği durumlarda B2B Konuk kullanıcılarının kimliğini doğrular. Self Servis kaydolma Kullanıcı akışı ayarlarken, izin verilen kimlik sağlayıcılarından biri olarak **e-posta One-Time geçiş kodu (Önizleme)** ekleyebilirsiniz. Bazı ayarlar gereklidir; bkz. [e-posta bir kerelik geçiş kodu kimlik doğrulaması](one-time-passcode.md).

- **Google**: Google Federasyonu, dış kullanıcıların kendi Gmail hesaplarıyla uygulamalarınızda oturum açarak davetlerini sizin tarafınızdan kullanmasına izin verir. Google Federation, self servis kaydolma Kullanıcı akışlarınızda da kullanılabilir. Bkz. [Google 'ı bir kimlik sağlayıcısı olarak ekleme](google-federation.md).
   > [!IMPORTANT]
   > **4 ocak 2021 tarihinden itibaren** Google, [WebView oturum açma desteğini kullanımdan](https://developers.googleblog.com/2020/08/guidance-for-our-effort-to-block-less-secure-browser-and-apps.html)kaldırır. Gmail ile Google Federasyonu veya self servis kaydolma kullanıyorsanız, [iş kolu yerel uygulamalarınızı uyumluluk için test](google-federation.md#deprecation-of-webview-sign-in-support)etmeniz gerekir.

- **Facebook**: bir uygulama oluştururken, self servis kaydolma 'yı yapılandırabilir ve Facebook Federasyonu 'ni etkinleştirerek kullanıcıların kendi Facebook hesaplarını kullanarak uygulamanıza kaydolmasını sağlayabilirsiniz. Facebook yalnızca self servis kaydolma Kullanıcı akışları için kullanılabilir ve kullanıcılar sizi davet etmek için bir oturum açma seçeneği olarak kullanılamaz. Bkz. [Facebook 'ı kimlik sağlayıcısı olarak ekleme](facebook-federation.md).

- **Doğrudan Federasyon**: SAML veya WS-Fed protokollerini destekleyen herhangi bir dış kimlik sağlayıcısıyla doğrudan Federasyonu de ayarlayabilirsiniz. Doğrudan Federasyon, dış kullanıcıların uygulamalarınızı mevcut sosyal veya kurumsal hesaplarıyla oturum açarak sizin tarafınızdan davet etmesine olanak tanır. Bkz. [doğrudan Federasyonu ayarlama](direct-federation.md).
   > [!NOTE]
   > Self Servis kaydolma Kullanıcı akışlarınızda doğrudan Federasyon kimlik sağlayıcıları kullanılamaz.

## <a name="adding-social-identity-providers"></a>Sosyal kimlik sağlayıcıları ekleme

Azure AD, self servis kaydolma için varsayılan olarak etkindir, bu nedenle kullanıcılar her zaman bir Azure AD hesabı kullanarak kaydolma seçeneğine sahip olur. Ancak, Google veya Facebook gibi sosyal kimlik sağlayıcıları da dahil olmak üzere diğer kimlik sağlayıcılarını etkinleştirebilirsiniz. Azure AD kiracınızda sosyal kimlik sağlayıcılarını ayarlamak için kimlik sağlayıcısında bir uygulama oluşturacak ve kimlik bilgilerini yapılandıracaksınız. Daha sonra Azure AD kiracınıza ekleyebileceğiniz bir istemci veya uygulama KIMLIĞI ve bir istemci ya da uygulama gizli anahtarı elde edersiniz.

Azure AD kiracınıza bir kimlik sağlayıcısı ekledikten sonra:

- Bir dış kullanıcıyı kuruluşunuzdaki uygulamalara veya kaynaklara davet ettiğinizde, dış Kullanıcı bu kimlik sağlayıcısıyla kendi hesabını kullanarak oturum açabilir.
- Uygulamalarınız için [self servis kaydolma](self-service-sign-up-overview.md) özelliğini etkinleştirdiğinizde, dış kullanıcılar, eklediğiniz kimlik sağlayıcıları ile kendi hesaplarını kullanarak uygulamalarınıza kaydolabilir. Kaydolma sayfasında kullanılabilir hale getirdiğiniz sosyal kimlik sağlayıcıları seçeneklerinden seçim yapabilecekler:

   ![Google ve Facebook seçenekleriyle oturum açma ekranını gösteren ekran görüntüsü](media/identity-providers/sign-in-with-social-identity.png)

En iyi oturum açma deneyimi sayesinde, davet eden konuklarına uygulamalarınıza erişirken sorunsuz bir oturum açma deneyimi sağlamak için mümkün olduğunda kimlik sağlayıcılarıyla yararlanın.  

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınıza oturum açma için kimlik sağlayıcıları eklemeyi öğrenmek için aşağıdaki makalelere bakın:

- [E-posta bir kerelik geçiş kodu kimlik doğrulaması ekleme](one-time-passcode.md)
- [Google](google-federation.md) 'ı izin verilen sosyal kimlik sağlayıcısı olarak ekleyin
- [Facebook](facebook-federation.md) 'ı izin verilen sosyal kimlik sağlayıcısı olarak ekleyin
- Kimlik sağlayıcısı SAML 2,0 veya WS-Fed protokolünü destekleyen herhangi bir kuruluşla [doğrudan Federasyon ayarlayın](direct-federation.md) . Doğrudan Federasyonun self servis kaydolma Kullanıcı akışları için bir seçenek olmadığına unutmayın.
