---
title: Azure Active Directory ile dış Kullanıcı kimlik doğrulamasında esnekliği derleme
description: BT yöneticileri ve Mimarlar için dış kullanıcılar için dayanıklı kimlik doğrulaması oluşturma kılavuzu
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/30/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae8e24341c321fbfffb84d9b072abc4cf925aff3
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95920069"
---
# <a name="build-resilience-in-external-user-authentication"></a>Dış Kullanıcı kimlik doğrulamasında esnekliği derleme

[Azure ACTIVE DIRECTORY B2B işbirliği](https://docs.microsoft.com/azure/active-directory/external-identities/what-is-b2b) (Azure AD B2B), diğer kuruluşlar ve bireyler ile Işbirliğini sağlayan [dış kimliklerin](https://docs.microsoft.com/azure/active-directory/external-identities/delegate-invitations) bir özelliğidir. Kimlik bilgilerini yönetmek zorunda kalmadan, konuk kullanıcıların Azure AD kiracınıza güvenli bir şekilde eklenmesi için izin sağlar. Dış kullanıcılar, kimlik ve kimlik bilgilerini bir dış kimlik sağlayıcısından (IDP), bu nedenle yeni bir kimlik bilgisi anımsamak zorunda kalmaz. 

## <a name="ways-to-authenticate-external-users"></a>Dış kullanıcıların kimliğini doğrulamak için yollar

Dizininizde dış Kullanıcı kimlik doğrulaması yöntemlerini seçebilirsiniz. Microsoft IDPs veya diğer IDPs ' i kullanabilirsiniz.

Her dış IDP ile, IDP 'nin kullanılabilirliğine bir bağımlılık alırsınız. IDPs 'ye bağlanma yöntemleriyle ilgili bazı yöntemler sayesinde, esnekliği 'nizi artırabilirsiniz.

> [!NOTE] 
> Azure AD B2B, herhangi bir kullanıcının [Azure Active Directory](https://docs.microsoft.com/azure/active-directory) kiracısından veya kişisel bir [Microsoft hesabıyla](https://account.microsoft.com/account)kimlik doğrulaması gerçekleştirmek için yerleşik bir becerisine sahiptir. Bu yerleşik seçeneklerle herhangi bir yapılandırma yapmanız gerekmez.

### <a name="considerations-for-resilience-with-other-idps"></a>Diğer IDPs ile esnekliği ilgili hususlar

Konuk Kullanıcı kimlik doğrulaması için dış IDPs kullanırken kesintiler oluşmasını engellemek için sürdürmenize dikkat etmeniz gereken bazı konfigürasyonlar vardır.

| Kimlik Doğrulama Yöntemi| Esnekliği konuları |
| - | - |
| [Facebook](https://docs.microsoft.com/azure/active-directory/external-identities/facebook-federation) veya [Google](https://docs.microsoft.com/azure/active-directory/external-identities/google-federation)gibi sosyal IDPs ile Federasyon.| IDP ile hesabınızı korumanız ve Istemci KIMLIĞINIZI ve Istemci gizli anahtarını yapılandırmanız gerekir. |
| [SAML ve WS-Federation Identity sağlayıcılarıyla doğrudan Federasyon](https://docs.microsoft.com/azure/active-directory/external-identities/direct-federation)| Bağımlı olduğunuz uç noktalarına erişmek için IDP sahibiyle işbirliği yapmanız gerekir. <br>Sertifikaları ve uç noktaları içeren meta verileri korumanız gerekir. |
| [E-posta bir kerelik geçiş kodu](https://docs.microsoft.com/azure/active-directory/external-identities/one-time-passcode)| Bu yöntemle, Microsoft 'un e-posta sistemine, kullanıcının e-posta sistemine ve kullanıcının e-posta istemcisine bağlı olursunuz. |


 

## <a name="self-service-sign-up-preview"></a>Self Servis kaydolma (Önizleme)

Davetleri veya bağlantıları göndermeye alternatif olarak, [self servis kaydolma](https://docs.microsoft.com/azure/active-directory/external-identities/self-service-sign-up-overview)'yı etkinleştirebilirsiniz.  Bu, dış kullanıcıların bir uygulamaya erişim istemesine izin verir. Bir [API Bağlayıcısı](https://docs.microsoft.com/azure/active-directory/external-identities/self-service-sign-up-add-api-connector) oluşturmanız ve bunu bir Kullanıcı akışıyla ilişkilendirmeniz gerekir. Kullanıcı deneyimini tanımlayan Kullanıcı akışlarını bir veya daha fazla uygulamayla ilişkilendirirsiniz. 

Self Servis kaydolma Kullanıcı akışınızı, dış sistemlerin API 'Leri ile bütünleştirmek için [API bağlayıcıları](https://docs.microsoft.com/azure/active-directory/external-identities/api-connectors-overview) kullanmak mümkündür. Bu API Tümleştirmesi [özel onay iş akışları](https://docs.microsoft.com/azure/active-directory/external-identities/self-service-sign-up-add-approvals)için, [kimlik doğrulaması gerçekleştirmek](https://docs.microsoft.com/azure/active-directory/external-identities/code-samples-self-service-sign-up)ve kullanıcı özniteliklerinin üzerine yazma gibi diğer görevleri için kullanılabilir. API 'Leri kullanmak için aşağıdaki bağımlılıkları yönetmeniz gerekir.

* **API Bağlayıcısı kimlik doğrulaması**: bağlayıcı ayarlama, bir uç nokta URL 'si, Kullanıcı adı ve parola gerektirir. Bu kimlik bilgilerinin tutulacağı bir işlem ayarlayın ve herhangi bir süre sonu zamanlamasını bilmeniz için API sahibiyle birlikte çalışın.

* **API Bağlayıcısı yanıtı**: API kullanılamıyorsa, kayıt akışındaki API bağlayıcılarını sorunsuz bir şekilde devretmek için tasarlayın. API Geliştiricilerinize bu [örnek API yanıtlarını](https://docs.microsoft.com/azure/active-directory/external-identities/self-service-sign-up-add-api-connector) ve [sorun giderme için en iyi uygulamaları](https://docs.microsoft.com/azure/active-directory/external-identities/self-service-sign-up-add-api-connector)inceleyin ve sağlayın. Devamlılık, doğrulama hatası ve engelleme yanıtları dahil olmak üzere tüm olası yanıt senaryolarını test etmek için API geliştirme ekibiyle birlikte çalışın. 

## <a name="next-steps"></a>Sonraki adımlar
Yöneticiler ve Mimarlar için esnekliği kaynakları
 
* [Kimlik bilgisi yönetimiyle derleme esnekliği](resilience-in-credentials.md)

* [Cihaz durumlarıyla esnekliği derleme](resilience-with-device-states.md)

* [Sürekli erişim değerlendirmesi (CAE) kullanarak esnekliği oluşturma](resilience-with-continuous-access-evaluation.md)

* [Karma kimlik doğrulamalarınız içinde esnekliği oluşturma](resilience-in-hybrid.md)

* [Uygulama proxy 'Si ile uygulama erişimi içinde derleme esnekliği](resilience-on-premises-access.md)

Geliştiriciler için esnekliği kaynakları

* [Uygulamalarınızda ıAM esnekliği oluşturma](resilience-app-development-overview.md)

* [CıHAR sistemlerinizde derleme esnekliği](resilience-b2c.md)
 
