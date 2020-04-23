---
title: 'Öğretici: SQL Veritabanı ile ASP.NET Core'
description: Azure App Service’te çalışan ve bir SQL Veritabanı’na bağlantısı olan bir .NET Core uygulamasını nasıl edinebileceğinizi öğrenin.
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 08/06/2019
ms.custom: mvc, cli-validate, seodec18
ms.openlocfilehash: c4dacd06cd53ebb71ca9db2722fdf46aade841bc
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085458"
---
# <a name="tutorial-build-an-aspnet-core-and-sql-database-app-in-azure-app-service"></a>Öğretici: Azure Uygulama Hizmeti'nde ASP.NET Core ve SQL Veritabanı uygulaması oluşturun

> [!NOTE]
> Bu makalede bir uygulamanın Windows üzerinde App Service'e dağıtımı yapılır. _Linux'ta_App Service'e dağıtmak için [bkz.](./containers/tutorial-dotnetcore-sqldb-app.md)
>

[App Service](overview.md), Azure’da yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğretici, bir .NET Core uygulamasının nasıl oluşturulup bir SQL Veritabanına bağlanılmayı gösterir. Öğreticiyi tamamladığınızda, App Service’te çalışan bir .NET Core MVC uygulaması oluşturmuş olacaksınız.

![App Service’te çalışan uygulama](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

Aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da SQL Veritabanı oluşturma
> * .NET Core uygulamasını SQL Veritabanı’na bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Azure’daki tanılama günlüklerinin akışını sağlama
> * Uygulamayı Azure portalında yönetme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* [Git'i yükleyin](https://git-scm.com/)
* [Install .NET Core SDK](https://dotnet.microsoft.com/download)

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
dotnet tool install -g dotnet-ef --version 3.1.1
dotnet-ef database update
dotnet run
```

Bir tarayıcıda `http://localhost:5000` sayfasına gidin. **Yeni Oluştur** bağlantısını seçin ve _yapılacak_ birkaç iş oluşturun.

![SQL Veritabanı’na başarıyla bağlanır](./media/app-service-web-tutorial-dotnetcore-sqldb/local-app-in-browser.png)

.NET Core’u dilediğiniz zaman durdurmak için, terminalde `Ctrl+C` tuşlarına basın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-production-sql-database"></a>Üretim SQL Veritabanı oluşturma

Bu adımda, Azure’da bir SQL Veritabanı oluşturursunuz. Uygulamanız Azure’da dağıtıldığında bu bulut veritabanını kullanır.

SQL Veritabanı için bu öğreticide [Azure SQL Veritabanı](/azure/sql-database/) kullanılır.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-sql-database-logical-server"></a>SQL Veritabanı mantıksal sunucusu oluşturma

Bulut Kabuğu'nda, komutu [`az sql server create`](/cli/azure/sql/server?view=azure-cli-latest#az-sql-server-create) olan bir SQL Veritabanı mantıksal sunucusu oluşturun.

server_name * \<>* yer tutucuyu benzersiz bir SQL Veritabanı adı ile değiştirin. Bu ad, SQL Veritabanı uç noktasının bir parçası olan `<server_name>.database.windows.net` olarak kullanıldığından, adın Azure’daki tüm mantıksal sunucularda benzersiz olması gerekir. Ad yalnızca küçük harf, rakam ve tire (-) karakteri içerebilir; 3 ila 50 karakter uzunluğunda olmalıdır. Ayrıca, * \<>db_username* ve * \<db_password>* kullanıcı adı ve şifre ile değiştirin. 


```azurecli-interactive
az sql server create --name <server_name> --resource-group myResourceGroup --location "West Europe" --admin-user <db_username> --admin-password <db_password>
```

SQL Veritabanı mantıksal sunucusu oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

<pre>
{
  "administratorLogin": "&lt;db_username&gt;",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "&lt;server_name&gt;.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/&lt;server_name&gt;",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "&lt;server_name&gt;",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
</pre>

### <a name="configure-a-server-firewall-rule"></a>Sunucu güvenlik duvarı kurallarını yapılandırma

[`az sql server firewall create`](/cli/azure/sql/server/firewall-rule?view=azure-cli-latest#az-sql-server-firewall-rule-create) komutunu kullanarak [Azure SQL Veritabanı sunucusu düzeyinde güvenlik duvarı kuralı](../sql-database/sql-database-firewall-configure.md) oluşturun. Hem başlangıç hem bitiş IP’si 0.0.0.0 olarak ayarlandığında, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server_name> --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP] 
> [Yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](overview-inbound-outbound-ips.md#find-outbound-ips) güvenlik duvarı kurallarınızda daha da kısıtlayıcı olabilirsiniz.
>

### <a name="create-a-database"></a>Veritabanı oluşturma

[`az sql db create`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-create) komutunu kullanarak sunucuda [S0 performans düzeyine](../sql-database/sql-database-service-tiers-dtu.md) sahip bir veritabanı oluşturun.

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server_name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Bağlantı dizesi oluşturma

Aşağıdaki dizeyi * \<db_username * * \<>* server_name>ve * \<* daha önce kullandığınız db_password>ile değiştirin.

```
Server=tcp:<server_name>.database.windows.net,1433;Database=coreDB;User ID=<db_username>;Password=<db_password>;Encrypt=true;Connection Timeout=30;
```

Bu, .NET Core uygulamanızın bağlantı dizesidir. Dizeyi daha sonra kullanmak üzere kopyalayın.

## <a name="deploy-app-to-azure"></a>Uygulamayı Azure’da dağıtma

Bu adımda, SQL Veritabanı’na bağlı .NET Core uygulamanızı App Service’e dağıtırsınız.

### <a name="configure-local-git-deployment"></a>Yerel git dağıtımını yapılandırma

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app-dotnetcore-win-no-h.md)] 

### <a name="configure-connection-string"></a>Bağlantı dizelerini yapılandırma

Azure uygulamanız için bağlantı dizeleri [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) ayarlamak için Bulut Kabuğu'ndaki komutu kullanın. Aşağıdaki komutta, * \<uygulama adı>* ve * \<daha* önce oluşturduğunuz bağlantı dizesiyle connection_string>parametresini değiştirin.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection="<connection_string>" --connection-string-type SQLServer
```

ASP.NET Core'da,`MyDbConnection` *appsettings.json'da*belirtilen herhangi bir bağlantı dizesi gibi standart deseni kullanarak bu adlandırılmış bağlantı dizesini kullanabilirsiniz. Bu durumda, `MyDbConnection` *appsettings.json'unuzda*da tanımlanır. App Service'de çalışırken, App Service'de tanımlanan bağlantı dizesi, *appsettings.json'unuzda*tanımlanan bağlantı dizesinden önceliklidir. Kod, yerel geliştirme sırasında *appsettings.json* değerini kullanır ve dağıtıldığında aynı kod Uygulama Hizmeti değerini kullanır.

Bağlantı dizesinin kodunuzda nasıl başvuruldığını görmek [için üretimde SQL Veritabanına Bağlan'a](#connect-to-sql-database-in-production)bakın.

### <a name="configure-environment-variable"></a>Çevre değişkenini yapılandırma

Sonra, `ASPNETCORE_ENVIRONMENT` uygulama ayarını _Üretim_ olarak belirleyin. Bu ayar, yerel geliştirme ortamınız için SQLite ve Azure ortamınız için SQL Veritabanı kullandığınız için Azure'da çalışıp çalışmadığınızı bilmenizi sağlar.

Aşağıdaki örnek, Azure `ASPNETCORE_ENVIRONMENT` uygulamanızda bir uygulama ayarını yapılandırır. app_name * \<>* yer tutucuyu değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings ASPNETCORE_ENVIRONMENT="Production"
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
            options.UseSqlite("Data Source=localdatabase.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<MyDatabaseContext>().Database.Migrate();
```

Bu kod üretimde çalıştığını algılarsa (Azure ortamını gösterir), SQL Veritabanı'na bağlanmak için yapılandırdığınız bağlantı dizesini kullanır.

Arama, `Database.Migrate()` .NET Core uygulamanızın geçiş yapılandırmasına bağlı olarak ihtiyaç duyduğu veritabanlarını otomatik olarak oluşturduğundan, Azure'da çalıştırıldığında size yardımcı olur. 

> [!IMPORTANT]
> Ölçeklendirmesi gereken üretim uygulamaları için, [üretimdeki geçişleri uygulamadaki](/aspnet/core/data/ef-rp/migrations#applying-migrations-in-production)en iyi uygulamaları izleyin.
> 

Değişikliklerinizi kaydedin ve Git deponuza işleyin. 

```bash
git add .
git commit -m "connect to SQLDB in Azure"
```

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

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
To https://&lt;app_name&gt;.scm.azurewebsites.net/&lt;app_name&gt;.git
 * [new branch]      master -> master
</pre>

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına göz atın

Web tarayıcınızı kullanarak dağıtılan uygulamaya göz atın.

```bash
http://<app_name>.azurewebsites.net
```

Yapılacak birkaç iş ekleyin.

![App Service’te çalışan uygulama](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

**Tebrikler!** App Service’te veri temelli bir .NET Core uygulaması çalıştırıyorsunuz.

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

`Create([Bind("ID,Description,CreatedDate")] Todo todo)` metodunu bulun ve `Done` değerini `Bind` özniteliğindeki özellik listesine ekleyin. Hazır olduğunuzda, `Create()` metot imzanız aşağıdaki koda benzer şekilde görünür:

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

Kullanım `git push` tamamlandıktan sonra, Uygulama Hizmeti uygulamanıza gidin ve yapılacaklar öğesi eklemeyi deneyin ve Bitti'yi kontrol **edin.**

![Kod İlk Geçiş'ten sonra Azure uygulaması](./media/app-service-web-tutorial-dotnetcore-sqldb/this-one-is-done.png)

Mevcut yapılacak öğeleriniz görüntülenmeye devam eder. .NET Core uygulamanızı yeniden yayımladığınızda, SQL Veritabanınızdaki varolan veriler kaybolmaz. Ayrıca, Entity Framework Code Migrations yalnızca veri şemasını değiştirir ve mevcut verilerinizde herhangi bir değişiklik yapmaz.

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

ASP.NET Core uygulaması Azure Uygulama Hizmeti'nde çalışırken, konsol günlüklerini Bulut Bulut'a iletebilirsiniz. Böylece, uygulama hatalarını ayıklamanıza yardımcı olan tanılama iletilerinin aynısını alabilirsiniz.

Örnek proje, [azure'da ASP.NET Core Logging'deki](https://docs.microsoft.com/aspnet/core/fundamentals/logging#azure-app-service-provider) iki yapılandırma değişikliğiyle birlikte aşağıdaki leri takip eder:

- `Microsoft.Extensions.Logging.AzureAppServices` *DotNetCoreSqlDb.csproj*bir referans içerir.
- `loggerFactory.AddAzureWebAppDiagnostics()` *Program.cs'da*aramalar.

App Service'deki ASP.NET Çekirdek [günlük düzeyini](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-level) varsayılan `Error`düzeye [`az webapp log config`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-config) `Information` ayarlamak için Bulut Kabuğu'ndaki komutu kullanın.

```azurecli-interactive
az webapp log config --name <app_name> --resource-group myResourceGroup --application-logging true --level information
```

> [!NOTE]
> Projenin günlük düzeyi zaten `Information` *appsettings.json*ayarlanır.
> 

Günlük akışını başlatmak için [`az webapp log tail`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-tail) Bulut Kabuğu'ndaki komutu kullanın.

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
```

Günlük akışı başladıktan sonra, bazı web trafiği almak için tarayıcıdaki Azure uygulamasını yenileyin. Artık konsol günlüklerinin terminale yöneltildiğini görebilirsiniz. Konsol günlüklerini hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

Günlük akışını herhangi bir zamanda `Ctrl` + `C`durdurmak için .

ASP.NET Core günlüklerini özelleştirme hakkında daha fazla bilgi için ASP.NET [Core'da Oturum Açma'ya](https://docs.microsoft.com/aspnet/core/fundamentals/logging)bakın.

## <a name="manage-your-azure-app"></a>Azure uygulamanızı yönetme

Oluşturduğunuz uygulamayı [Azure portalında](https://portal.azure.com)görmek için App **Services'ı**arayın ve seçin.

![Azure portalında Uygulama Hizmetleri'ni seçin](./media/app-service-web-tutorial-dotnetcore-sqldb/app-services.png)

Uygulama **Hizmetleri** sayfasında Azure uygulamanızın adını seçin.

![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-dotnetcore-sqldb/access-portal.png)

Varsayılan olarak, portal uygulamanızın **Genel Bakış** sayfasını gösterir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafında açabileceğiniz farklı yapılandırma sayfaları gösterilmektedir.

![Azure portalında App Service sayfası](./media/app-service-web-tutorial-dotnetcore-sqldb/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

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
> [Varolan özel bir DNS adını Azure Uygulama Hizmetiile eşleme](app-service-web-tutorial-custom-domain.md)
