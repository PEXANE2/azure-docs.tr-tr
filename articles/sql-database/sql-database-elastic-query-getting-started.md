---
title: Ölçeklenmiş bulut veritabanları arasında rapor oluşturma
description: Birden çok veritabanı arasında rapor vermek için veritabanı arası veritabanları sorgularını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: sstein
ms.date: 10/10/2019
ms.openlocfilehash: bad52b364dc83994e7985fc80b1b9f9e7f50481e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823785"
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a>Ölçeklenmiş bulut veritabanları arasında rapor (önizleme)

[Elastik](sql-database-elastic-query-overview.md)bir sorgu kullanarak tek bir bağlantı noktasından birden çok Azure SQL veritabanından raporlar oluşturabilirsiniz. Veritabanları yatay olarak bölümlere ayrılmalıdır ("kırık" olarak da bilinir).

Varolan bir veritabanınız varsa, [varolan veritabanlarını ölçeklenmiş veritabanlarına geçirme'ye](sql-database-elastic-convert-to-use-elastic-tools.md)bakın.

Sorgulamak için gereken SQL nesnelerini anlamak [için, yatay olarak bölümlenmiş veritabanları arasında Sorgu'ya](sql-database-elastic-query-horizontal-partitioning.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

[Elastik Veritabanı araçları örneği ile başlarken](sql-database-elastic-scale-get-started.md)indirin ve çalıştırın.

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Örnek uygulamayı kullanarak bir parça harita yöneticisi oluşturma
Burada birkaç parça ile birlikte bir parça harita yöneticisi oluşturacak ve ardından parçalara veri eklenecektir. Zaten içinde kırık veri bulunan kırıklar kurulumu varsa, aşağıdaki adımları atlayabilir ve bir sonraki bölüme geçebilirsiniz.

1. Makale bölümündeki adımları izleyerek **Elastik Veritabanı araçları** örnek uygulamasını oluşturun [ve çalıştırın örnek uygulamayı indirin ve çalıştırın.](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app-1) Tüm adımları tamamladıktan sonra, aşağıdaki komut istemini görürsünüz:

    ![Komut][1]
2. Komut penceresinde "1" yazın ve **Enter**tuşuna basın. Bu, parça lı harita yöneticisini oluşturur ve sunucuya iki parça ekler. Sonra "3" yazın ve **Enter**tuşuna basın; eylemi dört kez tekrarlayın. Bu, parçalarınıza örnek veri satırları ekler.
3. [Azure portalı](https://portal.azure.com) sunucunuzda üç yeni veritabanı göstermelidir:

   ![Visual Studio onayı][2]

   Bu noktada, çapraz veritabanı sorguları Elastik Veritabanı istemci kitaplıkları aracılığıyla desteklenir. Örneğin, komut penceresinde seçenek 4'i kullanın. Çok parçalı bir sorgunun sonuçları her zaman tüm parçaların sonuçlarının bir **UNION ALL** vardır.

   Sonraki bölümde, verilerin kırıklar arasında daha zengin sorgulanmasını destekleyen örnek bir veritabanı bitiş noktası oluştururuz.

## <a name="create-an-elastic-query-database"></a>Esnek bir sorgu veritabanı oluşturma
1. Azure [portalını](https://portal.azure.com) açın ve oturum açın.
2. Parça kurulumunuzla aynı sunucuda yeni bir Azure SQL veritabanı oluşturun. Veritabanına "ElasticDBQuery" adını ver.

    ![Azure portalı ve fiyatlandırma katmanı][3]

    > [!NOTE]
    > varolan bir veritabanı kullanabilirsiniz. Bunu yapabilirseniz, sorgularınızı yürütmek istediğiniz parçalardan biri olmamalıdır. Bu veritabanı, elastik bir veritabanı sorgusu için meta veri nesneleri oluşturmak için kullanılır.
    >

## <a name="create-database-objects"></a>Veritabanı nesneleri oluşturma
### <a name="database-scoped-master-key-and-credentials"></a>Veritabanı kapsamı ana anahtar ve kimlik bilgileri
Bunlar, parça harita yöneticisine ve kırıklara bağlanmak için kullanılır:

1. Visual Studio'da SQL Server Management Studio veya SQL Server Veri Araçları'nı açın.
2. ElasticDBQuery veritabanına bağlanın ve aşağıdaki T-SQL komutlarını çalıştırın:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master_key_password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    "kullanıcı adı" ve "şifre" bölümü Download adım 3 kullanılan giriş bilgileri ile aynı olmalıdır ve **Elastik Veritabanı araçları** makale ile başlarken örnek uygulama [çalıştırın.](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app)

### <a name="external-data-sources"></a>Dış veri kaynakları
Harici bir veri kaynağı oluşturmak için ElasticDBQuery veritabanında aşağıdaki komutu uygulayın:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 "CustomerIDShardMap" elastik veritabanı araçları örnek kullanarak parça harita ve parça harita yöneticisi oluşturduysanız, parça harita adıdır. Ancak, bu örnek için özel kurulumunuzu kullandıysanız, uygulamanızda seçtiğiniz parça harita adı olmalıdır.

### <a name="external-tables"></a>Dış tablolar
ElasticDBQuery veritabanında aşağıdaki komutu çalıştırarak kırıklar üzerindeki Müşteriler tablosuyla eşleşen harici bir tablo oluşturun:

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Örnek elastik veritabanı T-SQL sorgusu yürütme
Harici veri kaynağınızı ve harici tablolarınızı tanımladıktan sonra artık dış tablolarınız üzerinden tam T-SQL kullanabilirsiniz.

Bu sorguyu ElasticDBQuery veritabanında yürüt:

    select count(CustomerId) from [dbo].[Customers]

Sorgunun tüm parçaparçadan sonuçları topladığını ve aşağıdaki çıktıyı verdiğini fark edeceksiniz:

![Çıktı ayrıntıları][4]

## <a name="import-elastic-database-query-results-to-excel"></a>Elastik veritabanı sorgu sonuçlarını Excel'e aktarma
 Bir sorgunun sonuçlarını bir Excel dosyasına aktarabilirsiniz.

1. Excel 2013'e başlatın.
2. **Veri** şeridine gidin.
3. **Diğer Kaynaklardan'ı** tıklatın ve **SQL Server'dan**tıklayın.

   ![Diğer kaynaklardan Excel alma][5]
4. Veri **Bağlantısı Sihirbazı'nda** sunucu adını ve giriş kimlik bilgilerini yazın. Ardından **İleri**’ye tıklayın.
5. İletişim kutusunda **Istediğiniz verileri içeren veritabanını seçin,** **ElasticDBQuery** veritabanını seçin.
6. Liste görünümünde **Müşteriler** tablosunu seçin ve **İleri'yi**tıklatın. Ardından, **Son**'a tıklayın.
7. Verileri **İçe Aktar** **formunda, çalışma kitabınızda bu verileri nasıl görüntülemek istediğinizi seçin**' in **altında, Tablo'yu** seçin ve **Tamam'ı**tıklatın.

Farklı kırıklarda depolanan **Müşteriler** tablosundaki tüm satırlar Excel sayfasını doldurur.

Artık Excel'in güçlü veri görselleştirme işlevlerini kullanabilirsiniz. BI ve veri tümleştirme araçlarınızı elastik sorgu veritabanına bağlamak için sunucu adınız, veritabanı adınız ve kimlik bilgilerinizle bağlantı dizesini kullanabilirsiniz. SQL Server'ın aracınız için veri kaynağı olarak desteklendirdiğinden emin olun. Aracınızla bağlanacağınız diğer SQL Server veritabanı ve SQL Server tabloları gibi elastik sorgu veritabanına ve harici tablolara başvurabilirsiniz.

### <a name="cost"></a>Maliyet
Elastik Veritabanı Sorgusu özelliğini kullanmak için ek ücret alınmaz.

Fiyatlandırma bilgileri için SQL [Veritabanı Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/sql-database/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorguya genel bakış için [bkz.](sql-database-elastic-query-overview.md)
* Dikey bölümleme öğreticisi için [bkz.](sql-database-elastic-query-getting-started-vertical.md)
* Dikey olarak bölümlenmiş veriler için sözdizimi ve örnek sorgular için [bkz.](sql-database-elastic-query-vertical-partitioning.md)
* Yatay olarak bölümlenmiş veriler için sözdizimi ve örnek sorgular için bkz: [Yatay olarak bölümlenmiş verileri sorgula)](sql-database-elastic-query-horizontal-partitioning.md)
* Tek bir uzak Azure SQL Veritabanında Transact-SQL deyimini yürüten veya yatay bir bölümleme düzeninde parça olarak hizmet veren veritabanları kümesini yürüten [sp\_ \_execute remote'a](https://msdn.microsoft.com/library/mt703714) bakın.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
