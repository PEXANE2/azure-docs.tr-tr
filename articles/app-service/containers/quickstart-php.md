---
title: "Quickstart: Linux'ta bir PHP uygulaması oluşturun"
description: İlk PHP uygulamanızı App Service'teki bir Linux konteynerine dağıtarak Azure Uygulama Hizmeti'ndeki Linux uygulamalarıyla başlayın.
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.topic: quickstart
ms.date: 03/27/2019
ms.custom: mvc, cli-validatem seodec18
ms.openlocfilehash: 5a2abaf49071c90ea4fe0d5b5a454ce91f2cb1e4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80046065"
---
# <a name="create-a-php-app-in-app-service-on-linux"></a>Linux'ta App Service'de PHP uygulaması oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Linux üzerinde App Service'e dağıtımı yapılır. _Windows'da_Uygulama Hizmeti'ne dağıtmak için [azure'da php uygulaması oluşturma](../app-service-web-get-started-php.md)'na bakın.
>

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu hızlı başlangıç öğreticisi, [Cloud Shell'i](https://docs.microsoft.com/azure/cloud-shell/overview)kullanarak Linux'ta Bir PHP uygulamasının Linux'taki Azure Uygulama Hizmetine nasıl dağıtılanın caiz olduğunu gösterir.

![Azure'da çalışan örnek uygulama](media/quickstart-php/hello-world-in-browser.png)

Mac, Windows veya Linux makinesi kullanarak bu makaledeki adımları izleyebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* <a href="https://git-scm.com/" target="_blank">Git'i yükleyin</a>
* <a href="https://php.net" target="_blank">PHP'yi yükleyin</a>

## <a name="download-the-sample"></a>Örneği indirme

Bir terminal penceresinde, örnek uygulamayı yerel makinenize kopyalamak ve örnek kodu içeren dizine gitmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Azure'a dağıttığınızda nasıl görüneceğini görmek için uygulamayı yerel olarak çalıştırın. Yerleşik PHP web sunucusunu başlatmak için bir terminal penceresi açın ve `php` komutunu kullanın.

```bash
php -S localhost:8080
```

Bir web tarayıcısı açın ve `http://localhost:8080` konumundaki örnek uygulamaya gidin.

Sayfada gösterilen örnek uygulamada **Merhaba Dünya!** iletisini görürsünüz.

![Yerel olarak çalışan örnek uygulama](media/quickstart-php/localhost-hello-world-in-browser.png)

Terminal pencerenizde **Ctrl+C** tuşlarına basarak web sunucusundan çıkın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-php-linux-no-h.md)] 

Yeni oluşturulan uygulamanızı yerleşik görüntüyle görmek için siteye göz atın. _ &lt;Uygulama adını>_ uygulama adınız ile değiştirin.

```bash
http://<app_name>.azurewebsites.net
```

Yeni uygulamanız şu şekilde görünmelidir:

![Boş uygulama sayfası](media/quickstart-php/app-service-web-service-created.png)

[!INCLUDE [Push to Azure](../../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 2, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 352 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '25f18051e9'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.php'
remote: Ignoring: .git
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak, dağıtılan uygulamanın konumuna gidin.

```bash
http://<app_name>.azurewebsites.net
```

PHP örnek kodu, Yerleşik görüntüyle Linux'taki App Service'de çalışıyor.

![Azure'da çalışan örnek uygulama](media/quickstart-php/hello-world-in-browser.png)

**Tebrikler!** Linux’ta App Service’e ilk PHP uygulamanızı dağıttınız.

## <a name="update-locally-and-redeploy-the-code"></a>Kodu yerel makinede güncelleştirme ve yeniden dağıtma

Yerel dizinde, `index.php` dosyasını PHP uygulaması içinde açın ve `echo` öğesinin yanındaki dizenin içinde bulunan metinde küçük bir değişiklik yapın:

```php
echo "Hello Azure!";
```

Değişikliklerinizi Git’e işleyin ve ardından kod değişikliklerini Azure’a gönderin.

```bash
git commit -am "updated output"
git push azure master
```

Dağıtım tamamlandıktan sonra **Uygulamaya göz atma** adımında açılan tarayıcı penceresine dönüp sayfayı yenileyin.

![Azure'da çalışan güncelleştirilmiş örnek uygulama](media/quickstart-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-app"></a>Yeni Azure uygulamanızı yönetme

Oluşturduğunuz uygulamayı yönetmek için <a href="https://portal.azure.com" target="_blank">Azure portalına</a> gidin.

Sol menüden **Uygulama Hizmetleri'ni**ve ardından Azure uygulamanızın adını tıklatın.

![Azure uygulamasına portal gezintisi](./media/quickstart-php/php-docs-hello-world-app-service-list.png)

Uygulamanızın Genel Bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

![Azure portalında App Service sayfası](media/quickstart-php/php-docs-hello-world-app-service-detail.png)

Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MySQL ile PHP uygulaması](tutorial-php-mysql-app.md)

> [!div class="nextstepaction"]
> [PHP uygulamasını yapılandırma](configure-language-php.md)
