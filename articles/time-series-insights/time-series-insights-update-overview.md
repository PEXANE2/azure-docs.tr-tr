---
title: 'Genel Bakış: Azure Time Series Insights önizlemesi nedir? -Azure Time Series Insights | Microsoft Docs'
description: Azure Time Series Insights önizlemede değişiklikler, iyileştirmeler ve özellikler hakkında bilgi edinin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: overview
ms.date: 04/13/2020
ms.custom: seodec18
ms.openlocfilehash: 5e8ea56de84f3d6b92bc23c0e3ab139d03bd68a9
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86232262"
---
# <a name="what-is-azure-time-series-insights-preview"></a>Azure Time Series Insights Önizleme nedir?

Azure Time Series Insights önizlemesi, uçtan uca bir hizmet olarak platform (PaaS) teklifiyle yapılır. Bu özelliği, Nesnelerin İnterneti (IoT) ölçekli verileri toplamak, işlemek, depolamak, analiz etmek ve sorgulamak için kullanabilirsiniz. Bu, zaman serisi için yüksek düzeyde olan ve en iyileştirilmiş veriler. 

Time Series Insights, geçici veri araştırması ve işletimsel analizler için tasarlanmıştır. Endüstriyel IoT dağıtımlarının geniş ihtiyaçlarını karşılayan genişletilebilir ve özelleştirilmiş bir hizmet sunumudur.

## <a name="video"></a>Video

Azure Time Series Insights önizleme hakkında daha fazla bilgi edinin.

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Time-Series-Insights-e2e-solution-for-industrial-IoT-analytics/player]

## <a name="definition-of-iot-data"></a>IoT verilerinin tanımı

Sektör açısından yoğun kuruluşlarda endüstriyel IoT verileri genellikle endüstriyel bir ayarda cihazların ve algılayıcıların farklılaştırmasından dolayı yapısal tutarlılık içermemekte değildir. Bu akışların verileri, önemli boşluklar ve bazen bozulmuş iletiler ve yanlış okumalar ile belirlenir. IoT verileri genellikle ilk taraf veya üçüncü kaynaklardan (örneğin, uçtan uca iş akışları için bağlam ekleyen) gelen ek veri girişlerinin bağlamında anlamlıdır. Hava durumu verileri gibi üçüncü taraf veri kaynaklarından gelen girişler, belirli bir yüklemede telemetri akışlarının sağlanmasına yardımcı olabilir. 

Tüm bu işlemler, yalnızca işletimsel ve iş amaçları için verilerin bir bölümü için kullanılır ve analizler için contextuselleştirilmesi gerekir. Endüstriyel veriler genellikle daha uzun süre içinde daha fazla bilgi sahibi olmak için historicized eğilimleri anlamak ve eğilimleri ilişkilendirmek için kullanılır. Toplanan IoT verilerini eyleme dönüştürülebilir içgörüler halinde açmak şunları gerektirir: 

* Verilerin çözümlenmesi, filtreleneceği, enterpola, dönüştürülemesinde ve analiz amacıyla hazırlanması için veri işleme.
* Verileri normalleştirerek, diğer bir deyişle verileri normalleştirime ve anlayan bir yapı.
* İşlenen (veya türetilen) verilerin ve ham verilerin uzun veya sonsuz tutulması için ekonomik depolama.

Bu veriler, iş analizi ve raporlama için tutarlı, kapsamlı, güncel ve doğru bilgiler sağlar.

Aşağıdaki görüntüde tipik bir IoT veri akışı gösterilmektedir.

[![IoT veri akışı](media/v2-update-overview/overview-one.png)](media/v2-update-overview/overview-one.png#lightbox)

## <a name="azure-time-series-insights-for-industrial-iot"></a>Endüstriyel IoT için Azure Time Series Insights

IoT yatay, üretim, oto ve enerji, yardımcı programlar, akıllı binalar ve danışmanlık gibi çeşitli sektör segmentlerini kapsayan müşterilerle aynıdır. Büyük ölçekli IoT verilerine hedeflenmiş kapsamlı analizler sağlayan, bulut tabanlı bu çok sayıda endüstriyel IoT Market genelinde gelişmeye devam etmektedir. 

Azure Time Series Insights, zaman serisi verilerinin, varlık tabanlı öngörülerin ve bulma, trma, anomali algılama ve operasyonel zeka için en iyi kullanıcı deneyiminin yerleşik olarak belirlenmesi için zengin anlam modellemesi sunarak bu Pazar gereksinimini ortadan kaldırabilirsiniz. 

Etkileşimli veri araştırma olanaklarımızla birleştirilmiş bir zengin işletimsel analiz platformu, IoT varlıklarından toplanan verilerden daha fazla değer elde etmek için Time Series Insights kullanabilirsiniz. Önizleme teklifi şunları destekler: 

* Sıcak ve soğuk analiz desteğiyle çok katmanlı depolama çözümü, müşterilere, normal veriler üzerinde etkileşimli analizler ve geçmiş verilerin deklikler üzerinde işletimsel zeka 'lar arasında veri yönlendirme seçeneği sağlar. 

    *    Sık gerçekleştirilen ve kısa süre içinde çok sayıda sorgu daha kısa sürede gerçekleştirmeye yönelik son derece etkileşimli bir sıcak analiz çözümü 
    *    Azure depolama 'yı temel alan ölçeklenebilir, performansı en iyi duruma getirilmiş bir zaman serisi veri Gölü ve müşterilerin, zaman serisi verilerinin Saniyeler içinde eğilimini eğilim altına almalarına olanak tanır. 

* Varlıkların ve cihazların türetilmiş ve ham sinyalleriyle ilişkili etki alanını ve meta verileri açıklayan anlam modeli desteği.

* Müşterinin sahip olduğu Azure depolama hesabında geçmiş zaman serisi verilerini depolamak için esnek analiz platformu, böylelikle müşterilerin IoT verilerinin sahipliğine sahip olmasını sağlar. Veriler, tahmine dayalı analiz, makine öğrenimi gibi çeşitli veri senaryolarında bağlantı ve birlikte çalışabilme ve Spark, Databricks ve Jupyıter gibi tanıdık teknolojiler kullanılarak gerçekleştirilen diğer özel hesaplamalar arasında bağlantı ve birlikte çalışabilirliği sağlayan açık kaynaklı Apache Parquet biçiminde depolanır.

* Zengin çözünürlüklü, ölçeklenebilir sorgu API 'Leri ve Kullanıcı deneyimi ile varlık tabanlı veri içgörüleri birleştiren zengin analizler; enterpolasyon, skaler ve toplama işlevleri, kategorik değişkenler, dağılım çizimleri ve ayrıntılı analiz için zaman serisi sinyalleriyle ilgili zaman dizisi sinyalleri için destek sunar.

*    Kurumsal IoT müşterilerimiz için ölçek, performans, güvenlik ve güvenilirlik ihtiyaçlarını destekleyecek kurumsal sınıf platformu.

* Uçtan uca analiz için genişletilebilirlik ve tümleştirme desteği. Time Series Insights çeşitli veri senaryoları için genişletilebilir bir analiz platformu sağlar. Time Series Insights Power BI Bağlayıcısı, müşterilerin tek bir cam bölmesinde bı ve zaman serisi analizlerinin Birleşik görünümünü almak için doğrudan Power BI Time Series Insights yaptıkları sorguları almasını sağlar.

Aşağıdaki diyagramda, üst düzey veri akışı gösterilmektedir.

  [![Temel işlevler](media/v2-update-overview/overview-two.png)](media/v2-update-overview/overview-two.png#lightbox)

Azure Time Series Insights, veri işleme, depolama (veri ve meta veriler) ve sorgu için, müşterilerin kullanımını iş taleplerine uyacak şekilde ayarlamanıza olanak tanıyan ölçeklenebilir bir Kullandıkça Öde fiyatlandırma modeli sunar. 
 
Bu temel Endüstriyel IoT özelliklerine giriş ile Time Series Insights, aşağıdaki başlıca avantajları da sağlar.  

| Yetenek | Avantaj |
| ---| ---|
| IoT ölçeğinde zaman serisi verileri için çok katmanlı depolama | Verilerin içe aktarılmasıyla ilgili bir paylaşılan veri işleme işlem hattı sayesinde, verileri hem normal hem de soğuk depolara aktarabilirsiniz. Büyük hacimli verileri depolamak için etkileşimli sorgular ve soğuk mağaza için ısınma mağazasını kullanın. Yüksek performanslı varlık tabanlı sorgulardan nasıl yararlanabilmeniz hakkında daha fazla bilgi için bkz. [sorgular](./concepts-query-overview.md). |
| Ham telemetri eklemek ve varlık tabanlı Öngörüler türetmeye yönelik zaman serisi modeli | Tarih serisi modelini, zaman serisi verileriniz için örnekler, hiyerarşiler, türler ve değişkenler oluşturmak için kullanabilirsiniz. Zaman serisi modeli hakkında daha fazla bilgi edinmek için bkz. [zaman serisi modeli](./concepts-model-overview.md).  |
| Diğer veri çözümleriyle sorunsuz ve sürekli tümleştirme | Time Series Insights soğuk depodaki veriler, açık kaynaklı Apache Parquet dosyalarında [depolanır](concepts-storage.md) . Bu, iş zekası, gelişmiş makine öğrenimi ve tahmine dayalı analiz gibi senaryolar için diğer veri çözümleri, 1 veya 3. taraf ile veri tümleştirmeyi sağlar. |
| Neredeyse gerçek zamanlı veri araştırması | [Azure Time Series Insights Preview Explorer](./time-series-insights-update-explorer.md) Kullanıcı deneyimi, alma işlem hattı aracılığıyla tüm veri akışı için görselleştirme sağlar. Bir olay kaynağını bağlandıktan sonra olay verilerini görüntüleyebilir, keşfedebilir ve sorgulayabilirsiniz. Bu şekilde, bir cihazın verileri beklendiği gibi yayıp yaymadığını doğrulayabilirsiniz. Ayrıca, bir IoT varlığını sistem durumu, üretkenlik ve genel verimlilik için izleyebilirsiniz. | 
| Genişletilebilirlik ve Tümleştirme | Azure Time Series Insights Power BI Bağlayıcısı tümleştirmesi, **dışa aktarma** seçeneği aracılığıyla doğrudan zaman serisi Gezgin Kullanıcı deneyiminde kullanılabilir ve müşterilerin kendi Kullanıcı deneyimimizde oluşturdukları zaman serisi sorgularını, diğer bı analizlerine göre doğrudan Power BI masaüstüne dışarı aktarmalarına izin verir. Bu, IoT zaman serisi dahil çeşitli veri kaynaklarından analizler üzerinde tek bir cam bölmesi sunarak Power BI yatırım yapan endüstriyel IoT kuruluşları için bir yeni senaryolar sınıfına açar. | 
| Time Series Insights platformunda oluşturulan özel uygulamalar | Time Series Insights [JavaScript SDK 'sını](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)destekler. SDK, sorgulara zengin denetimler ve Basitleştirilmiş erişim sağlar. İş gereksinimlerinize uyacak şekilde Time Series Insights en üstünde özel IoT uygulamaları oluşturmak için SDK 'Yı kullanın. Ayrıca, verileri özel IoT uygulamalarına yönlendirmek için doğrudan Time Series Insights [sorgu API 'lerini](./concepts-query-overview.md) de kullanabilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Time Series Insights Preview ile çalışmaya başlama:

> [!div class="nextstepaction"]
> [Hızlı başlangıç kılavuzu](./time-series-insights-update-quickstart.md)

Kullanım örnekleri hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Azure Time Series Insights Önizleme kullanım örnekleri](./time-series-insights-update-use-cases.md)