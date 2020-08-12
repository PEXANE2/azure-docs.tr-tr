---
title: Microsoft Identity platform-Azure
description: Azure Active Directory (Azure AD) kimlik hizmeti ve geliştirici platformunun bir evrimi olan Microsoft Identity platform hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: overview
ms.workload: identity
ms.date: 12/09/2019
ms.author: ryanwi
ms.reviewer: agirling, saeeda, benv, marsma
ms.custom: aaddev
ms.openlocfilehash: 23ab8c5092a546a7b66b93900c6c083ac8e84394
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88117779"
---
# <a name="evolution-of-microsoft-identity-platform"></a>Microsoft Identity Platform'un gelişimi

Microsoft kimlik platformu, Azure Active Directory (Azure AD) geliştirici platformunun geliştirilmesiyle ortaya çıkmıştır. Geliştiricilerin kullanıcılara oturum açmasını, Microsoft Graph gibi API 'Leri çağırmak için belirteçleri veya geliştiricilerin derolduğu API 'Leri çağırmasını sağlar. Bir kimlik doğrulama hizmeti, açık kaynak kitaplıkları, uygulama kaydı ve yapılandırma (bir geliştirici portalı ve uygulama API 'SI aracılığıyla), tam geliştirici belgeleri, hızlı başlangıç örnekleri, kod örnekleri, Öğreticiler, nasıl yapılır kılavuzlarından ve diğer geliştirici içeriğiyle oluşur. Microsoft Identity Platform OAuth 2.0 ve OpenID Connect gibi sektör standardı protokolleri destekler.

Birçok geliştirici daha önce Azure AD v 1.0 platformundan, Azure AD kimlik doğrulama kitaplığı (ADAL), uygulama kaydı ve yapılandırma için Azure portal ve programlı uygulama yapılandırması için Microsoft Graph API 'sini kullanarak iş ve okul hesaplarının (Azure AD tarafından sağlanan) kimlik doğrulaması yapmasını sağlar.

Birleşik Microsoft Identity platformu (v 2.0) ile, bir kez kod yazabilir ve uygulamanızda herhangi bir Microsoft kimliğinin kimliğini doğrulayabilirsiniz. Birçok platformda, tam olarak desteklenen açık kaynaklı Microsoft kimlik doğrulama kitaplığı (MSAL) kimlik Platform uç noktalarına karşı kullanım için önerilir. MSAL kullanımı basittir, kullanıcılarınız için harika bir çoklu oturum açma (SSO) deneyimi sağlar, yüksek güvenilirlik ve performans elde etmenize yardımcı olur ve Microsoft güvenli geliştirme yaşam döngüsü (SDL) kullanılarak geliştirilir. API 'Leri çağırırken uygulamanızı artımlı izin avantajlarından yararlanarak, uygulamanın kullanımı çalışma zamanında bunu kabul edene kadar daha fazla kapsam için izin isteğini geciktirebilmeniz için uygulamanızı yapılandırabilirsiniz.  MSAL Ayrıca Azure Active Directory B2C destekler, böylece müşterileriniz, uygulamalarınıza ve API 'lerinize çoklu oturum açma erişimi sağlamak için tercih edilen sosyal, kurumsal veya yerel hesap kimliklerini kullanır.

Microsoft Identity platformu ile bu tür kullanıcılar için erişim alanınızı genişletin:

- İş ve okul hesapları (Azure AD tarafından sağlanan hesaplar)
- Kişisel hesaplar (Outlook.com veya Hotmail.com gibi)
- MSAL ve Azure AD B2C aracılığıyla kendi e-postalarını veya sosyal kimliğini (LinkedIn, Facebook, Google) getiren müşterileriniz

Uygulamanızı kaydetmek ve yapılandırmak için Azure portal kullanabilir ve programlı uygulama yapılandırması için Microsoft Graph API 'sini kullanabilirsiniz.

Uygulamanızı kendi hızınızda güncelleştirin. ADAL kitaplıklarıyla oluşturulan uygulamalar desteklenmeye devam eder. ADAL ile oluşturulan uygulamalardan ve MSAL kitaplıklarıyla oluşturulmuş uygulamalarla oluşan karışık uygulama portföylerinin de desteklenme. Yani, en son ADAL ve en son MSAL kullanan uygulamalar, bu kitaplıklar arasındaki paylaşılan belirteç önbelleği tarafından belirtilen portföy genelinde SSO sunacaktır. ADAL 'den MSAL ' e güncelleştirilmiş uygulamalar, yükseltmeden sonra Kullanıcı oturum açma durumunu korur.

## <a name="microsoft-identity-platform-experience"></a>Microsoft kimlik platformu deneyimi

Aşağıdaki diyagramda, uygulama kaydı deneyimi, SDK'lar, uç noktalar ve desteklenen kimlikler de dahil olmak üzere, üst düzeyde Microsoft kimlik deneyimi gösterilir.

![Bugün Microsoft kimlik platformu](./media/about-microsoft-identity-platform/about-microsoft-identity-platform.svg)

### <a name="app-registration-experience"></a>Uygulama kayıt deneyimi

Azure portal **[uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908)** deneyimi, Microsoft Identity platformu ile tümleştirmiş olduğunuz tüm uygulamaları yönetmek için bir portal deneyimidir. Uygulama kayıt portalını kullanıyorsanız bunun yerine Azure portal uygulama kayıt deneyimini kullanmaya başlayın.

Azure AD B2C tümleştirme için (sosyal veya yerel kimliklerin kimliğini doğrularken), uygulamanızı bir Azure AD B2C kiracısına kaydetmeniz gerekir. Bu deneyim Ayrıca Azure portal bir parçasıdır.

Herhangi bir Microsoft kimliği için kimlik doğrulaması için Microsoft Identity platformu ile tümleştirilmiş uygulamalarınızı programlı bir şekilde yapılandırmak için [uygulama API](/graph/api/resources/application?view=graph-rest-1.0) 'sini kullanın.

### <a name="msal-libraries"></a>MSAL kitaplıkları

MSAL kitaplığını kullanarak tüm Microsoft kimliklerinin kimliğini doğrulayan uygulamalar oluşturabilirsiniz. .NET ve JavaScript 'teki MSAL kitaplıkları genel kullanıma sunulmuştur. İOS ve Android için MSAL kitaplıkları önizleme aşamasındadır ve bir üretim ortamında kullanım için uygundur. Genel kullanıma açık olan MSAL ve ADAL sürümleri için yaptığımız için önizleme aşamasında MSAL kitaplıkları için aynı üretim düzeyi desteği sağlıyoruz.

Uygulamanızı Azure AD B2C ile bütünleştirmek için MSAL kitaplıklarını da kullanabilirsiniz.

Web uygulamaları ve Web API 'Leri oluşturmaya yönelik sunucu tarafı kitaplıkları genel kullanıma sunulmuştur: [ASP.net](/aspnet/overview) ve [ASP.NET Core](/aspnet/core/?view=aspnetcore-2.2)

### <a name="microsoft-identity-platform-endpoint"></a>Microsoft Identity platform uç noktası

Microsoft Identity platform (v 2.0) uç noktası artık OıDC sertifikalı. Microsoft kimlik doğrulama kitaplıkları (MSAL) veya standartlarla uyumlu başka bir kitaplık ile birlikte kullanılır. Endüstri standartlarına uygun olarak insanlarca okunabilir kapsamlar uygular.

## <a name="next-steps"></a>Sonraki adımlar

v1.0 ve v2.0 hakkında daha fazla bilgi edinin.

* [Microsoft Identity platform (v 2.0)](../develop/v2-overview.md) genel bakış
* [Geliştiriciler için Azure Active Directory (v 1.0)](v1-overview.md) genel bakış