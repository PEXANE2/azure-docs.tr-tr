---
title: 'SSMS: Veri bağlama ve sorgulama'
description: SQL Server Management Studio (SSMS) kullanarak Azure'da SQL Database'e nasıl bağlanılacağını öğrenin. Ardından, verileri sorgulamak ve düzenlemek için Transact-SQL (T-SQL) deyimleri çalıştırın.
keywords: sql veritabanına bağlanma,sql server management studio
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/10/2020
ms.openlocfilehash: 31bd47128a272e75d7021180b536fe6bf7420f55
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79299303"
---
# <a name="quickstart-use-sql-server-management-studio-to-connect-and-query-an-azure-sql-database"></a>Quickstart: Azure SQL veritabanını bağlamak ve sorgulamak için SQL Server Management Studio'u kullanın

Bu hızlı başlangıçta, Bir Azure SQL veritabanına bağlanmak ve bazı sorguları çalıştırmak için SQL Server Management Studio'nun (SSMS) nasıl kullanılacağını öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlatmayı tamamlamak için aşağıdaki öğeler yer almaktadır:

- [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms/).
- AdventureWorksLT örnek veritabanı. AdventureWorksLT veritabanının çalışan bir kopyasına ihtiyacınız varsa, [Azure SQL veritabanı nı hızlı](sql-database-single-database-get-started.md) başlat'ı oluştur'u tamamlayarak bir tane oluşturun.
    - Bu makaledeki komut dosyaları AdventureWorksLT veritabanını kullanmak için yazılmıştır. Yönetilen bir örnek kullanıyorsanız, AdventureWorks veritabanını bir örnek veritabanına aktarmanız veya Geniş Dünya İthalatçılar veritabanını kullanmak için bu makaledeki komut dosyalarını değiştirmeniz gerekir.

SSMS yüklemeden bazı geçici sorgular çalıştırmak istiyorsanız, [bkz.](sql-database-connect-query-portal.md)

## <a name="get-sql-server-connection-information"></a>SQL sunucu bağlantı bilgilerini alın

Veritabanınıza bağlanmak için gereken bağlantı bilgilerini alın. Bu hızlı başlatmayı tamamlamak için tam nitelikli sunucu adı veya ana bilgisayar adı, veritabanı adı ve giriş bilgilerine ihtiyacınız vardır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. Sorgulamak istediğiniz **SQL veritabanına** veya **SQL yönetilen örneğine** gidin.

3. Genel **Bakış** sayfasında, tam nitelikli sunucu adını kopyalayın. Tek bir veritabanı için **Sunucu adının** yanında veya yönetilen bir örnek için **Host'un** yanında tam nitelikli sunucu adı vardır. Tam nitelikli ad gibi görünüyor: *servername.database.windows.net*, gerçek sunucu adı dışında.

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

SSMS'de Azure SQL Veritabanı sunucunuza bağlanın.

> [!IMPORTANT]
> Bir Azure SQL Veritabanı sunucusu 1433 bağlantı noktasında dinler. Kurumsal bir güvenlik duvarının arkasından bir SQL Veritabanı sunucusuna bağlanmak için güvenlik duvarının bu bağlantı noktasının açık olması gerekir.

1. SSMS’i açın.

2. **Sunucuya Bağlan** iletişim kutusu görüntülenir. Aşağıdaki bilgileri girin:

   | Ayar      | Önerilen değer    | Açıklama |
   | ------------ | ------------------ | ----------- |
   | **Sunucu türü** | Veritabanı altyapısı | Gerekli değer. |
   | **Sunucu adı** | Tam sunucu adı | Gibi bir şey: **servername.database.windows.net**. |
   | **Kimlik doğrulaması** | SQL Server Kimlik Doğrulaması | Bu öğretici, SQL Kimlik Doğrulama'yı kullanır. |
   | **Oturum açma** | Sunucu admin hesabı kullanıcı kimliği | Sunucu yönetici hesabından kullanıcı kimliği sunucu oluşturmak için kullanılır. |
   | **Parola** | Sunucu admin hesap parolası | Sunucu yönetici hesabından parola sunucu oluşturmak için kullanılır. |
   ||||

   ![sunucuya bağlan](./media/sql-database-connect-query-ssms/connect.png)  

3. **Sunucuya Bağlan** iletişim kutusunda **Seçenekler'i** seçin. Veritabanı açılır **menüsüne bağlan** menüsünde **mySampleDatabase'i**seçin. [Önkoşullar bölümünde](#prerequisites) hızlı bir şekilde başlatış tamamlamak, mySampleDatabase adında bir AdventureWorksLT veritabanı oluşturur. AdventureWorks veritabanının çalışan kopyası mySampleDatabase'den farklı bir ada sahipse, bunun yerine seçin.

   ![sunucuda veritabanına bağlanma](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. **Bağlan**’ı seçin. Nesne Gezgini penceresi açılır.

5. Veritabanının nesnelerini görüntülemek için **Veritabanlarını** genişletin ve veritabanı düğümünüzi genişletin.

   ![mySampleDatabase nesneleri](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="query-data"></a>Verileri sorgulama

Kategoriye göre en iyi 20 ürünü sorgulamak için bu [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL kodunu çalıştırın.

1. Object Explorer'da **mySampleDatabase'e** sağ tıklayın ve **Yeni Sorgu'yı**seçin. Veritabanınıza bağlı yeni bir sorgu penceresi açılır.

2. Sorgu penceresinde, aşağıdaki SQL sorgusunu yapıştırın:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Araç çubuğunda, sorguyu çalıştırmak ve `Product` `ProductCategory` tablolardan veri almak için Yürüt'''ün'u seçin. **Execute**

    ![tablo Ürün ve ÜrünKategorisi'nden veri almak için sorgu](./media/sql-database-connect-query-ssms/query2.png)

### <a name="insert-data"></a>Veri ekleme

Tabloda yeni bir ürün oluşturmak için bu [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL kodunu çalıştırın. `SalesLT.Product`

1. Önceki sorguyu bununla değiştirin.

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate] )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. Tabloya yeni bir satır eklemek `Product` için Yürüt'ün'u seçin. **Execute** **İletiler** bölmesi görüntüler **(1 satır etkilenen)**.

#### <a name="view-the-result"></a>Sonucu görüntüleme

1. Önceki sorguyu bununla değiştirin.

   ```sql
   SELECT * FROM [SalesLT].[Product]
   WHERE Name='myNewProduct'
   ```

2. **Yürüt**’ü seçin. Aşağıdaki sonuç görüntülenir.

   ![Ürün tablosu sorgusunun sonucu](./media/sql-database-connect-query-ssms/result.png)

### <a name="update-data"></a>Verileri güncelleştirme

Yeni ürününüzü değiştirmek için bu [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL kodunu çalıştırın.

1. Önceki sorguyu, daha önce oluşturulan yeni kaydı döndüren bu sorguyla değiştirin:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Tabloda belirtilen satırı güncelleştirmek `Product` için Yürüt'''ün'u seçin. **Execute** **İletiler** bölmesi görüntüler **(1 satır etkilenen)**.

### <a name="delete-data"></a>Verileri silme

Yeni ürününüzü kaldırmak için bu [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL kodunu çalıştırın.

1. Önceki sorguyu bununla değiştirin.

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Tabloda belirtilen satırı silmek `Product` için Yürüt''ün'u seçin. **Execute** **İletiler** bölmesi görüntüler **(1 satır etkilenen)**.

## <a name="next-steps"></a>Sonraki adımlar

- SSMS hakkında daha fazla bilgi için [SQL Server Management Studio'ya](https://msdn.microsoft.com/library/ms174173.aspx)bakın.
- Azure portalını kullanarak bağlanmak ve sorgulamak için [Azure portalı SQL Sorgu düzenleyicisiyle Bağlan ve sorgula' ya](sql-database-connect-query-portal.md)bakın.
- Visual Studio Code’u kullanarak bağlanmak ve sorgulamak için bkz. [Visual Studio Code ile bağlanma ve sorgulama](sql-database-connect-query-vscode.md).
- .NET kullanarak bağlanıp sorgulamak için bkz. [.NET ile bağlanma ve sorgulama](sql-database-connect-query-dotnet.md).
- PHP kullanarak bağlanıp sorgulamak için bkz. [PHP ile bağlanma ve sorgulama](sql-database-connect-query-php.md).
- Node.js kullanarak bağlanıp sorgulamak için bkz. [Node.js ile bağlanma ve sorgulama](sql-database-connect-query-nodejs.md).
- Java kullanarak bağlanıp sorgulamak için bkz. [Java ile bağlanma ve sorgulama](sql-database-connect-query-java.md).
- Python kullanarak bağlanıp sorgulamak için bkz. [Python ile bağlanma ve sorgulama](sql-database-connect-query-python.md).
- Ruby kullanarak bağlanıp sorgulamak için bkz. [Ruby ile bağlanma ve sorgulama](sql-database-connect-query-ruby.md).
