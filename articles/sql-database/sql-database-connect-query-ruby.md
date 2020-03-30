---
title: Sorgu yapmak için Ruby'yi kullanma
description: Bu konu başlığı altında, Ruby kullanarak Azure SQL Veritabanına bağlanan ve Transact-SQL deyimleriyle veritabanını sorgulayan bir program oluşturma işlemi gösterilir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ruby
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: 4bef55b049ee542efdb9d72d13fa196c989c75ec
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73826919"
---
# <a name="quickstart-use-ruby-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Ruby kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlangıç, Bir Azure SQL veritabanına bağlanmak ve Transact-SQL deyimleri ile verileri sorgulamak için [Ruby'nin](https://www.ruby-lang.org) nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki ön koşullara ihtiyacınız vardır:

- Bir Azure SQL veritabanı. Azure SQL Veritabanı'nda bir veritabanı oluşturmak ve yapılandırmak için bu hızlı başlangıçlardan birini kullanabilirsiniz:

  || Tek veritabanı | Yönetilen örnek |
  |:--- |:--- |:---|
  | Oluşturma| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [Powershell](scripts/sql-database-create-and-configure-database-powershell.md) | [Powershell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Yapılandırma | [Sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)| [VM'den bağlantı](sql-database-managed-instance-configure-vm.md)|
  |||[Siteden bağlantı](sql-database-managed-instance-configure-p2s.md)
  |Veri yükleme|Adventure Works quickstart başına yüklenen|[Geniş Dünya İthalatçıları Geri Yükleme](sql-database-managed-instance-get-started-restore.md)
  |||[GitHub'dan](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) [BACPAC](sql-database-import.md) dosyasından Adventure Works'ü geri yükleme veya alma|
  |||

  > [!IMPORTANT]
  > Bu makaledeki komut dosyaları Adventure Works veritabanını kullanmak için yazılmıştır. Yönetilen bir örnekle, Adventure Works veritabanını bir örnek veritabanına aktarmanız veya Geniş Dünya İthalatçılar veritabanını kullanmak için bu makaledeki komut dosyalarını değiştirmeniz gerekir.
  
- İşletim sisteminiz için ruby ve ilgili yazılımlar:
  
  - **MacOS**: Homebrew, rbenv ve yakut yapımı, Ruby, FreeTDS ve TinyTDS'i yükleyin. [MacOS'ta SQL Server'ı kullanarak Ruby uygulamaları oluşturma'daki](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/)1.2, 1.3, 1.4, 1.5 ve 2.1 adımlarını görün.
  
  - **Ubuntu**: Ruby, rbenv ve ruby-build, Ruby, FreeTDS ve TinyTDS için ön koşulları yükleyin. [Ubuntu'da SQL Server'ı kullanarak Ruby uygulamaları oluşturma'daki](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/)1.2, 1.3, 1.4, 1.5 ve 2.1 adımlarını görün.
  
  - **Windows**: Ruby, Ruby Devkit ve TinyTDS'yi yükleyin. Bkz. [Ruby geliştirme için geliştirme ortamını yapılandırın.](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development)

## <a name="get-sql-server-connection-information"></a>SQL sunucu bağlantı bilgilerini alın

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Gelecek yordamlar için tam nitelikli sunucu adı veya ana bilgisayar adı, veritabanı adı ve giriş bilgilerine ihtiyacınız vardır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. **SQL veritabanlarına** veya **SQL yönetilen örnekler** sayfasına gidin.

3. Genel **Bakış** sayfasında, tek bir veritabanı için **Sunucu adının** yanındaki tam nitelikli sunucu adını veya yönetilen bir örnek için **Host'un** yanındaki tam nitelikli sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine tıklayın ve **Kopyasimgesini** seçin. 

## <a name="create-code-to-query-your-sql-database"></a>SQL veritabanınızı sorgulamak için kod oluşturma

1. Bir metin veya kod düzenleyicisinde *sqltest.rb*adında yeni bir dosya oluşturun.
   
1. Aşağıdaki kodu ekleyin. Azure SQL veritabanınızdaki değerleri `<server>`, `<database>` `<username>`, `<password>`, ve .
   
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
   
1. Veritabanınızdaki en iyi 20 Kategori/Ürün satırının döndürüldünüzü doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
- [İlk Azure SQL veritabanınızı tasarleyin.](sql-database-design-first-database.md)
- [TinyTDS için GitHub deposu.](https://github.com/rails-sqlserver/tiny_tds)
- [Sorunları bildirin veya TinyTDS hakkında sorular sorun.](https://github.com/rails-sqlserver/tiny_tds/issues)
- [SQL Server için Ruby sürücüsü.](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
