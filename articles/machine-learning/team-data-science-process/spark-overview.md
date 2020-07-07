---
title: Azure HDInsight 'ta Spark kullanan veri bilimi-ekip veri bilimi Işlemi
description: Spark MLlib araç seti, dağıtılmış HDInsight ortamına önemli ölçüde makine öğrenimi modelleme özellikleri sunar.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 64caa1228cd073358bef496721c22b17554031d3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82189292"
---
# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Azure HDInsight 'ta Spark kullanarak veri bilimine genel bakış

Bu konu grubu, veri alma, özellik Mühendisliği, modelleme ve model değerlendirmesi gibi yaygın veri bilimi görevlerini tamamlamaya yönelik HDInsight Spark 'ın nasıl kullanılacağını gösterir. Kullanılan veriler 2013 NYC TAXI seyahat ve tarifeli havayolu veri kümesinin bir örneğidir. Oluşturulan modeller lojistik ve doğrusal regresyon, rastgele ormanlar ve gradyan artırılmış ağaçlar içerir. Konular Ayrıca bu modellerin Azure Blob Storage 'da nasıl depolanacağını ve tahmine dayalı performanslarını nasıl değerlendileceğini ve değerlendirileceğini gösterir. Daha gelişmiş konularda modellerin çapraz doğrulama ve hiper parametre kullanımı kullanılarak nasıl eğitilmiş olduğu ele alınmaktadır. Bu genel bakış konusu Ayrıca, sunulan izlenecek yollarda bulunan adımları tamamlayabilmeniz için ihtiyaç duyduğunuz Spark kümesini ayarlamayı anlatan konulara başvurur.

## <a name="spark-and-mllib"></a>Spark ve MLlib
[Spark](https://spark.apache.org/) , büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen açık kaynaklı bir paralel işleme çerçevesidir. Spark işleme altyapısı hız, kullanım kolaylığı ve gelişmiş analiz için oluşturulmuştur. Spark 'ın bellek içi dağıtılmış hesaplama özellikleri, makine öğrenimi ve grafik hesaplamaları 'nda kullanılan yinelemeli algoritmalar için iyi bir seçenek yapar. [Mllib](https://spark.apache.org/mllib/) , algoritmik modelleme yeteneklerini bu dağıtılmış ortama getiren, Spark 'ın ölçeklenebilir makine öğrenimi kitaplığıdır.

## <a name="hdinsight-spark"></a>HDInsight Spark
[HDInsight Spark](../../hdinsight/spark/apache-spark-overview.md) , Azure 'da barındırılan ve açık kaynaklı Spark sunumudur. Ayrıca Spark kümesinde bulunan **jupi pyspark Not defterleri** için destek Içerir ve Azure Bloblarında depolanan verileri dönüştürmek, filtrelemek ve görselleştirmek IÇIN Spark SQL etkileşimli sorgularını çalıştırabilir (edb). PySpark, Spark için Python API 'sidir. Çözümleri sağlayan kod parçacıkları ve buradaki verileri görselleştirmek için Spark kümelerine yüklenen jupi not defterlerinde çalıştırılan ilgili çizimleri gösterir. Bu konulardaki modelleme adımları, her model türünü eğitme, değerlendirme, kaydetme ve tüketme şeklini gösteren kodu içerir.

## <a name="setup-spark-clusters-and-jupyter-notebooks"></a>Kurulum: Spark kümeleri ve Jupyter Not defterleri
Bu izlenecek yolda, HDInsight Spark 1,6 kullanımı için kurulum adımları ve kodu sunulmaktadır. Ancak, hem HDInsight Spark 1,6 hem de Spark 2,0 kümelerinde Jupyıter Not defterleri sağlanır. Not defterlerinin ve bunlara bağlantıların açıklaması, bunları içeren GitHub deposu için [README.MD](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) içinde verilmiştir. Üstelik, burada ve bağlantılı Not defterlerindeki kod geneldir ve tüm Spark kümeleri üzerinde çalışmalıdır. HDInsight Spark kullanmıyorsanız, küme kurulumu ve yönetim adımları burada gösterilenden biraz farklı olabilir. Kolaylık olması için, Spark 1,6 için Jupyter Notebook (Jupyter Notebook sunucusunun pySpark çekirdeğindedir) ve Spark 2,0 (Jupyter Notebook sunucusunun pySpark3 çekirdeğinde çalıştırılacak şekilde) bağlantıları aşağıda verilmiştir:

### <a name="spark-16-notebooks"></a>Spark 1,6 Not defterleri
Bu not defterleri Jupyter Not defteri sunucusunun pySpark çekirdeğinde çalıştırılır.

- [Pyspark-Machine-Learning-Data-Science-Spark-Data-araştırmayla-Modellendirme. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): birçok farklı algoritmayla veri keşif, modelleme ve Puanlama gerçekleştirme hakkında bilgi sağlar.
- [Pyspark-Machine-Learning-Data-Science-Spark-gelişmiş-veri-araştırma-modelleme. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): not defteri #1 konuları ve hiper parametre ayarlama ve çapraz doğrulama kullanarak model geliştirmeyi içerir.
- [Pyspark-Machine-Learning-Data-Science-Spark-model-tüketim. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb): HDInsight kümelerinde Python kullanarak kaydedilmiş bir modeli nasıl kullanıma sunmayı gösterir.

### <a name="spark-20-notebooks"></a>Spark 2,0 Not defterleri
Bu not defterleri, Jupyter Not defteri sunucusunun pySpark3 çekirdeğinde çalıştırılır.

- [Spark 2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-araştırması-Modellendirme. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Bu dosya, Spark 2,0 kümelerinde, [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data)açıklanan NYC Taxi seyahat ve tarifeli havayolu veri kümesini kullanarak veri keşif, modelleme ve Puanlama gerçekleştirme hakkında bilgi sağlar. Bu not defteri, Spark 2,0 için sağladığımız kodu hızlı bir şekilde keşfetmek için iyi bir başlangıç noktası olabilir. Daha ayrıntılı bir not defteri, NYC TAXI verilerini analiz etmek için bu listedeki bir sonraki not defterine bakın. Bu not defterlerini karşılaştıran bu listenin altındaki notlara bakın.
- [Spark 2.0-pySpark3_NYC_Taxi_Tip_Regression. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Bu dosya, [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data)açıklanan NYC TAXI seyahat ve tarifeli havayolu veri kümesini kullanarak veri denetimi (Spark SQL ve dataframe işlemleri), araştırma, modelleme ve Puanlama gerçekleştirme işlemini gösterir.
- [Spark 2.0-pySpark3_Airline_Departure_Delay_Classification. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Bu dosya, 2011 ve 2012 ' den iyi bilinen Airline ile veri kümesini kullanarak veri denetimi (Spark SQL ve dataframe işlemleri), araştırma, modelleme ve Puanlama gerçekleştirme işlemini gösterir. Hava yolu veri kümesini modellemeye başlamadan önce Havaalanı hava durumu verileriyle tümleştirdik (örneğin, wınvs hızı, sıcaklık, yükseklik vb.), bu hava durumu özellikleri modele dahil edilebilir.

<!-- -->

> [!NOTE]
> , Sınıflandırma algoritmalarının kullanımını daha iyi göstermek için Spark 2,0 not defterlerine hava yolu veri kümesi eklenmiştir. Uçak veri kümesi ve hava durumu veri kümesi hakkında bilgi edinmek için aşağıdaki bağlantılara bakın:
> 
> - Airline, zamanında ayrılma verileri:[https://www.transtats.bts.gov/ONTIME/](https://www.transtats.bts.gov/ONTIME/)
> 
> - Havaalanı hava durumu verileri:[https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)

<!-- -->

<!-- -->

> [!NOTE]
> NYC TAXI ve hava yolu Uçuş gecikmesi veri kümelerindeki Spark 2,0 Not defterleri, çalıştırmak için 10 dakika veya daha fazla sürebilir (HDI kümenizin boyutuna bağlı olarak). Yukarıdaki listede yer aldığı ilk not defteri, veri gezgininin birçok yönünü gösterir. TAXI ve tarifeli havayolu dosyalarının önceden katıldığı, alt örneklenmiş NYC veri kümesiyle çalıştırılması daha az zaman alan bir not defterinde görselleştirme ve ML modeli eğitimi: [Spark 2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-araştırmayı-Modellendirme. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb). Bu Not defterinin tamamlanmasının daha kısa sürmesi (2-3 dakika) ve Spark 2,0 için sağladığımız kodu hızlı bir şekilde keşfetmek için iyi bir başlangıç noktası olabilir.

<!-- -->

Bir Spark 2,0 modelinin ve Puanlama için model tüketiminin kullanıma hazır hale getirme hakkında rehberlik için, gerekli adımları bir örnek olarak göstermek üzere [Tüketim üzerinde Spark 1,6 belgesine](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) bakın. Bu örneği Spark 2,0 ' de kullanmak için Python kod dosyasını [Bu dosyayla](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py)değiştirin.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yordamlar Spark 1,6 ile ilgilidir. Spark 2,0 sürümü için, yukarıda açıklanan ve daha önce bağlanılan not defterlerini kullanın.

1. Bir Azure aboneliğiniz olmalıdır. Henüz bir tane yoksa, bkz. [Azure Ücretsiz deneme sürümü](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. Bu izlenecek yolu tamamlamak için bir Spark 1,6 kümesine ihtiyacınız vardır. Bir tane oluşturmak için bkz. [Başlarken: Azure HDInsight üzerinde Apache Spark oluşturma](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Küme türü ve sürümü, **küme türü seç** menüsünden belirtilmiştir.

![Kümeyi Yapılandır](./media/spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [!NOTE]
> Uçtan uca bir veri bilimi işleminin görevlerini tamamlaması için Python yerine Scala 'nın nasıl kullanılacağını gösteren bir konu için bkz. [Azure 'Da Spark Ile Scala kullanarak veri bilimi](scala-walkthrough.md).
>
>

<!-- -->

> [!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]
>
>

## <a name="the-nyc-2013-taxi-data"></a>NYC 2013 TAXI verileri
NYC TAXI seyahat verileri, 173.000.000 ' den fazla ayrı ve her seyahat için ödenen farktan fazla 20 GB 'lık sıkıştırılmış virgülle ayrılmış değerler (CSV) dosyası (~ 48 GB sıkıştırılmamış) ile ilgilidir. Her seyahat kaydı, toplama ve bırakma konumunu ve saatini, anonimleştirilmiş Hack (sürücü) lisans numarasını ve medalon (TAXI 'nin benzersiz kimliği) numarasını içerir. Veriler, 2013 yılında yapılan tüm döngüleri kapsamakta ve her ay için aşağıdaki iki veri kümelerinde sunulmaktadır:

1. ' Trip_data ' CSV dosyaları, pasmacılar sayısı, alma ve çıkarma noktaları, seyahat süresi ve seyahat uzunluğu gibi seyahat ayrıntılarını içerir. Aşağıda birkaç örnek kayıt verilmiştir:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. ' Trip_fare ' CSV dosyaları, ödeme türü, tarifeli havayolu miktarı, ek ücret, vergiler, ipuçları ve Tolls ve ödenen toplam miktar gibi her seyahat için ödenen tarifeli havayolu ayrıntılarını içerir. Aşağıda birkaç örnek kayıt verilmiştir:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Bu dosyaların% 0,1 bir örneğini aldık ve seyahat \_ verilerini ve seyahat \_ tarifeli havayolu CVS dosyalarını Bu izlenecek yol için giriş veri kümesi olarak kullanılacak tek bir veri kümesine katılmış. Seyahat \_ verilerine ve seyahat tarifeli havayolu katılacak benzersiz anahtar \_ alanlardan oluşur: medtalon, Hack \_ Lisans ve Pickup \_ DateTime. Veri kümesinin her kaydı, bir NYC Vergileni seyahati temsil eden aşağıdaki öznitelikleri içerir:

| Alan | Kısa açıklama |
| --- | --- |
| medalon |Anonimleştirilmiş TAXI medalon (benzersiz TAXI kimliği) |
| hack_license |Anonimleştirilmiş Hackney No lisans numarası |
| vendor_id |Taxı satıcı kimliği |
| rate_code |NYC TAXI tarifeli havayolu oranı |
| store_and_fwd_flag |Depola ve ilet bayrağı |
| pickup_datetime |Tarihi & zamanı seçin |
| dropoff_datetime |Açılan Tarih & zamanı |
| pickup_hour |Saati seçin |
| pickup_week |Yılın haftasını seçin |
| HAFTANINGÜNÜ |Hafta içi (Aralık 1-7) |
| passenger_count |Bir taxm yolculuğuna ilişkin pascuların sayısı |
| trip_time_in_secs |Saniye cinsinden seyahat süresi |
| trip_distance |Mil cinsinden seyahat uzaklığı |
| pickup_longitude |Boylam 'yi seçin |
| pickup_latitude |Enlem 'yi seçin |
| dropoff_longitude |Boylam |
| dropoff_latitude |Daha açılan Enlem |
| direct_distance |Toplama ve açılan konumlar arasındaki doğrudan uzaklık |
| payment_type |Ödeme türü (nakit, kredi kartı vb.) |
| fare_amount |Tarifeli havayolu miktar |
| ek maliyeti |Ek maliyeti |
| mta_tax |MTA metro ulaşım vergisi |
| tip_amount |İpucu miktarı |
| tolls_amount |Tolls miktarı |
| total_amount |Toplam tutar |
| eğik |Eğimli (Hayır veya Evet için 0/1) |
| tip_class |İpucu sınıfı (0: $0, 1: $0-5, 2: $6-10, 3: $11-20, 4: > $20) |

## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Spark kümesindeki bir Jupyter Not defterinden kod yürütme
Jupyter Notebook Azure portal başlatabilirsiniz. Panonuzda Spark kümenizi bulun ve kümenizin yönetim sayfasına girmek için tıklayın. Spark kümesiyle ilişkili Not defterini açmak için Jupyter Notebook **küme panoları**' na tıklayın  ->  **Jupyter Notebook**.

![Küme panoları](./media/spark-overview/spark-jupyter-on-portal.png)

Ayrıca ***`https://CLUSTERNAME.azurehdinsight.net/jupyter`*** , jupi not defterlerine erişmek için öğesine de gidebilirsiniz. Bu URL 'nin CLUSTERNAME bölümünü kendi kümenizin adıyla değiştirin. Not defterlerine erişmek için Yönetici hesabınızın parolasının olması gerekir.

![Jupyıter not defterlerine gözatıyorum](./media/spark-overview/spark-jupyter-notebook.png)

PySpark API kullanan önceden paketlenmiş not defterlerine birkaç örnek içeren bir dizini görmek için PySpark ' ı seçin. Bu Spark konusunun kod örneklerini içeren Not defterleri [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) 'da kullanılabilir

Not defterlerini doğrudan [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) 'dan Spark kümenizdeki Jupyter Not defteri sunucusuna yükleyebilirsiniz. Jupi 'nizin giriş sayfasında, ekranın sağ tarafındaki **karşıya yükle** düğmesine tıklayın. Bir dosya Gezgini açar. Burada, Not defterinin GitHub (ham içerik) URL 'sini yapıştırabilir ve **Aç**' a tıklayabilirsiniz.

Dosya adını Jupyıter dosya listenizde bir kez **karşıya yükle** düğmesine yeniden görürsünüz. Bu **karşıya yükle** düğmesine tıklayın. Artık Not defterini içeri aktardınız. Bu yönergeden diğer not defterlerini karşıya yüklemek için bu adımları tekrarlayın.

> [!TIP]
> Tarayıcınızdaki bağlantıları sağ tıklatıp GitHub ham içerik URL 'sini almak için **Bağlantıyı Kopyala** ' yı seçebilirsiniz. Bu URL 'YI Jupyıter karşıya yükleme dosya Gezgini iletişim kutusuna yapıştırabilirsiniz.
> 
> 

Artık şunları yapabilirsiniz:

* Not defterine tıklayarak koda bakın.
* Her hücreyi **SHIFT-enter**tuşlarına basarak yürütün.
* Tüm Not defteri ' ni tıklatarak **hücre**  ->  **Çalıştır**' a tıklayın.
* Sorguların otomatik görselleştirilmesini kullanın.

> [!TIP]
> PySpark çekirdeği, SQL (HiveQL) sorgularının çıkışını otomatik olarak görselleştirir. Not defterindeki **tür** menü düğmelerini kullanarak birkaç farklı görselleştirme türü (tablo, pasta, çizgi, alan veya çubuk) arasından seçim yapabilirsiniz:
>
>

![Genel yaklaşım için lojistik regresyon ROC eğrisi](./media/spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Sırada ne var?
Bir HDInsight Spark kümesiyle ayarlandığınıza ve Jupyıter not defterlerini karşıya yüklediğinize göre, bu üç PySpark not defterlerine karşılık gelen konularda çalışmaya hazırsınız demektir. Bunlar, verilerinizi nasıl araştırıp daha sonra modeller oluşturma ve kullanma işlemlerinin nasıl yapılacağını gösterir. Gelişmiş veri araştırması ve modelleme not defteri, çapraz doğrulama, hiper parametre kullanımı ve model değerlendirmesinin nasıl ekleneceğini gösterir.

**Spark Ile veri araştırması ve modelleme:** [Spark MLlib araç seti ile veri için ikili sınıflandırma ve regresyon modelleri oluşturma](spark-data-exploration-modeling.md) konusunun veri kümesini İnceleme ve makine öğrenimi modelleri oluşturma, Puanlama ve değerlendirme.

**Model tüketimi:** Bu konuda oluşturulan sınıflandırma ve regresyon modellerinin nasıl puan alabileceğinizi öğrenmek için bkz. [Spark ile oluşturulmuş makine öğrenimi modellerini Puanlama ve değerlendirme](spark-model-consumption.md).

**Çapraz doğrulama ve hiper parametre**kullanımı: modellerin çapraz doğrulama ve hiper parametre tatkiyle nasıl eğitilmesine ilişkin [Spark ile gelişmiş veri araştırması ve modellemeye](spark-advanced-data-exploration-modeling.md) bakın

