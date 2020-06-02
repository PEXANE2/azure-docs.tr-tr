---
title: Sorgulamak Için Visual Studio ile .NET ve C# kullanın
description: Azure SQL veritabanı veya Azure SQL yönetilen örneği 'nde bir veritabanına bağlanan ve sorguları çalıştıran bir C# uygulaması oluşturmak için Visual Studio 'Yu kullanın.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2 
ms.devlang: dotnet
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 05/29/2020
ms.openlocfilehash: 94f005ee4b2542e4cc0908880d589a2ee6a4d798
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267435"
---
# <a name="quickstart-use-net-and-c-in-visual-studio-to-connect-to-and-query-a-database-in-azure-sql-database-or-azure-sql-managed-instance"></a>Hızlı başlangıç: Azure SQL veritabanı veya Azure SQL yönetilen örneği 'ne bağlanmak ve veritabanını sorgulamak için Visual Studio 'Da .NET ve C# kullanın
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Bu hızlı başlangıçta, Azure SQL veritabanı 'nda Transact-SQL deyimleriyle bir veritabanını sorgulamak için Visual Studio 'da [.NET Framework](https://www.microsoft.com/net/) ve C# kodunun nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Bir Azure SQL veritabanı. Azure SQL veritabanı 'nda bir veritabanı oluşturmak ve yapılandırmak için bu hızlı başlangıçlardan birini kullanabilirsiniz:

  || SQL Veritabanı | SQL Yönetilen Örnek | Azure VM’lerde SQL Server |
  |:--- |:--- |:---|:---|
  | Oluştur| [Portal](single-database-create-quickstart.md) | [Portal](../managed-instance/instance-create-quickstart.md) | [Portal](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  || [CLI](scripts/create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/create-and-configure-database-powershell.md) | [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md) | [PowerShell](../virtual-machines/windows/sql-vm-create-powershell-quickstart.md)
  | Yapılandırma | [Sunucu düzeyi IP güvenlik duvarı kuralı](firewall-create-server-level-portal-quickstart.md)| [Bir VM 'den bağlantı](../managed-instance/connect-vm-instance-configure.md)|
  |||[Şirket içinden bağlantı](../managed-instance/point-to-site-p2s-configure.md) | [SQL Server’a bağlanma](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  |Veri yükleme|Hızlı başlangıç başına yüklenen Adventure Works|[Geniş dünyada içeri aktarıcılar geri yükleme](../managed-instance/restore-sample-database-quickstart.md) | [Geniş dünyada içeri aktarıcılar geri yükleme](../managed-instance/restore-sample-database-quickstart.md) |
  |||[GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 'Dan bir [bacpac](database-import.md) dosyasından Adventure Works 'ü geri yükleme veya içeri aktarma| [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 'Dan bir [bacpac](database-import.md) dosyasından Adventure Works 'ü geri yükleme veya içeri aktarma|
  |||

  > [!IMPORTANT]
  > Bu makaledeki betikler, Adventure Works veritabanını kullanmak için yazılmıştır. SQL yönetilen örneği ile, Adventure Works veritabanını bir örnek veritabanına aktarmanız veya bu makaledeki betikleri Wide World Importers veritabanını kullanacak şekilde değiştirmeniz gerekir.

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) Community, Professional veya Enterprise Edition.

## <a name="get-server-connection-information"></a>Sunucu bağlantı bilgilerini al

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, Azure SQL veritabanı 'ndaki bir veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya Azure SQL yönetilen örneği için **konak** ' ın YANıNDAKI tam sunucu adını (veya IP adresini) veya Azure VM 'de SQL Server ' yi gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

> [!NOTE]
> Azure VM 'de SQL Server yönelik bağlantı bilgileri için bkz. [SQL Server örneğine bağlanma](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server).

## <a name="create-code-to-query-the-database-in-azure-sql-database"></a>Azure SQL veritabanı 'nda veritabanını sorgulamak için kod oluşturma

1. Visual Studio 'da **Dosya**  >  **Yeni**  >  **Proje**' yi seçin. 
   
1. **Yeni proje** iletişim kutusunda, **Visual C#**' yi seçin ve ardından **konsol uygulaması (.NET Framework)** öğesini seçin.
   
1. Proje adı için *SQLtest* girin ve ardından **Tamam**' ı seçin. Yeni proje oluşturulur. 
   
1. **Proje**  >  **NuGet Paketlerini Yönet**' i seçin. 
   
1. **NuGet Paket Yöneticisi**' nde, **Gözden** geçirme sekmesini seçin, sonra **System. Data. SqlClient**öğesini arayıp seçin.
   
1. **System. Data. SqlClient** sayfasında, **Install**' ı seçin. 
   - İstenirse, yüklemeye devam etmek için **Tamam** ' ı seçin. 
   - Bir **Lisans kabul** penceresi görüntülenirse **kabul ediyorum**' u seçin.
   
1. Yüklemesi tamamlandığında, **NuGet Paket Yöneticisi 'ni**kapatabilirsiniz. 
   
1. Kod Düzenleyicisi 'nde, **program.cs** içeriğini aşağıdaki kodla değiştirin. ,, Ve için değerlerinizi değiştirin `<server>` `<username>` `<password>` `<database>` .
   
   >[!IMPORTANT]
   >Bu örnekteki kod, veritabanınızı oluştururken kaynak olarak seçebileceğiniz örnek AdventureWorksLT verilerini kullanır. Veritabanınızda farklı veriler varsa, SELECT sorgusunda kendi veritabanınızdaki tabloları kullanın. 
   
   ```csharp
   using System;
   using System.Data.SqlClient;
   using System.Text;
   
   namespace sqltest
   {
       class Program
       {
           static void Main(string[] args)
           {
               try 
               { 
                   SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                   builder.DataSource = "<server>.database.windows.net"; 
                   builder.UserID = "<username>";            
                   builder.Password = "<password>";     
                   builder.InitialCatalog = "<database>";
   
                   using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                   {
                       Console.WriteLine("\nQuery data example:");
                       Console.WriteLine("=========================================\n");
                       
                       StringBuilder sb = new StringBuilder();
                       sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                       sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                       sb.Append("JOIN [SalesLT].[Product] p ");
                       sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                       String sql = sb.ToString();
   
                       using (SqlCommand command = new SqlCommand(sql, connection))
                       {
                           connection.Open();
                           using (SqlDataReader reader = command.ExecuteReader())
                           {
                               while (reader.Read())
                               {
                                   Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                               }
                           }
                       }                    
                   }
               }
               catch (SqlException e)
               {
                   Console.WriteLine(e.ToString());
               }
               Console.ReadLine();
           }
       }
   }
   ```

## <a name="run-the-code"></a>Kodu çalıştırma

1. Uygulamayı çalıştırmak için **hata**  >  **ayıklamayı Başlat hata**Ayıkla ' yı seçin ya da araç çubuğundan **Başlat** ' ı seçin veya **F5**tuşuna basın.
1. Veritabanınızdaki ilk 20 kategori/ürün satırı döndürüldüğünden emin olun ve ardından uygulama penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- Windows/Linux/macOS 'ta [.NET Core kullanarak Azure SQL veritabanı 'nda bir veritabanını bağlamayı ve sorgulamayı](connect-query-dotnet-core.md) öğrenin.  
- [Komut satırını kullanarak Windows/Linus/macOS’ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli) hakkında bilgi edinin.
- [SMS kullanarak ilk veritabanınızı Azure SQL veritabanı 'nda nasıl tasarlayacağınızı](design-first-database-tutorial.md) öğrenin veya [.NET kullanarak Azure SQL veritabanı 'Nda ilk veritabanınızı tasarlayın](design-first-database-csharp-tutorial.md).
- .NET hakkında daha fazla bilgi edinmek için [.NET belgelerine](https://docs.microsoft.com/dotnet/) bakın.
- Yeniden deneme mantığı örneği: [ADO.net Ile Azure SQL 'e dayanıklı bağlantısı bağlayın][step-4-connect-resiliently-to-sql-with-ado-net-a78n].


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net

