---
title: Bir veritabanını sorgulamak için Python kullanma
description: Bu konu başlığı altında, Python kullanarak Azure SQL veritabanına bağlanan ve Transact-SQL deyimlerini kullanarak sorgulayan bir program oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir.
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
ms.openlocfilehash: 9929c483c2e27983254033e2e26b3b753699260b
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76758317"
---
# <a name="quickstart-use-python-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Python kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlangıçta Python kullanarak bir Azure SQL veritabanına bağlanır ve T-SQL deyimlerini kullanarak verileri sorgulayın.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- [Azure SQL veritabanı](sql-database-single-database-get-started.md)
  
- [Python](https://python.org/downloads) 3 ve ilgili yazılımlar

  # <a name="macostabmacos"></a>[macOS](#tab/macos)

  Homebrew ve Python 'u, ODBC sürücüsünü ve SQLCMD 'yi ve SQL Server için Python sürücüsünü yüklemek için [macOS 'ta SQL Server kullanarak Python uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/python/mac/)bölümünde **1,2**, **1,3**ve **2,1** adımlarını kullanın.

  Daha fazla bilgi için bkz. [macOS 'Ta MICROSOFT ODBC sürücüsü](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="ubuntutabubuntu"></a>[Ubuntu](#tab/ubuntu)

  Python ve diğer gerekli paketleri yüklemek için `sudo apt-get install python python-pip gcc g++ build-essential`kullanın.

  SQL Server için ODBC sürücüsünü, SQLCMD 'yi ve Python sürücüsünü yüklemek için bkz. [pyodbc Python geliştirmesi için bir ortam yapılandırma](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).

  Daha fazla bilgi için bkz. [Linux 'Ta MICROSOFT ODBC sürücüsü](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="windowstabwindows"></a>[Windows](#tab/windows)

  Python 'u, ODBC sürücüsünü ve SQLCMD 'yi ve SQL Server için Python sürücüsünü yüklemek için bkz. [pyodbc Python geliştirmesi için bir ortam yapılandırma](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

  Daha fazla bilgi için bkz. [MICROSOFT ODBC sürücüsü](/sql/connect/odbc/microsoft-odbc-driver-for-sql-server).

---

> [!IMPORTANT]
> Bu makaledeki betikler, **Adventure Works** veritabanını kullanmak için yazılmıştır.

> [!NOTE]
> İsteğe bağlı olarak bir Azure SQL yönetilen örneği kullanmayı seçebilirsiniz.
>
> Oluşturmak ve yapılandırmak için [Azure portalını](sql-database-managed-instance-get-started.md), [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md)'i veya [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)'Yi kullanın, sonra [yerinde veya](sql-database-managed-instance-configure-p2s.md) [VM](sql-database-managed-instance-configure-vm.md) bağlantısı kurun.
>
> Verileri yüklemek için bkz. [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) dosyası Ile [bacpac ile geri yükleme](sql-database-import.md) veya bkz. [Wide World Importers veritabanını geri yükleme](sql-database-managed-instance-get-started-restore.md).

Python ve Azure SQL veritabanı hakkında daha fazla bilgi için bkz. [Python Için Azure SQL veritabanı kitaplıkları](/python/api/overview/azure/sql), [pyodbc deposu](https://github.com/mkleehammer/pyodbc/wiki/)ve bir [pyodbc örneği](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="get-sql-server-connection-information"></a>SQL Server bağlantı bilgilerini al

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, tek bir veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya yönetilen örnek Için **ana bilgisayar ' ın** yanındaki tam sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

## <a name="create-code-to-query-your-sql-database"></a>SQL veritabanınızı sorgulamak için kod oluşturma 

1. Bir metin düzenleyicisinde, *SQLtest.py*adlı yeni bir dosya oluşturun.  
   
1. Aşağıdaki kodu ekleyin. \<Server >, \<veritabanı >, \<Kullanıcı adı > ve \<Password > için kendi değerlerinizi değiştirin.
   
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

1. İlk 20 kategori/ürün satırlarının döndürüldüğünden emin olun ve ardından komut penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [SQL Server için Microsoft Python sürücüleri](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/?v=17.23h)

