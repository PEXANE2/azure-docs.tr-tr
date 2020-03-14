---
title: 'SSMS: verileri bağlama ve sorgulama'
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
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79299303"
---
# <a name="quickstart-use-sql-server-management-studio-to-connect-and-query-an-azure-sql-database"></a>Hızlı Başlangıç: Bağlanmak ve bir Azure SQL veritabanı sorgulamak kullanım SQL Server Management Studio

Bu hızlı başlangıçta, SQL Server Management Studio (SSMS) kullanarak Azure SQL veritabanına bağlanıp bazı sorguları nasıl çalıştıracağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki öğeler gereklidir:

- [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms/).
- AdventureWorksLT örnek veritabanı. AdventureWorksLT veritabanının çalışan bir kopyasına ihtiyacınız varsa [Azure SQL veritabanı oluşturma](sql-database-single-database-get-started.md) hızlı başlangıcı ' nı tamamlayarak bir tane oluşturun.
    - Bu makaledeki betikler, AdventureWorksLT veritabanını kullanmak için yazılmıştır. Yönetilen bir örnek kullanıyorsanız, AdventureWorks veritabanını bir örnek veritabanına içeri aktarmanız veya bu makaledeki betikleri Wide World Importers veritabanını kullanacak şekilde değiştirmeniz gerekir.

Yalnızca bazı geçici sorguları SSMS yüklemeden çalıştırmak istiyorsanız, bkz. [hızlı başlangıç: Azure Portal sorgu düzenleyicisini kullanarak BIR SQL veritabanını sorgulama](sql-database-connect-query-portal.md).

## <a name="get-sql-server-connection-information"></a>SQL Server bağlantı bilgilerini al

Veritabanınıza bağlanmak için gereken bağlantı bilgilerini alın. Bu hızlı başlangıcı tamamlayabilmeniz için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Sorgulamak istediğiniz **SQL veritabanı** veya **SQL yönetilen örneği** 'ne gidin.

3. **Genel bakış** sayfasında, tam sunucu adını kopyalayın. Tek bir veritabanı için **sunucu adı** veya yönetilen örnek Için **ana bilgisayar** ' ın yanındaki tam sunucu adı ' nın yanında. Tam nitelikli ad şöyle görünür: *ServerName.Database.Windows.net*, gerçek sunucu adı olması dışında.

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

SSMS 'de Azure SQL veritabanı sunucunuza bağlanın.

> [!IMPORTANT]
> Azure SQL veritabanı sunucusu 1433 numaralı bağlantı noktasını dinler. Şirket güvenlik duvarının arkasındaki bir SQL veritabanı sunucusuna bağlanmak için, güvenlik duvarında bu bağlantı noktasının açık olması gerekir.

1. SSMS’i açın.

2. **Sunucuya Bağlan** iletişim kutusu görüntülenir. Aşağıdaki bilgileri girin:

   | Ayar      | Önerilen değer    | Açıklama |
   | ------------ | ------------------ | ----------- |
   | **Sunucu türü** | Veritabanı altyapısı | Gerekli değer. |
   | **Sunucu adı** | Tam sunucu adı | Şöyle bir şey: **ServerName.Database.Windows.net**. |
   | **Kimlik doğrulaması** | SQL Server Kimlik Doğrulaması | Bu öğreticide, SQL kimlik doğrulaması kullanır. |
   | **Oturum açma** | Sunucu yönetici hesabının kullanıcı kimliği | Sunucu oluşturmak için kullanılan sunucu yönetici hesabına ait kullanıcı kimliği. |
   | **Parola** | Sunucu yönetici hesabı parolası | Sunucu oluşturmak için kullanılan sunucu yönetici hesabı parolası. |
   ||||

   ![sunucuya bağlan](./media/sql-database-connect-query-ssms/connect.png)  

3. **Sunucuya Bağlan** Iletişim kutusunda **seçenekleri** belirleyin. **Veritabanına Bağlan** açılan menüsünde **mysampledatabase**' i seçin. [Önkoşullar bölümünde](#prerequisites) hızlı başlangıcı tamamlamak, mysampledatabase adlı bir AdventureWorksLT veritabanı oluşturur. AdventureWorks veritabanının çalışma kopyanız mySampleDatabase öğesinden farklı bir ada sahipse, bunun yerine bunu seçin.

   ![sunucuda veritabanına bağlanma](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. **Bağlan**’ı seçin. Nesne Gezgini penceresi açılır.

5. Veritabanının nesnelerini görüntülemek için **veritabanları** ' nı genişletin ve ardından veritabanı düğümünüz ' ı genişletin.

   ![mySampleDatabase nesneleri](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="query-data"></a>Verileri sorgulama

Kategoriye göre ilk 20 ürünü sorgulamak için bu [Select](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL kodunu çalıştırın.

1. Nesne Gezgini, **Mysampledatabase** ' e sağ tıklayın ve **Yeni sorgu**' yu seçin. Veritabanınıza bağlı yeni bir sorgu penceresi açılır.

2. Sorgu penceresinde, aşağıdaki SQL sorgusunu yapıştırın:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Araç çubuğunda, sorguyu çalıştırmak ve `Product` ve `ProductCategory` tablolarından veri almak için **Yürüt** ' ü seçin.

    ![Tablo ürününden ve ProductCategory 'tan veri alma sorgusu](./media/sql-database-connect-query-ssms/query2.png)

### <a name="insert-data"></a>Veri ekleme

`SalesLT.Product` tablosunda yeni bir ürün oluşturmak için bu [Insert](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL kodunu çalıştırın.

1. Önceki sorguyu Bununla değiştirin.

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

2. `Product` tablosuna yeni bir satır eklemek için **Yürüt** ' ü seçin. **İletiler** bölmesi **(1 satır etkilendi)** görüntülenir.

#### <a name="view-the-result"></a>Görünüm sonucu

1. Önceki sorguyu Bununla değiştirin.

   ```sql
   SELECT * FROM [SalesLT].[Product]
   WHERE Name='myNewProduct'
   ```

2. **Yürüt**’ü seçin. Aşağıdaki sonuç görüntülenir.

   ![Ürün tablosu sorgusunun sonucu](./media/sql-database-connect-query-ssms/result.png)

### <a name="update-data"></a>Verileri güncelleştirme

Yeni ürününüzü değiştirmek için bu [güncelleştirme](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL kodunu çalıştırın.

1. Önceki sorguyu, daha önce oluşturulan yeni kaydı döndüren bir ile değiştirin:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. `Product` tablosundaki belirtilen satırı güncelleştirmek için **Yürüt** ' ü seçin. **İletiler** bölmesi **(1 satır etkilendi)** görüntülenir.

### <a name="delete-data"></a>Verileri silme

Yeni ürününüzü kaldırmak için bu [Delete](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL kodunu çalıştırın.

1. Önceki sorguyu Bununla değiştirin.

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. `Product` tablosundaki belirtilen satırı silmek için **Yürüt** ' ü seçin. **İletiler** bölmesi **(1 satır etkilendi)** görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

- SSMS hakkında daha fazla bilgi için bkz. [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).
- Azure portalını kullanarak bağlanmak ve sorgulamak için bkz. [Azure Portal SQL Sorgusu düzenleyicisiyle bağlanma ve sorgulama](sql-database-connect-query-portal.md).
- Visual Studio Code’u kullanarak bağlanmak ve sorgulamak için bkz. [Visual Studio Code ile bağlanma ve sorgulama](sql-database-connect-query-vscode.md).
- .NET kullanarak bağlanıp sorgulamak için bkz. [.NET ile bağlanma ve sorgulama](sql-database-connect-query-dotnet.md).
- PHP kullanarak bağlanıp sorgulamak için bkz. [PHP ile bağlanma ve sorgulama](sql-database-connect-query-php.md).
- Node.js kullanarak bağlanıp sorgulamak için bkz. [Node.js ile bağlanma ve sorgulama](sql-database-connect-query-nodejs.md).
- Java kullanarak bağlanıp sorgulamak için bkz. [Java ile bağlanma ve sorgulama](sql-database-connect-query-java.md).
- Python kullanarak bağlanıp sorgulamak için bkz. [Python ile bağlanma ve sorgulama](sql-database-connect-query-python.md).
- Ruby kullanarak bağlanıp sorgulamak için bkz. [Ruby ile bağlanma ve sorgulama](sql-database-connect-query-ruby.md).
