---
title: Azure Synapse Analytics - Team Data Science Process kullanarak bir model oluşturun ve dağıtın
description: Azure Synapse Analytics'i kullanarak, herkese açık bir veri kümesiyle bir makine öğrenimi modeli oluşturun ve dağıtın.
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
ms.openlocfilehash: 9c4c1cfdb927cfd2ee607bfe2a951e06c80f9bfb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418550"
---
# <a name="the-team-data-science-process-in-action-using-azure-synapse-analytics"></a>Ekip Veri Bilimi Süreci iş başında: Azure Synapse Analytics'i kullanma
Bu eğitimde, kamuya açık bir veri seti olan [NYC Taksi Gezileri](https://www.andresmh.com/nyctaxitrips/) veri seti için Azure Synapse Analytics'i kullanarak bir makine öğrenimi modeli oluşturmave dağıtma konusunda size yol alıyoruz. Yapılan ikili sınıflandırma modeli, bir bahşişin bir yolculuk için ödenip ödenmediğini tahmin eder.  Modeller çok sınıflı sınıflandırma (bir ipucu olup olmadığını) ve gerileme (ödenen ihbar tutarları için dağıtım) içerir.

Yordam, [Ekip Veri Bilimi Süreci (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) iş akışını izler. Bir veri bilimi ortamının nasıl ayarlanacağımı, verilerin Azure Synapse Analytics'e nasıl yükleneceğimi ve verileri keşfetmek ve özellikleri modellemek için iPython Not Defteri'ni nasıl kullanacağımızı gösteriyoruz. Daha sonra Azure Machine Learning ile bir modelin nasıl oluşturup dağıtılanın gösteriş yapılacağını gösteriyoruz.

## <a name="the-nyc-taxi-trips-dataset"></a><a name="dataset"></a>NYC Taksi Gezileri veri seti
NYC Taksi Gezisi verileri sıkıştırılmış CSV dosyaları (~ 48 GB sıkıştırılmamış), 173 milyondan fazla bireysel geziler ve her yolculuk için ödenen ücretler kayıt yaklaşık 20 GB oluşur. Her seyahat kaydı, pikap ve bırakma yerleri ve saatleri, anonim hack (sürücü) lisans numarası ve madalyon (taksinin benzersiz kimlik) numarasını içerir. Veriler 2013 yılındaki tüm gezileri kapsar ve her ay için aşağıdaki iki veri kümesinde sağlanır:

1. **trip_data.csv** dosyası, yolcu sayısı, teslim alma ve bırakma noktaları, seyahat süresi ve seyahat uzunluğu gibi seyahat ayrıntılarını içerir. Aşağıda birkaç örnek kayıt veörnekleri verebleri verebleri bulabilirsiniz:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. **trip_fare.csv** dosyası, ödeme türü, ücret tutarı, ek ücret ve vergiler, bahşişler ve geçiş ücretleri ve ödenen toplam tutar gibi her seyahat için ödenen ücretin ayrıntılarını içerir. Aşağıda birkaç örnek kayıt veörnekleri verebleri verebleri bulabilirsiniz:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat **unique key** \_verilerine ve seyahat\_ücretine katılmak için kullanılan benzersiz anahtar aşağıdaki üç alandan oluşur:

* Madalyon
* hack\_lisans ve
* alma\_tarihi.

## <a name="address-three-types-of-prediction-tasks"></a><a name="mltasks"></a>Üç tür tahmin görevi adresle
Üç tür modelleme görevini göstermek için *\_uç miktarına* göre üç tahmin sorunu formüle ediyoruz:

1. **İkili sınıflandırma**: Bir yolculuk için bir bahşişin ödenip ödenmediğini, yani 0 TL'den büyük bir *\_bahşiş tutarının* olumlu bir örnek olduğunu, 0 TL'lik *\_bahşiş tutarının* ise olumsuz bir örnek olduğunu tahmin etmek.
2. **Çok sınıflı sınıflandırma**: Yolculuk için ödenen bahşiş aralığını tahmin etmek. *\_İpucu tutarını* beş kutuya veya sınıfa böleriz:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Regresyon görevi**: Bir yolculuk için ödenen bahşiş miktarını tahmin etmek.

## <a name="set-up-the-azure-data-science-environment-for-advanced-analytics"></a><a name="setup"></a>Gelişmiş analitik için Azure veri bilimi ortamını ayarlama
Azure Veri Bilimi ortamınızı ayarlamak için aşağıdaki adımları izleyin.

**Kendi Azure blob depolama hesabınızı oluşturun**

* Kendi Azure blob depolama alanınızı sağlarken, Azure blob depolama alanınız için NYC Taksi verilerinin depolandığı **South Central US'a**mümkün olduğunca yakın veya yakın bir coğrafi konum seçin. Veriler, ortak blob depolama kabından kendi depolama hesabınızdaki bir kapsayıcıya AzCopy kullanılarak kopyalanır. Azure blob depolama alanınız South Central US'a ne kadar yakınsa, bu görev (Adım 4) o kadar hızlı tamamlanır.
* Kendi Azure Depolama hesabınızı oluşturmak için, [Azure Depolama hakkında hesaplarında](../../storage/common/storage-create-storage-account.md)özetlenen adımları izleyin. Bu izlenecek yolda daha sonra ihtiyaç duyulacak şekilde depolama hesabı kimlik bilgilerini izleyen değerler hakkında notlar yaptığınızdan emin olun.

  * **Depolama Hesap Adı**
  * **Depolama Hesap Anahtarı**
  * **Kapsayıcı Adı** (verilerin Azure blob depolama alanında depolanmasını istediğiniz)

**Azure Synapse Analytics örneğini sağlama.**
Azure Synapse Analytics örneğini sağlamak için [Azure portalında bir Azure SQL Veri Ambarı Oluştur'daki](../../synapse-analytics/sql-data-warehouse/create-data-warehouse-portal.md) belgeleri izleyin ve sorgulayın. Sonraki adımlarda kullanılacak aşağıdaki Azure Synapse Analytics kimlik bilgileriüzerinde notlar yaptığınızdan emin olun.

* **Sunucu**Adı \<: sunucu Adı>.database.windows.net
* **SQLDW (Veritabanı) Adı**
* **Username**
* **Parola**

**Visual Studio ve SQL Server Veri Araçları'nı yükleyin.** Talimatlar için, [SQL Veri Ambarı için Visual Studio 2019 ile başlarken](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-install-visual-studio.md)bakın.

**Visual Studio ile Azure Synapse Analytics'inize bağlanın.** Talimatlar için, [Azure Synapse Analytics'te SQL Analytics'e Bağlan'da](../../synapse-analytics/sql/connect-overview.md)1 & 2 adımlarına bakın.

> [!NOTE]
> **Bir ana anahtar oluşturmak**için Azure Synapse Analytics'inizde oluşturduğunuz veritabanında (bağlantı konusunun 3. adımda sağlanan sorgu yerine) aşağıdaki SQL sorgusunu çalıştırın.
>
>

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Azure aboneliğiniz altında bir Azure Machine Learning çalışma alanı oluşturun.** Yönergeler için [bkz.](../studio/create-workspace.md)

## <a name="load-the-data-into-azure-synapse-analytics"></a><a name="getdata"></a>Verileri Azure Synapse Analytics'e yükleyin
Windows PowerShell komut konsolunu açın. GitHub'da sizinle paylaştığımız örnek SQL komut dosyalarını parametre *-DestDir*ile belirttiğiniz yerel bir dizine indirmek için aşağıdaki PowerShell komutlarını çalıştırın. Parametre *-DestDir* değerini herhangi bir yerel dizine değiştirebilirsiniz. *-DestDir* yoksa, PowerShell komut dosyası tarafından oluşturulur.

> [!NOTE]
> *DestDir* dizininizin oluşturmak veya yazmak için Yönetici ayrıcalığına ihtiyacı varsa, aşağıdaki PowerShell komut dosyasını çalıştırırken **Yönetici olarak çalıştırmanız** gerekebilir.
>
>

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Başarılı bir yürütmeden sonra, geçerli çalışma dizini *-DestDir*olarak değişir. Aşağıdaki gibi ekranı görebilmelisin:

![Geçerli çalışma dizini değişiklikleri][19]

*-DestDir'inizde,* aşağıdaki PowerShell komut dosyasını yönetici modunda çalıştırın:

    ./SQLDW_Data_Import.ps1

PowerShell komut dosyası ilk kez çalıştığında, Azure Synapse Analytics ve Azure blob depolama hesabınızdan bilgileri girişiniz istenir. Bu PowerShell komut dosyası ilk kez çalışmayı tamamladığında, girilen kimlik bilgileri sqldw.conf yapılandırma dosyasına bugünkü çalışma dizininde yazılmış olacaktır. Bu PowerShell komut dosyası dosyasının gelecekteki çalışması, bu yapılandırma dosyasından gerekli tüm parametreleri okuma seçeneğine sahiptir. Bazı parametreleri değiştirmeniz gerekiyorsa, bu yapılandırma dosyasını silerek ve parametreleri istendiği gibi girdikten veya *-DestDir* dizininizde SQLDW.conf dosyasını düzenleyerek parametre değerlerini değiştirerek ekrana parametre girişi yapmayı seçebilirsiniz.

> [!NOTE]
> Azure Azure Synapse Analytics'inizde zaten var olan şema adlarıyla çakışmasını önlemek için, parametreleri doğrudan SQLDW.conf dosyasından okurken, SQLDW.conf dosyasından şema adına her çalıştırma için varsayılan şema adı olarak 3 basamaklı bir rasgele sayı eklenir. PowerShell komut dosyası bir şema adı için sizi isteyebilir: ad kullanıcı takdirine bağlı olarak belirtilebilir.
>
>

Bu **PowerShell komut dosyası** dosyası aşağıdaki görevleri tamamlar:

* **AzCopy**zaten yüklü değilse, AzCopy indirir ve yükler

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
* AzCopy ile ortak blob dan **özel blob depolama hesabınıza kopyalar veri**

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* Aşağıdaki komutlarla özel blob depolama hesabınızdan **Azure Synapse Analytics'inize Polybase kullanarak (LoadDataToSQLDW.sql çalıştırarak) verileri yükler.**

  * Şema oluşturma

          EXEC (''CREATE SCHEMA {schemaname};'');
  * Veritabanı kapsamı yla ilgili kimlik bilgisi oluşturma

          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * Azure Depolama blob'u için harici bir veri kaynağı oluşturma

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
  * CSV dosyası için harici bir dosya biçimi oluşturun. Veriler sıkıştırılmamış ve alanlar boru karakteri ile ayrılır.

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
  * Azure blob depolama alanında NYC taksi veri seti için harici ücret ve seyahat tabloları oluşturun.

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

    - Azure blob depolamasındaki harici tablolardan verileri Azure Synapse Analytics'e yükleme

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

    - Bir örnek veri tablosu (NYCTaxi_Sample) oluşturun ve gezi ve ücret tablolarında SQL sorguları seçerek veri ekleyin. (Bu gözden geçirmenin bazı adımlarının bu örnek tabloyu kullanması gerekir.)

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

Depolama hesaplarınızın coğrafi konumu yükleme sürelerini etkiler.

> [!NOTE]
> Özel blob depolama hesabınızın coğrafi konumuna bağlı olarak, genel bir yığından özel depolama hesabınıza veri kopyalama işlemi yaklaşık 15 dakika, hatta daha uzun sürebilir ve depolama hesabınızdan Azure Azure Synapse Analytics'inize veri yükleme işlemi 20 dakika veya daha uzun sürebilir.
>
>

Yinelenen kaynak ve hedef dosyalarınız varsa ne yapacağına karar vermeniz gerekir.

> [!NOTE]
> Ortak blob depolama alanından özel blob depolama hesabınıza kopyalanacak .csv dosyaları zaten özel blob depolama hesabınızda varsa, AzCopy bunları üzerine yazmak isteyip istemediğiniz soracaktır. Üzerine yazmak istemiyorsanız, istendiğinde **n** girişi yapın. **Bunların hepsinin** üzerine yazmak istiyorsanız, istendiğinde **bir** giriş yazın. .csv dosyalarını tek tek yazmak için **y'yi** de giriş yapabilirsiniz.
>
>

![AzCopy'den Çıktı][21]

Kendi verilerinizi kullanabilirsiniz. Verileriniz gerçek uygulamanızda şirket içi makinenizdeyse, şirket içi verileri özel Azure blob depolamanıza yüklemek için AzCopy'yi kullanmaya devam edebilirsiniz. PowerShell komut dosyası **Source** dosyasının `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`AzCopy komutundaki Kaynak konumu yalnızca verilerinizi içeren yerel dizine değiştirmeniz gerekir.

> [!TIP]
> Verileriniz gerçek uygulamanızdaki özel Azure blob depolama alanınızda zaten varsa, PowerShell komut dosyasındaki AzCopy adımını atlayabilir ve verileri doğrudan Azure Synapse Analytics'e yükleyebilirsiniz. Bu, komut dosyasının verilerinizin biçimine göre uyarlamak için ek denetimler gerektirir.
>
>

Bu PowerShell komut dosyası ayrıca Azure Synapse Analytics bilgilerini veri arama örnek dosyaları SQLDW_Explorations.sql, SQLDW_Explorations.ipynb ve SQLDW_Explorations_Scripts.py'ye bağlar, böylece bu üç dosya PowerShell komut dosyası tamamlandıktan sonra anında denenmeye hazır olur.

Başarılı bir yürütmeden sonra, aşağıdaki gibi ekran göreceksiniz:

![Başarılı bir komut dosyası yürütme çıktısı][20]

## <a name="data-exploration-and-feature-engineering-in-azure-synapse-analytics"></a><a name="dbexplore"></a>Azure Synapse Analytics'te veri arama ve özellik mühendisliği
Bu bölümde, Doğrudan **Visual Studio Veri Araçları'nı**kullanarak Azure Synapse Analytics'e karşı SQL sorguları çalıştırarak veri arama ve özellik oluşturma gerçekleştiriyoruz. Bu bölümde kullanılan tüm SQL sorguları *SQLDW_Explorations.sql*adlı örnek komut dosyasında bulunabilir. Bu dosya powershell komut dosyası tarafından yerel dizininize zaten indirilmiştir. Ayrıca [GitHub'dan](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql)da alabilirsiniz. Ancak GitHub'daki dosyada Azure Synapse Analytics bilgileri takılı değildir.

Azure Synapse Analytics oturum adı ve parolasıyla Visual Studio'yu kullanarak Azure Synapse Analytics'inize bağlanın ve veritabanının ve tabloların içe aktarıldığını doğrulamak için **SQL Object Explorer'ı** açın. *SQLDW_Explorations.sql* dosyasını alın.

> [!NOTE]
> Paralel Veri Ambarı (PDW) sorgu düzenleyicisini açmak için, PDW'niz **SQL Object Explorer'da**seçilirken **Yeni Sorgu** komutunu kullanın. Standart SQL sorgu düzenleyicisi PDW tarafından desteklenmez.
>
>

Bu bölümde gerçekleştirilen veri arama ve özellik oluşturma görevlerinin türleri şunlardır:

* Değişen zaman pencerelerinde birkaç alanın veri dağılımlarını keşfedin.
* Boylam ve enlem alanlarının veri kalitesini araştırın.
* **Uç miktarına\_** göre ikili ve çok sınıflı sınıflandırma etiketleri oluşturun.
* Özellikler oluşturun ve yolculuk mesafelerini hesaplayın/karşılaştırın.
* İki tabloya katılın ve modeller oluşturmak için kullanılacak rastgele bir örnek ayıklayın.

### <a name="data-import-verification"></a>Veri alma doğrulaması
Bu sorgular, Polybase'in paralel toplu alma işlemi kullanılarak daha önce doldurulan tablolardaki satır ve sütun sayısının hızlı bir şekilde doğrulanmasını sağlar,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Çıktı:** 173.179.759 satır ve 14 sütun almalısınız.

### <a name="exploration-trip-distribution-by-medallion"></a>Keşif: Madalyon ile gezi dağılımı
Bu örnek sorgu, belirli bir süre içinde 100'den fazla seyahati tamamlayan madalyonları (taksi numaraları) tanımlar. Bu **\_alma datetime**bölümleme düzeni tarafından koşullandırılmış olduğundan sorgu bölümlenmiş tablo erişimi yararlanacaktır. Tam veri kümesini sorgulamak, bölümlenmiş tablo ve/veya dizin tarar'ı da kullanır.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Çıktı:** Sorgu, 13.369 madalyon (taksi) ve 2013 yılında tamamlanan seyahat sayısını belirten satırlar içeren bir tablo döndürmelidir. Son sütun, tamamlanan gezi sayısının sayısını içerir.

### <a name="exploration-trip-distribution-by-medallion-and-hack_license"></a>Keşif: Madalyon ve hack_license göre gezi dağılımı
Bu örnek, belirli bir süre içinde 100'den fazla seyahati tamamlayan madalyonlar (taksi numaraları) ve hack_license sayılarını (sürücüleri) tanımlar.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Çıktı:** Sorgu, 2013 yılında 100'den fazla seyahati tamamlamış 13.369 araç/sürücü belgesini belirten 13.369 satırlık bir tablo döndürmelidir. Son sütun, tamamlanan gezi sayısının sayısını içerir.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Veri kalitesi değerlendirmesi: Kayıtları yanlış boylam ve/veya enlemle doğrulayın
Bu örnek, boylam ve/veya enlem alanlarından herhangi birinin geçersiz bir değer esahip olup olmadığını (radyan dereceleri -90 ile 90 arasında olmalıdır) veya (0, 0) koordinatlara sahip olup olmadığını araştırır.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Çıktı:** Sorgu, geçersiz boylam ve/veya enlem alanlarına sahip 837.467 seyahat döndürür.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Keşif: Uçlu vs uçlu değil uçlu geziler dağıtım
Bu örnek, belirli bir zaman diliminde bahşiş verilmeyen (veya tam yıl burada ayarlandığını kapsıyorsa tam veri kümesinde) uçlu tur ların sayısını bulur. Bu dağılım, daha sonra ikili sınıflandırma modellemesi için kullanılacak ikili etiket dağılımını yansıtır.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Çıktı:** Sorgu, 2013 yılı için aşağıdaki ipucu sıklıklarını döndürmelidir: 90.447.622 uçlu ve 82.264.709 uçlu değil.

### <a name="exploration-tip-classrange-distribution"></a>Ekspertİz: İpucu sınıfı/menzil dağılımı
Bu örnek, belirli bir zaman diliminde (veya tam yılı kapsıyorsa tam veri kümesinde) uç aralıklarının dağılımını hesaplar. Etiket sınıflarının bu dağılımı daha sonra çok sınıflı sınıflandırma modellemesi için kullanılacaktır.

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

**Çıkış:**

| tip_class | tip_freq |
| --- | --- |
| 1 |82230915 |
| 2 |6198803 |
| 3 |1932223 |
| 0 |82264625 |
| 4 |85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Keşif: Yolculuk mesafeni hesaplama ve karşılaştırma
Bu örnek, alma ve bırakma boylaşunu ve enlemisql coğrafya noktalarına dönüştürür, SQL coğrafya puan farkını kullanarak yolculuk mesafesini hesaplanır ve karşılaştırma sonuçlarının rasgele bir örneğini döndürür. Örnek, sonuçları yalnızca daha önce kapsanan veri kalitesi değerlendirme sorgusunu kullanarak geçerli koordinatlarla sınırlar.

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

### <a name="feature-engineering-using-sql-functions"></a>SQL işlevlerini kullanarak özellik mühendisliği
Bazen SQL işlevleri özellik mühendisliği için etkili bir seçenek olabilir. Bu izbarada, teslim alma ve bırakma konumları arasındaki doğrudan mesafeyi hesaplamak için bir SQL işlevi tanımladık. **Visual Studio Veri Araçları'nda**aşağıdaki SQL komut dosyalarını çalıştırabilirsiniz.

Burada mesafe işlevini tanımlayan SQL komut dosyası.

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

SQL sorgusunuzda özellikler oluşturmak için bu işlevi çağırmak için bir örnek aşağıda verilmiştir:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Çıktı:** Bu sorgu, teslim alma ve bırakma enlemleri ve boylamları ve kilometreler halinde karşılık gelen doğrudan mesafeleriçeren bir tablo (2.803.538 satırlı) oluşturur. İşte ilk üç satır için sonuçlar şunlardır:

|  | pickup_latitude | pickup_longitude | dropoff_latitude | dropoff_longitude | DirectDistance |
| --- | --- | --- | --- | --- | --- |
| 1 |40.731804 |-74.001083 |40.736622 |-73.988953 |.7169601222 |
| 2 |40.715794 |-74,010635 |40.725338 |-74.00399 |.7448343721 |
| 3 |40.761456 |-73.999886 |40.766544 |-73.988228 |0.7037227967 |

### <a name="prepare-data-for-model-building"></a>Model oluşturma için veri hazırlama
Aşağıdaki sorgu **\_nyctaxi gezisi** ve **nyctaxi\_ücret** tabloları katılır, bir ikili sınıflandırma etiketi **uçlu**oluşturur , çok sınıflı sınıflandırma etiket ipucu **\_sınıfı**, ve tam birleştirilmiş veri kümesinden bir örnek ayıklar. Örnekleme, teslim alma süresine göre gezilerin bir alt kümesini alarak yapılır.  Bu sorgu kopyalanabilir ve azure'daki SQL Veritabanı örneğinden doğrudan veri alımı için [Azure Machine Learning Studio (klasik)](https://studio.azureml.net) Alma [Veri][alma] modülüne doğrudan yapıştırılabilir. Sorgu, yanlış (0, 0) koordinatları olan kayıtları hariç tutar.

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

Azure Machine Learning'e geçmeye hazır olduğunuzda şunları da hissedebilirsin

1. Verileri ayıklamak ve örneklemek ve sorguyu doğrudan Azure Machine Learning'de[alma veri] modülüne kopyalamak için son SQL sorgusunu kaydedin veya
2. Yeni bir Azure Synapse Analytics tablosunda model oluşturma için kullanmayı planladığınız örneklenmiş ve tasarlanmış verileri devam edin ve Azure Machine Learning'de [Veri Alma][verisi] modülündeki yeni tabloyu kullanın. Önceki adımda PowerShell komut dosyası sizin için bu görevi yaptı. Veri Alma modülünde doğrudan bu tablodan okuyabilirsiniz.

## <a name="data-exploration-and-feature-engineering-in-ipython-notebook"></a><a name="ipnb"></a>IPython dizüstü bilgisayarda veri arama ve özellik mühendisliği
Bu bölümde, daha önce oluşturulan Azure Synapse Analytics'e karşı hem Python hem de SQL sorgularını kullanarak veri arama ve özellik oluşturma gerçekleştireceğiz. **SQLDW_Explorations.ipynb** adlı örnek bir IPython dizüstü bilgisayar ve **SQLDW_Explorations_Scripts.py** python komut dosyası dosyası yerel dizininize indirilmiştir. Onlar da [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW)mevcuttur. Bu iki dosya Python komut dosyalarında aynıdır. IPython Notebook sunucunuz yoksa Python komut dosyası dosyası size sağlanır. Bu iki örnek Python dosyaları **Python 2.7**altında tasarlanmıştır.

Örnek IPython Notebook'ta ve yerel makinenize indirilen Python komut dosyasıdosyasındaki gerekli Azure Synapse Analytics bilgileri daha önce PowerShell komut dosyası tarafından fişe takılmıştır. Herhangi bir değişiklik yapılmadan çalıştırılabilirler.

Azure Machine Learning çalışma alanı ayarladıysanız, örnek IPython Not Defterini doğrudan AzureML IPython Notebook hizmetine yükleyebilir ve çalıştırmaya başlayabilirsiniz. AzureML IPython Notebook hizmetine yükleme adımları şunlardır:

1. Azure Machine Learning çalışma alanınızda oturum açın, üstteki **Studio"yu** tıklatın ve web sayfasının sol tarafındaki **NOTEBOOK'ları** tıklatın.

    ![Studio'ya tıklayın ve NOT DEFTERLER][22]
2. Web sayfasının sol alt köşesinde **Yenİ'yi** tıklatın ve **Python 2'yi**seçin. Ardından, not defterine bir ad sağlayın ve yeni boş IPython Not Defteri'ni oluşturmak için onay işaretini tıklatın.

    ![Yenİ'yi tıklatın ve Python 2'yi seçin][23]
3. Yeni IPython Notebook'un sol üst köşesindeki **Jupyter** simgesini tıklatın.

    ![Jupyter simgesini tıklatın][24]
4. Örnek IPython Not Defterini AzureML IPython Notebook hizmetinizin **ağaç** sayfasına sürükleyip bırakın ve **Yükle'yi**tıklatın. Ardından, örnek IPython Notebook AzureML IPython Notebook hizmetine yüklenir.

    ![Yükle'yi tıklatın][25]

Örnek IPython Notebook veya Python komut dosyası dosyasını çalıştırmak için aşağıdaki Python paketleri gereklidir. AzureML IPython Notebook hizmetini kullanıyorsanız, bu paketler önceden yüklenmiş.

- Pandalar
- nümpil
- matplotlib
- pyodbc
- PyTables

Azure Machine Learning'de büyük verilerle gelişmiş analitik çözümler üretilirken, önerilen sıra aşağıda verilmiştir:

* Verilerin küçük bir örneğinde bellek içi bir veri çerçevesi ne kadar okunur.
* Örneklenmiş verileri kullanarak bazı görselleştirmeler ve keşifler gerçekleştirin.
* Örneklenmiş verileri kullanarak özellik mühendisliği ile denemeler.
* Daha büyük veri arama, veri işleme ve özellik mühendisliği için, SQL Sorgularını doğrudan Azure Synapse Analytics'e karşı vermek için Python'u kullanın.
* Örnek boyutunun Azure Machine Learning model oluşturmaya uygun olacağına karar verin.

Aşağıda birkaç veri arama, veri görselleştirme ve özellik mühendisliği örnekleri verilmiştir. Örnek IPython Notebook'ta ve örnek Python komut dosyası dosyasında daha fazla veri keşfi bulunabilir.

### <a name="initialize-database-credentials"></a>Veritabanı kimlik bilgilerini başlatma
Veritabanı bağlantı ayarlarınızı aşağıdaki değişkenlerde başlatma:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Veritabanı bağlantısı oluşturma
Veritabanına bağlantı oluşturan bağlantı dizesi aşağıda veda eder.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxi_trip"></a>Tablo <satır ve sütun sayısını nyctaxi_trip>
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

* Toplam satır sayısı = 173179759
* Toplam sütun sayısı = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxi_fare"></a>Tablodaki satır ve sütun sayısını rapor <nyctaxi_fare>
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

* Toplam satır sayısı = 173179759
* Toplam sütun sayısı = 11

### <a name="read-in-a-small-data-sample-from-the-azure-synapse-analytics-database"></a>Azure Synapse Analytics Veritabanı'ndan küçük bir veri örneği okuma
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

Örnek tabloyu okuma süresi 14.096495 saniyedir.
Alınan satır ve sütun sayısı = (1000, 21).

### <a name="descriptive-statistics"></a>Açıklayıcı istatistikler
Artık örneklenmiş verileri keşfetmeye hazırsınız. **Seyahat\_mesafesi** (veya belirtmeyi seçtiğiniz diğer alanlar) için bazı açıklayıcı istatistiklere bakarak başlarız.

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Görselleştirme: Kutu çizimi örneği
Daha sonra quantiles görselleştirmek için yolculuk mesafesi için kutu arsa bakmak.

    df1.boxplot(column='trip_distance',return_type='dict')

![Kutu çizim çıktısı][1]

### <a name="visualization-distribution-plot-example"></a>Görselleştirme: Dağıtım çizimi örneği
Dağılım ve örneklenmiş yolculuk mesafeleri için bir histogram görselleştirmek Çizimler.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Dağıtım çizimi çıktısı][2]

### <a name="visualization-bar-and-line-plots"></a>Görselleştirme: Çubuk ve çizgi çizimleri
Bu örnekte, yolculuk mesafesini beş kutuya sığdırıyoruz ve binning sonuçlarını görselleştiriyoruz.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Yukarıdaki çöp kutusu dağılımını bir çubuk veya çizgi çiziminde çizebiliriz:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Çubuk çizim çıktısı][3]

ve

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Çizgi çizim çıktısı][4]

### <a name="visualization-scatterplot-examples"></a>Görselleştirme: Scatterplot örnekleri
Herhangi bir korelasyon olup olmadığını görmek için **\_\_\_secs** ve **\_gezi mesafesi** içinde yolculuk süresi arasında dağılım arsa göstermek

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Zaman ve uzaklık arasındaki ilişkinin dağılım çizimi çıktısı][6]

Benzer **şekilde, fiyat\_kodu** ve **yolculuk\_mesafesi**arasındaki ilişkiyi kontrol edebiliriz.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Kod ve uzaklık arasındaki ilişkinin dağılım çizimi çıktısı][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>IPython not defterindeki SQL sorgularını kullanarak örneklenmiş veriler üzerinde veri arama
Bu bölümde, yukarıda oluşturduğumuz yeni tabloda kalıcı olan örneklenmiş verileri kullanarak veri dağılımlarını inceleriz. Benzer keşifler orijinal tablolar kullanılarak yapılabilir.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Eksplin: Örneklenmiş tablodaki satır ve sütun sayısını rapor
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Keşif: Uçlu /tökezleyen Dağıtım
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Eksperton: İpucu sınıf dağılımı
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Keşif: Uç dağılımını sınıfa göre çizin
    tip_class_dist['tip_freq'].plot(kind='bar')

![Arsa #26][26]

#### <a name="exploration-daily-distribution-of-trips"></a>Keşif: Gezilerin günlük dağılımı
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Keşif: Madalyon başına gezi dağılımı
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Keşif: Madalyon ve hack lisansı ile Trip dağıtım
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Keşif: Yolculuk süresi dağılımı
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Keşif: Yolculuk mesafesi dağılımı
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Arama: Ödeme türü dağılımı
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Featurized tablonun son şeklini doğrulayın
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="build-models-in-azure-machine-learning"></a><a name="mlmodel"></a>Azure Machine Learning'de modeller oluşturun
[Azure Machine Learning'de](https://studio.azureml.net)model oluşturma ve model dağıtımına geçmeye hazırız. Veriler, daha önce tanımlanan tahmin sorunlarından herhangi birinde kullanılmaya hazırdır:

1. **İkili sınıflandırma**: Bir seyahat için bahşiş ödenip ödenmediğini tahmin etmek.
2. **Çok sınıflı sınıflandırma**: Daha önce tanımlanan sınıflara göre ödenen bahşiş aralığını tahmin etmek.
3. **Regresyon görevi**: Bir yolculuk için ödenen bahşiş miktarını tahmin etmek.

Modelleme alıştırmasına başlamak için Azure **Machine Learning (klasik)** çalışma alanınızda oturum açın. Henüz bir makine öğrenme çalışma alanı oluşturmadıysanız, [bkz.](../studio/create-workspace.md)

1. Azure Machine Learning'e başlamak için Azure [Machine Learning Studio (klasik) nedir?](../studio/what-is-ml-studio.md)
2. Azure Machine Learning Studio 'ya giriş yapın [(klasik)](https://studio.azureml.net).
3. Machine Learning Studio (klasik) Ana sayfa bilgi, video, öğreticiler, Modüller Referans bağlantılar ve diğer kaynakların bir zenginlik sağlar. Azure Machine Learning hakkında daha fazla bilgi için [Azure Machine Learning Dokümantasyon Merkezi'ne](https://azure.microsoft.com/documentation/services/machine-learning/)bakın.

Tipik bir eğitim deneyi aşağıdaki adımlardan oluşur:

1. **+Yenİ** deneme oluşturun.
2. Verileri Azure Machine Learning Studio'ya (klasik) alın.
3. Verileri gerektiği gibi önceden işleme, dönüştürme ve işleme.
4. Gerektiğinde özellikler oluşturun.
5. Verileri eğitim/doğrulama/test veri kümelerine bölün (veya her biri için ayrı veri kümelerine sahip oyla) ayırın.
6. Çözmek için öğrenme sorununa bağlı olarak bir veya daha fazla makine öğrenme algoritması seçin. Örneğin, ikili sınıflandırma, çok sınıflı sınıflandırma, regresyon.
7. Eğitim veri kümesini kullanarak bir veya daha fazla model eğitin.
8. Eğitilmiş modeli(ler) kullanarak doğrulama veri kümesini puan.
9. Öğrenme sorunu yla ilgili ölçümleri hesaplamak için modeli(ler) değerlendirin.
10. Modeli(ler) ayarlayın ve dağıtmak için en iyi modeli seçin.

Bu alıştırmada, Azure Synapse Analytics'teki verileri araştırdık ve tasarladık ve Azure Machine Learning Studio'da (klasik) sindirilen örnek boyutuna karar verdik. Tahmin modellerinden birini veya daha fazlasını oluşturma yordamı aşağıda veda edin:

1. **Verileri, Veri Girişi ve Çıktısı** bölümünde bulunan [Alma][Verisi alma verimodülasyon] modüllerini kullanarak Azure Machine Learning Studio'ya (klasik) alın. Daha fazla bilgi [için, İçe Aktar][Veri-veri] modülü başvuru sayfasına bakın.

    ![Azure ML Alma Verileri][17]
2. **Özellikler** panelinde **Veri kaynağı** olarak Azure **SQL Veritabanı'nı** seçin.
3. **Veritabanı sunucu adı** alanına veritabanı DNS adını girin. Biçim:`tcp:<your_virtual_machine_DNS_name>,1433`
4. İlgili alana **Veritabanı adını** girin.
5. Server *kullanıcı* hesabı adına SQL kullanıcı **adını**ve Server kullanıcı **hesabı parolasına** *parolayı* girin.
7. Veritabanı **sorgu** metnini edit alanında, gerekli veritabanı alanlarını ayıklayan sorguyu (etiketler gibi tüm işlemalanları dahil) yapıştırın ve verileri istenen örnek boyutuna indirin.

Doğrudan Azure Synapse Analytics veritabanından veri okuma ikili sınıflandırma denemesiörneği aşağıdaki şekildedir (şema adı ve izbölümünizde kullandığınız tablo adları ile nyctaxi_trip ve nyctaxi_fare tablo adlarını değiştirmeyi unutmayın). Benzer deneyler çok sınıflı sınıflandırma ve regresyon sorunları için oluşturulabilir.

![Azure ML Tren][10]

> [!IMPORTANT]
> Önceki bölümlerde sağlanan modelleme veri ayıklama ve örnekleme sorgusu örneklerinde, **üç modelleme alıştırması için tüm etiketler sorguya dahil edilir.** Modelleme çalışmalarının her birinde önemli bir (gerekli) adım, diğer iki sorun ve diğer **hedef sızıntıları**için gereksiz etiketleri **dışlamaktır.** Örneğin, ikili sınıflandırma kullanırken, **uçlu** etiketi kullanın ve alanları **ucu sınıfı,\_uç** **\_miktarı**ve toplam **\_tutarı**hariç. İkincisi, bahşişin ödendiğini ima ettikleri için hedef sızıntılarıdır.
>
> Gereksiz sütunları veya hedef sızıntıları hariç tutmak [için, Dataset modülündeki Sütunları Seç'i][select-columns] veya [Meta Verilerini Edit'i][edit-metadata]kullanabilirsiniz. Daha fazla bilgi için Bkz. [Veri Kümesi'ndeki Sütunları Seç][select-columns] ve Meta veri başvuru sayfalarını [edin.][edit-metadata]
>
>

## <a name="deploy-models-in-azure-machine-learning"></a><a name="mldeploy"></a>Azure Machine Learning'de modelleri dağıtma
Modeliniz hazır olduğunda, doğrudan denemeden web hizmeti olarak kolayca dağıtabilirsiniz. Azure ML web hizmetlerini dağıtma hakkında daha fazla bilgi [için](../studio/deploy-a-machine-learning-web-service.md)bkz.

Yeni bir web hizmeti dağıtmak için şunları yapmanız gerekir:

1. Bir puanlama deneyi oluşturun.
2. Web hizmetini dağıtın.

**Bitmiş** bir eğitim deneyinden bir puanlama denemesi oluşturmak için, alt eylem çubuğunda **CREATE SCORING EXPERIMENT'i** tıklatın.

![Azure Puanlama][18]

Azure Machine Learning, eğitim deneyinin bileşenlerine dayalı bir puanlama denemesi oluşturmaya çalışır. Özellikle, olacak:

1. Eğitimli modeli kaydedin ve model eğitim modüllerini kaldırın.
2. Beklenen giriş veri şemasını temsil etmek için mantıksal bir **giriş bağlantı noktası** tanımlayın.
3. Beklenen web hizmeti çıktı şemasını temsil edecek mantıksal bir **çıktı bağlantı noktası** tanımlayın.

Puanlama denemesi oluşturulduğunda, sonuçları gözden geçirin ve gerektiği gibi ayarlayın. Bu etiket alanları hizmeti ararken şema eşlenen olmaz, çünkü tipik bir ayarlama, giriş veri kümesi veya sorgu etiket alanları hariç biri ile değiştirmektir. Giriş veri kümesinin ve/veya sorgunun boyutunu birkaç kayda düşürmek de iyi bir uygulamadır ve giriş şemasını belirtmek için yeterlidir. Çıkış bağlantı noktası için, tüm giriş alanlarını dışlamak ve [yalnızca Dataset modülündeki Sütunları Seç'i][select-columns] kullanarak çıktıya **Yalnızca Puanlı Etiketler** ve **Puanlı Olasılıkları** eklemek yaygındır.

Örnek puanlama deneyi aşağıdaki şekilde verilmiştir. Dağıtmaya hazır olduğunuzda, alt eylem çubuğundaki **WEB SERVICE'i yayımla** düğmesini tıklatın.

![Azure ML Yayın][11]

## <a name="summary"></a>Özet
Bu izblama öğreticisinde yaptıklarımızı özetlemek için, bir Azure veri bilimi ortamı oluşturdunuz, büyük bir genel veri kümesiyle çalıştınız, ekip veri bilimi sürecinden geçirerek, veri toplamadan model eğitimine ve ardından bir Azure Machine Learning web hizmetinin dağıtımına kadar.

### <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve beraberindeki komut dosyaları ve IPython not defterleri Microsoft tarafından MIT lisansı altında paylaşılır. Daha fazla bilgi için GitHub'daki örnek kodun dizinindeki LICENSE.txt dosyasını kontrol edin.

## <a name="references"></a>Başvurular
- [Andrés Monroy NYC Taksi Gezileri İndirme Sayfası](https://www.andresmh.com/nyctaxitrips/)
- [Chris Whong tarafından NYC's Taksi Trip Veri FOILing](https://chriswhong.com/open-data/foil_nyc_taxi/)
- [NYC Taksi ve Limuzin Komisyonu Araştırma ve İstatistik](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

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
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
