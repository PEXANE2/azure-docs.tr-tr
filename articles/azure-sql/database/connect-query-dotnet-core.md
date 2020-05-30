---
title: Sorgulamak için .NET Core kullanın
description: Bu konu başlığı altında, .NET Core kullanarak Azure SQL veritabanı 'nda bir veritabanına bağlanan ve Transact-SQL deyimlerini kullanarak sorgulayan bir program oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2
ms.devlang: dotnet
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 07/29/2019
ms.openlocfilehash: ff90cf7de3bb83a235f866d8035ccb036e021b83
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84189571"
---
# <a name="quickstart-use-net-core-c-to-query-a-database-in-azure-sql-database"></a>Hızlı başlangıç: Azure SQL veritabanı 'nda bir veritabanını sorgulamak için .NET Core (C#) kullanma
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu hızlı başlangıçta, Azure SQL veritabanı 'nda bir veritabanına bağlanmak için [.NET Core](https://www.microsoft.com/net/) ve C# Code 'u kullanacaksınız. Daha sonra verileri sorgulamak için bir Transact-SQL ifadesini çalıştıracaksınız.

> [!TIP]
> Aşağıdaki Microsoft Learn modülü, [Azure SQL veritabanında bir veritabanını sorgulayan bir ASP.NET uygulamasının nasıl geliştirileceği ve yapılandırılacağı](https://docs.microsoft.com/learn/modules/develop-app-that-queries-azure-sql/) hakkında bilgi edinmenize yardımcı olur

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Azure SQL veritabanı 'nda bir veritabanı. Azure SQL veritabanı 'nda bir veritabanı oluşturmak ve yapılandırmak için bu hızlı başlangıçlardan birini kullanabilirsiniz:

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

  > [!IMPORTANT]
  > Bu makaledeki betikler, Adventure Works veritabanını kullanmak için yazılmıştır. SQL yönetilen örneği ile, Adventure Works veritabanını bir örnek veritabanına aktarmanız veya bu makaledeki betikleri Wide World Importers veritabanını kullanacak şekilde değiştirmeniz gerekir.

- [İşletim sisteminiz için .NET Core](https://www.microsoft.com/net/core) yüklendi.

> [!NOTE]
> Bu hızlı başlangıç, *Mysampledatabase* veritabanını kullanır. Farklı bir veritabanı kullanmak istiyorsanız, veritabanı başvurularını değiştirmeniz ve `SELECT` C# kodundaki sorguyu değiştirmeniz gerekecektir.

## <a name="get-server-connection-information"></a>Sunucu bağlantı bilgilerini al

Azure SQL veritabanı 'nda veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, Azure SQL veritabanı 'nda veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya Azure SQL yönetilen örneği için **konak** ' ın YANıNDAKI tam sunucu adını (veya IP adresini) veya Azure VM 'de SQL Server ' yi gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

> [!NOTE]
> Azure VM 'de SQL Server yönelik bağlantı bilgileri için bkz. [SQL Server örneğine bağlanma](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server).

## <a name="get-adonet-connection-information-optional---sql-database-only"></a>ADO.NET bağlantı bilgilerini al (isteğe bağlı-yalnızca SQL veritabanı)

1. **Mysampledatabase** sayfasına gidin ve **Ayarlar**altında **bağlantı dizeleri**' ni seçin.

2. Tam **ADO.NET** bağlantı dizesini gözden geçirin.

    ![ADO.NET bağlantı dizesi](./media/connect-query-dotnet-core/adonet-connection-string2.png)

3. Kullanmak istiyorsanız **ADO.net** bağlantı dizesini kopyalayın.
  
## <a name="create-a-new-net-core-project"></a>Yeni bir .NET Core projesi oluştur

1. Komut istemini açın ve **sqltest** adlı bir klasör oluşturun. Bu klasöre gidin ve bu komutu çalıştırın.

    ```cmd
    dotnet new console
    ```

    Bu komut, bir ilk C# kod dosyası (**program.cs**), bir XML yapılandırma dosyası (**SQLtest. csproj**) ve gerekli ikili dosyalar dahil olmak üzere yeni uygulama proje dosyaları oluşturur.

2. Bir metin düzenleyicisinde **SQLtest. csproj** dosyasını açın ve aşağıdaki XML 'i Etiketler arasına yapıştırın `<Project>` . Bu XML `System.Data.SqlClient` bir bağımlılık olarak ekler.

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.6.0" />
    </ItemGroup>
    ```

## <a name="insert-code-to-query-the-database-in-azure-sql-database"></a>Azure SQL veritabanı 'nda veritabanını sorgulamak için kod ekleme

1. Bir metin düzenleyicisinde **program.cs**öğesini açın.

2. İçeriği aşağıdaki kodla değiştirin ve sunucunuz, veritabanınız, Kullanıcı adınız ve parolanız için uygun değerleri ekleyin.

> [!NOTE]
> Bir ADO.NET bağlantı dizesi kullanmak için, sunucu, veritabanı, Kullanıcı adı ve parolayı aşağıdaki satırla değiştirmek üzere koddaki 4 satırı değiştirin. Dizesinde, Kullanıcı adınızı ve parolanızı ayarlayın.
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

1. Komut isteminde aşağıdaki komutları çalıştırın.

   ```cmd
   dotnet restore
   dotnet run
   ```

2. İlk 20 satırın döndürüldüğünden emin olun.

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

3. Uygulama penceresini kapatmak için **ENTER** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Komut satırını kullanarak Windows/Linus/macOS’ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli).
- [.NET Framework ve Visual Studio kullanarak Azure SQL veritabanı 'nda bir veritabanını bağlamayı ve sorgulamayı](connect-query-dotnet-visual-studio.md)öğrenin.  
- [Azure SQL veritabanı 'NDA SSMS kullanarak ilk veritabanınızı tasarlamayı](design-first-database-tutorial.md) veya [Azure SQL veritabanı 'Nda bir veritabanı tasarlayarak C# ve ADO.NET ile bağlanma](design-first-database-csharp-tutorial.md)hakkında bilgi edinin.
- .NET hakkında daha fazla bilgi edinmek için [.NET belgelerine](https://docs.microsoft.com/dotnet/) bakın.
