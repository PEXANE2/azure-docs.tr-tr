---
title: Bir veritabanını sorgulamak için Node. js kullanma
description: Node. js kullanarak Azure SQL veritabanına bağlanan ve T-SQL deyimlerini kullanarak sorgulayan bir program oluşturma.
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
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76768596"
---
# <a name="quickstart-use-nodejs-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Node.js kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlangıçta, Node. js kullanarak Azure SQL veritabanına bağlanın ve T-SQL deyimlerini kullanarak verileri sorgulayın.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure SQL veritabanı](sql-database-single-database-get-started.md)
- [Node. js](https://nodejs.org)ile ilgili yazılımlar

  # <a name="macostabmacos"></a>[macOS](#tab/macos)

  Homebrew ve Node. js ' yi yükleyip, [macOS üzerinde SQL Server kullanarak Node. js uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/node/mac/)bölümünde **1,2** ve **1,3** adımlarını kullanarak ODBC sürücüsünü ve sqlcmd 'yi yüklersiniz.

  # <a name="ubuntutabubuntu"></a>[Ubuntu](#tab/ubuntu)

  Node. js ' yi yükledikten sonra, [Ubuntu üzerinde SQL Server kullanarak Node. js uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/)bölümünde **1,2** ve **1,3** adımlarını kullanarak ODBC sürücüsünü ve sqlcmd 'yi yüklersiniz.

  # <a name="windowstabwindows"></a>[Windows](#tab/windows)

  Chocolatey ve Node. js ' yi yükleyip [Windows üzerinde SQL Server kullanarak Node. js uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/node/windows/)bölümünde **1,2** ve **1,3** adımlarını kullanarak ODBC sürücüsünü ve sqlcmd 'yi yüklersiniz.

  ---

> [!IMPORTANT]
> Bu makaledeki betikler, **Adventure Works** veritabanını kullanmak için yazılmıştır.

> [!NOTE]
> İsteğe bağlı olarak bir Azure SQL yönetilen örneği kullanmayı seçebilirsiniz.
>
> Oluşturmak ve yapılandırmak için [Azure portalını](sql-database-managed-instance-get-started.md), [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md)'i veya [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)'Yi kullanın, sonra [yerinde veya](sql-database-managed-instance-configure-p2s.md) [VM](sql-database-managed-instance-configure-vm.md) bağlantısı kurun.
>
> Verileri yüklemek için bkz. [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) dosyası Ile [bacpac ile geri yükleme](sql-database-import.md) veya bkz. [Wide World Importers veritabanını geri yükleme](sql-database-managed-instance-get-started-restore.md).

## <a name="get-sql-server-connection-information"></a>SQL Server bağlantı bilgilerini al

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, tek bir veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya yönetilen örnek Için **ana bilgisayar ' ın** yanındaki tam sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin. 

## <a name="create-the-project"></a>Proje oluşturma

Komut istemini açın ve *sqltest* adlı bir klasör oluşturun. Oluşturduğunuz klasörü açın ve şu komutu çalıştırın:

  ```bash
  npm init -y
  npm install tedious
  ```

## <a name="add-code-to-query-database"></a>Sorgu veritabanına kod ekleme

1. En sevdiğiniz metin düzenleyicisinde *SQLtest. js*adlı yeni bir dosya oluşturun.

1. İçeriğini aşağıdaki kodla değiştirin. Daha sonra sunucunuz, veritabanınız, Kullanıcı ve parolanız için uygun değerleri ekleyin.

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

1. Komut isteminde programı çalıştırın.

    ```bash
    node sqltest.js
    ```

1. İlk 20 satırın döndürüldüğünden emin olun ve uygulama penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Server için Microsoft Node.js Sürücüsü](/sql/connect/node-js/node-js-driver-for-sql-server)

- [.NET Core](sql-database-connect-query-dotnet-core.md), [Visual Studio Code](sql-database-connect-query-vscode.md)veya [SSMS](sql-database-connect-query-ssms.md) Ile Windows/Linux/MacOS 'a bağlanma ve sorgulama (yalnızca Windows)

- [Komut satırını kullanarak Windows/Linux/macOS 'ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli)

- [.Net](sql-database-design-first-database-csharp.md) veya [SSMS](sql-database-design-first-database.md) kullanarak ilk Azure SQL veritabanınızı tasarlama
