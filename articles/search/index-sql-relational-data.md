---
title: Alma ve dizin oluşturma için SQL ilişkisel verileri modelle
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da dizin oluşturma ve tam metin arama için ilişkisel verileri nasıl modelleştirip normalden kaldırıldıktan sonra düz bir sonuç kümesine dönüştüreceklerini öğrenin.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 3b973dd05d23d190c77986ca9bf6d39656739cd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72790089"
---
# <a name="how-to-model-relational-sql-data-for-import-and-indexing-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da içe aktarma ve dizine alma için ilişkisel SQL verileri nasıl modelalınır?

Azure Bilişsel Arama, [dizin oluşturma ardışık dizinine](search-what-is-an-index.md)giriş olarak düz bir satır kümesini kabul eder. Kaynak verileriniz BIRSQL Server ilişkisel veritabanındaki birleştirilmiş tablolardan kaynaklanıyorsa, bu makalede sonuç kümesinin nasıl oluşturulabildiğini ve Azure Bilişsel Arama dizininde üst-alt ilişkisinin nasıl modellenileceğini açıklar.

Bir illüstrasyon olarak, [demo verilere](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/hotels)dayalı varsayımsal oteller veritabanına başvuracağız. Veritabanının 50 otelli Hotels$ masası ve toplam 750 oda için farklı tip, tarife ve olanaklara sahip odalara sahip odalardan oluştuğunu varsayalım. Tablolar arasında bir-çok ilişkisi vardır. Yaklaşımımızda, bir görünüm, her satıra gömülü ilişkili oda ayrıntısı ile otel başına bir satır olmak üzere 50 satır döndüren sorguyu sağlar.

   ![Oteller veritabanında tablolar ve görünüm](media/index-sql-relational-data/hotels-database-tables-view.png "Oteller veritabanında tablolar ve görünüm")


## <a name="the-problem-of-denormalized-data"></a>Normalize edilmiş veri sorunu

Bir-çok ilişkisiyle çalışmadaki zorluklardan biri, birleştirilmiş tablolar da dahil olmak üzere standart sorguların normalden arındırılmış verileri döndürmesidir ve bu da Azure Bilişsel Arama senaryosunda iyi çalışmaz. Otellere ve odalara katılan aşağıdaki örneği göz önünde bulundurun.

```sql
SELECT * FROM Hotels$
INNER JOIN Rooms$
ON Rooms$.HotelID = Hotels$.HotelID
```
Bu sorgunun sonuçları, her oda değeri için ön otel bilgilerinin yinelenmesiyle birlikte tüm Otel alanlarını ve ardından tüm Oda alanlarını döndürür.

   ![Normalize edilmiş veriler, oda alanları eklendiğinde gereksiz otel verileri](media/index-sql-relational-data/denormalize-data-query.png "Normalize edilmiş veriler, oda alanları eklendiğinde gereksiz otel verileri")


Bu sorgu yüzeyde başarılı olsa da (tüm verileri düz bir satır kümesinde sağlar), beklenen arama deneyimi için doğru belge yapısını teslim etmede başarısız olur. Dizin oluşturma sırasında Azure Bilişsel Arama, yutulan her satır için bir arama belgesi oluşturur. Eğer arama belgeleriniz yukarıdaki sonuçlara benzeseydi, sadece Twin Dome oteli için yedi ayrı belge olan kopyaları algılardınız. "Florida'daki oteller" ile ilgili bir sorgu, sadece Twin Dome oteli için yedi sonuç döndürerek diğer ilgili otelleri arama sonuçlarının derinliklerine itecekti.

Otel başına beklenen bir belge deneyimini elde etmek için, doğru parçalı, ancak tam bilgi ile bir satır seti sağlamalısınız. Neyse ki, bu makalede teknikleri benimseyerek kolayca yapabilirsiniz.

## <a name="define-a-query-that-returns-embedded-json"></a>Katıştılmış JSON döndüren bir sorgu tanımlama

Beklenen arama deneyimini sunmak için veri kümenizin Azure Bilişsel Arama'daki her arama belgesi için bir satırdan oluşması gerekir. Örneğimizde, her otel için bir satır istiyoruz, ancak kullanıcılarımızın gece fiyatı, yatak sayısı veya plajın manzarası gibi önemsedikleri diğer oda yla ilgili alanlarda da arama yapabilmelerini istiyoruz.

Çözüm, iç içe json olarak oda ayrıntısını yakalamak ve ikinci adımda gösterildiği gibi JSON yapısını bir görünümde bir alana eklemektir. 

1. 50 otel ve 750 odanın ayrıntılarını içeren ve HotelID alanında birleştirilmiş iki birleştirilmiş masanız olduğunu varsayalım, Oteller$ ve Odalar$. Bu masalarda 50 otel ve 750 ilgili oda bulunmaktadır.

    ```sql
    CREATE TABLE [dbo].[Hotels$](
      [HotelID] [nchar](10) NOT NULL,
      [HotelName] [nvarchar](255) NULL,
      [Description] [nvarchar](max) NULL,
      [Description_fr] [nvarchar](max) NULL,
      [Category] [nvarchar](255) NULL,
      [Tags] [nvarchar](255) NULL,
      [ParkingIncluded] [float] NULL,
      [SmokingAllowed] [float] NULL,
      [LastRenovationDate] [smalldatetime] NULL,
      [Rating] [float] NULL,
      [StreetAddress] [nvarchar](255) NULL,
      [City] [nvarchar](255) NULL,
      [State] [nvarchar](255) NULL,
      [ZipCode] [nvarchar](255) NULL,
      [GeoCoordinates] [nvarchar](255) NULL
    ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
    GO

    CREATE TABLE [dbo].[Rooms$](
      [HotelID] [nchar](10) NULL,
      [Description] [nvarchar](255) NULL,
      [Description_fr] [nvarchar](255) NULL,
      [Type] [nvarchar](255) NULL,
      [BaseRate] [float] NULL,
      [BedOptions] [nvarchar](255) NULL,
      [SleepsCount] [float] NULL,
      [SmokingAllowed] [float] NULL,
      [Tags] [nvarchar](255) NULL
    ) ON [PRIMARY]
    GO
    ```

2. İç içe sorgu çıktısını içeren yeni`SELECT * from dbo.Hotels$`bir *Odalar* alanının eklenmesiyle ana tablodaki tüm alanlardan oluşan bir görünüm oluşturun. JSON olarak çıktı `SELECT * from dbo.Rooms$` yapıları üzerinde **Bir For JSON AUTO** maddesi. 

     ```sql
   CREATE VIEW [dbo].[HotelRooms]
   AS
   SELECT *, (SELECT *
              FROM dbo.Rooms$
              WHERE dbo.Rooms$.HotelID = dbo.Hotels$.HotelID FOR JSON AUTO) AS Rooms
   FROM dbo.Hotels$
   GO
   ```

   Aşağıdaki ekran görüntüsü, alttaki *Odalar* nvarchar alanı ile ortaya çıkan görünümü gösterir. *Odalar* alanı sadece HotelRooms görünümünde mevcuttur.

   ![HotelRooms görünümü](media/index-sql-relational-data/hotelsrooms-view.png "HoteRooms görünümü")

1. Satır `SELECT * FROM dbo.HotelRooms` kümesini almak için çalıştırın. Bu sorgu, otel başına bir tane olmak üzere 50 satır döndürür ve json koleksiyonu olarak ilişkili oda bilgileri yle birlikte. 

   ![HotelRooms görünümünden Rowset](media/index-sql-relational-data/hotelrooms-rowset.png "HotelRooms görünümünden Rowset")

Bu satır kümesi artık Azure Bilişsel Arama'ya aktarılmaya hazır.

> [!NOTE]
> Bu yaklaşım, gömülü [JSON'Un SQL Server'ın maksimum sütun boyutu sınırları](https://docs.microsoft.com/sql/sql-server/maximum-capacity-specifications-for-sql-server)altında olduğunu varsayar. Verileriniz uymuyorsa, [Örnek: Azure Bilişsel Arama için AdventureWorks Envanter veritabanını modelleyin'](search-example-adventureworks-modeling.md)de gösterildiği gibi programlı bir yaklaşım deneyebilirsiniz.

 ## <a name="use-a-complex-collection-for-the-many-side-of-a-one-to-many-relationship"></a>Bir-çok ilişkisinin "çok" tarafı için karmaşık bir koleksiyon kullanma

Azure Bilişsel Arama tarafında, iç içe json kullanarak bir-çok ilişkisi modeller bir dizin şeması oluşturun. Önceki bölümde oluşturduğunuz sonuç kümesi genellikle aşağıda sağlanan indeks şema sına karşılık gelir (kısalık için bazı alanları keseriz).

Aşağıdaki örnek, karmaşık veri [türlerinin nasıl modellenir'deki](search-howto-complex-data-types.md#creating-complex-fields)örneğe benzer. Bu makalenin odak noktası olan *Odalar* yapısı, *oteller*adlı bir dizinin alan koleksiyonunda yer almaktadır. Bu örnek, bir koleksiyonda izin verilen birden çok rasgele öğe sayısının aksine, sabit bir öğe kümesinden oluşması nedeniyle *Odalar'dan* farklı olan *Adres*için karmaşık bir türü de gösterir.

```json
{
  "name": "hotels",
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
    { "name": "Description_fr", "type": "Edm.String", "searchable": true, "analyzer": "fr.lucene" },
    { "name": "Category", "type": "Edm.String", "searchable": true, "filterable": false },
    { "name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "facetable": true },
    { "name": "Address", "type": "Edm.ComplexType",
      "fields": [
        { "name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
        { "name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
        { "name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true }
      ]
    },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)",
      "fields": [
        { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
        { "name": "Description_fr", "type": "Edm.String", "searchable": true, "analyzer": "fr.lucene" },
        { "name": "Type", "type": "Edm.String", "searchable": true },
        { "name": "BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true },
        { "name": "BedOptions", "type": "Edm.String", "searchable": true, "filterable": true, "facetable": true },
        { "name": "SleepsCount", "type": "Edm.Int32", "filterable": true, "facetable": true },
        { "name": "SmokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true },
        { "name": "Tags", "type": "Edm.Collection", "searchable": true }
      ]
    }
  ]
}
```

Önceki sonuç kümesi ve yukarıdaki dizin şeması göz önüne alındığında, başarılı bir dizin oluşturma işlemi için gerekli tüm bileşenlere sahipsiniz. Düzleştirilmiş veri kümesi dizin oluşturma gereksinimlerini karşılar, ancak ayrıntı bilgilerini korur. Azure Bilişsel Arama dizininde, arama sonuçları tek tek odaların bağlamını ve özelliklerini korurken kolayca otel tabanlı varlıklara düşer.

## <a name="next-steps"></a>Sonraki adımlar

Kendi veri kümenizi kullanarak, dizin oluşturmak ve yüklemek için [Veri Aktar](search-import-data-portal.md) sihirbazını kullanabilirsiniz. Sihirbaz, *Odalar'da*bulunan gibi gömülü JSON koleksiyonunu algılar ve karmaşık bir tür koleksiyonu içeren bir dizin şeması çıkartır. 

  ![İçe Aktarma veri siması tarafından çıkarılan dizin](media/index-sql-relational-data/search-index-rooms-complex-collection.png "İçe Aktarma veri siması tarafından çıkarılan dizin")

İçe Aktarma verisihirbazının temel adımlarını öğrenmek için aşağıdaki hızlı başlat'ı deneyin.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure portalLarını kullanarak arama dizini oluşturma](search-get-started-portal.md)