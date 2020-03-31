---
title: Azure Veri Gezgini'ni kullanarak Azure Veri Gölü'ndeki verileri sorgula
description: Azure Veri Gezgini'ni kullanarak Azure Veri Gölü'nde verileri nasıl sorgulayabilirsiniz öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/17/2019
ms.openlocfilehash: 8240b1a01aa39e53b9ae41f73543ccf9774290b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77161758"
---
# <a name="query-data-in-azure-data-lake-using-azure-data-explorer"></a>Azure Veri Gezgini'ni kullanarak Azure Veri Gölü'ndeki verileri sorgula

Azure Veri Gölü Depolama, büyük veri analitiği için yüksek ölçeklenebilir ve uygun maliyetli bir veri gölü çözümüdür. Yüksek performanslı bir dosya sisteminin gücünü büyük ölçekli ve ekonomiyle birleştirerek zamanınızı hızla anlamanızı sağlar. Data Lake Storage Gen2, Azure Blob Depolama özelliklerini genişletir ve analitik iş yükleri için optimize edilecektir.
 
Azure Veri Gezgini, Azure Blob Depolama ve Azure Veri Gölü Depolama (Gen1 ve Gen2) ile tümleşerek göldeki verilere hızlı, önbelleğe alınmış ve dizine erişim sağlar. Azure Veri Gezgini'ne girmeden göldeki verileri analiz edebilir ve sorgulayabilirsiniz. Ayrıca, yutulan ve uningested yerel göl verileri arasında aynı anda sorgulayabilirsiniz.  

> [!TIP]
> En iyi sorgu performansı, Azure Veri Gezgini'nde veri aktarMasını gerektirir. Dış verileri önceden sn amacını kullanmadan sorgulama özelliği yalnızca geçmiş veriler veya nadiren sorgulanan veriler için kullanılmalıdır. En iyi sonuçlar için [göldeki sorgu performansınızı optimize edin.](#optimize-your-query-performance)
 

## <a name="create-an-external-table"></a>Harici tablo oluşturma

 > [!NOTE]
 > Şu anda desteklenen depolama hesapları Azure Blob Depolama veya Azure Veri Gölü Depolama (Gen1 ve Gen2) vardır.

1. Azure `.create external table` Veri Gezgini'nde harici bir tablo oluşturmak için komutu kullanın. , `.show`, `.drop`gibi ek dış `.alter` tablo komutları ve [Dış tablo komutları](/azure/kusto/management/externaltables)belgelenir.

    ```Kusto
    .create external table ArchivedProducts(
    Timestamp:datetime,
    ProductId:long, ProductDescription:string) 
    kind=blob
    partition by bin(Timestamp, 1d) 
    dataformat=csv (h@'http://storageaccount.blob.core.windows.net/container1;secretKey') 
    with (compressed = true)  
    ```
    
    > [!NOTE]
    > * Daha parçalı bölümleme ile daha fazla performans bekleniyor. Örneğin, günlük bölümleri olan dış tablolar üzerindeki sorgular, aylık bölümlenmiş tablolara sahip sorgulardan daha iyi performansa sahip olacaktır.
    > * Bölümleri olan harici bir tablo tanımladığınızda, depolama yapısının aynı olması beklenir.
Örneğin, tablo yyyy/MM/dd biçiminde (varsayılan) DateTime bölümüyle tanımlanırsa, URI depolama dosyası yolu *konteyner1/yyyy/MM/dd/all_exported_blobs*olmalıdır. 
    > * Dış tablo bir datetime sütununa göre bölümlenmişse, her zaman sorgunuzda kapalı bir `ArchivedProducts | where Timestamp between (ago(1h) .. 10m)` aralık için bir zaman filtresi ekleyin `ArchivedProducts | where Timestamp > ago(1h)` (örneğin, sorgu - - bu (açılan aralıktan) daha iyi performans göstermelidir . 
    > * Desteklenen tüm [yutma biçimleri](ingest-data-overview.md#supported-data-formats) dış tablolar kullanılarak sorgulanabilir.

1. Dış tablo Web UI'nin sol bölmesinde görünür

    ![web UI'deki dış tablo](media/data-lake-query-data/external-tables-web-ui.png)

### <a name="create-an-external-table-with-json-format"></a>json formatında harici bir tablo oluşturma

Json formatında harici bir tablo oluşturabilirsiniz. Daha fazla bilgi için [Bkz. Dış tablo komutları](/azure/kusto/management/externaltables)

1. `.create external table` *ExternalTableJson*adlı bir tablo oluşturmak için komutu kullanın:

    ```kusto
    .create external table ExternalTableJson (rownumber:int, rowguid:guid) 
    kind=blob
    dataformat=json
    ( 
       h@'http://storageaccount.blob.core.windows.net/container1;secretKey'
    )
    with 
    (
       docstring = "Docs",
       folder = "ExternalTables",
       namePrefix="Prefix"
    ) 
    ```
 
1. Json biçimi, aşağıda gösterildiği gibi sütunlara eşleme oluşturmanın ikinci bir adımını gerektirir. Aşağıdaki sorguda, *mappingName*adlı belirli bir json eşleme oluşturun:

    ```kusto
    .create external table ExternalTableJson json mapping "mappingName" '[{ "column" : "rownumber", "datatype" : "int", "path" : "$.rownumber"},{ "column" : "rowguid", "path" : "$.rowguid" }]' 
    ```

### <a name="external-table-permissions"></a>Dış tablo izinleri
 
* Veritabanı kullanıcısı harici bir tablo oluşturabilir. Tablo oluşturucusu otomatik olarak tablo yöneticisi olur.
* Küme, veritabanı veya tablo yöneticisi varolan bir tabloyu edinebilir.
* Herhangi bir veritabanı kullanıcısı veya okuyucusu harici bir tabloyu sorgulayabilir.
 
## <a name="query-an-external-table"></a>Harici tabloyu sorgula
 
Harici bir tabloyu sorgulamak için `external_table()` işlevi kullanın ve işlev bağımsız değişkeni olarak tablo adını sağlayın. Sorgunun geri kalanı standart Kusto sorgu dilidir.

```Kusto
external_table("ArchivedProducts") | take 100
```

> [!TIP]
> Intellisense şu anda dış tablo sorgularında desteklenmez.

### <a name="query-an-external-table-with-json-format"></a>Json formatında harici bir tabloyu sorgula

Json biçiminde harici bir tabloyu `external_table()` sorgulamak için işlevi kullanın ve işlev bağımsız değişkenleri olarak hem tablo adı hem de eşleme adı sağlayın. Aşağıdaki sorguda, *mappingName* belirtilmemişse, daha önce oluşturduğunuz bir eşleme kullanılır.

```kusto
external_table(‘ExternalTableJson’, ‘mappingName’)
```

## <a name="query-external-and-ingested-data-together"></a>Harici ve yutulan verileri birlikte sorgula

Aynı sorgu içinde hem dış tabloları hem de yutulan veri tablolarını sorgulayabilirsiniz. Siz [`join`](/azure/kusto/query/joinoperator) [`union`](/azure/kusto/query/unionoperator) veya Azure Veri Gezgini, SQL sunucuları veya diğer kaynaklardan ek veriler içeren harici tablo. A'yı [`let( ) statement`](/azure/kusto/query/letstatement) harici bir tablo başvurusuna bir steno adı atamak için kullanın.

Aşağıdaki örnekte, *Ürünler* yutulan bir veri tablosudur ve *Arşivlenmiş Ürünler,* Azure Veri Gölü Depolama Gen2'deki verileri içeren harici bir tablodur:

```kusto
let T1 = external_table("ArchivedProducts") |  where TimeStamp > ago(100d);
let T = Products; //T is an internal table
T1 | join T on ProductId | take 10
```

## <a name="query-taxirides-external-table-in-the-help-cluster"></a>Yardım kümesindeki *TaxiRides* harici tablosunu sorgula

*TaxiRides* örnek veri seti NYC Taksi [ve Limuzin Komisyonu](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)New York Taksi verileri içerir.

### <a name="create-external-table-taxirides"></a>Dış tablo *TaxiRides* oluşturun 

> [!NOTE]
> Bu *bölümde, yardım* kümesinde *TaxiRides* dış tablosunu oluşturmak için kullanılan sorgu gösterilebilir. Bu tablo zaten oluşturulduğu için bu bölümü atlayabilir ve [ *sorgu TaxiRides* dış tablo verileri](#query-taxirides-external-table-data)gerçekleştirebilirsiniz. 

1. Aşağıdaki sorgu yardım kümesinde dış tablo *TaxiRides* oluşturmak için kullanılmıştır. 

    ```kusto
    .create external table TaxiRides
    (
    trip_id: long,
    vendor_id: string, 
    pickup_datetime: datetime,
    dropoff_datetime: datetime,
    store_and_fwd_flag: string,
    rate_code_id: int,
    pickup_longitude: real,
    pickup_latitude: real,
    dropoff_longitude: real,
    dropoff_latitude: real,
    passenger_count: int,
    trip_distance: real,
    fare_amount: real,
    extra: real,
    mta_tax: real,
    tip_amount: real,
    tolls_amount: real,
    ehail_fee: real,
    improvement_surcharge: real,
    total_amount: real,
    payment_type: string,
    trip_type: int,
    pickup: string,
    dropoff: string,
    cab_type: string,
    precipitation: int,
    snow_depth: int,
    snowfall: int,
    max_temperature: int,
    min_temperature: int,
    average_wind_speed: int,
    pickup_nyct2010_gid: int,
    pickup_ctlabel: string,
    pickup_borocode: int,
    pickup_boroname: string,
    pickup_ct2010: string,
    pickup_boroct2010: string,
    pickup_cdeligibil: string,
    pickup_ntacode: string,
    pickup_ntaname: string,
    pickup_puma: string,
    dropoff_nyct2010_gid: int,
    dropoff_ctlabel: string,
    dropoff_borocode: int,
    dropoff_boroname: string,
    dropoff_ct2010: string,
    dropoff_boroct2010: string,
    dropoff_cdeligibil: string,
    dropoff_ntacode: string,
    dropoff_ntaname: string,
    dropoff_puma: string
    )
    kind=blob 
    partition by bin(pickup_datetime, 1d)
    dataformat=csv
    ( 
        h@'http://storageaccount.blob.core.windows.net/container1;secretKey''
    )
    ```
1. Elde edilen tablo *yardım* kümesinde oluşturuldu:

    ![TaxiRides harici tablo](media/data-lake-query-data/taxirides-external-table.png) 

### <a name="query-taxirides-external-table-data"></a>Sorgu *TaxiRides* harici tablo verileri 

*TaxiRides* [https://dataexplorer.azure.com/clusters/help/databases/Samples](https://dataexplorer.azure.com/clusters/help/databases/Samples) dış tabloyu sorgulamak için oturum açın. 

#### <a name="query-taxirides-external-table-without-partitioning"></a>Sorgu *TaxiRides* dış tablo bölümleme olmadan

Tüm veri kümesi boyunca, haftanın her günü için sürmek göstermek için dış tablo *TaxiRides* [bu sorguyu çalıştırın.](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAx3LSwqAMAwFwL3gHYKreh1xL7F9YrCtElP84OEV9zM4DZo5DsZjhGt6PqWTgL1p6+qhvaTEKjeI/FqyuZbGiwJf63QAi9vEL2UbAhtMEv6jyAH6+VhS9jOr1dULfUgAm2cAAAA=) 

```kusto
external_table("TaxiRides")
| summarize count() by dayofweek(pickup_datetime)
| render columnchart
```

Bu sorgu haftanın en yoğun gününü gösterir. Veriler bölümlenmediğinden, bu sorgunun sonuçları döndürmesi uzun sürebilir (birkaç dakikaya kadar).

![bölümlenmemiş sorgu oluşturma](media/data-lake-query-data/taxirides-no-partition.png)

#### <a name="query-taxirides-external-table-with-partitioning"></a>Sorgu TaxiRides dış tablo bölümleme ile 

[Bu sorguyu,](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA13NQQqDMBQE0L3gHT6ukkVF3fQepXv5SQYMNWmIP6ilh68WuinM6jHMYBPkyPMobGao5s6bv3mHpdF19aZ1QgYlbx8ljY4F4gPIQFYgkvqJGrr+eun6I5ralv58OP27t5QQOPsXiOyzRFGazE6WzSh7wtnIiA75uISdOEtdfQDLWmP+ogAAAA==) Ocak 2017'de kullanılan taksi türlerini (sarı veya yeşil) gösteren *taxiRides* dış tablosunda çalıştırın. 

```kusto
external_table("TaxiRides")
| where pickup_datetime between (datetime(2017-01-01) .. datetime(2017-02-01))
| summarize count() by cab_type
| render piechart
```

Bu sorgu, sorgu süresini ve performansını en iyi duruma getiren bölümleme kullanır. Sorgu, bölümlenmiş bir sütuna (pickup_datetime) filtre lenir ve sonuçları birkaç saniye içinde döndürür.

![bölümlü sorgu oluşturma](media/data-lake-query-data/taxirides-with-partition.png)
  
Harici tablo *TaxiRides* üzerinde çalıştırmak ve veriler hakkında daha fazla bilgi edinmek için ek sorgular yazabilirsiniz. 

## <a name="optimize-your-query-performance"></a>Sorgu performansınızı optimize edin

Dış verileri sorgulamak için aşağıdaki en iyi uygulamaları kullanarak sorgu performansınızı gölde optimize edin. 
 
### <a name="data-format"></a>Veri biçimi
 
Aşağıdakilerden beri analitik sorgular için sütun biçimi kullanın:
* Yalnızca bir sorguyla ilgili sütunlar okunabilir. 
* Sütun kodlama teknikleri veri boyutunu önemli ölçüde azaltabilir.  
Azure Veri Gezgini Parke ve ORC sütun biçimlerini destekler. Parke formatı optimize uygulama nedeniyle önerilmektedir. 
 
### <a name="azure-region"></a>Azure bölgesi 
 
Dış verilerin Azure Veri Gezgini kümenizle aynı Azure bölgesinde bulunduğunu sapta. Bu, maliyet ve veri getirme süresini azaltır.
 
### <a name="file-size"></a>Dosya boyutu
 
En uygun dosya boyutu, dosya başına yüzlerce Mb 'dir (1 Gb'a kadar). Daha yavaş dosya numaralandırma işlemi ve sütun biçiminin sınırlı kullanımı gibi gereksiz ek yükü gerektiren birçok küçük dosyadan kaçının. Dosya sayısının Azure Veri Gezgini kümenizdeki CPU çekirdeği sayısından daha fazla olması gerektiğini unutmayın. 
 
### <a name="compression"></a>Sıkıştırma
 
Uzak depolamadan alınan veri miktarını azaltmak için sıkıştırmayı kullanın. Parke formatı için, sütun gruplarını ayrı ayrı sıkıştıran ve böylece ayrı ayrı okumanızı sağlayan dahili Parke sıkıştırma mekanizmasını kullanın. Sıkıştırma mekanizmasının kullanımını doğrulamak için, dosyaların aşağıdaki gibi<filename>adlandırıldığını denetleyin: "<filename>.parke" veya " .snappy.parke" yerine "<filename>.parke.gz"). 
 
### <a name="partitioning"></a>Bölümleme
 
Sorgunun alakasız yolları atlamasını sağlayan "klasör" bölümlerini kullanarak verilerinizi düzenleyin. Bölümleme planlarken, zaman damgası veya kiracı kimliği gibi sorgularınızdaki dosya boyutunu ve yaygın filtreleri göz önünde bulundurun.
 
### <a name="vm-size"></a>VM boyutu
 
Daha fazla çekirdek ve daha yüksek ağ iş girişine sahip VM SUS'ları seçin (bellek daha az önemlidir). Daha fazla bilgi için azure [veri gezgini kümeniz için doğru VM SKU'yu seçin'](manage-cluster-choose-sku.md)e bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Gezgini'ni kullanarak verilerinizi Azure Veri Gölü'nde sorgula. Sorgu [yazmayı](write-queries.md) öğrenin ve verilerinizden ek bilgiler elde edin.
