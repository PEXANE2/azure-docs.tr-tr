---
title: Sorgulamak için .NET ve C# ile Visual Studio'u kullanma
description: Azure SQL Veritabanı'na bağlanan ve Transact-SQL deyimleriyle sorgulayan bir C# uygulaması oluşturmak için Visual Studio'yı kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: dotnet
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/31/2019
ms.openlocfilehash: 75f84f87fc59a870a0c88fd24cb1b1b8894b3e2a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79209564"
---
# <a name="quickstart-use-net-and-c-in-visual-studio-to-connect-to-and-query-an-azure-sql-database"></a>Quickstart: Bir Azure SQL veritabanına bağlanmak ve sorgulamak için Visual Studio'da .NET ve C# kullanın

Bu hızlı başlangıç, Visual Studio'da [.NET Framework](https://www.microsoft.com/net/) ve C# kodunu kullanarak Transact-SQL deyimleriyle bir Azure SQL veritabanını sorgulayın.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

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

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) Topluluk, Profesyonel veya Kurumsal sürüm.

## <a name="get-sql-server-connection-information"></a>SQL sunucu bağlantı bilgilerini alın

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Gelecek yordamlar için tam nitelikli sunucu adı veya ana bilgisayar adı, veritabanı adı ve giriş bilgilerine ihtiyacınız vardır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. **SQL veritabanlarına** veya **SQL yönetilen örnekler** sayfasına gidin.

3. Genel **Bakış** sayfasında, tek bir veritabanı için **Sunucu adının** yanındaki tam nitelikli sunucu adını veya yönetilen bir örnek için **Host'un** yanındaki tam nitelikli sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine tıklayın ve **Kopyasimgesini** seçin. 

## <a name="create-code-to-query-the-sql-database"></a>SQL veritabanını sorgulamak için kod oluşturma

1. Visual Studio'da **Dosya** > **Yeni** > **Projesi'ni**seçin. 
   
1. Yeni **Proje** iletişim kutusunda **Visual C#** seçeneğini belirleyin ve ardından **Konsol Uygulaması'nı (.NET Framework)** seçin.
   
1. Proje adı için *sqltest* girin ve sonra **Tamam'ı**seçin. Yeni proje oluşturuldu. 
   
1. **Proje** > **Yi Yönet NuGet Paketlerini**Seçin. 
   
1. **NuGet Package Manager'da** **Gözat** sekmesini seçin, ardından **System.Data.SqlClient'ı**arayın ve seçin.
   
1. **System.Data.SqlClient** sayfasında **Install**'ı seçin. 
   - İstenirse, yüklemeye devam etmek için **Tamam'ı** seçin. 
   - Lisans **Kabul** penceresi görünüyorsa, **Kabul Et'i**seçin.
   
1. Yükleme tamamlandığında **NuGet Paket Yöneticisi'ni**kapatabilirsiniz. 
   
1. Kod düzenleyicisinde, **Program.cs** içeriğini aşağıdaki kodla değiştirin. Değerlerinizi `<server>`, `<username>`, `<password>`ve `<database>`.
   
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

1. Uygulamayı çalıştırmak için **Hata Ayıklama** > **Başlatma Hata Ayıklama'yı**seçin veya araç çubuğunda **Başlat'ı** seçin veya **F5 tuşuna**basın.
1. Veritabanınızdaki en iyi 20 Kategori/Ürün satırının döndürüldünüzü doğrulayın ve ardından uygulama penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- Windows/Linux/macOS'ta [.NET Core'u kullanarak bir Azure SQL veritabanına nasıl bağlanıp sorgulandığınızı](sql-database-connect-query-dotnet-core.md) öğrenin.  
- [Komut satırını kullanarak Windows/Linus/macOS’ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli) hakkında bilgi edinin.
- [SSMS kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database.md) veya [.NET kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database-csharp.md) öğrenin.
- .NET hakkında daha fazla bilgi edinmek için [.NET belgelerine](https://docs.microsoft.com/dotnet/) bakın.
- Mantık örneğini yeniden deneyin: [ADO.NET ile SQL'e esnek bir şekilde bağlanın.][step-4-connect-resiliently-to-sql-with-ado-net-a78n]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net

