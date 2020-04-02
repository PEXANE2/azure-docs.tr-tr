---
title: Azure Marketi SaaS Uygulamaları Teknik Yayın Kılavuzu
description: SaaS uygulamalarını Azure Marketi'nde yayınlamak için adım adım kılavuz ve yayımlama denetim listeleri
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
author: keithcharlie
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: kevidal
ms.openlocfilehash: 641297ea74a08dea163cf768b8e9b245348824a4
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80544292"
---
# <a name="saas-applications-offer-publishing-guide"></a>SaaS Uygulaması Teklifi Yayımlama Kılavuzu

SaaS uygulamaları üç farklı eylem çağrısıyla pazarda yayınlanabilir: "Bana Ulaşın", "Şimdi deneyin" ve "Hemen alın." Bu kılavuz, her biri için gereksinimler de dahil olmak üzere bu üç seçeneği açıklar. 

## <a name="offer-overview"></a>Teklife genel bakış  

SaaS uygulamaları her iki Azure Storefronts mevcuttur Aşağıdaki tablo geçerli kullanılabilir seçenekleri açıklar:

| Storefront seçeneği | Liste | Deneme/İşleme |  
| --- | --- | --- |  
| AppSource | Evet | Evet |
| Azure pazar yeri | Hayır | Evet |   

**Liste:**  Listeleme yayımlama seçeneği, Bir İletişim Beni teklif türünden oluşur ve Deneme veya İşlem düzeyinde bir katılım mümkün olmadığında kullanılır. Bu yaklaşımın yararı, pazarda bir çözüme sahip yayıncıların işinizi artırmak için fırsatlara dönüştürülebilecek müşteri adaylarını hemen almaya başlamalarını sağlamasıdır.  
**Deneme/İşlem:**  Müşteri, çözümünüz için doğrudan bir deneme sürümü satın alma veya deneme isteğinde bulunma seçeneğine sahiptir. Deneme deneyimi sağlamak, müşterilere sunulan etkileşim düzeyini artırır ve müşterilerin satın almadan önce çözümünüzü keşfetmesini sağlar. Deneme deneyimi ile, vitrinlerde daha fazla promosyon şansınız olacak ve müşteri katılımlarından daha fazla ve daha zengin müşteri adayları beklemelisiniz. Denemeler, en azından deneme süresi boyunca ücretsiz destek içermelidir.  

| SaaS Apps Teklif | İş Gereksinimleri | Teknik Gereksinimler |  
| --- | --- | --- |  
| **Bize Ulaşın** | Evet | Hayır |  
| **PowerBI / Dinamikler** | Evet | Evet (Azure AD tümleştirmesi) |  
| **SaaS Uygulamaları**| Evet | Evet (Azure AD tümleştirmesi) |     

## <a name="saas-list"></a>SaaS Listesi

Deneme sürümü ve faturalandırma işlevi olmayan bir SaaS girişi için eylem çağrısı "Bana Ulaşın." 

Bir SaaS uygulamasını listelemek için Azure Active Directory'yi yapılandırmanız gerekmez. 

|Gereksinimler  |Ayrıntılar  |
|---------|---------|
|Uygulamanız bir SaaS teklifidir  |   Çözümünüz bir SaaS teklifidir ve çok kiracılı bir SaaS ürünü sunarsınız.      |


## <a name="saas-trial"></a>SaaS Davası

Ücretsiz deneme, hizmet olarak yazılım (SaaS) tabanlı deneme sürümü kullanarak bir çözüm veya uygulama sağlarsınız. Ücretsiz deneme teklifleri sınırlı kullanımlı veya sınırlı süreli deneme hesabı olarak sunulabilir. 


|Gereksinimler  |Ayrıntılar  |
|---------|---------|
|Uygulamanız bir SaaS teklifidir  |   Çözümünüz bir SaaS teklifidir ve çok kiracılı bir SaaS ürünü sunarsınız.      |
|Uygulamanız AAD etkin     |   Müşteri etki alanınıza yeniden yönlendirilecek ve siz de müşteriyle doğrudan işlem yapacaksınız.       |


## <a name="saas-trial-technical-requirements"></a>SaaS Deneme Teknik gereksinimleri

SaaS uygulamaları için teknik gereksinimler basittir. Yayıncıların yayımlanmak üzere yalnızca Azure Etkin Dizini (Azure AD) ile tümleşik olması gerekir. Uygulamalarla Azure AD tümleştirmesi iyi belgelenmiştir ve Microsoft bunu gerçekleştirmek için birden çok SDK ve kaynak sağlar.  

Başlangıç olarak, Azure Marketi yayıncılığınız için özel bir aboneliğiniz olmasını öneririz ve bu da çalışmayı diğer girişimlerden yalıtmanıza olanak tanır. Bu işlem yapıldıktan sonra geliştirme çalışmasını başlatmak için SaaS uygulamanızı bu abonelikte dağıtmaya başlayabilirsiniz.  

En iyi Azure Etkin Dizin belgeleri, örnekleri ve kılavuzlar aşağıdaki sitelerde bulunur: 

* [Azure Active Directory Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

* [Azure Etkin Dizini ile Tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate)

* [Uygulamaları Azure Etkin Dizini ile Tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

* [Azure Yol Haritası - Güvenlik ve Kimlik](https://azure.microsoft.com/roadmap/?category=security-identity)

Video eğitimleri için aşağıdakileri gözden geçirin:

* [Vittorio Bertocci ile Azure Active Directory Kimlik Doğrulama](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration)

* [Azure Active Directory Identity Teknik Brifing - Bölüm 1 / 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration)

* [Azure Active Directory Identity Teknik Brifing - Bölüm 2 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration)

* [Microsoft Azure Active Directory ile Uygulama Oluşturma](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration)

* [Active Directory'ye odaklanan Microsoft Azure Videoları](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

Ücretsiz Azure Active Directory eğitimi ne kadar az  
* [BT Profesyonelleri İçerik Serisi için Microsoft Azure: Azure Active Directory](https://mva.microsoft.com/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

Buna ek olarak, Azure Etkin Dizini, Hizmet Güncelleştirmelerini denetlemek için bir site sağlar   
* [Azure AD Hizmeti güncelleştirmeleri](https://azure.microsoft.com/updates/?product=active-directory)

## <a name="using-azure-active-directory-to-enable-trials"></a>Denemeleri etkinleştirmek için Azure Etkin Dizini kullanma  

Microsoft, Azure AD ile tüm Market kullanıcılarının kimliğini doğrular, böylece kimlik doğrulaması yapılan bir kullanıcı Market'teki Deneme kaydınızı tıkladığında ve Deneme ortamınıza yönlendirildiğinde, kullanıcıyı ek bir oturum açma adımı gerektirmeden doğrudan Deneme Sürümü'ne sağlayabilirsiniz. Uygulamanızın kimlik doğrulama sırasında Azure AD'den aldığı belirteç, uygulamanızda bir kullanıcı hesabı oluşturmak için kullanabileceğiniz değerli kullanıcı bilgilerini içerir ve böylece sağlama deneyimini otomatikleştirebilmenizi ve dönüşüm olasılığını artırabilirsiniz. Belirteç hakkında daha fazla bilgi için [Örnek Belirteçler'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims) bakın.

Uygulamanızda veya Deneme'nizde 1 tıklamayla kimlik doğrulamasını etkinleştirmek için Azure AD'yi kullanmak aşağıdakileri yapar:  
* Pazardan Deneme'ye müşteri deneyimini kolaylaştırır.  
* Kullanıcı Market'ten etki alanınıza veya Deneme ortamınıza yönlendirildiğinde bile 'ürün içi deneyim' hissini korur.  
* Ek bir oturum açma adımı olmadığından, yeniden yönlendirmede terk edilme olasılığını azaltır.  
* Azure AD kullanıcılarının büyük nüfusu için dağıtım engellerini azaltır.  

## <a name="certifying-your-azure-ad-integration-for-marketplace"></a>Azure AD tümleştirmenizi Market için onaylama  

Azure AD tümleştirmenizi, uygulamanızın tek kiracılı mı yoksa çok kiracılı mı olduğuna ve Azure AD federe tek oturum açma (SSO) için yeni olup olmadığınıza veya zaten destekleyip desteklemediğine bağlı olarak birkaç farklı şekilde onaylayın.  

**Çok kiracılı uygulamalar için:**  

Azure AD'yi zaten destekliyorsanız aşağıdakileri yapın:
1.  Uygulamanızı Azure portalına kaydedin
2.  'Tek tıklamayla' deneme deneyimi elde etmek için Azure AD'deki çoklu kira desteği özelliğini etkinleştirin. Daha spesifik bilgileri [burada](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)bulabilirsiniz.  

Azure AD Federe SSO'da yeniyseniz, aşağıdakileri yapın: 
1.  Uygulamanızı Azure portalına kaydedin
2.  [OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) veya [OAuth 2.0'ı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code)kullanarak Azure AD ile SSO geliştirin.
3.  'Tek tıklamayla' deneme deneyimi elde etmek için AAD'de çoklu kira destek özelliğini etkinleştirin Daha spesifik bilgilere [buradan](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified)ulaşabilirsiniz.  

**Tek kiracılı uygulama için aşağıdaki seçeneklerden birini kullanın:**  
* [Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) kullanarak kullanıcıları dizininize konuk kullanıcı olarak ekleme
* 'Bana Ulaşın' kullanarak müşteriler için denemeleri el ile sağlama
* Müşteri başına 'Test Sürüşü' geliştirme
* SSO ile çok kiracılı örnek demo uygulaması oluşturun

## <a name="saas-subscriptions"></a>SaaS Abonelikleri

Müşterilerinizin SaaS tabanlı, teknik çözümünüzü abonelik olarak satın alabilmesi için SaaS uygulama teklif türünü kullanın. SaaS uygulamanız için aşağıdaki gereksinimlerin karşılanması gerekir:
- Hizmeti düz (aylık veya yıllık) veya kullanıcı başına bir oranda fiyatlandırın ve faturalandırın.
- Hizmeti herhangi bir zamanda yükseltmek veya iptal etmek için bir yöntem sağlayın.
Microsoft ticaret işlemini barındırıyor. Microsoft, müşterinizi sizin adınıza faturalar. Abonelik olarak bir SaaS Uygulaması sunmak için, SaaS karşılama API'leri ile tümleştirmeniz gerekir.  Hizmetiniz sağlama, yükseltme ve iptali desteklemelidir.

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
|Faturalama ve ölçüm | Teklifiniz, yayımlanmadan önce seçtiğiniz fiyatlandırma modeline (sabit fiyat veya kullanıcı başına) göre fiyatlandırılır.  Sabit oranlı modeli kullanıyorsanız, isteğe bağlı olarak, müşterileri sabit fiyata dahil olmayan kullanımiçin ücretlendirmek için kullanılan ek boyutlar ekleyebilirsiniz. |  
|İptal | Teklifiniz müşteri tarafından her zaman iptal edilebilir. |  
|İşlem açılış sayfası | Kullanıcıların SaaS hizmet hesaplarını oluşturup yönetebilecekleri bir Azure ortak markalı işlem açılış sayfası barındırıyorsunuz. |   
| Abonelik API'si | Bir kullanıcı hesabı ve hizmet planı oluşturmak, güncelleştirmek ve silmek için SaaS Aboneliği ile etkileşimkurabilen bir hizmeti ortaya çıkarırsınız. Kritik API değişiklikleri 24 saat içinde desteklenmelidir. Kritik olmayan API değişiklikleri periyodik olarak yayımlanacaktır. |  

>[!Note]
>Bulut Çözüm Sağlayıcıları (CSP) iş ortağı kanal tercihi artık kullanılabilir.  Microsoft CSP iş ortağı kanalları aracılığıyla teklifinizi pazarlama hakkında daha fazla bilgi için lütfen [Bulut Çözüm Sağlayıcıları'na](./cloud-solution-providers.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar
Eğer bunu yapmadıysanız,

- Pazara [kaydolun.](https://azuremarketplace.microsoft.com/sell)

Kayıtlıysanız ve yeni bir teklif oluşturuyorsanız veya varolan bir teklif üzerinde çalışıyorsanız,

- Teklifinizi oluşturmak veya tamamlamak için [Cloud İş Ortağı Portalı'nda oturum](https://cloudpartner.azure.com) açın.
- Daha fazla bilgi için [Azure SaaS uygulama teklifine](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer) bakın.
