---
title: SQL Server VM'de bir model oluşturma ve dağıtma - Takım Veri Bilimi Süreci
description: Azure VM'de, herkese açık bir veri kümesiyle SQL Server'ı kullanarak bir makine öğrenme modeli oluşturun ve dağıtın.
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
ms.openlocfilehash: a47f30cf00624faf098c8b605534cf355eacadee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251589"
---
# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Takım Veri Bilimi Süreci iş başında: SQL Server kullanarak
Bu öğreticide, SQL Server ve kamuya açık bir veri seti olan [NYC Taxi Trips](https://www.andresmh.com/nyctaxitrips/) veri kümesini kullanarak bir makine öğrenimi modeli oluşturma ve dağıtma işlemini yürütür. Prosedür standart bir veri bilimi iş akışını izler: verileri yutmak ve keşfetmek, öğrenmeyi kolaylaştırmak için mühendislik özellikleri, sonra oluşturmak ve bir model dağıtmak.

## <a name="nyc-taxi-trips-dataset-description"></a><a name="dataset"></a>NYC Taksi Gezileri Dataset Açıklama
NYC Taksi Trip veri sıkıştırılmış CSV dosyaları yaklaşık 20 GB (~ 48 GB sıkıştırılmamış), 173 milyondan fazla bireysel geziler ve her yolculuk için ödenen ücretler oluşan. Her seyahat kaydı pickup ve bırakma yeri ve saati, anonim hack (sürücü) lisans numarası ve madalyon (taksinin benzersiz kimlik) numarasını içerir. Veriler 2013 yılındaki tüm gezileri kapsar ve her ay için aşağıdaki iki veri kümesinde sağlanır:

1. 'trip_data' CSV yolcu sayısı, teslim alma ve bırakma noktaları, seyahat süresi ve seyahat uzunluğu gibi seyahat ayrıntılarını içerir. Aşağıda birkaç örnek kayıt veörnekleri verebleri verebleri bulabilirsiniz:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. 'trip_fare' CSV, ödeme türü, ücret tutarı, ek ücret ve vergiler, bahşişler ve geçiş ücretleri ve ödenen toplam tutar gibi her seyahat için ödenen ücretin ayrıntılarını içerir. Aşağıda birkaç örnek kayıt veörnekleri verebleri verebleri bulabilirsiniz:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat\_verilerine ve seyahat\_ücretine katılmak için eşsiz anahtar\_alanlardan\_oluşur: madalyon, hack lisansı ve teslim alma tarihi.

## <a name="examples-of-prediction-tasks"></a><a name="mltasks"></a>Tahmin Görevleri Örnekleri
Bahşiş *\_miktarına*göre üç tahmin problemi formüle edeceğiz, yani:

* İkili sınıflandırma: Bir seyahat için bir bahşişin ödenip ödenmediğini, yani 0 TL'den büyük bir *bahşiş\_tutarının* olumlu bir örnek olduğunu, 0 TL'lik *\_bahşiş tutarının* ise olumsuz bir örnek olduğunu tahmin edin.
* Çok sınıflı sınıflandırma: Yolculuk için ödenen bahşiş aralığını tahmin etmek. *\_İpucu tutarını* beş kutuya veya sınıfa böleriz:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
* Regresyon görevi: Bir yolculuk için ödenen bahşiş miktarını tahmin etmek.  

## <a name="setting-up-the-azure-data-science-environment-for-advanced-analytics"></a><a name="setup"></a>Gelişmiş analitik için Azure veri bilimi ortamını ayarlama
[Çevrenizi Planla](plan-your-environment.md) kılavuzundan da görebileceğiniz gibi, Azure'daki NYC Taksi Gezileri veri kümesiyle çalışmak için birkaç seçenek vardır:

* Azure blobs'taki verilerle çalışın ve Azure Machine Learning'de modellik edin
* Azure Machine Learning'de verileri SQL Server veritabanına yükleyin ve ardından model

Bu eğitimde, sql Server Management Studio'yu kullanarak ve IPython Notebook kullanarak verilerin paralel toplu olarak SQL Server'a aktarımını, veri araştırmasını, özellik mühendisliğini ve aşağı örneklemesini göstereceğiz. [Örnek komut dosyaları](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) ve [IPython dizüstü bilgisayarlar](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) GitHub'da paylaşılır. Azure lekeleri verileriyle çalışmak için örnek bir IPython dizüstü bilgisayar da aynı konumda kullanılabilir.

Azure Veri Bilimi ortamınızı ayarlamak için:

1. [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md)
2. [Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md)
3. Sql Server ve IPython Notebook sunucusu sağlayan [bir Veri Bilimi Sanal Makine'yi sağlama.](../data-science-virtual-machine/setup-sql-server-virtual-machine.md)
   
   > [!NOTE]
   > Örnek komut dosyaları ve IPython dizüstü bilgisayarlar kurulum işlemi sırasında Veri Bilimi sanal makinenize indirilir. VM yükleme sonrası komut dosyası tamamlandığında, örnekler VM'nin Belgeler kitaplığında olacaktır:  
   > 
   > * Örnek Komut Dosyaları:`C:\Users\<user_name>\Documents\Data Science Scripts`  
   > * Örnek IPython Dizüstü Bilgisayarlar:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
   >   VM'inizin Windows giriş adı nerededir. `<user_name>` Örnek klasörlere Örnek Komut **Dosyaları** ve **Örnek IPython Not Defterleri**olarak atıfta bulunacağız.
   > 
   > 

Veri kümesi boyutuna, veri kaynağı konumuna ve seçili Azure hedef ortamına bağlı olarak, bu senaryo [Senaryo \#5'e benzer: Yerel dosyalardaki büyük veri kümesi, Azure VM'de SQL Server'ı hedefleyin.](plan-sample-scenarios.md#largelocaltodb)

## <a name="get-the-data-from-public-source"></a><a name="getdata"></a>Verileri Ortak Kaynaktan Alın
[NYC Taksi Gezileri](https://www.andresmh.com/nyctaxitrips/) veri kümesini genel konumundan almak için, verileri yeni sanal makinenize kopyalamak için Verileri Azure [Blob Depolama'ya taşıy'ta](move-azure-blob.md) açıklanan yöntemlerden herhangi birini kullanabilirsiniz.

AzCopy kullanarak verileri kopyalamak için:

1. Sanal makinenizde oturum açın (VM)
2. VM'nin veri diskinde yeni bir dizin oluşturun (Not: VM ile birlikte gelen Geçici Disk'i Veri Diski olarak kullanmayın).
3. Komut İstemi penceresinde, <path_to_data_folder> (2) oluşturulan veri klasörünüzle değiştirerek aşağıdaki Azcopy komut satırını çalıştırın:
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    AzCopy tamamlandığında, toplam 24 sıkıştırılmış CSV dosyaları (12\_gezi verileri için\_ve 12 trip ücreti için) veri klasöründe olmalıdır.
4. İndirilen dosyaların zip'ini aç. Sıkıştırılmamış dosyaların bulunduğu klasöre dikkat edin. Bu\_klasör,\_veri\_dosyalarına\>giden <yolu olarak anılacaktır.

## <a name="bulk-import-data-into-sql-server-database"></a><a name="dbload"></a>SQL Server Veritabanına Toplu Alma Verileri
Büyük miktarda veriyi sql veritabanına yükleme/aktarma performansı ve sonraki sorgular *Bölümlenmiş Tablolar ve Görünümler*kullanılarak geliştirilebilir. Bu bölümde, yeni bir veritabanı oluşturmak ve verileri paralel olarak bölümlenmiş tablolara yüklemek için [PARALEL Toplu Veri Alma'da açıklanan](parallel-load-sql-partitioned-tables.md) yönergeleri takip edeceğiz.

1. VM'nizde oturum açtığınızda **SQL Server Management Studio'yı**başlatın.
2. Windows Kimlik Doğrulaması'nı kullanarak bağlanın.
   
    ![SSMS Bağlan][12]
3. SQL Server kimlik doğrulama modunu henüz değiştirmediyseniz ve yeni bir SQL giriş kullanıcısı oluşturduysanız, **Örnek Komut Dosyaları** klasöründe **change\_auth.sql** adlı komut dosyası dosyasını açın. Varsayılan kullanıcı adını ve parolayı değiştirin. Komut dosyasını çalıştırmak için araç çubuğunda **Yürüt'e** tıklayın.
   
    ![Komut Dosyalarını Çalıştır][13]
4. Yeni oluşturulan veritabanlarının bir Veri Diski'nde depolanmasından emin olmak için SQL Server varsayılan veritabanını ve günlük klasörlerini doğrulayın ve/veya değiştirin. Veri depolama yükleri için optimize edilmiş SQL Server VM görüntüsü, veri ve günlük diskleriyle önceden yapılandırılmıştır. VM'niz bir Veri Diski içermediyse ve VM kurulum işlemi sırasında yeni sanal sabit diskler eklediyseniz, varsayılan klasörleri aşağıdaki gibi değiştirin:
   
   * Sol paneldeki SQL Server adını sağ tıklatın ve **Özellikler'i**tıklatın.
     
       ![SQL Server Özellikleri][14]
   * Soldaki sayfa listesini **seçin'den** **Veritabanı Ayarları'nı** seçin.
   * **Veritabanı varsayılan konumlarını** seçtiğiniz Veri **Diski** konumları ile doğrulayın ve/veya değiştirin. Bu konum, varsayılan ayarlarla oluşturulmuşsa yeni veritabanlarının bulunduğu yerdir.
     
       ![SQL Veritabanı Varsayılanları][15]  
5. Bölümlenmiş tabloları tutmak için yeni bir veritabanı ve dosya grupları kümesi oluşturmak için, örnek komut dosyası **db\_\_default.sql oluşturun açın.** Komut dosyası, varsayılan veri konumunda **TaxiNYC** adında yeni bir veritabanı ve 12 dosya grubu oluşturur. Her dosya grubu bir aylık\_seyahat\_verilerini ve seyahat ücreti verilerini tutar. İstenirse veritabanı adını değiştirin. Komut dosyasını çalıştırmak için **Çalıştır'ı** tıklatın.
6. Ardından, biri seyahat verileri için\_diğeri de seyahat\_ücreti için olmak üzere iki bölüm tabloları oluşturun. Örnek komut dosyası açın **bölümlenmiş\_\_tablo.sql oluşturmak**, hangi olacak:
   
   * Verileri aya göre bölmek için bir bölüm işlevi oluşturun.
   * Her ayın verilerini farklı bir dosya grubuyla eşlemek için bir bölüm düzeni oluşturun.
   * Bölüm şemasına eşlenmiş iki bölümlü tablo oluşturun: nyctaxi\_gezisi seyahat verilerini tutacak\_ve **\_nyctaxi** **\_ücreti** seyahat ücreti verilerini tutacak.
     
     Komut dosyasını çalıştırmak ve bölümlenmiş tabloları oluşturmak için **Yürüt'ü** tıklatın.
7. Örnek **Komut Dosyaları** klasöründe, SQL Server tablolarına paralel toplu veri aktarımlarını göstermek için sağlanan iki örnek PowerShell komut dosyası vardır.
   
   * **bcp\_\_parallel generic.ps1,** toplu alma verilerini tabloya paralel olarak genel bir komut dosyasıdır. Giriş ve hedef değişkenleri komut dosyasındaki açıklama satırlarında belirtildiği şekilde ayarlamak için bu komut dosyasını değiştirin.
   * **bcp\_\_paralel nyctaxi.ps1** genel komut dosyasının önceden yapılandırılmış bir sürümüdür ve NYC Taksi Gezileri verileri için her iki tabloyu yüklemek için kullanılabilir.  
8. **BCP\_paralel\_nyctaxi.ps1** komut dosyası adını sağ tıklatın ve PowerShell'de açmak için **Edit'e** tıklayın. Önceden ayarlanmış değişkenleri gözden geçirin ve seçtiğiniz veritabanı adına, giriş veri klasörüne, hedef günlük klasörüne ve örnek format dosyalarına giden yollara göre değiştirin **nyctaxi_trip.xml** ve **nyctaxi\_fare.xml** **(Örnek Komut Dosyaları** klasöründe sağlanmıştır).
   
    ![Toplu Alma Verileri][16]
   
    Ayrıca kimlik doğrulama modunu seçebilirsiniz, varsayılan Windows Kimlik Doğrulama olduğunu. Çalıştırmak için araç çubuğundaki yeşil oku tıklatın. Komut dosyası, her bölümlenmiş tablo için 12 olmak üzere paralel olarak 24 toplu alma işlemi başlatacaktır. Yukarıda ayarlanan SQL Server varsayılan veri klasörünü açarak veri alma ilerlemesini izleyebilirsiniz.
9. PowerShell komut dosyası başlangıç ve bitiş saatlerini bildirir. Tüm toplu içeri aktarımlar tamamlandığında, bitiş saati bildirilir. Toplu aktarımların başarılı olduğunu doğrulamak için hedef günlük klasörünü, yani hedef günlük klasöründe hata bildirilmemiştir.
10. Veritabanınız artık arama, özellik mühendisliği ve diğer işlemler için istenilen şekilde hazırdır. Tablolar **teslim\_alma tarihi** alanına göre bölümlere ayrıldığından, **WHERE** yan tümcesinde teslim **alma\_tarih saati** koşullarını içeren sorgular bölümleme düzeninden yararlanır.
11. **SQL Server Management Studio'da,** sağlanan örnek komut dosyası örnek **\_sorgularını keşfedin.sql**. Örnek sorgulardan herhangi birini çalıştırmak için sorgu satırlarını vurgulayın ve araç çubuğunda **Yürüt'ü** tıklatın.
12. NYC Taksi Gezileri verileri iki ayrı tabloya yüklenir. Birleştirme işlemlerini geliştirmek için tabloları dizine eklemeönerilir. Örnek komut dosyası **\_bölümlenmiş\_index.sql oluşturmak** bileşik birleştirme anahtar **madalyon,\_hack\_lisans ve alma datetime**bölümlenmiş dizinler oluşturur.

## <a name="data-exploration-and-feature-engineering-in-sql-server"></a><a name="dbexplore"></a>SQL Server'da Veri Arama ve Özellik Mühendisliği
Bu bölümde, daha önce oluşturulan SQL Server veritabanını kullanarak doğrudan **SQL Server Management Studio'da SQL** sorguları çalıştırarak veri arama ve özellik oluşturma gerçekleştireceğiz. **Örnek Komut Dosyaları** klasöründe örnek **\_sorgular.sql** adlı örnek komut dosyası sağlanır. Varsayılandan farklıysa veritabanı adını değiştirmek için komut dosyasını değiştirin: **TaxiNYC**.

Bu alıştırmada, biz olacak:

* Windows Kimlik Doğrulaması'nı veya SQL Kimlik Doğrulaması'nı ve SQL oturum açma adını ve parolasını kullanarak **SQL Server Management Studio'ya** bağlanın.
* Değişen zaman pencerelerinde birkaç alanın veri dağılımlarını keşfedin.
* Boylam ve enlem alanlarının veri kalitesini araştırın.
* **Uç miktarına\_** göre ikili ve çok sınıflı sınıflandırma etiketleri oluşturun.
* Özellikler oluşturun ve yolculuk mesafelerini hesaplayın/karşılaştırın.
* İki tabloya katılın ve modeller oluşturmak için kullanılacak rastgele bir örnek ayıklayın.

Azure Machine Learning'e geçmeye hazır olduğunuzda şunları da hissedebilirsin  

1. Verileri ayıklamak ve örneklemek ve sorguyu doğrudan Azure Machine Learning'de [Bir Alma Verimodülüne][import-data] kopyalamak için son SQL sorgusunu kaydedin veya
2. Yeni bir veritabanı tablosunda model oluşturma için kullanmayı planladığınız örneklenmiş ve tasarlanmış verileri devam edin ve Azure Machine Learning'de [Veri Alma][import-data] modülündeki yeni tabloyu kullanın.

Bu bölümde, verileri ayıklamak ve örneklemek için son sorguyu kaydedeceğiz. İkinci [yöntem, IPython Notebook bölümündeki Veri Arama ve Özellik Mühendisliği bölümünde](#ipnb) gösterilmiştir.

Paralel toplu alma kullanılarak daha önce doldurulan tablolardaki satır ve sütun sayısının hızlı bir şekilde doğrulanması için,

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Keşif: Madalyon ile gezi dağılımı
Bu örnek, belirli bir süre içinde 100'den fazla seyahat ile madalyon (taksi numaraları) tanımlar. Bu **\_alma datetime**bölümleme düzeni tarafından koşullandırılmış olduğundan sorgu bölümlenmiş tablo erişimi yararlanacaktır. Tam veri kümesini sorgulamak, bölümlenmiş tablo ve/veya dizin tarar'ı da kullanır.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hack_license"></a>Keşif: Madalyon ve hack_license göre gezi dağılımı
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Veri Kalitesi Değerlendirmesi: Kayıtları yanlış boylam ve/veya enlemle doğrulayın
Bu örnek, boylam ve/veya enlem alanlarından herhangi birinin geçersiz bir değer esahip olup olmadığını (radyan dereceleri -90 ile 90 arasında olmalıdır) veya (0, 0) koordinatlara sahip olup olmadığını araştırır.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Keşif: Uçlu vs Uçlu Değil Uçlu Trips dağıtım
Bu örnek, belirli bir zaman diliminde (veya tam yılı kapsıyorsa tam veri kümesinde) uçlu ve bahşiş verilmeyen seyahat lerin sayısını bulur. Bu dağılım, daha sonra ikili sınıflandırma modellemesi için kullanılacak ikili etiket dağılımını yansıtır.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Keşif: İpucu Sınıfı/Menzil Dağılımı
Bu örnek, belirli bir zaman diliminde (veya tam yılı kapsıyorsa tam veri kümesinde) uç aralıklarının dağılımını hesaplar. Etiket sınıflarının bu dağılımı daha sonra çok sınıflı sınıflandırma modellemesi için kullanılacaktır.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Keşif: Hesaplama ve Seyahat Mesafesi karşılaştırın
Bu örnek, alma ve bırakma boylaşunu ve enlemisql coğrafya noktalarına dönüştürür, SQL coğrafya puan farkını kullanarak yolculuk mesafesini hesaplanır ve karşılaştırma sonuçlarının rasgele bir örneğini döndürür. Örnek, sonuçları yalnızca daha önce kapsanan veri kalitesi değerlendirme sorgusunu kullanarak geçerli koordinatlarla sınırlar.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>SQL Sorgularında Özellik Mühendisliği
Etiket oluşturma ve coğrafya dönüşüm arama sorguları, sayım kısmını kaldırarak etiket/özellik oluşturmak için de kullanılabilir. [IPython Notebook bölümündeveri arama ve özellik mühendisliği](#ipnb) bölümünde ek özellik mühendisliği SQL örnekleri verilmiştir. Doğrudan SQL Server veritabanı örneğinde çalışan SQL sorgularını kullanarak özellik oluşturma sorgularını tam veri kümesinde veya büyük bir alt kümesinde çalıştırmak daha verimlidir. Sorgular SQL Server **Management Studio,** IPython Notebook veya veritabanına yerel veya uzaktan erişebilen herhangi bir geliştirme aracı veya ortamında yürütülebilir.

#### <a name="preparing-data-for-model-building"></a>Model Oluşturma Için Veri Hazırlama
Aşağıdaki sorgu **\_nyctaxi gezisi** ve **nyctaxi\_ücret** tabloları katılır, bir ikili sınıflandırma etiketi **uçlu**oluşturur , çok sınıflı sınıflandırma etiket ipucu **\_sınıfı**, ve tam birleştirilmiş veri kümesinden% 1 rasgele örnek ayıklar. Bu sorgu, Azure'daki SQL Server veritabanı örneğinden doğrudan veri alımı için doğrudan [Azure Machine Learning Studio](https://studio.azureml.net) Alma [Veri][import-data] modülüne kopyalanabilir. Sorgu, yanlış (0, 0) koordinatları olan kayıtları hariç tutar.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="data-exploration-and-feature-engineering-in-ipython-notebook"></a><a name="ipnb"></a>IPython Notebook'ta Veri Arama ve Özellik Mühendisliği
Bu bölümde, daha önce oluşturulan SQL Server veritabanına karşı python ve SQL sorgularını kullanarak veri arama ve özellik oluşturma gerçekleştireceğiz. **Örnek IPython Notebook'lar** klasöründe **machine-Learning-data-science-process-sql-story.ipynb** adlı örnek bir IPython dizüstü bilgisayarı sağlanır. Bu dizüstü bilgisayar [GitHub'da](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks)da mevcuttur.

Büyük verilerle çalışırken, önerilen şu sırayı izleyin:

* Verilerin küçük bir örneğinde bellek içi bir veri çerçevesi ne kadar okunur.
* Örneklenmiş verileri kullanarak bazı görselleştirmeler ve keşifler gerçekleştirin.
* Örneklenmiş verileri kullanarak özellik mühendisliği ile denemeler.
* Daha büyük veri arama, veri işleme ve özellik mühendisliği için, SQL Sorgularını doğrudan Azure VM'deki SQL Server veritabanına karşı vermek için Python'u kullanın.
* Azure Machine Learning model oluşturma için kullanılacak örnek boyutuna karar verin.

Azure Machine Learning'e geçmeye hazır olduğunuzda şunları da hissedebilirsin  

1. Verileri ayıklamak ve örneklemek ve sorguyu doğrudan Azure Machine Learning'de [Bir Alma Verimodülüne][import-data] kopyalamak için son SQL sorgusunu kaydedin. Bu yöntem, Azure [Machine Learning](#mlmodel) bölümünde Yapı Modelleri bölümünde gösterilmiştir.    
2. Yeni bir veritabanı tablosunda model oluşturma için kullanmayı planladığınız örneklenmiş ve tasarlanmış verileri devam edin ve ardından [Veri Alma][import-data] modülündeki yeni tabloyu kullanın.

Aşağıda birkaç veri arama, veri görselleştirme ve özellik mühendisliği örnekleri verilmiştir. Daha fazla örnek için Örnek **IPython Notebooklar** klasöründeki örnek SQL IPython not defterine bakın.

#### <a name="initialize-database-credentials"></a>Veritabanı Kimlik Bilgilerini Başlatma
Veritabanı bağlantı ayarlarınızı aşağıdaki değişkenlerde başlatma:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Veritabanı Bağlantısı Oluşturma
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxi_trip"></a>Tablonyctaxi_trip satır ve sütun sayısını bildirin
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Toplam satır sayısı = 173179759  
* Toplam sütun sayısı = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>SQL Server Veritabanından küçük bir veri örneği okuyun
    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Örnek tabloyu okuma süresi 6,492000 saniye  
Alınan satır ve sütun sayısı = (84952, 21)

#### <a name="descriptive-statistics"></a>Açıklayıcı İstatistikler
Şimdi örneklenmiş verileri keşfetmeye hazır. **Biz yolculuk\_mesafesi** (veya başka bir) alan(lar) için açıklayıcı istatistikler bakarak başlar:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Görselleştirme: Kutu ÇizimI Örneği
Daha sonra quantiles görselleştirmek için yolculuk mesafesi için kutu arsa bakmak

    df1.boxplot(column='trip_distance',return_type='dict')

![Arsa #1][1]

#### <a name="visualization-distribution-plot-example"></a>Görselleştirme: Dağıtım Konusu Örneği
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Arsa #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Görselleştirme: Çubuk ve Çizgi Çizimleri
Bu örnekte, yolculuk mesafesini beş kutuya sığdırıyoruz ve binning sonuçlarını görselleştiriyoruz.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Yukarıdaki çöp kutusu dağılımını aşağıdaki gibi bir çubuk veya çizgi çiziminde çizebiliriz

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Arsa #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Arsa #4][4]

#### <a name="visualization-scatterplot-example"></a>Görselleştirme: Scatterplot Örneği
Herhangi bir korelasyon olup olmadığını görmek için **\_\_\_secs** ve **\_gezi mesafesi** içinde yolculuk süresi arasında dağılım arsa göstermek

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Arsa #6][6]

Benzer **şekilde, fiyat\_kodu** ve **yolculuk\_mesafesi**arasındaki ilişkiyi kontrol edebiliriz.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Arsa #8][8]

### <a name="sub-sampling-the-data-in-sql"></a>SQL'de Verilerin Alt Örneklemesi
[Azure Machine Learning Studio'da](https://studio.azureml.net)model oluşturma için veri hazırlarken, **SQL sorgusunda doğrudan İçe Alma Verimodülünde kullanılmasına** karar verebilir veya yeni [Import Data][import-data] tabloda basit bir SELECT * FROM **<\_yeni\_tablo\_adınızı>** kullanabileceğiniz yeni bir tabloda tasarlanmış ve örneklenmiş verileri devam edebilirsiniz.

Bu bölümde, örneklenen ve tasarlanmış verileri tutmak için yeni bir tablo oluşturacağız. SQL Server bölümündeki [Veri Arama ve Özellik Mühendisliği'nde](#dbexplore) model oluşturma için doğrudan SQL sorgusuörneği verilmiştir.

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Örnek Tablo oluşturun ve Birleştirilmiş Tabloların %1'i ile Doldurma. Varsa Önce Tabloyu Bırak.
Bu bölümde, biz tablolar **\_nyctaxi trip** ve **nyctaxi\_ücret**katılmak , bir% 1 rasgele örnek ayıklamak, ve yeni bir tablo adı **nyctaxi\_bir\_yüzde**örneklenmiş veri devam:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>IPython Notebook'ta SQL Sorgularını Kullanarak Veri Arama
Bu bölümde, yukarıda oluşturduğumuz yeni tabloda kalıcı olan %1 örneklenmiş verileri kullanarak veri dağılımlarını inceleyeceğiz. Benzer keşifler orijinal tablolar kullanılarak, isteğe bağlı olarak arama örneğini sınırlamak için **TABLESAMPLE** kullanılarak veya sonuçları SQL Server bölümündeki [Veri Arama ve Özellik Mühendisliği'nde](#dbexplore) gösterildiği gibi **\_toplama tarih bölümleri** kullanılarak belirli bir zaman dilimiyle sınırlandırarak gerçekleştirilebilir.

#### <a name="exploration-daily-distribution-of-trips"></a>Keşif: Gezilerin günlük dağılımı
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Keşif: Madalyon başına gezi dağılımı
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>IPython Notebook'ta SQL Sorgularını Kullanma Özellik Oluşturma
Bu bölümde, bir önceki bölümde oluşturduğumuz %1'lik örnek tabloda çalışan SQL sorgularını kullanarak doğrudan yeni etiketler ve özellikler oluşturacağız.

#### <a name="label-generation-generate-class-labels"></a>Etiket Oluşturma: Sınıf Etiketleri Oluştur
Aşağıdaki örnekte, modelleme için kullanılacak iki etiket kümesi oluştururuz:

1. İkili Sınıf **Etiketleri uçlu** (bir ipucu verilip verilmeyeceğini tahmin etme)
2. Çok Sınıflı Etiketler **ipucu\_sınıfı** (uç kutusu veya aralığı tahmin etme)
   
        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''
   
        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()
   
        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''
   
        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Özellik Mühendisliği: Kategorik Sütunlar için Sayma Özellikleri
Bu örnek, her kategoriyi verilerdeki oluşum sayısıyla değiştirerek kategorik bir alanı sayısal bir alana dönüştürür.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Özellik Mühendisliği: Sayısal Sütunlar için Bin özellikleri
Bu örnek, sürekli sayısal alanı önceden ayarlanmış kategori aralıklarına, yani sayısal alanı kategorik bir alana dönüştürür.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Özellik Mühendisliği: Ondalık Enlem/Boylamdan Konum Özelliklerini Ayıkla
Bu örnek, bir enlem ve/veya boylam alanının ondalık gösterimini ülke/bölge, şehir, kasaba, blok vb. gibi farklı tanecikli birden çok bölge alanına ayırır. Yeni coğrafi alanlar gerçek konumlara eşlenmez. Coğrafi kod konumları eşleme hakkında bilgi için [Bing Maps REST Services'e](https://msdn.microsoft.com/library/ff701710.aspx)bakın.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Featurized tablonun son şeklini doğrulayın
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

[Azure Machine Learning'de](https://studio.azureml.net)model oluşturma ve model dağıtımına geçmeye hazırız. Veriler, daha önce tanımlanan tahmin sorunlarından herhangi biri için hazırdır:

1. İkili sınıflandırma: Bir seyahat için bahşiş ödenip ödenmediğini tahmin etmek.
2. Çok sınıflı sınıflandırma: Daha önce tanımlanan sınıflara göre ödenen bahşiş aralığını tahmin etmek.
3. Regresyon görevi: Bir yolculuk için ödenen bahşiş miktarını tahmin etmek.  

## <a name="building-models-in-azure-machine-learning"></a><a name="mlmodel"></a>Azure Makine Öğreniminde Modeller Oluşturma
Modelleme alıştırmasına başlamak için Azure Machine Learning çalışma alanınızda oturum açın. Henüz bir makine öğrenme çalışma alanı oluşturmadıysanız, [bkz.](../studio/create-workspace.md)

1. Azure Machine Learning'e başlamak için Azure [Machine Learning Studio nedir?](../studio/what-is-ml-studio.md)
2. [Azure Machine Learning Studio'ya](https://studio.azureml.net)giriş yapın.
3. Studio Home sayfası bilgi, video, öğreticiler, Modüller Referans bağlantılar ve diğer kaynakların bir zenginlik sağlar. Azure Machine Learning hakkında daha fazla bilgi için [Azure Machine Learning Dokümantasyon Merkezi'ne](https://azure.microsoft.com/documentation/services/machine-learning/)başvurun.

Tipik bir eğitim deneyi aşağıdaki adımlardan oluşur:

1. **+Yenİ** deneme oluşturun.
2. Verileri Azure Machine Learning'e taşıyın.
3. Verileri gerektiği gibi önceden işleme, dönüştürme ve işleme.
4. Gerektiğinde özellikler oluşturun.
5. Verileri eğitim/doğrulama/test veri kümelerine bölün (veya her biri için ayrı veri kümelerine sahip oyla) ayırın.
6. Çözmek için öğrenme sorununa bağlı olarak bir veya daha fazla makine öğrenme algoritması seçin. Örneğin, ikili sınıflandırma, çok sınıflı sınıflandırma, regresyon.
7. Eğitim veri kümesini kullanarak bir veya daha fazla model eğitin.
8. Eğitilmiş modeli(ler) kullanarak doğrulama veri kümesini puan.
9. Öğrenme sorunu yla ilgili ölçümleri hesaplamak için modeli(ler) değerlendirin.
10. Modeli(ler) ayarlayın ve dağıtmak için en iyi modeli seçin.

Bu alıştırmada, SQL Server'daki verileri araştırdık ve tasarladık ve Azure Machine Learning'de yutmak için örnek boyutuna karar verdik. Tahmin modellerinden birini veya daha fazlasını oluşturmak için karar verdik:

1. **Verileri, Veri Girişi ve Çıktısı** bölümünde bulunan [Alma Verileri][import-data] modüllerini kullanarak Azure Machine Learning'e alın. Daha fazla bilgi için [Veri İçe Aktar][import-data] modülü başvuru sayfasına bakın.
   
    ![Azure Machine Learning Alma Verileri][17]
2. **Özellikler** panelinde **Veri kaynağı** olarak Azure **SQL Veritabanı'nı** seçin.
3. **Veritabanı sunucu adı** alanına veritabanı DNS adını girin. Biçim:`tcp:<your_virtual_machine_DNS_name>,1433`
4. İlgili alana **Veritabanı adını** girin.
5. Server **kullanıcı** hesabı adına SQL kullanıcı **adını**ve Server kullanıcı **hesabı parolasına** **parolayı** girin.
7. Veritabanı **sorgu** metnini edit alanında, gerekli veritabanı alanlarını ayıklayan sorguyu (etiketler gibi tüm işlemalanları dahil) yapıştırın ve verileri istenen örnek boyutuna indirin.

Doğrudan SQL Server veritabanından veri okuma ikili sınıflandırma deney örneği aşağıdaki şekildedir. Benzer deneyler çok sınıflı sınıflandırma ve regresyon sorunları için oluşturulabilir.

![Azure Machine Learning Train][10]

> [!IMPORTANT]
> Önceki bölümlerde sağlanan modelleme veri ayıklama ve örnekleme sorgusu örneklerinde, **üç modelleme alıştırması için tüm etiketler sorguya dahil edilir.** Modelleme çalışmalarının her birinde önemli bir (gerekli) adım, diğer iki sorun ve diğer **hedef sızıntıları**için gereksiz etiketleri **dışlamaktır.** Örneğin, ikili sınıflandırma kullanırken, **uçlu** etiketi kullanın ve alanları **\_ihbar sınıfı,** **uç\_miktarı**ve **toplam\_tutarı**hariç. İkincisi, bahşişin ödendiğini ima ettikleri için hedef sızıntılarıdır.
> 
> Gereksiz sütunları ve/veya hedef sızıntılarını dışlamak [için, Veri Kümesi modülündeki Sütunları Seç'i][select-columns] veya [Meta Verilerini Edit'i][edit-metadata]kullanabilirsiniz. Daha fazla bilgi için Bkz. [Veri Kümesi'ndeki Sütunları Seç][select-columns] ve Meta veri başvuru sayfalarını [edin.][edit-metadata]
> 
> 

## <a name="deploying-models-in-azure-machine-learning"></a><a name="mldeploy"></a>Azure Makine Öğreniminde Modelleri Dağıtma
Modeliniz hazır olduğunda, doğrudan denemeden web hizmeti olarak kolayca dağıtabilirsiniz. Azure Machine Learning web hizmetlerini dağıtma hakkında daha fazla bilgi [için](../studio/deploy-a-machine-learning-web-service.md)bkz.

Yeni bir web hizmeti dağıtmak için şunları yapmanız gerekir:

1. Bir puanlama deneyi oluşturun.
2. Web hizmetini dağıtın.

**Bitmiş** bir eğitim deneyinden bir puanlama denemesi oluşturmak için, alt eylem çubuğunda **CREATE SCORING EXPERIMENT'i** tıklatın.

![Azure Puanlama][18]

Azure Machine Learning, eğitim deneyinin bileşenlerine dayalı bir puanlama denemesi oluşturmaya çalışır. Özellikle, olacak:

1. Eğitimli modeli kaydedin ve model eğitim modüllerini kaldırın.
2. Beklenen giriş veri şemasını temsil etmek için mantıksal bir **giriş bağlantı noktası** tanımlayın.
3. Beklenen web hizmeti çıktı şemasını temsil edecek mantıksal bir **çıktı bağlantı noktası** tanımlayın.

Puanlama denemesi oluşturulduğunda, gözden geçirin ve gerektiği gibi ayarlayın. Tipik bir ayarlama, giriş veri kümesini ve/veya sorguyu etiket alanlarını dışlayan bir aygıtla değiştirmektir, çünkü bu etiketler hizmet çağrıldığında şemada kullanılamaz. Giriş veri kümesinin ve/veya sorgunun boyutunu birkaç kayda düşürmek de iyi bir uygulamadır ve giriş şemasını belirtmek için yeterlidir. Çıkış bağlantı noktası için, tüm giriş alanlarını dışlamak ve [yalnızca Dataset modülündeki Sütunları Seç'i][select-columns] kullanarak çıktıya **Yalnızca Puanlı Etiketler** ve **Puanlı Olasılıkları** eklemek yaygındır.

Örnek puanlama deneyi aşağıdaki şekildedir. Dağıtmaya hazır olduğunuzda, alt eylem çubuğundaki **WEB SERVICE'i yayımla** düğmesini tıklatın.

![Azure Machine Learning Publish][11]

Özetlemek gerekirse, bu izbiş öğreticisinde, bir Azure veri bilimi ortamı oluşturdunuz, veri toplamadan model eğitimine ve azure machine learning web hizmetinin dağıtımına kadar geniş bir genel veri kümesiyle çalıştınız.

### <a name="license-information"></a>Lisans Bilgileri
Bu örnek gözden geçirme ve beraberindeki komut dosyaları ve IPython not defterleri Microsoft tarafından MIT lisansı altında paylaşılır. Daha fazla bilgi için GitHub'daki örnek kodun dizinindeki LICENSE.txt dosyasını kontrol edin.

### <a name="references"></a>Başvurular
• [Andrés Monroy NYC Taksi Gezileri İndirme Sayfası](https://www.andresmh.com/nyctaxitrips/)  
• [Chris Whong tarafından NYC's Taksi Trip Veri FOILing](https://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taksi ve Limuzin Komisyonu Araştırma ve İstatistik](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

[1]: ./media/sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/sql-walkthrough/azuremltrain.png
[11]: ./media/sql-walkthrough/azuremlpublish.png
[12]: ./media/sql-walkthrough/ssmsconnect.png
[13]: ./media/sql-walkthrough/executescript.png
[14]: ./media/sql-walkthrough/sqlserverproperties.png
[15]: ./media/sql-walkthrough/sqldefaultdirs.png
[16]: ./media/sql-walkthrough/bulkimport.png
[17]: ./media/sql-walkthrough/amlreader.png
[18]: ./media/sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
