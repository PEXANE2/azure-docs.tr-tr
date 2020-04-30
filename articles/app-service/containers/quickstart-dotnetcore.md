---
title: 'Hızlı başlangıç: Linux ASP.NET Core uygulaması çalıştırma'
description: İlk ASP.NET Core uygulamanızı App Service bir Linux kapsayıcısına dağıtarak Azure App Service Linux uygulamalarını kullanmaya başlayın.
keywords: azure app service, web uygulaması, dotnet, core, linux, oss
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.tgt_pltfrm: linux
ms.topic: quickstart
ms.date: 04/22/2020
ms.custom: mvc, cli-validate, seodec18
ms.openlocfilehash: 1eeb5bbd4b10ef660a50f40d6c1300b0ca214561
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82206688"
---
# <a name="create-an-aspnet-core-app-in-app-service-on-linux"></a>Linux üzerinde App Service ASP.NET Core uygulama oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Linux üzerinde App Service'e dağıtımı yapılır. _Windows_üzerinde App Service dağıtmak için bkz. [Azure 'Da ASP.NET Core uygulaması oluşturma](../app-service-web-get-started-dotnet.md).
>

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu hızlı başlangıçta Linux üzerinde App Service’te [.NET Core](https://docs.microsoft.com/aspnet/core/) uygulaması oluşturma gösterilmektedir. Uygulamayı [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)kullanarak oluşturur ve .NET Core kodunu uygulamaya dağıtmak için git 'i kullanırsınız.

![Azure'da çalışan örnek uygulama](media/quickstart-dotnetcore/dotnet-browse-azure.png)

Mac, Windows veya Linux makinesi kullanarak bu makaledeki adımları izleyebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

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

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-linux-no-h.md)]

Yeni oluşturduğunuz uygulamanıza gidin. _ &lt;App-name>_ değerini uygulamanızın adıyla değiştirin.

```bash
https://<app-name>.azurewebsites.net
```

Yeni uygulamanızın şöyle görünmesi gerekir:

![Boş uygulama sayfası](media/quickstart-dotnetcore/dotnet-browse-created.png)

[!INCLUDE [Push to Azure](../../../includes/app-service-web-git-push-to-azure.md)] 

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

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: SQL veritabanı ile uygulama ASP.NET Core](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [ASP.NET Core uygulamasını yapılandırma](configure-language-dotnetcore.md)
