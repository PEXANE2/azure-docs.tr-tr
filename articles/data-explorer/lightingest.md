---
title: LightIngest, Azure Veri Gezgini'ne giriş için bir komut satırı yardımcı programıdır.
description: Azure Veri Gezgini'ne geçici veri alımı için bir komut satırı yardımcı programı olan LightIngest hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/17/2020
ms.openlocfilehash: 99517e45892cd7a6167ae83ff3058edae1377b10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80109569"
---
# <a name="install-and-use-lightingest"></a>LightIngest'i kurun ve kullanın

LightIngest, Azure Veri Gezgini'ne geçici veri alımı için bir komut satırı yardımcı programıdır.
Yardımcı program, kaynak verilerini yerel bir klasörden veya Azure blob depolama kapsayıcısından çekebilir.

## <a name="prerequisites"></a>Ön koşullar

* LightIngest - [Microsoft.Azure.Kusto.Tools NuGet paketinin](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Tools/) bir parçası olarak indirin
* WinRAR - [www.win-rar.com/download.html](http://www.win-rar.com/download.html) indirin

## <a name="install-lightingest"></a>LightIngest'i yükleyin

1. LightIngest'i indirdiğiniz bilgisayarınızdaki konuma gidin. 
1. WinRAR'ı *kullanarak, araçları* bilgisayarınıza ayıklayın.

## <a name="run-lightingest"></a>En Hafif Çalıştır

1. Bilgisayarınızdaki ayıklanan *araçlar* dizinine gidin.
1. Konum çubuğundan varolan konum bilgilerini silin.
    
      ![Konum bilgilerini silme](media/lightingest/lightingest-location-bar.png)

1. Girin `cmd` ve **Enter**tuşuna basın.
1. Komut isteminde, `LightIngest.exe` ilgili komut satırı bağımsız değişkenini takip ederek girin.

    > [!Tip]
    > Desteklenen komut satırı bağımsız değişkenlerinin listesi `LightIngest.exe /help`için .
    >
    >![Komut satırı Yardım](media/lightingest/lightingest-cmd-line-help.png)

1. Azure `LightIngest` Veri Gezgini kümesine ve alım işlemini yönetecek bağlantı dizesini girin.
    Bağlantı dizesini çift tırnak içine alalım ve [Kusto bağlantı dizeleri belirtimini](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto)izleyin.

    Örnek:
    ```
    LightIngest "Data Source=https://{Cluster name and region}.kusto.windows.net;AAD Federated Security=True"  -db:{Database} -table:Trips -source:"https://{Account}.blob.core.windows.net/{ROOT_CONTAINER};{StorageAccountKey}" -pattern:"*.csv.gz" -format:csv -limit:2 -ignoreFirst:true -cr:10.0 -dontWait:true
    ```

* Önerilen yöntem, `LightIngest` `https://ingest-{yourClusterNameAndRegion}.kusto.windows.net`yutma bitiş noktası ile çalışmak için. Bu şekilde, Azure Veri Gezgini hizmeti yutma yükünü yönetebilir ve geçici hatalardan kolayca kurtulabilirsiniz. Ancak, doğrudan motor `LightIngest` bitiş noktası ile çalışmak için`https://{yourClusterNameAndRegion}.kusto.windows.net`yapılandırabilirsiniz ( ).
* En iyi yutma performansı için LightIngest'in ham veri `LightIngest` boyutunu bilmesi önemlidir ve böylece yerel dosyaların sıkıştırılmamış boyutunu tahmin edecektir. Ancak, `LightIngest` ilk önce indirmeden sıkıştırılmış lekelerin ham boyutunu doğru tahmin etmek mümkün olmayabilir. Bu nedenle, sıkıştırılmış lekeleri sindirirken, blob meta verilerindeki `rawSizeBytes` özelliği baytlarda sıkıştırılmamış veri boyutuna ayarlayın.

## <a name="general-command-line-arguments"></a>Genel komut satırı bağımsız değişkenleri

|Bağımsız değişken adı         |Kısa ad   |Tür    |Zorunlu |Açıklama                                |
|----------------------|-------------|--------|----------|-------------------------------------------|
|                      |             |string  |Zorunlu |[Azure Veri Gezgini Bağlantı Dizesi,](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto) yutma işlemini işleyecek Kusto uç noktasını belirtir. Çift tırnak içinde eklenmelidir |
|-veritabanı             |-db          |string  |İsteğe bağlı  |Hedef Azure Veri Gezgini veritabanı adı |
|-tablo                |             |string  |Zorunlu |Hedef Azure Veri Gezgini tablo adı |
|-sourcePath           |-kaynak      |string  |Zorunlu |Kaynak dosyaları veya blob kapsayıcı kök URI yolu. Veriler blobs ise, depolama hesabı anahtarı veya SAS içermelidir. Çift tırnak içine önerilir |
|-önek               |             |string  |İsteğe bağlı  |Yutulacak kaynak veriler blob depolamada bulunduğunda, bu URL öneki kapsayıcı adı hariç tüm blobs tarafından paylaşılır. <br>Örneğin, veriler içindeyse, `MyContainer/Dir1/Dir2`önek . `Dir1/Dir2` Çift tırnak içinde çevrelenen önerilir |
|-desen              |             |string  |İsteğe bağlı  |Kaynak dosyalarının/blob'ların seçildiği desen. Joker karakterleri destekler. Örneğin, `"*.csv"`. Çift tırnak içine önerilir |
|-zipPattern           |             |string  |İsteğe bağlı  |Zip arşivindeki hangi dosyaların yutulması gerektiğini seçerken kullanılacak normal ifade.<br>Arşivdeki diğer tüm dosyalar yoksayılır. Örneğin, `"*.csv"`. Çift tırnak içinde çevrelenebilmek için tavsiye edilir. |
|-format               |-f           |string  |İsteğe bağlı  |Kaynak veri biçimi. [Desteklenen biçimlerden](https://docs.microsoft.com/azure/kusto/management/data-ingestion/#supported-data-formats) biri olmalı |
|-yutmaMappingPath |-mappingPath |string  |İsteğe bağlı  |Yutma sütunu eşleme dosyasına giden yol (Json ve Avro biçimleri için zorunludur). [Veri eşlemelerine](https://docs.microsoft.com/azure/kusto/management/mappings) bakın |
|-ingestionMappingRef  |-mappingRef  |string  |İsteğe bağlı  |Önceden oluşturulmuş bir yutma sütunu eşlemeadı (Json ve Avro biçimleri için zorunludur). [Veri eşlemelerine](https://docs.microsoft.com/azure/kusto/management/mappings) bakın |
|-creationTimePattern  |             |string  |İsteğe bağlı  |Ayarlandığında, CreationTime özelliğini dosyadan veya blob yolundan ayıklamak için kullanılır. Bkz. [CreationTimePattern bağımsız değişkenini kullanma](#using-creationtimepattern-argument) |
|-ignoreFirstRow       |-ignoreFirst |bool    |İsteğe bağlı  |Ayarlanırsa, her dosya/blob'un ilk kaydı yoksayılır (örneğin, kaynak verilerin üstbilgileri varsa) |
|-etiket                  |             |string  |İsteğe bağlı  |[Etiketler](https://docs.microsoft.com/azure/kusto/management/extents-overview#extent-tagging) yutulan verilerle ilişkilendirilir. Birden fazla olaya izin verilir |
|-dontWait             |             |bool    |İsteğe bağlı  |'True' olarak ayarlanırsa, yutulması için beklemez. Büyük miktarda dosya/blob sindirirken kullanışlıdır |

### <a name="using-creationtimepattern-argument"></a>CreationTimePattern bağımsız değişkenini kullanma

Bağımsız `-creationTimePattern` değişken, CreationTime özelliğini dosya veya blob yolundan ayıklar. Desen, yalnızca kullanmak istediğiniz zaman damgasını çevreleyen bölümü, tüm öğe yolunu yansıtmak gerekmez.
Bağımsız değişkenin değeri üç bölümden oluşmalıdır:
* Zaman damgasından hemen önce, tek tırnak içinde kapalı sabit test
* Zaman damgası biçimi, standart [.NET DateTime gösteriminde](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings)
* Zaman damgasını hemen takip eden sabit metin Örneğin, blob adları 'historicalvalues19840101.parke' ile bitiyorsa (zaman damgası yıl için dört basamak, ay için iki `-creationTimePattern` basamak ve ayın günü için iki basamak), bağımsız değişkeniçin karşılık gelen değer 'tarihsel değerler'yyyyMDD'.parke'dir.

### <a name="command-line-arguments-for-advanced-scenarios"></a>Gelişmiş senaryolar için komut satırı bağımsız değişkenleri

|Bağımsız değişken adı         |Kısa ad   |Tür    |Zorunlu |Açıklama                                |
|----------------------|-------------|--------|----------|-------------------------------------------|
|-sıkıştırma          |-cr          |double  |İsteğe bağlı  |Sıkıştırma oranı ipucu. Azure Veri Gezgini'nin ham veri boyutunu değerlendirmesine yardımcı olmak için sıkıştırılmış dosyaları/blob'ları sindirirken kullanışlıdır. Sıkıştırılmış boyuta bölünen orijinal boyut olarak hesaplanır |
|-limit                |-l           |integer |İsteğe bağlı  |Ayarlanırsa, ilk N dosyalarıyla alımını sınırlar |
|-listSadece             |-liste        |bool    |İsteğe bağlı  |Ayarlanırsa, yalnızca yutma için seçili olan öğeleri görüntüler| 
|-yutmaTimeout        |             |integer |İsteğe bağlı  |Tüm yutma işlemleri tamamlanması için dakika içinde zaman zaman. Varsayılan lar`60`|
|-forceSync            |             |bool    |İsteğe bağlı  |Ayarlanırsa, senkronize yutmayı zorlar. Varsayılan lar`false` |
|-dataBatchSize        |             |integer |İsteğe bağlı  |Her yutma işleminin toplam boyut sınırını (MB, sıkıştırılmamış) ayarlar |
|-filesInBatch         |             |integer |İsteğe bağlı  |Her yutma işleminin dosya/blob sayım sınırını ayarlar |
|-devTracing           |-izleme       |string  |İsteğe bağlı  |Ayarlanırsa, tanılama günlükleri yerel bir dizine `RollingLogs` yazılır (varsayılan olarak geçerli dizinde veya anahtar değerini ayarlayarak değiştirilebilir) |

## <a name="blob-metadata-properties"></a>Blob meta veri özellikleri
Azure lekeleri ile kullanıldığında, `LightIngest` yutma işlemini artırmak için belirli blob meta veri özelliklerini kullanır.

|Meta veri özelliği                            | Kullanım                                                                           |
|---------------------------------------------|---------------------------------------------------------------------------------|
|`rawSizeBytes`, `kustoUncompressedSizeBytes` | Ayarlanırsa, sıkıştırılmamış veri boyutu olarak yorumlanır                       |
|`kustoCreationTime`, `kustoCreationTimeUtc`  | UTC zaman damgası olarak yorumlanır. Ayarlanırsa, Kusto'daki oluşturma süresini geçersiz kılmak için kullanılır. Senaryoları biriktirmek için kullanışlıdır |

## <a name="usage-examples"></a>Kullanım örnekleri

<!-- Waiting for Tzvia or Vladik to rewrite the instructions for this example before publishing it

### Ingesting a specific number of blobs in JSON format

* Ingest two blobs under a specified storage account {Account}, in `JSON` format matching the pattern `.json`
* Destination is the database {Database}, the table `SampleData`
* Indicate that your data is compressed with the approximate ratio of 10.0
* LightIngest won't wait for the ingestion to be completed

To use the LightIngest command below:
1. Create a table command and enter the table name into the LightIngest command, replacing `SampleData`.
1. Create a mapping command and enter the IngestionMappingRef command, replacing `SampleData_mapping`.
1. Copy your cluster name and enter it into the LightIngest command, replacing `{ClusterandRegion}`.
1. Enter the database name into the LightIngest command, replacing `{Database name}`.
1. Replace `{Account}` with your account name and replace `{ROOT_CONTAINER}?{SAS token}` with the appropriate information.

    ```
    LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.windows.net;Fed=True"  
        -db:{Database name} 
        -table:SampleData 
        -source:"https://{Account}.blob.core.windows.net/{ROOT_CONTAINER}?{SAS token}" 
        -IngestionMappingRef:SampleData_mapping 
        -pattern:"*.json" 
        -format:JSON 
        -limit:2 
        -cr:10.0 
        -dontWait:true
    ```
     
1. In Azure Data Explorer, open query count.

    ![Injestion result in Azure Data Explorer](media/lightingest/lightingest-show-failure-count.png)
-->

### <a name="ingesting-blobs-using-a-storage-account-key-or-a-sas-token"></a>Depolama hesabı anahtarı veya SAS belirteci kullanarak lekeleri yutma

* Belirtilen depolama hesabının `ACCOUNT`altında 10 blobs yutmak , klasörde `DIR`, konteyner `CONT`altında , ve desen eşleşen`*.csv.gz`
* Hedef veritabanı, `DB` `TABLE`tablo ve yutma `MAPPING` eşleme hedef üzerinde önceden oluşturulur
* Araç, yutma işlemleri tamamlanana kadar bekleyecek
* Hedef veritabanı ve depolama hesabı anahtarı ve SAS belirteci belirtmek için farklı seçenekler enot

```
LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.windows.net;Fed=True"
  -database:DB
  -table:TABLE
  -source:"https://ACCOUNT.blob.core.windows.net/{ROOT_CONTAINER};{StorageAccountKey}"
  -prefix:"DIR"
  -pattern:*.csv.gz
  -format:csv
  -mappingRef:MAPPING
  -limit:10

LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.windows.net;Fed=True;Initial Catalog=DB"
  -table:TABLE
  -source:"https://ACCOUNT.blob.core.windows.net/{ROOT_CONTAINER}?{SAS token}"
  -prefix:"DIR"
  -pattern:*.csv.gz
  -format:csv
  -mappingRef:MAPPING
  -limit:10
```

### <a name="ingesting-all-blobs-in-a-container-not-including-header-rows"></a>Üstbilgi satırları dahil değil, bir kapta tüm lekeleri yutma

* Belirtilen depolama hesabı `ACCOUNT`altında tüm lekeleri `DIR1/DIR2`yutmak `CONT`, klasörde , konteyner altında , ve desen eşleşen`*.csv.gz`
* Hedef veritabanı, `DB` `TABLE`tablo ve yutma `MAPPING` eşleme hedef üzerinde önceden oluşturulur
* Kaynak lekeleri üstbilgi satırı içerir, bu nedenle alete her blob'un ilk kaydını bırakması talimatı verilir
* Araç, verileri yutma için yayınlayacak ve yutma işlemlerinin tamamlanmasını beklemez

```
LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.windows.net;Fed=True"
  -database:DB
  -table:TABLE
  -source:"https://ACCOUNT.blob.core.windows.net/{ROOT_CONTAINER}?{SAS token}"
  -prefix:"DIR1/DIR2"
  -pattern:*.csv.gz
  -format:csv
  -mappingRef:MAPPING
  -ignoreFirstRow:true
```

### <a name="ingesting-all-json-files-from-a-path"></a>Tüm JSON dosyalarını bir yoldan yutma

* Tüm dosyaları yol `PATH`altında yutma , deseni eşleştirme`*.json`
* Hedef veritabanı, `DB` `TABLE`tablo ve yutma eşleme yerel dosyada tanımlanır`MAPPING_FILE_PATH`
* Araç, verileri yutma için yayınlayacak ve yutma işlemlerinin tamamlanmasını beklemez

```
LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.windows.net;Fed=True"
  -database:DB
  -table:TABLE
  -source:"PATH"
  -pattern:*.json
  -format:json
  -mappingPath:"MAPPING_FILE_PATH"
```

### <a name="ingesting-files-and-writing-diagnostic-trace-files"></a>Dosyaları yutma ve tanılama izleme dosyaları yazma

* Tüm dosyaları yol `PATH`altında yutma , deseni eşleştirme`*.json`
* Hedef veritabanı, `DB` `TABLE`tablo ve yutma eşleme yerel dosyada tanımlanır`MAPPING_FILE_PATH`
* Araç, verileri yutma için yayınlayacak ve yutma işlemlerinin tamamlanmasını beklemez
* Tanılama izleme dosyaları klasörün altında yerel olarak yazılır`LOGS_PATH`

```
LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.windows.net;Fed=True"
  -database:DB
  -table:TABLE
  -source:"PATH"
  -pattern:*.json
  -format:json
  -mappingPath:"MAPPING_FILE_PATH"
  -trace:"LOGS_PATH"
```
## <a name="changelog"></a>Changelog
|Sürüm        |Değişiklikler                                                                             |
|---------------|------------------------------------------------------------------------------------|
|4.0.9.0        |<ul><li>Eklenen `-zipPattern` bağımsız değişken</li><li>Eklenen `-listOnly` bağımsız değişken</li><li>Bağımsız değişkenler özeti çalıştırma başlamadan önce görüntülenir</li><li>CreationTime `-creationTimePattern` blob meta veri özelliklerinden veya blob veya dosya adından, bağımsız değişkene göre okunur</li></ul>|
