---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 12/10/2018
ms.author: genemi
ms.openlocfilehash: e30651cb0ed7d74082163a92acbc428c21018255
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67188449"
---
## <a name="c-program-example"></a>C# program örneği

Bu makalenin sonraki bölümleri, Transact-SQL (T-SQL) deyimlerini SQL veritabanına göndermek için ADO.NET kullanan bir C# programı sunar. C# programı aşağıdaki eylemleri gösterir:

- [ADO.NET kullanarak SQL veritabanına bağlanın](#cs_1_connect)
- [T-SQL deyimlerini döndüren yöntemler](#cs_2_return)
    - Tablo oluşturma
    - Tabloları verilerle doldurma
    - Verileri güncelleştirme, silme ve seçme
- [T-SQL'i veritabanına gönderme](#cs_3_submit)

### <a name="entity-relationship-diagram-erd"></a>Varlık İlişki Diyagramı (ERD)

İfadeler, `CREATE TABLE` iki tablo arasında *yabancı anahtar* (FK) ilişkisi oluşturmak için **REFERENCES** anahtar sözcüklerini içerir. *Tempdb*kullanıyorsanız, önde gelen `--REFERENCES` tireler çiftini kullanarak anahtar kelimeyi yorumla.

ERD, iki tablo arasındaki ilişkiyi görüntüler. **sekmeEmployee.DepartmentCode** *alt* sütunundaki **değerler, sekmeDepartment.DepartmentCode** *üst* sütunundaki değerlerle sınırlıdır.

![Yabancı anahtarı gösteren ERD](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)

> [!NOTE]
> Tablo adlarına bir satır aralığı `#` eklemek için T-SQL'i düzenleme seçeneğiniz vardır ve bu da bunları *tempdb'de*geçici tablo lar olarak oluşturur. Bu, hiçbir test veritabanı olmadığında, gösteri amaçları için yararlıdır. Yabancı anahtarlara yapılan herhangi bir başvuru kullanımları sırasında zorlanmaz ve program çalışmaya başladığında bağlantı kapandığında geçici tablolar otomatik olarak silinir.

### <a name="to-compile-and-run"></a>Derlemek ve çalıştırmak için

C# programı mantıksal olarak bir .cs dosyasıdır ve her bloğun anlaşılmasını kolaylaştırmak için fiziksel olarak birkaç kod bloğuna ayrılmıştır. Programı derlemek ve çalıştırmak için aşağıdaki adımları yapın:

1. Visual Studio'da bir C# projesi oluşturun. Proje türü, **Visual** > **C#** > **Windows Masaüstü** > **Konsol Uygulaması (.NET Framework)** altında bulunan bir *Konsol*olmalıdır.

1. Dosya *Program.cs,* kod başlangıç satırlarını aşağıdaki adımlarla değiştirin:

    1. Aşağıdaki kod bloklarını kopyalayıp yapıştırın, sunuldukları sırayla, [veritabanına bağlan,](#cs_1_connect) [T-SQL oluştur](#cs_2_return)ve [veritabanına gönder'e](#cs_3_submit)bakın.

    1. Yöntemde aşağıdaki değerleri `Main` değiştirin:

        - *Cb. Datasource*
        - *Cb. Userıd*
        - *Cb. Parola*
        - *Cb. Başlangıç Kataloğu*

1. Assembly *System.Data.dll'ye* başvurulacağını doğrulayın. Doğrulamak için **Çözüm Gezgini** bölmesindeki **Başvuru** düğümlerini genişletin.

1. Programı Visual Studio'dan oluşturmak ve çalıştırmak için **Başlat** düğmesini seçin. Guid değerleri test çalıştırmaları arasında farklılık gösterse de rapor çıktısı bir program penceresinde görüntülenir.

    ```Output
    =================================
    T-SQL to 2 - Create-Tables...
    -1 = rows affected.

    =================================
    T-SQL to 3 - Inserts...
    8 = rows affected.

    =================================
    T-SQL to 4 - Update-Join...
    2 = rows affected.

    =================================
    T-SQL to 5 - Delete-Join...
    2 = rows affected.

    =================================
    Now, SelectEmployees (6)...
    8ddeb8f5-9584-4afe-b7ef-d6bdca02bd35 , Alison , 20 , acct , Accounting
    9ce11981-e674-42f7-928b-6cc004079b03 , Barbara , 17 , hres , Human Resources
    315f5230-ec94-4edd-9b1c-dd45fbb61ee7 , Carol , 22 , acct , Accounting
    fcf4840a-8be3-43f7-a319-52304bf0f48d , Elle , 15 , NULL , NULL
    View the report output here, then press any key to end the program...
    ```

<a name="cs_1_connect"/>

### <a name="connect-to-sql-database-using-adonet"></a>ADO.NET kullanarak SQL veritabanına bağlanın

```csharp
using System;
using System.Data.SqlClient;   // System.Data.dll
//using System.Data;           // For:  SqlDbType , ParameterDirection

namespace csharp_db_test
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                var cb = new SqlConnectionStringBuilder();
                cb.DataSource = "your_server.database.windows.net";
                cb.UserID = "your_user";
                cb.Password = "your_password";
                cb.InitialCatalog = "your_database";

                using (var connection = new SqlConnection(cb.ConnectionString))
                {
                    connection.Open();

                    Submit_Tsql_NonQuery(connection, "2 - Create-Tables", Build_2_Tsql_CreateTables());

                    Submit_Tsql_NonQuery(connection, "3 - Inserts", Build_3_Tsql_Inserts());

                    Submit_Tsql_NonQuery(connection, "4 - Update-Join", Build_4_Tsql_UpdateJoin(),
                        "@csharpParmDepartmentName", "Accounting");

                    Submit_Tsql_NonQuery(connection, "5 - Delete-Join", Build_5_Tsql_DeleteJoin(),
                        "@csharpParmDepartmentName", "Legal");

                    Submit_6_Tsql_SelectEmployees(connection);
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }

            Console.WriteLine("View the report output here, then press any key to end the program...");
            Console.ReadKey();
        }
```

<a name="cs_2_return"/>

### <a name="methods-that-return-t-sql-statements"></a>T-SQL deyimlerini döndüren yöntemler

```csharp
static string Build_2_Tsql_CreateTables()
{
    return @"
        DROP TABLE IF EXISTS tabEmployee;
        DROP TABLE IF EXISTS tabDepartment;  -- Drop parent table last.

        CREATE TABLE tabDepartment
        (
            DepartmentCode  nchar(4)          not null    PRIMARY KEY,
            DepartmentName  nvarchar(128)     not null
        );

        CREATE TABLE tabEmployee
        (
            EmployeeGuid    uniqueIdentifier  not null  default NewId()    PRIMARY KEY,
            EmployeeName    nvarchar(128)     not null,
            EmployeeLevel   int               not null,
            DepartmentCode  nchar(4)              null
            REFERENCES tabDepartment (DepartmentCode)  -- (REFERENCES would be disallowed on temporary tables.)
        );
    ";
}

static string Build_3_Tsql_Inserts()
{
    return @"
        -- The company has these departments.
        INSERT INTO tabDepartment (DepartmentCode, DepartmentName)
        VALUES
            ('acct', 'Accounting'),
            ('hres', 'Human Resources'),
            ('legl', 'Legal');

        -- The company has these employees, each in one department.
        INSERT INTO tabEmployee (EmployeeName, EmployeeLevel, DepartmentCode)
        VALUES
            ('Alison'  , 19, 'acct'),
            ('Barbara' , 17, 'hres'),
            ('Carol'   , 21, 'acct'),
            ('Deborah' , 24, 'legl'),
            ('Elle'    , 15, null);
    ";
}

static string Build_4_Tsql_UpdateJoin()
{
    return @"
        DECLARE @DName1  nvarchar(128) = @csharpParmDepartmentName;  --'Accounting';

        -- Promote everyone in one department (see @parm...).
        UPDATE empl
        SET
            empl.EmployeeLevel += 1
        FROM
            tabEmployee   as empl
        INNER JOIN
            tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
        WHERE
            dept.DepartmentName = @DName1;
    ";
}

static string Build_5_Tsql_DeleteJoin()
{
    return @"
        DECLARE @DName2  nvarchar(128);
        SET @DName2 = @csharpParmDepartmentName;  --'Legal';

        -- Right size the Legal department.
        DELETE empl
        FROM
            tabEmployee   as empl
        INNER JOIN
            tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
        WHERE
            dept.DepartmentName = @DName2

        -- Disband the Legal department.
        DELETE tabDepartment
            WHERE DepartmentName = @DName2;
    ";
}

static string Build_6_Tsql_SelectEmployees()
{
    return @"
        -- Look at all the final Employees.
        SELECT
            empl.EmployeeGuid,
            empl.EmployeeName,
            empl.EmployeeLevel,
            empl.DepartmentCode,
            dept.DepartmentName
        FROM
            tabEmployee   as empl
        LEFT OUTER JOIN
            tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
        ORDER BY
            EmployeeName;
    ";
}
```

<a name="cs_3_submit"/>

### <a name="submit-t-sql-to-the-database"></a>T-SQL'i veritabanına gönderme

```csharp
static void Submit_6_Tsql_SelectEmployees(SqlConnection connection)
{
    Console.WriteLine();
    Console.WriteLine("=================================");
    Console.WriteLine("Now, SelectEmployees (6)...");

    string tsql = Build_6_Tsql_SelectEmployees();

    using (var command = new SqlCommand(tsql, connection))
    {
        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                Console.WriteLine("{0} , {1} , {2} , {3} , {4}",
                    reader.GetGuid(0),
                    reader.GetString(1),
                    reader.GetInt32(2),
                    (reader.IsDBNull(3)) ? "NULL" : reader.GetString(3),
                    (reader.IsDBNull(4)) ? "NULL" : reader.GetString(4));
            }
        }
    }
}

static void Submit_Tsql_NonQuery(
    SqlConnection connection,
    string tsqlPurpose,
    string tsqlSourceCode,
    string parameterName = null,
    string parameterValue = null
    )
{
    Console.WriteLine();
    Console.WriteLine("=================================");
    Console.WriteLine("T-SQL to {0}...", tsqlPurpose);

    using (var command = new SqlCommand(tsqlSourceCode, connection))
    {
        if (parameterName != null)
        {
            command.Parameters.AddWithValue(  // Or, use SqlParameter class.
                parameterName,
                parameterValue);
        }
        int rowsAffected = command.ExecuteNonQuery();
        Console.WriteLine(rowsAffected + " = rows affected.");
    }
}
} // EndOfClass
}
```