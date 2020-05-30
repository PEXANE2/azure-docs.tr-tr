---
title: Bir veritabanını sorgulamak için Python kullanma
description: Bu konu başlığı altında, Python kullanarak Azure SQL veritabanı 'nda bir veritabanına bağlanan ve Transact-SQL deyimlerini kullanarak sorgulayan bir program oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: seo-python-october2019, sqldbrb=2 
ms.devlang: python
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: 4a6bcee7e2868c80491471640f33bb7709f740a5
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84189167"
---
# <a name="quickstart-use-python-to-query-a-database-in-azure-sql-database"></a>Hızlı başlangıç: Azure SQL veritabanı 'nda bir veritabanını sorgulamak için Python kullanma
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu hızlı başlangıçta Python kullanarak bir Azure SQL veritabanına bağlanır ve T-SQL deyimlerini kullanarak verileri sorgulayın.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

  || SQL Veritabanı | SQL Yönetilen Örnek | Azure VM’lerde SQL Server |
  |:--- |:--- |:---|:---|
  | Oluştur| [Portal](single-database-create-quickstart.md) | [Portal](../managed-instance/instance-create-quickstart.md) | [Portal](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  || [CLI](scripts/create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/create-and-configure-database-powershell.md) | [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md) | [PowerShell](../virtual-machines/windows/sql-vm-create-powershell-quickstart.md)
  | Yapılandırma | [Sunucu düzeyi IP güvenlik duvarı kuralı](firewall-create-server-level-portal-quickstart.md)| [Bir VM 'den bağlantı](../managed-instance/connect-vm-instance-configure.md)|
  |||[Şirket içinden bağlantı](../managed-instance/point-to-site-p2s-configure.md) | [SQL Server örneğine bağlanma](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  |Veri yükleme|Hızlı başlangıç başına yüklenen Adventure Works|[Geniş dünyada içeri aktarıcılar geri yükleme](../managed-instance/restore-sample-database-quickstart.md) | [Geniş dünyada içeri aktarıcılar geri yükleme](../managed-instance/restore-sample-database-quickstart.md) |
  |||[GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 'Dan bir [bacpac](database-import.md) dosyasından Adventure Works 'ü geri yükleme veya içeri aktarma| [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 'Dan bir [bacpac](database-import.md) dosyasından Adventure Works 'ü geri yükleme veya içeri aktarma|
  |||

- [Python](https://python.org/downloads) 3 ve ilgili yazılımlar

  # <a name="macos"></a>[macOS](#tab/macos)

  Homebrew ve Python 'u, ODBC sürücüsünü ve SQLCMD 'yi ve SQL Server için Python sürücüsünü yüklemek için [macOS 'ta SQL Server kullanarak Python uygulamaları oluşturma](https://www.microsoft.com/sql-server/developer-get-started/python/mac/)bölümünde **1,2**, **1,3**ve **2,1** adımlarını kullanın.

  Daha fazla bilgi için bkz. [macOS 'Ta MICROSOFT ODBC sürücüsü](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Python ve diğer gerekli paketleri yüklemek için kullanın `sudo apt-get install python python-pip gcc g++ build-essential` .

  SQL Server için ODBC sürücüsünü, SQLCMD 'yi ve Python sürücüsünü yüklemek için bkz. [pyodbc Python geliştirmesi için bir ortam yapılandırma](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).

  Daha fazla bilgi için bkz. [Linux 'Ta MICROSOFT ODBC sürücüsü](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="windows"></a>[Windows](#tab/windows)

  Python 'u, ODBC sürücüsünü ve SQLCMD 'yi ve SQL Server için Python sürücüsünü yüklemek için bkz. [pyodbc Python geliştirmesi için bir ortam yapılandırma](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

  Daha fazla bilgi için bkz. [MICROSOFT ODBC sürücüsü](/sql/connect/odbc/microsoft-odbc-driver-for-sql-server).

---

> [!IMPORTANT]
> Bu makaledeki betikler, **Adventure Works** veritabanını kullanmak için yazılmıştır.

> [!NOTE]
> İsteğe bağlı olarak bir Azure SQL yönetilen örneği kullanmayı seçebilirsiniz.
>
> Oluşturmak ve yapılandırmak için [Azure Portal](../managed-instance/instance-create-quickstart.md), [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md)veya [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)kullanın ve ardından [Şirket içi](../managed-instance/point-to-site-p2s-configure.md) veya [VM](../managed-instance/connect-vm-instance-configure.md) bağlantısı ayarlayın.
>
> Verileri yüklemek için bkz. [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) dosyası Ile [bacpac ile geri yükleme](database-import.md) veya bkz. [Wide World Importers veritabanını geri yükleme](../managed-instance/restore-sample-database-quickstart.md).

Python ve Azure SQL veritabanı 'nda veritabanını daha fazla incelemek için bkz. [Python Için Azure SQL veritabanı kitaplıkları](/python/api/overview/azure/sql), [pyodbc deposu](https://github.com/mkleehammer/pyodbc/wiki/)ve bir [pyodbc örneği](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="get-server-connection-information"></a>Sunucu bağlantı bilgilerini al

Azure SQL veritabanı 'nda veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, Azure SQL veritabanı 'nda veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya Azure SQL yönetilen örneği için **konak** ' ın YANıNDAKI tam sunucu adını (veya IP adresini) veya Azure VM 'de SQL Server ' yi gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

> [!NOTE]
> Azure VM 'de SQL Server yönelik bağlantı bilgileri için bkz. [SQL Server örneğine bağlanma](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server).

## <a name="create-code-to-query-your-database"></a>Veritabanınızı sorgulamak için kod oluşturma 

1. Bir metin düzenleyicisinde, *SQLtest.py*adlı yeni bir dosya oluşturun.  
   
1. Aşağıdaki kodu ekleyin. ,, Ve için kendi değerlerinizi yerine koyun \<server> \<database> \<username> \<password> .
   
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

- [Azure SQL veritabanı 'nda ilk veritabanınızı tasarlama](design-first-database-tutorial.md)
- [SQL Server için Microsoft Python sürücüleri](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/?v=17.23h)

