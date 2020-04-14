---
title: 'Genel Bakış: Azure Zaman Serisi Öngörüleri Nedir? - Azure Zaman Serisi Öngörüleri | Microsoft Dokümanlar'
description: Zaman serisi verilerinin analizi ve IoT çözümleri için yeni bir hizmet olan Azure Time Series Insights'a giriş.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: overview
ms.date: 04/13/2020
ms.custom: seodec18
ms.openlocfilehash: 59149b2ca598104d8aca9b4e5e60194a8f6398bf
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81269967"
---
# <a name="what-is-azure-time-series-insights"></a>Azure Time Series Insights nedir?

Azure Zaman Serisi Öngörüleri, IoT aygıtları tarafından oluşturulan gibi büyük miktarda zaman serisi verilerini depolamak, görselleştirmek ve sorgulamak için oluşturulmuştur. Bulutta zaman serisi verilerini depolamak, yönetmek, sorgulamak veya görselleştirmek istiyorsanız Time Series Insights aradığınız çözüm olabilir.

[![Time Series Insights akış çizelgesi](media/overview/time-series-insights-flowchart.png)](media/overview/time-series-insights-flowchart.png#lightbox)

Time Series Insights dört temel işe sahiptir:

- Azure IoT Hub ve Azure Etkinlik Hub'ları gibi bulut ağ geçitleriyle tamamen entegre edilmiştir. Bu olay kaynaklarına kolayca bağlanır ve JSON kodu içindeki veri içeren iletilerle yapıları ayrıştırarak temiz satırlara ve sütunlara uygular. Meta verileri telemetri verileriyle birleştirerek verilerinizi sütunlu bir depoda dizinler.
- Time Series Insights verilerinizin depolanmasını yönetir. Verilere her zaman kolayca erişilebildiğinden emin olmak için verilerinizi bellekte ve SSD'lerde 400 güne kadar saklar. İsteğe bağlı olarak milyarlarca olayı saniyeler içinde etkileşimli olarak sorgulayabilirsiniz.
- Time Series Insights, Time Series Insights explorer aracılığıyla kutudan çıkma görselleştirme sağlar.
- Time Series Insights, hem Time Series Insights gezgininde hem de zaman serisi verilerinizi özel uygulamalara yerleştirmek için tümleştirmesi kolay API'ler kullanarak bir sorgu hizmeti sağlar.

İç tüketim veya harici müşterilerin kullanması için bir uygulama oluşturursanız, Time Series Insights'ı arka uç olarak kullanabilirsiniz. Zaman serileri verilerini dizilemek, depolamak ve toplamak için kullanabilirsiniz. Üstte özel bir görselleştirme ve kullanıcı deneyimi oluşturmak için [İstemci SDK'yı](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)kullanın. Zaman Serisi Öngörüleri, bu özelleştirilmiş senaryoları etkinleştirmek için çeşitli [Sorgu API'leri](how-to-shape-query-json.md) ile de donatılmıştır.

Zaman serisi verileri bir varlığın veya işlemin zaman içindeki değişimini gösterir. Zaman serisi verileri zaman damgalarına göre dizilenir ve zaman, bu tür verilerin düzenlendiği en anlamlı eksendir. Zaman serisi verileri genellikle sıralı sırayla geldiğinden, veritabanınıza bir güncelleştirme yerine bir kesici uç olarak kabul edilir.

Zaman serileri verilerini büyük hacimlerde depolamak, dizilemek, sorgulamak, analiz etmek ve görselleştirmek zor olabilir.
Azure Time Series Öngörüleri her yeni olayı bir satır olarak yakalar ve saklar ve değişim zaman içinde verimli bir şekilde ölçülür. Sonuç olarak, gelecekteki değişimi tahmin etmeye yardımcı olmak için geçmişten öngörüler çizmek için geriye bakabilirsiniz.

## <a name="video"></a>Video

### <a name="learn-more-about-azure-time-series-insights-the-cloud-based-iot-analytics-platformbr"></a>Bulut tabanlı IoT analiz platformu Azure Time Series Öngörüleri hakkında daha fazla bilgi edinin.</br>

[![Video](https://img.youtube.com/vi/GaARrFfjoss/0.jpg)](https://www.youtube.com/watch?v=GaARrFfjoss)

## <a name="primary-scenarios"></a>Birincil senaryolar

- Zaman serisi verilerini ölçeklenebilir bir şekilde depolayın.

   Time Series Insights'ın temelinde zaman serisi verilerine göre tasarlanmış bir veritabanı bulunur. Ölçeklenebilir ve tam olarak yönetilebilen bir kaynak olduğundan, Time Series Insights olayları depolama ve yönetme işini yönetir.

- Verileri neredeyse gerçek zamanlı olarak keşfedin.

   Zaman Serisi Öngörüleri, bir ortama akan tüm verileri görselleştiren bir gezgin sağlar. Bir olay kaynağına bağlandıktan kısa bir süre sonra, Etkinlik verilerini Time Series Öngörüleri içinde görüntüleyebilir, keşfedebilir ve sorgulayabilirsiniz. Veriler, bir aygıtın beklendiği gibi veri yayıp yayacağını doğrulamanıza ve sistem durumu, üretkenlik ve genel etkinlik için bir IoT varlığını izlemenize yardımcı olur.

- Kök-neden analizi ni yapın ve anomalileri tespit edin.

   Time Series Insights, çok aşamalı kök neden çözümlemesi yapmak ve kaydetmek için desenler ve perspektif görünümleri gibi araçlara sahiptir. Time Series Insights, Zaman Serisi Öngörüleri gezgininde uyarıları ve algılanan anormallikleri neredeyse gerçek zamanlı olarak görüntüleyebilmeniz için Azure Akış Analizi gibi uyarı hizmetleriyle de çalışır.

- Çok varlıklı veya site karşılaştırması için birbirinden farklı konumlardan akış yapan zaman serisi verilerinin genel görünümünü kazanın.

   Bir Time Series Insights ortamına birden fazla olay kaynağı bağlayabilirsiniz. Bu şekilde, birden çok farklı konumdan gelen verileri neredeyse gerçek zamanlı olarak birlikte görüntüleyebilirsiniz. Kullanıcılar, iş liderleriyle veri paylaşmak için bu görünürlüktan yararlanabilir. Sorunları çözmeye yardımcı olmak, en iyi uygulamaları uygulamak ve öğrenmeleri paylaşmak için uzmanlıklarını uygulayabilen etki alanı uzmanlarıyla daha iyi işbirliği yapabilir.

- Time Series Insights'ın üzerine bir müşteri uygulaması oluşturun.

   Zaman Serisi Öngörüleri, zaman serisi verilerini kullanan uygulamalar oluşturmak için kullanabileceğiniz REST Sorgu API'lerini ortaya çıkarır.

## <a name="capabilities"></a>Özellikler

- **Hızlı bir şekilde başlayın**: Azure Time Series Öngörüleri ön veri hazırlama gerektirmez, böylece IoT hub'ınızdaki veya etkinlik merkezinizdeki milyonlarca etkinliğe hızlı bir şekilde bağlanabilirsiniz. Bağlandıktan sonra, IoT çözümlerinizi hızlı bir şekilde doğrulamak için sensör verilerini görselleştirebilir ve etkileşimde olabilirsiniz. Kod yazmadan verilerinizle etkileşim kurabilirsiniz ve yeni bir dil öğrenmeniz gerekmez. Time Series Insights, gelişmiş kullanıcılar için ayrıntılı, serbest metin sorgu yüzeyi ve nokta ve tıkla arama sağlar.

- **Neredeyse gerçek zamanlı öngörüler**: Time Series Insights, bir dakikalık gecikme süresiyle günde milyonlarca sensör olayını yutabilir. Time Series Insights, sensör verilerinize ilişkin öngörüler elde eve girmenize yardımcı olur. Eğilimleri ve anormallikleri tespit etmek, kök neden analizleri yapmak ve maliyetli kapalı kalma sürelerini önlemek için kullanın. Gerçek zamanlı ve geçmiş veriler arasındaki çapraz ilişki, verilerdeki gizli eğilimleri bulmanıza yardımcı olur.

- **Özel çözümler oluşturun**: Azure Time Series Öngörüleri verilerini mevcut uygulamalarınıza gömün. Ayrıca Time Series Insights REST API'leri ile yeni özel çözümler oluşturabilirsiniz. Diğer kişilerin de içgörülerinizi incelemesini sağlamak için paylaşabileceğiniz kişiselleştirilmiş görünümler oluşturun.

- **Ölçeklenebilirlik**: Zaman Serisi Görüşleri, ölçekli olarak IoT'yi destekleyecek şekilde tasarlanmıştır. Günde 1 milyon ile 100 milyon arası olay alabilir ve varsayılan saklama süresi 31 gündür. Geçmiş verilerin yanı sıra canlı veri akışlarını neredeyse gerçek zamanlı olarak görselleştirebilir ve analiz edebilirsiniz.

## <a name="get-started"></a>Kullanmaya başlayın

Başlamak için aşağıdaki adımları izleyin.

1. Azure portalında Bir Zaman Serisi Öngörüleri ortamını sağlama.
1. IoT hub'ı veya olay hub'ı gibi bir olay kaynağına bağlanın.
1. Referans verilerini yükleyin. Bu ek bir hizmet değil.
1. Zaman Serisi Öngörüleri kaşifi ile verilerinizi dakikalar içinde gözden geçirin ve görüntüleyin.

## <a name="explorer"></a>Gezgin

Azure Zaman Serisi Öngörüleri kaşifini kullanarak verilerinizdeki eğilimleri görüntüleyin, analiz edin ve keşfedin.

![Time Series Insights gezgini](media/overview/time-series-insights-explorer-panel.png)

[Azure Zaman Serisi Öngörüleri kaşifini](time-series-insights-explorer.md) nasıl kullanacağınızı öğrenin ve verilerinizden öngörüler elde edin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Zaman Serisi Öngörüleri genel kullanılabilirlik [ücretsiz gösteri ortamını](./time-series-quickstart.md)keşfedin.

- [Time Series Insights ortamınızı nasıl planlayınız](time-series-insights-environment-planning.md) hakkında daha fazla bilgi edinin.
