---
title: Azure Veri Gölü ile Ölçeklenebilir Veri Bilimi - Ekip Veri Bilimi Süreci
description: Bir veri kümesinde veri arama ve ikili sınıflandırma görevleri yapmak için Azure Veri Gölü nasıl kullanılır.
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
ms.openlocfilehash: 9409f14b20684afa1a39d45e663ff316f405cc97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76717919"
---
# <a name="scalable-data-science-with-azure-data-lake-an-end-to-end-walkthrough"></a>Azure Veri Gölü ile Ölçeklenebilir Veri Bilimi: Uçtan uca Bir Geçiş
Bu izleme, bir bahşişin ücretle ödenip ödenmediğini tahmin etmek için NYC taksi yolculuğu ve ücret veri kümesinin bir örneğinde veri arama ve ikili sınıflandırma görevleri yapmak için Azure Veri Gölü'nün nasıl kullanılacağını gösterir. Veri toplamadan model eğitimine ve ardından modeli yayımlayan bir web hizmetinin dağıtımına kadar [Ekip Veri Bilimi Süreci'nin](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)adımlarından uçtan uca geçirmenize kadar size yol alar.

## <a name="technologies"></a>Teknolojiler

Bu teknolojiler bu geçiş te kullanılır.
* Azure Data Lake Analytics
* U-SQL ve Görsel Stüdyo
* Python
* Azure Machine Learning
* Betikler


### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
[Microsoft Azure Veri Gölü,](https://azure.microsoft.com/solutions/data-lake/) veri bilimcilerin herhangi bir boyut, şekil ve hızdaki verileri depolamasını ve veri işleme, gelişmiş analitik ve makine öğrenimi modellemesini yüksek ölçeklenebilirlik li maliyet açısından gerçekleştirmesini kolaylaştırmak için gereken tüm özelliklere sahiptir.   Yalnızca veriler gerçekten işlenirken, iş başına ödeme yapılır. Azure Veri Gölü Analizi, ÖLÇEKLENEBILIR dağıtılabilir sorgu özelliği sağlamak için SQL'in bildirimsel yapısını C#'nin ifade gücüyle harmanlayan bir dil olan U-SQL'i içerir. Okuma üzerine şema uygulayarak yapılandırılmamış verileri işlemenizi sağlar, özel mantık ve kullanıcı tanımlı işlevler (UDF) ekler ve ölçekte nasıl yürütülecekleri üzerinde ince taneli denetimi etkinleştirmek için genişletilebilirlik içerir. U-SQL'in arkasındaki tasarım felsefesi hakkında daha fazla bilgi edinmek için [Visual Studio blog gönderisine](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/)bakın.

Data Lake Analytics aynı zamanda Cortana Analytics Suite'in önemli bir parçasıdır ve Azure SQL Veri Ambarı, Power BI ve Veri Fabrikası ile çalışır. Bu kombinasyon size tam bir bulut büyük veri ve gelişmiş analiz platformu sağlar.

Bu izleme, veri bilimi işleme görevlerini tamamlamak için gereken ön koşulların ve kaynakların nasıl yüklenirlerini açıklayarak başlar. Daha sonra U-SQL kullanarak veri işleme adımlarını özetler ve tahmine dayalı modelleri oluşturmak ve dağıtmak için Python ve Hive'ı Azure Machine Learning Studio (klasik) ile nasıl kullanılacağını göstererek sona erer.

### <a name="u-sql-and-visual-studio"></a>U-SQL ve Görsel Stüdyo
Bu walkthrough, veri kümesini işlemek için U-SQL komut dosyalarını ayarlamak için Visual Studio'yu kullanmanızı önerir. U-SQL komut dosyaları burada açıklanır ve ayrı bir dosyada sağlanır. İşlem, verilerin sindirilmesi, araştırılması ve örneklenmesi içerir. Ayrıca, Azure portalından U-SQL komut dosyası yla yazılmış bir işin nasıl çalıştırılabildiğini de gösterir. Azure Machine Learning Studio'da ikili sınıflandırma modeli nin oluşturulmasını ve dağıtılmasını kolaylaştırmak için ilişkili bir HDInsight kümesindeki veriler için kovan tabloları oluşturulur.

### <a name="python"></a>Python
Bu gözden geçirme, Azure Machine Learning Studio ile Python'u kullanarak tahmine dayalı bir modelin nasıl oluşturup dağıtılacak yapılacağını gösteren bir bölüm de içerir. Bu işlemdeki adımlar için Python komut dosyaları ile bir Jupyter dizüstü bilgisayar sağlar. Not defteri, burada özetlenen ikili sınıflandırma modeline ek olarak, çok sınıflı sınıflandırma ve regresyon modelleme gibi bazı ek özellik mühendisliği adımları ve model yapımı için kod içerir. Regresyon görevi, diğer uç özelliklerine göre uç miktarını tahmin etmektir.

### <a name="azure-machine-learning"></a>Azure Machine Learning 
Azure Machine Learning Studio (klasik), tahmine dayalı modelleri iki yaklaşım kullanarak oluşturmak ve dağıtmak için kullanılır: önce Python komut dosyalarıyla, sonra da BIR HDInsight (Hadoop) kümesindeki Hive tablolarıyla.

### <a name="scripts"></a>Betikler
Bu gözden geçirme de yalnızca temel adımlar özetlenmiştir. [GitHub'dan](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough)tam **U-SQL komut dosyasını** ve **Jupyter Notebook'u** indirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu konulara başlamadan önce aşağıdakileri yapmalısınız:

* Azure aboneliği. Zaten bir sürümünüz yoksa, [bkz.](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)
* [Önerilen] Visual Studio 2013 veya sonrası. Bu sürümlerden biri zaten yüklü değilse, [Visual Studio Community'den](https://www.visualstudio.com/vs/community/)ücretsiz topluluk sürümünü indirebilirsiniz.

> [!NOTE]
> Visual Studio yerine Azure portalını kullanarak Azure Veri Gölü sorgularını gönderebilirsiniz. U-SQL ile **İşlem verileri**başlıklı bölümde hem Visual Studio ile hem de portalda nasıl yapılacağını anlatan talimatlar verilmektedir.
>
>


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Azure Veri Gölü için veri bilimi ortamı hazırlama
Veri bilimi ortamını bu gözden geçirme için hazırlamak için aşağıdaki kaynakları oluşturun:

* Azure Veri Gölü Depolama (ADLS)
* Azure Veri Gölü Analizi (ADLA)
* Azure Blob depolama hesabı
* Azure Machine Learning Studio (klasik) hesabı
* Visual Studio için Azure Veri Gölü Araçları (Önerilen)

Bu bölümde, bu kaynakların her birinin nasıl oluşturulacağına ilişkin yönergeler verilmektedir. Bir model oluşturmak için Python yerine Azure Machine Learning ile Hive tablolarını kullanmayı seçerseniz, bir HDInsight (Hadoop) kümesi sağlamanız gerekir. Bu alternatif yordam Seçenek 2 bölümünde açıklanmıştır.


> [!NOTE]
> **Azure Veri Gölü Deposu,** varsayılan depolama alanı olarak ayrı ayrı veya Azure Veri Gölü **Analizi'ni** oluşturduğunuzda oluşturulabilir. Bu kaynakların her birini ayrı ayrı oluşturmak için yönergelere başvurulmaktadır, ancak Veri Gölü depolama hesabının ayrı olarak oluşturulması gerekmez.
>
>

### <a name="create-an-azure-data-lake-storage"></a>Azure Veri Gölü Depolaması Oluşturma


[Azure portalından](https://portal.azure.com)bir ADLS oluşturun. Ayrıntılar için, [Azure portalLarını kullanarak Data Lake Store ile HDInsight kümesi oluşturma'ya](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)bakın. Burada açıklanan **İsteğe Bağlı Yapılandırma** bıçağının **DataSource** bıçağında Küme AAD Kimliğini ayarladıklarından emin olun.

 ![3](./media/data-lake-walkthrough/3-create-ADLS.PNG)

### <a name="create-an-azure-data-lake-analytics-account"></a>Azure Veri Gölü Analizi hesabı oluşturma
[Azure portalından](https://portal.azure.com)bir ADLA hesabı oluşturun. Ayrıntılar için [Bkz. Öğretici: Azure portalLarını kullanarak Azure Veri Gölü Analizi'ni kullanmaya başlayın.](../../data-lake-analytics/data-lake-analytics-get-started-portal.md)

 ![4](./media/data-lake-walkthrough/4-create-ADLA-new.PNG)

### <a name="create-an-azure-blob-storage-account"></a>Azure Blob depolama hesabı oluşturma
[Azure portalından](https://portal.azure.com)bir Azure Blob depolama hesabı oluşturun. Ayrıntılar için Azure [Depolama hesapları hakkında'ndaki](../../storage/common/storage-create-storage-account.md)depolama hesabı oluştur bölümüne bakın.

 ![5](./media/data-lake-walkthrough/5-Create-Azure-Blob.PNG)

### <a name="set-up-an-azure-machine-learning-studio-classic-account"></a>Azure Machine Learning Studio (klasik) hesabı ayarlama
[Azure Machine Learning studio](https://azure.microsoft.com/services/machine-learning/) sayfasından Azure Machine Learning Studio'ya (klasik) kaydolun/kaydolun. **Şimdi Başla** düğmesine tıklayın ve ardından bir "Boş Çalışma Alanı" veya "Standart Çalışma Alanı" seçin. Artık Azure Machine Learning stüdyosunda denemeler oluşturmaya hazır.

### <a name="install-azure-data-lake-tools-recommended"></a>Azure Veri Gölü Araçlarını Yükle [Önerilen]
Visual Studio için Azure Veri Gölü Araçları'ndan Visual Studio sürümünüz için Azure [Veri Gölü Araçları'nı](https://www.microsoft.com/download/details.aspx?id=49504)yükleyin.

 ![6](./media/data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Kurulum bittikten sonra Visual Studio'yu açın. Üstteki menüdeki Veri Gölü sekmesini görmelisiniz. Azure hesabınızda oturum açken Azure kaynaklarınız sol panelde görünmelidir.

 ![7](./media/data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)

## <a name="the-nyc-taxi-trips-dataset"></a>NYC Taksi Gezileri veri seti
Burada kullanılan veri kümesi, herkese açık bir veri kümesidir - [NYC Taksi Gezileri veri seti.](https://www.andresmh.com/nyctaxitrips/) NYC Taksi Gezisi verileri sıkıştırılmış CSV dosyaları (~ 48 GB sıkıştırılmamış), 173 milyondan fazla bireysel geziler ve her yolculuk için ödenen ücretler kayıt yaklaşık 20 GB oluşur. Her seyahat kaydı, pikap ve bırakma yerleri ve saatleri, anonim hack (sürücü) lisans numarası ve madalyon (taksinin benzersiz kimlik) numarasını içerir. Veriler 2013 yılındaki tüm gezileri kapsar ve her ay için aşağıdaki iki veri kümesinde sağlanır:

'trip_data' CSV yolcu sayısı, teslim alma ve bırakma noktaları, seyahat süresi ve seyahat uzunluğu gibi seyahat ayrıntılarını içerir. Aşağıda birkaç örnek kayıt veörnekleri verebleri verebleri bulabilirsiniz:

       medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868



'trip_fare' CSV, ödeme türü, ücret tutarı, ek ücret ve vergiler, bahşişler ve geçiş ücretleri ve ödenen toplam tutar gibi her seyahat için ödenen ücretin ayrıntılarını içerir. Aşağıda birkaç örnek kayıt veörnekleri verebleri verebleri bulabilirsiniz:

       medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat\_verilerine ve seyahat\_ücretine katılmak için benzersiz anahtar aşağıdaki\_üç alandan oluşur: madalyon, hack lisansı ve teslim alma\_tarihi. Ham CSV dosyalarına Azure Depolama blob'undan erişilebilir. Bu birleştirme için U-SQL komut [dosyası, Yolculuk ve ücret tablolarına katıl](#join) bölümünde yer almaktadır.

## <a name="process-data-with-u-sql"></a>U-SQL ile veri işleme
Bu bölümde gösterilen veri işleme görevleri, verileri yutma, kaliteyi denetleme, keşfetme ve örneklemeyi içerir. Gezi ve ücret tablolarına nasıl katılacağız da gösterilmiştir. Son bölümde, Azure portalından U-SQL komut dosyası iş çalıştırılır. Burada her alt bölüme bağlantılar şunlardır:

* [Veri alımı: genel blob verileri okuyun](#ingest)
* [Veri kalitesi kontrolleri](#quality)
* [Veri arama](#explore)
* [Seyahat ve ücret tablolarına katılın](#join)
* [Veri örneklemesi](#sample)
* [U-SQL işlerini çalıştırma](#run)

U-SQL komut dosyaları burada açıklanır ve ayrı bir dosyada sağlanır. [GitHub'dan](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough)tam **U-SQL komut dosyalarını** indirebilirsiniz.

U-SQL,Open Visual Studio'>, **Dosya -> Yeni --> Project'i**tıklatın, **U-SQL Project'i**seçin, adını belirleyin ve bir klasöre kaydedin.

![8](./media/data-lake-walkthrough/8-create-USQL-project.PNG)

> [!NOTE]
> Visual Studio yerine U-SQL yürütmek için Azure Portalını kullanmak mümkündür. Portaldaki Azure Veri Gölü Analizi kaynağına gidebilir ve sorguları aşağıdaki şekilde gösterildiği gibi doğrudan gönderebilirsiniz:
>
>

![9](./media/data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="data-ingestion-read-in-data-from-public-blob"></a><a name="ingest"></a>Veri Alımı: Genel blob verileri okuyun

Azure blob'daki verilerin **konumu, name.blob.core.windows.net/blob_name wasb://container\_\@ad\_blob depolama\_hesabı\_** olarak başvurulur ve **Extractors.Csv()** kullanılarak ayıklanabilir. \_Wasb adresindeki konteyner adı\@blob\_depolama\_hesabı\_adı için aşağıdaki komut dosyalarında kendi kapsayıcı adınızı ve depolama hesap adınızı değiştirin. Dosya adları aynı biçimde olduğundan, 12 gezi dosyasının tümlerinde okumak için **gezi\_verilerini\_\{\*\}.csv** kullanmak mümkündür.

    ///Read in Trip data
    @trip0 =
        EXTRACT
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

İlk satırda üstbilgi olduğundan, üstbilgi kaldırmanız ve sütun türlerini uygun başlıklar halinde değiştirmeniz gerekir. İşlenen verileri **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ kullanarak Azure Veri Gölü Depolamasına veya **wasb://container_name\@blob_storage_account_name.blob.core.windows.net/blob_name**kullanarak Azure Blob depolama hesabına kaydedebilirsiniz.

    // change data types
    @trip =
        SELECT
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv();

    ////Output data to blob
    OUTPUT @trip
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();

Benzer şekilde ücret veri kümelerini de okuyabilirsiniz. Sağ tıkladığınızda Azure Veri Gölü Depolama'ya, Visual Studio'daki Veri Gezgini veya **Dosya Gezgini** **> Azure portalında** verilerinize bakmayı seçebilirsiniz.

 ![10](./media/data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/data-lake-walkthrough/11-data-in-ADL.PNG)

### <a name="data-quality-checks"></a><a name="quality"></a>Veri kalitesi kontrolleri
Gezi ve ücret tabloları okunduktan sonra veri kalitesi kontrolleri aşağıdaki şekilde yapılabilir. Ortaya çıkan CSV dosyaları Azure Blob depolamasına veya Azure Veri Gölü Depolamasına çıktı olabilir.

Madalyon sayısını ve eşsiz madalyon sayısını bulun:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;

    @ex_1 =
        SELECT
        pickup_month,
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv();

100'den fazla seyahati olan madalyonları bulun:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv();

pickup_longitude açısından bu geçersiz kayıtları bulun:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv();

Bazı değişkenler için eksik değerleri bulma:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;

    @trip_summary6 =
        SELECT
            vendor_id,
        SUM(missing_medallion) AS medallion_empty,
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="data-exploration"></a><a name="explore"></a>Veri arama
Verileri daha iyi anlamak için aşağıdaki komut dosyalarıyla bazı veri aramaları yapın.

Uçlu ve uçlu olmayan gezilerin dağılımını bulun:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;

    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv();

0, 5, 10 ve 20 dolar: kesme değerleri ile uç miktarı dağılımını bulun.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv();

Seyahat mesafesinin temel istatistiklerini bulun:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Seyahat mesafesinin yüzdelik artışlarını bulun:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv();


### <a name="join-trip-and-fare-tables"></a><a name="join"></a>Seyahat ve ücret tablolarına katılın
Gezi ve ücret tablolarıma madalyon, hack_license ve pickup_time ile katılabilir.

    //join trip and fare table

    @model_data_full =
    SELECT t.*,
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv();

    ////output data to ADL
    OUTPUT @model_data_full
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv();


Yolcu sayısının her düzeyi için, kayıt sayısını, ortalama bahşiş miktarını, bahşiş miktarının farkını, uçlu seyahatlerin yüzdesini hesaplayın.

    // contingency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="data-sampling"></a><a name="sample"></a>Veri örneklemesi
İlk olarak, birleştirilmiş tablodaki verilerin %0,1'ini rasgele seçin:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;

    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;

    OUTPUT @model_data_random_sample_1_1000
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv();

Sonra ikili değişken tip_class tarafından katmanlı örnekleme yapın:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;

    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv();
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv();


### <a name="run-u-sql-jobs"></a><a name="run"></a>U-SQL işlerini çalıştırma
U-SQL komut dosyalarını düzenlemeden sonra, Azure Veri Gölü Analizi hesabınızı kullanarak bunları sunucuya gönderebilirsiniz. **Veri Gölü'ne**tıklayın, **İş Gönder'i**tıklatın, **Analitik Hesabınızı**seçin, **Paralellik'i**seçin ve **Gönder** düğmesini tıklatın.

 ![12](./media/data-lake-walkthrough/12-submit-USQL.PNG)

İş başarıyla yerine geldiğinde, işinizin durumu izlemek için Visual Studio'da görüntülenir. İş tamamlandıktan sonra, iş yürütme işlemini yeniden oynatabilir ve iş verimliliğinizi artırmak için darboğaz adımlarını öğrenebilirsiniz. U-SQL işlerinizin durumunu denetlemek için Azure portalına da gidebilirsiniz.

 ![13](./media/data-lake-walkthrough/13-USQL-running-v2.PNG)

 ![14](./media/data-lake-walkthrough/14-USQL-jobs-portal.PNG)

Artık çıktı dosyalarını Azure Blob depolama veya Azure portalında denetleyebilirsiniz. Bir sonraki adımda modellememiz için katmanlı örnek verileri kullanın.

 ![15](./media/data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)

## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Azure Machine Learning'de modeller oluşturun ve dağıtın
Oluşturmak ve oluşturmak için Azure Machine Learning'e veri çekmeniz için iki seçenek mevcuttur

* İlk seçenekte, bir Azure Blob'una yazılmış örneklenmiş verileri (yukarıdaki **Veri örnekleme** adımında) ve Azure Machine Learning'den modeller oluşturmak ve dağıtmak için Python'u kullanırsınız.
* İkinci seçenekte, Azure Veri Gölü'ndeki verileri doğrudan bir Kovan sorgusu kullanarak sorgularsınız. Bu seçenek, yeni bir HDInsight kümesi oluşturmanızı veya Hive tablolarının Azure Veri Gölü Depolama'daki NY Taksi verilerini işaret ettiği varolan bir HDInsight kümesini kullanmanızı gerektirir.  Bu seçeneklerin her ikisi de aşağıdaki bölümlerde ele alınmıştır.

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Seçenek 1: Makine öğrenimi modelleri oluşturmak ve dağıtmak için Python'u kullanın
Python'u kullanarak makine öğrenimi modelleri oluşturmak ve dağıtmak için yerel makinenizde veya Azure Machine Learning Studio'da bir Jupyter Notebook oluşturun. [GitHub'da](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) sağlanan Jupyter Notebook, verileri keşfetmek, görselleştirmek, özellik mühendisliği, modelleme ve dağıtım için tam kodu içerir. Bu makalede, sadece modelleme ve dağıtım kapsamındadır.

### <a name="import-python-libraries"></a>Python kitaplıklarını içe aktarma
Jupyter Notebook veya Python komut dosyası dosyasını çalıştırmak için aşağıdaki Python paketleri gereklidir. Azure Machine Learning Notebook hizmetini kullanıyorsanız, bu paketler önceden yüklenmiş.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Blob'dan gelen verileri okuyun
* Bağlantı Dizesi

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
* Metin olarak okuma

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

  ![17](./media/data-lake-walkthrough/17-python_readin_csv.PNG)
* Sütun adları ve ayrı sütunlar ekleme

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
* Bazı sütunları sayısal olarak değiştirme

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Makine öğrenme modelleri oluşturun
Burada, bir gezinin uçlu olup olmadığını tahmin etmek için bir ikili sınıflandırma modeli oluşturursunuz. Jupyter Notebook'ta diğer iki modeli bulabilirsiniz: çok sınıflı sınıflandırma ve regresyon modelleri.

* Öncelikle scikit-learn modellerinde kullanılabilecek sahte değişkenler oluşturmanız gerekir

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')
* Modelleme için veri çerçevesi oluşturma

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])

        X = data.iloc[:,1:]
        Y = data.tipped
* Eğitim ve test 60-40 bölünmüş

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)
* Eğitim setinde Lojistik Regresyon

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/data-lake-walkthrough/c1-py-logit-coefficient.PNG)
* Puan testi veri seti

        Y_test_pred = logit_fit.predict(X_test)
* Değerlendirme ölçümlerini hesapla

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train

        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred)
        print fpr_test, tpr_test, thresholds_test

        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)

        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/data-lake-walkthrough/c2-py-logit-evaluation.PNG)

### <a name="build-web-service-api-and-consume-it-in-python"></a>Web Hizmeti API'si oluşturun ve Python'da tüketin
Makine öğrenme modelini üretildikten sonra işlevsel hale getirmek istiyorsunuz. İkili lojistik modeli burada örnek olarak kullanılır. Yerel makinenizdeki scikit-learn sürümünün 0.15.1 olduğundan emin olun (Azure Machine Learning Studio zaten en azından bu sürümde).

* Azure Machine Learning Studio (klasik) ayarlarından çalışma alanı kimlik bilgilerinizi bulun. Azure Machine Learning Studio'da **Ayarlar** --> **Adı** --> **Yetkilendirme Belirteçleri'ni**tıklatın.

    ![c3](./media/data-lake-walkthrough/c3-workspace-id.PNG)

        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

* Web Hizmeti Oluştur

        @services.publish(workspaceid, auth_token)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)
* Web hizmeti kimlik bilgilerini alma

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key

        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass
* Web hizmeti API'yi arayın. Genellikle, önceki adımdan sonra 5-10 saniye bekleyin.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/data-lake-walkthrough/c4-call-API.PNG)

## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Seçenek 2: Doğrudan Azure Machine Learning'de modeller oluşturma ve dağıtma
Azure Machine Learning Studio (klasik), verileri doğrudan Azure Veri Gölü Depolama'dan okuyabilir ve ardından modeller oluşturmak ve dağıtmak için kullanılabilir. Bu yaklaşım, Azure Veri Gölü Depolama'yı işaret eden bir Hive tablosu kullanır. Hive tablosu için ayrı bir Azure HDInsight kümesinin sağlanması gerekir. 

### <a name="create-an-hdinsight-linux-cluster"></a>HDInsight Linux Kümesi Oluşturma
[Azure portalından](https://portal.azure.com)bir HDInsight Kümesi (Linux) oluşturun. Ayrıntılar için, [Azure portalını kullanarak Data Lake Store ile HDInsight kümesi oluştur](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)bölümünde Azure Veri Gölü Depolama **bölümüne erişen bir HDInsight kümesi** oluşturun'a bakın.

 ![18](./media/data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>HDInsight'ta Kovan tablosu oluşturma
Şimdi, önceki adımda Azure Veri Gölü Depolama'da depolanan verileri kullanarak HDInsight kümesindeki Azure Machine Learning Studio'da (klasik) kullanılmak üzere Hive tabloları oluşturuyorsunuz. Oluşturulan HDInsight kümesine gidin. **Ayarlar** --> **Özellikleri** --> **Kümesi AAD Identity** --> **ADLS Access'i**tıklatın , Azure Veri Gölü Depolama hesabınızın okuma, yazma ve yürütme haklarıyla listeye eklandığından emin olun.

 ![19](./media/data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)

Ardından **Ayarlar** düğmesinin yanındaki **Pano'yu** tıklatın ve bir pencere açılır. Sayfanın sağ üst köşesinde **Kovan Görünümü'ne** tıklayın ve Sorgu Düzenleyicisi'ni görmeniz gerekir. **Query Editor**

 ![20](./media/data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)

Tablo oluşturmak için aşağıdaki Hive komut dosyalarını yapıştırın. Veri kaynağının konumu Azure Veri Gölü Depolama başvurusuşuyla' dır: **adl://data_lake_store_name.azuredatalakestore.net:443/folder_name/file_name**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Sorgu tamamlandığında, aşağıdaki gibi sonuçları görmeniz gerekir:

 ![22](./media/data-lake-walkthrough/22-Hive-Query-results.PNG)

### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da modeller oluşturun ve dağıtın
Artık Azure Machine Learning ile bir ipucunun ödenip ödenmediğini tahmin eden bir model oluşturmaya ve dağıtmaya hazırsınız. Katmanlı örnek lem verileri bu ikili sınıflandırma (uç veya değil) sorununda kullanılmaya hazırdır. Çok sınıflı sınıflandırma (tip_class) ve regresyon (tip_amount) kullanan tahmine dayalı modeller, Azure Machine Learning Studio ile de oluşturulabilir ve dağıtılabilir, ancak burada yalnızca ikili sınıflandırma modelini kullanarak servis talebinin nasıl işleyeceğiniz gösterilir.

1. **Verileri, Veri Girişi ve Çıktısı** bölümünde bulunan **Alma Verileri** modüllerini kullanarak Azure Machine Learning Studio'ya (klasik) alın. Daha fazla bilgi için [Veri İçe Aktar modülü](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) başvuru sayfasına bakın.
2. **Özellikler** panelinde **Veri kaynağı** olarak **Kovan Sorgusu'nu** seçin.
3. **Hive veritabanı sorgu** düzenleyicisinde aşağıdaki Hive komut dosyasını yapıştırın

        select * from nyc_stratified_sample;
4. HDInsight kümesinin URI'sini (bu URI Azure portalında bulunabilir), Hadoop kimlik bilgilerini, çıktı verilerinin konumunu ve Azure Depolama hesap adı/anahtarı/kapsayıcı adını girin.

   ![23](./media/data-lake-walkthrough/23-reader-module-v3.PNG)

Kovan tablosundaki ikili sınıflandırma deneyi okuma verilerine bir örnek aşağıdaki şekilde gösterilmiştir:

 ![24](./media/data-lake-walkthrough/24-AML-exp.PNG)

Deneme oluşturulduktan sonra Web Hizmeti --> Tahmin Edici Web**Hizmeti** **Ayarla'yı**tıklatın

 ![25](./media/data-lake-walkthrough/25-AML-exp-deploy.PNG)

Otomatik olarak oluşturulan puanlama denemesini çalıştırın, bittiğinde **Web Hizmetini Dağıt'ı** tıklatın

 ![26](./media/data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Web hizmeti panosu kısa bir süre görüntülenir:

 ![27](./media/data-lake-walkthrough/27-AML-web-api.PNG)

## <a name="summary"></a>Özet
Bu izbiyi tamamlayarak, Azure Veri Gölü'nde ölçeklenebilir uçtan uca çözümler oluşturmak için bir veri bilimi ortamı oluşturdunuz. Bu ortam, veri toplamadan model eğitimine ve modelin bir web hizmeti olarak dağıtılmasına kadar, Veri Bilimi Süreci'nin kanonik adımlarından geçerek büyük bir genel veri kümesini analiz etmek için kullanılmıştır. U-SQL verileri işlemek, araştırmak ve örneklemek için kullanıldı. Python ve Hive, tahmine dayalı modeller oluşturmak ve dağıtmak için Azure Machine Learning Studio (klasik) ile kullanıldı.

## <a name="whats-next"></a>Sırada ne var?
[Ekip Veri Bilimi Süreci (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) için öğrenme yolu, gelişmiş analitik sürecindeki her adımı açıklayan konulara bağlantılar sağlar. Çeşitli tahmine dayalı analitik senaryolarda kaynakların ve hizmetlerin nasıl kullanılacağını gösteren [Ekip Veri Bilimi Süreci izthroughleri](walkthroughs.md) sayfasında ayrıntılı bir dizi izlenme vardır:

* [Takım Veri Bilimi Süreci iş başında: SQL Veri Ambarı kullanma](sqldw-walkthrough.md)
* [Ekip Veri Bilimi Süreci iş başında: HDInsight Hadoop kümelerini kullanarak](hive-walkthrough.md)
* [Takım Veri Bilimi Süreci: SQL Server kullanarak](sql-walkthrough.md)
* [Azure HDInsight'ta Kıvılcım kullanarak Veri Bilimi Sürecine Genel Bakış](spark-overview.md)
