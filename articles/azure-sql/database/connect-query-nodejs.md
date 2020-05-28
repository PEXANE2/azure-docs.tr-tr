---
title: Bir veritabanını sorgulamak için Node. js kullanma
description: Node. js kullanarak Azure SQL veritabanı 'nda bir veritabanına bağlanan ve T-SQL deyimlerini kullanarak sorgulayan bir program oluşturma.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: nodejs
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
ms.date: 03/25/2019
ms.custom: seo-javascript-september2019, seo-javascript-october2019, sqldbrb=2 
ms.openlocfilehash: 5f53d6b3e8b477d7b93eb1063679126a9533ef03
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84054482"
---
# <a name="quickstart-use-nodejs-to-query-a-microsoft-azure-sql-database"></a>Hızlı başlangıç: Microsoft Azure SQL veritabanını sorgulamak için Node. js kullanma
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu hızlı başlangıçta, Node. js kullanarak Azure SQL veritabanına bağlanın ve T-SQL deyimlerini kullanarak verileri sorgulayın.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

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


- [Node. js](https://nodejs.org)ile ilgili yazılımlar

  # <a name="macos"></a>[macOS](#tab/macos)

  Homebrew ve Node. js ' yi yükleyip, [macOS üzerinde SQL Server kullanarak Node. js uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/node/mac/)bölümünde **1,2** ve **1,3** adımlarını kullanarak ODBC sürücüsünü ve sqlcmd 'yi yüklersiniz.

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Node. js ' yi yükledikten sonra, [Ubuntu üzerinde SQL Server kullanarak Node. js uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/)bölümünde **1,2** ve **1,3** adımlarını kullanarak ODBC sürücüsünü ve sqlcmd 'yi yüklersiniz.

  # <a name="windows"></a>[Windows](#tab/windows)

  Chocolatey ve Node. js ' yi yükleyip [Windows üzerinde SQL Server kullanarak Node. js uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/node/windows/)bölümünde **1,2** ve **1,3** adımlarını kullanarak ODBC sürücüsünü ve sqlcmd 'yi yüklersiniz.

  ---

> [!IMPORTANT]
> Bu makaledeki betikler, **Adventure Works** veritabanını kullanmak için yazılmıştır.

> [!NOTE]
> İsteğe bağlı olarak bir Azure SQL yönetilen örneği kullanmayı seçebilirsiniz.
>
> Oluşturmak ve yapılandırmak için [Azure Portal](../managed-instance/instance-create-quickstart.md), [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md)veya [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)kullanın, sonra [yerinde veya](../managed-instance/point-to-site-p2s-configure.md) [VM](../managed-instance/connect-vm-instance-configure.md) bağlantısı kurun.
>
> Verileri yüklemek için bkz. [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) dosyası Ile [bacpac ile geri yükleme](database-import.md) veya bkz. [Wide World Importers veritabanını geri yükleme](../managed-instance/restore-sample-database-quickstart.md).

## <a name="get-sql-server-connection-information"></a>SQL Server bağlantı bilgilerini al

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, bir Azure SQL veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya Azure SQL yönetilen örneği için **konak** ' ın YANıNDAKI tam sunucu adını (veya IP adresini) veya bir Azure VM 'de SQL Server ' yi inceleyin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

> [!NOTE]
> Azure VM 'de SQL Server bağlantı bilgileri için bkz. [Connect SQL Server](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server)

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

- [.NET Core](connect-query-dotnet-core.md), [Visual Studio Code](connect-query-vscode.md)veya [SSMS](connect-query-ssms.md) Ile Windows/Linux/MacOS 'a bağlanma ve sorgulama (yalnızca Windows)

- [Komut satırını kullanarak Windows/Linux/macOS 'ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli)

- [.Net](design-first-database-csharp-tutorial.md) veya [SSMS](design-first-database-tutorial.md) kullanarak ilk Azure SQL veritabanınızı tasarlama
