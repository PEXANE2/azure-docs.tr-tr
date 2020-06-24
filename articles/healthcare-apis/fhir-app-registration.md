---
title: FHıR için Azure API Azure Active Directory uygulamalarını kaydetme
description: Bu öğreticide, Azure için FHıR ve FHıR sunucusu için Azure API 'SI için hangi uygulamaların kaydedilmesi gerektiği açıklanmaktadır.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: overview
ms.reviewer: mihansen
ms.author: matjazl
author: matjazl
ms.date: 10/13/2019
ms.openlocfilehash: d46280c5009769a9a1962cc0bbb489872a815556
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "84820219"
---
# <a name="register-the-azure-active-directory-apps-for-azure-api-for-fhir"></a>FHıR için Azure API Azure Active Directory uygulamalarını kaydetme

FHıR için Azure API 'sini veya Azure için FHıR sunucusunu (OSS) ayarlarken aralarından seçim yapabileceğiniz çeşitli yapılandırma seçenekleriniz vardır. Açık kaynak için kendi kaynak uygulaması kaydınızı oluşturmanız gerekir. FHıR için Azure API 'SI için bu kaynak uygulaması otomatik olarak oluşturulur.

## <a name="application-registrations"></a>Uygulama kayıtları

Uygulamanın Azure AD ile etkileşime geçmesini sağlamak için, kayıtlı olması gerekir. FHıR sunucusu bağlamında, tartışmak için iki tür uygulama kaydı vardır:

1. Kaynak uygulama kayıtları.
1. İstemci uygulama kayıtları.

**Kaynak uygulamalar** , Azure AD ile güvenli hale GETIRILEN bir API veya kaynak IÇIN Azure AD 'de yer alan temsilleridir. Bu, özellikle de fhır IÇIN Azure API 'sidir. Azure API için bir kaynak uygulaması, hizmeti sağladığınızda otomatik olarak oluşturulur, ancak açık kaynaklı sunucu kullanıyorsanız, Azure AD 'ye [bir kaynak uygulaması kaydetmeniz](register-resource-azure-ad-client-app.md) gerekir. Bu kaynak uygulamanın bir tanımlayıcı URI 'SI olacaktır. Bu URI 'nin FHıR sunucusunun URI 'siyle aynı olması önerilir. Bu URI, `Audience` fhır sunucusu için olarak kullanılmalıdır. Bir istemci uygulaması, bir belirteç istediğinde bu FHıR sunucusuna erişim isteğinde bulunabilir.

*İstemci uygulamaları* , belirteç istemek için istemcilerin kayıtlarıdır. Genellikle OAuth 2,0 ' de, en az üç farklı uygulama türünü ayırt ediyoruz:

1. Azure AD 'de Web Apps olarak da bilinen **gizli istemciler**. Gizli istemciler, oturum açmış kullanıcının adına geçerli kimlik bilgileri sunan bir belirteç almak için [yetkilendirme kodu akışını](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code) kullanan uygulamalardır. Gizli istemciler olarak adlandırılırlar, çünkü bir gizli dizi tutabilir ve bir belirteç için kimlik doğrulama kodunu değiş tokuşu yaparken bu gizli anahtarı Azure AD 'ye sunacaktır. Gizli istemciler, istemci gizli anahtarını kullanarak kendileri kimlik doğrulayabileceğinden, ortak istemcilerden daha fazla güvenilir ve daha uzun süreli belirteçlere sahip olabilir ve yenileme belirteci verilebilir. [Gizli bir istemciyi kaydetme](register-confidential-azure-ad-client-app.md)hakkındaki ayrıntıları okuyun. İstemcinin yetkilendirme kodunu alacak yanıt URL 'sini kaydetmek için önemli olduğunu unutmayın.
1. **Ortak istemciler**. Bunlar, gizli tutan istemcilerdir. Genellikle bu bir mobil cihaz uygulaması veya istemcideki bir parolanın bir kullanıcı tarafından keşfedildiği tek sayfalı bir JavaScript uygulamasıdır. Ortak istemciler ayrıca yetkilendirme kodu akışını kullanır, ancak belirteç alırken gizli dizi belirteçlerine sahip olabilecek ve yenileme belirteci olmayan bir gizli dizi bulunmasına izin verilmez. [Ortak istemciyi kaydetme](register-public-azure-ad-client-app.md)hakkındaki ayrıntıları okuyun.
1. Hizmet istemcileri. Bu istemciler, [istemci kimlik bilgileri akışını](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)kullanarak kendileri adına belirteçleri (bir kullanıcı adına değil) alır. Genellikle, FHıR sunucusuna etkileşimli olmayan bir şekilde erişen uygulamaları temsil eder. Örnek, bir alma işlemi olacaktır. Bir hizmet istemcisi kullanırken, uç noktaya çağrısıyla belirteç alma işlemini başlatmak gerekli değildir `/authorize` . Bir hizmet istemcisi, `/token` bir belirteci almak için doğrudan uç noktaya gidebilir ve ISTEMCI kimliği ve istemci gizli anahtarı sunabilir. [Hizmet istemcisini kaydetme](register-service-azure-ad-client-app.md) hakkındaki ayrıntıları okuyun

## <a name="next-steps"></a>Sonraki adımlar

Bu genel bakışta, bir FHıR API 'SI ile çalışabilmek için ihtiyacınız olabilecek uygulama kaydı türleri üzerinde ilerdiniz.

Kurulumunuzu temel alarak, lütfen uygulamalarınızı kaydetmek için nasıl yapılır kılavuzlarıyla bakın

* [Kaynak uygulamasını kaydetme](register-resource-azure-ad-client-app.md)
* [Gizli istemci uygulamasını kaydetme](register-confidential-azure-ad-client-app.md)
* [Genel istemci uygulamasını kaydetme](register-public-azure-ad-client-app.md)
* [Hizmet uygulaması kaydetme](register-service-azure-ad-client-app.md)

Uygulamalarınızı kaydettikten sonra, FHıR için Azure API 'yi dağıtabilirsiniz.

>[!div class="nextstepaction"]
>[FHIR için Azure API'sini dağıtma](fhir-paas-powershell-quickstart.md)