---
title: Visual Studio App Center ve Azure hizmetleriyle mobil uygulama kullanımını ve Kullanıcı davranışını anlayın
description: Kullanıcıların mobil uygulamanızı nasıl kullandığını anlamak için akıllı iş kararları almanıza yardımcı olan App Center gibi hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 4dc3cea4497c55dda0d8da2ca29201615dadff19
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80241068"
---
# <a name="analyze-and-understand-mobile-application-use"></a>Mobil uygulama kullanımını analiz edin ve anlayın
Kullanıcılarınızın uygulamalarınızı nasıl kullandığını ne kadar iyi anlamış olursunuz? Uygulamanızda kaç tane etkin kullanıcı var ve kullanım zaman içinde nasıl değişiyor? Hangi özellikleri kullanıyor ve hangilerinin en çok kullanıldığı. Bu kullanıcılar nerede? Uygulamanın en son sürümünü kaç Kullanıcı kullanıyor? Bu soruların tümü, uygulamanızı başarılı bir işletmeye dönüştürmek için anlaşılması önemlidir. Bu tür kullanım analizi sorularını yanıtlamak için, uygulamalarınızdan kullanım verilerini toplamanız gerekir.

Verilere daha fazla baktığınız, uygulamanızı geliştirmenin ve kullanıcılarınızı memnun tutmanın yollarını da bulabilir. Eyleme dönüştürülebilir Öngörüler bulmak ve kullanıcıların karşılanmasını sağlamak için verilerin kullanılması önemlidir.

## <a name="importance-of-analytics"></a>Analiz önemi
- Kullanıcılarınızı, uygulamanızla nasıl etkileşime gireceğini, uygulamanızı daha iyi ayarlamaya ve işinizi büyütmeye yönelik harika deneyimler sağlamanıza göre anlayın.
- Uygulamanızı pazara sunma ve müşterilerinize daha iyi hale getirme hakkında bilinçli kararlar almak için kullanım ölçümlerini izleyin.
- Uygulama performansınızı ölçün.
- Uygulamanızın hangi bölümlerinin değer ve performans sağladığını öğrenin.
- Karmaşıklığa ve bekletmeye yönelik sorunlar hakkında veri odaklı Öngörüler elde edin.

Mobil uygulama analizlerini etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Analytics](/appcenter/analytics/) , önemli öneme odaklanarak hedef kitlenizi büyütmenize olanak tanır. Kullanıcı oturumları, en iyi cihazlar, işletim sistemi sürümleri ve davranış analizi hakkında ayrıntılı raporlama ve öngörüler sunar. Kapsamlı uygulama analiziyle her şeyi izlemek için kolayca özel olaylar oluşturun.

   **Önemli özellikler**
   - Kullanım düzenlerini, Kullanıcı benimsemeyi ve diğer katılım ölçümlerini ücretsiz izleyin.
   - Özel olaylar aracılığıyla eğilimleri, Kullanıcı davranışını ve katılımı belirler.
   - Tek bir panodaki uygulama kullanımı (günlük, haftalık, aylık), oturum, cihaz özellikleri ve Kullanıcı demografları hakkında kullanıma hazır ölçümler ve ayrıntılı Öngörüler edinin.
   - Sınırsız saklama için App Center Analytics verilerinizi sürekli olarak Azure 'a aktarın. App Center Analytics, Azure Blob depolama ve Azure Application Insights dışarı aktarmayı destekler.
   - Bekletme, huni Analizi ve cohortalar gibi daha ayrıntılı bilgiler için Azure Application Insights ile tümleştirin.
   - Dakikalar içinde başlamak için tek satırlık SDK tümleştirmesi kullanın.
   - İOS, Android, macOS, tvOS, Xamarin, bir yerel, Unity ve Cordova için platform desteği kazanın.

   **Başvurular**
   - [App Center kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
   - [App Center Analytics ile çalışmaya başlama](/appcenter/analytics/)

## <a name="azure-monitor"></a>Azure İzleyici
Azure Izleyici, performansı en üst düzeye çıkarmak ve mobil uygulamanızı izlemek için telemetri toplayıp analiz etmeye yönelik araçlar sağlayan [Application Insights](/azure/azure-monitor/app/app-insights-overview)içerir. Application Insights dışarı aktarmayı ayarlamak için App Center Analytics kullanarak Application Insights avantajlarından yararlanabilirsiniz. Application Insights, App Center sağladığı analiz araçlarının ötesinde, uygulamalarınızdan özel olay telemetrisini sorgulayabilir, kaydedebilir, filtreleyebilir ve analiz edebilir.

**Önemli özellikler**
   - Özel olay telemetrinizi sorgulayın.
   - Güçlü segmentasyon özelliklerine sahip olay telemetrisini filtreleyin.
   - Uygulamanızda dönüştürme, bekletme ve gezinme düzenlerini çözümleyin. Şunu kullanabilirsiniz:
     - İşlevler, dönüştürme oranlarını analiz etmek ve izlemek için.
     - Uygulamanızın kullanıcıları zaman içinde ne kadar iyi koruduğunu analiz etmek için bekletme.
     - Görselleştirmeleri ve metinleri paylaşılabilir bir raporda birleştiren çalışma kitapları.
     - Diğer analiz araçlarından kolayca başvurulabilmeleri için belirli kullanıcı veya olay gruplarını adlandırmak ve kaydetmek üzere cohorts

**Başvurular**
- [Azure portal](https://portal.azure.com/)
- [App Center ve Application Insights mobil uygulamanızı çözümleyin](/azure/azure-monitor/learn/mobile-center-quickstart)
- [Application Insights ile App Center Analytics kullanma](/azure/azure-monitor/app/usage-overview)

## <a name="azure-playfab"></a>Azure PlayFab
[Azure PlayFab](https://playfab.com/) , dünya çapındaki bulut bağlantılı Oyunlar oluşturmanız gereken oyun Hizmetleri, gerçek zamanlı analiz ve LiveOps ile tam bir arka uç platformu sunar. Bu hizmetler oyun geliştiricileri için başlatma sınırlamalarını azaltır. Bunlar, oyunlarıyla ölçeklenebilen büyük ve küçük Studios maliyetli geliştirme çözümleri sunar. Hizmetler, oyunculara Studios, devam etmenize ve yeniden çekmeye yardımcı olabilir. PlayFab sayesinde geliştiriciler, oyunları oluşturmak ve çalıştırmak, oyun verilerini çözümlemek ve genel oyun deneyimlerini geliştirmek için akıllı bulutu kullanabilir.

**Önemli özellikler**
   - Gerçek zamanlı panoları izleyin.
   - En üst ölçümler aracılığıyla oyununuzun performansını değerlendirin.
   - Otomatik olarak oluşan raporlar sayesinde oyununuzun özetlerini günlük ve aylık performanslarını gözden geçirin. Raporları Game Manager 'da görüntüleyebilir ve bunların günlük olarak gelen kutunuza yüklenmesini veya teslim edilmesini sağlayabilirsiniz.
   - Denemeleri çalıştırmak ve belirli bir değişken için en iyi ayarı öğrenmek üzere bir/B testi kullanın.
   - Otomatik oyuncu gruplandırmaları tanımlamak için oyuncuların segmentlemesini kullanın.
    
**Başvurular**
- [PlayFab portalı](https://developer.playfab.com/en-US/sign-up)
- [Analiz](/gaming/playfab/#pivot=documentation&panel=analytics)
- [Hızlı Başlangıçlar](/gaming/playfab/#pivot=documentation&panel=quickstarts) 
