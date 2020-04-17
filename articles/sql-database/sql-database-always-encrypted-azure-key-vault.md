---
title: Her Zaman Şifrelenmiş - Azure Anahtar Kasası
description: Bu makalede, SQL Server Management Studio'daki Her Zaman Şifrelenmiş Sihirbazı'nı kullanarak veri şifrelemesi ile bir SQL veritabanında hassas verilerin nasıl güvende oluncayabildiğiniz gösterilmektedir.
keywords: veri şifreleme, şifreleme anahtarı, bulut şifreleme
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: f1d08581c5d29fc41fb33541d766af7cece88cdc
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81451678"
---
# <a name="always-encrypted-protect-sensitive-data-and-store-encryption-keys-in-azure-key-vault"></a>Her Zaman Şifrelenmiş: Azure Key Vault'ta hassas verileri koruyun ve şifreleme anahtarlarını saklayın

Bu makalede, SQL Server Management Studio 'daki [(SSMS)](https://msdn.microsoft.com/library/hh213248.aspx) [Her Zaman Şifrelenmiş](https://msdn.microsoft.com/library/mt459280.aspx) Sihirbazı'nı kullanarak veri şifrelemesi ile bir SQL veritabanındaki hassas verilerin nasıl güvende olduğu gösterilmektedir. Ayrıca, her şifreleme anahtarını Azure Key Vault'ta nasıl depoladığınızı gösteren yönergeler de içerir.

Her Zaman Şifrelenmiş, Azure SQL Veritabanı ve SQL Server'da, istemci ve sunucu arasındaki hareket sırasında ve veriler kullanımdayken hassas verilerin sunucuda dinlenmesinde korunmasına yardımcı olan yeni bir veri şifreleme teknolojisidir. Her Zaman Şifrelenmiş, hassas verilerin veritabanı sistemi içinde hiçbir zaman düz metin olarak görünmemesini sağlar. Veri şifrelemesini yapılandırdıktan sonra, yalnızca istemci uygulamaları veya anahtarlara erişimi olan uygulama sunucuları düz metin verilerine erişebilir. Ayrıntılı bilgi için her [zaman şifrelenmiş (Veritabanı Motoru)](https://msdn.microsoft.com/library/mt163865.aspx)bakın.

Veritabanını Her Zaman Şifrelenmiş olarak yapılandırdıktan sonra, şifrelenmiş verilerle çalışmak için Visual Studio ile C# bir istemci uygulaması oluşturursunuz.

Bu makaledeki adımları izleyin ve Bir Azure SQL veritabanı için Her Zaman Şifrelenmiş'i nasıl ayarlayizleyeceğinizi öğrenin. Bu makalede, aşağıdaki görevleri nasıl gerçekleştireceğinizi öğreneceksiniz:

- Her Zaman [Şifrelenmiş anahtarları](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)oluşturmak için SSMS'te Her Zaman Şifrelenmiş sihirbazı kullanın.
  - Sütun [ana anahtarı (CMK)](https://msdn.microsoft.com/library/mt146393.aspx)oluşturun.
  - Sütun [şifreleme anahtarı (CEK)](https://msdn.microsoft.com/library/mt146372.aspx)oluşturun.
- Veritabanı tablosu oluşturun ve sütunları şifreleyin.
- Şifrelenmiş sütunlardan veri ekleyen, seçen ve görüntüleyen bir uygulama oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici için şunları yapmanız gerekir:

- Bir Azure hesabı ve aboneliği Eğer yoksa, [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)için kaydolun.
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) sürümü 13.0.700.242 veya sonrası.
- [.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) veya sonraki (istemci bilgisayarda).
- [Görsel Stüdyo](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
- [Azure PowerShell](/powershell/azure/overview) veya [Azure CLI](/cli/azure/install-azure-cli)

## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>SQL Veritabanı hizmetine erişmek için istemci uygulamanızı etkinleştirin

Bir Azure Active Directory (AAD) uygulaması ayarlayarak ve *uygulamanızın* kimliğini doğrulamak için gereken Uygulama Kimliği ve *anahtarını* kopyalayarak istemci uygulamanızın SQL Veritabanı hizmetine erişmesini sağlamanız gerekir.

*Uygulama Kimliği* ve *anahtarı*almak için, [kaynaklara erişebilen bir Azure Active Directory uygulaması ve hizmet ilkesi oluşturma](../active-directory/develop/howto-create-service-principal-portal.md)adımlarını izleyin.

## <a name="create-a-key-vault-to-store-your-keys"></a>Anahtarlarınızı saklamak için bir anahtar kasası oluşturun

Artık istemci uygulamanız yapılandırıldığına ve uygulama kimliğinize sahip olduğunuza göre, sizin ve uygulamanızın kasanın sırlarına (Her Zaman Şifrelenmiş anahtarlar) erişebilmeniz için önemli bir kasa oluşturma ve erişim politikasını yapılandırma nın zamanı gelmiştir. Yeni bir sütun ana anahtarı *oluşturmak*ve SQL Server Management Studio ile şifreleme kurmak için oluşturma , *al*, *liste*, *işaret*, *doğrulama*, *wrapKey*ve *unwrapKey* izinleri gereklidir.

Aşağıdaki komut dosyasını çalıştırarak hızlı bir şekilde bir anahtar kasası oluşturabilirsiniz. Bu komutların ayrıntılı bir açıklaması ve anahtar kasası oluşturma ve yapılandırma hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](../key-vault/general/overview.md)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi (RM) modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirmeler Az.Sql modülü içindir. AzureRM modülü en az Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecektir.  Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumlulukları hakkında daha fazla bilgi için [yeni Azure PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

```powershell
$subscriptionName = '<subscriptionName>'
$userPrincipalName = '<username@domain.com>'
$applicationId = '<applicationId from AAD application>'
$resourceGroupName = '<resourceGroupName>' # use the same resource group name when creating your SQL Database below
$location = '<datacenterLocation>'
$vaultName = '<vaultName>'

Connect-AzAccount
$subscriptionId = (Get-AzSubscription -SubscriptionName $subscriptionName).Id
Set-AzContext -SubscriptionId $subscriptionId

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

Set-AzKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
Set-AzKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $applicationId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
$subscriptionName = '<subscriptionName>'
$userPrincipalName = '<username@domain.com>'
$applicationId = '<applicationId from AAD application>'
$resourceGroupName = '<resourceGroupName>' # use the same resource group name when creating your SQL Database below
$location = '<datacenterLocation>'
$vaultName = '<vaultName>'

az login
az account set --subscription $subscriptionName

az group create --location $location --name $resourceGroupName

az keyvault create --name $vaultName --resource-group $resourceGroupName --location $location

az keyvault set-policy --name $vaultName --key-permissions create, get, list, sign, unwrapKey, verify, wrapKey --resource-group $resourceGroupName --upn $userPrincipalName
az keyvault set-policy --name $vaultName --key-permissions get, list, sign, unwrapKey, verify, wrapKey --resource-group $resourceGroupName --spn $applicationId
```

* * *

## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Kaynak** > **Veritabanı** > Oluşturma**SQL Veritabanı'na**gidin.
3. Yeni veya varolan **bir** sunucuda **Klinik** adında boş bir veritabanı oluşturun. Azure portalında veritabanı oluşturma hakkında ayrıntılı yol tarifi için [ilk Azure SQL veritabanınıza](sql-database-single-database-get-started.md)bakın.

    ![Boş veritabanı oluşturma](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Daha sonra öğreticide bağlantı dizesi gerekir, böylece veritabanını oluşturduktan sonra yeni Klinik veritabanına göz atın ve bağlantı dizesini kopyalayın. Bağlantı dizesini istediğiniz zaman alabilirsiniz, ancak Azure portalında kolayca kopyalayabilirsiniz.

1. SQL **veritabanlarına** > gidin**Klinole** > **Veritabanı bağlantı dizelerini göster.**
2. Bağlantı dizesini **ADO.NET**için kopyalayın.

    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a>SSMS ile veritabanına bağlanma

SSMS'i açın ve Klinik veritabanıyla sunucuya bağlanın.

1. SSMS’i açın. (Açık değilse **Sunucuya Bağlan** penceresini açmak için Veritabanını **Bağla** > **Altyapısına** gidin.)

2. Sunucu adınızı ve kimlik bilgilerinizi girin. Sunucu adı SQL veritabanı bıçak ve daha önce kopyalanan bağlantı dize bulunabilir. *database.windows.net*da dahil olmak üzere sunucu adının tamamını yazın.

    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Yeni **Güvenlik Duvarı Kuralı** penceresi açılırsa, Azure'da oturum açın ve SSMS'in sizin için yeni bir güvenlik duvarı kuralı oluşturmasına izin verin.

## <a name="create-a-table"></a>Bir tablo oluşturma

Bu bölümde, hasta verilerini tutmak için bir tablo oluşturacaksınız. Başlangıçta şifrelenmemiştir-- bir sonraki bölümde şifrelemeyi yapılandıracak.

1. **Veritabanlarını**genişletin.
2. **Klinik** veritabanına sağ tıklayın ve **Yeni Sorgu'yu**tıklatın.
3. Aşağıdaki İşlem-SQL'i (T-SQL) yeni sorgu penceresine yapıştırın ve **çalıştırın.**

```sql
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
```

## <a name="encrypt-columns-configure-always-encrypted"></a>Sütunları şifreleme (Her Zaman Şifrelenmiş yapılandırın)

SSMS, sütun ana anahtarını, sütun şifreleme anahtarını ve şifrelenmiş sütunları sizin için ayarlayarak Her Zaman Şifrelenmiş'i kolayca yapılandırmanıza yardımcı olan bir sihirbaz sağlar.

1. **Veritabanları** > **Klinik** > **Tablolar**genişletin.
2. **Hastalar** tablosuna sağ tıklayın ve Her Zaman Şifrelenen sihirbazı açmak için **Sütunları** Şifrele'yi seçin:

    ![Sütunları şifreleme](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

Her Zaman Şifrelenmiş sihirbaz aşağıdaki bölümleri içerir: **Sütun Seçimi**, **Ana Anahtar Yapılandırması**, **Doğrulama**ve **Özet**.

### <a name="column-selection"></a>Sütun Seçimi

**Sütun Seçimi** sayfasını açmak için **Giriş** sayfasında **İleri'yi** tıklatın. Bu sayfada, hangi sütunları şifrelemek istediğinizi, [şifreleme türünü ve hangi sütun şifreleme anahtarını (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) kullanacağınızı seçeceksiniz.

Her hasta için **SSN** ve **Doğum Tarihi** bilgilerini şifreleyin. SSN sütununda, eşitlik aramalarını, birleşimlerini ve gruplandırmayı destekleyen deterministik şifreleme kullanılır. BirthDate sütunu, işlemleri desteklemeyen randomize şifreleme kullanır.

SSN sütunu için **Şifreleme Türünü** **Deterministic** ve BirthDate sütununa **Randomize**olarak ayarlayın. **İleri**’ye tıklayın.

![Sütunları şifreleme](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Ana Anahtar Yapılandırması

**Ana Anahtar Yapılandırma** sayfası, CMK'nızı ayarladığınız ve CMK'nın depolanacak anahtar deposu sağlayıcısını seçtiğiniz yerdir. Şu anda, Windows sertifika mağazasında, Azure Anahtar Kasasında veya donanım güvenlik modülünde (HSM) bir CMK depolayabilirsiniz.

Bu öğretici, anahtarlarınızı Azure Key Vault'ta nasıl depolayacağımız gösterir.

1. **Azure Anahtar Kasası'nı**seçin.
2. Açılan listeden istenen anahtar kasasını seçin.
3. **İleri**’ye tıklayın.

![Ana anahtar yapılandırması](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a>Doğrulama

Sütunları şimdi şifreleyebilir veya daha sonra çalışacak bir PowerShell komut dosyası kaydedebilirsiniz. Bu öğretici **için, şimdi bitirmek için Devam et'i** seçin ve **İleri'yi**tıklatın.

### <a name="summary"></a>Özet

Ayarların tümünde doğru olduğundan doğrulayın ve Her Zaman Şifrelenmiş'in kurulumlarını tamamlamak için **Bitir'i** tıklatın.

![Özet](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-the-wizards-actions"></a>Sihirbazın hareketlerini doğrulama

Sihirbaz bittikten sonra veritabanınız Her Zaman Şifrelenmiş olarak ayarlanır. Sihirbaz aşağıdaki eylemleri gerçekleştirdi:

- Bir sütun ana anahtarı oluşturdu ve Azure Key Vault'ta depolandı.
- Bir sütun şifreleme anahtarı oluşturdu ve Azure Key Vault'ta depolandı.
- Şifreleme için seçili sütunları yapılandırıldı. Hastalar tablosunda şu anda veri bulunmamaktadır, ancak seçili sütunlarda varolan veriler artık şifrelenmiştir.

**Klinik** > **Güvenlik** > **Her Zaman Şifreli Anahtarları**genişleterek SSMS anahtarlarıoluşturulmasını doğrulayabilirsiniz.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Şifrelenmiş verilerle çalışan bir istemci uygulaması oluşturma

Her Zaman Şifrelenmiş olarak ayarlanın, ekler *gerçekleştiren* ve şifrelenmiş *sütunlarda seçen* bir uygulama oluşturabilirsiniz.  

> [!IMPORTANT]
> Uygulamanız, her zaman şifrelenmiş sütunlara sahip düz metin verilerini sunucuya aktarırken [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) nesnelerini kullanmalıdır. SqlParameter nesneleri kullanmadan gerçek değerlerin geçirilmesi bir özel durumla sonuçlanır.

1. Visual Studio'yu açın ve yeni bir C# **Console Uygulaması** (Visual Studio 2015 ve daha önce) veya Konsol **Uygulaması (.NET Framework)** (Visual Studio 2017 ve sonrası) oluşturun. Projenizin **.NET Framework 4.6** veya sonraki olarak ayarlandığından emin olun.
2. **ProjealwaysEncryptedConsoleAKVApp** adı ve **Tamam**tıklayın.
3. **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'na**giderek aşağıdaki NuGet paketlerini yükleyin.

Paket Yöneticisi Konsolunda bu iki kod satırı çalıştırın:

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Her Zaman Şifrelenmiş'i etkinleştirmek için bağlantı dizenizi değiştirin

Bu bölümde, veritabanı bağlantı dizenizde Her Zaman Şifrelenmiş nasıl etkinleştirilir.

Her Zaman Şifrelenmiş'i etkinleştirmek **için, sütun şifreleme ayar** anahtar sözcük anahtar sözcüklerini bağlantı dizenize eklemeniz ve **etkin**olarak ayarlamanız gerekir.

Bunu doğrudan bağlantı dizesi olarak ayarlayabilirsiniz veya [SqlConnectionStringBuilder'ı](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx)kullanarak ayarlayabilirsiniz. Sonraki bölümdeki örnek uygulama **SqlConnectionStringBuilder'ın**nasıl kullanılacağını gösterir.

### <a name="enable-always-encrypted-in-the-connection-string"></a>Bağlantı dizesinde Her Zaman Şifrelenmiş'i Etkinleştir

Bağlantı dizenize aşağıdaki anahtar kelimeyi ekleyin.

   `Column Encryption Setting=Enabled`

### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>SqlConnectionStringBuilder ile Her Zaman Şifrelenmeyi Etkinleştir

Aşağıdaki kod, [SqlConnectionStringBuilder.ColumnEncryptionSetting'i](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [Etkin](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx)Olarak Ayarlayarak Her Zaman Şifrelenmiş'i nasıl etkinleştireceklerini gösterir.

```csharp
// Instantiate a SqlConnectionStringBuilder.
SqlConnectionStringBuilder connStringBuilder = new SqlConnectionStringBuilder("replace with your connection string");

// Enable Always Encrypted.
connStringBuilder.ColumnEncryptionSetting = SqlConnectionColumnEncryptionSetting.Enabled;
```

## <a name="register-the-azure-key-vault-provider"></a>Azure Anahtar Kasası sağlayıcısını kaydedin
Aşağıdaki kod, Azure Anahtar Kasa sağlayıcısının ADO.NET sürücüsüne nasıl kaydedilebildiğini gösterir.

```csharp
private static ClientCredential _clientCredential;

static void InitializeAzureKeyVaultProvider() {
    _clientCredential = new ClientCredential(applicationId, clientKey);

    SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider = new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

    Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers = new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

    providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
    SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
}
```

## <a name="always-encrypted-sample-console-application"></a>Her Zaman Şifreli örnek konsol uygulaması

Bu örnek nasıl gösteriş yapılacağını gösterir:

- Her Zaman Şifrelenmiş'i etkinleştirmek için bağlantı dizenizi değiştirin.
- Azure Key Vault'u uygulamanın önemli mağaza sağlayıcısı olarak kaydedin.  
- Verileri şifrelenmiş sütunlara ekleyin.
- Şifreli bir sütunda belirli bir değer için filtre uygulayarak bir kayıt seçin.

*Program.cs* içeriğini aşağıdaki kodla değiştirin. Azure portalından geçerli bağlantı dizeniz ile Ana yöntemden doğrudan önce gelen satırdaki global bağlantıString değişkeni için bağlantı dizesini değiştirin. Bu kodda yapmanız gereken tek değişiklik budur.

Her Zaman Şifreli'yi iş başında görmek için uygulamayı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data;
using System.Data.SqlClient;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

namespace AlwaysEncryptedConsoleAKVApp {
    class Program {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string applicationId = @"<application ID from your AAD application>";
        static string clientKey = "<key from your AAD application>";

        static void Main(string[] args) {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

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

            InsertPatient(new Patient() {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient() {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient() {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient() {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient() {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });

            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients()) {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null) {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }

        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider() {
            _clientCredential = new ClientCredential(applicationId, clientKey);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope) {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
        }

        static int InsertPatient(Patient newPatient) {
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

            using (sqlCmd.Connection = new SqlConnection(connectionString)) {
                try {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex) {
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


        static List<Patient> SelectAllPatients() {
            List<Patient> patients = new List<Patient>();

            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));

            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString)) {
                try {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows) {
                        while (reader.Read()) {
                            patients.Add(new Patient() {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex) {
                    throw;
                }
            }

            return patients;
        }

        static Patient SelectPatientBySSN(string ssn) {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);

            using (sqlCmd.Connection = new SqlConnection(connectionString)) {
                try {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows) {
                        while (reader.Read()) {
                            patient = new Patient() {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else {
                        patient = null;
                    }
                }
                catch (Exception ex) {
                    throw;
                }
            }
            return patient;
        }

        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable() {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString)) {
                try {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex) {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
}
```

## <a name="verify-that-the-data-is-encrypted"></a>Verilerin şifrelenmiş olduğundan doğrulayın

Sunucudaki gerçek verilerin Hastalar verilerini SSMS ile sorgulayarak şifrelenerek şifrelenmediğini hızlı bir şekilde kontrol edebilirsiniz **(Sütun Şifreleme Ayarı** henüz etkinleştirilmediğiniz geçerli bağlantınızı kullanarak).

Aşağıdaki sorguyu Klinik veritabanında çalıştırın.

```sql
SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
```

Şifrelenmiş sütunların düz metin verileri içermediğini görebilirsiniz.

   ![Yeni konsol uygulaması](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

Düz metin verilerine erişmek için SSMS'i kullanmak için öncelikle kullanıcının Azure Anahtar Kasası'na uygun izinlere sahip olduğundan emin olmanız gerekir: *anahtarlarını* *al*ve *doğrula.* Ayrıntılı bilgi için sütun [ana anahtarlarını oluştur ve depola (Her Zaman Şifrelenmiş)](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted)bakın.

Ardından bağlantınız sırasında *Sütun Şifreleme Ayarı=etkin* parametreekleyin.

1. SSMS'te Object **Explorer'da** sunucunuzu sağ tıklatın ve **Bağlantıyı Kesme'yi**seçin.
2. **Sunucuya Bağlan** penceresini açmak için**Veritabanı Altyapısını** **Bağla'yı** > tıklatın ve **Seçenekler'i**tıklatın.
3. **Ek Bağlantı Parametreleri'ni** tıklatın ve **sütun şifreleme ayarı yazın=etkin .**

    ![Yeni konsol uygulaması](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)

4. Aşağıdaki sorguyu Klinik veritabanında çalıştırın.

   ```sql
   SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   ```

     Artık düz metin verilerini şifreli sütunlarda görebilirsiniz.
     ![Yeni konsol uygulaması](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)

## <a name="next-steps"></a>Sonraki adımlar

Her Zaman Şifrelenmiş'i kullanan bir veritabanı oluşturduktan sonra aşağıdakileri yapmak isteyebilirsiniz:

- [Anahtarlarınızı döndürün ve temizleyin.](https://msdn.microsoft.com/library/mt607048.aspx)
- [Zaten Her Zaman Şifrelenmiş ile şifrelenmiş verileri geçirin.](https://msdn.microsoft.com/library/mt621539.aspx)

## <a name="related-information"></a>İlgili bilgiler

- [Her Zaman Şifrelenmiş (istemci geliştirme)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Saydam veri şifrelemesi](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL Server şifreleme](https://msdn.microsoft.com/library/bb510663.aspx)
- [Her Zaman Şifrelenmiş sihirbaz](https://msdn.microsoft.com/library/mt459280.aspx)
- [Her zaman Şifreli blog](https://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
