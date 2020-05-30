---
title: Bağlama ve sorgu İçerik başvurusu
description: Azure SQL veritabanı hızlı başlangıçlarından bir başvuru, Azure SQL veritabanı 'na bağlanma ve sorgulama işlemlerinin nasıl yapılacağını gösterir.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/18/2018
ms.openlocfilehash: 12e53e6960cbd55b78c8459ae9db080f32e2ffb6
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84189647"
---
# <a name="quickstarts-azure-sql-database-connect-and-query"></a>Hızlı başlangıç: Azure SQL veritabanı Connect ve Query
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Aşağıdaki belge, Azure SQL veritabanı 'na bağlanma ve sorgulama işlemlerinin nasıl yapılacağını gösteren Azure örneklerinin bağlantılarını içerir. Ayrıca Taşıma Düzeyi Güvenliği için bazı öneriler sunar.

## <a name="quickstarts"></a>Hızlı Başlangıçlar

| |  |
|---|---|
|[SQL Server Management Studio](connect-query-ssms.md)|Bu hızlı başlangıçta SSMS kullanarak Azure SQL veritabanındaki bir veritabanına bağlanma ve ardından Transact-SQL deyimlerini kullanarak veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı gösterilmiştir.|
|[Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)|Bu hızlı başlangıçta, Azure SQL veritabanındaki bir veritabanına bağlanmak için Azure Data Studio kullanma ve ardından Transact-SQL (T-SQL) deyimlerini kullanarak Azure Data Studio öğreticilerde kullanılan Tutorialdb 'yi oluşturma gösterilmektedir.|
|[Azure portal](connect-query-portal.md)|Bu hızlı başlangıçta sorgu Düzenleyicisi 'ni kullanarak Azure SQL veritabanındaki bir veritabanına bağlanma ve ardından Transact-SQL deyimlerini kullanarak veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı gösterilmiştir.|
|[Visual Studio Code](connect-query-vscode.md)|Bu hızlı başlangıçta, Azure SQL veritabanındaki bir veritabanına bağlanmak için Visual Studio Code kullanma ve ardından Transact-SQL deyimlerini kullanarak veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı gösterilmiştir.|
|[Visual Studio ile .NET](connect-query-dotnet-visual-studio.md)|Bu hızlı başlangıçta, .NET Framework kullanarak Visual Studio ile Azure SQL veritabanı 'nda bir veritabanına bağlanma ve Transact-SQL deyimleriyle veri sorgulayan bir C# programı oluşturma işlemlerinin nasıl yapılacağı gösterilmiştir.|
|[.NET Core](connect-query-dotnet-core.md)|Bu hızlı başlangıçta, Windows/Linux/macOS 'ta .NET Core kullanarak Azure SQL veritabanındaki bir veritabanına bağlanma ve Transact-SQL deyimleriyle veri sorgulayan bir C# programı oluşturma işlemlerinin nasıl yapılacağı gösterilmiştir.|
|[Git](connect-query-go.md)|Bu hızlı başlangıçta Go kullanarak Azure SQL veritabanı 'nda bir veritabanına nasıl bağlanabileceğinizi göstermektedir. Verileri sorgulamak ve değiştirmek için Transact-SQL bildirimleri de gösterilir.|
|[Java](connect-query-java.md)|Bu hızlı başlangıçta Java kullanarak Azure SQL veritabanı 'nda bir veritabanına bağlanma ve ardından Transact-SQL deyimleriyle veri sorgulama işlemlerinin nasıl yapılacağı gösterilmiştir.|
|[Node.js](connect-query-nodejs.md)|Bu hızlı başlangıçta, Node. js kullanarak Azure SQL veritabanındaki bir veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir program oluşturma işlemlerinin nasıl yapılacağı gösterilmiştir.|
|[PHP](connect-query-php.md)|Bu hızlı başlangıçta, PHP kullanarak Azure SQL veritabanındaki bir veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir program oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir.|
|[Python](connect-query-python.md)|Bu hızlı başlangıçta Python kullanarak Azure SQL veritabanındaki bir veritabanına bağlanma ve Transact-SQL deyimleriyle veri sorgulama işlemlerinin nasıl yapılacağı gösterilmiştir. |
|[Ruby](connect-query-ruby.md)|Bu hızlı başlangıçta, Ruby kullanarak Azure SQL veritabanındaki bir veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir program oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir.|
|[R](connect-query-r.md)|Bu hızlı başlangıçta, Azure SQL veritabanı 'nda bir veritabanına bağlanmak üzere bir program oluşturmak ve verileri sorgulamak için Transact-SQL deyimlerini kullanmak üzere Azure SQL veritabanı ile R 'yi kullanma Machine Learning Services gösterilmektedir.|
|||

## <a name="tls-considerations-for-sql-database-connectivity"></a>SQL Veritabanı bağlanabilirliği için TLS konuları

Aktarım Katmanı Güvenliği (TLS), Microsoft 'un Azure SQL veritabanı 'nda bir veritabanına bağlanmak için sağladığı veya desteklediği tüm sürücüler tarafından kullanılır. Özel yapılandırma gerekli değildir. Bir SQL Server örneğine, Azure SQL veritabanı 'na veya Azure SQL yönetilen örneği örneğine yönelik tüm bağlantılar için, tüm uygulamaların aşağıdaki konfigürasyonları veya bunların eşdeğerleri ayarlaması önerilir:

- **Encrypt = On**
- **TrustServerCertificate = Off**

Bazı sistemler bu yapılandırma anahtar sözcükleri için farklı ancak eşdeğer anahtar sözcükler kullanmaktadır. Bu yapılandırmalar istemci sürücüsünün sunucudan alınan TLS sertifikası kimliğini doğrulamasını sağlamaktadır.

Ayrıca Ödeme Kartı Endüstrisi - Veri Güvenliği Standardı’na (PCI-DSS) uymanız gerekiyorsa istemcide TLS 1.1 ve 1.0’ı devre dışı bırakmanızı öneririz.

Microsoft olmayan sürücüler varsayılan olarak TLS’yi kullanmayabilir. Bu, Azure SQL Veritabanına bağlanırken bir faktör olabilir. Ekli sürücüleri olan uygulamalar bu bağlantı ayarlarını denetlemenize izin vermeyebilir. Hassas verilerle etkileşimde bulunan sistemlerde kullanmadan önce bu tarz sürücülerin ve uygulamaların güvenliğini incelemenizi öneririz.

## <a name="libraries"></a>Kitaplıklar

Azure SQL veritabanı veya Azure SQL yönetilen örneği 'ne bağlanmak için çeşitli kitaplıkları ve çerçeveleri kullanabilirsiniz. C#, Java, Node. js, PHP ve Python gibi programlama dillerini hızlıca kullanmaya başlamak için [Başlarken öğreticilerimize](https://aka.ms/sqldev) göz atın. Ardından, macOS üzerinde Linux üzerinde SQL Server veya Windows ya da Docker kullanarak bir uygulama oluşturun.

Aşağıdaki tabloda, istemci uygulamalarının şirket içinde veya bulutta çalışan SQL Server bağlanmak ve kullanmak için çeşitli dillerden kullanabileceği bağlantı kitaplıkları veya *sürücüler* listelenmiştir. Bunları Linux, Windows veya Docker üzerinde kullanabilir ve Azure SQL veritabanı ve Azure SQL veri ambarı 'na bağlanmak için kullanabilirsiniz.

| Dil | Platform | Ek kaynaklar | İndirme | başlarken |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [SQL Server için Microsoft ADO.NET](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-sql-server) | [İndir](https://www.microsoft.com/net/download/) | [başlarken](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [SQL Server için Microsoft JDBC sürücüsü](https://msdn.microsoft.com/library/mt484311.aspx) | [İndir](https://go.microsoft.com/fwlink/?linkid=852460) |  [başlarken](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [SQL Server için PHP SQL sürücüsü](https://docs.microsoft.com/sql/connect/php/microsoft-php-driver-for-sql-server) | [İndir](https://docs.microsoft.com/sql/connect/php/download-drivers-php-sql-server) | [başlarken](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/)
| Node.js | Windows, Linux, macOS | [SQL Server için Node. js sürücüsü](https://msdn.microsoft.com/library/mt652093.aspx) | [Yükleme](https://msdn.microsoft.com/library/mt652094.aspx) |  [başlarken](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Python SQL sürücüsü](https://msdn.microsoft.com/library/mt652092.aspx) | Seçenekleri yüklensin: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](https://msdn.microsoft.com/library/mt763257.aspx) |  [başlarken](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [SQL Server için Ruby sürücüsü](https://msdn.microsoft.com/library/mt691981.aspx) | [Yükleme](https://msdn.microsoft.com/library/mt711041.aspx) | [başlarken](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [SQL Server için Microsoft ODBC sürücüsü](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) | [İndir](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) |  

Aşağıdaki tabloda, istemci uygulamalarının SQL Server, Azure SQL veritabanı, Azure SQL yönetilen örneği veya Azure SYNAPSE Analytics ile kullanabileceği nesne ilişkisel eşleme (ORM) çerçeveleri ve Web çerçeveleri örnekleri listelenmektedir. Çerçeveleri Linux, Windows veya Docker üzerinde kullanabilirsiniz.

| Dil | Platform | ORM (s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Varlık Çerçevesi](https://docs.microsoft.com/ef)<br>[Entity Framework Core](https://docs.microsoft.com/ef/core/index) |
| Java | Windows, Linux, macOS |[Bu ORM hazırda beklet](https://hibernate.org/orm)|
| PHP | Windows, Linux, macOS | [Laralevel (eloquent)](https://laravel.com/docs/eloquent)<br>[Doctrine](https://www.doctrine-project.org/projects/orm.html) |
| Node.js | Windows, Linux, macOS | [Sequelize ORM](https://sequelize.org/) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on rayları](https://rubyonrails.org/) |
||||

## <a name="next-steps"></a>Sonraki adımlar

- Bağlanabilirlik mimarisi bilgileri için bkz. [Azure SQL Veritabanı Bağlanabilirlik Mimarisi](connectivity-architecture.md)
- İstemci uygulamalarından bağlanmak için kullanılan [SQL Server sürücülerini](https://msdn.microsoft.com/library/mt654049.aspx) bulun.
- Azure SQL veritabanı 'na bağlanma:
  - [.NET (C#) kullanarak SQL Veritabanı’na bağlanma](connect-query-dotnet-core.md)
  - [PHP kullanarak SQL Veritabanı’na bağlanma](connect-query-php.md)
  - [Node.js kullanarak SQL Veritabanı’na bağlanma](connect-query-nodejs.md)
  - [Java kullanarak SQL Veritabanı’na bağlanma](connect-query-java.md)
  - [Python kullanarak SQL Veritabanı’na bağlanma](connect-query-python.md)
  - [Ruby kullanarak SQL Veritabanı’na bağlanma](connect-query-ruby.md)
- Yeniden deneme mantığı kodu örnekleri:
  - [ADO.NET ile SQL veritabanına dayanıklı bağlantısı bağlama][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
  - [Dayanıklı bağlantısı 'i PHP ile SQL veritabanı 'na bağlama][step-4-connect-resiliently-to-sql-with-php-p42h]

<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php
