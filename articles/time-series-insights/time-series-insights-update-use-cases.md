---
title: Önizleme kullanım örnekleri - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Azure Time Series Öngörüleri Önizleme kullanım örnekleri hakkında bilgi edinin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 50ac2a728750c6b01dfc57fa7e20df25c856395a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77087379"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Azure Zaman Serisi Öngörüleri Önizleme kullanım örnekleri

Bu makalede, Azure Zaman Serisi Öngörüleri Önizlemesi için birkaç yaygın kullanım vakası özetlenmiştir. Bu makaledeki öneriler, Time Series Insights ile uygulamalarınızı ve çözümlerinizi geliştirmek için bir başlangıç noktası görevi görededir.

Özellikle, bu makalede aşağıdaki soruları yanıtlar:

* Time Series Insights için yaygın kullanım örnekleri nelerdir?
* [Veri arama ve görsel anomali tespiti](#data-exploration-and-visual-anomaly-detection)için Time Series Insights kullanmanın yararları nelerdir?
* [Operasyonel analiz ve süreç verimliliği](#operational-analysis-and-driving-process-efficiency)için Time Series Öngörülerini kullanmanın yararları nelerdir?
* [Gelişmiş analitik](#advanced-analytics)için Time Series Öngörülerini kullanmanın avantajları nelerdir?

Bu kullanım senaryolarına genel bir bakış aşağıdaki bölümlerde açıklanmıştır.

## <a name="introduction"></a>Giriş

Azure Time Series Öngörüleri, hizmet olarak uça bir tekliftir. Yüksek oranda bağlamsallaştırılmış, zaman serileri için optimize edilmiş IoT ölçeğine göre verileri toplamak, işlemek, depolamak, analiz etmek ve sorgulamak için kullanılır. Time Series Insights, özel veri arama ve operasyonel analiz için idealdir. Time Series Insights, endüstriyel IoT dağıtımlarının geniş gereksinimlerini karşılayan benzersiz bir şekilde genişletilebilir, özelleştirilmiş bir hizmet teklifidir.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Veri arama ve görsel anomali tespiti

Anormallikleri tespit etmek ve verilerinizdeki gizli eğilimleri keşfetmek için milyarlarca olayı anında keşfedin ve analiz edin. Time Series Insights, IoT ve DevOps analiz iş yükleriniçin neredeyse gerçek zamanlı performans sunar.

[![Veri gezgini](media/v2-update-use-cases/data-explorer.png)](media/v2-update-use-cases/data-explorer.png#lightbox)

Çoğu müşteri, içgörü kazanmak için gereken en az sürenin Time Series Insights'ın öne çıkan özelliklerinden biri olduğu konusunda hemfikirdir:

* Time Series Insights hiçbir ön veri hazırlama gerektirir. 
* Azure IoT Hub'ınızdaki veya Azure Etkinlik Hub'ları örneklerinizde dakikalar içinde milyarlarca etkinliğe bağlanmanız için hızlı çalışır. 
* Bağlandıktan sonra, anormallikleri tespit etmek ve verilerinizdeki gizli eğilimleri keşfetmek için milyarlarca olayı görselleştirebilir ve analiz edebilirsiniz.

Time Series Insights sezgisel ve kullanımı basittir. Tek bir kod satırı yazmadan verilerinizle etkileşim kurabilirsiniz. Time Series Insights, SQL'e aşina olan gelişmiş kullanıcılar için ayrıntılı metin tabanlı bir sorgu dili sağlasa da, öğrenmeniz gereken yeni bir dil de yoktur. Ayrıca acemiler için seçin ve tıkla keşif sağlar.

Müşteriler, varlıkla ilgili sorunları hızlı bir şekilde tanılamak için hızdan yararlanabilir. Bir IoT çözümündeki bir hatanın kök nedenine ulaşmak için DevOps çözümlemesi gerçekleştirebilirler. Ayrıca, veri bilimi girişimlerinin bir parçası olarak daha fazla araştırma için işaretlenebilen alanları da belirleyebilirler. 

Zaman Serisi Öngörüleri'nde depolanan verilerle etkileşim kurmanın üç temel yolu vardır:

* Başlamak için ilk ve en kolay yolu Time Series Insights Preview explorer ile. Tüm IoT verilerinizi tek bir yerde hızla görselleştirmek için kullanabilirsiniz. Verilerinizdeki anormallikleri tespit etmenize yardımcı olmak için ısı haritası gibi araçlar sağlar. Ayrıca bir perspektif görünümü sağlar. Tek bir panodaki bir veya daha fazla Zaman Serisi Öngörüsünü ortamdan en fazla dört görüntülemeyi karşılaştırmak için kullanın. Pano, zaman serisi verilerinizin tüm konumlarınızda görünümünü sağlar. Zaman Serisi [Öngörüleri Önizleme kaşifi](./time-series-insights-update-explorer.md)hakkında daha fazla bilgi edinin. Zaman Serisi Öngörüleri ortamınızı planlamak için [Time Series Insights planlamasını](./time-series-insights-update-plan.md)okuyun.

* Başlatmanın ikinci yolu, web uygulamanıza güçlü grafikleri ve grafikleri hızla yerleştirmek için JavaScript SDK'yı kullanmaktır. Sadece birkaç kod satırıyla güçlü sorgular yazabilirsiniz. Satır grafikleri, pasta grafikleri, çubuk grafikleri, ısı haritalarını, veri Kılavuzlarını ve daha fazlasını doldurmak için bunları kullanın. Tüm bu öğeler SDK kullanılarak kutunun dışında bulunmaktadır. SDK ayrıca Zaman Serisi Öngörüleri sorgu API'lerini de özetler. Bunları, panoda göstermek istediğiniz verileri sorgulamak için SQL benzeri yüklemler yazarken kullanabilirsiniz. Zaman Serisi Öngörüler, karma sunum katmanı çözümleri için parametreli URL'ler sunar. Verilere derinlemesine dalmak için Zaman Serisi Öngörüleri Önizleme gezgini ile kesintisiz bağlantı noktaları sağlarlar.

  * JavaScript SDK hakkında daha fazla bilgi edinmek için [Time Series Insights JS istemci kitaplığı](https://github.com/microsoft/tsiclient/blob/master/docs/API.md) ve [Time Series Insights istemci](https://github.com/Microsoft/tsiclient) belgeleri hakkında bilgi edinin.

  * [Azure Zaman Serisi Öngörüleri Önizleme gezgininde Visualize verilerini](time-series-insights-update-explorer.md)inceleyerek URL'leri ve yeni web aralarını paylaşma hakkında daha fazla bilgi edinin.

* Başlatmanın üçüncü yolu, Time Series Insights'ta depolanan verileri sorgulamak için güçlü API'leri kullanmaktır. Zaman Serisi Insights `from`gibi zamansal operatörler vardır , , `to` `first`, ve `last`. Bu toplama ve dönüşümler gibi `average` `min`vardır `max` `split by`, `order by`, `DateHistogram`, , , ve . Ayrıca , `has`, `in` `and` `or`, , `greater than`, ve `REGEX`. Tüm bu operatörler, verilerinizdeki ilginç eğilimleri ve desenleri hızla bulmak için akış aşağı uygulamaları sağlar. Anormallikleri tespit etmek için yerel görselleri doldurmak için bunları kullanın.

## <a name="operational-analysis-and-driving-process-efficiency"></a>Operasyonel analiz ve sürüş süreci verimliliği

Ölçeğinde ekipmanın durumunu, kullanımını ve performansını izlemek için Zaman Serisi Öngörülerini kullanın. Time Series Insights, operasyonel verimliliği ölçmenin kolay bir yolunu sunar. Time Series Insights, çeşitli ve öngörülemeyen IoT iş yüklerinin, yutma veya sorgu performansından ödün vermeden yönetilmesine yardımcı olur.

[![Genel Bakış](media/v2-update-use-cases/overview.png)](media/v2-update-use-cases/overview.png#lightbox)

Operasyonel süreçlerden gelen verilerin akışı ve sürekli işlenmesi, doğru teknoloji veya çözümle birleştiğinde herhangi bir işletmeyi başarıyla dönüştürebilir. Genellikle bu çözümler birden çok sistem bir arada vardır. Özellikle IoT aleminde sürekli değişen verilerin araştırılmasını ve analiz edilmesini sağlar ve ortak bir örüntü paylaşır.

Bu desenler genellikle çeşitli yerel lere yayılan aygıtlardan ve sensörlerden milyarlarca olay yutan IoT özellikli platformlarla başlar. Bu sistemler, gerçek zamanlı öngörüler ve eylemler elde etmek için akış verilerini işleyip analiz eder. Veriler genellikle yakın gerçek zamanlı ve toplu iş analitiği için sıcak ve soğuk depoya arşivlenir.

Toplanan veriler, akış aşağı sorgulama ve analiz senaryoları için temizlemek ve bağlamsallaştırmak için bir dizi işlemden geçer. Azure, varlık bakımı ve üretim gibi IoT senaryolarına uygulanabilen zengin hizmetler sunar. Bu hizmetler arasında Time Series Insights, IoT Hub, Etkinlik Hub'ları, Azure Akış Analizi, Azure İşlevleri, Azure Mantıksal Uygulamaları, Azure Databricks, Azure Machine Learning ve Power BI yer alır.

Çözüm mimarisi aşağıdaki şekilde elde edilebilir:

* Sınıfının en iyisi güvenlik, iş ortası ve gecikme için IoT Hub veya Olay Hub'ları üzerinden veri alın.
* Veri işleme ve hesaplamalar gerçekleştirin. Akış Analizi, Mantık Uygulamaları ve Azure Fonksiyonları gibi hizmetler aracılığıyla veri akışı. Kullandığınız hizmet, belirli veri işleme gereksinimlerine bağlıdır.
* İşleme ardışık boru hattından gelen hesaplanmış sinyaller depolama ve analiz için Time Series Insights'a itilir.

Time Series Insights, geçmiş veriler üzerinde neredeyse gerçek zamanlı veri arama ve varlık tabanlı öngörüler sunar. İş gereksinimlerinize bağlı olarak, MapReduce ve Hive işleri, Time Series Insights'ı Azure HDInsight'a bağlayarak Time Series Insights'ta depolanan verilerle işleyebilir. Time Series Insights'ta depolanan veriler Power BI ve diğer müşteri uygulamalarında Time Series Insights genel yüzey sorgusu API'leri aracılığıyla kullanılabilir. Bu veriler derin iş ve operasyonel istihbarat senaryoları için kullanılabilir.

## <a name="advanced-analytics"></a>Gelişmiş analiz

Machine Learning ve Azure Databricks gibi gelişmiş analiz hizmetleriyle tümleştirin. Time Series Insights, milyonlarca cihazdan ham veri girişi yapar. Azure analiz hizmetleri paketi tarafından sorunsuz bir şekilde tüketilebilen bağlamsal veriler ekler.

[![Analytics](media/v2-update-use-cases/advanced-analytics.png)](media/v2-update-use-cases/advanced-analytics.png#lightbox)

Gelişmiş analitik ve makine öğrenimi büyük hacimli verileri tüketir ve işler. Bu veriler, veri odaklı kararlar almak ve tahmine dayalı analizler yapmak için kullanılır. IoT kullanım durumlarında, gelişmiş analiz algoritmaları milyonlarca cihazdan toplanan verilerden öğrenir. Bu aygıtlar verileri saniyede birden çok kez iletir. IoT aygıtlarından toplanan veriler hamdır. Cihazın konumu ve sensör okuma birimi gibi bağlamsal bilgilerden yoksun. Sonuç olarak, ham verileri doğrudan gelişmiş analizler için tüketmek zordur.

Time Series Insights, IoT verileri ile gelişmiş analitik arasındaki boşluğu iki basit ve uygun maliyetli şekilde birbirine kapatır:

* İlk olarak, Time Series Insights, IoT Hub'ı kullanarak milyonlarca cihazdan ham telemetri verileri toplar. Verileri bağlamsal bilgilerle zenginleştirir ve verileri parke biçimine dönüştürür. Bu biçim, Machine Learning, Azure Databricks ve üçüncü taraf uygulamaları gibi diğer gelişmiş analiz hizmetleriyle kolayca tümleşebilir.

    Time Series Insights, kuruluştaki tüm veriler için gerçeğin kaynağı olabilir. Akış aşağı analiz iş yükleri için merkezi bir depo oluşturur. Time Series Insights neredeyse gerçek zamanlı depolama hizmeti olduğundan, gelişmiş analitik modelleri gelen IoT telemetri verilerinden sürekli olarak öğrenebilir. Sonuç olarak, modeller daha doğru tahminler de yapabilir.

* İkinci olarak, makine öğrenimi ve tahmin modellerinin çıktısı, sonuçlarını görselleştirmek ve depolamak için Time Series Insights'a aktarılabilir. Bu yordam, kuruluşların modellerini optimize etmesine ve düzeltmesine yardımcı olur. Time Series Insights, akış telemetri verilerini eğitimli model çıktıları ile aynı düzlemde görselleştirmeyi kolaylaştırır. Bu şekilde, veri bilimi ekiplerinin anormallikleri tespit etmelerine ve desenleri belirlemelerine yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar

* Zaman Serisi [Öngörüleri Önizleme kaşifi](./time-series-insights-update-explorer.md)hakkında daha fazla bilgi edinin.
* [Zaman Serisi Öngörüleri](./time-series-insights-update-plan.md) Çevrenizi planlamayı okuyun.
* Time [Series Insights istemci](https://github.com/Microsoft/tsiclient) belgelerini okuyun.
