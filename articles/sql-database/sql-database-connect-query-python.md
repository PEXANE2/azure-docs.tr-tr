---
title: Veritabanını sorgulamak için Python'u kullanma
description: Bu konu, Bir Azure SQL veritabanına bağlanan bir program oluşturmak ve Transact-SQL deyimlerini kullanarak sorgulamak için Python'u nasıl kullanacağınızı gösterir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: seo-python-october2019
ms.devlang: python
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: e82f8feae0096202e48a58296dd2e9d21bb61885
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76768567"
---
# <a name="quickstart-use-python-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Python kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlatmada, Bir Azure SQL veritabanına bağlanmak için Python'u ve verileri sorgulamak için T-SQL deyimlerini kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Azure SQL veritabanı](sql-database-single-database-get-started.md)
- [Python](https://python.org/downloads) 3 ve ilgili yazılımlar

  # <a name="macos"></a>[Macos](#tab/macos)

  ODBC sürücüsü ve SQLCMD olan Homebrew ve Python'u ve SQL Server için Python sürücüsünü yüklemek için [macOS'ta SQL Server'ı kullanarak Python uygulamaları oluşturmak](https://www.microsoft.com/sql-server/developer-get-started/python/mac/)için **1.2**, **1.3**ve **2.1** adımlarını kullanın.

  Daha fazla bilgi için [macOS'taki Microsoft ODBC Driver'a](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server)bakın.

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Python ve diğer gerekli paketleri `sudo apt-get install python python-pip gcc g++ build-essential`yüklemek için.

  SQL Server için ODBC sürücüsünü, SQLCMD'yi ve Python sürücüsünü yüklemek [için pyodbc Python geliştirme için bir ortam yapılandırmak için](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux)bkz.

  Daha fazla bilgi için [Linux'taki Microsoft ODBC Driver'a](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server)bakın.

  # <a name="windows"></a>[Windows](#tab/windows)

  Python yüklemek için, ODBC sürücüsü ve SQLCMD ve SQL Server için Python sürücüsü, [pyodbc Python geliştirme için bir ortam yapılandırma](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows)bakın.

  Daha fazla bilgi için [Microsoft ODBC Driver'a](/sql/connect/odbc/microsoft-odbc-driver-for-sql-server)bakın.

---

> [!IMPORTANT]
> Bu makaledeki komut dosyaları Adventure **Works** veritabanını kullanmak için yazılmıştır.

> [!NOTE]
> İsteğe bağlı olarak Azure SQL yönetilen bir örneği kullanmayı seçebilirsiniz.
>
> Oluşturmak ve yapılandırmak için [Azure Portalını,](sql-database-managed-instance-get-started.md) [PowerShell'i](scripts/sql-database-create-configure-managed-instance-powershell.md)veya [CLI'yi,](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)ardından [yerinde](sql-database-managed-instance-configure-p2s.md) veya [VM](sql-database-managed-instance-configure-vm.md) bağlantısını kurun.
>
> Verileri yüklemek için, [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) [dosyasıyla BACPAC ile geri yükleme'ye](sql-database-import.md) bakın veya [Geniş Dünya İthalatçılar veritabanını geri yükleyin.](sql-database-managed-instance-get-started-restore.md)

Python ve Azure SQL veritabanını daha fazla keşfetmek için Python, [pyodbc deposu](https://github.com/mkleehammer/pyodbc/wiki/)ve [pyodbc örneği](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)için Azure SQL veritabanı [kitaplıklarına](/python/api/overview/azure/sql)bakın.

## <a name="get-sql-server-connection-information"></a>SQL sunucu bağlantı bilgilerini alın

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Gelecek yordamlar için tam nitelikli sunucu adı veya ana bilgisayar adı, veritabanı adı ve giriş bilgilerine ihtiyacınız vardır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. **SQL veritabanlarına** veya **SQL yönetilen örnekler** sayfasına gidin.

3. Genel **Bakış** sayfasında, tek bir veritabanı için **Sunucu adının** yanındaki tam nitelikli sunucu adını veya yönetilen bir örnek için **Host'un** yanındaki tam nitelikli sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine tıklayın ve **Kopyasimgesini** seçin.

## <a name="create-code-to-query-your-sql-database"></a>SQL veritabanınızı sorgulamak için kod oluşturma 

1. Metin *düzenleyicisinde, sqltest.py*adlı yeni bir dosya oluşturun.  
   
1. Aşağıdaki kodu ekleyin. Sunucu>, \<veritabanı \<>, \<kullanıcı adı> \<ve parola> için kendi değerlerinizi değiştirin.
   
   >[!IMPORTANT]
   >Bu örnekteki kod, veritabanınızı oluştururken kaynak olarak seçebileceğiniz örnek AdventureWorksLT verilerini kullanır. Veritabanınızda farklı veriler varsa, SELECT sorgusunda kendi veritabanınızdaki tabloları kullanın. 
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   driver= '{ODBC Driver 17 for SQL Server}'
   cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
   cursor = cnxn.cursor()
   cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
   row = cursor.fetchone()
   while row:
       print (str(row[0]) + " " + str(row[1]))
       row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde aşağıdaki komutu çalıştırın:

   ```cmd
   python sqltest.py
   ```

1. En iyi 20 Kategori/Ürün satırının döndürüldünden sonra komut penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [SQL Server için Microsoft Python sürücüleri](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python geliştirici merkezi](https://azure.microsoft.com/develop/python/?v=17.23h)

