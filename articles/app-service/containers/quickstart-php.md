---
title: 'Hızlı başlangıç: Linux üzerinde bir PHP uygulaması oluşturma'
description: İlk PHP uygulamanızı App Service bir Linux kapsayıcısına dağıtarak Azure App Service Linux uygulamalarını kullanmaya başlayın.
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.topic: quickstart
ms.date: 05/25/2020
ms.custom: mvc, cli-validatem seodec18
ms.openlocfilehash: f0307088d274c9afee8fb8816b41a6357b4ce9f2
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848150"
---
# <a name="create-a-php-app-in-app-service-on-linux"></a>Linux üzerinde App Service bir PHP uygulaması oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Linux üzerinde App Service'e dağıtımı yapılır. _Windows_üzerinde App Service dağıtmak için bkz. [Azure 'Da php uygulaması oluşturma](../app-service-web-get-started-php.md).
>

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu hızlı başlangıç öğreticisinde, [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)kullanarak Linux üzerinde Azure App SERVICE bir PHP uygulamasının nasıl dağıtılacağı gösterilmektedir.

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

Yeni oluşturduğunuz uygulamanızı yerleşik görüntüyle görmek için siteye gidin. _ &lt; Uygulama adını>_ uygulama adınızla değiştirin.

```bash
http://<app-name>.azurewebsites.net
```

Yeni uygulamanızın şöyle görünmesi gerekir:

![Boş uygulama sayfası](media/quickstart-php/app-service-web-service-created.png)

[!INCLUDE [Push to Azure](../../../includes/app-service-web-git-push-to-azure.md)] 

<pre>
Counting objects: 26, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (23/23), done.
Writing objects: 100% (26/26), 4.95 KiB | 0 bytes/s, done.
Total 26 (delta 9), reused 0 (delta 0)
remote: Deploy Async
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'df425ea6ef'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
remote: .
remote: .
remote: .
remote: Deployment successful.
remote: Deployment Logs : 'https://&lt;app-name&gt;.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/.../log'
To https://&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git
 * [new branch]      master -> master
</pre>

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak, dağıtılan uygulamanın konumuna gidin.

```bash
http://<app-name>.azurewebsites.net
```

PHP örnek kodu, yerleşik görüntüyle Linux üzerinde App Service çalışmaktadır.

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

## <a name="manage-your-new-azure-app"></a>Yeni Azure uygulamanızı yönetin

Oluşturduğunuz uygulamayı yönetmek için <a href="https://portal.azure.com" target="_blank">Azure Portal</a> gidin.

Sol menüden **uygulama hizmetleri**' ne ve ardından Azure uygulamanızın adına tıklayın.

![Azure uygulamasına portal gezintisi](./media/quickstart-php/php-docs-hello-world-app-service-list.png)

Uygulamanızın genel bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

![Azure portalında App Service sayfası](media/quickstart-php/php-docs-hello-world-app-service-detail.png)

Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MySQL ile PHP uygulaması](tutorial-php-mysql-app.md)

> [!div class="nextstepaction"]
> [PHP uygulamasını yapılandırma](configure-language-php.md)
