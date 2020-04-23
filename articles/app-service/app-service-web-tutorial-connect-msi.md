---
title: 'Öğretici: Yönetilen kimlikle verilere erişin'
description: Yönetilen bir kimlik kullanarak veritabanı bağlantısını nasıl daha güvenli hale getirabileceğinizi ve diğer Azure hizmetlerine nasıl uygulayacağınızı öğrenin.
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/18/2019
ms.custom: mvc, cli-validate
ms.openlocfilehash: b66874cf95ed29d9be0a2d1ea397704131c7b21d
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085447"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>Öğretici: Yönetilen kimlik kullanarak App Service’tan Azure SQL Veritabanı bağlantısını güvenli hale getirme

[App Service](overview.md), Azure’da yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Ayrıca, uygulamanız için [Azure SQL Veritabanı](/azure/sql-database/)’na ve diğer Azure hizmetlerine erişimi güvenli hale getirmeye yönelik anahtar teslim bir çözüm olan [yönetilen kimliği](overview-managed-identity.md) sağlar. App Service içindeki yönetilen kimlikler, bağlantı dizelerindeki kimlik bilgileri gibi uygulamanızdaki gizli dizileri ortadan kaldırarak uygulamanızı daha güvenli hale getirir. Bu öğreticide, aşağıdaki öğreticilerden birinde oluşturduğunuz örnek web uygulamasına yönetilen kimlik ekleyeceğiz: 

- [Öğretici: Azure’da SQL Veritabanı ile ASP.NET uygulaması derleme](app-service-web-tutorial-dotnet-sqldatabase.md)
- [Öğretici: Azure Uygulama Hizmeti'nde ASP.NET Core ve SQL Veritabanı uygulaması oluşturun](app-service-web-tutorial-dotnetcore-sqldb.md)

İşiniz bittiğinde, örnek uygulamanız kullanıcı adı ve parolaya gerek kalmadan SQL Veritabanıa güvenli bir şekilde bağlanacaktır.

> [!NOTE]
> Bu öğreticide kapsanan adımlar aşağıdaki sürümleri destekler:
> 
> -  .NET Framework 4.7.2
> - .NET Core 2.2
>

Ne öğreneceksiniz:

> [!div class="checklist"]
> * Yönetilen kimlikleri etkinleştirme
> * Yönetilen kimliğe SQL Veritabanı erişimi verme
> * SQL Veritabanı ile Azure AD kimlik doğrulamasını kullanacak şekilde Varlık Çerçevesi'ni yapılandırın
> * Azure AD kimlik doğrulamasını kullanarak Visual Studio'dan SQL Veritabanına bağlanın

> [!NOTE]
>Azure AD kimlik doğrulaması, şirket içi Active Directory'deki (AD DS) [Tümleşik Windows kimlik doğrulaması'ndan](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) _farklıdır._ AD DS ve Azure AD tamamen farklı kimlik doğrulama protokolleri kullanır. Daha fazla bilgi için Azure [AD Etki Alanı Hizmetleri belgelerine](https://docs.microsoft.com/azure/active-directory-domain-services/)bakın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makale, Tutorial: Sql Database veya Tutorial [ile Azure'da ASP.NET bir uygulama oluşturma:](app-service-web-tutorial-dotnet-sqldatabase.md) [Azure Uygulama Hizmeti'nde ASP.NET Core ve SQL Veritabanı uygulaması oluşturun' da](app-service-web-tutorial-dotnetcore-sqldb.md)kaldığınız yerden devam edin. Henüz yapmadıysanız, önce iki öğreticiden birini izleyin. Alternatif olarak, adımları SQL Database ile kendi .NET uygulamanız için uyarlayabilirsiniz.

ARKA UÇ OLARAK SQL Veritabanı'nı kullanarak uygulamanızı hata ayıklamak için bilgisayarınızdan istemci bağlantısına izin verdiğinizden emin olun. Değilse, [Azure portalını kullanarak sunucu düzeyindeki IP güvenlik duvarı kurallarını yönet'teki](../sql-database/sql-database-firewall-configure.md#use-the-azure-portal-to-manage-server-level-ip-firewall-rules)adımları izleyerek istemci IP'sini ekleyin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="grant-database-access-to-azure-ad-user"></a>Azure AD kullanıcısına veritabanı erişimi verme

Önce, bir Azure AD kullanıcısını SQL Veritabanı sunucusunun Active Directory yöneticisi olarak atayarak Azure AD kimlik doğrulamasını SQL Veritabanı'na etkinleştirin. Bu kullanıcı, Azure aboneliğinize kaydolmak için kullandığınız Microsoft hesabından farklıdır. Oluşturduğunuz, içe aktardığınız, senkronize ettiğiniz veya Azure AD'ye davet ettiğiniz bir kullanıcı olmalıdır. İzin verilen Azure AD kullanıcıları hakkında daha fazla bilgi için [SQL Veritabanı'ndaki Azure REKLAM özellikleri ve sınırlamaları'na](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations)bakın.

Azure AD kiracınızda henüz bir kullanıcı yoksa, Azure Etkin [Dizini'ni kullanarak Ekle'deki](../active-directory/fundamentals/add-users-azure-active-directory.md)adımları izleyerek bir kullanıcı oluşturun veya kullanıcıları silin.

Azure AD kullanıcının nesne kimliğini [`az ad user list`](/cli/azure/ad/user?view=azure-cli-latest#az-ad-user-list) kullanarak bulun ve * \<kullanıcı-temel adı>* değiştirin. Sonuç bir değişkene kaydedilir.

```azurecli-interactive
azureaduser=$(az ad user list --filter "userPrincipalName eq '<user-principal-name>'" --query [].objectId --output tsv)
```
> [!TIP]
> Azure AD'deki tüm kullanıcı temel adlarının `az ad user list --query [].userPrincipalName`listesini görmek için çalıştırın.
>

Bu Azure AD kullanıcısını Bulut Kabuğu'ndaki komutu kullanarak [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin-create) Etkin Dizin yöneticisi olarak ekleyin. Aşağıdaki komutta, * \<sunucu adı>* SQL Veritabanı sunucu adı `.database.windows.net` (sonek olmadan) değiştirin.

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server-name> --display-name ADMIN --object-id $azureaduser
```

Active Directory yöneticisi ekleme hakkında daha fazla bilgi için Azure [SQL Veritabanı Sunucunuz için Azure Active Directory yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) konusuna bakın

## <a name="set-up-visual-studio"></a>Visual Studio'yu ayarlama

### <a name="windows"></a>Windows
Windows için Visual Studio, Azure AD kimlik doğrulaması ile tümleştirilir. Visual Studio'da geliştirme ve hata ayıklamayı etkinleştirmek için, menüden **Dosya** > **Hesabı Ayarları'nı** seçerek Visual Studio'daki Azure REKLAM kullanıcınızı ekleyin ve **hesap ekle'yi**tıklatın.

Azure hizmet kimlik doğrulaması için Azure AD kullanıcısını ayarlamak için menüden **Araçlar** > **Seçenekleri'ni** seçin ve ardından Azure Hizmet **Kimlik Doğrulama** > **Hesabı Seçimi'ni**seçin. Eklediğiniz Azure REKLAM kullanıcısını seçin ve **Tamam'ı**tıklatın.

Azure AD kimlik doğrulamasını kullanarak, arka uç olarak SQL Veritabanı ile uygulamanızı geliştirmeye ve hata ayıklamaya hazırsınız.

### <a name="macos"></a>macOS

Mac için Visual Studio, Azure AD kimlik doğrulamasıyla tümleşik değildir. Ancak, daha sonra kullanacağınız [Microsoft.Azure.Services.AppAuthentication kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) Azure CLI'den gelen belirteçleri kullanabilir. Visual Studio'da geliştirme ve hata ayıklamayı etkinleştirmek için öncelikle [Azure CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli) yerel makinenize yüklemeniz gerekir.

Azure CLI yerel makinenize yüklendikten sonra, Azure AD kullanıcınızı kullanarak aşağıdaki komutla Azure CLI'de oturum açın:

```bash
az login --allow-no-subscriptions
```
Azure AD kimlik doğrulamasını kullanarak, arka uç olarak SQL Veritabanı ile uygulamanızı geliştirmeye ve hata ayıklamaya hazırsınız.

## <a name="modify-your-project"></a>Projenizi değiştirme

Projeniz için izleyeceğiniz adımlar, projenin ASP.NET bir proje mi yoksa ASP.NET Core projesi mi olduğuna bağlıdır.

- [ASP.NET değiştirin](#modify-aspnet)
- [ASP.NET Çekirdeğini Değiştir](#modify-aspnet-core)

### <a name="modify-aspnet"></a>ASP.NET değiştirin

Visual Studio'da Paket Yöneticisi Konsolu'nu açın ve NuGet paketi [Microsoft.Azure.Services.AppAuthentication'ı](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)ekleyin:

```powershell
Install-Package Microsoft.Azure.Services.AppAuthentication -Version 1.3.1
```

*Web.config'de,* dosyanın üst kısmından çalışma ve aşağıdaki değişiklikleri yapmak:

- In `<configSections>`, içinde aşağıdaki bölüm bildirimi ekleyin:

    ```xml
    <section name="SqlAuthenticationProviders" type="System.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
    ```

- kapanış `</configSections>` etiketinin altına aşağıdaki XML `<SqlAuthenticationProviders>`kodunu ekleyin.

    ```xml
    <SqlAuthenticationProviders>
      <providers>
        <add name="Active Directory Interactive" type="Microsoft.Azure.Services.AppAuthentication.SqlAppAuthenticationProvider, Microsoft.Azure.Services.AppAuthentication" />
      </providers>
    </SqlAuthenticationProviders>
    ```    

- Çağrılan `MyDbConnection` bağlantı dizesini `connectionString` bulun `"server=tcp:<server-name>.database.windows.net;database=<db-name>;UID=AnyString;Authentication=Active Directory Interactive"`ve değerini ' le değiştirin. _ \<Sunucu adı>_ ve _ \<db adı>_ sunucu adınız ve veritabanı adınız ile değiştirin.

> [!NOTE]
> Yeni kaydettiğiniz SqlAuthenticationProvider, daha önce yüklediğiniz AppAuthentication kitaplığı'nın üst kısmını temel alınmaktadır. Varsayılan olarak, sistem tarafından atanmış bir kimlik kullanır. Kullanıcı tarafından atanan bir kimlikyararlanmak için ek bir yapılandırma sağlamanız gerekir. Lütfen AppAuthentication kitaplığı için [bağlantı dize desteğine](../key-vault/general/service-to-service-authentication.md#connection-string-support) bakın.

SQL Veritabanı'na bağlanmak için gereken her şey bu. Visual Studio'da hata ayıklama yaparken kodunuz, [Visual Studio'yu](#set-up-visual-studio)ayarla'da yapılandırdığınız Azure AD kullanıcısını kullanır. Uygulama Hizmeti uygulamanızın yönetilen kimliğinden bağlantıya izin vermek için daha sonra SQL Veritabanı sunucusunu kurarsınız.

Uygulamayı `Ctrl+F5` yeniden çalıştırmak için yazın. Tarayıcınızdaki aynı CRUD uygulaması artık Azure AD kimlik doğrulamasını kullanarak doğrudan Azure SQL Veritabanı'na bağlanıyor. Bu kurulum, Visual Studio'dan veritabanı geçişlerini çalıştırmanızı sağlar.

### <a name="modify-aspnet-core"></a>ASP.NET Çekirdeğini Değiştir

Visual Studio'da Paket Yöneticisi Konsolu'nu açın ve NuGet paketi [Microsoft.Azure.Services.AppAuthentication'ı](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)ekleyin:

```powershell
Install-Package Microsoft.Azure.Services.AppAuthentication -Version 1.3.1
```

ASP.NET [Core ve SQL Veritabanı](app-service-web-tutorial-dotnetcore-sqldb.md) `MyDbConnection` öğreticisinde, yerel geliştirme ortamı bir Sqlite veritabanı dosyası kullandığından ve Azure üretim ortamı App Service'den bir bağlantı dizesi kullandığından bağlantı dizesi hiç kullanılmaz. Etkin Dizin kimlik doğrulaması ile her iki ortamın da aynı bağlantı dizesini kullanmasını istersiniz. *appsettings.json'da*bağlantı dizesinin `MyDbConnection` değerini aşağıdakilerle değiştirin:

```json
"Server=tcp:<server-name>.database.windows.net,1433;Database=<database-name>;"
```

*Startup.cs,* daha önce eklediğiniz kod bölümünü kaldırın:

```csharp
// Use SQL Database if in Azure, otherwise, use SQLite
if (Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") == "Production")
    services.AddDbContext<MyDatabaseContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
else
    services.AddDbContext<MyDatabaseContext>(options =>
            options.UseSqlite("Data Source=localdatabase.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<MyDatabaseContext>().Database.Migrate();
```

Ve aşağıdaki kod ile değiştirin:

```csharp
services.AddDbContext<MyDatabaseContext>(options => {
    options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection"));
});
```

Ardından, SQL Veritabanı'nın erişim belirteciyle Varlık Çerçevesi veritabanı bağlamını sağlarsınız. *Data\MyDatabaseContext.cs'de,* boş `MyDatabaseContext (DbContextOptions<MyDatabaseContext> options)` oluşturucunun kıvırcık ayraçlarının içine aşağıdaki kodu ekleyin:

```csharp
var conn = (System.Data.SqlClient.SqlConnection)Database.GetDbConnection();
conn.AccessToken = (new Microsoft.Azure.Services.AppAuthentication.AzureServiceTokenProvider()).GetAccessTokenAsync("https://database.windows.net/").Result;
```

> [!NOTE]
> Bu gösterim kodu netlik ve basitlik için eşzamanlıdır.

SQL Veritabanı'na bağlanmak için gereken her şey bu. Visual Studio'da hata ayıklama yaparken kodunuz, [Visual Studio'yu](#set-up-visual-studio)ayarla'da yapılandırdığınız Azure AD kullanıcısını kullanır. Uygulama Hizmeti uygulamanızın yönetilen kimliğinden bağlantıya izin vermek için daha sonra SQL Veritabanı sunucusunu kurarsınız. Sınıf, `AzureServiceTokenProvider` bellekteki belirteci önbelleğe alır ve son kullanma tarihinden hemen önce Azure AD'den alır. Belirteci yenilemek için özel bir kodgerekmez.

> [!TIP]
> Yapıya konfigüre ettiğiniz Azure AD kullanıcısı birden `GetAccessTokenAsync("https://database.windows.net/", tenantid)` çok kiracıya erişebiliyorsa, uygun erişim jetonunu almak için istediğiniz kiracı kimliğiyle birlikte arayın.

Uygulamayı `Ctrl+F5` yeniden çalıştırmak için yazın. Tarayıcınızdaki aynı CRUD uygulaması artık Azure AD kimlik doğrulamasını kullanarak doğrudan Azure SQL Veritabanı'na bağlanıyor. Bu kurulum, Visual Studio'dan veritabanı geçişlerini çalıştırmanızı sağlar.

## <a name="use-managed-identity-connectivity"></a>Yönetilen kimlik bağlantısını kullanma

Ardından, Uygulama Hizmeti uygulamanızı, sistem tarafından atanmış yönetilen bir kimlikle SQL Veritabanı'na bağlanacak şekilde yapılandırın.

> [!NOTE]
> Bu bölümdeki yönergeler sistem tarafından atanmış bir kimlik için olsa da, kullanıcı tarafından atanan bir kimlik de kolayca kullanılabilir. Bunu yapmak için: istenilen kullanıcı tarafından `az webapp identity assign command` atanan kimliği atamak için değişiklik gerekir. Ardından, SQL kullanıcısını oluştururken, site adı yerine kullanıcı tarafından atanan kimlik kaynağının adını kullandığınızdan emin olun.

### <a name="enable-managed-identity-on-app"></a>Uygulamada yönetilen kimliği etkinleştirme

Azure uygulamanızda bir yönetilen kimlik etkinleştirmek için Cloud Shell’de [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) komutunu kullanın. Aşağıdaki komutta, * \<uygulama adını>* değiştirin.

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app-name>
```

Çıktının bir örneği aşağıda verilmiştir:

<pre>
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
</pre>

### <a name="grant-permissions-to-managed-identity"></a>Yönetilen kimliğe izin verme

> [!NOTE]
> İsterseniz, kimliği bir [Azure REKLAM grubuna](../active-directory/fundamentals/active-directory-manage-groups.md)ekleyebilir ve ardından kimlik yerine Azure AD grubuna SQL Veritabanı erişimi izni verebilirsiniz. Örneğin, aşağıdaki komutlar önceki adımdaki yönetilen kimliği _myAzureSQLDBAccessGroup_adlı yeni bir gruba ekler:
> 
> ```azurecli-interactive
> groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
> msiobjectid=$(az webapp identity show --resource-group myResourceGroup --name <app-name> --query principalId --output tsv)
> az ad group member add --group $groupid --member-id $msiobjectid
> az ad group member list -g $groupid
> ```
>

Cloud Shell’de SQLCMD komutunu kullanarak SQL Veritabanı oturumunu açın. _ \<Sunucu adı>_ SQL Veritabanı sunucu adınız, _ \<db adı>_ uygulamanızın kullandığı veritabanı adı ve _ \<aad kullanıcı adı>_ ve _ \<aad-şifre>_ Azure AD kullanıcınızın kimlik bilgileriyle değiştirin.

```azurecli-interactive
sqlcmd -S <server-name>.database.windows.net -d <db-name> -U <aad-user-name> -P "<aad-password>" -G -l 30
```

İstediğiniz veritabanının SQL komut isteminde, Azure AD grubunu eklemek ve uygulamanızın ihtiyaç duyduğu izinleri vermek için aşağıdaki komutları çalıştırın. Örneğin, 

```sql
CREATE USER [<identity-name>] FROM EXTERNAL PROVIDER;
ALTER ROLE db_datareader ADD MEMBER [<identity-name>];
ALTER ROLE db_datawriter ADD MEMBER [<identity-name>];
ALTER ROLE db_ddladmin ADD MEMBER [<identity-name>];
GO
```

kimlik adı>, Azure AD'de yönetilen kimliğin adıdır. * \<* Kimlik sistemle atanmışsa, ad her zaman Uygulama Hizmeti uygulamanızın adı ile aynıdır. Bir Azure REKLAM grubu için izin vermek için, grubun görüntü adını (örneğin, *myAzureSQLDBAccessGroup)* kullanın.

Cloud Shell istemine geri dönmek için `EXIT` yazın.

> [!NOTE]
> Yönetilen kimliklerin arka uç hizmetleri, yalnızca süresi dolduğunda hedef kaynağın belirtecigüncelleştirmebir [belirteç önbelleği](overview-managed-identity.md#obtain-tokens-for-azure-resources) de tutar. SQL Veritabanı izinlerinizi yapılandırarak hata yapar sanız ve uygulamanızla ilgili bir belirteç almaya *çalıştıktan sonra* izinleri değiştirmeye çalışırsanız, önbelleğe alınmış belirteç süresi dolana kadar güncelleştirilmiş izinlerle birlikte yeni bir belirteç alamazsınız.

### <a name="modify-connection-string"></a>Bağlantı dizesini değiştirme

*Web.config* veya *appsettings.json'da* yaptığınız değişikliklerin yönetilen kimlikle çalıştığını unutmayın, bu nedenle yapmanız gereken tek şey Visual Studio'nun uygulamanızı ilk kez dağıtarak oluşturduğu App Service'deki varolan bağlantı dizesini kaldırmaktır. Aşağıdaki komutu kullanın, ancak * \<uygulama adı>* uygulamanızın adıyla değiştirin.

```azurecli-interactive
az webapp config connection-string delete --resource-group myResourceGroup --name <app-name> --setting-names MyDbConnection
```

## <a name="publish-your-changes"></a>Değişikliklerinizi yayımlama

Bundan sonra tek yapmanız gereken, değişikliklerinizi Azure'da yayımlamaktır.

** [Tutorial: SQL Database ile Azure'da bir ASP.NET uygulaması oluşturun,](app-service-web-tutorial-dotnet-sqldatabase.md)** değişikliklerinizi Visual Studio'da yayınlayın. **Çözüm Gezgini**’nde **DotNetAppSqlDb** projenize sağ tıklayıp **Yayımla**’yı seçin.

![Çözüm Gezgini'nden yayımlama](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Yayımlama sayfasında **Yayımla**'ya tıklayın. 

** [Tutorial: Azure Uygulama Hizmeti'nde ASP.NET Core ve SQL Veritabanı uygulaması oluşturun,](app-service-web-tutorial-dotnetcore-sqldb.md)** değişikliklerinizi Git kullanarak aşağıdaki komutlarla yayınlayın:

```bash
git commit -am "configure managed identity"
git push azure master
```

Yeni web sayfası yapılacaklar listenizi gösterdiğinde uygulamanızın yönetilen kimliğini kullanarak veritabanına bağlanmakta olduğu anlamına gelir.

![Kod İlk Geçiş'ten sonra Azure uygulaması](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Bundan sonra yapılacaklar listesini daha önce olduğu gibi düzenleyebilirsiniz.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * Yönetilen kimlikleri etkinleştirme
> * Yönetilen kimliğe SQL Veritabanı erişimi verme
> * SQL Veritabanı ile Azure AD kimlik doğrulamasını kullanacak şekilde Varlık Çerçevesi'ni yapılandırın
> * Azure AD kimlik doğrulamasını kullanarak Visual Studio'dan SQL Veritabanına bağlanın

Web uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Varolan özel bir DNS adını Azure Uygulama Hizmetiile eşleme](app-service-web-tutorial-custom-domain.md)
