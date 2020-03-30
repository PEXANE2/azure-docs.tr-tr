---
title: Barındırılan Test Sürüşü | Azure Marketi
description: Pazar Yeri Barındırılan Test Sürüşü nasıl kurulamaz?
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 6d054064e236e121e02bf58a0eb73b5a62f24a09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278272"
---
# <a name="hosted-test-drive"></a>Barındırılan Test Sürüşü

Barındırılan Test Sürüşü, Microsoft barındırma nın kurulum karmaşıklığını kaldırır ve Test Sürücüsü kullanıcı sağlama ve düzenlemeyi gerçekleştiren hizmeti korur. Bu makale, AppSource'da teklif veren veya yeni bir teklif oluşturan ve Müşteri Katılımı için Dynamics 365, Finans ve Operasyonlar için Dynamics 365 veya Dynamics 365 Business Central ile bağlantı kuran Barındırılan Test Sürüşü sunmak isteyen Yayıncılar içindir. Örnek.

## <a name="how-to-publish-a-test-drive"></a>Test Sürüşü nasıl yayımlanır?

Varolan teklife gidin veya yeni bir teklif oluşturun.

Yan menüden Test Sürüşü seçeneğini seçin.

Test \'\' Sürüşünü \'\' Etkinleştir seçeneği için Evet'i seçin.

\'Ayrıntılar\' bölümünde aşağıdaki alanları sağlayın.

- **Açıklama**: Test Sürüşünüzün genel görünümünü sağlayın. Bu metin, Test Sürüşü verilirken kullanıcıya gösterilir. Biçimlendirilmiş içerik sağlamak istiyorsanız, bu alan HTML'yi destekler.
- **Kullanım Kılavuzu**: Test Sürüşü kullanıcılarının Uygulamanızı nasıl kullanacaklarını anlamalarına yardımcı olan ayrıntılı bir kullanım kılavuzu (.pdf türü dosyası) yükleyin.
- **Test Sürüşü Demo Videosu**: İsteğe bağlı olarak Uygulamanızı gösteren bir video yükleyin.

[Burada](https://github.com/Microsoft/AppSource/blob/patch-1/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md)bulunan talimatları kullanarak kiracınızdaki Test Sürüşü kullanıcılarını sağlama ve yok etme için AppSource izni ver.

Bu adımda, aşağıda \'belirtilen Azure\' AD \'Uygulama Kimliği\' ve Azure AD Uygulama Anahtarı değerlerini oluşturursunuz.

\'Teknik Yapılandırma\' bölümünde aşağıdaki alanları sağlayın:

- **Test Sürüşü**Türü \': Microsoft Barındırılan 'ı seçin (Müşteri Etkileşimi için örnek Dynamics 365)' seçeneğini belirleyin. Bu, Microsoft'un Test Sürüşü kullanıcı sağlama ve deprovisioning gerçekleştiren hizmeti barındıracağını ve sürdüreceğini gösterir.
- **Maksimum Eşzamanlı Test Sürücüleri**: Bu alanı, belirli bir zamanda etkin bir Test Sürüşüne sahip olabilecek eşzamanlı kullanıcı sayısına ayarlayın. Her kullanıcı, Test Sürüşü etkinken bir Dynamics lisansı tüketir, bu nedenle Test Sürüşü kullanıcıları için en azından bu kadar Dynamics lisansı na sahip olmanız gerekir. Önerilen değer 3-5.
- **Test Sürüşsüresi (saat)**: Bu alanı, test sürüşünün etkin olacağı saat sayısına ayarlayın. Bu kadar saatten sonra, kullanıcı kiracınızdan indirilir. Uygulamanızın karmaşıklığına bağlı olarak 2-24 saat önerilir. Kullanıcı, zamanı biterse ve Test Sürüşüne yeniden erişmek isterse her zaman başka bir Test Sürüşü isteyebilir.
- **Örnek URL:** Test Sürüşü kullanıcısının Test Sürüşü'ne başladıklarında başlangıçta gezineceği bir URL sağlayın. Bu genellikle, Uygulamanızın ve örnek verilerinizin üzerine yüklenmiş olan Dynamics 365 örneğinizin URL'sidir. Örnek Değer:\/https: /testdrive.crm.dynamics.com
- **Azure AD Kiracı Kimliği**: Dynamics 365 Örneğiniz için Azure Kiracısının kimliğini sağlayın. Bu değeri almak için Azure portalına \'giriş yapın\'  - \> ve menü den\> Azure Active Directory Select Properties 'e gidin - Dizin Kimliğini kopyalayın. Örnek değer: 72f988bf-86f1-41af-91ab-2d7cd0111234
- **Azure AD Uygulama Kimliği**: Adım 7'de oluşturduğunuz Azure AD Uygulamasının kimliği.\ Örnek Değer: 53852862-a2ae-4e43-9461-faa49650a096
- **Azure AD Uygulama Anahtarı**: Adım 7'de oluşturulan Azure AD Uygulaması için gizli.\ Örnek Değer: IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=
- **Azure AD Kiracı Adı**: Dynamics 365 Örneğiniz için Azure Kiracısının adını sağlayın. \<Kiracı adının biçimini kullanın. \>onmicrosoft.com. Örnek Değer: testdrive.onmicrosoft.com
- **Örnek Web API URL'si**: Dynamics 365 Örneğiniz için Web API URL'sini sağlayın. Bu değeri, Microsoft Dynamics 365 örneğinize giriş yaparak ve\> Ayar\> - Özelleştirme\> - Geliştirici Kaynakları - Örnek Web API'sine (Bu URL'yi kopyalayarak) yönlendirerek alabilirsiniz. Örnek değer:\/https: /testdrive.crm.dynamics.com/api/data/v9.0
- **Rol Adı**: Test Sürüşü için oluşturduğunuz özel Dynamics 365 Güvenlik Rolü'nün adını sağlayın. Bu, Test Sürüşü sırasında kullanıcılara atanacak roldür. Örnek Değer: testdriverole

## <a name="next-steps"></a>Sonraki adımlar

Teklifinizi **yayınlamaya** hazır olduğunuzda, uygulamanız sertifikayı geçtikten sonra, teklifinizin bir **önizlemesini** alırsınız. UI'de bir Test Sürüşü başlatın ve Test Sürücülerinizin doğru çalıştığını doğrulayın. Bir kez önizleme sunan rahat hissediyorum, şimdi **canlı gitmek zamanı!**
