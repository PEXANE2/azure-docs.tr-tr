---
title: Microsoft Azure'da güvenli uygulamaları dağıtma
description: Bu makalede, web uygulama projenizin sürüm ve yanıt aşamalarında göz önünde bulundurulması gereken en iyi uygulamalar tartışılmaktadır.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/12/2019
ms.topic: article
ms.service: security
ms.subservice: security-develop
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: dfe4f09d00a5629249a3041946190f56e83c3480
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68934881"
---
# <a name="deploy-secure-applications-on-azure"></a>Azure'da güvenli uygulamaları dağıtma
Bu makalede, bulut için uygulamaları dağıtırken göz önünde bulundurulması gereken güvenlik etkinlikleri ve denetimleri sıyoruz. Microsoft [Güvenlik Geliştirme Yaşam Döngüsü'nün (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) sürüm ve yanıt aşamalarında göz önünde bulundurulması gereken güvenlik soruları ve kavramları ele alınır. Amaç, daha güvenli bir uygulama dağıtmak için kullanabileceğiniz etkinlikleri ve Azure hizmetlerini tanımlamanıza yardımcı olmaktır.

Aşağıdaki SDL aşamaları bu makalede ele alınmıştır:

- Yayınla
- Yanıt

## <a name="release"></a>Yayınla
Yayın aşamasının odak noktası, kamu yada bir proje hazırlamaktır.
Bu, sürüm sonrası hizmet görevlerini etkili bir şekilde gerçekleştirmenin ve daha sonra oluşabilecek güvenlik açıklarını gidermenin planlama yollarını içerir.

### <a name="check-your-applications-performance-before-you-launch"></a>Başlatmadan önce uygulamanızın performansını kontrol edin

Uygulamanızı başlatmadan veya güncelleştirmeleri üretime dağıtmadan önce uygulamanızın performansını denetleyin. Uygulamanızdaki performans sorunlarını bulmak, dağıtım kalitesini artırmak, uygulamanızın her zaman açık veya kullanılabilir olduğundan ve uygulamanızın lansmanınız için trafiği kaldırabileceğinden emin olmak için Visual Studio'yu kullanarak bulut tabanlı [yük testleri](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) çalıştırın.

### <a name="install-a-web-application-firewall"></a>Web uygulaması güvenlik duvarı yükleme

Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklar arasında yaygın sql enjeksiyon saldırıları ve çapraz site komut dosyası saldırıları vardır. Uygulama kodunda bu saldırıları önlemek zor olabilir. Uygulama topolojisinin birçok katmanında sıkı bakım, yama ve izleme gerektirebilir. Merkezi waf, güvenlik yönetimini kolaylaştırmanıza yardımcı olur. WAF çözümü, bilinen bir güvenlik açığını merkezi bir konumda yamalayarak ve her bir web uygulamasını güvence altına alarak bir güvenlik tehdidine de tepki verebilir.

[Azure Application Gateway WAF,](../../application-gateway/waf-overview.md) web uygulamalarınızın yaygın açıklardan ve güvenlik açıklarına karşı merkezi koruma sağlar. WAF, [OWASP temel kuralının](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 veya 2.2.9 belirlediği kurallara dayanmaktadır.

### <a name="create-an-incident-response-plan"></a>Bir olay yanıtı planı oluşturma


Bir olay yanıt planı hazırlamak, zaman içinde ortaya çıkabilecek yeni tehditleri ele almanıza yardımcı olmak için çok önemlidir. Olay yanıt planının hazırlanması, uygun güvenlik acil durum bağlantılarını tanımlamayı ve kuruluştaki diğer gruplardan ve lisanslı üçüncü taraf kodundan devralınan kod için güvenlik servis planlarını oluşturmayı içerir.

### <a name="conduct-a-final-security-review"></a>Son bir güvenlik incelemesi gerçekleştirin

Gerçekleştirilen tüm güvenlik etkinliklerini kasıtlı olarak gözden geçirmek, yazılım sürümünüzün veya uygulamanızın hazır olmasını sağlamaya yardımcı olur. Son güvenlik incelemesi (FSR), genellikle gereksinim aşamasında tanımlanan kalite kapıları ve hata çubuklarına karşı tehdit modellerinin, araç çıktılarının ve performansın incelenmesini içerir.

### <a name="certify-release-and-archive"></a>Serbest bırakma ve arşivleme sertifikası

Yazılımı bir sürümden önce belgelemek, güvenlik ve gizlilik gereksinimlerinin karşılanmasını sağlamaya yardımcı olur. Tüm ilgili verilerin arşivlenmesi, sürüm sonrası hizmet görevlerini gerçekleştirmek için çok önemlidir. Arşivleme, sürdürülebilir yazılım mühendisliğiyle ilişkili uzun vadeli maliyetlerin azaltılmasına da yardımcı olur.

## <a name="response"></a>Yanıt
Yanıt, geliştirme ekibinin ortaya çıkan yazılım tehditleri ve güvenlik açıkları hakkındaki raporlara uygun şekilde yanıt verebilme lerini merkez alır.

### <a name="execute-the-incident-response-plan"></a>Olay yanıt planını yürütme

Sürüm aşamasında uygulanan olay yanıt planını uygulayabilmek, müşterilerin yazılım güvenliğinden veya ortaya çıkan gizlilik açıklarından korunmasına yardımcı olmak için önemlidir.

### <a name="monitor-application-performance"></a>Uygulama performansını izleme

Uygulamanızın dağıtıldıktan sonra sürekli izlenmesi, performans sorunlarının yanı sıra güvenlik açıklarını da algılamanıza yardımcı olabilir.
Uygulama izlemeye yardımcı olan Azure hizmetleri şunlardır:

  - Azure Application Insights
  - Azure Güvenlik Merkezi

#### <a name="application-insights"></a>Application Insights

[Application Insights,](../../azure-monitor/app/app-insights-overview.md) web geliştiricileri için birden çok platformda genişletilebilir bir Uygulama Performans Yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için kullanabilirsiniz. Uygulama Öngörüleri performans anormalliklerini otomatik olarak algılar. Sorunları tanılamanıza ve kullanıcıların uygulamanızla gerçekte ne yaptığını anlamanıza yardımcı olacak güçlü analiz araçları içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır.

#### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi,](../../security-center/security-center-intro.md) web uygulamaları da dahil olmak üzere Azure kaynaklarınızın güvenliğini artırarak (ve üzerinde denetimle) tehditleri önlemenize, algılamanıza ve bunlara yanıt vermenize yardımcı olur. Azure Güvenlik Merkezi, aksi takdirde fark edilmeyecek tehditleri algılamaya yardımcı olur. Çeşitli güvenlik çözümleri ile çalışır.

Security Center'ın Ücretsiz katmanı yalnızca Azure kaynaklarınız için sınırlı güvenlik sağlar. [Güvenlik Merkezi Standart katmanı,](../../security-center/security-center-onboarding.md) bu yetenekleri şirket içi kaynaklara ve diğer bulutlara genişletir.
Güvenlik Merkezi Standardı size yardımcı olur:

  - Güvenlik açıklarını bulun ve düzeltin.
  - Kötü amaçlı etkinliği engellemek için erişim ve uygulama denetimleri uygulayın.
  - Analitik ve istihbarat kullanarak tehditleri algılayın.
  - Saldırı altındayken hızlı bir şekilde yanıt verin.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, güvenli uygulamalar tasarlamanıza ve geliştirmenize yardımcı olabilecek güvenlik denetimleri ve etkinlikleri öneririz.

- [Güvenli uygulamalar tasarla](secure-design.md)
- [Güvenli uygulamalar geliştirme](secure-develop.md)
