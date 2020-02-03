---
title: Azure SYNAPSE Analytics-Team Data Science Işlemini kullanarak model oluşturma ve dağıtma
description: Genel kullanıma açık bir veri kümesiyle Azure SYNAPSE Analytics 'i kullanarak bir makine öğrenimi modeli oluşturun ve dağıtın.
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
ms.openlocfilehash: e64b951a8bb96b25a6ef917b4cebe077d6dd6657
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76718455"
---
# <a name="the-team-data-science-process-in-action-using-azure-synapse-analytics"></a>Team Data Science süreci: Azure SYNAPSE Analytics 'i kullanma
Bu öğreticide, genel kullanıma açık bir veri kümesi ( [NYC TAXI gezileri](https://www.andresmh.com/nyctaxitrips/) veri kümesi) Için Azure SYNAPSE Analytics 'i kullanarak bir makine öğrenimi modeli oluşturma ve dağıtma konusunda size kılavuzluk ederiz. Oluşturulan ikili sınıflandırma modeli, seyahat için bir tıp ödenip ödenmediğini tahmin eder.  Modeller birden çok Lass sınıflandırması (bir ipucu olup olmadığına bakılmaksızın) ve gerileme (ücretli ücret miktarları için dağıtım) içerir.

Yordam, [Team Data Science Process (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) iş akışını izler. Veri bilimi ortamının nasıl ayarlanacağını, verilerin Azure SYNAPSE Analytics 'e nasıl yükleneceğini ve modele veri ve mühendislik özelliklerini araştırmak için Azure SYNAPSE Analytics ya da bir IPython Not defteri nasıl kullanılacağını göstereceğiz. Ardından nasıl oluşturacağınızı ve Azure Machine Learning ile model dağıtma göstereceğiz.

## <a name="dataset"></a>NYC TAXI gidiş veri kümesi
Yaklaşık 20 GB sıkıştırılmış CSV dosyalar (sıkıştırmadan ~ 48 GB), NYC taksi seyahat verilerini oluşuyorsa, 173 milyondan fazla bireysel gelişlerin ve fares kaydetmek için her bir seyahat Ücretli. Her bir seyahat kayıt, toplama ve dropoff konumları ve süreleri, anonim hack (sürücü) lisans numarası ve medallion (taksi'nın benzersiz Tanımlayıcı) numarası içerir. Veriler tüm dönüş 2013 yılında kapsar ve aşağıdaki iki veri kümesi için her ay sağlanır:

1. **Trip_data. csv** dosyası, pascucılar, toplama ve açılan noktaların sayısı, seyahat süresi ve seyahat uzunluğu gibi seyahat ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. **Trip_fare. csv** dosyası, ödeme türü, tarifeli havayolu miktarı, ek ücret, vergiler, ipuçları ve Tolls ve ödenen toplam miktar gibi her seyahat için ödenen tarifeli havayolu ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat\_veri ve seyahat\_tarifeli havayolu birleştirmek için kullanılan **benzersiz anahtar** aşağıdaki üç alandan oluşur:

* medallion,
* Hack\_lisansı ve
* \_Tarih/çekme.

## <a name="mltasks"></a>Üç tahmin görevi türünü ele edin
Üç tür modelleme görevini göstermek için *ipucu\_miktarına* göre üç tahmin sorunu şekilliyoruz:

1. **İkili sınıflandırma**: bir ipucunun seyahat için ödenip ödenmediğini tahmin etmek için, $0 'den büyük bir ipucu *\_* pozitif bir örnektir ve *İpucu\_$0 miktarı* negatif bir örnektir.
2. **Birden çok Lass sınıflandırması**: seyahat için ödenen ipucu aralığını tahmin etmek için. *İpucu\_miktarını* beş veya daha fazla sınıfa böyoruz:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Regresyon görevi**: seyahat için ödenen ipucu miktarını tahmin etmek için.

## <a name="setup"></a>Gelişmiş analiz için Azure veri bilimi ortamını ayarlama
Azure veri bilimi ortamı oluşturmanız için bu adımları izleyin.

**Kendi Azure Blob depolama hesabınızı oluşturun**

* Kendi Azure Blob depolama alanınızı sağladığınızda, Azure Blob depolama alanınızı bir coğrafi konum seçin veya **Orta Güney ABD**, bu, NYC TAXI verilerinin saklandığı yerdir. Kendi depolama hesabında bir kapsayıcı için ortak bir blob depolama kapsayıcısından AzCopy kullanarak veri kopyalanır. Daha yakından Azure blob depolama alanınızın Güney Orta ABD için bu görevi (adım 4) daha hızlı tamamlanır.
* Kendi Azure depolama hesabınızı oluşturmak için [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md)bölümünde açıklanan adımları izleyin. Bu kılavuzda daha sonra gerekli şekilde aşağıdaki depolama hesabı kimlik değerleri hakkında notlar alın emin olun.

  * **Depolama hesabı adı**
  * **Depolama hesabı anahtarı**
  * **Kapsayıcı adı** (verilerin Azure Blob depolamada depolanmasını istediğiniz)

**Azure SYNAPSE Analytics örneğinizi sağlayın.**
Azure SYNAPSE Analytics örneği sağlamak için [Azure Portal Azure SQL veri ambarı oluşturma ve sorgulama bölümündeki](../../sql-data-warehouse/create-data-warehouse-portal.md) belgeleri izleyin. Sonraki adımlarda kullanılacak aşağıdaki Azure SYNAPSE Analytics kimlik bilgileri üzerinde gösterimler olduğunuzdan emin olun.

* **Sunucu adı**: \<sunucu adı >. Database. Windows. net
* **SQLDW (veritabanı) adı**
* **Kullanıcı Adı**
* **Parola**

**Visual Studio 'Yu ve SQL Server Veri Araçları 'yi yükleyip.** Yönergeler için bkz. [SQL Data Warehouse Için Visual Studio 2019 ile çalışmaya](../../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md)başlama.

**Visual Studio ile Azure SYNAPSE Analytics 'e bağlanın.** Yönergeler için bkz. Adım 1 & 2 [Azure SQL veri ambarı 'Na bağlanma](../../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

> [!NOTE]
> **Bir ana anahtar oluşturmak**Için, Azure SYNAPSE Analytics 'te oluşturduğunuz veritabanında (bağlantı konusunun 3. adımında belirtilen sorgu yerine) aşağıdaki SQL sorgusunu çalıştırın.
>
>

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Azure aboneliğiniz altında bir Azure Machine Learning çalışma alanı oluşturun.** Yönergeler için bkz. [Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md).

## <a name="getdata"></a>Verileri Azure SYNAPSE Analytics 'e yükleme
Bir Windows PowerShell komutu konsolunu açın. GitHub 'da sizinle paylaşdığımız örnek SQL komut dosyalarını, *-DestDir*parametresiyle belirttiğiniz yerel bir dizine indirmek Için aşağıdaki PowerShell komutlarını çalıştırın. *-DestDir* parametresinin değerini herhangi bir yerel dizin olarak değiştirebilirsiniz. *-DestDir* yoksa, PowerShell betiği tarafından oluşturulur.

> [!NOTE]
> *DestDir* dizininizin bu dosyayı oluşturmak veya yazmak için yönetici ayrıcalıklarına ihtiyacı varsa, aşağıdaki PowerShell betiğini yürütürken **yönetici olarak çalıştırmanız** gerekebilir.
>
>

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Başarılı yürütmeden sonra, geçerli çalışma dizininiz *-DestDir*olarak değişir. Ekran görebilmek için aşağıdaki gibi:

![Geçerli çalışma dizini değişiklikleri][19]

*-DestDir*' de, aşağıdaki PowerShell betiğini yönetici modunda yürütün:

    ./SQLDW_Data_Import.ps1

PowerShell betiği ilk kez çalıştırıldığında, Azure SYNAPSE Analizinizden ve Azure Blob Storage hesabınızdan bilgileri girmek isteyip istemediğiniz sorulur. Bu PowerShell Betiği tamamlandıktan sonra ilk kez kimlik bilgilerini çalıştıran, giriş SQLDW.conf mevcut çalışma dizininde bir yapılandırma dosyasına yazılmış. Bu PowerShell Betiği gelecekteki yürütülmesi tüm gerekli parametreleri bu yapılandırma dosyasından okumak için seçeneği vardır. Bazı parametreleri değiştirmeniz gerekiyorsa, bu yapılandırma dosyasını silerek ve parametreler değerlerini istenen şekilde kaydederek veya parametre değerlerini *-DestDir* dizininizde sqldw. conf dosyasını düzenleyerek değiştirmek üzere ekranda parametreleri yeniden seçebilirsiniz.

> [!NOTE]
> Şema adının Azure Azure SYNAPSE Analytics 'te zaten var olan olanlarla çakışmasını önlemek için, parametreleri doğrudan SQLDW. conf dosyasından okurken, varsayılan şema olarak SQLDW. conf dosyasındaki şema adına 3 basamaklı bir rastgele sayı eklenir Her çalıştırmanın adı. PowerShell Betiği, bir şema adı isteyebilir: kullanıcı kararımıza adı belirtilebilir.
>
>

Bu **PowerShell betik** dosyası aşağıdaki görevleri tamamlar:

* AzCopy zaten yüklü değilse **AzCopy indirir ve yükler**

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
               Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }
* **Özel BLOB depolama hesabınıza** , AzCopy ile genel Blobun verileri kopyalar

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* , **PolyBase kullanarak verileri yükler (LoadDataToSQLDW. SQL dosyasını çalıştırarak)** , özel BLOB depolama hesabınızdan aşağıdaki komutlarla Azure SYNAPSE analizlerinizi kullanın.

  * Bir şema oluşturun

          EXEC (''CREATE SCHEMA {schemaname};'');
  * Veritabanı kapsamlı kimlik bilgileri oluşturma

          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * Azure Depolama Blobu için dış veri kaynağı oluşturma

          CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;

          CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
  * Csv dosyaları için bir dış dosya biçimine oluşturun. Veri sıkıştırılmamış ve alanlar dikey çizgi karakteriyle ayrılır.

          CREATE EXTERNAL FILE FORMAT {csv_file_format}
          WITH
          (
              FORMAT_TYPE = DELIMITEDTEXT,
              FORMAT_OPTIONS
              (
                  FIELD_TERMINATOR ='','',
                  USE_TYPE_DEFAULT = TRUE
              )
          )
          ;
  * Dış taksi ve NYC taksi veri kümesi için seyahat tablolar Azure blob depolama alanında oluşturun.

          CREATE EXTERNAL TABLE {external_nyctaxi_fare}
          (
              medallion varchar(50) not null,
              hack_license varchar(50) not null,
              vendor_id char(3),
              pickup_datetime datetime not null,
              payment_type char(3),
              fare_amount float,
              surcharge float,
              mta_tax float,
              tip_amount float,
              tolls_amount float,
              total_amount float
          )
          with (
              LOCATION    = ''/nyctaxifare/'',
              DATA_SOURCE = {nyctaxi_fare_storage},
              FILE_FORMAT = {csv_file_format},
              REJECT_TYPE = VALUE,
              REJECT_VALUE = 12
          )

            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                   medallion varchar(50) not null,
                   hack_license varchar(50)  not null,
                   vendor_id char(3),
                   rate_code char(3),
                   store_and_fwd_flag char(3),
                   pickup_datetime datetime  not null,
                   dropoff_datetime datetime,
                   passenger_count int,
                   trip_time_in_secs bigint,
                   trip_distance float,
                   pickup_longitude varchar(30),
                   pickup_latitude varchar(30),
                   dropoff_longitude varchar(30),
                   dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12
            )

    - Azure Blob depolama 'daki dış tablolardaki verileri Azure SYNAPSE Analytics 'e yükleme

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Bir örnek veri tablosu (NYCTaxi_Sample) oluşturma ve verileri için seyahat ve taksi tablolarını SQL sorguları seçeneğini ekleyin. (Bu izlenecek yolun bazı adımlarının bu örnek tabloyu kullanması gerekir.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Depolama hesaplarınızı coğrafi konumunu, yükleme süreleri etkiler.

> [!NOTE]
> Özel BLOB depolama hesabınızın coğrafi konumuna bağlı olarak, genel bir Blobun verileri özel depolama hesabınıza kopyalama işlemi yaklaşık 15 dakika, hatta daha uzun sürebilir ve depolama hesabınızdan Azure 'a veri yükleme işlemi gerçekleştirebilir Azure SYNAPSE Analytics 20 dakika veya daha uzun sürebilir.
>
>

Yinelenen kaynak ve hedef dosya varsa, hangi karar vermeniz gerekir.

> [!NOTE]
> Kopyalanacak .csv dosyalarını özel blob depolama hesabınıza ortak blob depolama, özel bir blob depolama hesabınızda zaten var, AzCopy üzerine yazmak isteyip istemediğinizi sorar. Üzerine yazmak istemiyorsanız, istendiğinde **n** girin. Bunların **tümünün** üzerine yazmak isterseniz, istendiğinde **bir** girin. Ayrıca,. csv dosyalarının üzerine yazmak için **y** girişi de yapabilirsiniz.
>
>

![AzCopy çıktısı][21]

Kendi verilerinizi kullanabilirsiniz. Verilerinizi şirket içi makinenizde, gerçek hayatta uygulamanızda varsa, yine de şirket içi verileri, özel bir Azure blob depolama alanına yüklemek için AzCopy kullanabilirsiniz. Yalnızca, PowerShell betik dosyasının AzCopy komutunda `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`**kaynak** konumunu, verilerinizi içeren yerel dizine değiştirmeniz gerekir.

> [!TIP]
> Verileriniz gerçek yaşam uygulamanızda zaten özel Azure Blob depolarlarda varsa, PowerShell betiğine AzCopy adımını atlayabilir ve verileri doğrudan Azure Azure SYNAPSE Analytics 'e yükleyebilirsiniz. Bu ek düzenlemeler verilerinizi biçimine uygun hale getirmek için komut dosyası gerektirir.
>
>

Bu PowerShell betiği Ayrıca, bu üç dosyanın denenmeye hazır olması için SQLDW_Explorations. SQL, SQLDW_Explorations. ipynb ve SQLDW_Explorations_Scripts. Kopyala gibi veri araştırma örnek dosyalarına Azure SYNAPSE Analytics bilgilerini de takar PowerShell betiği tamamlandıktan hemen sonra.

Başarılı yürütme sonrasında bir ekran görürsünüz. aşağıdaki gibi:

![Başarılı betik yürütme çıktısı][20]

## <a name="dbexplore"></a>Azure SYNAPSE Analytics 'te veri araştırması ve özellik Mühendisliği
Bu bölümde, **Visual Studio veri araçları**kullanarak doğrudan Azure SYNAPSE Analytics 'e karşı SQL sorguları çalıştırarak veri keşif ve özellik oluşturma gerçekleştiririz. Bu bölümde kullanılan tüm SQL sorguları *SQLDW_Explorations. SQL*adlı örnek betikte bulunabilir. Bu dosya yerel dizininize PowerShell betiği tarafından zaten yüklendi. Ayrıca, [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql)'dan da alabilirsiniz. Ancak GitHub 'daki dosyada Azure SYNAPSE Analytics bilgileri takılı değildir.

Azure SYNAPSE Analytics oturum açma adı ve parolasıyla Visual Studio 'Yu kullanarak Azure SYNAPSE Analytics 'e bağlanın ve veritabanını ve tabloları içeri aktardığından emin olmak için **SQL nesne Gezgini** açın. *SQLDW_Explorations. SQL* dosyasını alın.

> [!NOTE]
> Bir paralel veri ambarı (PDW) sorgu Düzenleyicisi açmak için, **SQL nesne Gezgini**'da PDW seçildiğinde **Yeni sorgu** komutunu kullanın. Standart SQL sorgu Düzenleyicisi'ni PDW tarafından desteklenmiyor.
>
>

Bu bölümde gerçekleştirilen veri keşif ve özellik oluşturma görevlerinin türleri şunlardır:

* Değişen zaman pencereleri bazı alanların veri dağıtımları keşfedin.
* Veri Kalitesi boylam ve enlem alanlarının araştırın.
* **İpucu\_miktarına**göre ikili ve çoklu sınıf sınıflandırma etiketleri oluşturun.
* Özellikleri oluşturma ve işlem/seyahat uzaklıkları karşılaştırın.
* İki tabloyu birleştirmek ve modelleri oluşturmak için kullanılan rastgele bir örnek ayıklayın.

### <a name="data-import-verification"></a>Veri içeri aktarma doğrulama
Bu sorguların hızlı bir satır sayısı doğrulaması sağlamak ve daha önce Polybase'nın Paralel toplu kullanarak doldurulmuş tablolardaki sütunlara içeri aktarma

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Çıkış:** 173.179.759 satır ve 14 sütun almalısınız.

### <a name="exploration-trip-distribution-by-medallion"></a>İnceleme: Seyahat dağıtım medallion tarafından
Bu örnekte sorgu, belirtilen süre içinde 100'den fazla gelişlerin tamamlandı medallions (taksi numaraları) tanımlar. Sorgu, **\_Tarih/saatteki**bölüm şeması tarafından koşullu olduğundan, bölümlenmiş tablo erişiminizden faydalanır. Tam bir veri kümesinin sorgulanmasını ayrıca bölümlenmiş tablosunu kullanmak ve/veya dizin tarama yapar.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Çıkış:** Sorgu, 13.369 medalons (Tax,) ve 2013 ' de tamamlanan gidiş sayısını belirten satırları içeren bir tablo döndürmelidir. Son sütun tamamlandı gelişlerin sayısını içerir.

### <a name="exploration-trip-distribution-by-medallion-and-hack_license"></a>İnceleme: Seyahat dağıtım medallion ve hack_license
Bu örnekte medallions (taksi numaraları) tanımlar ve hack_license (sürücüler) tamamlanan birden fazla 100 gelişlerin belirtilen bir süre içinde numaralandırır.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Çıkış:** Sorgu, 100 2013 ' te daha fazla tamamlanan 13.369 otomobil/sürücü kimliklerini belirten 13.369 satırlık bir tablo döndürmelidir. Son sütun tamamlandı gelişlerin sayısını içerir.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Veri Kalitesi değerlendirme: yanlış boylam ve/veya enlem kayıtlarla doğrulayın
Herhangi bir boylam ve/veya enlem alanı ya da geçersiz bir değer içeriyorsa, bu örnekte araştırır (radian derece -90 ile 90 arasında olmalıdır), veya (0, 0) koordinatları.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Çıkış:** Sorgu, geçersiz boylam ve/veya enlem alanları olan 837.467 gezileri döndürüyor.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>İnceleme: değil Eğimli gelişlerin dağıtım Eğimli
Bu örnek, belirli bir süre içinde (veya buraya ayarlanır gibi tam yıl kapsayan, tam veri kümesi) Eğimli değil, sayı Eğimli dönüş sayısı bulur. Bu dağıtım için ikili sınıflandırma modelleme daha sonra kullanılmak üzere ikili etiket dağılımı yansıtır.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Çıkış:** Sorgu, 2013:90.447.622 eğimli ve 82.264.709-eğik olmayan yıl için aşağıdaki uç sıklığı döndürmelidir.

### <a name="exploration-tip-classrange-distribution"></a>İnceleme: İpucu sınıfı/aralığı dağıtım
Bu örnek, belirli bir süre içinde (veya kapsayan tam yıl, tam veri kümesi) dağıtım ipucu aralıklarının hesaplar. Bu etiket sınıfları dağıtımı, daha sonra birden çok Lass sınıflandırma modelleme için kullanılacaktır.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Çıktıların**

| tip_class | tip_freq |
| --- | --- |
| 1 |82230915 |
| 2 |6198803 |
| 3 |1932223 |
| 0 |82264625 |
| 4 |85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>İnceleme: İşlem ve seyahat uzaklık karşılaştırın
Bu örnek dönüştürür alma ve dropoff boylam ve enlem SQL coğrafi konum için işaret, SQL Coğrafya noktaları fark kullanarak seyahat uzaklığı hesaplar ve sonuçları karşılaştırma için rastgele oluşturulmuş bir örnek döndürür. Bu örnek yalnızca daha önce veri kalitesi değerlendirme sorgusu kullanarak geçerli koordinat sonuçları sınırlar.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert to radians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert to miles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>SQL işlevleri kullanarak özellik Mühendisliği
Bazı durumlarda SQL işlevleri, özellik Mühendisliği için verimli bir seçenek olabilir. Bu kılavuzda, toplama ve dropoff konumlar arasında doğrudan uzaklığı hesaplamak için bir SQL işlev tanımladık. **Visual Studio veri araçları**'NDA aşağıdaki SQL betiklerini çalıştırabilirsiniz.

Distance işlevi tanımlayan SQL komut dosyası aşağıda verilmiştir.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert to radians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert to miles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

Özellikler, SQL sorgunuzun oluşturmak için bu işlevi çağırmak için bir örnek aşağıda verilmiştir:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Çıkış:** Bu sorgu, potitudes ve Longitudes ve mil içindeki karşılık gelen doğrudan uzaklıklarla birlikte toplama ve Dropoff ile bir tablo (2.803.538 satırlık) oluşturur. İlk üç satırın sonuçları aşağıda verilmiştir:

|  | pickup_latitude | pickup_longitude | dropoff_latitude | dropoff_longitude | DirectDistance |
| --- | --- | --- | --- | --- | --- |
| 1 |40.731804 |-74.001083 |40.736622 |-73.988953 |.7169601222 |
| 2 |40.715794 |-74,010635 |40.725338 |-74.00399 |.7448343721 |
| 3 |40.761456 |-73.999886 |40.766544 |-73.988228 |0.7037227967 |

### <a name="prepare-data-for-model-building"></a>Veri hazırlama için model oluşturma
Aşağıdaki sorgu **nyctaxi\_seyahat** ve **nyctaxi\_tarifeli havayolu** tablolarını birleştirir, **eğimli**bir ikili sınıflandırma etiketi, bir çok sınıf sınıflandırma etiketi **İpucu\_sınıfı**oluşturur ve tam Birleşik veri kümesinden bir örnek ayıklar. Örnekleme, toplama zamana dayalı gelişlerin kümesini alarak gerçekleştirilir.  Bu sorgu, Azure 'daki SQL veritabanı örneğinden doğrudan veri alımı için [Azure Machine Learning Studio (klasik)](https://studio.azureml.net) [içeri aktarma verileri][içeri aktarma-veri] modülünde kopyalanabilir. Sorgu yanlış kayıtlarla dışlar (0, 0) koordinatları.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Azure Machine Learning için devam etmeye hazır olduğunuzda, şunlardan birini yapabilirsiniz:

1. Verileri ayıklamak ve örneklemek için son SQL sorgusunu kaydedin ve sorguyu Azure Machine Learning[içeri aktarma-veri] modülüne doğrudan bir Içeri aktarma verileri içine yapıştırın veya
2. Model oluşturma için kullanmayı planladığınız örneklenmiş ve uygulanan verileri yeni bir Azure SYNAPSE Analytics tablosunda kalıcı hale getirin ve yeni tabloyu Azure Machine Learning [içeri aktarma verileri Içeri aktar][içeri aktarma-veri] modülünde kullanın. Önceki adımda bulunan PowerShell betiği sizin için bu görevi tamamladınız. Bu tablodaki verileri içeri aktarma modülü doğrudan okuyabilir.

## <a name="ipnb"></a>IPython not defterinde veri araştırması ve özellik Mühendisliği
Bu bölümde, daha önce oluşturulan Azure SYNAPSE Analytics 'te hem Python hem de SQL sorguları kullanarak veri araştırması ve özellik oluşturma işlemi gerçekleştiririz. **SQLDW_Explorations. ipynb** adlı bir örnek IPython Not defteri ve bir Python betik dosyası **SQLDW_Explorations_Scripts. Kopyala** yerel dizininize indirildi. Bunlar [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW)'da da kullanılabilir. Bu iki dosyayı Python betiklerini aynıdır. Python betik dosyası Ipython not defteri olmaması durumunda size sağlanır. Bu iki örnek Python dosyası, **Python 2,7**altında tasarlanmıştır.

Örnek IPython Not defteri ve yerel makinenize indirilen Python betik dosyasındaki gerekli Azure SYNAPSE Analytics bilgileri, daha önce PowerShell betiği tarafından prize takılmış. Bunlar herhangi bir değişiklik yapılmadan yürütülebilir.

Zaten bir Azure Machine Learning çalışma alanı ayarladıysanız, örnek IPython Not defterini AzureML ıdion Not defteri hizmetine doğrudan yükleyebilir ve çalıştırmaya başlayabilirsiniz. Aşağıda, AzureML ıvthon Not defteri hizmetine yükleme adımları verilmiştir:

1. Azure Machine Learning çalışma alanınızda oturum açın, üstteki **Studio "** ya ve Web sayfasının sol tarafındaki **Not defterleri** ' ne tıklayın.

    ![Studio Not DEFTERLERİNİ tıklatın][22]
2. Web sayfasının sol alt köşesinde **Yeni** ' ye tıklayın ve **Python 2**' yi seçin. Ardından, not defteri için bir ad sağlayın ve yeni boş Ipython not defteri oluşturmak için onay işaretine tıklayın.

    ![YENİ'ye tıklayın, sonra Python 2'yi seçin][23]
3. Yeni IPython Not defterinin sol üst köşesindeki **Jupyter** simgesine tıklayın.

    ![Jupyter simgesine tıklayın][24]
4. Örnek IPython Not defterini sürükleyip, AzureML VMM 'nizin **ağaç** sayfasına bırakın ve **karşıya yükle**' ye tıklayın. Ardından, Ipython Notebook örnek AzureML Ipython Notebook hizmete yüklenir.

    ![Yükle'yi tıklatın][25]

Örneği çalıştırmak için Ipython Notebook veya Python, paketleri gereken aşağıdaki Python betik dosyası. AzureML Ipython Notebook hizmeti kullanıyorsanız, bu paketlerin önceden yüklü olmuştur.

- pandas
- numpy
- matplotlib
- pyodbc
- PyTables

Büyük verilerle Azure Machine Learning gelişmiş analitik çözümler oluştururken önerilen sıra aşağıda verilmiştir:

* Verilerin küçük bir örnek, bir bellek içi veri çerçevesine okuyun.
* Bazı görselleştirmeler ve araştırmaları örneklenen verileri kullanarak gerçekleştirin.
* Örnek verileri kullanarak özellik Mühendisliği ile denemeler yapın.
* Daha büyük veri araştırması, veri işleme ve özellik Mühendisliği için Python kullanarak doğrudan Azure SYNAPSE Analytics 'e karşı SQL sorguları verme.
* Azure Machine Learning modeli oluşturma için uygun olması için örnek boyutunu karar verin.

Aşağıdakilere birkaç veri keşfi, veri Görselleştirme ve örnekler mühendislik özelliği olan. Daha fazla veri araştırmaları, Ipython Notebook örnek ve örnek Python betik dosyası bulunamadı.

### <a name="initialize-database-credentials"></a>Veritabanı kimlik bilgileri Başlat
Veritabanı bağlantı ayarlarınızı aşağıdaki değişkenleri başlatın:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Veritabanı bağlantısı oluşturma
Veritabanı bağlantısı oluşturur. bağlantı dizesi aşağıda verilmiştir.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxi_trip"></a>< Nyctaxi_trip > Tablo içindeki satırları ve sütunları rapor sayısı
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Toplam satır sayısı 173179759 =
* Toplam sütun sayısı = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxi_fare"></a>< Nyctaxi_fare > Tablo içindeki satırları ve sütunları rapor sayısı
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Toplam satır sayısı 173179759 =
* Toplam sütun sayısı 11 =

### <a name="read-in-a-small-data-sample-from-the-azure-synapse-analytics-database"></a>Azure SYNAPSE Analytics veritabanından küçük bir veri örneğine okuma
    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Örnek Tablo 14.096495 saniyedir okuma süresi.
Satır ve sütun sayısı, alınan = (1000 21).

### <a name="descriptive-statistics"></a>Açıklayıcı istatistikleri
Örneklenen verileri araştırmak hazırsınız. **Seyahat\_mesafesi** (veya belirtmek için seçtiğiniz diğer alanlar) için bazı açıklayıcı istatistiklere bakmaya başladık.

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Görselleştirme: Kutusu Diagram örneği
Sonraki Kutu Çizimi quantiles görselleştirmek seyahat uzaklığı için atacağız.

    df1.boxplot(column='trip_distance',return_type='dict')

![Kutusu çizim çıkış][1]

### <a name="visualization-distribution-plot-example"></a>Görselleştirme: Dağıtım Diagram örneği
Dağıtım ve örneklenen seyahat uzaklıkları histogramını görselleştirin çizimleri.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Dağıtım çizim çıkış][2]

### <a name="visualization-bar-and-line-plots"></a>Görselleştirme: Çubuk ve çizgi çizer
Bu örnekte, beş depo seyahat uzaklığı bin ve Gruplama sonuçları görselleştirebilirsiniz.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Yukarıdaki çubuk bin dağıtım çizim veya çizim ile satır:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Çubuk grafik çıkış][3]

ile

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Satırı çizim çıkışı][4]

### <a name="visualization-scatterplot-examples"></a>Görselleştirme: Dağılım grafiği örnekleri
**\_saniye içinde seyahat\_zaman\_** arasında dağılım çizimi gösteririz ve herhangi bir bağıntı olup olmadığını görmek için **seyahat\_mesafe**

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Dağılım Grafiği çıkış saati uzaklığı arasındaki ilişki][6]

Benzer şekilde, **oran\_kod** ve **seyahat\_uzaklığı**arasındaki ilişkiyi kontrol edebilirsiniz.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Dağılım Grafiği çıktısını kod ve uzaklık arasındaki ilişki][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Veri keşfi Ipython notebook SQL sorgularını kullanarak örneklenen verileri
Bu bölümde, yukarıda oluşturduğumuz yeni tabloda kalıcı olan örneklenmiş verileri kullanarak veri dağıtımlarını araştırıyoruz. Benzer araştırmalar, özgün tablolar kullanılarak gerçekleştirilebilir.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>İnceleme: Rapor sayısı örneklenen tablo içindeki satırları ve sütunları
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>İnceleme: Eğimli olmayan dağıtım dönüş
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>İnceleme: İpucu dağıtımıdır
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>İnceleme: ipucu dağıtım sınıfı tarafından Çiz
    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 Çiz][26]

#### <a name="exploration-daily-distribution-of-trips"></a>İnceleme: Günlük gelişlerin dağılımı
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>İnceleme: Seyahat dağıtım medallion başına
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>İnceleme: Seyahat dağıtım medallion ve hack lisans tarafından
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>İnceleme: Seyahat süresi dağılımı
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>İnceleme: Seyahat uzaklık dağıtım
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>İnceleme: Ödeme türü dağılımı
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Özellikleri tespit tablonun son biçimini doğrulayın
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Azure Machine Learning modelleri derleme
Artık [Azure Machine Learning](https://studio.azureml.net)' de model oluşturma ve model dağıtımına devam etmeye hazırsınız. Verileri daha önce yani tanımlanan tahmin sorunları hiçbirinde kullanılmaya hazır:

1. **İkili sınıflandırma**: bir ipucunun seyahat için ödenip ödenmediğini tahmin etmek için.
2. **Birden çok Lass sınıflandırması**: önceden tanımlanmış sınıflara göre ücretli ipucu aralığını tahmin etmek için.
3. **Regresyon görevi**: seyahat için ödenen ipucu miktarını tahmin etmek için.

Modelleme alıştırmaya başlamak için **Azure Machine Learning (klasik)** çalışma alanınızda oturum açın. Henüz bir Machine Learning çalışma alanı oluşturmadıysanız, bkz. [Azure Machine Learning Studio (klasik) çalışma alanı oluşturma](../studio/create-workspace.md).

1. Azure Machine Learning kullanmaya başlamak için bkz. [Azure Machine Learning Studio (klasik) nedir?](../studio/what-is-ml-studio.md)
2. [Azure Machine Learning Studio (klasik)](https://studio.azureml.net)' de oturum açın.
3. Machine Learning Studio (klasik) giriş sayfası, çok çeşitli bilgiler, videolar, Öğreticiler, modüller başvurusu ve diğer kaynaklar için bağlantılar sağlar. Azure Machine Learning hakkında daha fazla bilgi için bkz. [Azure Machine Learning belge merkezi](https://azure.microsoft.com/documentation/services/machine-learning/).

Tipik eğitim denemesini aşağıdaki adımlardan oluşur:

1. **+ Yeni** bir deneme oluşturun.
2. Verileri Azure Machine Learning Studio (klasik) içine alın.
3. Verileri gereken şekilde ön işleme, dönüştürme ve değiştirme.
4. Özellikler, gerektiği gibi oluşturun.
5. Verileri eğitim/doğrulama/test kümelerine bölme (veya ayrı veri kümelerinde her biri için).
6. Bir veya daha fazla makine öğrenimi algoritmaları öğrenme problemi çözmek için bağlı olarak seçin. Örneğin, ikili sınıflandırma, birden çok Lass sınıflandırması, regresyon.
7. Eğitim veri kümesi kullanarak bir veya daha fazla modeller eğitin.
8. Eğitilen model kullanarak doğrulama dataset puan.
9. İlgili ölçümleri öğrenme problemi için işlem modellere değerlendirin.
10. Model (ler) i ayarlayın ve dağıtılacak en iyi modeli seçin.

Bu alıştırmada, Azure SYNAPSE Analytics 'te verileri araştırmış ve sunuyoruz ve Azure Machine Learning Studio (klasik) örnek boyutuna karar vermiş olduğumuz. Bir veya daha fazla öngörü modelleri oluşturmak için bir yordam şöyledir:

1. Veri **girişi ve çıkışı** bölümünde bulunan [veri][içeri aktarma-veri] modülünü kullanarak verileri Azure Machine Learning Studio (klasik) alın. Daha fazla bilgi için içeri aktarma [verileri][içeri aktarma-veri] modülü başvurusu sayfasına bakın.

    ![Verileri Azure ML İçeri Aktar][17]
2. **Özellikler** panelinde **veri kaynağı** olarak **Azure SQL veritabanı** ' nı seçin.
3. Veritabanı **sunucusu adı** ALANıNA veritabanı DNS adını girin. Biçim: `tcp:<your_virtual_machine_DNS_name>,1433`
4. Karşılık gelen alana **veritabanı adını** girin.
5. **Sunucu Kullanıcı hesabı adı**' na *SQL Kullanıcı adı* ' nı ve **sunucu Kullanıcı hesabı parolasıyla** *parolayı* girin.
7. **Veritabanı sorgusu** düzenleme metin alanında, gerekli veritabanı alanlarını (Etiketler gibi hesaplanan alanlar da dahil olmak üzere) çıkaran sorguyu yapıştırın ve verileri istenen örnek boyutuna doğru örnekleyin.

İkili sınıflandırmanın bir örneği, doğrudan Azure SYNAPSE Analytics veritabanından veri okumayı deneyin (tablo adlarını nyctaxi_trip ve nyctaxi_fare şema adı ve kullandığınız tablo adlarına göre değiştirmeyi unutmayın. İzlenecek yol). Benzer denemeleri, çok sınıflı sınıflandırma ve regresyon sorunları için oluşturulabilir.

![Azure ML Train][10]

> [!IMPORTANT]
> Önceki bölümlerde sağlanan modelleme veri ayıklama ve örnekleme sorgusu örneklerinde, **üç modellemeye yönelik tüm Etiketler sorguya dahil**edilmiştir. Modelleme alýþtýrmalarının her birinde önemli (gerekli) bir adım, diğer iki soruna ve diğer **hedef sızıntılara**yönelik gereksiz etiketleri **dışlayamazsınız** . Örneğin, ikili sınıflandırma kullanırken, **eğimli** etiketini kullanın ve alanları **İpucu\_sınıfı**, **tıp\_tutarı**ve **Toplam\_tutarı**hariç tutun. Bunlar tip kapsıyor ikinci hedef sızıntılarını, çünkü Ücretli.
>
> Gereksiz sütunları veya hedef sızıntılarını dışlamak için, [veri kümesi modülünde sütunları seç][select-columns] veya [verileri Düzenle][edit-metadata]' yi kullanabilirsiniz. Daha fazla bilgi için bkz. [veri kümesindeki sütunları seçme][select-columns] ve [meta veri][edit-metadata] başvuru sayfalarını düzenleme.
>
>

## <a name="mldeploy"></a>Azure Machine Learning modelleri dağıtma
Modeliniz hazır olduğunda, kolayca, denemeyi doğrudan bir web hizmeti olarak dağıtabilirsiniz. Azure ML Web Hizmetleri 'ni dağıtma hakkında daha fazla bilgi için bkz. [Azure Machine Learning Web hizmeti dağıtma](../studio/deploy-a-machine-learning-web-service.md).

Yeni bir web hizmeti dağıtmak için yapmanız:

1. Puanlama deneme oluşturun.
2. Web hizmeti dağıtın.

**Tamamlanmış** bir eğitim deneyminden bir Puanlama denemesi oluşturmak için, alt eylem çubuğunda **Puanlama denemesi oluştur** ' a tıklayın.

![Azure Puanlama][18]

Azure Machine Learning eğitim denemesini bileşenlerine göre bir Puanlama deneme oluşturmaya çalışır. Özellikle de yapar:

1. Eğitilen modeli kaydedin ve model eğitim modülleri kaldırın.
2. Beklenen giriş verisi şemasını temsil eden bir mantıksal **giriş bağlantı noktası** belirler.
3. Beklenen Web hizmeti çıkış şemasını temsil eden bir mantıksal **çıkış bağlantı noktası** belirler.

Puanlama denemesi oluşturulduğunda, sonuçları gözden geçirin ve gerektiğinde ayarla ' yı yapın. Tipik bir ayar, giriş veri kümesini veya sorguyu etiket alanlarını dışladığı bir şekilde değiştirmek, çünkü bu etiket alanları hizmet çağrılırken şemaya eşlenmeyecektir. Ayrıca, giriş veri kümesinin boyutunu ve/veya birkaç kayıt için sorgu, giriş şemasını gösterecek kadar azaltmak iyi bir uygulamadır. Çıkış bağlantı noktası için, tüm giriş alanlarını hariç tutmak ve yalnızca [veri kümesindeki sütunları seç][select-columns] modülünde yer alan Select sütunları kullanılarak **çıktıda puanlanmış** olan **olasılıkların** dahil olması yaygındır.

Bir örnek denemeyi Puanlama, aşağıdaki şekilde sağlanır. Dağıtıma hazırsanız, alt eylem çubuğunda **Web hizmeti Yayımla** düğmesine tıklayın.

![Azure ML yayımlama][11]

## <a name="summary"></a>Özet
Ne Bu izlenecek yol öğreticide uyguladığımız özeti için büyük genel kümesiyle çalışan bir Azure veri bilimi ortamını Team Data Science Process model eğitiminin ve ardından için veri alım gelen tüm aracılığıyla alma oluşturdunuz bir Azure Machine Learning web hizmeti dağıtımı.

### <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve kendi eşlik eden betikleri ve Ipython notebook(s) paylaşılır Microsoft tarafından MIT lisansı altında. Daha fazla ayrıntı için GitHub 'daki örnek kodun dizinindeki LICENSE. txt dosyasını denetleyin.

## <a name="references"></a>Başvurular
- [AndrMonroy NYC TAXI gezi Indirme sayfası](https://www.andresmh.com/nyctaxitrips/)
- [NYC 'nin taxt verileri, Chris Whong 'a göre veri seyahat](https://chriswhong.com/open-data/foil_nyc_taxi/)
- [NYC Taxi ve Limousine Komisyonu araştırması ve Istatistikleri](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

[1]: ./media/sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/sqldw-walkthrough/azuremltrain.png
[11]: ./media/sqldw-walkthrough/azuremlpublish.png
[12]: ./media/sqldw-walkthrough/ssmsconnect.png
[13]: ./media/sqldw-walkthrough/executescript.png
[14]: ./media/sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/sqldw-walkthrough/bulkimport.png
[17]: ./media/sqldw-walkthrough/amlreader.png
[18]: ./media/sqldw-walkthrough/amlscoring.png
[19]: ./media/sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/sqldw-walkthrough/ps_load_data.png
[21]: ./media/sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[içeri aktarma-veri]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
