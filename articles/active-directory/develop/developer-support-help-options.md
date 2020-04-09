---
title: Azure AD uygulama geliştiricileri için destek ve yardım seçenekleri
description: Microsoft kimlikleriyle tümleşen uygulama oluştururken geliştirmeyle ilgili sorular ve sorunlar için nasıl yardım ve destek alacağınızı öğrenin (Azure Active Directory ve Microsoft hesabı)
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
ms.openlocfilehash: ea1afd2ecac8974c0e865e235288da545a9f1244
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885540"
---
# <a name="support-and-help-options-for-developers"></a>Geliştiriciler için destek ve yardım seçenekleri

Azure Active Directory (Azure AD), Microsoft kimlikleri veya Microsoft Graph API ile tümleştirmeye yeni başlıyorsanız veya uygulamanızda yeni bir özellik uygularken, topluluktan yardım almanız veya geliştirici olarak sahip olduğunuz destek seçeneklerini anlamanız gereken zamanlar vardır. Bu makale, şu seçenekler de dahil olmak üzere bu seçenekleri anlamanıza yardımcı olur:

> [!div class="checklist"]
> * Sorunuzun topluluk tarafından yanıtlanıp yanıtlanmadıveya yoksa uygulamaya çalıştığınız özellik için varolan bir belge nin zaten var olup olmadığını arama
> * Bazı durumlarda, yalnızca belirli bir sorunu hata ayıklamanıza yardımcı olmak için destek araçlarımızı kullanmak istiyorsunuz
> * İhtiyacınız olan yanıtı *bulamazsanız, Stack Taşma* hakkında bir soru sormak isteyebilirsiniz
> * Kimlik doğrulama kitaplığılarımızdan biriyle ilgili bir sorun bulursanız, *Bir GitHub* sorunu
> * Son olarak, biriyle konuşmanız gerekiyorsa, bir destek isteği açmak isteyebilirsiniz

## <a name="search"></a>Arama

Geliştirmeyle ilgili bir sorunuz varsa, yanıtı belgelerde, [GitHub örneklerinde](https://github.com/azure-samples)veya [Stack Overflow](https://www.stackoverflow.com) sorularının yanıtlarında bulabilirsiniz.

### <a name="scoped-search"></a>Kapsamlı arama

Daha hızlı sonuçlar için, aramanızı Sık kullandığınız arama motorunda aşağıdaki sorguyu kullanarak Yığın Taşma, belge ve kod örneklerine kadar kapsamınıza bakın:

```
{Your Search Terms} (site:stackoverflow.com OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/graph)
```

*{Arama Terimleriniz}* arama anahtar kelimelerinize karşılık gelir.

## <a name="use-the-development-support-tools"></a>Geliştirme destek araçlarını kullanma

| Araç  | Açıklama  |
|---------|---------|
| [jwt.ms](https://jwt.ms) | Talep adlarını ve değerlerini çözmek için bir kimlik veya erişim belirteci yapıştırın. |
| [Microsoft Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer)| Microsoft Graph API'ye karşı istekte bulunup yanıtları görmenizi sağlayan araç. |

## <a name="post-a-question-to-stack-overflow"></a>Yığın Taşma'ya soru gönderme

Stack Overflow geliştirme ile ilgili sorular için tercih edilen kanaldır. Burada, geliştirici topluluğunun üyeleri ve Microsoft ekip üyeleri, sorunlarınızı çözmenize doğrudan yardımcı olur.

Arama yoluyla sorunuza bir yanıt bulamıyorsanız, Stack Overflow'a yeni bir soru gönderin. Topluluğun sorunuza daha hızlı bir şekilde tanımlanmasına ve yanıtlamasına yardımcı olmak için soru sorarken aşağıdaki etiketlerden birini kullanın:

|Bileşen/alan  | Etiketler |
|---------|---------|
| ADAL kütüphanesi | [[adal]](https://stackoverflow.com/questions/tagged/adal) |
| MSAL kütüphanesi     | [[msal]](https://stackoverflow.com/questions/tagged/msal) |
| OWIN ara yazılım  | [[azure-etkin-dizin]](https://stackoverflow.com/questions/tagged/azure-active-directory) |
| [Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  | [[azure-ad-b2b]](https://stackoverflow.com/questions/tagged/azure-ad-b2b) |
| [Azure B2C](https://azure.microsoft.com/services/active-directory-b2c/)  | [[azure-ad-b2c]](https://stackoverflow.com/questions/tagged/azure-ad-b2c) |
| [Microsoft Graph API'si](https://developer.microsoft.com/graph/) | [[microsoft-grafik]](https://stackoverflow.com/questions/tagged/microsoft-graph) |
| Kimlik doğrulama veya yetkilendirme konularıyla ilgili diğer tüm alan | [[azure-etkin-dizin]](https://stackoverflow.com/questions/tagged/azure-active-directory) |

Stack Overflow'daki aşağıdaki gönderiler, soru sorma ve kaynak kodu ekleme hakkında ipuçları içerir. Topluluk üyelerinin sorunuza hızlı bir şekilde değerlendirme ve yanıt verme şansını artırmak için aşağıdaki yönergeleri izleyin:

* [Nasıl iyi bir soru sorabilirim](https://stackoverflow.com/help/how-to-ask)
* [En az, eksiksiz ve doğrulanabilir bir örnek oluşturma](https://stackoverflow.com/help/mcve)

## <a name="create-a-github-issue"></a>GitHub sorunu oluşturma

Kitaplıklarımızla ilgili bir hata veya sorun bulursanız, GitHub depolarımızda bir sorun çıkarın. Kitaplıklarımız açık kaynak kodolduğundan, çekme isteği de gönderebilirsiniz.

Kitaplıkların ve GitHub depolarının listesi için aşağıdakileri görün:

* [Azure Active Directory Authentication Library (ADAL)](../azuread-dev/active-directory-authentication-libraries.md) kitaplıkları ve GitHub depoları
* [Microsoft Kimlik Doğrulama Kitaplığı (MSAL)](reference-v2-libraries.md) kitaplıkları ve GitHub depoları

## <a name="open-a-support-request"></a>Destek isteği açma

Biriyle konuşmanız gerekiyorsa, bir destek isteği açabilirsiniz. Azure müşterisiyseniz, birkaç destek seçeneği vardır. Planları karşılaştırmak için [bu sayfaya](https://azure.microsoft.com/support/plans/)bakın. Azure müşterileri için geliştirici desteği de mevcuttur. Geliştirici destek planlarını nasıl satın alınız hakkında bilgi için [bu sayfaya](https://azure.microsoft.com/support/plans/developer/)bakın.

* Azure Destek Planınız zaten varsa, [burada bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

* Azure müşterisi değilseniz, [ticari desteğimiz](https://support.microsoft.com/en-us/gp/contactus81?Audience=Commercial)aracılığıyla Microsoft ile bir destek isteği de açabilirsiniz.

Ayrıca destek almak veya soru sormak için sanal bir [aracı](https://support.microsoft.com/contactus/?ws=support) yı deneyebilirsiniz.
