---
title: Azure Cosmos DB ile yerleşik analizler için kullanım örnekleri.
description: Azure Cosmos DB ile farklı kullanım durumlarında yerleşik analitiği nasıl kullanacağınızı öğrenin.
author: markjbrown
ms.author: mjbrown
ms.topic: conceptual
ms.service: cosmos-db
ms.date: 09/26/2019
ms.reviewer: sngun
ms.openlocfilehash: adf1db2d737de7004b5ab71c8e0e44d61d1da8c9
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81768507"
---
# <a name="use-cases-for-built-in-analytics-with-azure-cosmos-db"></a>Azure Cosmos DB ile yerleşik analizler için kullanım örnekleri

Aşağıdaki kullanım örnekleri, Azure Cosmos DB'de Apache Spark ile yerleşik analiz kullanılarak elde edilebilir:

## <a name="htap-scenarios"></a>HTAP senaryoları

Azure Cosmos DB'deki yerleşik analizler şu şekilde kullanılabilir:

* İşlem işlemleri sırasında dolandırıcılığı algıla.
* Bir E-Ticaret mağazasına göz atarken alışveriş yapanlara öneriler sağlayın.
* Operatörleri kritik bir üretim ekipmanıparçasının yaklaşan arızalarına karşı uyarın.
* Gerçek zamanlı analizleri doğrudan operasyonel verilere katıştırarak hızlı ve işlem uygulanabilir bilgiler oluşturun.

Azure Cosmos DB, yerel olarak yerleşik Apache Spark'ı kullanarak Karma İşlemsel/Analitik İşlemleme (HTAP) senaryolarını destekler. Azure Cosmos DB, geleneksel sistemlerle birlikte gelen operasyonel ve analitik ayrımı kaldırır.

## <a name="globally-distributed-data-lake-without-requiring-any-etl"></a>Herhangi bir ETL gerektirmeden küresel olarak dağıtılan veri gölü

Azure Cosmos DB, yerel olarak yerleşik Apache Spark ile, tek bir sistem görüntüsü sağlayan küresel olarak dağıtılmış bir veri gölü oluşturmak için hızlı, basit ve ölçeklenebilir bir yol sağlar. Küresel olarak dağıtılan çok modelli veriler, pahalı ETL boru hatlarına yatırım yapma ihtiyacını ortadan kaldırarak, veri ekiplerinin veri kümelerini analiz etme şeklinde devrim çıkarır.

## <a name="time-series-analytics-over-historic-data"></a>Geçmiş veriler üzerinde zaman serisi analitiği

Bazı durumlarda, geçmişte tamamlanan olaylar üzerinde belirli bir zamanda olduğu gibi verilere dayalı soruları yanıtlamanız gerekebilir. Örneğin, belirli bir tarihte CRM etkinlik durumlarının sayısını almak için. Raporu bir hafta önce çalıştırdıysanız, durum sayısı, o anda her etkinliğin durumlarına göre olacaktır. Bugün aynı raporu çalıştırmak size durumları bugünkü gibi olan faaliyetlerin sayısını verecektir, geçen haftadan beri değişmiş olabilir, çünkü yaşam döngülerini açıktan kapanışa kadar devam ederler. Yani, davanın yaşam döngüsünün her aşamasında anlık rapor gerekir.

Azure Cosmos DB ile kullanıcılar, zaman yolculuğu kavramını uygulayarak, veriler üzerinde geçmişe dönük olarak analiz sorgulayıp çalıştırabilme ve verilerin tarihteki belirli bir zaman noktasına nasıl baktığını sorabilme olana sahiptir. Bu, kullanıcıların verilerin hem geçerli hem de tarihi görünümlerini kolayca görüntüleyebileceği ve üzerinde analiz çalıştırabileceği anlamına gelir.

## <a name="globally-distributed-machine-learning-and-ai"></a>Küresel olarak dağıtılan makine öğrenimi ve Yapay AI

İşletmeler hızla büyüyen veri hacimleri ve genişleyen veri türleri ve biçimleri ile uğraşırken, dünya çapında petabayt ölçeğinde daha derin ve daha doğru öngörüler elde etme yeteneği neredeyse imkansız hale gelir. Azure Cosmos DB, yerel olarak yerleşik Apache Spark ile kapsamlı makine öğrenimi algoritmaları kitaplığı sunan küresel olarak dağıtılmış bir analiz platformu sağlar. Modelleri ve küme yönetimi özelliklerini oluşturmak ve eğitmek için etkileşimli Jupyter dizüstü bilgisayarları kullanabilirsiniz. Bu özellikler, isteğe bağlı olarak son derece ayarlanmış ve otomatik elastik Spark kümeleri sağlamanıza olanak tanır.

## <a name="deep-learning-on-multi-model-globally-distributed-data"></a>Çok modelli küresel olarak dağıtılmış veriler de Derin Öğrenme

Derin öğrenme, veri hacmi ve karmaşıklığı artmaya devam ettikçe büyük veri tahmine dayalı analitik çözümleri sağlamanın ideal yoludur. Derin öğrenme ile işletmeler, ai, doğal dil işleme ve daha fazlası gibi tekniklerden yararlanan kullanım durumlarını sunmak için yapılandırılmamış ve yarı yapılandırılmış verilerin gücünden yararlanabilirler.

## <a name="reporting-integrating-with-power-apps-power-bi"></a>Raporlama (Power Apps, Power BI ile entegre)

Power BI, son kullanıcıların kendi başlarına rapor ve pano lar oluşturmalarını sağlayarak self servis iş zekası özellikleriyle etkileşimli görselleştirmeler sağlar. Yerleşik Spark konektörünü kullanarak Power BI Desktop'ı Azure Cosmos DB'deki Apache Spark kümelerine bağlayabilirsiniz. Bu bağlayıcı, Azure Cosmos DB'deki Apache Spark'a işlemeyi boşaltmak için doğrudan sorgu kullanmanızı sağlar, bu da Power BI'ye yüklemek istemediğiniz büyük miktarda veriye sahip olduğunuzda veya gerçek zamanlı analize yakın bir şekilde gerçekleştirmek istediğinizde harikadır.

## <a name="iot-analytics-at-global-scale"></a>Küresel ölçekte IoT analitiği

Artan ağ sensörlerinden elde edilen veriler, daha önce opak sistemlere ve süreçlere benzersiz bir görünürlük sağlar. Anahtar, IoT cihazlarının dünya çapında nerede dağıtıldıklarına bakılmaksızın bu bilgi torrentinde eyleme geçirilebilir öngörüler bulmaktır. Azure Cosmos DB, IOT şirketlerinin dünyanın her yerinde yüksek hızlı sensörü ve zaman serisi verilerini gerçek zamanlı olarak analiz etmesine olanak tanır. Gelişmiş müşteri deneyimleri, operasyonel verimlilikler ve yeni gelir fırsatları sunmak için birbirine bağlı bir dünyanın gerçek değerini kullanmanızı sağlar.

## <a name="stream-processing-and-event-analytics"></a>Akış işleme ve olay analizi 

Günlük dosyalarından sensör verilerine kadar, işletmeler giderek daha fazla veri "akışları" ile başa çıkmak için ihtiyaç var. Bu veriler, genellikle aynı anda birden çok kaynaktan sabit bir akışla gelir. Bu veri akışlarını diskte depolamak ve geriye dönük olarak analiz etmek mümkün olsa da, verileri geldiğinde işlemek ve üzerinde işlem yapmak bazen mantıklı veya önemli olabilir. Örneğin, finansal işlemlerle ilgili veri akışları, potansiyel olarak sahte hareketleri tanımlamak ve reddetmek için gerçek zamanlı olarak işlenebilir.

## <a name="interactive-analytics"></a>İnteraktif analitik

Satış, üretkenlik veya hisse senedi fiyatları için statik panolar oluşturmak için önceden tanımlanmış sorguları çalıştırmanın yanı sıra, verileri etkileşimli olarak keşfetmek isteyebilirsiniz. Etkileşimli analitik, soru sormanızı, sonuçları görüntülemenize, yanıta dayalı olarak ilk soruyu değiştirmenize veya sonuçlara daha derinlemesine girmenize olanak sağlar. Azure Cosmos DB'deki Apache Spark, hızlı bir şekilde yanıt vererek ve uyarlayarak etkileşimli sorguları destekler.

## <a name="data-exploration-using-jupyter-notebooks"></a>Jupyter dizüstü bilgisayarları kullanarak veri arama

Yeni bir veri setiniz olduğunda, çalışan modellere ve testlere dalmadan önce verilerinizi incelemeniz gerekir. Başka bir deyişle, araştırmacı veri çözümlemesi gerçekleştirmeniz gerekir. Veri arama çeşitli kararları bilgilendirebilir. Örneğin, verilerinizde kullanımı uygun yöntemler, verilerin belirli modelleme varsayımlarını karşılayıp karşılamadığı, verilerin temizlenmesi, yeniden yapılandırılması vb. gibi ayrıntıları bulabilirsiniz. Azure Cosmos DB'nin yerel olarak yerleşik Jupyter dizüstü bilgisayarlarını ve Apache Spark'ı kullanarak, işlemsel ve analitik veriler üzerinde hızlı ve etkili araştırmacı veri analizi yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere gidin bu kullanım örnekleri ile başlamak için:

* [Azure Cosmos DB'de yerleşik Jupyter dizüstü bilgisayarlar](cosmosdb-jupyter-notebooks.md)
* [Azure Cosmos hesapları için not defterleri etkinleştirme](enable-notebooks.md)
* [Verileri analiz etmek ve görselleştirmek için not defteri oluşturma](create-notebook-visualize-data.md)