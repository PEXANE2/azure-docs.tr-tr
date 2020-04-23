---
title: 'Öğretici: Linux ASP.NET Core SQL DB ile'
description: Sql Veritabanı'na bağlanarak Azure Uygulama Hizmeti'nde çalışan veri odaklı bir Linux ASP.NET Core uygulamasını nasıl edineceklerini öğrenin.
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 08/06/2019
ms.custom: mvc, cli-validate, seodec18
ms.openlocfilehash: 7eed76a6d5272b6e4411497ad4e1d5dbc62b6252
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085972"
---
# <a name="build-an-aspnet-core-and-sql-database-app-in-azure-app-service-on-linux"></a>Linux'ta Azure Uygulama Hizmeti'nde ASP.NET Core ve SQL Veritabanı uygulaması oluşturun

> [!NOTE]
> Bu makalede bir uygulamanın Linux üzerinde App Service'e dağıtımı yapılır. _Windows'da_Uygulama Hizmeti'ne dağıtmak için azure [uygulama hizmetinde bir .NET Core ve SQL Veritabanı uygulaması oluşturun'a](../app-service-web-tutorial-dotnetcore-sqldb.md)bakın.
>

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu öğretici, bir .NET Core uygulamasının nasıl oluşturulup bir SQL Veritabanına bağlanılmayı gösterir. Öğreticiyi tamamladığınızda, Linux’ta App Service üzerinde çalışan bir .NET Core MVC uygulaması oluşturmuş olacaksınız.

![Linux’ta App Service üzerinde çalışan uygulama](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da SQL Veritabanı oluşturma
> * .NET Core uygulamasını SQL Veritabanı’na bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Azure’daki tanılama günlüklerinin akışını sağlama
> * Uygulamayı Azure portalında yönetme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* [Git'i yükleyin](https://git-scm.com/)
* [Yükle .NET Çekirdek SDK 2.2](https://dotnet.microsoft.com/download/dotnet-core/2.2)

## <a name="create-local-net-core-app"></a>Yerel .NET Core uygulaması oluşturma

Bu adımda, yerel .NET Core projesini ayarlarsınız.

### <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Terminal penceresinde, `cd` ile bir çalışma dizinine gidin.

Örnek depoyu kopyalamak ve kökünü değiştirmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

Örnek proje, [Entity Framework Core](https://docs.microsoft.com/ef/core/) kullanan temel bir CRUD (oluşturma-okuma-güncelleştirme-silme) uygulaması içerir.

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Gerekli paketleri yüklemek, veritabanı geçişlerini çalıştırmak ve uygulamayı başlatmak için aşağıdaki komutları çalıştırın.

```bash
dotnet restore
dotnet ef database update
dotnet run
```

Bir tarayıcıda `http://localhost:5000` sayfasına gidin. **Yeni Oluştur** bağlantısını seçin ve _yapılacak_ birkaç iş oluşturun.

![SQL Veritabanı’na başarıyla bağlanır](./media/tutorial-dotnetcore-sqldb-app/local-app-in-browser.png)

.NET Core’u dilediğiniz zaman durdurmak için, terminalde `Ctrl+C` tuşlarına basın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-production-sql-database"></a>Üretim SQL Veritabanı oluşturma

Bu adımda, Azure’da bir SQL Veritabanı oluşturursunuz. Uygulamanız Azure’da dağıtıldığında bu bulut veritabanını kullanır.

SQL Veritabanı için bu öğreticide [Azure SQL Veritabanı](/azure/sql-database/) kullanılır.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-a-sql-database-logical-server"></a>SQL Veritabanı mantıksal sunucusu oluşturma

Bulut Kabuğu'nda, komutu [`az sql server create`](/cli/azure/sql/server?view=azure-cli-latest#az-sql-server-create) olan bir SQL Veritabanı mantıksal sunucusu oluşturun.

Sunucu * \<adı>* yer tutucuyu benzersiz bir SQL Veritabanı adı ile değiştirin. Bu ad, SQL Veritabanı uç noktasının bir parçası olan `<server-name>.database.windows.net` olarak kullanıldığından, adın Azure’daki tüm mantıksal sunucularda benzersiz olması gerekir. Ad yalnızca küçük harf, rakam ve tire (-) karakteri içerebilir; 3 ila 50 karakter uzunluğunda olmalıdır. Ayrıca, * \<db-kullanıcı adı>* ve * \<db-şifre>* değiştirin bir kullanıcı adı ve seçtiğiniz şifre ile. 


```azurecli-interactive
az sql server create --name <server-name> --resource-group myResourceGroup --location "West Europe" --admin-user <db-username> --admin-password <db-password>
```

SQL Veritabanı mantıksal sunucusu oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

<pre>
{
  "administratorLogin": "sqladmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "&lt;server-name&gt;.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/&lt;server-name&gt;",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "&lt;server-name&gt;",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
</pre>

### <a name="configure-a-server-firewall-rule"></a>Sunucu güvenlik duvarı kurallarını yapılandırma

[`az sql server firewall create`](/cli/azure/sql/server/firewall-rule?view=azure-cli-latest#az-sql-server-firewall-rule-create) komutunu kullanarak [Azure SQL Veritabanı sunucusu düzeyinde güvenlik duvarı kuralı](../../sql-database/sql-database-firewall-configure.md) oluşturun. Hem başlangıç hem bitiş IP’si 0.0.0.0 olarak ayarlandığında, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server-name> --name AllowAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

### <a name="create-a-database"></a>Veritabanı oluşturma

[`az sql db create`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-create) komutunu kullanarak sunucuda [S0 performans düzeyine](../../sql-database/sql-database-service-tiers-dtu.md) sahip bir veritabanı oluşturun.

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server-name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Bağlantı dizesi oluşturma

Aşağıdaki dizeyi * \<sunucu adı>, * * \<db kullanıcı adı>* ve * \<* daha önce kullandığınız db-şifre>değiştirin.

```
Server=tcp:<server-name>.database.windows.net,1433;Database=coreDB;User ID=<db-username>;Password=<db-password>;Encrypt=true;Connection Timeout=30;
```

Bu, .NET Core uygulamanızın bağlantı dizesidir. Dizeyi daha sonra kullanmak üzere kopyalayın.

## <a name="deploy-app-to-azure"></a>Uygulamayı Azure’da dağıtma

Bu adımda, Linux’ta App Service üzerinde SQL Veritabanı’na bağlı .NET Core uygulamanızı dağıtırsınız.

### <a name="configure-local-git-deployment"></a>Yerel git dağıtımını yapılandırma

[!INCLUDE [Configure a deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-linux-no-h.md)] 

### <a name="configure-connection-string"></a>Bağlantı dizelerini yapılandırma

Azure uygulamanız için bağlantı dizeleri [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) ayarlamak için Bulut Kabuğu'ndaki komutu kullanın. Aşağıdaki komutta, * \<uygulama adı>* ve * \<bağlantı dizesini>* parametresini daha önce oluşturduğunuz bağlantı dizesiyle değiştirin.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='<connection-string>' --connection-string-type SQLServer
```

ASP.NET Core'da,`MyDbConnection` *appsettings.json'da*belirtilen herhangi bir bağlantı dizesi gibi standart deseni kullanarak bu adlandırılmış bağlantı dizesini kullanabilirsiniz. Bu durumda, `MyDbConnection` *appsettings.json'unuzda*da tanımlanır. App Service'de çalışırken, App Service'de tanımlanan bağlantı dizesi, *appsettings.json'unuzda*tanımlanan bağlantı dizesinden önceliklidir. Kod, yerel geliştirme sırasında *appsettings.json* değerini kullanır ve dağıtıldığında aynı kod Uygulama Hizmeti değerini kullanır.

Bağlantı dizesinin kodunuzda nasıl başvuruldığını görmek [için üretimde SQL Veritabanına Bağlan'a](#connect-to-sql-database-in-production)bakın.

### <a name="configure-environment-variable"></a>Çevre değişkenini yapılandırma

Sonra, `ASPNETCORE_ENVIRONMENT` uygulama ayarını _Üretim_ olarak belirleyin. Bu ayar, yerel geliştirme ortamınız için SQLite ve Azure ortamınız için SQL Veritabanı kullandığınız için Azure'da çalışıp çalışmadığınızı bilmenizi sağlar.

Aşağıdaki örnek, Azure `ASPNETCORE_ENVIRONMENT` uygulamanızda bir uygulama ayarını yapılandırır. Uygulama * \<adını>* yer tutucuyu değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group myResourceGroup --settings ASPNETCORE_ENVIRONMENT="Production"
```

Kodunuzda ortam değişkeninin nasıl başvuruldu olduğunu görmek [için üretimde SQL Veritabanına Bağlan'a](#connect-to-sql-database-in-production)bakın.

### <a name="connect-to-sql-database-in-production"></a>Üretimde SQL Veritabanına bağlanma

Yerel deponuzda, Startup.cs dosyasını açın ve aşağıdaki kodu bulun:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Kodu, daha önce yapılandırdığınız ortam değişkenlerini kullanan aşağıdaki kodla değiştirin.

```csharp
// Use SQL Database if in Azure, otherwise, use SQLite
if(Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") == "Production")
    services.AddDbContext<MyDatabaseContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
else
    services.AddDbContext<MyDatabaseContext>(options =>
            options.UseSqlite("Data Source=MvcMovie.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<MyDatabaseContext>().Database.Migrate();
```

Bu kod üretimde çalıştığını algılarsa (Azure ortamını gösterir), SQL Veritabanı'na bağlanmak için yapılandırdığınız bağlantı dizesini kullanır. Uygulama Hizmeti'nde uygulama ayarlarına nasıl erişilenhakkında bilgi için [Erişim ortamı değişkenlerine](configure-language-dotnetcore.md#access-environment-variables)bakın.

Arama, `Database.Migrate()` .NET Core uygulamanızın geçiş yapılandırmasına bağlı olarak ihtiyaç duyduğu veritabanlarını otomatik olarak oluşturduğundan, Azure'da çalıştırıldığında size yardımcı olur.

Değişikliklerinizi kaydedin ve Git deponuza işleyin.

```bash
git add .
git commit -m "connect to SQLDB in Azure"
```

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

<pre>
Counting objects: 98, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (92/92), done.
Writing objects: 100% (98/98), 524.98 KiB | 5.58 MiB/s, done.
Total 98 (delta 8), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: .
remote: Updating submodules.
remote: Preparing deployment for commit id '0c497633b8'.
remote: Generating deployment script.
remote: Project file path: ./DotNetCoreSqlDb.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git
 * [new branch]      master -> master
</pre>

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına göz atın

Web tarayıcınızı kullanarak dağıtılan uygulamaya göz atın.

```bash
http://<app-name>.azurewebsites.net
```

Yapılacak birkaç iş ekleyin.

![Linux’ta App Service üzerinde çalışan uygulama](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

**Tebrikler!** Linux’ta App Service üzerinde veri temelli bir .NET Core uygulaması çalıştırıyorsunuz.

## <a name="update-locally-and-redeploy"></a>Yerel olarak güncelleştirme ve yeniden dağıtma

Bu adımda, veritabanı şemanızda bir değişiklik yapacak ve bunu Azure’da yayımlayacaksınız.

### <a name="update-your-data-model"></a>Veri modelinizi güncelleştirme

Kod düzenleyicide _Models\Todo.cs_ dosyasını açın. `ToDo` sınıfına aşağıdaki özelliği ekleyin:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Code First Migrations’ı yerel olarak çalıştırma

Yerel veritabanınızda güncelleştirme yapmak için birkaç komut çalıştırın.

```bash
dotnet ef migrations add AddProperty
```

Yerel veritabanınızı güncelleştirin:

```bash
dotnet ef database update
```

### <a name="use-the-new-property"></a>Yeni özelliği kullanma

`Done` özelliğini kullanarak kodunuzda birkaç değişiklik yapın. Bu öğreticide, daha kolay uygulama için, işlemin nasıl çalıştığını görmek üzere yalnızca `Index` ve `Create` görünümlerini değiştireceksiniz.

_Controllers\TodosController.cs_ dosyasını açın.

`Create()` metodunu bulun ve `Done` değerini `Bind` özniteliğindeki özellik listesine ekleyin. Hazır olduğunuzda, `Create()` metot imzanız aşağıdaki koda benzer şekilde görünür:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

_Views\Todos\Create.cshtml_ dosyasını açın.

Razor kodunda, `Description` için `<div class="form-group">` öğesi ve `CreatedDate` için başka bir `<div class="form-group">` öğesi görürsünüz. Aşağıdaki iki öğeyi takip eden `Done` için başka bir `<div class="form-group">` öğesi ekleyin:

```csharp
<div class="form-group">
    <label asp-for="Done" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Done" class="form-control" />
        <span asp-validation-for="Done" class="text-danger"></span>
    </div>
</div>
```

_Views\Todos\Index.cshtml_ dosyasını açın.

Boş `<th></th>` öğesini arayın. Bu öğenin hemen üstüne aşağıdaki Razor kodunu ekleyin:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

`<td>` etiket yardımcısını içeren `asp-action` öğesini bulun. Bu öğenin hemen üstüne aşağıdaki Razor kodunu ekleyin:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

`Index` ve `Create` görünümlerindeki değişiklikleri görmek için yapmanız gerekenler bu kadardır.

### <a name="test-your-changes-locally"></a>Değişikliklerinizi yerel olarak test etme

Uygulamayı yerel olarak çalıştırın.

```bash
dotnet run
```

Tarayıcınızda `http://localhost:5000/` adresine gidin. Artık yapılacak bir öğe ekleyip öğeyi **Bitti** olarak işaretleyebilirsiniz. Daha sonra öğe, ana sayfanızda tamamlanmış bir öğe olarak görünmelidir. `Edit` görünümünü değiştirmediğinizden, `Edit` görünümünün `Done` alanında görünmediğini göz önünde bulundurun.

### <a name="publish-changes-to-azure"></a>Değişiklikleri Azure’da yayımlama

```bash
git add .
git commit -m "added done field"
git push azure master
```

İş `git push` tamamlandıktan sonra Azure uygulamanıza gidin ve yeni işlevselliği deneyin.

![Kod İlk Geçiş'ten sonra Azure uygulaması](./media/tutorial-dotnetcore-sqldb-app/this-one-is-done.png)

Mevcut yapılacak öğeleriniz görüntülenmeye devam eder. .NET Core uygulamanızı yeniden yayımladığınızda, SQL Veritabanınızdaki varolan veriler kaybolmaz. Ayrıca, Entity Framework Code Migrations yalnızca veri şemasını değiştirir ve mevcut verilerinizde herhangi bir değişiklik yapmaz.

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

Örnek proje, [azure'da ASP.NET Core Logging'deki](https://docs.microsoft.com/aspnet/core/fundamentals/logging#azure-app-service-provider) iki yapılandırma değişikliğiyle birlikte aşağıdaki leri takip eder:

- `Microsoft.Extensions.Logging.AzureAppServices` *DotNetCoreSqlDb.csproj*bir referans içerir.
- `loggerFactory.AddAzureWebAppDiagnostics()` *Startup.cs'da*aramalar.

> [!NOTE]
> Projenin günlük düzeyi `Information` *appsettings.json*olarak ayarlanır.
>

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

ASP.NET Core günlüklerini özelleştirme hakkında daha fazla bilgi için ASP.NET [Core'da Oturum Açma'ya](https://docs.microsoft.com/aspnet/core/fundamentals/logging)bakın.

## <a name="manage-your-azure-app"></a>Azure uygulamanızı yönetme

Oluşturduğunuz uygulamayı görmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **Uygulama Hizmetleri'ni**tıklatın ve ardından Azure uygulamanızın adını tıklatın.

![Azure uygulamasına portal gezintisi](./media/tutorial-dotnetcore-sqldb-app/access-portal.png)

Varsayılan olarak, portal uygulamanızın **Genel Bakış** sayfasını gösterir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service sayfası](./media/tutorial-dotnetcore-sqldb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * Azure’da SQL Veritabanı oluşturma
> * .NET Core uygulamasını SQL Veritabanı’na bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Azure’daki günlüklerin terminalinize akışını sağlama
> * Uygulamayı Azure portalında yönetme

Özel bir DNS adının uygulamanızla nasıl eşleştinolduğunu öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: Uygulamanıza özel DNS adını haritalandırın](../app-service-web-tutorial-custom-domain.md)

Veya diğer kaynaklara göz atın:

> [!div class="nextstepaction"]
> [Core uygulamasını yapılandırmaASP.NET](configure-language-dotnetcore.md)
