---
title: Veritabanını sorgulamak için Düğüm.js'yi kullanma
description: Bir Azure SQL veritabanına bağlanan bir program oluşturmak ve T-SQL deyimlerini kullanarak sorgulamak için Node.js nasıl kullanılır?
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: nodejs
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
ms.date: 03/25/2019
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: c0da38a41bf613237ea3b164d70e4729a7284ca7
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76768596"
---
# <a name="quickstart-use-nodejs-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Node.js kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlatmada, Bir Azure SQL veritabanına bağlanmak için Node.js'i ve verileri sorgulamak için T-SQL deyimlerini kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Azure SQL veritabanı](sql-database-single-database-get-started.md)
- [Node.js](https://nodejs.org)ile ilgili yazılım

  # <a name="macos"></a>[Macos](#tab/macos)

  Homebrew ve Node.js'i yükleyin, ardından [macOS'ta SQL Server'ı kullanarak Düğüm.js uygulamaları oluşturma'da](https://www.microsoft.com/sql-server/developer-get-started/node/mac/) **1.2** ve **1.3** adımlarını kullanarak ODBC sürücüsünü ve SQLCMD'yi yükleyin.

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Node.js yükleyin, sonra [Ubuntu'da SQL Server'ı kullanarak Düğüm.js uygulamaları oluştur'da](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/) **1.2 ve 1.3** adımlarını kullanarak ODBC sürücüsünü ve SQLCMD'yi yükleyin. **1.3**

  # <a name="windows"></a>[Windows](#tab/windows)

  Chocolatey ve Node.js'i yükleyin, ardından [Windows'da SQL Server'ı kullanarak Düğüm.js uygulamaları oluşturun'da](https://www.microsoft.com/sql-server/developer-get-started/node/windows/) **1.2** ve **1.3** adımlarını kullanarak ODBC sürücüsünü ve SQLCMD'yi yükleyin.

  ---

> [!IMPORTANT]
> Bu makaledeki komut dosyaları Adventure **Works** veritabanını kullanmak için yazılmıştır.

> [!NOTE]
> İsteğe bağlı olarak Azure SQL yönetilen bir örneği kullanmayı seçebilirsiniz.
>
> Oluşturmak ve yapılandırmak için [Azure Portalını,](sql-database-managed-instance-get-started.md) [PowerShell'i](scripts/sql-database-create-configure-managed-instance-powershell.md)veya [CLI'yi,](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)ardından [yerinde](sql-database-managed-instance-configure-p2s.md) veya [VM](sql-database-managed-instance-configure-vm.md) bağlantısını kurun.
>
> Verileri yüklemek için, [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) [dosyasıyla BACPAC ile geri yükleme'ye](sql-database-import.md) bakın veya [Geniş Dünya İthalatçılar veritabanını geri yükleyin.](sql-database-managed-instance-get-started-restore.md)

## <a name="get-sql-server-connection-information"></a>SQL sunucu bağlantı bilgilerini alın

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Gelecek yordamlar için tam nitelikli sunucu adı veya ana bilgisayar adı, veritabanı adı ve giriş bilgilerine ihtiyacınız vardır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. **SQL veritabanlarına** veya **SQL yönetilen örnekler** sayfasına gidin.

3. Genel **Bakış** sayfasında, tek bir veritabanı için **Sunucu adının** yanındaki tam nitelikli sunucu adını veya yönetilen bir örnek için **Host'un** yanındaki tam nitelikli sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine tıklayın ve **Kopyasimgesini** seçin. 

## <a name="create-the-project"></a>Proje oluşturma

Komut istemini açın ve *sqltest* adlı bir klasör oluşturun. Oluşturduğunuz klasörü açın ve aşağıdaki komutu çalıştırın:

  ```bash
  npm init -y
  npm install tedious
  ```

## <a name="add-code-to-query-database"></a>Sorgu veritabanına kod ekleme

1. En sevdiğiniz metin düzenleyicisinde, *sqltest.js*adlı yeni bir dosya oluşturun.

1. İçeriğini aşağıdaki kodla değiştirin. Ardından sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

    ```js
    const { Connection, Request } = require("tedious");

    // Create connection to database
    const config = {
      authentication: {
        options: {
          userName: "username", // update me
          password: "password" // update me
        },
        type: "default"
      },
      server: "your_server.database.windows.net", // update me
      options: {
        database: "your_database", //update me
        encrypt: true
      }
    };

    const connection = new Connection(config);

    // Attempt to connect and execute queries if connection goes through
    connection.on("connect", err => {
      if (err) {
        console.error(err.message);
      } else {
        queryDatabase();
      }
    });

    function queryDatabase() {
      console.log("Reading rows from the Table...");

      // Read all rows from table
      const request = new Request(
        `SELECT TOP 20 pc.Name as CategoryName,
                       p.name as ProductName
         FROM [SalesLT].[ProductCategory] pc
         JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid`,
        (err, rowCount) => {
          if (err) {
            console.error(err.message);
          } else {
            console.log(`${rowCount} row(s) returned`);
          }
        }
      );

      request.on("row", columns => {
        columns.forEach(column => {
          console.log("%s\t%s", column.metadata.colName, column.value);
        });
      });
      
      connection.execSql(request);
    }
    ```

> [!NOTE]
> Kod örneği, Azure SQL için **AdventureWorksLT** örnek veritabanını kullanır.

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde, programı çalıştırın.

    ```bash
    node sqltest.js
    ```

1. En üstteki 20 satırın döndürüldünden doğrulayın ve uygulama penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Server için Microsoft Node.js Sürücüsü](/sql/connect/node-js/node-js-driver-for-sql-server)

- [.NET çekirdekli](sql-database-connect-query-dotnet-core.md)Windows/Linux/macOS'ta bağlanma ve sorgulama , [Visual Studio Kodu](sql-database-connect-query-vscode.md)veya [SSMS](sql-database-connect-query-ssms.md) (yalnızca Windows)

- [Komut satırını kullanarak Windows/Linux/macOS'ta .NET Core ile başlayın](/dotnet/core/tutorials/using-with-xplat-cli)

- İlk Azure SQL veritabanınızı [.NET](sql-database-design-first-database-csharp.md) veya [SSMS](sql-database-design-first-database.md) kullanarak tasarla
