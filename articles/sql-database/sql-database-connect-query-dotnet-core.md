---
title: Sorgulamak için .NET Core'u kullanın
description: Bu konu, Bir Azure SQL Veritabanına bağlanan ve Transact-SQL deyimlerini kullanarak sorgulayan bir program oluşturmak için .NET Core'u nasıl kullanacağınızı gösterir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: dotnet
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 07/29/2019
ms.openlocfilehash: 369c708fd3181076c6deb9d7ac9134c57a18f819
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73827101"
---
# <a name="quickstart-use-net-core-c-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: .NET Core (C#) kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlatmada, bir Azure SQL veritabanına bağlanmak için [.NET Core](https://www.microsoft.com/net/) ve C# kodunu kullanırsınız. Daha sonra verileri sorgulamak için bir Transact-SQL deyimi çalıştırırsınız.

> [!TIP]
> Aşağıdaki Microsoft Learn modülü, [Azure SQL Veritabanı'nı sorgulayan bir ASP.NET uygulamanın nasıl geliştirilip yapılandırılabildiğini](https://docs.microsoft.com/learn/modules/develop-app-that-queries-azure-sql/) ücretsiz olarak öğrenmenize yardımcı olur

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici için şunları yapmanız gerekir:

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

- [.NET Core işletim sisteminiz için](https://www.microsoft.com/net/core) yüklü.

> [!NOTE]
> Bu hızlı başlatma *mySampleDatabase* veritabanını kullanır. Farklı bir veritabanı kullanmak istiyorsanız, veritabanı başvurularını değiştirmeniz ve `SELECT` C# kodundaki sorguyu değiştirmeniz gerekir.

## <a name="get-sql-server-connection-information"></a>SQL sunucu bağlantı bilgilerini alın

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Gelecek yordamlar için tam nitelikli sunucu adı veya ana bilgisayar adı, veritabanı adı ve giriş bilgilerine ihtiyacınız vardır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. **SQL veritabanlarına** veya **SQL yönetilen örnekler** sayfasına gidin.

3. Genel **Bakış** sayfasında, tek bir veritabanı için **Sunucu adının** yanındaki tam nitelikli sunucu adını veya yönetilen bir örnek için **Host'un** yanındaki tam nitelikli sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine tıklayın ve **Kopyasimgesini** seçin.

## <a name="get-adonet-connection-information-optional"></a>bağlantı bilgilerini ADO.NET (isteğe bağlı) alın

1. **mySampleDatabase** sayfasına gidin ve **Ayarlar**altında **Bağlantı dizeleri'ni**seçin.

2. Tam **ADO.NET** bağlantı dizesini gözden geçirin.

    ![ADO.NET bağlantı dizesi](./media/sql-database-connect-query-dotnet/adonet-connection-string2.png)

3. Kullanmak istiyorsanız **ADO.NET** bağlantı dizesini kopyalayın.
  
## <a name="create-a-new-net-core-project"></a>Yeni bir .NET Core projesi oluşturma

1. Komut istemini açın ve **sqltest** adlı bir klasör oluşturun. Bu klasöre gidin ve bu komutu çalıştırın.

    ```cmd
    dotnet new console
    ```
    Bu komut, bir XML yapılandırma dosyası**Program.cs****(sqltest.csproj)** ve gerekli ikili dosyalar dahil olmak üzere yeni uygulama proje dosyaları oluşturur.

2. Metin düzenleyicisinde **sqltest.csproj'u** açın ve etiketler arasında `<Project>` aşağıdaki XML'yi yapıştırın. Bu XML `System.Data.SqlClient` bağımlılık olarak ekler.

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.6.0" />
    </ItemGroup>
    ```

## <a name="insert-code-to-query-sql-database"></a>SQL veritabanını sorgulamak için kod girme

1. Metin düzenleyicisinde, **Program.cs**açın.

2. İçeriği aşağıdaki kodla değiştirin ve sunucunuz, veritabanınız, kullanıcı adınız ve parolanız için uygun değerleri ekleyin.

> [!NOTE]
> ADO.NET bağlantı dizesini kullanmak için, koddaki 4 satırı sunucu, veritabanı, kullanıcı adı ve parolayı aşağıdaki satırla değiştirin. Dizede, kullanıcı adınızı ve parolanızı ayarlayın.
>
>    `builder.ConnectionString="<your_ado_net_connection_string>";`

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

                builder.DataSource = "<your_server.database.windows.net>"; 
                builder.UserID = "<your_username>";            
                builder.Password = "<your_password>";     
                builder.InitialCatalog = "<your_database>";
         
                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nQuery data example:");
                    Console.WriteLine("=========================================\n");
                    
                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                    sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                    sb.Append("JOIN [SalesLT].[Product] p ");
                    sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                    String sql = sb.ToString();

                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
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
            Console.WriteLine("\nDone. Press enter.");
            Console.ReadLine(); 
        }
    }
}
```

## <a name="run-the-code"></a>Kodu çalıştırma

1. İsteyiş sırasında, aşağıdaki komutları çalıştırın.

   ```cmd
   dotnet restore
   dotnet run
   ```

2. En iyi 20 satırın döndürüldünden doğrulayın.

   ```text
   Query data example:
   =========================================

   Road Frames HL Road Frame - Black, 58
   Road Frames HL Road Frame - Red, 58
   Helmets Sport-100 Helmet, Red
   Helmets Sport-100 Helmet, Black
   Socks Mountain Bike Socks, M
   Socks Mountain Bike Socks, L
   Helmets Sport-100 Helmet, Blue
   Caps AWC Logo Cap
   Jerseys Long-Sleeve Logo Jersey, S
   Jerseys Long-Sleeve Logo Jersey, M
   Jerseys Long-Sleeve Logo Jersey, L
   Jerseys Long-Sleeve Logo Jersey, XL
   Road Frames HL Road Frame - Red, 62
   Road Frames HL Road Frame - Red, 44
   Road Frames HL Road Frame - Red, 48
   Road Frames HL Road Frame - Red, 52
   Road Frames HL Road Frame - Red, 56
   Road Frames LL Road Frame - Black, 58
   Road Frames LL Road Frame - Black, 60
   Road Frames LL Road Frame - Black, 62

   Done. Press enter.
   ```
3. Uygulama penceresini kapatmak için **Enter'u** seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Komut satırını kullanarak Windows/Linus/macOS’ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli).
- [.NET Framework ve Visual Studio'yu kullanarak bir Azure SQL veritabanına nasıl bağlanıp sorgulandığınızı](sql-database-connect-query-dotnet-visual-studio.md)öğrenin.  
- [SSMS'i kullanarak ilk Azure SQL veritabanınızı](sql-database-design-first-database.md) nasıl tasarlayıp [Azure SQL veritabanı tasarlayıp C# ve ADO.NET'ye nasıl bağlanıştınız](sql-database-design-first-database-csharp.md)öğrenin.
- .NET hakkında daha fazla bilgi edinmek için [.NET belgelerine](https://docs.microsoft.com/dotnet/) bakın.
