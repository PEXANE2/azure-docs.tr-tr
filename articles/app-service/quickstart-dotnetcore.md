---
title: 'Hızlı başlangıç: C# ASP.NET Core uygulaması oluşturma'
description: İlk ASP.NET Core uygulamanızı dağıtarak Azure App Service Web uygulamaları çalıştırmayı öğrenin.
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.topic: quickstart
ms.date: 08/01/2020
ms.custom: mvc, devcenter, vs-azure, seodec18
zone_pivot_groups: app-service-platform-windows-linux
ms.openlocfilehash: 61ab915ed709ab86e8420816b48036e915e5cfb5
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88085551"
---
# <a name="quickstart-create-an-aspnet-core-web-app-in-azure"></a>Hızlı başlangıç: Azure 'da ASP.NET Core Web uygulaması oluşturma

::: zone pivot="platform-windows"  

Bu hızlı başlangıçta, [Azure App Service](overview.md)için ilk ASP.NET Core Web uygulamanızı nasıl oluşturacağınızı ve dağıtacağınızı öğreneceksiniz. 

İşiniz bittiğinde, bir App Service barındırma planından ve dağıtılan bir Web uygulamasına sahip bir App Service içeren bir Azure Kaynak grubunuz olacaktır.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/dotnet/).
- Bu hızlı başlangıçta Windows üzerinde App Service bir uygulama dağıtılır. _Linux_üzerinde App Service dağıtım yapmak için, bkz. [App Service bir .NET Core Web uygulaması oluşturma](./quickstart-dotnetcore.md).
- **ASP.net ve Web geliştirme** iş yüküyle <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> ' i yükledikten sonra.

  Visual Studio 2019 zaten yüklüyse:

  - **Help**  >  **Güncelleştirmeler için yardım denetimi**' ni seçerek en son güncelleştirmeleri Visual Studio 'ya yükler.
  - **Araçlar**  >  **Al araçlar ve Özellikler '** i seçerek iş yükünü ekleyin.


## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Aşağıdaki adımları izleyerek Visual Studio 'da bir ASP.NET Core Web uygulaması oluşturun:

1. Visual Studio 'Yu açın ve **Yeni proje oluştur**' u seçin.

1. **Yeni proje oluştur**' da, **ASP.NET Core Web uygulaması** ' nı seçin ve **C#** ' ın bu seçime Ilişkin dillerde listelendiğini onaylayın, ardından **İleri**' yi seçin.

1. **Yeni projenizi yapılandırın**bölümünde, Web uygulaması projenizi *Myfirstazurewebapp*olarak adlandırın ve **Oluştur**' u seçin.

   ![Web uygulaması projenizi yapılandırma](./media/quickstart-dotnetcore/configure-web-app-project.png)

1. Azure 'a herhangi bir türde ASP.NET Core Web uygulaması dağıtabilirsiniz, ancak bu hızlı başlangıç için **Web uygulaması** şablonunu seçin. **Kimlik doğrulamasının** **kimlik doğrulaması yok**olarak ayarlandığından ve başka bir seçeneğin seçilolmadığından emin olun. Ardından **Oluştur**’u seçin.

   ![Yeni bir ASP.NET Core Web uygulaması oluşturma](./media/quickstart-dotnetcore/create-aspnet-core-web-app.png) 
   
1. **Debug**  >  Web uygulamanızı yerel olarak çalıştırmak için Visual Studio menüsünden hata ayıklama**olmadan Başlat** ' ı seçin.

   ![Yerel olarak çalışan Web uygulaması](./media/quickstart-dotnetcore/web-app-running-locally.png)

## <a name="publish-your-web-app"></a>Web uygulamanızı yayımlama

Web uygulamanızı yayımlamak için öncelikle uygulamanızı yayımlayacağınız yeni bir App Service oluşturmanız ve yapılandırmanız gerekir. 

App Service ayarlamanın bir parçası olarak şunları oluşturursunuz:

- Hizmet için tüm Azure kaynaklarını içerecek yeni bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/management/overview#terminology) .
- Uygulamanızı barındıran Web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirten yeni bir [barındırma planı](https://docs.microsoft.com/azure/app-service/overview-hosting-plans) .

App Service oluşturup Web uygulamanızı yayımlamak için aşağıdaki adımları izleyin:

1. **Çözüm Gezgini**, **Myfirstazurewebapp** projesine sağ tıklayın ve **Yayımla**' yı seçin. Daha önce Visual Studio 'dan Azure hesabınızda oturum açmadıysanız, **Hesap Ekle** veya **oturum aç**' ı seçin. Ayrıca, ücretsiz bir Azure hesabı da oluşturabilirsiniz.

1. **Bir yayımlama hedefi seç** iletişim kutusunda **App Service**' i seçin, **Yeni oluştur**' u seçin ve ardından **Profil oluştur**' u seçin.

   ![Yayımlama hedefi seçme](./media/quickstart-dotnetcore/pick-publish-target-vs2019.png)

1. **App Service: Yeni oluştur** iletişim kutusunda varsayılan adı kabul ederek ya da yeni bir ad girerek uygulamanız için genel olarak benzersiz bir **ad** sağlayın. Geçerli karakterler: `a-z` , `A-Z` , `0-9` , ve `-` . Bu **ad** , Web uygulamanızın URL ön eki olarak biçiminde kullanılır `http://<app_name>.azurewebsites.net` .

1. **Abonelik**için, listelenen aboneliği kabul edin veya açılan listeden yeni bir tane seçin.

1. **Kaynak grubu**' nda **Yeni**' yi seçin. **Yeni kaynak grubu adı**alanına *Myresourcegroup* yazın ve **Tamam**' ı seçin. 

1. **Barındırma planı**için **Yeni**' yi seçin. 

1. **Barındırma planı: Yeni oluştur** iletişim kutusunda, aşağıdaki tabloda belirtilen değerleri girin:

   | Ayar  | Önerilen Değer | Açıklama |
   | -------- | --------------- | ----------- |
   | **Barındırma Planı**  | *myFirstAzureWebAppPlan* | App Service planının adı. |
   | **Konum**      | *West Europe* | Web uygulamasının barındırıldığı veri merkezi. |
   | **Boyut**          | *Ücretsiz* | [Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), barındırma özelliklerini belirler. |
   
   ![Yeni barındırma planı oluştur](./media/quickstart-dotnetcore/create-new-hosting-plan-vs2019.png)

1. **Application Insights** , *none*olarak ayarlı bırakın.

1. **App Service: Yeni oluştur** iletişim kutusunda, Azure kaynaklarını oluşturmaya başlamak için **Oluştur** ' u seçin.

   ![Yeni App Service oluştur](./media/quickstart-dotnetcore/create-new-app-service-vs2019.png)

1. Sihirbaz tamamlandıktan sonra **Yayımla**' yı seçin.

   ![Web uygulamasını Azure 'da yayımlama](./media/quickstart-dotnetcore/publish-web-app-vs2019.png)

   Visual Studio ASP.NET Core Web uygulamanızı Azure 'da yayımlar ve uygulamayı varsayılan tarayıcınızda başlatır. 

   ![Azure 'da çalışan yayımlanmış ASP.NET Web uygulaması](./media/quickstart-dotnetcore/web-app-running-live.png)

**Tebrikler!** ASP.NET Core Web uygulamanız Azure App Service canlı olarak çalışıyor.

## <a name="update-the-app-and-redeploy"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

Web uygulamanızı güncelleştirmek ve yeniden dağıtmak için aşağıdaki adımları izleyin:

1. **Çözüm Gezgini**' de, projeniz altında, **Sayfalar**  >  **Index. cshtml**dosyasını açın.

1. Tüm `<div>` etiketi aşağıdaki kodla değiştirin:

   ```html
   <div class="jumbotron">
       <h1>ASP.NET in Azure!</h1>
       <p class="lead">This is a simple app that we've built that demonstrates how to deploy a .NET app to Azure App Service.</p>
   </div>
   ```

1. Azure’a yeniden dağıtmak için **Çözüm Gezgini**’nde **myFirstAzureWebApp** projesine sağ tıklayıp **Yayımla**’yı seçin.

1. **Yayımla** Özeti sayfasında **Yayımla**' yı seçin.

   ![Web uygulamasında güncelleştirme yayımlama](./media/quickstart-dotnetcore/publish-update-to-web-app-vs2019.png)

Yayımlama tamamlandığında Visual Studio, web uygulamasının URL’si ile bir tarayıcı başlatır.

![Azure 'da çalışan güncelleştirilmiş ASP.NET Web uygulaması](./media/quickstart-dotnetcore/updated-web-app-running-live.png)

## <a name="manage-the-azure-app"></a>Azure uygulamasını yönetme

Web uygulamanızı yönetmek için [Azure Portal](https://portal.azure.com)gidin ve **uygulama hizmetleri**' ni arayıp seçin.

![Uygulama hizmetlerini seçin](./media/quickstart-dotnetcore/app-services.png)

**Uygulama hizmetleri** sayfasında, Web uygulamanızın adını seçin.

![Azure uygulamasına portal gezintisi](./media/quickstart-dotnetcore/select-app-service.png)

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
[Linux’ta App Service](overview.md#app-service-on-linux) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu hızlı başlangıçta Linux üzerinde App Service’te [.NET Core](https://docs.microsoft.com/aspnet/core/) uygulaması oluşturma gösterilmektedir. Uygulamayı [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)kullanarak oluşturur ve .NET Core kodunu uygulamaya dağıtmak için git 'i kullanırsınız.

![Azure'da çalışan örnek uygulama](media/quickstart-dotnetcore/dotnet-browse-azure.png)

Mac, Windows veya Linux makinesi kullanarak bu makaledeki adımları izleyebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* <a href="https://git-scm.com/" target="_blank">Git'i yükleyin</a>
* <a href="https://dotnet.microsoft.com/download/dotnet-core/3.1" target="_blank">En son .NET Core 3,1 SDK 'sını yükler</a>

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

NuGet paketlerini geri yükleyip uygulamayı çalıştırın.

```bash
dotnet run
```

Bir web tarayıcısı açın ve `http://localhost:5000` konumundaki uygulamaya gidin.

Sayfada gösterilen örnek uygulamada **Hello World** iletisini görebilirsiniz.

![Tarayıcı ile test etme](media/quickstart-dotnetcore/dotnet-browse-local.png)

Terminal pencerenizde **Ctrl+C** tuşlarına basarak web sunucusundan çıkın. .NET Core projesi için bir Git deposu başlatın.

```bash
git init
git add .
git commit -m "first commit"
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app-dotnetcore-linux-no-h.md)]

Yeni oluşturduğunuz uygulamanıza gidin. _ &lt; App-name>_ değerini uygulamanızın adıyla değiştirin.

```bash
https://<app-name>.azurewebsites.net
```

Yeni uygulamanızın şöyle görünmesi gerekir:

![Boş uygulama sayfası](media/quickstart-dotnetcore/dotnet-browse-created.png)

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

<pre>
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 285 bytes | 95.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Deploy Async
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'd6b54472f7'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
remote:
remote: Oryx Version      : 0.2.20200114.13, Commit: 204922f30f8e8d41f5241b8c218425ef89106d1d, ReleaseTagName: 20200114.13
remote: Build Operation ID: |imoMY2y77/s=.40ca2a87_
remote: Repository Commit : d6b54472f7e8e9fd885ffafaa64522e74cf370e1
.
.
.
remote: Deployment successful.
remote: Deployment Logs : 'https://&lt;app-name&gt;.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/d6b54472f7e8e9fd885ffafaa64522e74cf370e1/log'
To https://&lt;app-name&gt;.scm.azurewebsites.net:443/&lt;app-name&gt;.git
   d87e6ca..d6b5447  master -> master
</pre>

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak, dağıtılan uygulamanın konumuna gidin.

```bash
http://<app_name>.azurewebsites.net
```

.NET Core örnek kodu, yerleşik bir görüntüyle Linux üzerinde App Service çalışmaktadır.

![Azure'da çalışan örnek uygulama](media/quickstart-dotnetcore/dotnet-browse-azure.png)

**Tebrikler!** Linux’ta App Service’e ilk .NET Core uygulamanızı dağıttınız.

## <a name="update-and-redeploy-the-code"></a>Kodu güncelleştirme ve yeniden dağıtma

Yerel dizinde, _Startup.cs_ dosyasını açın. `context.Response.WriteAsync` yöntem çağrısında metinde küçük bir değişiklik yapın:

```csharp
await context.Response.WriteAsync("Hello Azure!");
```

Değişikliklerinizi Git’e işleyin ve ardından kod değişikliklerini Azure’a gönderin.

```bash
git commit -am "updated output"
git push azure master
```

Dağıtım tamamlandıktan sonra **Uygulamaya göz at** adımında açılan tarayıcı penceresine dönüp yenile öğesine dokunun.

![Azure'da çalışan güncelleştirilmiş örnek uygulama](media/quickstart-dotnetcore/dotnet-browse-azure-updated.png)

## <a name="manage-your-new-azure-app"></a>Yeni Azure uygulamanızı yönetin

Oluşturduğunuz uygulamayı yönetmek için <a href="https://portal.azure.com" target="_blank">Azure Portal</a> gidin.

Sol menüden **uygulama hizmetleri**' ne ve ardından Azure uygulamanızın adına tıklayın.

![Azure uygulamasına portal gezintisi](./media/quickstart-dotnetcore/portal-app-service-list.png)

Uygulamanızın genel bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. 

![Azure portalında App Service sayfası](media/quickstart-dotnetcore/portal-app-overview.png)

Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: SQL veritabanı ile uygulama ASP.NET Core](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [ASP.NET Core uygulamasını yapılandırma](configure-language-dotnetcore.md)

::: zone-end  
