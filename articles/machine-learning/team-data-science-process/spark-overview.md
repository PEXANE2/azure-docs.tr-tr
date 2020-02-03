---
title: Azure HDInsight - Team Data Science Process Spark kullanan veri bilimi
description: Önemli makine öğrenimi modelleme özellikleri dağıtılmış HDInsight ortamına Spark MLlib Araç Seti sunar.
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
ms.openlocfilehash: 63148b99e65a5ccc49d54d4ae6c58adebc72c6d3
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76718523"
---
# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Azure HDInsight üzerinde Spark kullanan veri bilimi genel bakış

Bu paketi konuları veri alımı, özellik Mühendisliği, modelleme ve model değerlendirme gibi genel veri bilimi görevlerini tamamlamak için HDInsight Spark kullanmayı gösterir. Kullanılan veri kümesinin 2013 NYC taksi seyahat ve taksi bir örnektir. Mantıksal ve doğrusal regresyon, rastgele ormanları ve gradyan artırmalı ağaçları modellerinin içerir. Konular, Azure blob depolama (WASB) şu modelleri depolamak ve puan ve Tahmine dayalı performanslarını değerlendirmek de gösterir. Daha gelişmiş konular ele modelleri nasıl olabileceğini çapraz doğrulama ve hiper parametreli Süpürme kullanarak eğitilir. Bu genel bakış konusu, ayrıca sağlanan izlenecek adımları tamamlamak için gereken bir Spark kümesi oluşturma nasıl açıklayan konulara başvuruyor.

## <a name="spark-and-mllib"></a>Spark ve MLlib
[Spark](https://spark.apache.org/) , büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen açık kaynaklı bir paralel işleme çerçevesidir. Spark işleme altyapısı hız, kullanım kolaylığı ve Gelişmiş analiz için oluşturulmuştur. Spark'ın dağıtılmış bellek içi hesaplama özellikleri onu kullanılan makine öğrenimi ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçim haline getirir. [Mllib](https://spark.apache.org/mllib/) , algoritmik modelleme yeteneklerini bu dağıtılmış ortama getiren, Spark 'ın ölçeklenebilir makine öğrenimi kitaplığıdır.

## <a name="hdinsight-spark"></a>HDInsight Spark
[HDInsight Spark](../../hdinsight/spark/apache-spark-overview.md) , Azure 'da barındırılan ve açık kaynaklı Spark sunumudur. Ayrıca Spark kümesinde bulunan **jupi pyspark Not defterleri** için destek Içerir ve Azure Bloblarında depolanan verileri dönüştürmek, filtrelemek ve görselleştirmek IÇIN Spark SQL etkileşimli sorgularını çalıştırabilir (edb). PySpark Spark için Python API'dir. Çözümler ve burada yüklü üzerinde Spark kümeleri Jupyter not defterlerini çalıştırmak verileri görselleştirmek için ilgili çizimleri Göster kod parçacıkları. Bu konu başlıklarındaki modelleme adımları eğitmek, değerlendirmek, kaydetme ve her bir türü modelin kullanma işlemi gösterilmektedir kodunu içerir.

## <a name="setup-spark-clusters-and-jupyter-notebooks"></a>Kurulum: Spark kümelerine ve Jupyter Not Defterleri
Bu izlenecek yolda, kurulum adımları ve kod kullanarak bir HDInsight Spark 1.6 için sağlanır. Ancak Jupyter not defterleri, kümeler, HDInsight Spark 1.6 hem de Spark 2.0 için sağlanır. Not defterlerinin ve bunlara bağlantıların açıklaması, bunları içeren GitHub deposu için [README.MD](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) içinde verilmiştir. Ayrıca, kodu buraya bağlı not defterlerinde geneldir ve herhangi bir Spark kümesi üzerinde çalışması gerekir. HDInsight Spark kullanmıyorsanız, küme kurulum ve yönetim adımları ne burada gösterilenden biraz farklı olabilir. Kolaylık olması için, Spark 1,6 için Jupyter Notebook (Jupyter Notebook sunucusunun pySpark çekirdeğindedir) ve Spark 2,0 (Jupyter Notebook sunucusunun pySpark3 çekirdeğinde çalıştırılacak şekilde) bağlantıları aşağıda verilmiştir:

### <a name="spark-16-notebooks"></a>Spark 1.6 Not Defterleri
Bu not defterlerini Jupyter notebook sunucusu pySpark Çekirdeği'nde çalıştırılması üzeresiniz.

- [Pyspark-Machine-Learning-Data-Science-Spark-Data-araştırmayla-Modellendirme. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): birçok farklı algoritmayla veri keşif, modelleme ve Puanlama gerçekleştirme hakkında bilgi sağlar.
- [Pyspark-Machine-Learning-Data-Science-Spark-gelişmiş-veri-araştırma-modelleme. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): not defteri #1 konuları ve hiper parametre ayarlama ve çapraz doğrulama kullanarak model geliştirmeyi içerir.
- [Pyspark-Machine-Learning-Data-Science-Spark-model-tüketim. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb): HDInsight kümelerinde Python kullanarak kaydedilmiş bir modeli nasıl kullanıma sunmayı gösterir.

### <a name="spark-20-notebooks"></a>Spark 2.0 Not Defterleri
Bu not defterlerini Jupyter notebook sunucusu pySpark3 Çekirdeği'nde çalıştırılması üzeresiniz.

- [Spark 2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-araştırması-Modellendirme. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Bu dosya, Spark 2,0 kümelerinde, [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data)açıklanan NYC Taxi seyahat ve tarifeli havayolu veri kümesini kullanarak veri keşif, modelleme ve Puanlama gerçekleştirme hakkında bilgi sağlar. Bu not defteri hızla Spark 2.0 için sağladık Kodu Keşfetme için iyi bir başlangıç noktası olabilir. Daha ayrıntılı bir not defteri NYC taksi verileri analiz eder, bu listedeki sonraki not bakın. Bu not defterlerini karşılaştıran bu listenin altındaki notlara bakın.
- [Spark 2.0-pySpark3_NYC_Taxi_Tip_Regression. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Bu dosya, [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data)açıklanan NYC TAXI seyahat ve tarifeli havayolu veri kümesini kullanarak veri denetimi (Spark SQL ve dataframe işlemleri), araştırma, modelleme ve Puanlama gerçekleştirme işlemini gösterir.
- [Spark 2.0-pySpark3_Airline_Departure_Delay_Classification. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Bu dosya, 2011 ve 2012 ' den iyi bilinen Airline ile veri kümesini kullanarak veri denetimi (Spark SQL ve dataframe işlemleri), araştırma, modelleme ve Puanlama gerçekleştirme işlemini gösterir. Hava yolu veri kümesini modellemeye başlamadan önce Havaalanı hava durumu verileriyle tümleştirdik (örneğin, wınvs hızı, sıcaklık, yükseklik vb.), bu hava durumu özellikleri modele dahil edilebilir.

<!-- -->

> [!NOTE]
> Havayolu veri kümesini sınıflandırma algoritmalarının kullanımını daha iyi anlamak için Spark 2.0 not defterleri için eklendi. Aşağıdaki bağlantıları kalkış veri kümesi ve hava durumu dataset zamanında Havayolu hakkında bilgi için bkz.
> 
> - Airline açık zamanlı ayrılma verileri: [https://www.transtats.bts.gov/ONTIME/](https://www.transtats.bts.gov/ONTIME/)
> 
> - Havaalanı hava durumu verileri: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)

<!-- -->

<!-- -->

> [!NOTE]
> Spark 2.0 not defterleri ile ilgili NYC taksi ve Havayolu uçuş gecikme veri kümeleri, 10 dakika veya (HDI kümenizin boyutuna bağlı olarak) çalıştırmak için daha fazla sürebilir. Yukarıdaki listede yer aldığı ilk not defteri, veri gezgininin birçok yönünü gösterir. TAXI ve tarifeli havayolu dosyalarının önceden katıldığı, alt örneklenmiş NYC veri kümesiyle çalıştırılması daha az zaman alan bir not defterinde görselleştirme ve ML modeli eğitimi: [Spark 2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-araştırmayı-Modellendirme. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb). Bu Not defterinin tamamlanmasının daha kısa sürmesi (2-3 dakika) ve Spark 2,0 için sağladığımız kodu hızlı bir şekilde keşfetmek için iyi bir başlangıç noktası olabilir.

<!-- -->

Bir Spark 2,0 modelinin ve Puanlama için model tüketiminin kullanıma hazır hale getirme hakkında rehberlik için, gerekli adımları bir örnek olarak göstermek üzere [Tüketim üzerinde Spark 1,6 belgesine](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) bakın. Bu örneği Spark 2,0 ' de kullanmak için Python kod dosyasını [Bu dosyayla](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py)değiştirin.

### <a name="prerequisites"></a>Önkoşullar

Spark 1.6 için aşağıdaki yordamları ilgilidir. Spark 2.0 sürümüne için açıklanan ve için daha önce bağlı not defterlerini kullanma.

1. Bir Azure aboneliğiniz olmalıdır. Henüz bir tane yoksa, bkz. [Azure Ücretsiz deneme sürümü](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. Bu izlenecek yolu tamamlamak için bir Spark 1,6 kümesine ihtiyacınız vardır. Bir tane oluşturmak için bkz. [Başlarken: Azure HDInsight üzerinde Apache Spark oluşturma](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Küme türü ve sürümü, **küme türü seç** menüsünden belirtilmiştir.

![Küme yapılandırma](./media/spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [!NOTE]
> Uçtan uca bir veri bilimi işleminin görevlerini tamamlaması için Python yerine Scala 'nın nasıl kullanılacağını gösteren bir konu için bkz. [Azure 'Da Spark Ile Scala kullanarak veri bilimi](scala-walkthrough.md).
>
>

<!-- -->

> [!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]
>
>

## <a name="the-nyc-2013-taxi-data"></a>NYC 2013 taksi verileri
NYC taksi seyahat verilerini yaklaşık 20 GB sıkıştırılmış virgülle ayrılmış değerler (CSV) dosyaları (~ 48 sıkıştırılmamış GB), her seyahat için ücretli 173 milyondan fazla bireysel gelişlerin ve fares. Her bir seyahat kaydı alma ve dropoff konumu zaman, anonimleştirilmiş hack (sürücü) lisans numarası ve medallion (taksi'nın benzersiz tanımlayıcı) numarasını içerir. Veriler tüm dönüş 2013 yılında kapsar ve aşağıdaki iki veri kümesi için her ay sağlanır:

1. 'Trip_data' CSV dosyaları öğrenilip Yolcuların Sayısı gibi seyahat ayrıntıları içerir ve süresi ve seyahat uzunluğu dropoff işaret geçirmek. Birkaç örnek kayıt şunlardır:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. 'Trip_fare' CSV dosyaları için ödeme türü, taksi tutar, ek ücret ve vergiler, ipuçları ve Ücretli geçişler, gibi her seyahat Ücretli taksi ve ödenen toplam tutarı ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Bu dosyaların% 0,1 bir örneğini aldık ve seyahat\_verileri ve seyahat\_tarifeli havayolu CVS dosyalarını Bu izlenecek yol için giriş veri kümesi olarak kullanılacak tek bir veri kümesine katıldık. Seyahat\_veri ve seyahat\_tarifeli havayolu katılacak benzersiz anahtar, şu alanlardan oluşur: medtalon, Hack\_lisans ve toplama\_tarih saat. Her kayıt veri kümesinin bir NYC taksi seyahatini temsil eden aşağıdaki öznitelikleri içerir:

| Alan | Kısa açıklama |
| --- | --- |
| medallion |Anonimleştirilmiş taksi medallion (benzersiz taksi kimliği) |
| hack_license |Anonimleştirilmiş Hackney satır başı Ehliyet numarası |
| vendor_id |Taksi satıcı kimliği |
| rate_code |Taksi NYC taksi oranı |
| store_and_fwd_flag |Store ve iletme bayrağı |
| pickup_datetime |Tarih ve saati seçin |
| dropoff_datetime |Dropoff tarih ve saat |
| pickup_hour |Saati seçin |
| pickup_week |Haftanın günü seçin |
| Haftanın günü |Haftanın günü (aralık 1-7) |
| passenger_count |Taksi seyahat içinde Yolcuların Sayısı |
| trip_time_in_secs |Seyahat süresini saniye cinsinden |
| trip_distance |Mili imkansız seyahat uzaklık |
| pickup_longitude |Boylam seçin |
| pickup_latitude |Enlem seçin |
| dropoff_longitude |Dropoff boylam |
| dropoff_latitude |Dropoff enlem |
| direct_distance |Toplama ve açılan konumlar arasındaki doğrudan uzaklık |
| payment_type |Ödeme türü (nakit, kredi kartı vb.) |
| fare_amount |Taksi tutar |
| Ek maliyeti |Ek maliyeti |
| mta_tax |MTA metro ulaşım vergisi |
| tip_amount |İpucu tutar |
| tolls_amount |Ücretli geçişler tutar |
| total_amount |Toplam tutar |
| Eğimli |Eğimli (0/1 Hayır için Evet veya) |
| tip_class |Sınıf İpucu (0: 0, 1: $0-5, 2: $6-10, 3: $11-20, 4: > $20) |

## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Spark kümesinde Jupyter not defteri gelen kod yürütün
Jupyter not defterine Azure portalından başlatabilirsiniz. Spark kümenizde panonuzu bulun ve kümeniz için yönetim sayfası girmek için tıklatın. Spark kümesiyle ilişkili Not defterini açmak için, -> **Jupyter Notebook** **küme panoları** ' na tıklayın.

![Küme panoları](./media/spark-overview/spark-jupyter-on-portal.png)

Ayrıca, jupi not defterlerine erişmek için ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** de gidebilirsiniz. CLUSTERNAME bu URL'nin bir parçası, kendi kümenizin adıyla değiştirin. Not defterlerini erişmek, yönetici hesabı için parola gerekir.

![Jupyter not defterleri Gözat](./media/spark-overview/spark-jupyter-notebook.png)

PySpark API kullanan önceden paketlenmiş not defterlerine birkaç örnek içeren bir dizini görmek için PySpark ' ı seçin. Bu Spark konusunun kod örneklerini içeren Not defterleri [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) 'da kullanılabilir

Not defterlerini doğrudan [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) 'dan Spark kümenizdeki Jupyter Not defteri sunucusuna yükleyebilirsiniz. Jupi 'nizin giriş sayfasında, ekranın sağ tarafındaki **karşıya yükle** düğmesine tıklayın. Dosya Gezgini'ni açar. Burada, Not defterinin GitHub (ham içerik) URL 'sini yapıştırabilir ve **Aç**' a tıklayabilirsiniz.

Dosya adını Jupyıter dosya listenizde bir kez **karşıya yükle** düğmesine yeniden görürsünüz. Bu **karşıya yükle** düğmesine tıklayın. Şimdi bir not defteri aldınız. Bu izlenecek yolda diğer defterlerinden karşıya yüklemek için bu adımları yineleyin.

> [!TIP]
> Tarayıcınızdaki bağlantıları sağ tıklatıp GitHub ham içerik URL 'sini almak için **Bağlantıyı Kopyala** ' yı seçebilirsiniz. Bu URL'yi Jupyter karşıya dosya Gezgini iletişim kutusuna yapıştırabilirsiniz.
> 
> 

Artık şunları yapabilirsiniz:

* Not defterini tıklayarak koduna bakın.
* Her hücreyi **SHIFT-enter**tuşlarına basarak yürütün.
*  -> **Çalıştır**' a tıklayarak tüm Not defterini çalıştırın.
* Sorgu otomatik görselleştirme kullanın.

> [!TIP]
> PySpark Çekirdeği (HiveQL) SQL sorguları çıktısını otomatik olarak görselleştirir. Not defterindeki **tür** menü düğmelerini kullanarak birkaç farklı görselleştirme türü (tablo, pasta, çizgi, alan veya çubuk) arasından seçim yapabilirsiniz:
>
>

![Genel bir yaklaşım için Lojistik regresyon ROC eğrisi](./media/spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Sırada ne var?
Bir HDInsight Spark kümesi ile ayarlanır ve Jupyter not defterlerini karşıya yüklediğiniz göre bu üç PySpark not defterleri için karşılık gelen konuları ile çalışmaya hazır olursunuz. Bunlar, nasıl oluşturarak verilerinizi araştırmanıza ve nasıl oluşturulup tüketim modelleri görüntüleyin. Gelişmiş Veri keşfi ve modelleme Not Defteri, çapraz doğrulama, hyper-Süpürme saldırısı yapılabilir, parametre eklemek ve değerlendirme modeli gösterilmektedir.

**Spark Ile veri araştırması ve modelleme:** [Spark MLlib araç seti ile veri için ikili sınıflandırma ve regresyon modelleri oluşturma](spark-data-exploration-modeling.md) konusunun veri kümesini İnceleme ve makine öğrenimi modelleri oluşturma, Puanlama ve değerlendirme.

**Model tüketimi:** Bu konuda oluşturulan sınıflandırma ve regresyon modellerinin nasıl puan alabileceğinizi öğrenmek için bkz. [Spark ile oluşturulmuş makine öğrenimi modellerini Puanlama ve değerlendirme](spark-model-consumption.md).

**Çapraz doğrulama ve hiper parametre**kullanımı: modellerin çapraz doğrulama ve hiper parametre tatkiyle nasıl eğitilmesine ilişkin [Spark ile gelişmiş veri araştırması ve modellemeye](spark-advanced-data-exploration-modeling.md) bakın

