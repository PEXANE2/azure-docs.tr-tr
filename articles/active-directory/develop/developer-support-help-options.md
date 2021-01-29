---
title: Azure AD uygulama geliştiricileri için destek ve yardım seçenekleri
description: Microsoft kimlikleri ile tümleştirilen uygulama oluştururken geliştirmeyle ilgili sorular ve sorunlar için nasıl yardım ve destek edineceğimizi öğrenin (Azure Active Directory ve Microsoft hesabı)
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/23/2019
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: 1d50e0ed6ac67ed1595b289d27bd08a089840308
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99051915"
---
# <a name="support-and-help-options-for-developers"></a>Geliştiriciler için destek ve yardım seçenekleri

Azure Active Directory (Azure AD), Microsoft kimlikleri veya Microsoft Graph API ile tümleştirmeye başladıysanız veya uygulamanıza yeni bir özellik uyguladığınızda, topluluktan yardım almanız veya bir geliştirici olarak sahip olduğunuz destek seçeneklerini anlamanız gereken durumlar vardır. Bu makale, aşağıdakiler dahil olmak üzere bu seçenekleri anlamanıza yardımcı olur:

> [!div class="checklist"]
> * Sorunuzun topluluk tarafından yanıtlanmadığı veya uygulamaya çalıştığınız özelliğe yönelik mevcut bir belge zaten var olup olmadığını nasıl arayıyoruz
> * Bazı durumlarda, belirli bir sorunu ayıklamanıza yardımcı olması için yalnızca destek araçlarımızı kullanmak isteyeceksiniz
> * İhtiyacınız olan yanıtı bulamazsanız, *Microsoft Q&a* 'da soru sormak isteyebilirsiniz.
> * Kimlik doğrulama kitaplıklarımızdan biriyle ilgili bir sorun bulursanız, bir *GitHub* sorunu oluştur
> * Son olarak, birisiyle konuşmanız gerekiyorsa, bir destek isteği açmak isteyebilirsiniz

## <a name="search"></a>Arayın

Geliştirmeyle ilgili bir sorunuz varsa, yanıtı belgeler, [GitHub örnekleri](https://github.com/azure-samples)veya [bir soru&Microsoft soru](https://docs.microsoft.com/answers/products/) -cevap bölümünde bulabilirsiniz.

### <a name="scoped-search"></a>Kapsamlı arama

Daha hızlı sonuçlar için aramanızı Microsoft Q&A, belgeleri ve kod örneklerini en sevdiğiniz arama altyapısında aşağıdaki sorguyu kullanarak yapın:

```
{Your Search Terms} (site:http://www.docs.microsoft.com/answers/products/ OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/graph)
```

*{Arama koşullarınız}* arama anahtar kelimelerinizle aynıdır.

## <a name="use-the-development-support-tools"></a>Geliştirme destek araçları 'nı kullanma

| Araç  | Açıklama  |
|---------|---------|
| [jwt.ms](https://jwt.ms) | Talep adlarını ve değerlerini çözmek için bir KIMLIK veya erişim belirteci yapıştırın. |
| [Microsoft Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer)| Microsoft Graph API 'sine karşı istek yapmanızı ve yanıtları görmenizi sağlayan araç. |

## <a name="post-a-question-to-microsoft-qa"></a>Microsoft Q&bir soru gönderin

Microsoft Q&A, geliştirmeyle ilgili sorular için tercih edilen kanaldır. Burada, geliştirici topluluğu ve Microsoft takım üyelerinin üyeleri, sorunlarınızı çözmenize yardımcı olmak için doğrudan ilgilidir.

Arama ile sorunuz için bir yanıt bulamazsanız, Microsoft Q&A 'ya yeni bir soru gönderin. Topluluğun sorunuzu daha hızlı belirlemesine ve yanıt vermesi için soru sorarken aşağıdaki etiketlerden birini kullanın:

|Bileşen/alan  | Etiketler |
|---------|---------|
| ADAL kitaplığı | [adal](https://docs.microsoft.com/answers/topics/azure-ad-adal-deprecation.html) |
| MSAL kitaplığı     | [msal](https://docs.microsoft.com/answers/topics/azure-ad-msal.html) |
| OWıN ara yazılımı  | [[Azure-Active-Directory]](https://docs.microsoft.com/answers/topics/azure-active-directory.html) |
| [Azure B2B](../external-identities/what-is-b2b.md)  | [[Azure-AD-B2B]](https://docs.microsoft.com/answers/topics/azure-ad-b2b.html) |
| [Azure B2C](https://azure.microsoft.com/services/active-directory-b2c/)  | [[Azure-AD-B2C]](https://docs.microsoft.com/answers/topics/azure-ad-b2c.html) |
| [Microsoft Graph API](https://developer.microsoft.com/graph/) | [[Azure-AD-grafik]](https://docs.microsoft.com/answers/topics/azure-ad-graph.html) |
| Kimlik doğrulama veya yetkilendirme konularıyla ilgili başka bir alan | [[Azure-Active-Directory]](https://docs.microsoft.com/answers/topics/azure-ad-graph.html) |

Microsoft Q&'e ait aşağıdaki gönderiler, soru sorma ve kaynak kodu ekleme hakkında ipuçları içerir. Topluluk üyelerinin sorunuzu hızlı bir şekilde değerlendirmesine ve yanıt verebilmesini artırmak için bu yönergeleri izleyin:

* [Nasıl yaparım? iyi bir soru sorun](https://docs.microsoft.com/answers/articles/24951/how-to-write-a-quality-question.html)
* [En az, tamamlanmış ve doğrulanabilir örnek oluşturma](https://docs.microsoft.com/answers/articles/24907/how-to-write-a-quality-answer.html)

## <a name="create-a-github-issue"></a>GitHub sorunu oluştur

Kitaplıklarımızla ilgili bir hata veya sorun bulursanız, GitHub depolarımızda bir sorun oluşturabilir. Kitaplıklarımız açık kaynak olduğundan, çekme isteği de gönderebilirsiniz.

Kitaplıkların ve GitHub depolarının listesi için aşağıdakilere bakın:

* [Azure Active Directory kimlik doğrulaması kitaplığı (ADAL)](../azuread-dev/active-directory-authentication-libraries.md) kitaplıkları ve GitHub depoları
* [Microsoft kimlik doğrulama kitaplığı (msal)](reference-v2-libraries.md) kitaplıkları ve GitHub depoları

## <a name="open-a-support-request"></a>Bir destek isteği açın

Birisiyle konuşmanız gerekiyorsa, bir destek isteği açabilirsiniz. Bir Azure müşterisiyseniz, çeşitli destek seçenekleri mevcuttur. Planları karşılaştırmak için [Bu sayfaya](https://azure.microsoft.com/support/plans/)bakın. Azure müşterileri için de Geliştirici desteği sunulmaktadır. Geliştirici destek planlarını satın alma hakkında daha fazla bilgi için [Bu sayfaya](https://azure.microsoft.com/support/plans/developer/)bakın.

* Zaten bir Azure Destek planınız varsa, [burada bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

* Bir Azure müşterisiyseniz, [ticari destek](https://support.serviceshub.microsoft.com/supportforbusiness)aracılığıyla Microsoft ile bir destek isteği de açabilirsiniz.

Ayrıca, destek almak veya soru sormak için bir [sanal aracıyı](https://support.microsoft.com/contactus/?ws=support) deneyebilirsiniz.