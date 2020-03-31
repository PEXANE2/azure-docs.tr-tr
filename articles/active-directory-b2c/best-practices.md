---
title: Azure AD B2C için en iyi uygulamalar
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C (Azure AD B2C) ile çalışırken göz önünde bulundurulması gereken öneriler ve en iyi uygulamalar.
services: active-directory-b2c
author: vigunase
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: vigunase
ms.subservice: B2C
ms.openlocfilehash: a76852a6e3cc5ffcdfcac62ce29fe47c97af3df1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136168"
---
# <a name="recommendations-and-best-practices-for-azure-active-directory-b2c"></a>Azure Active Directory B2C için öneriler ve en iyi uygulamalar

Aşağıdaki en iyi uygulamalar ve öneriler, Azure Active Directory (Azure AD) B2C'yi varolan veya yeni uygulama ortamlarına entegre etmenin bazı temel yönlerini kapsar.

## <a name="fundamentals"></a>Temel Konular

|  |  |
|--|--|
| Çoğu senaryo için kullanıcı akışlarını seçme | Azure AD B2C'nin Kimlik Deneyimi Çerçevesi, hizmetin temel gücüdür. İlkeler, kaydolma, kaydolma veya profil düzenleme gibi kimlik deneyimlerini tam olarak açıklar. Azure AD B2C portalı, en yaygın kimlik görevlerini ayarlamanıza yardımcı olmak için kullanıcı akışları adı verilen önceden tanımlanmış, yapılandırılabilir ilkeler içerir. Kullanıcı akışları ile, sadece birkaç tıklama ile, dakika içinde büyük kullanıcı deneyimleri oluşturabilirsiniz. [Kullanıcı akışları ve özel ilkeler ne zaman kullanılacağını öğrenin.](custom-policy-overview.md#comparing-user-flows-and-custom-policies)|
| Uygulama kayıtları | Güvenli hale gelen her uygulama (web, yerel) ve API Azure AD B2C'ye kaydedilmelidir. Bir uygulamada iOS ve Android'in hem web hem de yerel sürümü varsa, bunları aynı istemci kimliğine sahip Azure AD B2C'de tek bir uygulama olarak kaydedebilirsiniz. [OIDC, SAML, web ve yerel uygulamaları](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-register-applications?tabs=applications)nasıl kaydedebilirsiniz öğrenin. [Azure AD B2C'de kullanılabilecek uygulama türleri](https://docs.microsoft.com/azure/active-directory-b2c/application-types)hakkında daha fazla bilgi edinin. |
| Aylık etkin kullanıcı faturalandırmasına geçme | Azure AD B2C, aylık etkin kimlik doğrulamalarından aylık etkin kullanıcılara (MAU) faturalandırmaya geçti. Çoğu müşteri bu modeli uygun maliyetli bulur. [Aylık etkin kullanıcıların faturalandırması hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/updates/mau-billing/) |

## <a name="planning-and-design"></a>Planlama ve tasarım

Uygulama nızı ve hizmet mimarinizi, geçerli sistemleri envanterinizi tanımlayın ve Azure AD B2C'ye geçişinizi planlayın.

|  |  |
|--|--|
| Uçtan uca bir çözüm mimar | Azure AD B2C tümleştirmesini planlarken tüm uygulama bağımlılıklarınızın bağımlılıklarını ekleyin. Şu anda ortamınızda olan veya Azure İşlevleri, müşteri ilişkileri yönetimi (CRM) sistemleri, Azure API Yönetimi ağ geçidi ve depolama hizmetleri gibi çözüme eklenmesi gereken tüm hizmetleri ve ürünleri göz önünde bulundurun. Tüm hizmetlerin güvenliğini ve ölçeklenebilirliğini göz önünde bulundurun. |
| Kullanıcılarınızın deneyimlerini belgeleme | Müşterilerinizin uygulamanızda yaşayabileceği tüm kullanıcı yolculuklarını ayrıntılı olarak inceleyin. Uygulamanızın kimlik ve profil yönleriyle etkileşimde bulunduklarında karşılaşabilecekleri her ekranı ve dallanma akışlarını ekleyin. Planlamanıza kullanılabilirlik, erişilebilirlik ve yerelleştirme ekleyin. |
| Doğru kimlik doğrulama protokolünü seçin |  Farklı uygulama senaryolarının ve önerilen kimlik doğrulama akışlarının dökümü için [Senaryolar ve desteklenen kimlik doğrulama akışlarına](../active-directory/develop/authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)bakın. |
| Pilot bir kavram kanıtı (POC) uçtan uca kullanıcı deneyimi | [Microsoft kod örneklerimiz](code-samples.md) ve topluluk [örneklerimizle](https://github.com/azure-ad-b2c/samples)başlayın. |
| Geçiş planı oluşturma |İleriyi planlamak, geçişin daha sorunsuz geçmesini sağlayabilir. [Kullanıcı geçişi](user-migration.md)hakkında daha fazla bilgi edinin.|
| Kullanılabilirlik ve güvenlik | Çözümünüz, uygulama kullanılabilirliği ile kuruluşunuzun kabul edilebilir risk düzeyi arasında doğru dengeyi sağlanmalıdır. |
| Şirket içi bağımlılıkları buluta taşıma | Esnek bir çözüm sağlamaya yardımcı olmak için, varolan uygulama bağımlılıklarını buluta taşımayı düşünün. |
| Varolan uygulamaları b2clogin.com geçirin | login.microsoftonline.com amortismanı 04 Aralık 2020 tarihinde tüm Azure AD B2C kiracıları için yürürlüğe girmiştir. [Daha fazla bilgi edinin](b2clogin.md). |

## <a name="implementation"></a>Uygulama

Uygulama aşamasında, aşağıdaki önerileri göz önünde bulundurun.

|  |  |
|--|--|
| Visual Studio Code için Azure AD B2C uzantısı ile özel ilkeleri edin | Visual Studio Code'u ve Visual Studio Code Marketplace'ten bu topluluk tarafından oluşturulmuş [uzantıyı indirin.](https://marketplace.visualstudio.com/items?itemName=AzureADB2CTools.aadb2c) Resmi bir Microsoft ürünü olmasa da, Visual Studio Code için Azure AD B2C uzantısı, özel ilkelerle çalışmayı kolaylaştıran çeşitli özellikler içerir. |
| Azure AD B2C sorun giderme yi öğrenin | Geliştirme sırasında özel ilkeleri nasıl [gidereceklerini](https://docs.microsoft.com/azure/active-directory-b2c/troubleshoot-custom-policies?tabs=applications) öğrenin. Normal bir kimlik doğrulama akışının nasıl göründüğünü öğrenin ve anormallikleri ve hataları keşfetmek için araçlar kullanın. Örneğin, kullanıcı yolculuklarının çıktı günlüklerini incelemek için [Application Insights'ı](troubleshoot-with-application-insights.md) kullanın. |
| Kanıtlanmış özel politika desenlerinden oluşan kütüphanemizden yararlanın | Birkaç gelişmiş Azure AD B2C müşteri kimliği ve erişim yönetimi (CIAM) kullanıcı yolculukları için [örnekler](https://github.com/azure-ad-b2c/samples) bulun. |


## <a name="testing"></a>Sınama

Azure AD B2C uygulamanızı test edin ve otomatikleştirin.

|  |  |
|--|--|
| Küresel trafiği hesaba katın | Performans ve yerelleştirme gereksinimlerini test etmek için farklı genel adresten gelen trafik kaynaklarını kullanın. Tüm HTM'lerin, CSS'lerin ve bağımlılıkların performans gereksinimlerinizi karşıladığından emin olun. |
| İşlevsel ve UI testi | Kullanıcının uçuça akmasını test edin. Selenyum, VS Web Testi, vb kullanarak birkaç dakikada bir sentetik testler ekleyin. |
| Kalem testi | Çözümünüzle canlı yayına geçmeden önce, üçüncü taraf bağımlılıkları da dahil olmak üzere tüm bileşenlerin güvenli olduğunu doğrulamak için penetrasyon testi alıştırmaları gerçekleştirin. API'lerinizi erişim belirteçleriyle güvence altına aldığınızı ve uygulama senaryonuz için doğru kimlik doğrulama protokolünü kullandığınızı doğrulayın. [Penetrasyon testi](https://docs.microsoft.com/azure/security/fundamentals/pen-testing) ve [Microsoft Cloud Birleşik Penetrasyon Test Kuralları](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)hakkında daha fazla bilgi edinin. |
| A/B Testi | Tüm popülasyonunuza başlamadan önce yeni özelliklerinizi küçük, rastgele bir kullanıcı setiile uçun. Azure AD B2C'de JavaScript etkinleştirildiğinde, Optimize Edilebilir, Netlik ve diğerleri gibi A/B test araçlarıyla tümleştirebilirsiniz. |
| Yük test etme | Azure AD B2C ölçeklendirilebilir, ancak uygulamanız yalnızca tüm bağımlılıkları ölçeklendirebiliyorsa ölçeklendirilebilir. API'lerinizi ve CDN'lerinizi yükleyin. |
| Azaltma |  Azure AD B2C, kısa bir süre içinde aynı kaynaktan çok fazla istek gönderilirse trafiği daraltır. Yük testi sırasında birkaç trafik kaynağı `AADB2C90229` kullanın ve uygulamalarınızda hata kodunu incelikle işlayın. |
| Automation | [Azure DevOps](deploy-custom-policies-devops.md)gibi sınama ve dağıtımları otomatikleştirmek için sürekli tümleştirme ve teslim (CI/CD) ardışık hatlarını kullanın. |

## <a name="operations"></a>İşlemler

Azure AD B2C ortamınızı yönetin.

|  |  |
|--|--|
| Birden çok ortam oluşturma | Daha kolay işlemler ve dağıtım kullanıma sunulması için geliştirme, test, üretim öncesi ve üretim için ayrı ortamlar oluşturun. Her biri için Azure AD B2C kiracıları oluşturun. |
| Özel ilkeleriniz için sürüm denetimini kullanma | Azure AD B2C özel ilkeleriniz için GitHub, Azure Repos veya bulut tabanlı başka bir sürüm kontrol sistemi kullanmayı düşünün. |
| B2C kiracılarınızın yönetimini otomatikleştirmek için Microsoft Graph API'sini kullanın | Microsoft Grafik API'leri:<br/>[Kimlik Deneyimi Çerçeveyi](https://docs.microsoft.com/graph/api/resources/trustframeworkpolicy?view=graph-rest-beta) Yönetme (özel ilkeler)<br/>[Anahtarlar](https://docs.microsoft.com/graph/api/resources/trustframeworkkeyset?view=graph-rest-beta)<br/>[Kullanıcı Akışları](https://docs.microsoft.com/graph/api/resources/identityuserflow?view=graph-rest-beta) |
| Azure DevOps ile tümleştirme | [CI/CD ardışık hatlar,](deploy-custom-policies-devops.md) kodu farklı ortamlar arasında taşımayı kolaylaştırır ve her zaman üretime hazır olmasını sağlar.   |
| Azure Monitör ile tümleştirme | [Denetim günlüğü olayları](view-audit-logs.md) yalnızca yedi gün saklanır. Günlükleri uzun süreli kullanım için korumak için [Azure Monitor ile tümleştirin](azure-monitor.md) veya ortamınız hakkında bilgi edinmek için üçüncü taraf güvenlik bilgileri ve olay yönetimi (SIEM) araçlarıyla tümleştirin. |
| Kurulum etkin uyarı ve izleme | Uygulama Öngörüleri'ni kullanarak Azure AD B2C'deki [kullanıcı davranışını izleyin.](active-directory-b2c-custom-guide-eventlogger-appins.md) |


## <a name="support-and-status-updates"></a>Destek ve Durum Güncellemeleri

Hizmetin durumu hakkında güncel kalın ve destek seçeneklerini bulun.

|  |  |
|--|--|
| [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=active-directory-b2c) |  Azure AD B2C ürün güncellemeleri ve duyuruları ile güncel kalın. |
| [Microsoft Destek](support-options.md) | Azure AD B2C teknik sorunları için bir destek isteği dosyala. Faturalandırma ve abonelik yönetimi desteği ücretsiz olarak sağlanır. |
| [Azure durumu](https://status.azure.com/status) | Tüm Azure hizmetlerinin geçerli sistem durumu durumunu görüntüleyin. |