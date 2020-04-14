---
title: 'Genel Bakış: Azure Zaman Serisi Öngörüleri Önizlemesi nedir? - Azure Zaman Serisi Öngörüleri | Microsoft Dokümanlar'
description: Azure Zaman Serisi Öngörüleri Önizlemesi'ndeki değişiklikler, iyileştirmeler ve özellikler hakkında bilgi edinin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: overview
ms.date: 04/13/2020
ms.custom: seodec18
ms.openlocfilehash: 73244a635bbf14efcf33f1b978db14e9e2589581
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81271072"
---
# <a name="what-is-azure-time-series-insights-preview"></a>Azure Time Series Insights Önizleme nedir?

Azure Zaman Serisi Öngörüleri Önizleme, hizmet olarak uçuca bir platform (PaaS) teklifidir. Verileri Nesnelerin İnterneti (IoT) ölçeğinde toplamak, işlemek, depolamak, analiz etmek ve sorgulamak için kullanabilirsiniz-- yüksek düzeyde bağlamsallaştırılmış ve zaman serileri için optimize edilmiş veriler. 

Time Series Insights, geçici veri arama ve operasyonel analiz için tasarlanmıştır. Endüstriyel IoT dağıtımlarının geniş gereksinimlerini karşılayan genişletilebilir ve özelleştirilmiş bir hizmet teklifidir.

## <a name="video"></a>Video

Azure Zaman Serisi Öngörüleri Önizlemesi hakkında daha fazla bilgi edinin.

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Time-Series-Insights-e2e-solution-for-industrial-IoT-analytics/player]

## <a name="definition-of-iot-data"></a>IoT verilerinin tanımı

Varlık yoğun kuruluşlarda endüstriyel IoT verileri genellikle endüstriyel bir ortamda cihazların ve sensörlerin çeşitli doğası nedeniyle yapısal tutarlılık yoksundur. Bu akışlardan elde edilen veriler önemli boşluklar ve bazen bozuk iletiler ve yanlış okumalarla karakterize edilir. IoT verileri genellikle, uçtan uca iş akışları için bağlam ekleyen CRM veya ERP gibi birinci taraf veya üçüncü kaynaklardan gelen ek veri girişleri bağlamında anlamlıdır. Hava durumu verileri gibi üçüncü taraf veri kaynaklarından gelen girdiler, belirli bir yüklemedeki telemetri akışlarının artırılmasına yardımcı olabilir. 

Tüm bunlar, verilerin yalnızca bir kısmının operasyonel ve ticari amaçlar için kullanıldığını ve analizin bağlamsallaştırmayı gerektirdiğini ima eder. Endüstriyel veriler, eğilimleri anlamak ve ilişkilendirmek için daha uzun zaman aralıklarında derinlemesine analiz için genellikle tarihlenir. Toplanan IoT verilerinin işlem edilebilir öngörülere dönüştürülmesi şunları gerektirir: 

* Verileri temizlemek, filtrelemek, enterpolasyon yapmak, dönüştürmek ve analiz için veri hazırlamak için veri işleme.
* Verileri normalleştirmek ve bağlamsallaştırmak için verileri, yani verileri anlamak ve gezinmek için bir yapı.
* İşlenmiş (veya türetilen) verilerin ve ham verilerin uzun veya sonsuz tutulması için uygun maliyetli depolama.

Bu tür veriler, iş analizi ve raporlama sı için tutarlı, kapsamlı, güncel ve doğru bilgiler sağlar.

Aşağıdaki resimde tipik bir IoT veri akışı gösterilmektedir.

[![IoT veri akışı](media/v2-update-overview/overview-one.png)](media/v2-update-overview/overview-one.png#lightbox)

## <a name="azure-time-series-insights-for-industrial-iot"></a>Endüstriyel IoT için Azure Zaman Serisi Öngörüleri

IoT peyzaj üretim, otomotiv, enerji, kamu hizmetleri, akıllı binalar ve danışmanlık da dahil olmak üzere endüstri segmentleri çeşitli yayılan müşterileri ile çeşitlidir. Bu geniş kapsamlı endüstriyel IoT pazarında, büyük ölçekli IoT verilerini hedefleyen kapsamlı analitik sağlayan bulut ait çözümler hala gelişmektedir. 

Azure Time Series Öngörüleri, zaman serisi verilerinin bağlamsallaştırılması için zengin semantik modelleme, varlık tabanlı öngörüler ve keşif, trend, anormallik tespiti ve operasyonel zeka için sınıfının en iyisi kullanıcı deneyimi yle anahtar teslimi, uçtan uca IoT analitik çözümü sağlayarak bu pazar gereksinimini giderir. 

İnteraktif veri arama yeteneklerimizle birlikte zengin bir operasyonel analiz platformu olarak, IoT varlıklarından toplanan verilerden daha fazla değer elde etmek için Time Series Insights'ı kullanabilirsiniz. Önizleme teklifi destekler: 

* Sıcak ve soğuk analitikiçeren çok katmanlı depolama çözümü, müşterilere, on yıllardır süren geçmiş veriler üzerinden etkileşimli analizler için verileri sıcak ve soğuk arasında yönlendirme seçeneği sunar. 

    *    Daha kısa zaman aralığı verileri üzerinde sık ve çok sayıda sorgu gerçekleştirmek için son derece etkileşimli sıcak analitik çözümü 
    *    Azure Depolama'yı temel alan ölçeklenebilir, performans ve maliyet en iyi duruma getirilmiş zaman serisi veri gölü, müşterilerin yıllar süren zaman serisi verilerini saniyeler içinde eğilimlemesine olanak tanır. 

* Varlıklardan ve aygıtlardan türetilen ve ham sinyallerle ilişkili etki alanını ve meta verileri açıklayan anlamsal model desteği.

* Geçmiş zaman serisi verilerini müşteriye ait Azure Depolama hesabında depolamak için esnek analiz platformu, böylece müşterilerin IoT verilerine sahip olmasını sağlar. Veriler, tahmine dayalı analitik, makine öğrenimi ve Spark, Databricks ve Jupyter gibi tanıdık teknolojiler kullanılarak yapılan diğer özel hesaplamalar da dahil olmak üzere çeşitli veri senaryolarında bağlantı ve interop sağlayan açık kaynak Apache Parke formatında depolanır.

* Varlık tabanlı veri öngörülerini zengin, geçici veri analitiği ile interpolasyon, skaler ve toplu işlevler, kategorik değişkenler, dağılım çizimleri ve zaman kaydırma zaman serileri için destekle birleştiren gelişmiş sorgu API'leri ve kullanıcı deneyimine sahip zengin analitik.

*    Kurumsal IoT müşterilerimizin ölçek, performans, güvenlik ve güvenilirlik ihtiyaçlarını desteklemek için kurumsal sınıf platformu.

* Uçtan uca analiz için genişletilebilirlik ve tümleştirme desteği. Time Series Insights, çeşitli veri senaryoları için genişletilebilir bir analiz platformu sağlar. Time Series Insights Power BI konektörü, müşterilerin Bi ve zaman serisi analizlerini tek bir cam bölmede birleşik bir şekilde görüntülemek için Time Series Insights'ta yaptıkları sorguları doğrudan Power BI'ye almalarını sağlar.

Aşağıdaki diyagram, üst düzey veri akışını gösterir.

  [![Temel işlevler](media/v2-update-overview/overview-two.png)](media/v2-update-overview/overview-two.png#lightbox)

Azure Time Series Öngörüleri, veri işleme, depolama (veri ve meta veriler) ve sorgu için ölçeklenebilir bir kullanıma göre ödeme fiyatlandırma modeli sunarak müşterilerin kullanımlarını iş taleplerine uyacak şekilde ayarlamalarını sağlar. 
 
Bu önemli endüstriyel IoT yeteneklerinin piyasaya sürülmesiyle, Time Series Insights aşağıdaki önemli avantajları da sağlar.  

| | |
| ---| ---|
| IoT ölçeğinde zaman serisi verileri için çok katmanlı depolama | Verileri sindiren paylaşılan bir veri işleme ardışık işlem ardışık alanı yla, verileri hem sıcak hem de soğuk depolara alabilirsiniz. Etkileşimli sorgular için sıcak depove büyük hacimli verileri depolamak için soğuk depo kullanın. Yüksek performanslı varlık tabanlı sorgulardan nasıl yararlanılabildiğini öğrenmek için [sorgulara](./time-series-insights-update-tsq.md)bakın. |
| Raw telemetriyi bağlamsallaştırmak ve varlık tabanlı öngörüler elde etmek için Zaman Serisi Modeli | Zaman serisi verileriniz için örnekler, hiyerarşiler, türler ve değişkenler oluşturmak için zaman serisi modelini kullanabilirsiniz. Time Serisi Modeli hakkında daha fazla bilgi edinmek için [Time Series Modeli'ne](./time-series-insights-update-tsm.md)bakın.  |
| Diğer veri çözümleriyle sorunsuz ve sürekli entegrasyon | Time Series Insights'taki veriler açık kaynak kodlu Apache Parke dosyalarında [saklanır.](./time-series-insights-update-storage-ingress.md) Bu, iş zekası, gelişmiş makine öğrenimi ve tahmine dayalı analitik içeren senaryolar için 1. veya 3. |
| Yakın gerçek zamanlı veri arama | [Azure Time Series Öngörüleri Önizleme explorer](./time-series-insights-update-explorer.md) kullanıcı deneyimi, yutma ardışık boru hattı üzerinden tüm veri akışı için görselleştirme sağlar. Bir olay kaynağını bağladıktan sonra olay verilerini görüntüleyebilir, keşfedebilir ve sorgulayabilirsiniz. Bu şekilde, bir aygıtın beklendiği gibi veri yayıp yayacağını doğrulayabilirsiniz. Ayrıca, sağlık, üretkenlik ve genel etkinlik için bir IoT varlığını da izleyebilirsiniz. | 
| Genişletilebilirlik ve entegrasyon | Azure Time Series Insights Power BI Connector entegrasyonu, müşterilerin kullanıcı deneyimimizde oluşturdukları zaman serisi sorgularını doğrudan Power BI masaüstüne aktarmalarına ve zaman serisi grafiklerini diğer BI analizleriyle birlikte görüntülemelerine olanak tanıyan **Dışa Aktarma** seçeneği aracılığıyla Doğrudan Time Series Explorer kullanıcı deneyiminde kullanılabilir. Bu, IoT zaman serisi de dahil olmak üzere çeşitli veri kaynaklarından analitik üzerinde tek bir cam bölmesi sağlayarak Power BI'ye yatırım yapmış endüstriyel IoT işletmeleri için yeni bir senaryo sınıfına kapı açar. | 
| Time Series Insights platformunda yerleşik özel uygulamalar | Zaman Serisi Insights [JavaScript SDK](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)destekler. SDK zengin denetimler ve sorgulara basitleştirilmiş erişim sağlar. İş gereksinimlerinize uyacak şekilde Time Series Insights'ın üstüne özel IoT uygulamaları oluşturmak için SDK'yı kullanın. Verileri özel IoT uygulamalarına yönlendirmek için Zaman Serisi Öngörüleri [Sorgu API'lerini](./time-series-insights-update-tsq.md) de kullanabilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Time Series Öngörüleri Önizlemesi ile başlayın:

> [!div class="nextstepaction"]
> [Hızlı başlangıç kılavuzu](./time-series-insights-update-quickstart.md)

Kullanım örnekleri hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Azure Zaman Serisi Öngörüleri Önizleme kullanım örnekleri](./time-series-insights-update-use-cases.md)