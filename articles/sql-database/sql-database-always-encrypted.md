---
title: Her Zaman Şifreli - Windows sertifika mağazası
description: Bu makalede, SQL Server Management Studio 'da (SSMS) Her Zaman Şifrelenmiş Sihirbazı'nı kullanarak veritabanı şifrelemesi ile bir SQL veritabanındaki hassas verilerin nasıl güvende olduğu gösterilmektedir. Ayrıca, şifreleme anahtarlarınızı Windows sertifika mağazasında nasıl depoladığınızı da gösterir.
keywords: şifreleme verileri, sql şifreleme, veritabanı şifreleme, hassas veri, Her zaman Şifreli
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviwer: ''
ms.date: 03/08/2019
ms.openlocfilehash: 82c3c3274a8a9d66019ce906ee7be47cedac7470
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822053"
---
# <a name="always-encrypted-protect-sensitive-data-and-store-encryption-keys-in-the-windows-certificate-store"></a>Her Zaman Şifrelenmiş: Hassas verileri koruyun ve şifreleme anahtarlarını Windows sertifika deposunda saklayın

Bu makalede, SQL Server Management Studio 'da [(SSMS)](https://msdn.microsoft.com/library/hh213248.aspx) [Her Zaman Şifrelenmiş](https://msdn.microsoft.com/library/mt459280.aspx) Sihirbazı'nı kullanarak veritabanı şifrelemesi ile bir SQL veritabanındaki hassas verilerin nasıl güvende olduğu gösterilmektedir. Ayrıca, şifreleme anahtarlarınızı Windows sertifika mağazasında nasıl depoladığınızı da gösterir.

Her Zaman Şifrelenmiş, Azure SQL Veritabanı ve SQL Server'da, istemci ve sunucu arasındaki hareket sırasında ve veriler kullanımdayken, hassas verilerin hiçbir zaman veritabanı sistemi içinde düz metin. Verileri şifreledikten sonra, yalnızca istemci uygulamaları veya anahtarlara erişimi olan uygulama sunucuları düz metin verilerine erişebilir. Ayrıntılı bilgi için her [zaman şifrelenmiş (Veritabanı Motoru)](https://msdn.microsoft.com/library/mt163865.aspx)bakın.

Veritabanını Her Zaman Şifrelenmiş olarak yapacak şekilde yapılandırdıktan sonra, şifrelenmiş verilerle çalışmak için Visual Studio ile C# bir istemci uygulaması oluşturacaksınız.

Azure SQL veritabanı için Her Zaman Şifrelenmiş'i nasıl ayarlayizleyeceğinizi öğrenmek için bu makaledeki adımları izleyin. Bu makalede, aşağıdaki görevleri nasıl gerçekleştireceğinizi öğreneceksiniz:

* Her Zaman [Şifrelenmiş Anahtarları](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)oluşturmak için SSMS'te Her Zaman Şifrelenmiş sihirbazı kullanın.
  * Sütun [Ana Anahtar (CMK)](https://msdn.microsoft.com/library/mt146393.aspx)oluşturun.
  * Sütun [Şifreleme Anahtarı (CEK)](https://msdn.microsoft.com/library/mt146372.aspx)oluşturun.
* Veritabanı tablosu oluşturun ve sütunları şifreleyin.
* Şifrelenmiş sütunlardan veri ekleyen, seçen ve görüntüleyen bir uygulama oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici için şunları yapmanız gerekir:

* Bir Azure hesabı ve aboneliği Eğer yoksa, [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)için kaydolun.
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) sürümü 13.0.700.242 veya sonrası.
* [.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) veya sonraki (istemci bilgisayarda).
* [Görsel Stüdyo](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).

## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Kaynak > **Veri + Depolama** > SQL Veritabanı **Oluştur'u**tıklatın.**SQL Database**
3. Yeni veya varolan **bir** sunucuda **Klinik** adında boş bir veritabanı oluşturun. Azure portalında veritabanı oluşturma yla ilgili ayrıntılı talimatlar için [ilk Azure SQL veritabanınıza](sql-database-single-database-get-started.md)bakın.

    ![Boş veritabanı oluşturma](./media/sql-database-always-encrypted/create-database.png)

Daha sonra öğreticide bağlantı dizesi gerekir. Veritabanı oluşturulduktan sonra, yeni Klinik veritabanına gidin ve bağlantı dizesini kopyalayın. Bağlantı dizesini istediğiniz zaman alabilirsiniz, ancak Azure portalındayken kolayca kopyalayabilirsiniz.

1. **SQL veritabanları** > **Clinic** > **Veritabanı bağlantı dizelerini göster'i**tıklatın.
2. Bağlantı dizesini **ADO.NET**için kopyalayın.

    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a>SSMS ile veritabanına bağlanma

SSMS'i açın ve Klinik veritabanıyla sunucuya bağlanın.

1. SSMS’i açın. (Açık değilse **Sunucuya Bağlan** penceresini açmak için**Veritabanı Altyapısını** **Bağla'yı** > tıklatın).
2. Sunucu adınızı ve kimlik bilgilerinizi girin. Sunucu adı SQL veritabanı bıçak ve daha önce kopyalanan bağlantı dize bulunabilir. *database.windows.net*dahil olmak üzere tam sunucu adını yazın.

    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted/ssms-connect.png)

Yeni **Güvenlik Duvarı Kuralı** penceresi açılırsa, Azure'da oturum açın ve SSMS'in sizin için yeni bir güvenlik duvarı kuralı oluşturmasına izin verin.

## <a name="create-a-table"></a>Bir tablo oluşturma

Bu bölümde, hasta verilerini tutmak için bir tablo oluşturacaksınız. Bu başlangıçta normal bir tablo olacak-- sonraki bölümde şifrelemeyi yapılandıracaksınız.

1. **Veritabanlarını**genişletin.
2. **Klinik** veritabanına sağ tıklayın ve **Yeni Sorgu'yu**tıklatın.
3. Aşağıdaki İşlem-SQL'i (T-SQL) yeni sorgu penceresine yapıştırın ve **çalıştırın.**

        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO

## <a name="encrypt-columns-configure-always-encrypted"></a>Sütunları şifreleme (Her Zaman Şifrelenmiş yapılandırın)

SSMS, cmk, CEK ve şifrelenmiş sütunları sizin için ayarlayarak Her Zaman Şifrelenmiş'i kolayca yapılandırmak için bir sihirbaz sağlar.

1. **Veritabanları** > **Klinik** > **Tablolar**genişletin.
2. **Hastalar** tablosuna sağ tıklayın ve Her Zaman Şifrelenen sihirbazı açmak için **Sütunları** Şifrele'yi seçin:

    ![Sütunları şifreleme](./media/sql-database-always-encrypted/encrypt-columns.png)

Her Zaman Şifrelenmiş sihirbaz aşağıdaki bölümleri içerir: **Sütun Seçimi,** **Ana Anahtar Yapılandırması** (CMK), **Doğrulama**ve **Özet.**

### <a name="column-selection"></a>Sütun Seçimi

**Sütun Seçimi** sayfasını açmak için **Giriş** sayfasında **İleri'yi** tıklatın. Bu sayfada, hangi sütunları şifrelemek istediğinizi, [şifreleme türünü ve hangi sütun şifreleme anahtarını (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) kullanacağınızı seçeceksiniz.

Her hasta için **SSN** ve **Doğum Tarihi** bilgilerini şifreleyin. **SSN** sütununda, eşitlik aramalarını, birleşimlerini ve gruplandırmayı destekleyen deterministik şifreleme kullanılır. **BirthDate** sütunu, işlemleri desteklemeyen randomize şifreleme kullanır.

**SSN** sütunu için **Şifreleme Türünü** **Deterministic** ve **BirthDate** sütununa **Randomize**olarak ayarlayın. **İleri**'ye tıklayın.

![Sütunları şifreleme](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Ana Anahtar Yapılandırması

**Ana Anahtar Yapılandırma** sayfası, CMK'nızı ayarladığınız ve CMK'nın depolanacak anahtar deposu sağlayıcısını seçtiğiniz yerdir. Şu anda, Windows sertifika mağazasında, Azure Anahtar Kasasında veya donanım güvenlik modülünde (HSM) bir CMK depolayabilirsiniz. Bu öğretici, anahtarlarınızı Windows sertifika mağazasında nasıl depolarınızı gösterir.

Windows **sertifika deposunun** seçildiğini doğrulayın ve **İleri'yi**tıklatın.

![Ana anahtar yapılandırması](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a>Doğrulama

Sütunları şimdi şifreleyebilir veya daha sonra çalışacak bir PowerShell komut dosyası kaydedebilirsiniz. Bu öğretici **için, şimdi bitirmek için Devam et'i** seçin ve **İleri'yi**tıklatın.

### <a name="summary"></a>Özet

Ayarların tümünde doğru olduğundan doğrulayın ve Her Zaman Şifrelenmiş'in kurulumlarını tamamlamak için **Bitir'i** tıklatın.

![Özet](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-the-wizards-actions"></a>Sihirbazın hareketlerini doğrulama

Sihirbaz bittikten sonra veritabanınız Her Zaman Şifrelenmiş olarak ayarlanır. Sihirbaz aşağıdaki eylemleri gerçekleştirdi:

* CmK oluşturuldu.
* Bir CEK oluşturuldu.
* Şifreleme için seçili sütunları yapılandırıldı. **Hastalar** tablonuzşu anda veriye sahip değildir, ancak seçili sütunlarda varolan veriler artık şifrelenir.

SSMS'te anahtarların oluşturulmasını **Klinik** > **Güvenlik** > **Her Zaman Şifrelenmiş Anahtarlar'a**giderek doğrulayabilirsiniz. Artık sihirbazın sizin için oluşturduğu yeni anahtarları görebilirsiniz.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Şifrelenmiş verilerle çalışan bir istemci uygulaması oluşturma

Her Zaman Şifrelenmiş olarak ayarlanın, ekler *gerçekleştiren* ve şifrelenmiş *sütunlarda seçen* bir uygulama oluşturabilirsiniz. Örnek uygulamayı başarıyla çalıştırmak için, her zaman şifrelenmiş sihirbazı çalıştırdığınız aynı bilgisayarda çalıştırmanız gerekir. Uygulamayı başka bir bilgisayarda çalıştırmak için, Her Zaman Şifrelenmiş sertifikalarınızı istemci uygulamasını çalıştıran bilgisayara dağıtmanız gerekir.  

> [!IMPORTANT]
> Uygulamanız, her zaman şifrelenmiş sütunlara sahip düz metin verilerini sunucuya aktarırken [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) nesnelerini kullanmalıdır. SqlParameter nesneleri kullanmadan gerçek değerlerin geçirilmesi bir özel durumla sonuçlanır.

1. Visual Studio'yu açın ve yeni bir C# konsol uygulaması oluşturun. Projenizin **.NET Framework 4.6** veya sonraki olarak ayarlandığından emin olun.
2. Projeyi **AlwaysEncryptedConsoleApp'a** adlandırın ve **Tamam'a**tıklayın.

![Yeni konsol uygulaması](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Her Zaman Şifrelenmiş'i etkinleştirmek için bağlantı dizenizi değiştirin

Bu bölümde, veritabanı bağlantı dizenizde Her Zaman Şifrelenmiş nasıl etkinleştirilir. Bir sonraki bölümde oluşturduğunuz konsol uygulamasını "Her zaman şifrelenmiş örnek konsol uygulaması" olarak değiştireceksiniz.

Her Zaman Şifrelenmiş'i etkinleştirmek **için, sütun şifreleme ayar** anahtar sözcük anahtar sözcüklerini bağlantı dizenize eklemeniz ve **etkin**olarak ayarlamanız gerekir.

Bunu doğrudan bağlantı dizesi olarak ayarlayabilirsiniz veya [sqlconnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx)kullanarak ayarlayabilirsiniz. Sonraki bölümdeki örnek uygulama **SqlConnectionStringBuilder'ın**nasıl kullanılacağını gösterir.

> [!NOTE]
> Bu, Her Zaman Şifrelenmiş'e özgü bir istemci uygulamasında gereken tek değişikliktir. Bağlantı dizesini harici olarak depolayan varolan bir uygulamanız varsa (diğer bir deyişle, bir config dosyasında), herhangi bir kodu değiştirmeden Her Zaman Şifrelenmiş'i etkinleştirebilirsiniz.

### <a name="enable-always-encrypted-in-the-connection-string"></a>Bağlantı dizesinde Her Zaman Şifrelenmiş'i Etkinleştir

Bağlantı dizenize aşağıdaki anahtar kelimeyi ekleyin:

    Column Encryption Setting=Enabled

### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Her Zaman Bir SqlConnectionStringBuilder ile Şifrelenmiş etkinleştirin

Aşağıdaki kod, [SqlConnectionStringBuilder.ColumnEncryptionSetting'i](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [Etkin](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx)Olarak Ayarlayarak Her Zaman Şifrelenmiş'i nasıl etkinleştireceklerini gösterir.

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="always-encrypted-sample-console-application"></a>Her Zaman Şifreli örnek konsol uygulaması

Bu örnek nasıl gösteriş yapılacağını gösterir:

* Her Zaman Şifrelenmiş'i etkinleştirmek için bağlantı dizenizi değiştirin.
* Verileri şifrelenmiş sütunlara ekleyin.
* Şifreli bir sütunda belirli bir değer için filtre uygulayarak bir kayıt seçin.

**Program.cs** içeriğini aşağıdaki kodla değiştirin. Ana yöntemin hemen üstündeki satırdaki global bağlantıString değişkeni için bağlantı dizesini Azure portalından geçerli bağlantı dizenizle değiştirin. Bu kodda yapmanız gereken tek değişiklik budur.

Her Zaman Şifreli'yi iş başında görmek için uygulamayı çalıştırın.

```cs
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Globalization;

namespace AlwaysEncryptedConsoleApp
{
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Data Source = SPE-T640-01.sys-sqlsvr.local; Initial Catalog = Clinic; Integrated Security = true";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();

            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            CultureInfo culture = CultureInfo.CreateSpecificCulture("en-US");
            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993", culture)
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
}
```

## <a name="verify-that-the-data-is-encrypted"></a>Verilerin şifrelenmiş olduğundan doğrulayın

Sunucudaki gerçek verilerin **SSMS** ile Hastalar verilerini sorgulayarak şifrelenerek şifrelenebileceğini hızlı bir şekilde kontrol edebilirsiniz. (Sütun şifreleme ayarı henüz etkinleştirilmediğini n için geçerli bağlantınızı kullanın.)

Aşağıdaki sorguyu Klinik veritabanında çalıştırın.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Şifrelenmiş sütunların düz metin verileri içermediğini görebilirsiniz.

   ![Yeni konsol uygulaması](./media/sql-database-always-encrypted/ssms-encrypted.png)

Düz metin verilerine erişmek için SSMS'i kullanmak için bağlantıya **Sütun Şifreleme Ayarı=etkin** parametreyi ekleyebilirsiniz.

1. SSMS'te, **Object Explorer'da**sunucunuzu sağ tıklatın ve ardından **Bağlantıyı Kesme'yi**tıklatın.
2. **Sunucuya Bağlan** penceresini açmak için**Veritabanı Altyapısını** **Bağla'yı** > tıklatın ve ardından **Seçenekler'i**tıklatın.
3. **Ek Bağlantı Parametreleri'ni** tıklatın ve **sütun şifreleme ayarı yazın=etkin .**

    ![Yeni konsol uygulaması](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. Aşağıdaki sorguyu **Klinik** veritabanında çalıştırın.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Artık düz metin verilerini şifreli sütunlarda görebilirsiniz.

    ![Yeni konsol uygulaması](./media/sql-database-always-encrypted/ssms-plaintext.png)

> [!NOTE]
> Farklı bir bilgisayardan SSMS (veya herhangi bir istemci) ile bağlanırsanız, şifreleme anahtarlarına erişimi olmaz ve verilerin şifresini çözemez.

## <a name="next-steps"></a>Sonraki adımlar

Her Zaman Şifrelenmiş'i kullanan bir veritabanı oluşturduktan sonra aşağıdakileri yapmak isteyebilirsiniz:

* Bu örneği farklı bir bilgisayardan çalıştırın. Şifreleme anahtarlarına erişimi olmayacaktır, bu nedenle düz metin verilerine erişimi olmaz ve başarılı bir şekilde çalışmaz.
* [Anahtarlarınızı döndürün ve temizleyin.](https://msdn.microsoft.com/library/mt607048.aspx)
* [Zaten Her Zaman Şifrelenmiş ile şifrelenmiş verileri geçirin.](https://msdn.microsoft.com/library/mt621539.aspx)
* [Her Zaman Şifrelenmiş sertifikaları diğer istemci makinelere dağıtın](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (bkz. "Sertifikaları Uygulamalar ve Kullanıcılar için Kullanılabilir Hale Getirme" bölümüne bakın).

## <a name="related-information"></a>İlgili bilgiler

* [Her Zaman Şifrelenmiş (istemci geliştirme)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Şeffaf Veri Şifreleme](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Sunucu Şifreleme](https://msdn.microsoft.com/library/bb510663.aspx)
* [Her Zaman Şifrelenmiş Sihirbaz](https://msdn.microsoft.com/library/mt459280.aspx)
* [Her zaman Şifreli Blog](https://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)