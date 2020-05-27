---
title: 'Öğretici: SQL DB ile Linux ASP.NET Core'
description: SQL veritabanı ile bağlantı ile Azure App Service çalışan, veri odaklı bir Linux ASP.NET Core uygulamasının nasıl alınacağını öğrenin.
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/23/2020
ms.custom: mvc, cli-validate, seodec18
ms.openlocfilehash: 6c2ed68c18cc7845d45bebffc31842879353f2c2
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83846945"
---
# <a name="tutorial-build-an-aspnet-core-and-sql-database-app-in-azure-app-service-on-linux"></a>Öğretici: Linux üzerinde Azure App Service bir ASP.NET Core ve SQL veritabanı uygulaması oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Linux üzerinde App Service'e dağıtımı yapılır. _Windows_üzerinde App Service dağıtmak için, bkz. [Azure App Service .NET Core ve SQL veritabanı uygulaması oluşturma](../app-service-web-tutorial-dotnetcore-sqldb.md).
>

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu öğreticide bir .NET Core uygulamasının nasıl oluşturulacağı ve bir SQL veritabanına nasıl bağlanacağı gösterilmektedir. Öğreticiyi tamamladığınızda, Linux’ta App Service üzerinde çalışan bir .NET Core MVC uygulaması oluşturmuş olacaksınız.

![Linux’ta App Service üzerinde çalışan uygulama](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da SQL Veritabanı oluşturma
> * .NET Core uygulamasını SQL Veritabanı’na bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Azure’daki tanılama günlüklerinin akışını sağlama
> * Uygulamayı Azure portalında yönetme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Git'i yükleyin](https://git-scm.com/)
* [.NET Core SDK 2,2 'yi yükler](https://dotnet.microsoft.com/download/dotnet-core/2.2)

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
dotnet tool install -g dotnet-ef
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

Cloud Shell, komutuyla bir SQL veritabanı mantıksal sunucusu oluşturun [`az sql server create`](/cli/azure/sql/server?view=azure-cli-latest#az-sql-server-create) .

* \< Sunucu adı>* yer tutucusunu *benzersiz* bir SQL veritabanı adıyla değiştirin. Bu ad, genel olarak benzersiz SQL veritabanı uç noktasının parçası olarak kullanılır `<server-name>.database.windows.net` . Geçerli karakterler şunlardır `a` - `z` `0` - `9` `-` . Ayrıca, * \< DB-username>* ve * \< DB-Password>* yerine istediğiniz bir Kullanıcı adı ve parola koyun. 


```azurecli-interactive
az sql server create --name <server-name> --resource-group myResourceGroup --location "West Europe" --admin-user <db-username> --admin-password <db-password>
```

SQL Veritabanı mantıksal sunucusu oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

<pre>
{
  "administratorLogin": "&lt;db-username&gt;",
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

> [!TIP] 
> [Yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](../overview-inbound-outbound-ips.md#find-outbound-ips) güvenlik duvarı kurallarınızda daha da kısıtlayıcı olabilirsiniz.
>

Cloud Shell, * \< ıp adresi>* [yerel IPv4 IP adresinizle](https://www.whatsmyip.org/)değiştirerek yerel bilgisayarınızdan erişime izin vermek için komutu yeniden çalıştırın.

```azurecli-interactive
az sql server firewall-rule create --name AllowLocalClient --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address>
```

### <a name="create-a-database"></a>Veritabanı oluşturma

[`az sql db create`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-create) komutunu kullanarak sunucuda [S0 performans düzeyine](../../sql-database/sql-database-service-tiers-dtu.md) sahip bir veritabanı oluşturun.

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server-name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Bağlantı dizesi oluşturma

Komutunu kullanarak bağlantı dizesini alın [`az sql db show-connection-string`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-show-connection-string) .

```azurecli-interactive
az sql db show-connection-string --client ado.net --server cephalin-core --name coreDB
```

Komut çıkışında, * \< UserName>* ve * \< Password>* , daha önce kullandığınız veritabanı yönetici kimlik bilgileriyle değiştirin.

Bu, .NET Core uygulamanızın bağlantı dizesidir. Dizeyi daha sonra kullanmak üzere kopyalayın.

### <a name="configure-app-to-connect-to-production-database"></a>Uygulamayı üretim veritabanına bağlanacak şekilde yapılandırma

Yerel deponuzda, Startup.cs dosyasını açın ve aşağıdaki kodu bulun:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Aşağıdaki kodla değiştirin.

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
```

> [!IMPORTANT]
> Ölçeği ölçeklendirilmesi gereken üretim uygulamaları için [üretimde geçişleri uygulama](/aspnet/core/data/ef-rp/migrations#applying-migrations-in-production)konusunda en iyi uygulamaları izleyin.
> 

### <a name="run-database-migrations-to-the-production-database"></a>Veritabanı geçişlerini üretim veritabanına Çalıştır

Uygulamanız Şu anda yerel bir SQLite veritabanına bağlanıyor. Artık bir Azure SQL veritabanı yapılandırdığınıza göre, hedefi hedeflemek için ilk geçişi yeniden oluşturun. 

Depo kökünden aşağıdaki komutları çalıştırın. * \< Bağlantı dizesi>* , daha önce oluşturduğunuz bağlantı dizesiyle değiştirin.

```
# Delete old migrations
rm Migrations -r
# Recreate migrations
dotnet ef migrations add InitialCreate

# Set connection string to production database
# PowerShell
$env:ConnectionStrings:MyDbConnection="<connection-string>"
# CMD (no quotes)
set ConnectionStrings:MyDbConnection=<connection-string>
# Bash
export ConnectionStrings__MyDbConnection="<connection-string>"

# Run migrations
dotnet ef database update
```

### <a name="run-app-with-new-configuration"></a>Uygulamayı yeni yapılandırmayla Çalıştır

Artık veritabanı geçişleri üretim veritabanında çalıştırıldığına göre, aşağıdakileri çalıştırarak uygulamanızı test edin:

```
dotnet run
```

Bir tarayıcıda `http://localhost:5000` sayfasına gidin. **Yeni Oluştur** bağlantısını seçin ve _yapılacak_ birkaç iş oluşturun. Uygulamanız artık üretim veritabanına veri okuyor ve bu verileri yazıyor.

Yerel değişikliklerinizi işleyin ve git deponuza işleyin. 

```bash
git add .
git commit -m "connect to SQLDB in Azure"
```

Şimdi kodunuzu dağıtmaya hazırsınız demektir.

## <a name="deploy-app-to-azure"></a>Uygulamayı Azure’da dağıtma

Bu adımda, Linux’ta App Service üzerinde SQL Veritabanı’na bağlı .NET Core uygulamanızı dağıtırsınız.

### <a name="configure-local-git-deployment"></a>Yerel git dağıtımını yapılandırma

[!INCLUDE [Configure a deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-linux-no-h.md)] 

### <a name="configure-connection-string"></a>Bağlantı dizesini Yapılandır

Azure uygulamanıza yönelik bağlantı dizelerini ayarlamak için [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) Cloud Shell komutunu kullanın. Aşağıdaki komutta, * \< app-name>* ve * \< Connection-String>* parametresini daha önce oluşturduğunuz bağlantı dizesiyle değiştirin.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app-name> --settings MyDbConnection="<connection-string>" --connection-string-type SQLAzure
```

ASP.NET Core, bu adlandırılmış bağlantı dizesini ( `MyDbConnection` ), *appSettings. JSON*içinde belirtilen herhangi bir bağlantı dizesi gibi standart kalıbı kullanarak kullanabilirsiniz. Bu durumda, `MyDbConnection` *appSettings. JSON*' da de tanımlanmıştır. App Service çalışırken, App Service tanımlanan bağlantı dizesi *appSettings. JSON*' da tanımlanan bağlantı dizesi üzerinden önceliklidir. Kod, yerel geliştirme sırasında *appSettings. JSON* değerini kullanır ve aynı kod dağıtıldığında App Service değeri kullanır.

Kodunuzda bağlantı dizesinin nasıl başvurulduğunu görmek için bkz. [üretim veritabanına bağlanmak için uygulamayı yapılandırma](#configure-app-to-connect-to-production-database).

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [push-to-azure-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

<pre>
Enumerating objects: 273, done.
Counting objects: 100% (273/273), done.
Delta compression using up to 4 threads
Compressing objects: 100% (175/175), done.
Writing objects: 100% (273/273), 1.19 MiB | 1.85 MiB/s, done.
Total 273 (delta 96), reused 259 (delta 88)
remote: Resolving deltas: 100% (96/96), done.
remote: Deploy Async
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cccecf86c5'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
remote: .
remote: .
remote: .
remote: Done.
remote: Running post deployment command(s)...
remote: Triggering recycle (preview mode disabled).
remote: Deployment successful.
remote: Deployment Logs : 'https://&lt;app-name&gt;.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/cccecf86c56493ffa594e76ea1deb3abb3702d89/log'
To https://&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git
 * [new branch]      master -> master
</pre>

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına gidin

Web tarayıcınızı kullanarak dağıtılan uygulamaya gidin.

```bash
http://<app-name>.azurewebsites.net
```

Yapılacak birkaç iş ekleyin.

![Linux’ta App Service üzerinde çalışan uygulama](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

**Tebrikler!** Linux’ta App Service üzerinde veri temelli bir .NET Core uygulaması çalıştırıyorsunuz.

## <a name="update-locally-and-redeploy"></a>Yerel olarak güncelleştirme ve yeniden dağıtma

Bu adımda, veritabanı şemanızda bir değişiklik yapacak ve bunu Azure’da yayımlayacaksınız.

### <a name="update-your-data-model"></a>Veri modelinizi güncelleştirme

Kod düzenleyicisinde _modeller/Todo. cs_ öğesini açın. `ToDo` sınıfına aşağıdaki özelliği ekleyin:

```csharp
public bool Done { get; set; }
```

### <a name="rerun-database-migrations"></a>Veritabanı geçişlerini yeniden çalıştır

Üretim veritabanında güncelleştirme yapmak için birkaç komut çalıştırın.

```bash
dotnet ef migrations add AddProperty
dotnet ef database update
```

> [!NOTE]
> Yeni bir Terminal penceresi açarsanız, [üretim veritabanına veritabanı geçişlerini Çalıştır](#run-database-migrations-to-the-production-database)' da yaptığınız gibi, bağlantı dizesini terminaldeki üretim veritabanına ayarlamanız gerekir.
>

### <a name="use-the-new-property"></a>Yeni özelliği kullanma

`Done` özelliğini kullanarak kodunuzda birkaç değişiklik yapın. Bu öğreticide, daha kolay uygulama için, işlemin nasıl çalıştığını görmek üzere yalnızca `Index` ve `Create` görünümlerini değiştireceksiniz.

_Denetleyicileri/TodosController. cs_dosyasını açın.

`Create([Bind("ID,Description,CreatedDate")] Todo todo)` metodunu bulun ve `Done` değerini `Bind` özniteliğindeki özellik listesine ekleyin. Hazır olduğunuzda, `Create()` metot imzanız aşağıdaki koda benzer şekilde görünür:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

_Görünümleri/Todos/Create. cshtml_dosyasını açın.

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

_Görünümleri/Todos/Index. cshtml_dosyasını açın.

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

> [!NOTE]
> Yeni bir Terminal penceresi açarsanız, [üretim veritabanına veritabanı geçişlerini Çalıştır](#run-database-migrations-to-the-production-database)' da yaptığınız gibi, bağlantı dizesini terminaldeki üretim veritabanına ayarlamanız gerekir.
>

Tarayıcınızda `http://localhost:5000/` adresine gidin. Artık yapılacak bir öğe ekleyip öğeyi **Bitti** olarak işaretleyebilirsiniz. Daha sonra öğe, ana sayfanızda tamamlanmış bir öğe olarak görünmelidir. `Edit` görünümünü değiştirmediğinizden, `Edit` görünümünün `Done` alanında görünmediğini göz önünde bulundurun.

### <a name="publish-changes-to-azure"></a>Değişiklikleri Azure’da yayımlama

```bash
git add .
git commit -m "added done field"
git push azure master
```

Tamamlandıktan sonra `git push` App Service uygulamanıza gidin ve Yapılacaklar öğesi eklemeyi deneyin ve **Tamam**' ı işaretleyin.

![Code First geçişten sonra Azure uygulaması](./media/tutorial-dotnetcore-sqldb-app/this-one-is-done.png)

Mevcut yapılacak öğeleriniz görüntülenmeye devam eder. ASP.NET Core uygulamanızı yeniden yayımladığınızda, SQL veritabanınızdaki mevcut veriler kaybedilmez. Ayrıca, Entity Framework Code Migrations yalnızca veri şemasını değiştirir ve mevcut verilerinizde herhangi bir değişiklik yapmaz.

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

ASP.NET Core uygulama Azure App Service çalışırken konsol günlüklerini Cloud Shell üzerinden alabilirsiniz. Böylece, uygulama hatalarını ayıklamanıza yardımcı olan tanılama iletilerinin aynısını alabilirsiniz.

Örnek proje, iki yapılandırma değişikliğiyle [Azure 'da ASP.NET Core oturum açma](https://docs.microsoft.com/aspnet/core/fundamentals/logging#azure-app-service-provider) kılavuzundaki kılavuza zaten uyar:

- `Microsoft.Extensions.Logging.AzureAppServices` *Dotnetcoressqldb. csproj*içinde öğesine bir başvuru içerir.
- `loggerFactory.AddAzureWebAppDiagnostics()` *Program.cs*içindeki çağrılar.

App Service ASP.NET Core [günlük düzeyini](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-level) varsayılan düzeyden olarak ayarlamak için `Information` `Error` , [`az webapp log config`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-config) Cloud Shell komutunu kullanın.

```azurecli-interactive
az webapp log config --name <app-name> --resource-group myResourceGroup --application-logging true --level information
```

> [!NOTE]
> Projenin günlük düzeyi zaten `Information` *appSettings. JSON*içinde olarak ayarlanmıştır.
> 

Günlük akışını başlatmak için [`az webapp log tail`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-tail) Cloud Shell komutunu kullanın.

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group myResourceGroup
```

Günlük akışı başladıktan sonra bazı web trafiği almak için tarayıcıda Azure uygulamasını yenileyin. Artık konsol günlüklerinin terminale yöneltildiğini görebilirsiniz. Konsol günlüklerini hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

Günlük akışını istediğiniz zaman durdurmak için, yazın `Ctrl` + `C` .

ASP.NET Core günlüklerini özelleştirme hakkında daha fazla bilgi için bkz. [ASP.NET Core oturum açma](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="manage-your-azure-app"></a>Azure uygulamanızı yönetme

Oluşturduğunuz uygulamayı görmek için [Azure Portal](https://portal.azure.com) gidin.

Sol menüden **uygulama hizmetleri**' ne ve ardından Azure uygulamanızın adına tıklayın.

![Azure uygulamasına portal gezintisi](./media/tutorial-dotnetcore-sqldb-app/access-portal.png)

Varsayılan olarak, Portal uygulamanızın **genel bakış** sayfasını gösterir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

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

Özel bir DNS adını uygulamanıza nasıl eşleyeceğinizi öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: özel DNS adını uygulamanıza eşleyin](../app-service-web-tutorial-custom-domain.md)

Ya da diğer kaynaklara göz atın:

> [!div class="nextstepaction"]
> [ASP.NET Core uygulamasını yapılandırma](configure-language-dotnetcore.md)
