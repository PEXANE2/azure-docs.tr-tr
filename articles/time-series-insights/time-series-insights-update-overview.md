---
title: 'Genel Bakış: Azure Time Series Insights Önizleme | Microsoft Docs'
description: Azure Time Series Insights önizlemeye genel bakış.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: overview
ms.date: 10/22/2019
ms.custom: seodec18
ms.openlocfilehash: 588d6dafd3703743f5f4c50bea4f27ef0225bbf7
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72989714"
---
# <a name="what-is-azure-time-series-insights-preview"></a>Azure Time Series Insights Önizleme nedir?

Azure Time Series Insights önizlemesi, uçtan uca bir hizmet olarak platform (PaaS) teklifiyle yapılır. Yüksek düzeyde tanımlı, zaman serisi iyileştirilmiş IoT ölçeğinde verileri toplamak, işlemek, depolamak, çözümlemek ve sorgulamak için kullanılır. Time Series Insights, geçici veri araştırması ve işletimsel analizler için idealdir. Time Series Insights, endüstriyel IoT dağıtımlarının geniş ihtiyaçlarını karşılayan benzersiz bir şekilde genişletilebilir ve özelleştirilmiş bir hizmet sunumudur.

> [!TIP]
> Genel kullanılabilirlik (GA) özellikleri için [Azure TIME SERIES INSIGHTS GA genel bakış](time-series-insights-overview.md)makalesini okuyun.

## <a name="video"></a>Video

### <a name="learn-more-about-azure-time-series-insights-preview-br"></a>Azure Time Series Insights önizleme hakkında daha fazla bilgi edinin. </br>

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Time-Series-Insights-e2e-solution-for-industrial-IoT-analytics/player]

## <a name="define-iot-data"></a>IoT verilerini tanımlama

IoT verileri, varlık açısından yoğun kuruluşlarda bulunan tüm endüstriyel verileri kullanır. IoT verileri, oldukça gürültülü ölçümler kaydeden varlıklardan gönderildiği için genellikle yüksek oranda yapılandırılmamış. Bu ölçümler sıcaklık, hareket ve nem içerir. Bu veri akışları genellikle önemli boşluklar, bozuk iletiler ve yanlış okumalar tarafından belirlenir. Herhangi bir çözümlemenin gerçekleşebilmesi için Bu akışlardan verilerin temizlenmesi gerekir.

IoT verileri genellikle yalnızca, CRM veya ERP gibi birinci taraf kaynaklardan gelen ek veri girişlerinin bağlamında anlamlıdır. Girişler, hava durumu veya konum gibi üçüncü taraf veri kaynaklarından de gelir.

Sonuç olarak, yalnızca işletimsel ve iş amaçları için verilerin bir bölümü kullanılır. Bu veriler, iş raporlama ve analizine yönelik tutarlı, kapsamlı, güncel ve doğru bilgiler sağlar. Toplanan IoT verilerini eyleme dönüştürülebilir içgörüler halinde açmak şunları gerektirir:

* Verilerin çözümlenmesi, filtreleneceği, enterpola, dönüştürülemesinde ve analiz amacıyla hazırlanması için veri işleme.
* Verileri normalleştirerek, diğer bir deyişle verileri normalleştirmek ve anlamak için bir yapı.
* İşlenen veya türetilmiş, veri ve ham veriler için uzun veya sınırsız saklama için ekonomik depolama maliyeti.

Aşağıdaki görüntüde tipik bir IoT veri akışı gösterilmektedir.

  [![IoT veri akışı](media/v2-update-overview/overview-one.png)](media/v2-update-overview/overview-one.png#lightbox)

## <a name="azure-time-series-insights-for-industrial-iot"></a>Endüstriyel IoT için Azure Time Series Insights

Geçerli IoT yatay, farklı. Müşteriler, üretim, oto ve enerji, yardımcı programları, akıllı binalar ve danışmanlık sektörlere yaymıştır. Senaryolar, verilerin şeklinin bilinmediği, geçici veri araştırması içerir. Senaryolar Ayrıca, işlem verimliliğini şema üzerinde veya açıkça modellenen verileri kapsayan işlem analizini de kapsar. Bu senaryolar genellikle yan yana bulunur ve farklı kullanım durumlarını destekler. Endüstriyel IoT kuruluşları ve bunların dijital devrimlerinin başarısına yönelik anahtar olan platform özellikleri şunlardır:

- Çoklu katmanlı depolama, hem sıcak hem de soğuk.
- Zaman serisi verilerinin Decades 'i depolama olanağı.
- Varlık tabanlı operasyonel zeka için sorguları açıkça modelleyebilir ve iyileştirmenize olanak tanır.

Time Series Insights IoT veri araştırması ve operasyonel içgörüler için kapsamlı, uçtan uca PaaS sunumudur. Time Series Insights, IoT ölçeğinde zaman serisi verilerini çözümlemek için tam olarak yönetilen bir bulut hizmeti sunar.

Ham verileri, bir şema daha az, bellek içi depoda saklayabilirsiniz. Daha sonra, dağıtılmış bir sorgu altyapısı ve API aracılığıyla etkileşimli geçici sorgular gerçekleştirebilirsiniz. Birkaç saniye içinde milyarlarca olayı görselleştirmek için zengin Kullanıcı deneyimini kullanın. [Veri araştırma özellikleri](./time-series-insights-overview.md)hakkında daha fazla bilgi edinin.

Time Series Insights Şu anda önizleme aşamasında olan operasyonel içgörüler özellikleri de sunmaktadır. Etkileşimli veri araştırması ve operasyonel zeka ile birlikte, IoT varlıklarından toplanan verilerden daha fazla değer elde etmek için Time Series Insights kullanabilirsiniz. Önizleme teklifi şunları destekler:

* Ölçeklenebilir ve performans ve uygun maliyetli bir zaman serisi veri deposu. Bu bulut tabanlı IoT çözümü, birkaç saniye içinde zaman serisi verilerinin eğilim yaşına sahip olabilir.
* Varlık ve cihazlardan gelen ve türetilmiş olmayan sinyallerle ilişkili etki alanını ve meta verileri açıklayan anlam modeli desteği.
* Varlık tabanlı veri öngörülerini zengin, geçici veri analiziyle birleştiren gelişmiş bir kullanıcı deneyimi. Bu bileşim, iş ve operasyonel zeka sürücüleri.
* Gelişmiş makine öğrenimi ve analiz araçlarıyla tümleştirme. Araçlar Azure Databricks, Apache Spark, Azure Machine Learning, Jupyıter not defterlerini ve Power BI içerir. Bu araçlar, zaman serisi veri zorluklarını ve operasyonel verimliliği kullanmanıza yardımcı olur.

İşletimsel içgörüler ve veri araştırması, veri işleme, depolama ve sorgu için basit bir Kullandıkça Öde fiyatlandırma modeliyle sunulur. Bu Faturalandırma modeli, değişen iş gereksinimlerinize uygun.

Bu üst düzey veri akışı diyagramı güncelleştirmeleri gösterir.

  [![temel özellikleri](media/v2-update-overview/overview-two.png)](media/v2-update-overview/overview-two.png#lightbox)

Bu temel Endüstriyel IoT özelliklerine giriş ile Time Series Insights aşağıdaki temel avantajları sağlar.

| | |
| ---| ---|
| IoT ölçeğinde zaman serisi verileri için çok katmanlı depolama | Verileri almak için ortak bir veri işleme işlem hattı ile etkileşimli sorgular için verileri ısınma depolamada saklayabilirsiniz. Ayrıca, büyük miktarlarda veri için verileri soğuk depolamada saklayabilirsiniz. Yüksek performanslı varlık tabanlı [sorgulardan](./time-series-insights-update-tsq.md)yararlanın. |
| Ham telemetri eklemek ve varlık tabanlı Öngörüler türetmeye yönelik zaman serisi modeli | Tanımlayıcı [zaman serisi modeliyle](./time-series-insights-update-tsm.md)ham telemetri verileri oluştur. Yüksek performans ve uygun maliyetli cihaz tabanlı sorgularla zengin operasyonel zeka türetebilirsiniz. |
| Diğer veri çözümleriyle sorunsuz ve sürekli tümleştirme | Time Series Insights veriler, açık kaynaklı Apache Parquet dosyalarında [depolanır](./time-series-insights-update-storage-ingress.md) . Bu diğer veri çözümleriyle tümleştirme, birinci veya üçüncü taraf, uçtan uca senaryolar için kolaydır. Bu senaryolar iş zekası, gelişmiş makine öğrenimi ve tahmine dayalı analiz bilgilerini içerir. |
| Neredeyse gerçek zamanlı veri araştırması | [Azure Time Series Insights Preview Explorer](./time-series-insights-update-explorer.md) Kullanıcı deneyimi, alma işlem hattı aracılığıyla tüm veri akışı için görselleştirme sağlar. Bir olay kaynağını bağlandıktan kısa süre sonra olay verilerini görüntüleyebilir, keşfedebilir ve sorgulayabilirsiniz. Bu şekilde, bir cihazın verileri beklendiği gibi yayıp yaymadığını doğrulayabilirsiniz. Ayrıca, bir IoT varlığını sistem durumu, üretkenlik ve genel verimlilik için izleyebilirsiniz. |
| Kök nedeni analiz ve anomali algılama | [Azure Time Series Insights önizleme Gezgini](./time-series-insights-update-explorer.md) , çoklu adım, kök neden analizini yürütmek ve kaydetmek için hem model hem de perspektif görünümlerini destekler. Azure Stream Analytics ile birlikte, uyarıları ve anormallikleri neredeyse gerçek zamanlı olarak algılamak için Time Series Insights kullanabilirsiniz. |
| Time Series Insights platformunda oluşturulan özel uygulamalar | Time Series Insights [JavaScript SDK 'sını](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)destekler. SDK, sorgulara zengin denetimler ve Basitleştirilmiş erişim sağlar. Belirli iş gereksinimlerinize uyacak şekilde Time Series Insights en üstünde özel IoT uygulamaları oluşturmak için SDK 'Yı kullanın. Ayrıca, verileri özel IoT uygulamalarına yönlendirmek için doğrudan Time Series Insights [sorgu API 'lerini](./time-series-insights-update-tsq.md) de kullanabilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Time Series Insights Preview ile çalışmaya başlama:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç kılavuzunu okuyun](./time-series-insights-update-quickstart.md)

Kullanım örnekleri hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Azure Time Series Insights Önizleme kullanım örnekleri](./time-series-insights-update-use-cases.md)
