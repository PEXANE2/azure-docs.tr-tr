---
title: Microsoft Identity platform-Azure
description: Azure Active Directory (Azure AD) kimlik hizmeti ve geliştirici platformunun bir evrimi olan Microsoft Identity platform hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/03/2019
ms.author: ryanwi
ms.reviewer: agirling, saeeda, benv
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c5fdc1c937136eb512eccf1d4df02a4fcdc3911
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74533096"
---
# <a name="evolution-of-microsoft-identity-platform"></a>Microsoft Identity Platform'un gelişimi

Microsoft kimlik platformu, Azure Active Directory (Azure AD) geliştirici platformunun geliştirilmesiyle ortaya çıkmıştır. Geliştiricilerin kullanıcılara oturum açmasını, Microsoft Graph gibi API 'Leri çağırmak için belirteçleri veya geliştiricilerin derolduğu API 'Leri çağırmasını sağlar. Bir kimlik doğrulama hizmeti, açık kaynak kitaplıkları, uygulama kaydı ve yapılandırma (bir geliştirici portalı ve uygulama API 'SI aracılığıyla), tam geliştirici belgeleri, hızlı başlangıç örnekleri, kod örnekleri, Öğreticiler, nasıl yapılır kılavuzlarından ve Diğer geliştirici içerikleri. Microsoft Identity Platform OAuth 2.0 ve OpenID Connect gibi sektör standardı protokolleri destekler.

Bu aşamada, çoğu geliştirici Azure AD v 1.0 uç noktasından belirteçleri isteyerek Azure AD kimlik doğrulama kitaplığı 'nı (ADAL) kullanarak iş ve okul hesaplarının (Azure AD tarafından sağlanan) kimliğini doğrulamak için Azure AD v 1.0 platformu ile çalıştık Azure portal programlı uygulama yapılandırması için uygulama kaydı ve yapılandırması ve Azure AD Graph API.

Microsoft Identity platform (v 2.0) ile, bu tür kullanıcılar için erişim alanınızı genişletin:

- İş ve okul hesapları (Azure AD tarafından sağlanan hesaplar)
- Kişisel hesaplar (Outlook.com veya Hotmail.com gibi)
- Kendi e-postalarını veya sosyal kimlik bilgilerini (LinkedIn, Facebook, Google gibi) Azure AD B2C teklif aracılığıyla getiren müşterileriniz

Birleşik Microsoft Identity platformu ile kod yazabilir ve uygulamanızda herhangi bir Microsoft kimliğinin kimliğini doğrulayabilirsiniz. Çeşitli platformlar için, Microsoft kimlik doğrulama kitaplığı (MSAL) adlı tam olarak desteklenen bir açık kaynak kitaplığı vardır. MSAL kullanımı basittir, kullanıcılarınız için harika bir çoklu oturum açma (SSO) deneyimi sağlar, yüksek güvenilirlik ve performans elde etmenize yardımcı olur ve Microsoft güvenli geliştirme yaşam döngüsü (SDL) kullanılarak geliştirilir. API 'Leri çağırırken uygulamanızı artımlı izin avantajlarından yararlanarak, uygulamanın kullanımı çalışma zamanında bunu kabul edene kadar daha fazla kapsam için izin isteğini geciktirebilmeniz için uygulamanızı yapılandırabilirsiniz.

Uygulamanızı kaydetmek ve yapılandırmak için Azure portal kullanabilir ve programlı uygulama yapılandırması için Microsoft Graph API 'sini kullanabilirsiniz.

Uygulamanızı kendi hızınızda güncelleştirin. ADAL kitaplıklarıyla oluşturulan uygulamalar desteklenmeye devam eder. ADAL ile oluşturulan uygulamalardan ve MSAL kitaplıklarıyla oluşturulmuş uygulamalarla oluşan karışık uygulama portföylerinin de desteklenme. Yani, en son ADAL ve en son MSAL kullanan uygulamalar, bu kitaplıklar arasındaki paylaşılan belirteç önbelleği tarafından belirtilen portföy genelinde SSO sunacaktır. ADAL 'den MSAL ' e güncelleştirilmiş uygulamalar, yükseltmeden sonra Kullanıcı oturum açma durumunu korur.

## <a name="microsoft-identity-platform-experience"></a>Microsoft kimlik platformu deneyimi

Aşağıdaki diyagramda, uygulama kaydı deneyimi, SDK'lar, uç noktalar ve desteklenen kimlikler de dahil olmak üzere, üst düzeyde Microsoft kimlik deneyimi gösterilir.

![Bugün Microsoft kimlik platformu](./media/about-microsoft-identity-platform/about-microsoft-identity-platform.svg)

### <a name="app-registration-experience"></a>Uygulama kayıt deneyimi

Azure portal **[uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908)** deneyimi, Microsoft Identity platformu ile tümleştirmiş olduğunuz tüm uygulamaları yönetmek için bir portal deneyimidir. Uygulama kayıt portalını kullanıyorsanız bunun yerine Azure portal uygulama kayıt deneyimini kullanmaya başlayın.

Azure AD B2C tümleştirme için (sosyal veya yerel kimliklerin kimliğini doğrularken), uygulamanızı bir Azure AD B2C kiracısına kaydetmeniz gerekir. Bu deneyim Ayrıca Azure portal bir parçasıdır.

**Microsoft Graph içindeki uygulama API 'si** Şu anda önizlemededir. Herhangi bir Microsoft kimliği için kimlik doğrulaması için Microsoft Identity platformu ile tümleştirilmiş uygulamalarınızı programlı olarak yapılandırmak için bu API 'yi kullanın. Ancak, bu API genel kullanılabilirliğe ulaşana kadar Azure AD Graph 1,6 API 'sini ve uygulama bildirimini kullanmanız gerekir.

### <a name="msal-libraries"></a>MSAL kitaplıkları

MSAL kitaplığını kullanarak tüm Microsoft kimliklerinin kimliğini doğrulayan uygulamalar oluşturabilirsiniz. .NET ve JavaScript 'teki MSAL kitaplıkları genel kullanıma sunulmuştur. İOS ve Android için MSAL kitaplıkları önizleme aşamasındadır ve bir üretim ortamında kullanım için uygundur. Genel kullanıma açık olan MSAL ve ADAL sürümleri için yaptığımız için önizleme aşamasında MSAL kitaplıkları için aynı üretim düzeyi desteği sağlıyoruz.

Uygulamanızı Azure AD B2C ile bütünleştirmek için MSAL kitaplıklarını da kullanabilirsiniz.

Web uygulamaları ve Web API 'Leri oluşturmaya yönelik sunucu tarafı kitaplıkları genel kullanıma sunulmuştur: [ASP.net](https://docs.microsoft.com/aspnet/overview) ve [ASP.NET Core](https://docs.microsoft.com/aspnet/core/?view=aspnetcore-2.2)

### <a name="microsoft-identity-platform-endpoint"></a>Microsoft Identity platform uç noktası

Microsoft Identity platform (v 2.0) uç noktası artık OıDC sertifikalı. Microsoft kimlik doğrulama kitaplıkları (MSAL) veya standartlarla uyumlu başka bir kitaplık ile birlikte kullanılır. Endüstri standartlarına uygun olarak insanlarca okunabilir kapsamlar uygular.

## <a name="next-steps"></a>Sonraki adımlar

v1.0 ve v2.0 hakkında daha fazla bilgi edinin.

* [Microsoft Identity platform (v 2.0) genel bakış](v2-overview.md)
* [Geliştiriciler için Azure Active Directory (v 1.0) genel bakış](v1-overview.md)
