---
title: .NET Core kullanarak bir veritabanını bağlama ve sorgulama
description: Bu konu başlığında, .NET Core kullanarak Azure SQL veritabanı veya Azure SQL yönetilen örneği 'nde bir veritabanına bağlanan ve Transact-SQL deyimlerini kullanarak sorgulayan bir program oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2, devx-track-csharp
ms.devlang: dotnet
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 05/29/2020
ms.openlocfilehash: 37cd2051670221b8b78c075f249f633f9f447099
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97674316"
---
# <a name="quickstart-use-net-core-c-to-query-a-database-in-azure-sql-database-or-azure-sql-managed-instance"></a>Hızlı başlangıç: Azure SQL veritabanı veya Azure SQL yönetilen örneği 'nde bir veritabanını sorgulamak için .NET Core (C#) kullanma
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi-asa.md)]

Bu hızlı başlangıçta, bir veritabanına bağlanmak için [.NET Core](https://www.microsoft.com/net/) ve C# Code 'u kullanacaksınız. Daha sonra verileri sorgulamak için bir Transact-SQL ifadesini çalıştıracaksınız.

> [!TIP]
> Aşağıdaki Microsoft Learn modülü, [Azure SQL veritabanında bir veritabanını sorgulayan bir ASP.NET uygulamasının nasıl geliştirileceği ve yapılandırılacağı](/learn/modules/develop-app-that-queries-azure-sql/) hakkında bilgi edinmenize yardımcı olur

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakilere ihtiyacınız vardır:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Bir veritabanı. Bir veritabanı oluşturup yapılandırmak için bu hızlı başlangıçlardan birini kullanabilirsiniz:

  | Eylem | SQL Veritabanı | SQL Yönetilen Örnek | Azure VM’lerde SQL Server | Azure Synapse Analytics |
  |:--- |:--- |:---|:---|:---|
  | Oluştur| [Portal](single-database-create-quickstart.md) | [Portal](../managed-instance/instance-create-quickstart.md) | [Portal](../virtual-machines/windows/sql-vm-create-portal-quickstart.md) | [Portal](/azure/synapse-analytics/quickstart-create-workspace.md) |
  || [CLI](scripts/create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) | | [CLI](/azure/synapse-analytics/quickstart-create-workspace-cli.md) |
  || [PowerShell](scripts/create-and-configure-database-powershell.md) | [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md) | [PowerShell](../virtual-machines/windows/sql-vm-create-powershell-quickstart.md) | [PowerShell](/azure/synapse-analytics/quickstart-create-workspace-powershell.md) |
  || | | [Dağıtım şablonu](/azure/azure-sql/virtual-machines/windows/create-sql-vm-resource-manager-template.md) | [Dağıtım şablonu](/azure/synapse-analytics/quickstart-deployment-template-workspaces.md) | 
  | Yapılandırma | [Sunucu düzeyi IP güvenlik duvarı kuralı](firewall-create-server-level-portal-quickstart.md)| [Bir VM 'den bağlantı](../managed-instance/connect-vm-instance-configure.md)| |
  |||[Şirket içinden bağlantı](../managed-instance/point-to-site-p2s-configure.md) | [SQL Server örneğine bağlanma](../virtual-machines/windows/sql-vm-create-portal-quickstart.md) |
  ||||

- [İşletim sisteminiz için .NET Core](https://www.microsoft.com/net/core) yüklendi.

## <a name="get-server-connection-information"></a>Sunucu bağlantı bilgilerini al

Azure SQL veritabanı 'nda veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, Azure SQL veritabanı 'nda veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya Azure SQL yönetilen örneği için **konak** ' ın YANıNDAKI tam sunucu adını (veya IP adresini) veya Azure VM 'de SQL Server ' yi gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

> [!NOTE]
> Azure VM 'de SQL Server yönelik bağlantı bilgileri için bkz. [SQL Server örneğine bağlanma](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server).

## <a name="get-adonet-connection-information-optional---sql-database-only"></a>ADO.NET bağlantı bilgilerini al (isteğe bağlı-yalnızca SQL veritabanı)

1. Azure portal veritabanı dikey penceresine gidin ve **Ayarlar** altında **bağlantı dizeleri**' ni seçin.

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

1. Bir metin düzenleyicisinde **program.cs** öğesini açın.

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

                    String sql = "SELECT name, collation_name FROM sys.databases";

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

2. Satırların döndürüldüğünden emin olun.

   ```text
   Query data example:
   =========================================

   master   SQL_Latin1_General_CP1_CI_AS
   tempdb   SQL_Latin1_General_CP1_CI_AS
   WideWorldImporters   Latin1_General_100_CI_AS

   Done. Press enter.
   ```

3. Uygulama penceresini kapatmak için **ENTER** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Komut satırını kullanarak Windows/Linus/macOS’ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli).
- [.NET Framework ve Visual Studio kullanarak Azure SQL veritabanı veya Azure SQL yönetilen örneği bağlama ve sorgulama](connect-query-dotnet-visual-studio.md)hakkında bilgi edinin.  
- [SSMS ile ilk veritabanınızı tasarlamayı](design-first-database-tutorial.md) veya [bir veritabanı tasarlamanızı ve C# ve ADO.NET ile nasıl bağlanacağınızı](design-first-database-csharp-tutorial.md)öğrenin.
- .NET hakkında daha fazla bilgi edinmek için [.NET belgelerine](/dotnet/) bakın.
