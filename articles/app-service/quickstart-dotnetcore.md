---
title: 'Hızlı başlangıç: C# ASP.NET Core uygulaması oluşturma'
description: İlk ASP.NET Core uygulamanızı dağıtarak Azure App Service Web uygulamaları çalıştırmayı öğrenin.
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.topic: quickstart
ms.date: 11/23/2020
ms.custom: devx-track-csharp, mvc, devcenter, vs-azure, seodec18, contperf-fy21q1
zone_pivot_groups: app-service-platform-windows-linux
adobe-target: true
adobe-target-activity: DocsExp–386541–A/B–Enhanced-Readability-Quickstarts–2.19.2021
adobe-target-experience: Experience B
adobe-target-content: ./quickstart-dotnetcore-uiex
ms.openlocfilehash: 77e0768a617ef79ab8510f88bfdcd41d5647f9bf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101701646"
---
# <a name="quickstart-create-an-aspnet-core-web-app-in-azure"></a>Hızlı başlangıç: Azure 'da ASP.NET Core Web uygulaması oluşturma

::: zone pivot="platform-windows"  

Bu hızlı başlangıçta, [Azure App Service](overview.md)için ilk ASP.NET Core Web uygulamanızı nasıl oluşturacağınızı ve dağıtacağınızı öğreneceksiniz. App Service .NET 5,0 uygulamalarını destekler.

İşiniz bittiğinde, bir App Service barındırma planından ve dağıtılan bir Web uygulamasına sahip bir App Service içeren bir Azure Kaynak grubunuz olacaktır.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/dotnet/).
- **ASP.net ve Web geliştirme** iş yüküyle <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> ' i yükledikten sonra.

  Visual Studio 2019 zaten yüklüyse:

  -   >  **Güncelleştirmeler için yardım denetimi**' ni seçerek en son güncelleştirmeleri Visual Studio 'ya yükler. En son güncelleştirmeler .NET 5,0 SDK 'sını içerir.
  - **Araçlar**  >  **Al araçlar ve Özellikler '** i seçerek iş yükünü ekleyin.


## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Aşağıdaki adımları izleyerek Visual Studio 'da bir ASP.NET Core Web uygulaması oluşturun:

# <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

1. Visual Studio 'Yu açın ve **Yeni proje oluştur**' u seçin.

1. **Yeni proje oluştur**' da, **ASP.NET Core Web uygulaması** ' nı seçin ve **C#** ' ın bu seçime Ilişkin dillerde listelendiğini onaylayın, ardından **İleri**' yi seçin.

1. **Yeni projenizi yapılandırın** bölümünde, Web uygulaması projenizi *Myfirstazurewebapp* olarak adlandırın ve **Oluştur**' u seçin.

   ![Web uygulaması projenizi yapılandırma](./media/quickstart-dotnetcore/configure-web-app-project.png)

1. Azure 'a herhangi bir türde ASP.NET Core Web uygulaması dağıtabilirsiniz, ancak bu hızlı başlangıç için **Web uygulaması** şablonunu seçin. **Kimlik doğrulamasının** **kimlik doğrulaması yok** olarak ayarlandığından ve başka bir seçeneğin seçilolmadığından emin olun. Ardından **Oluştur**’u seçin.

   ![Yeni bir ASP.NET Core Web uygulaması oluşturma](./media/quickstart-dotnetcore/create-aspnet-core-web-app.png) 
   
1.   >  Web uygulamanızı yerel olarak çalıştırmak için Visual Studio menüsünden hata ayıklama **olmadan Başlat** ' ı seçin.

   ![Yerel olarak çalışan Web uygulaması](./media/quickstart-dotnetcore/web-app-running-locally.png)

# <a name="net-50"></a>[.NET 5.0](#tab/net50)

1. Visual Studio 'Yu açın ve **Yeni proje oluştur**' u seçin.

1. **Yeni proje oluştur**' da, **ASP.NET Core Web uygulaması** ' nı seçin ve **C#** ' ın bu seçime Ilişkin dillerde listelendiğini onaylayın, ardından **İleri**' yi seçin.

1. **Yeni projenizi yapılandırın** bölümünde, Web uygulaması projenizi *Myfirstazurewebapp* olarak adlandırın ve **Oluştur**' u seçin.

   ![Web uygulaması projenizi yapılandırma](./media/quickstart-dotnetcore/configure-web-app-project.png)

1. Bir .NET 5,0 uygulaması için, açılan menüde **ASP.NET Core 5,0** ' i seçin.

1. Azure 'a herhangi bir türde ASP.NET Core Web uygulaması dağıtabilirsiniz, ancak bu hızlı başlangıç için, **ASP.NET Core Web uygulaması** şablonunu seçin. **Kimlik doğrulamasının** **kimlik doğrulaması yok** olarak ayarlandığından ve başka bir seçeneğin seçilolmadığından emin olun. Ardından **Oluştur**’u seçin.

   ![Yeni bir ASP.NET Core Web uygulaması oluşturma](./media/quickstart-dotnetcore/create-aspnet-core-web-app-5.png) 
   
1.   >  Web uygulamanızı yerel olarak çalıştırmak için Visual Studio menüsünden hata ayıklama **olmadan Başlat** ' ı seçin.

   ![Yerel olarak çalışan Web uygulaması](./media/quickstart-dotnetcore/web-app-running-locally.png)

---

## <a name="publish-your-web-app"></a>Web uygulamanızı yayımlama

Web uygulamanızı yayımlamak için öncelikle uygulamanızı yayımlayacağınız yeni bir App Service oluşturmanız ve yapılandırmanız gerekir. 

App Service ayarlamanın bir parçası olarak şunları oluşturursunuz:

- Hizmet için tüm Azure kaynaklarını içerecek yeni bir [kaynak grubu](../azure-resource-manager/management/overview.md#terminology) .
- Uygulamanızı barındıran Web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirten yeni bir [barındırma planı](./overview-hosting-plans.md) .

App Service oluşturup Web uygulamanızı yayımlamak için aşağıdaki adımları izleyin:

1. **Çözüm Gezgini**, **Myfirstazurewebapp** projesine sağ tıklayın ve **Yayımla**' yı seçin. 

1. **Yayımla**' da **Azure** ' ı seçin ve **İleri**' ye tıklayın.

1. Seçenekleriniz, Azure 'da oturum açmış olup olmadığınız ve bir Azure hesabına bağlı bir Visual Studio hesabınız olup olmadığına bağlıdır. Azure aboneliğinizde oturum açmak için **Hesap Ekle** veya **oturum aç** seçeneklerinden birini belirleyin. Zaten oturum açtıysanız istediğiniz hesabı seçin.

   ![Azure'da oturum açma](./media/quickstart-dotnetcore/sign-in-azure-vs2019.png)

1. **App Service örneklerinin** sağında, ' ye tıklayın **+** .

   ![Yeni App Service uygulaması](./media/quickstart-dotnetcore/publish-new-app-service.png)

1. **Abonelik** için, listelenen aboneliği kabul edin veya açılan listeden yeni bir tane seçin.

1. **Kaynak grubu** için **Yeni**' yi seçin. **Yeni kaynak grubu adı** alanına *Myresourcegroup* yazın ve **Tamam**' ı seçin. 

1. **Barındırma planı** için **Yeni**' yi seçin. 

1. **Barındırma planı: Yeni oluştur** iletişim kutusunda, aşağıdaki tabloda belirtilen değerleri girin:

   | Ayar  | Önerilen Değer | Description |
   | -------- | --------------- | ----------- |
   | **Barındırma Planı**  | *myFirstAzureWebAppPlan* | App Service planının adı. |
   | **Konum**      | *West Europe* | Web uygulamasının barındırıldığı veri merkezi. |
   | **Boyut**          | *Ücretsiz* | [Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), barındırma özelliklerini belirler. |
   
   ![Yeni barındırma planı oluştur](./media/quickstart-dotnetcore/create-new-hosting-plan-vs2019.png)

1. **Ad** alanına,,, ve yalnızca geçerli karakterleri içeren benzersiz bir uygulama adı girin `a-z` `A-Z` `0-9` `-` . Otomatik olarak oluşturulan benzersiz adı kabul edebilirsiniz. Web uygulamasının URL'si `http://<app-name>.azurewebsites.net` şeklindedir; burada `<app-name>`, uygulamanızın adıdır.

2. Azure kaynaklarını oluşturmak için **Oluştur** ' u seçin.

   ![Uygulama kaynakları oluşturma](./media/quickstart-dotnetcore/web-app-name-vs2019.png)

   Sihirbaz tamamlandıktan sonra Azure kaynakları sizin için oluşturulur ve yayımlamaya hazırlarsınız.

3. Sihirbazı kapatmak için **son** ' u seçin.

1. **Yayımla** sayfasında **Yayımla**' ya tıklayın. Visual Studio uygulamayı oluşturur, paketler ve Azure 'da yayımlar ve ardından uygulamayı varsayılan tarayıcıda başlatır.

   ![Azure 'da çalışan yayımlanmış ASP.NET Web uygulaması](./media/quickstart-dotnetcore/web-app-running-live.png)

**Tebrikler!** ASP.NET Core Web uygulamanız Azure App Service canlı olarak çalışıyor.

## <a name="update-the-app-and-redeploy"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

Web uygulamanızı güncelleştirmek ve yeniden dağıtmak için aşağıdaki adımları izleyin:

1. **Çözüm Gezgini**' de, projeniz altında, **Sayfalar**  >  **Index. cshtml** dosyasını açın.

1. Tüm `<div>` etiketi aşağıdaki kodla değiştirin:

   ```html
   <div class="jumbotron">
       <h1>ASP.NET in Azure!</h1>
       <p class="lead">This is a simple app that we've built that demonstrates how to deploy a .NET app to Azure App Service.</p>
   </div>
   ```

1. Azure’a yeniden dağıtmak için **Çözüm Gezgini**’nde **myFirstAzureWebApp** projesine sağ tıklayıp **Yayımla**’yı seçin.

1. **Yayımla** Özeti sayfasında **Yayımla**' yı seçin.

   <!-- ![Publish update to web app](./media/quickstart-dotnetcore/publish-update-to-web-app-vs2019.png) -->

    Yayımlama tamamlandığında Visual Studio, web uygulamasının URL’si ile bir tarayıcı başlatır.

    ![Azure 'da çalışan güncelleştirilmiş ASP.NET Web uygulaması](./media/quickstart-dotnetcore/updated-web-app-running-live.png)

## <a name="manage-the-azure-app"></a>Azure uygulamasını yönetme

Web uygulamanızı yönetmek için [Azure Portal](https://portal.azure.com)gidin ve **uygulama hizmetleri**' ni arayıp seçin.

![Uygulama hizmetlerini seçin](./media/quickstart-dotnetcore/app-services.png)

**Uygulama hizmetleri** sayfasında, Web uygulamanızın adını seçin.

:::image type="content" source="./media/quickstart-dotnetcore/select-app-service.png" alt-text="Örnek bir Web uygulaması seçili olan App Services sayfasının ekran görüntüsü.":::

Web uygulamanıza ilişkin **genel bakış** sayfası, göz at, durdur, Başlat, yeniden Başlat ve Sil gibi temel yönetime yönelik seçenekler içerir. Sol menü, uygulamanızı yapılandırmak için daha fazla sayfa sağlar.

![Azure portal App Service](./media/quickstart-dotnetcore/web-app-overview-page.png)

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure App Service bir ASP.NET Core Web uygulaması oluşturmak ve dağıtmak için Visual Studio 'Yu kullandınız.

Bir .NET Core uygulaması oluşturmayı ve bir SQL veritabanına bağlamayı öğrenmek için bir sonraki makaleye ilerleyin:

> [!div class="nextstepaction"]
> [SQL Veritabanı ile ASP.NET Core](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [ASP.NET Core uygulamasını yapılandırma](configure-language-dotnetcore.md)

::: zone-end  

::: zone pivot="platform-linux"
[Linux’ta App Service](overview.md#app-service-on-linux) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu hızlı başlangıçta, bir [.NET Core](/aspnet/core/) uygulamasının nasıl oluşturulacağı ve [Azure CLI](/cli/azure/get-started-with-azure-cli)kullanılarak Linux tarafından barındırılan bir App Service nasıl dağıtılacağı gösterilmektedir.

![Azure'da çalışan örnek uygulama](media/quickstart-dotnetcore/dotnet-browse-azure.png)

Mac, Windows veya Linux makinesi kullanarak bu makaledeki adımları izleyebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-initial-environment"></a>İlk ortamınızı ayarlama

# <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

Bu hızlı başlangıcı tamamlamak için:

* <a href="https://dotnet.microsoft.com/download/dotnet-core/3.1" target="_blank">En son .NET Core 3,1 SDK 'Sını yükler</a>.
* <a href="/cli/azure/install-azure-cli" target="_blank">Azure CLI’nin son sürümünü yükleyin</a>.

# <a name="net-50"></a>[.NET 5.0](#tab/net50)

Bu hızlı başlangıcı tamamlamak için:

* <a href="https://dotnet.microsoft.com/download/dotnet/5.0" target="_blank">En son .net 5,0 SDK 'Sını yükler</a>.
* <a href="/cli/azure/install-azure-cli" target="_blank">Azure CLI’nin son sürümünü yükleyin</a>.

---

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/DotNetAppServiceLinuxQuickStart)

## <a name="create-the-app-locally"></a>Uygulamayı yerel olarak oluşturma

Makinenizde bir terminal penceresinde, `hellodotnetcore` adlı bir dizin oluşturup geçerli dizin olarak değiştirin.

```bash
mkdir hellodotnetcore
cd hellodotnetcore
```

Yeni bir .NET Core uygulaması oluşturun.

```bash
dotnet new web
```

## <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Azure'a dağıttığınızda nasıl görüneceğini görmek için uygulamayı yerel olarak çalıştırın. 

```bash
dotnet run
```

Bir web tarayıcısı açın ve `http://localhost:5000` konumundaki uygulamaya gidin.

Sayfada gösterilen örnek uygulamada **Hello World** iletisini görebilirsiniz.

![Tarayıcı ile test etme](media/quickstart-dotnetcore/dotnet-browse-local.png)

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/DotNetAppServiceLinuxQuickStart)

## <a name="sign-into-azure"></a>Azure oturumu açma
Terminal pencerenizde aşağıdaki komutla Azure 'da oturum açın:

```azurecli
az login
```

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Şu komutu kullanarak kodu yerel klasörünüzde (*hellodotnetcore*) dağıtın `az webapp up` :

```azurecli
az webapp up --sku F1 --name <app-name>
```

- `az`Komut tanınmazsa, [Ilk ortamınızı ayarlama](#set-up-your-initial-environment)bölümünde AÇıKLANDıĞı gibi Azure CLI 'nin yüklü olduğundan emin olun.
- `<app-name>`Tüm Azure genelinde benzersiz olan bir adla değiştirin (*geçerli karakterler `a-z` , `0-9` ve `-`*). İyi bir model, şirketinizin adının ve uygulama tanımlayıcısının bir birleşimini kullanmaktır.
- `--sku F1`Bağımsız değişkeni, ücretsiz fiyatlandırma katmanında Web uygulaması oluşturur. Saatlik maliyet içeren daha hızlı bir Premium katmanı kullanmak için bu bağımsız değişkeni atlayın.
- İsteğe bağlı olarak `--location <location-name>` `<location-name>` , kullanılabilir bir Azure bölgesi olan bağımsız değişkeni ekleyebilirsiniz. Komutunu çalıştırarak, Azure hesabınız için izin verilen bölgelerin bir listesini alabilirsiniz [`az account list-locations`](/cli/azure/appservice#az-appservice-list-locations) .

Komutun tamamlanması birkaç dakika sürebilir. Çalıştırılırken, kaynak grubu oluşturma, App Service planı ve barındırma uygulaması, günlüğü yapılandırma ve ardından ZIP dağıtımı gerçekleştirme hakkında iletiler sağlar. Daha sonra, "uygulamayı &lt; &gt; Azure 'da uygulamanın URL 'si olan http://app-name. azurewebsites.net konumunda başlatabilirsiniz.

# <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

![Az WebApp up komutunun örnek çıktısı](./media/quickstart-dotnetcore/az-webapp-up-output-3.1.png)

# <a name="net-50"></a>[.NET 5.0](#tab/net50)

<!-- Deploy the code in your local folder (*hellodotnetcore*) using the `az webapp up` command:

```azurecli
az webapp up --sku B1 --name <app-name> --os-type linux
```

- If the `az` command isn't recognized, be sure you have the Azure CLI installed as described in [Set up your initial environment](#set-up-your-initial-environment).
- Replace `<app-name>` with a name that's unique across all of Azure (*valid characters are `a-z`, `0-9`, and `-`*). A good pattern is to use a combination of your company name and an app identifier.
- The `--sku B1` argument creates the web app in the Basic pricing tier, which incurs an hourly cost. Omit this argument to use a faster premium tier, which costs more.
- You can optionally include the argument `--location <location-name>` where `<location-name>` is an available Azure region. You can retrieve a list of allowable regions for your Azure account by running the [`az account list-locations`](/cli/azure/appservice#az-appservice-list-locations) command.

The command may take a few minutes to complete. While running, it provides messages about creating the resource group, the App Service plan and hosting app, configuring logging, then performing ZIP deployment. It then gives the message, "You can launch the app at http://&lt;app-name&gt;.azurewebsites.net", which is the app's URL on Azure. -->

![Az WebApp up komutunun örnek çıktısı](./media/quickstart-dotnetcore/az-webapp-up-output-5.0.png)

---

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/DotNetAppServiceLinuxQuickStart)

[!include [az webapp up command note](../../includes/app-service-web-az-webapp-up-note.md)]

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak, dağıtılan uygulamanın konumuna gidin.

```bash
http://<app_name>.azurewebsites.net
```

.NET Core örnek kodu, yerleşik bir görüntüyle Linux üzerinde App Service çalışmaktadır.

![Azure'da çalışan örnek uygulama](media/quickstart-dotnetcore/dotnet-browse-azure.png)

**Tebrikler!** Linux’ta App Service’e ilk .NET Core uygulamanızı dağıttınız.

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/DotNetAppServiceLinuxQuickStart)

## <a name="update-and-redeploy-the-code&quot;></a>Kodu güncelleştirme ve yeniden dağıtma

Yerel dizinde, _Startup.cs_ dosyasını açın. `context.Response.WriteAsync` yöntem çağrısında metinde küçük bir değişiklik yapın:

```csharp
await context.Response.WriteAsync(&quot;Hello Azure!");
```

Değişikliklerinizi kaydettikten sonra `az webapp up` komutu tekrar kullanarak uygulamayı yeniden dağıtın:

```azurecli
az webapp up --os-type linux
```

Bu komut, uygulama adı, kaynak grubu ve App Service planı dahil olmak üzere *. Azure/config* dosyasında yerel olarak önbelleğe alınan değerleri kullanır.

Dağıtım tamamlandıktan sonra **Uygulamaya göz at** adımında açılan tarayıcı penceresine dönüp yenile öğesine dokunun.

![Azure'da çalışan güncelleştirilmiş örnek uygulama](media/quickstart-dotnetcore/dotnet-browse-azure-updated.png)

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/DotNetAppServiceLinuxQuickStart)

## <a name="manage-your-new-azure-app"></a>Yeni Azure uygulamanızı yönetin

Oluşturduğunuz uygulamayı yönetmek için <a href="https://portal.azure.com" target="_blank">Azure Portal</a> gidin.

Sol menüden **uygulama hizmetleri**' ne ve ardından Azure uygulamanızın adına tıklayın.

:::image type="content" source="./media/quickstart-dotnetcore/portal-app-service-list-up.png" alt-text="Uygulama Hizmetleri sayfasının seçili örnek bir Azure uygulaması gösteren ekran görüntüsü.":::

Uygulamanızın genel bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. 

![Azure portalında App Service sayfası](media/quickstart-dotnetcore/portal-app-overview-up.png)

Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/DotNetAppServiceLinuxQuickStart)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: SQL veritabanı ile uygulama ASP.NET Core](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [ASP.NET Core uygulamasını yapılandırma](configure-language-dotnetcore.md)

::: zone-end
