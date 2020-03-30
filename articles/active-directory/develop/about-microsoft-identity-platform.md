---
title: Microsoft kimlik platformunun evrimi - Azure
description: Azure Active Directory (Azure AD) kimlik hizmeti ve geliştirici platformunun bir evrimi olan Microsoft kimlik platformu hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: overview
ms.workload: identity
ms.date: 12/09/2019
ms.author: ryanwi
ms.reviewer: agirling, saeeda, benv
ms.custom: aaddev
ms.openlocfilehash: 8714b7a96197cb4a59b29bada31b5559961bf8e3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78300222"
---
# <a name="evolution-of-microsoft-identity-platform"></a>Microsoft Identity Platform'un gelişimi

Microsoft kimlik platformu, Azure Active Directory (Azure AD) geliştirici platformunun geliştirilmesiyle ortaya çıkmıştır. Geliştiricilerin kullanıcıları oturum açan uygulamalar oluşturmasına, Microsoft Graph veya geliştiricilerin oluşturduğu API'ler gibi API'leri aramak için belirteçler almalarına olanak tanır. Bir kimlik doğrulama hizmeti, açık kaynak kitaplıkları, uygulama kaydı ve yapılandırma (geliştirici portalı ve uygulama API aracılığıyla), tam geliştirici belgeleri, hızlı başlatma örnekleri, kod örnekleri, öğreticiler, nasıl yapılacağını kılavuzları ve diğer geliştirici içeriği. Microsoft Identity Platform OAuth 2.0 ve OpenID Connect gibi sektör standardı protokolleri destekler.

Şimdiye kadar çoğu geliştirici, Azure AD kimlik doğrulama kitaplığını (ADAL), Azure portalı kullanarak Azure AD v1.0 bitiş noktasından belirteçler isteyerek iş ve okul hesaplarının (Azure AD tarafından sağlanan) kimliğini doğrulamak için Azure AD v1.0 platformuyla birlikte çalıştı. uygulama kaydı ve yapılandırması ve programlı uygulama yapılandırması için Microsoft Graph API.

Birleştirilmiş Microsoft kimlik platformu (v2.0) ile bir kez kod yazabilir ve uygulamanıza herhangi bir Microsoft kimliğinin kimliğini doğrulayabilirsiniz. Çeşitli platformlar için, tam olarak desteklenen açık kaynak Microsoft Kimlik Doğrulama Kitaplığı (MSAL) kimlik platformu uç noktalarına karşı kullanılması önerilir. MSAL kullanımı kolaydır, kullanıcılarınız için harika tek oturum açma (SSO) deneyimleri sağlar, yüksek güvenilirlik ve performans elde etmenize yardımcı olur ve Microsoft Secure Development Lifecycle (SDL) kullanılarak geliştirilmiştir. API'leri ararken, uygulamanızı artımlı onaydan yararlanacak şekilde yapılandırabilirsiniz, bu da uygulamanın kullanımı bunu çalışma zamanında garanti edene kadar daha fazla kapsam için onay isteğini geciktirmenize olanak tanır.  MSAL ayrıca Azure Active Directory B2C'yi de destekler, böylece müşterileriniz uygulamalarınız ve API'lerinize tek oturum açma erişimi elde etmek için tercih ettikleri sosyal, kurumsal veya yerel hesap kimliklerini kullanır.

Microsoft kimlik platformu ile erişiminizi bu tür kullanıcılara genişletin:

- İş ve okul hesapları (Azure AD tarafından sağlanan hesaplar)
- Kişisel hesaplar (Outlook.com veya Hotmail.com gibi)
- MSAL ve Azure AD B2C aracılığıyla kendi e-postalarını veya sosyal kimliklerini (LinkedIn, Facebook, Google gibi) getiren müşterileriniz

Uygulamanızı kaydetmek ve yapılandırmak için Azure portalını kullanabilir ve programlı uygulama yapılandırması için Microsoft Graph API'yi kullanabilirsiniz.

Uygulamanızı kendi hızınızda güncelleyin. ADAL kütüphaneleri ile yapılan başvurular desteklenmeye devam etmektedir. ADAL ile yapılan uygulamalar ve MSAL kütüphaneleri ile yapılan uygulamalardan oluşan karma uygulama portföyleri de destekleniyor. Bu, en son ADAL ve en son MSAL'ı kullanan uygulamaların, bu kitaplıklar arasındaki paylaşılan belirteç önbelleği tarafından sağlanan sso'yu portföy genelinde sunacağı anlamına gelir. ADAL'dan MSAL'a güncellenen uygulamalar, yükseltme yapıldıktan sonra kullanıcı oturum açma durumunu koruyacaktır.

## <a name="microsoft-identity-platform-experience"></a>Microsoft kimlik platformu deneyimi

Aşağıdaki diyagramda, uygulama kaydı deneyimi, SDK'lar, uç noktalar ve desteklenen kimlikler de dahil olmak üzere, üst düzeyde Microsoft kimlik deneyimi gösterilir.

![Bugün Microsoft kimlik platformu](./media/about-microsoft-identity-platform/about-microsoft-identity-platform.svg)

### <a name="app-registration-experience"></a>Uygulama kayıt deneyimi

Azure portalı **[App kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908)** deneyimi, Microsoft kimlik platformuyla tümleşik ettiğiniz tüm uygulamaları yönetmek için tek portal deneyimidir. Uygulama Kayıt Portalı'nı kullanıyorsanız, bunun yerine Azure portalı uygulama kayıt deneyimini kullanmaya başlayın.

Azure AD B2C ile tümleştirme için (sosyal veya yerel kimliklerin doğruluğunu doğrularken), uygulamanızı bir Azure AD B2C kiracısına kaydetmeniz gerekir. Bu deneyim aynı zamanda Azure portalının bir parçasıdır.

Uygulamalarınızı Microsoft kimlik platformuyla tümleşik olarak microsoft kimliğinidoğrulamak için programlı olarak yapılandırmak için [Uygulama API'sini](https://docs.microsoft.com/graph/api/resources/application?view=graph-rest-1.0) kullanın.

### <a name="msal-libraries"></a>MSAL kütüphaneleri

Tüm Microsoft kimliklerini doğrulayan uygulamalar oluşturmak için MSAL kitaplığını kullanabilirsiniz. .NET ve JavaScript'teki MSAL kitaplıkları genellikle kullanılabilir. iOS ve Android için MSAL kitaplıkları önizlemededir ve üretim ortamında kullanıma uygundur. MSAL ve ADAL sürümlerinde genel olarak kullanılabilen sürümlerde olduğu gibi önizlemede MSAL kitaplıkları için de aynı üretim düzeyi desteğini sağlıyoruz.

Uygulamanızı Azure AD B2C ile tümleştirmek için MSAL kitaplıklarını da kullanabilirsiniz.

Web uygulamaları ve web API'leri oluşturmak için sunucu tarafındaki kitaplıklar genellikle kullanılabilir: [ASP.NET](https://docs.microsoft.com/aspnet/overview) ve [ASP.NET Core](https://docs.microsoft.com/aspnet/core/?view=aspnetcore-2.2)

### <a name="microsoft-identity-platform-endpoint"></a>Microsoft kimlik platformu bitiş noktası

Microsoft kimlik platformu (v2.0) bitiş noktası artık OIDC sertifikalıdır. Microsoft Kimlik Doğrulama Kitaplıkları (MSAL) veya standartlara uygun başka bir kitaplıkla çalışır. Endüstri standartlarına uygun olarak, insan tarafından okunabilir kapsamları uygular.

## <a name="next-steps"></a>Sonraki adımlar

v1.0 ve v2.0 hakkında daha fazla bilgi edinin.

* [Microsoft kimlik platformu (v2.0) genel bakış](v2-overview.md)
* [Geliştiriciler için Azure Etkin Dizini (v1.0) genel bakış](../azuread-dev/v1-overview.md)
