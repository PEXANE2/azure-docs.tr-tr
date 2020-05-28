---
title: Sorgulamak için Ruby kullanma
description: Bu konu başlığı altında, Ruby kullanarak Azure SQL veritabanı 'nda bir veritabanına bağlanan ve Transact-SQL deyimlerini kullanarak sorgulayan bir program oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2 
ms.devlang: ruby
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: 1a0f65ff6d0b98d3764cb6a7b1d4b07ccb8ae92a
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84054410"
---
# <a name="quickstart-use-ruby-to-query-a-microsoft-azure-sql-database"></a>Hızlı başlangıç: Microsoft Azure SQL veritabanını sorgulamak için Ruby kullanma
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu hızlı başlangıçta, [Ruby](https://www.ruby-lang.org) kullanarak Azure SQL veritabanına bağlanma ve Transact-SQL deyimleriyle veri sorgulama işlemlerinin nasıl yapılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için aşağıdaki önkoşullara sahip olmanız gerekir:

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
  > Bu makaledeki betikler, Adventure Works veritabanını kullanmak için yazılmıştır. Yönetilen bir örnek ile, Adventure Works veritabanını bir örnek veritabanına aktarmanız veya bu makaledeki betikleri Wide World Importers veritabanını kullanacak şekilde değiştirmeniz gerekir.
  
- İşletim sisteminiz için Ruby ve ilgili yazılımlar:
  
  - **MacOS**: homebrew, rbenv ve Ruby-Build, Ruby, FreeTDS ve TinyTDS 'i yükler. [MacOS 'ta SQL Server kullanarak Ruby uygulamaları oluşturma konusunda](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/)1,2, 1,3, 1,4, 1,5 ve 2,1 adımlarına bakın.
  
  - **Ubuntu**: Ruby, rbenv ve Ruby-Build, Ruby, FreeTDS ve TinyTDS için önkoşulları yükler. [Ubuntu üzerinde SQL Server kullanarak Ruby uygulamaları oluşturma konusunda](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/)1,2, 1,3, 1,4, 1,5 ve 2,1 adımlarına bakın.
  
  - **Windows**: Ruby, Ruby devkit ve TinyTDS 'i yükler. Bkz. [Ruby geliştirmesi için geliştirme ortamını yapılandırma](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development).

## <a name="get-sql-server-connection-information"></a>SQL Server bağlantı bilgilerini al

Azure SQL veritabanı 'na bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, bir Azure SQL veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya Azure SQL yönetilen örneği için **konak** ' ın YANıNDAKI tam sunucu adını (veya IP adresini) veya bir Azure VM 'de SQL Server ' yi inceleyin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

> [!NOTE]
> Azure VM 'de SQL Server bağlantı bilgileri için bkz. [Connect SQL Server](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server)

## <a name="create-code-to-query-your-sql-database"></a>SQL veritabanınızı sorgulamak için kod oluşturma

1. Bir metin veya kod düzenleyicisinde *SQLtest. RB*adlı yeni bir dosya oluşturun.

1. Aşağıdaki kodu ekleyin. ,, Ve için Azure SQL veritabanınızdaki değerleri değiştirin `<server>` `<database>` `<username>` `<password>` .

   >[!IMPORTANT]
   >Bu örnekteki kod, veritabanınızı oluştururken kaynak olarak seçebileceğiniz örnek AdventureWorksLT verilerini kullanır. Veritabanınızda farklı veriler varsa, SELECT sorgusunda kendi veritabanınızdaki tabloları kullanın. 

   ```ruby
   require 'tiny_tds'
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   client = TinyTds::Client.new username: username, password: password, 
       host: server, port: 1433, database: database, azure: true
   
   puts "Reading data from table"
   tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
           ON pc.productcategoryid = p.productcategoryid"
   result = client.execute(tsql)
   result.each do |row|
       puts row
   end
   ```

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde aşağıdaki komutu çalıştırın:

   ```bash
   ruby sqltest.rb
   ```
   
1. Veritabanınızdaki ilk 20 kategori/ürün satırı döndürüldüğünden emin olun. 

## <a name="next-steps"></a>Sonraki adımlar
- [Ilk Azure SQL veritabanınızı tasarlayın](design-first-database-tutorial.md).
- [TinyTDS Için GitHub deposu](https://github.com/rails-sqlserver/tiny_tds).
- [Sorunları bildirin veya TinyTDS hakkında sorular sorun](https://github.com/rails-sqlserver/tiny_tds/issues).
- [SQL Server Için Ruby sürücüsü](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/).
