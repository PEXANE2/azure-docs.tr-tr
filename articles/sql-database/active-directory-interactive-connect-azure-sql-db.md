---
title: ActiveDirectoryInteractive SQL'e bağlanır
description: SqlAuthenticationMethod.ActiveDirectoryInteractive modunu kullanarak Azure SQL Veritabanı'na bağlanmak için açıklamalarla birlikte C# Kodu örneği.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: active directory
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: MirekS
ms.reviewer: GeneMi, vanto
ms.date: 10/11/2019
ms.openlocfilehash: 5e7d58e5e0fc79e407e77ae9d73314a1d5d22666
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73692309"
---
# <a name="connect-to-azure-sql-database-with-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama ile Azure SQL Veritabanına Bağlanın

Bu makalede, Azure SQL Veritabanı'na bağlanan bir C# programı sağlanmaktadır. Program, [Azure Çok Faktörlü Kimlik Doğrulaması'nı](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)destekleyen etkileşimli mod kimlik doğrulaması kullanır.

SQL araçları için Çok Faktörlü Kimlik Doğrulama desteği hakkında daha fazla bilgi için [SQL Server Veri Araçları'nda (SSDT) Azure Active Directory desteğine](https://docs.microsoft.com/sql/ssdt/azure-active-directory)bakın.

## <a name="multi-factor-authentication-for-azure-sql-database"></a>Azure SQL Veritabanı için Çok Faktörlü Kimlik Doğrulama

.NET Framework sürüm 4.7.2'den [`SqlAuthenticationMethod`](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) başlayarak, enumun yeni bir değeri vardır: `ActiveDirectoryInteractive`. Bir istemci C# programında, enum değeri sistemi, bir Azure SQL veritabanına bağlanmak için Çok Faktörlü Kimlik Doğrulama'yı destekleyen Azure Etkin Dizin (Azure AD) etkileşimli modunu kullanmaya yönlendirir. Programı çalıştıran kullanıcı aşağıdaki iletişim kutularını görür:

* Azure AD kullanıcı adını görüntüleyen ve kullanıcının parolasını soran bir iletişim kutusu.

   Kullanıcının etki alanı Azure AD ile beslenirse, parola gerekmedığından bu iletişim kutusu görünmez.

   Azure AD ilkesi kullanıcıya Çok Faktörlü Kimlik Doğrulaması dayatlarsa, sonraki iki iletişim kutusu görüntülenir.

* Bir kullanıcı Ilk kez Çok Faktörlü Kimlik Doğrulama'dan geçerken, sistem kısa mesaj göndermek için bir cep telefonu numarası isteyen bir iletişim kutusu görüntüler. Her ileti, kullanıcının bir sonraki iletişim kutusuna girmesi gereken *doğrulama kodunu* sağlar.

* Sistemin cep telefonuna gönderdiği Çok Faktörlü Kimlik Doğrulama doğrulama kodu isteyen bir iletişim kutusu.

Azure AD'yi Çok Faktörlü Kimlik Doğrulama gerektirecek şekilde yapılandırma hakkında bilgi için [buluttaki Azure Çok Faktörlü Kimlik Doğrulama'ya başlarken](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud)bkz.

Bu iletişim kutularının ekran görüntüleri [için, SQL Server Management Studio ve Azure AD için çok faktörlü kimlik doğrulamayı yapılandırma bölümüne](sql-database-ssms-mfa-authentication-configure.md)bakın.

> [!TIP]
> [.NET API Tarayıcı araç sayfası](https://docs.microsoft.com/dotnet/api/)ile .NET Framework API'leri arayabilirsiniz.
>
> Ayrıca [isteğe bağlı&lt;?term= arama&gt; değeri parametresi](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod)ile doğrudan arama yapabilirsiniz.

## <a name="configure-your-c-application-in-the-azure-portal"></a>Azure portalında C# uygulamanızı yapılandırın

Başlamadan önce bir Azure [SQL Veritabanı sunucusu](sql-database-get-started-portal.md) oluşturulmalı ve kullanılabilir olmalıdır.

### <a name="register-your-app-and-set-permissions"></a>Uygulamanızı kaydedin ve izinleri ayarlayın

Azure AD kimlik doğrulamasını kullanmak için C# programınızın Azure AD uygulaması olarak kaydolması gerekmektedir. Bir uygulamayı kaydettirmek için Azure AD yöneticisi veya Azure AD *Uygulama Geliştiricisi* rolüne atanmış bir kullanıcı olmanız gerekir. Rolleratama hakkında daha fazla bilgi için Azure [Etkin Dizini olan kullanıcılara yönetici ve yönetici olmayan roller atama 'ya](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)bakın.

Uygulama kaydının tamamlanması bir **uygulama kimliği**oluşturur ve görüntüler. Programınız bağlanmak için bu kimliği içermelidir.

Başvurunuz için gerekli izinleri kaydetmek ve ayarlamak için:

1. Azure portalında Azure **Active Directory** > **App kayıtlarını** > seçin**Yeni kayıt**.

    ![Uygulama kaydı](media/active-directory-interactive-connect-azure-sql-db/image1.png)

    Uygulama kaydı oluşturulduktan sonra **uygulama kimliği** değeri oluşturulur ve görüntülenir.

    ![Uygulama Kimliği görüntülendi](media/active-directory-interactive-connect-azure-sql-db/image2.png)

2. **API izinlerini** > seçin**İzin ekleyin.**

    ![Kayıtlı uygulama için izin ayarları](media/active-directory-interactive-connect-azure-sql-db/sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

3. **Kuruluşumun kullandığı >** arama > **Azure SQL Veritabanı'nı** yazın ve Azure **SQL Veritabanı'nı**seçin.

    ![Azure SQL Veritabanı için API'ye erişim ekleme](media/active-directory-interactive-connect-azure-sql-db/sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

4. **Temsilcilerle Yetkiuser_impersonation** > **user_impersonation** > **Ekleme İzinleri'ni**seçin.

    ![Azure SQL Veritabanı için API'ye temsilci izinleri](media/active-directory-interactive-connect-azure-sql-db/sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)

### <a name="set-an-azure-ad-admin-for-your-sql-database-server"></a>SQL Veritabanı sunucunuz için bir Azure AD yöneticisi ayarlama

C# programınızın çalışması için, Bir Azure SQL sunucu yöneticisinin SQL Veritabanı sunucunuz için bir Azure AD yöneticisi ataması gerekir. 

SQL **Server** sayfasında **Active Directory admin** > **Set admin'i**seçin.

Azure AD yöneticileri ve Azure SQL Veritabanı kullanıcıları hakkında daha fazla bilgi için, [Azure Active Directory kimlik doğrulamasını SQL Veritabanı ile Yapılandırma ve yönetme](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)ekran görüntülerine bakın.

### <a name="add-a-non-admin-user-to-a-specific-database-optional"></a>Belirli bir veritabanına yönetici olmayan bir kullanıcı ekleme (isteğe bağlı)

Bir SQL Veritabanı sunucusuiçin bir Azure AD yöneticisi C# örnek programını çalıştırabilir. Azure AD kullanıcısı, veritabanındaysa programı çalıştırabilir. Bir Azure AD SQL yöneticisi veya veritabanında zaten var `ALTER ANY USER` olan ve veritabanında izni olan bir Azure AD kullanıcısı bir kullanıcı ekleyebilir.

SQL [`Create User`](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) komutu ile veritabanına bir kullanıcı ekleyebilirsiniz. `CREATE USER [<username>] FROM EXTERNAL PROVIDER` bunun bir örneğidir.

Daha fazla bilgi için bkz: [SQL Veritabanı, Yönetilen Örnek veya SQL Veri Ambarı ile kimlik doğrulama için Azure Etkin Dizin Kimlik Doğrulamasını Kullanın.](sql-database-aad-authentication.md)

## <a name="new-authentication-enum-value"></a>Yeni kimlik doğrulama enum değeri

C# örneği [`System.Data.SqlClient`](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) ad alanına dayanır. Çok Faktörlü Kimlik Doğrulama için özel `SqlAuthenticationMethod`ilgi enum , aşağıdaki değerlere sahiptir:

- `SqlAuthenticationMethod.ActiveDirectoryInteractive`

   Çok Faktörlü Kimlik Doğrulama'yı uygulamak için bu değeri azure AD kullanıcı adı ile kullanın. Bu değer, bu makalenin odak noktasıdır. Kullanıcı parolası için iletişim kutuları görüntüleyerek etkileşimli bir deneyim ve bu kullanıcıya Çok Faktörlü Kimlik Doğrulama empoze edilirse Çok Faktörlü Kimlik Doğrulama doğrulaması için üretir. Bu değer .NET Framework sürümü 4.7.2 ile başlayarak kullanılabilir.

- `SqlAuthenticationMethod.ActiveDirectoryIntegrated`

  Federe bir *federated* hesap için bu değeri kullanın. Federe bir hesap için, kullanıcı adı Windows etki alanı tarafından bilinir. Bu kimlik doğrulama yöntemi Çok Faktörlü Kimlik Doğrulama'yı desteklemez.

- `SqlAuthenticationMethod.ActiveDirectoryPassword`

  Azure AD kullanıcı adı ve parola gerektiren kimlik doğrulama için bu değeri kullanın. Azure SQL Veritabanı kimlik doğrulaması yapar. Bu yöntem Çok Faktörlü Kimlik Doğrulama'yı desteklemez.

## <a name="set-c-parameter-values-from-the-azure-portal"></a>Azure portalından C# parametre değerlerini ayarlama

C# programının başarılı bir şekilde çalışması için statik alanlara uygun değerler atamanız gerekir. Burada örnek değerlere sahip alanlar gösterilmiştir. Ayrıca, gerekli değerleri elde edebilirsiniz Azure portal konumları gösterilir.

| Statik alan adı | Örnek değer | Azure portalının neresinde |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | "my-sqldb-svr.database.windows.net" | **SQL sunucuları** > **Ada göre filtreleme** |
| AzureAD_UserID | "kullanıcı\@abc.onmicrosoft.com" | **Azure Active Directory** > **Kullanıcı** > **Yeni konuk kullanıcı** |
| Initial_DatabaseName | "myDatabase" | **SQL sunucuları** > **SQL veritabanları** |
| Müşteri Başvuru KIMLIĞI | "a94f9c62-97fe-4d19-b06d-11111111111" | **Azure Active Directory** > **App kayıtları** > Ad > **Uygulama Kimliğine** **göre arama** |
| RedirectUri | yeni Uri("https://mywebserver.com/") | **Azure Active Directory** > **App kayıtları** > **Ada** > göre arama *[Uygulamanız kaydı]* > **Ayarlar** > **Yönlendirmeler**<br /><br />Burada kullanılmadığından, bu makale için geçerli bir değer RedirectUri için iyidir. |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="verify-with-sql-server-management-studio"></a>SQL Server Management Studio ile doğrulayın

C# programını çalıştırmadan önce, SQL Server Management Studio'da (SSMS) kurulum ve yapılandırmalarınızın doğru olup olmadığını kontrol etmek iyi bir fikirdir. Herhangi bir C# programı hatası daha sonra kaynak koduna daraltılabilir.

### <a name="verify-sql-database-firewall-ip-addresses"></a>SQL Veritabanı güvenlik duvarı IP adreslerini doğrulama

C# programını çalıştırmayı planladığınız aynı binada, aynı bilgisayardan SSMS çalıştırın. Bu sınama için, herhangi bir **Kimlik Doğrulama** modu tamamdır. Veritabanı sunucusu güvenlik duvarının IP adresinizi kabul etmediğine dair herhangi bir belirti varsa, yardım için [Azure SQL Veritabanı sunucu düzeyi ve veritabanı düzeyinde güvenlik duvarı kurallarına](sql-database-firewall-configure.md) bakın.

### <a name="verify-azure-active-directory-multi-factor-authentication"></a>Azure Etkin Dizin Çok Faktörlü Kimlik Doğrulamayı Doğrula

SSMS'i tekrar çalıştırın, bu kez **Kimlik Doğrulama** ile **Active Directory - Universal ile MFA desteği**ayarlayın. Bu seçenek, SSMS sürüm 17.5 veya daha sonra gerektirir.

Daha fazla bilgi için [bkz.](sql-database-ssms-mfa-authentication-configure.md)

> [!NOTE]
> Veritabanında konuk kullanıcıysanız, veritabanı için Azure AD etki alanı adını da sağlamanız gerekir: **Seçenekler** > **AD etki alanı adını veya kiracı kimliğini**seçin. Azure portalında alan adını bulmak için **Azure Active Directory** > **Custom alan adlarını**seçin. C# örnek programında, etki alanı adı sağlamak gerekli değildir.

## <a name="c-code-example"></a>C# kodu örneği

Örnek C# programı [*Microsoft.IdentityModel.Clients.ActiveDirectory*](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory) DLL derlemesi dayanır.

Bu paketi yüklemek için Visual Studio'da **Project** > **Manage NuGet Paketlerini**seçin. Arama ve **Microsoft.IdentityModel.Clients.ActiveDirectory yükleyin.**

Bu, C# kaynak kodunun bir örneğidir.

```csharp

using System;

// Reference to Azure AD authentication assembly
using Microsoft.IdentityModel.Clients.ActiveDirectory;

using DA = System.Data;
using SC = System.Data.SqlClient;
using AD = Microsoft.IdentityModel.Clients.ActiveDirectory;
using TX = System.Text;
using TT = System.Threading.Tasks;

namespace ADInteractive5
{
    class Program
    {
        // ASSIGN YOUR VALUES TO THESE STATIC FIELDS !!
        static public string Az_SQLDB_svrName = "<Your SQL DB server>";
        static public string AzureAD_UserID = "<Your User ID>";
        static public string Initial_DatabaseName = "<Your Database>";
        // Some scenarios do not need values for the following two fields:
        static public readonly string ClientApplicationID = "<Your App ID>";
        static public readonly Uri RedirectUri = new Uri("<Your URI>");

        public static void Main(string[] args)
        {
            var provider = new ActiveDirectoryAuthProvider();

            SC.SqlAuthenticationProvider.SetProvider(
                SC.SqlAuthenticationMethod.ActiveDirectoryInteractive,
                //SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated,  // Alternatives.
                //SC.SqlAuthenticationMethod.ActiveDirectoryPassword,
                provider);

            Program.Connection();
        }

        public static void Connection()
        {
            SC.SqlConnectionStringBuilder builder = new SC.SqlConnectionStringBuilder();

            // Program._  static values that you set earlier.
            builder["Data Source"] = Program.Az_SQLDB_svrName;
            builder.UserID = Program.AzureAD_UserID;
            builder["Initial Catalog"] = Program.Initial_DatabaseName;

            // This "Password" is not used with .ActiveDirectoryInteractive.
            //builder["Password"] = "<YOUR PASSWORD HERE>";

            builder["Connect Timeout"] = 15;
            builder["TrustServerCertificate"] = true;
            builder.Pooling = false;

            // Assigned enum value must match the enum given to .SetProvider().
            builder.Authentication = SC.SqlAuthenticationMethod.ActiveDirectoryInteractive;
            SC.SqlConnection sqlConnection = new SC.SqlConnection(builder.ConnectionString);

            SC.SqlCommand cmd = new SC.SqlCommand(
                "SELECT '******** MY QUERY RAN SUCCESSFULLY!! ********';",
                sqlConnection);

            try
            {
                sqlConnection.Open();
                if (sqlConnection.State == DA.ConnectionState.Open)
                {
                    var rdr = cmd.ExecuteReader();
                    var msg = new TX.StringBuilder();
                    while (rdr.Read())
                    {
                        msg.AppendLine(rdr.GetString(0));
                    }
                    Console.WriteLine(msg.ToString());
                    Console.WriteLine(":Success");
                }
                else
                {
                    Console.WriteLine(":Failed");
                }
                sqlConnection.Close();
            }
            catch (Exception ex)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Connection failed with the following exception...");
                Console.WriteLine(ex.ToString());
                Console.ResetColor();
            }
        }
    } // EOClass Program.

    /// <summary>
    /// SqlAuthenticationProvider - Is a public class that defines 3 different Azure AD
    /// authentication methods.  The methods are supported in the new .NET 4.7.2.
    ///  . 
    /// 1. Interactive,  2. Integrated,  3. Password
    ///  . 
    /// All 3 authentication methods are based on the Azure
    /// Active Directory Authentication Library (ADAL) managed library.
    /// </summary>
    public class ActiveDirectoryAuthProvider : SC.SqlAuthenticationProvider
    {
        // Program._ more static values that you set!
        private readonly string _clientId = Program.ClientApplicationID;
        private readonly Uri _redirectUri = Program.RedirectUri;

        public override async TT.Task<SC.SqlAuthenticationToken>
            AcquireTokenAsync(SC.SqlAuthenticationParameters parameters)
        {
            AD.AuthenticationContext authContext =
                new AD.AuthenticationContext(parameters.Authority);
            authContext.CorrelationId = parameters.ConnectionId;
            AD.AuthenticationResult result;

            switch (parameters.AuthenticationMethod)
            {
                case SC.SqlAuthenticationMethod.ActiveDirectoryInteractive:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,  // "https://database.windows.net/"
                        _clientId,
                        _redirectUri,
                        new AD.PlatformParameters(AD.PromptBehavior.Auto),
                        new AD.UserIdentifier(
                            parameters.UserId,
                            AD.UserIdentifierType.RequiredDisplayableId));
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_1 == '.ActiveDirectoryIntegrated'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserCredential());
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryPassword:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_2 == '.ActiveDirectoryPassword'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserPasswordCredential(
                            parameters.UserId,
                            parameters.Password));
                    break;

                default: throw new InvalidOperationException();
            }
            return new SC.SqlAuthenticationToken(result.AccessToken, result.ExpiresOn);
        }

        public override bool IsSupported(SC.SqlAuthenticationMethod authenticationMethod)
        {
            return authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryInteractive
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryPassword;
        }
    } // EOClass ActiveDirectoryAuthProvider.
} // EONamespace.  End of entire program source code.

```

&nbsp;

Bu, C# test çıktısına bir örnektir.

```
[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>> ADInteractive5.exe
In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.
******** MY QUERY RAN SUCCESSFULLY!! ********

:Success

[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>>
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

- [Get-AzSqlServerActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserveractivedirectoryadministrator)
