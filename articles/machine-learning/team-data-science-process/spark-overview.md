---
title: Azure HDInsight'ta Spark'ı kullanarak veri bilimi - Ekip Veri Bilimi Süreci
description: Spark MLlib araç seti, dağıtılmış HDInsight ortamına önemli ölçüde makine öğrenimi modelleme özellikleri getirir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76718523"
---
# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Azure HDInsight'ta Spark'ı kullanarak veri bilimine genel bakış

Bu konu paketi, veri alımı, özellik mühendisliği, modelleme ve model değerlendirmesi gibi yaygın veri bilimi görevlerini tamamlamak için HDInsight Spark'ın nasıl kullanılacağını gösterir. Kullanılan veriler, 2013 NYC taksi seyahati ve ücret veri kümesinin bir örneğidir. Üretilen modeller arasında lojistik ve doğrusal regresyon, rastgele ormanlar ve degrade artırılmış ağaçlar yer almaktadır. Konular ayrıca bu modellerin Azure blob depolamasında (WASB) nasıl depolandığını ve tahmine dayalı performanslarını nasıl puanlayıp değerlendireceklerini de gösterir. Daha gelişmiş konular, modellerin çapraz doğrulama ve hiper-parametre süpürme kullanılarak nasıl eğitilebileceğini kapsar. Bu genel bakış konusu, sağlanan izyolboyunca adımları tamamlamak için gereken Kıvılcım kümesini nasıl ayarladığınızı açıklayan konulara da başvurur.

## <a name="spark-and-mllib"></a>Kıvılcım ve MLlib
[Spark,](https://spark.apache.org/) büyük veri analitik uygulamaların performansını artırmak için bellek içi işlemeyi destekleyen açık kaynaklı paralel bir işlem çerçevesidir. Spark işleme motoru hız, kullanım kolaylığı ve gelişmiş analizler için üretilmiştir. Spark'ın bellek içi dağıtılmış hesaplama yetenekleri, makine öğrenimi ve grafik hesaplamalarında kullanılan yinelemeli algoritmalar için iyi bir seçim dir. [MLlib,](https://spark.apache.org/mllib/) bu dağıtılmış ortama algoritmik modelleme yeteneklerini getiren Spark'ın ölçeklenebilir makine öğrenimi kütüphanesidir.

## <a name="hdinsight-spark"></a>HDInsight Spark
[HDInsight Spark,](../../hdinsight/spark/apache-spark-overview.md) açık kaynak kodlu Kıvılcım'ın Azure barındırılan teklifidir. Ayrıca, Azure Blobs'ta (WASB) depolanan verileri dönüştürmek, filtreleme ve görselleştirme için SQL etkileşimli sorguları çalıştırabilen Spark kümesindeki **Jupyter PySpark dizüstü bilgisayarları** için destek de içerir. PySpark, Spark'ın Python API'si. Çözümleri sağlayan ve burada Spark kümelerine yüklenen Jupyter dizüstü bilgisayarlarda çalışan verileri görselleştirmek için ilgili çizimleri gösteren kod parçacıkları. Bu konulardaki modelleme adımları, her model türünü nasıl eğitecek, değerlendirecek, kaydedin ve tüketeceklerini gösteren kodlar içerir.

## <a name="setup-spark-clusters-and-jupyter-notebooks"></a>Kurulum: Spark kümeleri ve Jupyter dizüstü bilgisayarlar
HDInsight Spark 1.6'yı kullanmak için bu geçiş te kurulum adımları ve kod sağlanır. Ancak Jupyter dizüstü bilgisayarlar hem HDInsight Spark 1.6 hem de Spark 2.0 kümeleri için sağlanır. Bunları içeren GitHub deposunun [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) dizüstü bilgisayarların ve onlara bağlantılar bir açıklama sağlanır. Ayrıca, burada ve bağlantılı not defterlerinde kod geneldir ve herhangi bir Kıvılcım kümesi üzerinde çalışması gerekir. HDInsight Spark kullanmıyorsanız, küme kurulumu ve yönetim adımları burada gösterilenden biraz farklı olabilir. Kolaylık sağlamak için, burada Spark 1.6 (Jupyter Notebook sunucusunun pySpark çekirdeğinde çalıştırılacak) ve Spark 2.0 (Jupyter Notebook sunucusunun pySpark3 çekirdeğinde çalıştırılacak) için Jupyter dizüstü bilgisayarlara bağlantılar:

### <a name="spark-16-notebooks"></a>Kıvılcım 1.6 dizüstü bilgisayarlar
Bu defterler Jupyter notebook sunucusunun pySpark çekirdeğinde çalıştırılacak.

- [pySpark-machine-learning-data-science-kıvılcım-veri-keşif-modelleme-modelleme.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): Birkaç farklı algoritma ile veri arama, modelleme ve puanlama nın nasıl yapılabilen bilgiler sağlar.
- [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Not defteri #1 konuları ve hiperparametre ayarı ve çapraz doğrulama kullanarak model geliştirme konularını içerir.
- [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb): HDInsight kümelerinde Python'u kullanarak kaydedilen bir modelin nasıl işletildiğini gösterir.

### <a name="spark-20-notebooks"></a>Kıvılcım 2.0 dizüstü bilgisayarlar
Bu defterler Jupyter dizüstü bilgisayar sunucusunun pySpark3 çekirdeğinde çalıştırılacak.

- [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Bu [dosya,](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data)nyc Taksi gezisi ve ücret veri seti kullanarak Spark 2.0 kümelerinde veri arama, modelleme ve puanlama nasıl yapılacılacılıcı bilgi sağlayıyor. Bu not defteri, Spark 2.0 için sağladığımız kodu hızlı bir şekilde keşfetmek için iyi bir başlangıç noktası olabilir. Daha ayrıntılı bir not defteri NYC Taksi verilerini analiz etmek için bu listedeki bir sonraki not defterine bakın. Bu not defterlerini karşılaştıran bu listeyi izleyen notlara bakın.
- [Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Bu [dosya, burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data)açıklanan NYC Taksi gezisi ve ücret veri seti kullanarak veri çekişmesi (Spark SQL ve dataframe işlemleri), keşif, modelleme ve puanlama nasıl yapılacağını gösterir.
- [Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Bu dosya, 2011 ve 2012 yılları arasında bilinen Havayolu Zamanında Kalkış veri setini kullanarak veri çekişmesinin (Spark SQL ve dataframe işlemleri), arama, modelleme ve puanlamanın nasıl gerçekleştirilip gerçekleştirilebildiğini göstermektedir. Modellemeden önce havayolu veri kümesini havaalanı hava durumu verileriyle (örneğin, rüzgar hızı, sıcaklık, irtifa vb.) entegre ettik, böylece bu hava durumu özellikleri modele dahil edilebilir.

<!-- -->

> [!NOTE]
> Havayolu veri kümesi, sınıflandırma algoritmalarının kullanımını daha iyi göstermek için Spark 2.0 dizüstü bilgisayarlarına eklendi. Havayolu zamanında kalkış veri seti ve hava durumu veri seti hakkında bilgi için aşağıdaki bağlantılara bakın:
> 
> - Havayolu zamanında kalkış verileri:[https://www.transtats.bts.gov/ONTIME/](https://www.transtats.bts.gov/ONTIME/)
> 
> - Havaalanı hava durumu verileri:[https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)

<!-- -->

<!-- -->

> [!NOTE]
> NYC taksi ve havayolu uçuş gecikme veri setleri Spark 2.0 dizüstü bilgisayarlar çalıştırmak için 10 dakika veya daha fazla sürebilir (HDI kümenin boyutuna bağlı olarak). Yukarıdaki listede ilk dizüstü bilgisayar, taksi ve ücret dosyaları önceden katıldı olan aşağı örneklenmiş NYC veri seti ile çalıştırmak için daha az zaman alır bir dizüstü bilgisayar da veri arama, görselleştirme ve ML modeli eğitimi birçok yönünü gösterir: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb). Bu dizüstü bilgisayarın tamamlanması çok daha kısa bir zaman alır (2-3 dakika) ve Spark 2.0 için sağladığımız kodu hızlı bir şekilde keşfetmek için iyi bir başlangıç noktası olabilir.

<!-- -->

Puanlama için Bir Spark 2.0 modelinin ve model tüketiminin işlevselleştirilmesi hakkında rehberlik için, gerekli adımları özetleyen bir örnek için [tüketimle ilgili Spark 1.6 belgesine](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) bakın. Bu örneği Spark 2.0'da kullanmak için Python kod dosyasını [bu dosyayla](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py)değiştirin.

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki prosedürler Spark 1.6 ile ilgilidir. Spark 2.0 sürümü için, daha önce açıklanan ve bağlantılı dizüstü bilgisayarları kullanın.

1. Bir Azure aboneliğiniz olmalıdır. Zaten bir sürümünüz yoksa, [bkz.](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

2. Bu gözden geçirmeyi tamamlamak için bir Kıvılcım 1.6 kümesine ihtiyacınız vardır. Bir tane oluşturmak için Başlat'ta sağlanan yönergelere [bakın: Azure HDInsight'ta Apache Spark oluşturun.](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md) Küme türü ve **sürümü, Cluster Type seç** menüsünden belirtilir.

![Kümeyi yapılandırma](./media/spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [!NOTE]
> Uçtan uca veri bilimi işlemi için görevleri tamamlamak için Python yerine Scala'nın nasıl kullanılacağını gösteren bir konu için, [Azure'da Scala ile Scala kullanan Veri Bilimi'ne](scala-walkthrough.md)bakın.
>
>

<!-- -->

> [!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]
>
>

## <a name="the-nyc-2013-taxi-data"></a>NYC 2013 Taksi verileri
NYC Taksi Gezisi verileri yaklaşık 20 GB sıkıştırılmış virgülayrılmış değerler (CSV) dosyaları (~ 48 GB sıkıştırılmamış), 173 milyondan fazla bireysel geziler ve ücretler her yolculuk için ödenen oluşan. Her seyahat kaydı pickup ve bırakma yeri ve saati, anonim hack (sürücü) lisans numarası ve madalyon (taksinin benzersiz kimlik) numarasını içerir. Veriler 2013 yılındaki tüm gezileri kapsar ve her ay için aşağıdaki iki veri kümesinde sağlanır:

1. 'trip_data' CSV dosyaları, yolcu sayısı, teslim alma ve bırakma noktaları, seyahat süresi ve seyahat uzunluğu gibi seyahat bilgilerini içerir. Aşağıda birkaç örnek kayıt veörnekleri verebleri verebleri bulabilirsiniz:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. 'trip_fare' CSV dosyaları, ödeme türü, ücret tutarı, ek ücret ve vergiler, bahşişler ve geçiş ücretleri ve ödenen toplam tutar gibi her seyahat için ödenen ücretin ayrıntılarını içerir. Aşağıda birkaç örnek kayıt veörnekleri verebleri verebleri bulabilirsiniz:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Bu dosyaların %0,1'ini aldık ve gezi\_verilerini\_ve seyahat ücreti CVS dosyalarını bu izlenme için giriş veri seti olarak kullanmak üzere tek bir veri kümesine katıldık. Seyahat\_verilerine ve seyahat\_ücretine katılmak için eşsiz anahtar\_alanlardan\_oluşur: madalyon, hack lisansı ve teslim alma tarihi. Veri kümesinin her kaydı, NYC Taksi seyahatini temsil eden aşağıdaki öznitelikleri içerir:

| Alan | Kısa Açıklama |
| --- | --- |
| Madalyon |Anonim taksi madalyonu (benzersiz taksi kimliği) |
| hack_license |Anonim Hackney Taşıma Ruhsatı numarası |
| vendor_id |Taksi satıcı kimliği |
| rate_code |ücret NYC taksi oranı |
| store_and_fwd_flag |Mağaza ve iletme bayrağı |
| pickup_datetime |Tarihi & saat |
| dropoff_datetime |Bırakma tarihi & saat |
| pickup_hour |Pick up saat |
| pickup_week |Yılın haftası alma |
| Hafta içi |Hafta içi (dağılım 1-7) |
| passenger_count |Taksi yolculuğundaki yolcu sayısı |
| trip_time_in_secs |Saniye cinsinden yolculuk süresi |
| trip_distance |Yolculuk mesafesi kilometreleriçinde seyahat etti |
| pickup_longitude |Boylam alma |
| pickup_latitude |Enlem alma |
| dropoff_longitude |Bırakma boylam |
| dropoff_latitude |Bırakma enlemi |
| direct_distance |Teslim alma ve bırakma konumları arasındaki doğrudan mesafe |
| payment_type |Ödeme türü (nakit, kredi kartı vb.) |
| fare_amount |Ücret tutarı |
| Ücretli |Ücretli |
| mta_tax |MTA Metro Ulaşım vergisi |
| tip_amount |İpucu miktarı |
| tolls_amount |Geçiş ücretleri tutarı |
| total_amount |Toplam tutar |
| Uçlu |Uçlu (hayır veya evet için 0/1) |
| tip_class |İpucu sınıfı (0: $0, 1: $0-5, 2: $6-10, 3: $11-20, 4: > $20) |

## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Kıvılcım kümesindeki bir Jupyter dizüstü bilgisayarından kodu yürütme
Jupyter Notebook'u Azure portalından başlatabilirsiniz. Gösterge panonuzda Kıvılcım kümenizi bulun ve kümenizin yönetim sayfasını girmek için bu kümeye tıklayın. Kıvılcım kümesiyle ilişkili not defterini açmak için **Cluster Dashboards** -> **Jupyter Notebook'u**tıklatın.

![Küme panoları](./media/spark-overview/spark-jupyter-on-portal.png)

Jupyter Notebook'lara erişmek için ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** de göz atabilirsiniz. Bu URL'nin CLUSTERNAME bölümünü kendi kümenizin adıyla değiştirin. Not defterlerine erişmek için yönetici hesabınızın parolasına ihtiyacınız vardır.

![Jupyter Dizüstü Bilgisayarlara Göz atın](./media/spark-overview/spark-jupyter-notebook.png)

PySpark API'sini kullanan önceden paketlenmiş not defterlerinin birkaç örneğini içeren bir dizini görmek için PySpark'ı seçin. Bu Kıvılcım konu paketinin kod örneklerini içeren dizüstü bilgisayarlar [GitHub'da](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) mevcuttur

Dizüstü bilgisayarları doğrudan [GitHub'dan](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) Spark kümenizdeki Jupyter dizüstü bilgisayar sunucusuna yükleyebilirsiniz. Jupyter'ınızın ana sayfasında, ekranın sağ kısmındaki **Yükle** düğmesini tıklatın. Bir dosya gezgini açar. Burada Not Defterinin GitHub (ham içerik) URL'sini yapıştırabilir ve **Aç'ı**tıklatabilirsiniz.

Jupyter dosya listenizde dosya adını tekrar **Yükle** düğmesiyle görürsünüz. Bu **Yükle** düğmesini tıklatın. Şimdi defteri ithal ettin. Bu iznin diğer not defterlerini yüklemek için bu adımları yineleyin.

> [!TIP]
> Tarayıcınızdaki bağlantılara sağ tıklayabilir ve GitHub ham içerik URL'sini almak için **Bağlantı** Kopyala'yı seçebilirsiniz. Bu URL'yi Jupyter Upload dosya gezgini iletişim kutusuna yapıştırabilirsiniz.
> 
> 

Artık şunları yapabilirsiniz:

* Not defterini tıklatarak kodu görün.
* **SHIFT-ENTER**tuşuna basarak her hücreyi çalıştırın.
* **Hücre** -> Çalıştır'ı tıklatarak tüm not defterini**çalıştırın.**
* Sorguların otomatik görselleştirmesini kullanın.

> [!TIP]
> PySpark çekirdeği SQL (HiveQL) sorgularının çıktısını otomatik olarak görselleştirir. Not defterindeki **Tür** menü düğmelerini kullanarak birkaç farklı görselleştirme türü (Tablo, Pasta, Satır, Alan veya Çubuk) arasından seçim yapabilirsiniz:
>
>

![Jenerik yaklaşım için lojistik regresyon ROC eğrisi](./media/spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Sırada ne var?
Artık bir HDInsight Spark kümesi yle kurulduğunuza ve Jupyter dizüstü bilgisayarlarını yüklediğinize göre, üç PySpark dizüstü bilgisayarına karşılık gelen konuları gözden geçirmeye hazırsınız. Verilerinizi nasıl inceleyeceğimize ve ardından modellerin nasıl oluşturulup tüketilenleri gösterirler. Gelişmiş veri arama ve modelleme not defteri çapraz doğrulama, hiper-parametre süpürme ve model değerlendirme nasıl dahil gösterir.

**Spark ile Veri Arama ve Modelleme:** Veri kümesini keşfedin ve [Spark MLlib araç seti konusuyla veri için ikili sınıflandırma ve regresyon modelleri oluştur](spark-data-exploration-modeling.md) üzerinden çalışarak makine öğrenimi modellerini oluşturun, puanlandırın ve değerlendirin.

**Model tüketimi:** Bu konuda oluşturulan sınıflandırma ve regresyon modellerini nasıl puanlandırıp değerlendireceklerini öğrenmek için [Puan'a bakın ve Kıvılcım yapımı makine öğrenimi modellerini değerlendirin.](spark-model-consumption.md)

**Çapraz doğrulama ve hiperparametre süpürme**: [Spark ile](spark-advanced-data-exploration-modeling.md) modellerin çapraz doğrulama ve hiper-parametre süpürme kullanılarak nasıl eğitilene ilişkin gelişmiş veri keşfi ve modellemesi

