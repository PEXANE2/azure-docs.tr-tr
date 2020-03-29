---
title: Veritabanı arası sorgularla başlayın
description: dikey bölümlenmiş veritabanları ile elastik veritabanı sorgusu nasıl kullanılır
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/25/2019
ms.openlocfilehash: af93035766eaf1afa12d124b8379ee55c5567260
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823788"
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Veritabanı arası sorgularla (dikey bölümleme) başlayın (önizleme)

Azure SQL Veritabanı için elastik veritabanı sorgusu (önizleme), tek bir bağlantı noktası kullanarak birden çok veritabanına yayılan T-SQL sorgularını çalıştırmanızı sağlar. Bu makale dikey [olarak bölümlenmiş veritabanları](sql-database-elastic-query-vertical-partitioning.md)için geçerlidir.  

Tamamlandığında, birden çok ilgili veritabanına yayılan sorguları gerçekleştirmek için Azure SQL Veritabanını nasıl yapılandıracağınızı ve kullanacağınızı öğreneceksiniz.

Esnek veritabanı sorgu özelliği hakkında daha fazla bilgi için Azure [SQL Veritabanı elastik veritabanı sorgusuna genel bakış](sql-database-elastic-query-overview.md)bilgisine bakın.

## <a name="prerequisites"></a>Ön koşullar

HERHANGI BIR Dış VERI KAYNAĞı izni değiştirin gereklidir. Bu izin ALTER DATABASE iznine dahildir. ALTER HERHANGI BIR Dış VERI Kaynağı izinleri temel veri kaynağıbaşvurmak için gereklidir.

## <a name="create-the-sample-databases"></a>Örnek veritabanlarını oluşturma

Başlangıç olarak, aynı veya farklı SQL Veritabanı sunucularında **müşteriler** ve **siparişler**olmak üzere iki veritabanı oluşturun.

**OrderInformation** tablosunu oluşturmak ve örnek verileri girdialmak için **Siparişler** veritabanında aşağıdaki sorguları yürütün.

    CREATE TABLE [dbo].[OrderInformation](
        [OrderID] [int] NOT NULL,
        [CustomerID] [int] NOT NULL
        )
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)

Şimdi, **Müşteri Bilgileri** tablosunu oluşturmak ve örnek verileri girdialmak için **Müşteriler** veritabanındaki sorguyu uygulayın.

    CREATE TABLE [dbo].[CustomerInformation](
        [CustomerID] [int] NOT NULL,
        [CustomerName] [varchar](50) NULL,
        [Company] [varchar](50) NULL
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC)
    )
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC')
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ')
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO')

## <a name="create-database-objects"></a>Veritabanı nesneleri oluşturma

### <a name="database-scoped-master-key-and-credentials"></a>Veritabanı kapsamı ana anahtar ve kimlik bilgileri

1. Visual Studio'da SQL Server Management Studio veya SQL Server Veri Araçları'nı açın.
2. Orders veritabanına bağlanın ve aşağıdaki T-SQL komutlarını çalıştırın:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master_key_password>';
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';  

    "Kullanıcı adı" ve "parola" Müşteri veritabanında oturum açmak için kullanılan kullanıcı adı ve parola olmalıdır.
    Elastik sorgularla Azure Etkin Dizini kullanılarak kimlik doğrulaması şu anda desteklenmez.

### <a name="external-data-sources"></a>Dış veri kaynakları

Harici bir veri kaynağı oluşturmak için Siparişler veritabanında aşağıdaki komutu uygulayın:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
        (TYPE = RDBMS,
        LOCATION = '<server_name>.database.windows.net',
        DATABASE_NAME = 'Customers',
        CREDENTIAL = ElasticDBQueryCred,
    ) ;

### <a name="external-tables"></a>Dış tablolar

Siparişler veritabanında Müşteri Bilgileri tablosunun tanımıyla eşleşen harici bir tablo oluşturun:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation]
    ( [CustomerID] [int] NOT NULL,
      [CustomerName] [varchar](50) NOT NULL,
      [Company] [varchar](50) NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc)

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Örnek elastik veritabanı T-SQL sorgusu yürütme

Harici veri kaynağınızı ve dış tablolarınızı tanımladıktan sonra, artık dış tablolarınızı sorgulamak için T-SQL'i kullanabilirsiniz. Bu sorguyu Siparişler veritabanında yürüt:

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company
    FROM OrderInformation
    INNER JOIN CustomerInformation
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID

## <a name="cost"></a>Maliyet

Şu anda, elastik veritabanı sorgu özelliği Azure SQL Veritabanınızın maliyetine dahildir.  

Fiyatlandırma bilgileri için [SQL Veritabanı Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/sql-database)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorguya genel bakış için [bkz.](sql-database-elastic-query-overview.md)
* Dikey olarak bölümlenmiş veriler için sözdizimi ve örnek sorgular için [bkz.](sql-database-elastic-query-vertical-partitioning.md)
* Yatay bölümleme (parçalama) öğreticisi [için](sql-database-elastic-query-getting-started.md)bkz.
* Yatay olarak bölümlenmiş veriler için sözdizimi ve örnek sorgular için bkz: [Yatay olarak bölümlenmiş verileri sorgula)](sql-database-elastic-query-horizontal-partitioning.md)
* Tek bir uzak Azure SQL Veritabanında Transact-SQL deyimini yürüten veya yatay bir bölümleme düzeninde parça olarak hizmet veren veritabanları kümesini yürüten [sp\_ \_execute remote'a](https://msdn.microsoft.com/library/mt703714) bakın.
