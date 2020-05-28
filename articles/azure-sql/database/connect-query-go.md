---
title: Sorguya git 'i kullanma
description: Git ' i kullanarak Azure SQL veritabanı 'nda bir veritabanına bağlanan ve Transact-SQL deyimleriyle verileri sorgulama ve değiştirme gibi bir program oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2 
ms.devlang: go
ms.topic: quickstart
author: David-Engel
ms.author: craigg
ms.reviewer: MightyPen
ms.date: 02/12/2019
ms.openlocfilehash: 58d0cc61ae01e63e707d81e33770d6ea1e6ba491
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84054500"
---
# <a name="quickstart-use-golang-to-query-a-azure-sql-database"></a>Hızlı başlangıç: Azure SQL veritabanını sorgulamak için Golang kullanma
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu hızlı başlangıçta, bir Azure SQL veritabanına bağlanmak için [Golang](https://godoc.org/github.com/denisenkom/go-mssqldb) programlama dilini kullanacaksınız. Ardından, verileri sorgulamak ve değiştirmek için Transact-SQL deyimlerini çalıştıracaksınız. [Golang](https://golang.org/) basit, güvenilir ve verimli yazılım oluşturmayı kolaylaştıran açık kaynaklı bir programlama dilidir.  

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Bir Azure SQL veritabanı. Azure SQL 'de bir veritabanı oluşturmak ve yapılandırmak için bu hızlı başlangıçlardan birini kullanabilirsiniz:

  || SQL Veritabanı | SQL Yönetilen Örnek | Azure VM'de SQL Server |
  |:--- |:--- |:---|:---|
  | Oluştur| [Portal](single-database-create-quickstart.md) | [Portal](../managed-instance/instance-create-quickstart.md) | [Portal](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  || [CLI](scripts/create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/create-and-configure-database-powershell.md) | [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md) | [PowerShell](../virtual-machines/windows/sql-vm-create-powershell-quickstart.md)
  | Yapılandırma | [Sunucu düzeyi IP güvenlik duvarı kuralı](firewall-create-server-level-portal-quickstart.md)| [Bir VM 'den bağlantı](../managed-instance/connect-vm-instance-configure.md)|
  |||[Siteden bağlantı](../managed-instance/point-to-site-p2s-configure.md) | [SQL Server’a bağlanma](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  |Veri yükleme|Hızlı başlangıç başına yüklenen Adventure Works|[Geniş dünyada içeri aktarıcılar geri yükleme](../managed-instance/restore-sample-database-quickstart.md) | [Geniş dünyada içeri aktarıcılar geri yükleme](../managed-instance/restore-sample-database-quickstart.md) |
  |||[GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 'Dan [bacpac](database-import.md) dosyasından Adventure Works 'ü geri yükleme veya içe aktarma| [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 'Dan [bacpac](database-import.md) dosyasından Adventure Works 'ü geri yükleme veya içe aktarma|
  |||

  > [!IMPORTANT]
  > Bu makaledeki betikler, Adventure Works veritabanını kullanmak için yazılmıştır. SQL yönetilen örneği ile, Adventure Works veritabanını bir örnek veritabanına aktarmanız veya bu makaledeki betikleri Wide World Importers veritabanını kullanacak şekilde değiştirmeniz gerekir.

- Yüklü işletim sisteminiz için golang ve ilgili yazılımlar:

  - **MacOS**: homebrew ve Golang 'yi yükler. Bkz. [Adım 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/mac/).
  - **Ubuntu**: Golang 'yi yükler. Bkz. [Adım 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/ubuntu/).
  - **Windows**: Golang 'yi yükler. Bkz. [Adım 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/windows/).

## <a name="get-sql-server-connection-information"></a>SQL Server bağlantı bilgilerini al

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, bir Azure SQL veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya Azure SQL yönetilen örneği için **konak** ' ın YANıNDAKI tam sunucu adını (veya IP adresini) veya bir Azure VM 'de SQL Server ' yi inceleyin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

> [!NOTE]
> Azure VM 'de SQL Server bağlantı bilgileri için bkz. [Connect SQL Server](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server)

## <a name="create-golang-project-and-dependencies"></a>Golang projesi ve bağımlılıkları oluşturma

1. Terminalden, **SqlServerSample** adlı yeni bir proje klasörü oluşturun. 

   ```bash
   mkdir SqlServerSample
   ```

2. **Sqlserversample** ' a gidin ve Go için SQL Server sürücüsünü yükler.

   ```bash
   cd SqlServerSample
   go get github.com/denisenkom/go-mssqldb
   go install github.com/denisenkom/go-mssqldb
   ```

## <a name="create-sample-data"></a>Örnek veri oluşturma

1. Bir metin düzenleyicisinde, **Sqlserversample** klasöründe **createtestdata. SQL** adlı bir dosya oluşturun. Dosyasında, bir şema, tablo oluşturan ve birkaç satır ekleyen bu T-SQL kodunu yapıştırın.

   ```sql
   CREATE SCHEMA TestSchema;
   GO

   CREATE TABLE TestSchema.Employees (
     Id       INT IDENTITY(1,1) NOT NULL PRIMARY KEY,
     Name     NVARCHAR(50),
     Location NVARCHAR(50)
   );
   GO

   INSERT INTO TestSchema.Employees (Name, Location) VALUES
     (N'Jared',  N'Australia'),
     (N'Nikita', N'India'),
     (N'Tom',    N'Germany');
   GO

   SELECT * FROM TestSchema.Employees;
   GO
   ```

2. `sqlcmd`Veritabanına bağlanmak ve yeni oluşturulan SQL betiğini çalıştırmak için kullanın. Sunucunuz, veritabanınız, kullanıcı adınız ve parolanız için uygun değerleri değiştirin.

   ```bash
   sqlcmd -S <your_server>.database.windows.net -U <your_username> -P <your_password> -d <your_database> -i ./CreateTestData.sql
   ```

## <a name="insert-code-to-query-sql-database"></a>SQL veritabanını sorgulamak için kod girme

1. **SqlServerSample** klasöründe **sample.go** adlı bir dosya oluşturun.

2. Dosyasında bu kodu yapıştırın. Sunucunuz, veritabanınız, Kullanıcı adınız ve parolanız için değerleri ekleyin. Bu örnek, etkin bir bağlantı olduğundan emin olmak için Golang [bağlam yöntemlerini](https://golang.org/pkg/context/) kullanır.

   ```go
   package main

   import (
       _ "github.com/denisenkom/go-mssqldb"
       "database/sql"
       "context"
       "log"
       "fmt"
       "errors"
   )

   var db *sql.DB

   var server = "<your_server.database.windows.net>"
   var port = 1433
   var user = "<your_username>"
   var password = "<your_password>"
   var database = "<your_database>"

   func main() {
       // Build connection string
       connString := fmt.Sprintf("server=%s;user id=%s;password=%s;port=%d;database=%s;",
           server, user, password, port, database)

       var err error

       // Create connection pool
       db, err = sql.Open("sqlserver", connString)
       if err != nil {
           log.Fatal("Error creating connection pool: ", err.Error())
       }
       ctx := context.Background()
       err = db.PingContext(ctx)
       if err != nil {
           log.Fatal(err.Error())
       }
       fmt.Printf("Connected!\n")

       // Create employee
       createID, err := CreateEmployee("Jake", "United States")
       if err != nil {
           log.Fatal("Error creating Employee: ", err.Error())
       }
       fmt.Printf("Inserted ID: %d successfully.\n", createID)

       // Read employees
       count, err := ReadEmployees()
       if err != nil {
           log.Fatal("Error reading Employees: ", err.Error())
       }
       fmt.Printf("Read %d row(s) successfully.\n", count)

       // Update from database
       updatedRows, err := UpdateEmployee("Jake", "Poland")
       if err != nil {
           log.Fatal("Error updating Employee: ", err.Error())
       }
       fmt.Printf("Updated %d row(s) successfully.\n", updatedRows)

       // Delete from database
       deletedRows, err := DeleteEmployee("Jake")
       if err != nil {
           log.Fatal("Error deleting Employee: ", err.Error())
       }
       fmt.Printf("Deleted %d row(s) successfully.\n", deletedRows)
   }

   // CreateEmployee inserts an employee record
   func CreateEmployee(name string, location string) (int64, error) {
       ctx := context.Background()
       var err error

       if db == nil {
           err = errors.New("CreateEmployee: db is null")
           return -1, err
       }

       // Check if database is alive.
       err = db.PingContext(ctx)
       if err != nil {
           return -1, err
       }

       tsql := "INSERT INTO TestSchema.Employees (Name, Location) VALUES (@Name, @Location); select convert(bigint, SCOPE_IDENTITY());"

       stmt, err := db.Prepare(tsql)
       if err != nil {
          return -1, err
       }
       defer stmt.Close()

       row := stmt.QueryRowContext(
           ctx,
           sql.Named("Name", name),
           sql.Named("Location", location))
       var newID int64
       err = row.Scan(&newID)
       if err != nil {
           return -1, err
       }

       return newID, nil
   }

   // ReadEmployees reads all employee records
   func ReadEmployees() (int, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           return -1, err
       }

       tsql := fmt.Sprintf("SELECT Id, Name, Location FROM TestSchema.Employees;")

       // Execute query
       rows, err := db.QueryContext(ctx, tsql)
       if err != nil {
           return -1, err
       }

       defer rows.Close()

       var count int

       // Iterate through the result set.
       for rows.Next() {
           var name, location string
           var id int

           // Get values from row.
           err := rows.Scan(&id, &name, &location)
           if err != nil {
               return -1, err
           }

           fmt.Printf("ID: %d, Name: %s, Location: %s\n", id, name, location)
           count++
       }

       return count, nil
   }

   // UpdateEmployee updates an employee's information
   func UpdateEmployee(name string, location string) (int64, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           return -1, err
       }

       tsql := fmt.Sprintf("UPDATE TestSchema.Employees SET Location = @Location WHERE Name = @Name")

       // Execute non-query with named parameters
       result, err := db.ExecContext(
           ctx,
           tsql,
           sql.Named("Location", location),
           sql.Named("Name", name))
       if err != nil {
           return -1, err
       }

       return result.RowsAffected()
   }

   // DeleteEmployee deletes an employee from the database
   func DeleteEmployee(name string) (int64, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           return -1, err
       }

       tsql := fmt.Sprintf("DELETE FROM TestSchema.Employees WHERE Name = @Name;")

       // Execute non-query with named parameters
       result, err := db.ExecContext(ctx, tsql, sql.Named("Name", name))
       if err != nil {
           return -1, err
       }

       return result.RowsAffected()
   }
   ```

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde aşağıdaki komutu çalıştırın.

   ```bash
   go run sample.go
   ```

2. Çıktıyı doğrulayın.

   ```text
   Connected!
   Inserted ID: 4 successfully.
   ID: 1, Name: Jared, Location: Australia
   ID: 2, Name: Nikita, Location: India
   ID: 3, Name: Tom, Location: Germany
   ID: 4, Name: Jake, Location: United States
   Read 4 row(s) successfully.
   Updated 1 row(s) successfully.
   Deleted 1 row(s) successfully.
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](design-first-database-tutorial.md)
- [Microsoft SQL Server için golang sürücüsü](https://github.com/denisenkom/go-mssqldb)
- [Sorun bildirin veya soru sorun](https://github.com/denisenkom/go-mssqldb/issues)

