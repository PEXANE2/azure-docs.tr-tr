---
title: Derleme ve bir SQL Server VM - Team Data Science Process model dağıtma
description: Oluşturun ve makine öğrenme modeli herkese bir veri kümesi ile Azure VM'de SQL Server'ı kullanarak dağıtın.
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
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78393409"
---
# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Team Data Science Process'in çalışması: SQL Server'ı kullanma
Bu öğreticide, SQL Server ve genel kullanıma açık bir veri kümesi ( [NYC TAXI gezileri](https://www.andresmh.com/nyctaxitrips/) veri kümesi) kullanarak makine öğrenimi modeli oluşturma ve dağıtma sürecini adım adım ilerleyebilirsiniz. Standart veri bilimi iş akışı yordamdan sonraki: alma ve verileri, mühendislik işlevlerini, öğrenme süreçlerini kolaylaştırmasına sonra yapı ve model dağıtma keşfedin.

## <a name="dataset"></a>NYC TAXI gidiş veri kümesi açıklaması
NYC TAXI seyahat verileri, 173.000.000 ' den fazla ayrı ve her seyahat için ödenen farktan fazla 20 GB sıkıştırılmış CSV dosyası (~ 48 GB sıkıştırılmamış) ile ilgilidir. Her bir seyahat kaydı alma ve dropoff konumu zaman, anonimleştirilmiş hack (sürücü) lisans numarası ve medallion (taksi'nın benzersiz tanımlayıcı) numarasını içerir. Veriler tüm dönüş 2013 yılında kapsar ve aşağıdaki iki veri kümesi için her ay sağlanır:

1. 'trip_data' CSV Yolcuların, toplama ve dropoff noktaları, seyahat süresini ve seyahat uzunluğu sayısı gibi seyahat ayrıntıları içerir. Birkaç örnek kayıt şunlardır:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. 'trip_fare' CSV ödeme türü, taksi tutar, ek ücret ve vergiler, ipuçları ve Ücretli geçişler, gibi her seyahat için ücretli taksi ve ödenen toplam tutarı ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat\_veri ve seyahat\_tarifeli havayolu katılacak benzersiz anahtar, şu alanlardan oluşur: medtalon, Hack\_lisans ve toplama\_tarih saat.

## <a name="mltasks"></a>Tahmin görevlerinin örnekleri
*İpucu\_miktarına*göre üç tahmin sorunu ifade edeceğiz, yani:

* İkili sınıflandırma: bir ucun seyahat için mi ödendiğini, diğer bir deyişle $0 ' den büyük bir ipucunu *\_* pozitif bir örnek olduğunu tahmin ederken, *İpucu\_$0 miktarı* negatif bir örnektir.
* Sınıflı sınıflandırma: ipucu için seyahat Ücretli aralığını tahmin etmek için. *İpucu\_miktarını* beş veya daha fazla sınıfa böyoruz:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
* Regresyon görev: bir seyahat için ücretli ipucu miktarını tahmin edin.  

## <a name="setup"></a>Gelişmiş analiz için Azure veri bilimi ortamını ayarlama
[Ortam kılavuzumuzu planınızdan](plan-your-environment.md) görebileceğiniz gibi, Azure 'da NYC TAXI gidiş veri kümesiyle çalışmak için birkaç seçenek vardır:

* Daha sonra Azure Machine learning'de model verileri Azure BLOB'ları ile çalışma
* Verileri bir SQL Server veritabanı sonra Azure Machine learning'de model yüklemek

Bu öğreticide, SQL Server Management Studio ve IPython Not defteri 'ni kullanarak, verilerin SQL Server, veri araştırmasına, özellik mühendisine ve aşağı örneklemeye yönelik paralel toplu içeri aktarmayı göstereceğiz. [Örnek betikler](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) ve [IPython Not defterleri](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) GitHub 'da paylaşılır. Bir örnek Ipython notebook'ın Azure bloblarındaki verilerden çalışmak için de aynı konumda kullanılabilir.

Azure veri bilimi ortamı oluşturmanız için:

1. [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md)
2. [Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md)
3. Bir SQL Server ve bir IPython Not defteri sunucusu sağlayan [veri bilimi sanal makinesi sağlayın](../data-science-virtual-machine/setup-sql-server-virtual-machine.md).
   
   > [!NOTE]
   > Örnek betikler ve Ipython not defterleri Kurulum işlemi sırasında veri bilimi sanal makinenize indirilir. Örnekler, VM yükleme sonrası betik tamamlandığında, sanal makinenizin Belge Kitaplığı'nda şöyle olacaktır:  
   > 
   > * Örnek betikler: `C:\Users\<user_name>\Documents\Data Science Scripts`  
   > * Örnek IPython Not defterleri: `C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
   >   Burada `<user_name>`, sanal makinenizin Windows oturum açma adıdır. Örnek klasörlere örnek **betikler** ve örnek **IPython Not defterleri**olarak başvuracağız.
   > 
   > 

Veri kümesi boyutu, veri kaynağı konumu ve seçili Azure hedef ortamı temel alınarak, bu senaryo [senaryo \#5: yerel dosyalardaki büyük veri kümesi, Azure VM 'de hedef SQL Server](plan-sample-scenarios.md#largelocaltodb)benzerdir.

## <a name="getdata"></a>Genel kaynaktaki verileri al
[NYC TAXI gidiş](https://www.andresmh.com/nyctaxitrips/) veri kümesini genel konumundan almak için, verileri yeni sanal makinenize kopyalamak üzere [Azure Blob Storage 'a veri taşıma](move-azure-blob.md) bölümünde açıklanan yöntemlerden herhangi birini kullanabilirsiniz.

AzCopy kullanarak verileri kopyalamak için:

1. Sanal makinenize (VM) oturum açın
2. VM 'nin veri diskinde yeni bir dizin oluşturun (Not: VM ile birlikte gelen geçici diski bir veri diski olarak kullanmayın).
3. Bir komut istemi penceresinde aşağıdaki Azcopy komutunu (2)'de oluşturulan veri klasörünüz < path_to_data_folder > yerine satırı çalıştırın:
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    AzCopy tamamlandığında, toplam 24 daraltılmış CSV dosyası (seyahat için 12\_verileri ve seyahat için 12\_tarifeli havayolu) veri klasöründe olmalıdır.
4. İndirilen dosyaları ayıklayın. Sıkıştırılması kaldırılan dosyaların bulunduğu klasör unutmayın. Bu klasör, veri\_\>dosyaları\_\_< yol olarak adlandırılacaktır.

## <a name="dbload"></a>Verileri SQL Server veritabanına toplu olarak Içeri aktarma
Bir SQL veritabanına büyük miktarlarda veri yükleme/aktarma performansı ve sonraki sorgularda *bölümlenmiş tablolar ve görünümler*kullanılarak iyileştirilen. Bu bölümde, yeni bir veritabanı oluşturmak ve verileri bölümlenmiş tablolara paralel olarak yüklemek için [SQL bölüm tablolarını kullanarak paralel toplu veri Içeri aktarma](parallel-load-sql-partitioned-tables.md) bölümünde açıklanan yönergeleri takip edeceğiz.

1. SANAL makinenizde oturum açıp **SQL Server Management Studio**başlatın.
2. Windows kimlik doğrulaması kullanarak bağlanın.
   
    ![SSMS bağlanma][12]
3. SQL Server kimlik doğrulama modunu henüz değiştirmediyseniz ve yeni bir SQL oturum açma kullanıcısı oluşturduysanız, **örnek betikler** klasöründe **\_Auth. SQL** adlı betik dosyasını açın. Varsayılan kullanıcı adı ve parolasını değiştirin. Betiği çalıştırmak için araç çubuğunda **Yürüt** ' e tıklayın.
   
    ![Komut dosyası yürütme][13]
4. Doğrulayın ve/veya veritabanlarını yeni oluşturulmuş bir veri diski depolanmış emin olmak için SQL Server varsayılan veritabanı ve günlük klasörleri değiştirme. Veri ambarı yüklemeleri için en iyi duruma getirilmiş SQL Server VM görüntüsü, veri ve günlük disklerle önceden yapılandırılmıştır. Sanal makinenizin veri diski dahil edilmemiştir ve yeni sanal sabit diskler VM Kurulum işlemi sırasında eklenen, varsayılan klasörler gibi değiştirin:
   
   * Sol panelde SQL Server adına sağ tıklayın ve **Özellikler**' e tıklayın.
     
       ![SQL Server özellikleri][14]
   * Sol taraftaki **bir sayfa seçin** listesinden **veritabanı ayarları** ' nı seçin.
   * **Veritabanı varsayılan konumlarını** doğrulayın ve/veya seçtiğiniz **veri diski** konumlarına göre değiştirin. Bu konum, varsayılan ayarlarla oluşturulduysa yeni veritabanlarının bulunduğu yerdir.
     
       ![SQL veritabanı Varsayılanları][15]  
5. Yeni bir veritabanı ve bölümlenmiş tabloları tutacak bir dosya grubu kümesi oluşturmak için örnek betik **oluştur\_db\_default. SQL**' yi açın. Betik, varsayılan veri konumunda **Taxınrivc** ve 12 dosya grupları adlı yeni bir veritabanı oluşturur. Her dosya grubu, veri\_seyahat eden bir ay tutar ve\_verileri seyahat eder. Veritabanı adı, isterseniz değiştirin. Betiği çalıştırmak için **Yürüt** ' e tıklayın.
6. Daha sonra, bir diğeri seyahat\_verileri için bir tane olmak üzere iki bölüm tablosu oluşturun ve seyahat\_tarifeli havayolu. Örnek betik **oluştur\_bölümlenmiş\_Table. SQL**' i açın ve şunları yapın:
   
   * Verileri aya göre bölmek için bölümleme işlevi oluşturun.
   * Her aya ait verileri farklı bir dosya grubuna eşlemek için bir bölüm düzeni oluşturun.
   * Bölüm düzenine eşlenmiş iki bölümlenmiş tablo oluşturun: **nyctaxi\_seyahat** , seyahat\_verileri ve **nyctaxi\_tarifeli havayolu** seyahat\_tarifeli havayolu verilerini tutacaktır.
     
     Betiği çalıştırmak ve bölümlenmiş tabloları oluşturmak için **Yürüt** ' e tıklayın.
7. **Örnek komut dosyaları** klasöründe, SQL Server tablolarına paralel toplu veri içeri aktarmaları göstermek için Iki örnek PowerShell komut dosyası sağlanır.
   
   * **bcp\_parallel\_Generic. ps1** , paralel toplu içe verileri bir tabloya aktarmak için genel bir betiktir. Komut dosyasındaki yorum satırlarında belirtildiği gibi giriş ve hedef değişkenlerini ayarlamak için bu betiği değiştirin.
   * **bcp\_parallel\_nyctaxi. ps1** , genel betiğin önceden yapılandırılmış bir sürümüdür ve NYC TAXI verileri için her iki tabloyu da yüklemek üzere kullanılabilir.  
8. **Bcp\_parallel\_nyctaxi. ps1** betik adına sağ tıklayın ve PowerShell 'de açmak için **Düzenle** ' ye tıklayın. Önceden ayarlanmış değişkenleri gözden geçirin ve seçtiğiniz veritabanı adı, giriş veri klasörü, hedef günlük klasörü ve örnek biçim dosyalarına göre değiştirin **nyctaxi_trip. xml** ve **nyctaxi\_tarifeli havayolu. xml** ( **örnek betikler** klasöründe verilmiştir).
   
    ![Toplu veri içeri aktar][16]
   
    Kimlik doğrulama modu da seçebilirsiniz, varsayılan Windows kimlik doğrulaması. Çalıştırmak için araç çubuğundaki yeşil oka tıklayın. Betik 24 toplu içeri aktarma işlemlerinde paralel 12 bölümlenmiş her tablo için başlatılır. Veri içeri aktarma ilerlemesi kümesi olarak yukarıdaki SQL Server varsayılan veri klasörü açarak izleyebilirsiniz.
9. PowerShell Betiği, başlangıç ve bitiş saatleri bildirir. Tüm toplu içeri aktarmalar tamamlandı, bitiş saati bildirilir. Toplu içeri aktarmaların başarılı olduğunu, yani hedef günlük klasöründe bildirilen bir hata olmadığını doğrulamak için hedef günlük klasörünü denetleyin.
10. Veritabanı keşfi, özellik Mühendisliği ve istediğiniz gibi diğer işlemler için artık hazırdır. Tablolar **toplama\_tarih saat** alanına göre bölümlendiğinden, **WHERE** yan tümcesindeki **toplama\_tarih saat** koşullarını içeren sorgular, bölüm düzeninden yarar olacaktır.
11. **SQL Server Management Studio**, belirtilen örnek betik **örneği\_sorguları. SQL**' i gezin. Örnek sorgulardan herhangi birini çalıştırmak için, sorgu satırlarını vurgulayın, ardından araç çubuğunda **Yürüt** ' e tıklayın.
12. NYC taksi dönüş verileri iki ayrı tablolarda yüklenir. Birleştirme işlemleri iyileştirmek için dizin tabloları için önerilir. Örnek betik, **create\_bölümlenmiş\_dizin. SQL** bileşik JOIN anahtarında bölümlendirilmiş **, Hack\_lisansı ve toplama\_Tarih/saatteki**dizinler oluşturur.

## <a name="dbexplore"></a>SQL Server 'de veri araştırması ve özellik Mühendisliği
Bu bölümde, daha önce oluşturulan SQL Server veritabanını kullanarak **SQL Server Management Studio** doğrudan SQL sorguları çalıştırarak veri keşif ve özellik oluşturmayı gerçekleştirecağız. Örnek **betikler** klasöründe örnek **\_sorguları. SQL** adlı örnek bir komut dosyası verilmiştir. Komut dosyasını, varsayılan: **Taxınyc**değerinden farklıysa veritabanı adını değiştirecek şekilde değiştirin.

Bu alıştırmada yapacağız:

* Windows kimlik doğrulamasını veya SQL kimlik doğrulaması 'nı ve SQL oturum açma adını ve parolasını kullanarak **SQL Server Management Studio** bağlanın.
* Değişen zaman pencereleri bazı alanların veri dağıtımları keşfedin.
* Veri Kalitesi boylam ve enlem alanlarının araştırın.
* **İpucu\_miktarına**göre ikili ve çoklu sınıf sınıflandırma etiketleri oluşturun.
* Özellikleri oluşturma ve işlem/seyahat uzaklıkları karşılaştırın.
* İki tabloyu birleştirmek ve modelleri oluşturmak için kullanılan rastgele bir örnek ayıklayın.

Azure Machine Learning için devam etmeye hazır olduğunuzda, şunlardan birini yapabilirsiniz:  

1. Verileri ayıklamak ve örneklendirme ve kopyalamak için son SQL sorgusunu kaydedin ve sorguyu Azure Machine Learning içindeki bir [Içeri aktarma veri][import-data] modülüne doğrudan yapıştırın veya
2. Model oluşturma için kullanmayı planladığınız örneklenmiş ve uygulanan verileri yeni bir veritabanı tablosunda kalıcı hale getirin ve Azure Machine Learning [verileri Içeri aktarma][import-data] modülünde yeni tabloyu kullanın.

Bu bölümde, verileri ayıklamak ve örneklemek için son sorguyu kaydedecağız. İkinci yöntem, [IPython Not defteri 'Ndeki veri araştırma ve özellik Mühendisliği](#ipnb) bölümünde gösterilmiştir.

Daha önce paralel toplu olarak içeri aktarma kullanarak doldurulmuş tablo içindeki satırları ve sütunları sayısını hızlı bir doğrulama için

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>İnceleme: Seyahat dağıtım medallion tarafından
Bu örnek, belirli bir süre içinde 100'den fazla gelişlerin medallion (taksi numaraları) tanımlar. Sorgu, **\_Tarih/saatteki**bölüm şeması tarafından koşullu olduğundan, bölümlenmiş tablo erişiminizden faydalanır. Tam bir veri kümesinin sorgulanmasını ayrıca bölümlenmiş tablosunu kullanmak ve/veya dizin tarama yapar.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hack_license"></a>İnceleme: Seyahat dağıtım medallion ve hack_license
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Veri Kalitesi değerlendirme: yanlış boylam ve/veya enlem kayıtlarla doğrulayın
Herhangi bir boylam ve/veya enlem alanı ya da geçersiz bir değer içeriyorsa, bu örnekte araştırır (radian derece -90 ile 90 arasında olmalıdır), veya (0, 0) koordinatları.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Araştırma: eğimli ve eğimli dönüşler dağıtımı
Bu örnek karşılaştırması belirli bir zaman dönemi (veya kapsayan tam yıl, tam veri kümesi) Eğimli değil Eğimli dönüş sayısı bulur. Bu dağıtım için ikili sınıflandırma modelleme daha sonra kullanılmak üzere ikili etiket dağılımı yansıtır.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>İnceleme: Sınıf/aralığı dağıtım İpucu
Bu örnek, belirli bir süre içinde (veya kapsayan tam yıl, tam veri kümesi) dağıtım ipucu aralıklarının hesaplar. Bu etiket sınıflarının dağıtımı, daha sonra çok sınıflı sınıflandırma modelleme için kullanılacaktır.

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

#### <a name="exploration-compute-and-compare-trip-distance"></a>İnceleme: İşlem ve seyahat uzaklık karşılaştırın
Bu örnek dönüştürür alma ve dropoff boylam ve enlem SQL coğrafi konum için işaret, SQL Coğrafya noktaları fark kullanarak seyahat uzaklığı hesaplar ve sonuçları karşılaştırma için rastgele oluşturulmuş bir örnek döndürür. Bu örnek yalnızca daha önce veri kalitesi değerlendirme sorgusu kullanarak geçerli koordinat sonuçları sınırlar.

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

#### <a name="feature-engineering-in-sql-queries"></a>SQL sorguları, özellik Mühendisliği
Etiket oluşturma ve geography dönüştürme araştırma sorgular sayım bölümü kaldırarak etiketleri/özellikler oluşturmak için de kullanılabilir. Ek özellik Mühendisliği SQL örnekleri, [IPython Not defteri 'Ndeki veri araştırma ve özellik Mühendisliği](#ipnb) bölümünde verilmiştir. Özellik oluşturma sorgularını tam veri kümesinde veya doğrudan SQL Server veritabanı örneğinde çalışan SQL sorgularını kullanarak büyük bir alt kümesinde çalıştırmak daha etkilidir. Sorgular **SQL Server Management Studio**, IPython Not defteri 'nde veya veritabanına yerel olarak veya uzaktan erişebilen herhangi bir geliştirme aracında veya ortamda çalıştırılabilir.

#### <a name="preparing-data-for-model-building"></a>Veri modeli oluşturma için hazırlanıyor
Aşağıdaki sorgu **nyctaxi\_seyahat** ve **nyctaxi\_tarifeli havayolu** tablolarını birleştirir, **eğimli**bir ikili sınıflandırma etiketi, çok sınıf sınıflandırma etiketi **İpucu\_sınıfı**oluşturur ve tam olarak birleştirilmiş veri kümesinden bir %1 rastgele örnek ayıklar. Bu sorgu, doğrudan Azure 'daki SQL Server veritabanı örneğinden doğrudan veri alımı için [Azure Machine Learning Studio](https://studio.azureml.net) [veri alma][import-data] modülüne doğrudan yapıştırılabilir. Sorgu yanlış kayıtlarla dışlar (0, 0) koordinatları.

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


## <a name="ipnb"></a>IPython not defterinde veri araştırması ve özellik Mühendisliği
Bu bölümde, biz veri keşfi ve daha önce oluşturduğunuz SQL Server veritabanında hem Python hem de SQL sorgularını kullanarak özellik nesil gerçekleştirir. **Machine-Learning-Data-Science-Process-SQL-öykü. ipynb** adlı örnek bir IPython Not defteri, **örnek IPython Not defterleri** klasöründe verilmiştir. Bu not defteri [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks)'da da kullanılabilir.

Büyük verilerle çalışırken önerilen bu sırayı izleyin:

* Verilerin küçük bir örnek, bir bellek içi veri çerçevesine okuyun.
* Bazı görselleştirmeler ve araştırmaları örneklenen verileri kullanarak gerçekleştirin.
* Örnek verileri kullanarak özellik Mühendisliği ile denemeler yapın.
* Büyük veri keşfi, veri işleme ve özellik Mühendisliği için doğrudan Azure VM'de SQL Server veritabanında SQL sorguları göndermek amacıyla Python'ı kullanın.
* Azure Machine Learning modeli yapı için kullanılacak örnek boyutunu karar verin.

Azure Machine Learning için devam etmeye hazır olduğunuzda, şunlardan birini yapabilirsiniz:  

1. Verileri ayıklamak ve örnekleyin ve kopyalamak için son SQL sorgusunu kaydedin ve sorguyu doğrudan Azure Machine Learning [Içeri aktarma verileri][import-data] modülüne yapıştırın. Bu yöntem, [Azure Machine Learning model oluşturma](#mlmodel) bölümünde gösterilmiştir.    
2. Model oluşturma için kullanmayı planladığınız örneklenmiş ve uygulanan verileri yeni bir veritabanı tablosunda kalıcı hale getirin ve ardından [verileri Içeri aktarma][import-data] modülündeki yeni tabloyu kullanın.

Birkaç veri keşfi, veri Görselleştirme ve örnekler mühendislik özelliğini aşağıda verilmiştir. Daha fazla örnek için **örnek IPython Not defterleri** KLASÖRÜNDEKI örnek SQL IPython Not defteri ' ne bakın.

#### <a name="initialize-database-credentials"></a>Veritabanı kimlik bilgileri Başlat
Veritabanı bağlantı ayarlarınızı aşağıdaki değişkenleri başlatın:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Veritabanı bağlantısı oluşturma
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxi_trip"></a>Tablo nyctaxi_trip içindeki satırları ve sütunları rapor sayısı
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

* Toplam satır sayısı 173179759 =  
* Toplam sütun sayısı = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Okuma-içinde SQL Server veritabanından bir küçük veri örneği
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

Örnek tablo 6.492000 saniyedir okuma süresi  
Satır ve sütun sayısı, alınan = (84952, 21)

#### <a name="descriptive-statistics"></a>Açıklayıcı istatistikleri
Artık, örneklenen verileri araştırmak hazırsınız. **Seyahat\_uzaklık** (veya diğer) alanları için açıklayıcı istatistiklere bakmaya başlıyoruz:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Görselleştirme: Kutusu Diagram örneği
Sonraki Kutu Çizimi quantiles görselleştirmek seyahat uzaklığı için şu konuları

    df1.boxplot(column='trip_distance',return_type='dict')

![#1 Çiz][1]

#### <a name="visualization-distribution-plot-example"></a>Görselleştirme: Dağıtım Diagram örneği
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![#2 Çiz][2]

#### <a name="visualization-bar-and-line-plots"></a>Görselleştirme: Çubuk ve çizgi çizimler
Bu örnekte, beş depo seyahat uzaklığı bin ve Gruplama sonuçları görselleştirebilirsiniz.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Biz de yukarıdaki bin dağıtım çubuğundaki çizim veya aşağıda gösterildiği gibi çizim satır

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 Çiz][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 Çiz][4]

#### <a name="visualization-scatterplot-example"></a>Görselleştirme: Dağılım grafiği örnek
**\_saniye içinde seyahat\_zaman\_** arasında dağılım çizimi gösteririz ve herhangi bir bağıntı olup olmadığını görmek için **seyahat\_mesafe**

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 Çiz][6]

Benzer şekilde, **oran\_kod** ve **seyahat\_uzaklığı**arasındaki ilişkiyi kontrol edebilirsiniz.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 Çiz][8]

### <a name="sub-sampling-the-data-in-sql"></a>SQL verileri alt örnekleme
[Azure Machine Learning Studio](https://studio.azureml.net)model oluşturma için veri hazırlarken, verileri **içeri aktarma modülünde doğrudan kullanmak üzere SQL sorgusuna** karar verebilir veya uygulanan ve örneklenmiş verileri yeni bir tabloda kalıcı hale getirebilmeniz gerekir. bu, **\_yeni\_tablonuz\_ad > < basit bir SELECT *** ile [veri içeri aktarma][import-data] modülünde kullanabilirsiniz.

Bu bölümde, Örneklenmiş ve uygulanan verileri tutacak yeni bir tablo oluşturacağız. Model oluşturma için doğrudan SQL sorgusunun bir örneği, [SQL Server bölümündeki veri araştırma ve özellik Mühendisliği](#dbexplore) bölümünde verilmiştir.

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Bir örnek oluşturun %1 ile birleştirilmiş tablolar doldurma ve tablo. Varsa Tablo ilk bırakın.
Bu bölümde, **nyctaxi\_seyahat** ve **nyctaxi\_tarifeli havayolu**tablosuna katılıyoruz, 1% rastgele bir örnek ayıkladık ve örneklenen verileri yeni bir tablo adı **nyctaxi\_bir\_yüzde**olarak kalıcı hale getiriyoruz:

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

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Veri keşfi Ipython Notebook SQL sorgularını kullanma
Bu bölümde, yukarıda oluşturduğumuz yeni tabloda kalıcı olan 1% örneklenmiş verileri kullanarak veri dağıtımlarını araştırıyoruz. Benzer araştırmalar, özgün tablolar kullanılarak, isteğe bağlı olarak, araştırma örneğini sınırlamak için veya, SQL Server bölümündeki veri araştırması [ve özellik Mühendisliği](#dbexplore) bölümünde gösterildiği gibi, **çekme\_tarih saat** **bölümleri kullanılarak sonuçları** belirli bir zaman dilimine sınırlayarak gerçekleştirilebilir.

#### <a name="exploration-daily-distribution-of-trips"></a>İnceleme: Günlük gelişlerin dağılımı
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>İnceleme: Seyahat dağıtım medallion başına
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>SQL sorgularını Ipython Not Defteri kullanarak özellik oluşturma
Bu bölümde yeni etiketler oluşturacağız ve özellikler, doğrudan SQL sorguları kullanarak %1 örnek tablo üzerinde çalışan önceki bölümde oluşturduğumuz.

#### <a name="label-generation-generate-class-labels"></a>Etiket oluşturma: Sınıf etiketleri oluştur
Aşağıdaki örnekte, biz iki model için kullanılacak etiketleri oluşturun:

1. İkili sınıf etiketleri **eğimli** (bir ipucu verildiyse tahmin edilir)
2. Birden çok Lass etiketleri **ipucu\_sınıfı** (İpucu kutusu veya aralığını tahmin etme)
   
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

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Özellik Mühendisliği: Kategorik sütunlar Count özellikleri
Bu örnek, bir kategorik alan veri alt yineleme sayısı ile her kategori değiştirerek bir sayısal alana dönüştürür.

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

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Özellik Mühendisliği: Sayısal bir sütun depo özellikleri
Bu örnek, sürekli bir sayısal alanı önceden ayarlanmış kategori aralıklarına dönüştürür, diğer bir deyişle sayısal alanı bir kategorik alana dönüştürür.

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

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Özellik Mühendisliği: Ondalık enlem/boylam konumu özellikleri ayıklayın.
Bu örnek, bir enlem ve/veya boylam alanının ondalık temsilini, ülke/bölge, şehir, şehir, blok vb. gibi farklı ayrıntı düzeyi alanlarının birden çok bölge alanına ayırır. Yeni coğrafi alanlar gerçek konumlara eşlenmedi. Kodlamayı konumlarını eşleme hakkında bilgi için bkz. [Bing Haritalar Rest Hizmetleri](https://msdn.microsoft.com/library/ff701710.aspx).

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

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Özellikleri tespit tablonun son biçimini doğrulayın
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Artık [Azure Machine Learning](https://studio.azureml.net)' de model oluşturma ve model dağıtımına devam etmeye hazırsınız. Veri herhangi biri daha önce yani tanımlanan tahmin sorunları için hazırdır:

1. İkili sınıflandırma: tahmin etmek için olup olmadığını bir ipucu Ücretli bir seyahat için.
2. Sınıflı sınıflandırma: aralığı, önceden tanımlanmış sınıfları göre Ücretli ipucu tahmin etmek için.
3. Regresyon görev: bir seyahat için ücretli ipucu miktarını tahmin edin.  

## <a name="mlmodel"></a>Azure Machine Learning modeller oluşturma
Modelleme alıştırma başlamak için Azure Machine Learning çalışma alanına oturum açın. Henüz bir Machine Learning çalışma alanı oluşturmadıysanız, bkz. [Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md).

1. Azure Machine Learning kullanmaya başlamak için bkz. [Azure Machine Learning Studio nedir?](../studio/what-is-ml-studio.md)
2. [Azure Machine Learning Studio](https://studio.azureml.net)oturum açın.
3. Studio giriş sayfası, modül başvurusu ve diğer kaynaklar için çok sayıda bilgi, videolar, öğreticiler, bağlantılar sağlar. Azure Machine Learning hakkında daha fazla bilgi için [Azure Machine Learning belge merkezine](https://azure.microsoft.com/documentation/services/machine-learning/)başvurun.

Tipik eğitim denemesini aşağıdaki adımlardan oluşur:

1. **+ Yeni** bir deneme oluşturun.
2. Azure Machine Learning için verileri elde edersiniz.
3. Verileri gereken şekilde ön işleme, dönüştürme ve değiştirme.
4. Özellikler, gerektiği gibi oluşturun.
5. Verileri eğitim/doğrulama/test kümelerine bölme (veya ayrı veri kümelerinde her biri için).
6. Bir veya daha fazla makine öğrenimi algoritmaları öğrenme problemi çözmek için bağlı olarak seçin. Örneğin, ikili sınıflandırma, birden çok Lass sınıflandırması, regresyon.
7. Eğitim veri kümesi kullanarak bir veya daha fazla modeller eğitin.
8. Eğitilen model kullanarak doğrulama dataset puan.
9. İlgili ölçümleri öğrenme problemi için işlem modellere değerlendirin.
10. Model (ler) i ayarlayın ve dağıtılacak en iyi modeli seçin.

Bu alıştırmada biz zaten incelediniz ve SQL Server'daki verileri mühendislik ve örnek boyutuna, Azure Machine Learning'de alma karar verdi. Bir veya daha fazla tahmin modeli oluşturmak için şu kararı veriyoruz:

1. Veri **giriş ve çıkış** bölümünde bulunan [veri alma][import-data] modülünü kullanarak Azure Machine Learning verileri alın. Daha fazla bilgi için bkz. [veri modülü başvurusunu Içeri aktarma][import-data] sayfası.
   
    ![Azure Machine Learning verileri içeri aktar][17]
2. **Özellikler** panelinde **veri kaynağı** olarak **Azure SQL veritabanı** ' nı seçin.
3. Veritabanı **sunucusu adı** ALANıNA veritabanı DNS adını girin. Biçim: `tcp:<your_virtual_machine_DNS_name>,1433`
4. Karşılık gelen alana **veritabanı adını** girin.
5. **Sunucu Kullanıcı hesabı adı**' na **SQL Kullanıcı adı** ' nı ve **sunucu Kullanıcı hesabı parolasıyla** **parolayı** girin.
7. **Veritabanı sorgusu** düzenleme metin alanında, gerekli veritabanı alanlarını (Etiketler gibi hesaplanan alanlar da dahil olmak üzere) çıkaran sorguyu yapıştırın ve verileri istenen örnek boyutuna doğru örnekleyin.

Aşağıdaki çizimde SQL Server veritabanından doğrudan verileri okuma bir ikili sınıflandırma deneme örneğidir. Benzer denemeleri, çok sınıflı sınıflandırma ve regresyon sorunları için oluşturulabilir.

![Azure Machine Learning eğitimi][10]

> [!IMPORTANT]
> Önceki bölümlerde sağlanan modelleme veri ayıklama ve örnekleme sorgusu örneklerinde, **üç modellemeye yönelik tüm Etiketler sorguya dahil**edilmiştir. Modelleme alýþtýrmalarının her birinde önemli (gerekli) bir adım, diğer iki soruna ve diğer **hedef sızıntılara**yönelik gereksiz etiketleri **dışlayamazsınız** . Örneğin, ikili sınıflandırma kullanırken, **eğimli** etiketini kullanın ve alanları **İpucu\_sınıfı**, **tıp\_tutarı**ve **Toplam\_tutarı**hariç tutun. Bunlar tip kapsıyor ikinci hedef sızıntılarını, çünkü Ücretli.
> 
> Gereksiz sütunları ve/veya hedef sızıntılarını dışlamak için, [veri kümesi modülünde sütunları seç][select-columns] veya [Veri Düzenle][edit-metadata]' yi kullanabilirsiniz. Daha fazla bilgi için bkz. [veri kümesindeki sütunları seçme][select-columns] ve [meta veri][edit-metadata] başvuru sayfalarını düzenleme.
> 
> 

## <a name="mldeploy"></a>Azure Machine Learning modelleri dağıtma
Modeliniz hazır olduğunda, kolayca, denemeyi doğrudan bir web hizmeti olarak dağıtabilirsiniz. Azure Machine Learning Web hizmetlerini dağıtma hakkında daha fazla bilgi için bkz. [Azure Machine Learning Web hizmeti dağıtma](../studio/deploy-a-machine-learning-web-service.md).

Yeni bir web hizmeti dağıtmak için yapmanız:

1. Puanlama deneme oluşturun.
2. Web hizmeti dağıtın.

**Tamamlanmış** bir eğitim deneyminden bir Puanlama denemesi oluşturmak için, alt eylem çubuğunda **Puanlama denemesi oluştur** ' a tıklayın.

![Azure Puanlama][18]

Azure Machine Learning eğitim denemesini bileşenlerine göre bir Puanlama deneme oluşturmaya çalışır. Özellikle de yapar:

1. Eğitilen modeli kaydedin ve model eğitim modülleri kaldırın.
2. Beklenen giriş verisi şemasını temsil eden bir mantıksal **giriş bağlantı noktası** belirler.
3. Beklenen Web hizmeti çıkış şemasını temsil eden bir mantıksal **çıkış bağlantı noktası** belirler.

Puanlama denemeyi oluşturulduğunda gözden geçirin ve gerektiği gibi ayarlayın. Tipik bir ayar, giriş veri kümesini ve/veya sorguyu etiket alanlarını dışladığı bir şekilde değiştirmek, çünkü bu Etiketler hizmet çağrıldığında şemada kullanılabilir olmayacaktır. Ayrıca, giriş veri kümesinin boyutunu ve/veya birkaç kayıt için sorgu, giriş şemasını gösterecek kadar azaltmak iyi bir uygulamadır. Çıkış bağlantı noktası için, tüm giriş alanlarını hariç tutmak ve yalnızca [veri kümesindeki sütunları seç][select-columns] modülünde yer alan Select sütunları kullanılarak **çıktıda puanlanmış** olan **olasılıkların** dahil olması yaygındır.

Deneme Puanlama bir örnek, aşağıdaki çizimde gösterilmektedir. Dağıtıma hazırsanız, alt eylem çubuğunda **Web hizmeti Yayımla** düğmesine tıklayın.

![Azure Machine Learning yayımlama][11]

Bu izlenecek yol öğreticide bilgilerin üzerinden geçelim için model eğitimi ve bir Azure Machine Learning web hizmeti dağıtımı için tüm veri alma büyük genel kümesinden ile birlikte çalışarak bir Azure veri bilimi ortamını oluşturdunuz.

### <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve kendi eşlik eden betikleri ve Ipython notebook(s) paylaşılır Microsoft tarafından MIT lisansı altında. Daha fazla ayrıntı için GitHub 'daki örnek kodun dizinindeki LICENSE. txt dosyasını denetleyin.

### <a name="references"></a>Başvurular
• [Andr, Monroy NYC Taxi seyahatler Indirme sayfası](https://www.andresmh.com/nyctaxitrips/)  
• [Cwhong  NYC 'Nin TAXI veri yolculuğu](https://chriswhong.com/open-data/foil_nyc_taxi/)  
• [NYC Taxi ve Limousine Komisyonu araştırması ve istatistikleri](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

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
