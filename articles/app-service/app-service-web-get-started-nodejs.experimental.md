---
title: 'Quickstart: Bir Düğüm.js web uygulaması oluşturma'
description: İlk Düğüm.js Hello World'ünüzü birkaç dakika içinde Azure Uygulama Hizmetine dağıtın. Uygulama Hizmetine dağıtmanın birçok yollarından biri olan bir ZIP paketi kullanarak dağıtAbilirsiniz.
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 08/23/2019
ms.custom: mvc, devcenter, seodec18
experimental: false
experiment_id: a231f2b4-2625-4d
ms.openlocfilehash: 84953c79093ebd4e53e7e7feef5ab72b8afce002
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80047536"
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Azure App Service'te Node.js web uygulaması oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Windows üzerinde App Service'e dağıtımı yapılır. _Linux_ üzerinde App Service'e dağıtım yapmak için bkz. [Linux üzerinde Azure App Service'te Node.js web uygulaması oluşturma](./containers/quickstart-nodejs.md).
>

[Azure App Service](overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıç, Bir Düğüm.js uygulamasını Azure Uygulama Hizmeti'ne nasıl dağıtılanın gösterir. Web uygulamasını [Bulut Kabuğu'nu](https://docs.microsoft.com/azure/cloud-shell/overview)kullanarak oluşturursunuz, ancak bu komutları [Azure CLI](/cli/azure/install-azure-cli)ile yerel olarak da çalıştırabilirsiniz. Az [webapp dağıtım kaynağı config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) komutunu kullanarak örnek Node.js kodunu web uygulamasına dağırsınız.  

![Azure'da çalışan örnek uygulama](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Mac, Windows veya Linux makinesi kullanarak buradaki adımları izleyebilirsiniz. Adımları tamamlamak yaklaşık üç dakika sürer.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Örneği indirme

Cloud Shell'de bir quickstart dizini oluşturun ve o dizine geçin.

```console
mkdir quickstart

cd $HOME/quickstart
```

Ardından, örnek uygulama deposunu quickstart dizininize kopyalamak için aşağıdaki komutu çalıştırın.

```console
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:

```output
Cloning into 'nodejs-docs-hello-world'...
remote: Counting objects: 40, done.
remote: Total 40 (delta 0), reused 0 (delta 0), pack-reused 40
Unpacking objects: 100% (40/40), done.
Checking connectivity... done.
```

> [!NOTE]
> Örnek index.js dinleme bağlantı noktasını process.env.PORT olarak ayarlar. Bu ortam değişkeni App Service tarafından atanır.
>

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-scus.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-scus.md)]

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Bulut Kabuğu'nda, `myAppServicePlan` [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) Uygulama Hizmeti planında komutla birlikte bir web uygulaması oluşturun.

Aşağıdaki örnekte `<app_name>` kısmını genel olarak benzersiz bir uygulama adıyla değiştirin (geçerli karakterler `a-z`, `0-9` ve `-` şeklindedir).

```azurecli-interactive
# Bash and Powershell
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name>
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="set-nodejs-runtime"></a>Node.js çalışma zamanını ayarlama

Düğüm çalışma saatini 10.14.1 olarak ayarlayın. Desteklenen tüm çalışma sürelerini [`az webapp list-runtimes`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-list-runtimes)görmek için çalıştırın.

```azurecli-interactive
# Bash and Powershell
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITE_NODE_DEFAULT_VERSION=10.14.1
```

Yeni oluşturduğunuz web uygulamasına göz atın. Benzersiz `<app_name>` bir uygulama adı ile değiştirin.

```http
http://<app_name>.azurewebsites.net
```

İşte yeni web uygulamanız gibi ![görünmelidir: Boş web uygulaması sayfası](media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)

## <a name="deploy-zip-file"></a>ZIP dosyası dağıtma

Bulut Kabuğu'nda, uygulamanızın kök dizinine gidin ve örnek projeniz için yeni bir ZIP dosyası oluşturun.

```console
cd nodejs-docs-hello-world  

zip -r myUpdatedAppFiles.zip *.*
```

ZIP dosyasını [az webapp dağıtım kaynağı config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) komutunu kullanarak web uygulamanıza dağıtın.  

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src myUpdatedAppFiles.zip
```

Bu komut ZIP içindeki dosyaları ve dizinleri App Service uygulama klasörünüze (`\home\site\wwwroot`) dağıtır ve uygulamayı yeniden başlatır. Ek özel derleme işlemi yapılandırılmışsa bu işlemler de çalışır. Daha fazla bilgi için [Bkz. Kudu belgeleri.](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak, dağıtılan uygulamanın konumuna gidin.

```http
http://<app_name>.azurewebsites.net
```

Node.js örnek kodu bir Azure App Service web uygulamasında çalışıyor.

![Azure'da çalışan örnek uygulama](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

> [!NOTE]
> Azure App Service'te uygulama [iisnode](https://github.com/Azure/iisnode) kullanılarak IIS'de çalıştırılır. Uygulamanın iisnode ile çalıştırılmasını sağlamak için kök uygulama dizininde bir web.config dosyası bulunur. Bu dosya IIS tarafından okunabilir ve iisnode ile ilgili ayarlar [iisnode GitHub deposunda](https://github.com/Azure/iisnode/blob/master/src/samples/configuration/web.config) belgelenmiştir.

**Tebrikler!** App Service’e ilk Node.js uygulamanızı dağıttınız.

## <a name="update-and-redeploy-the-code"></a>Kodu güncelleştirme ve yeniden dağıtma

Bulut Kabuğu'nda, `code index.js` Cloud Shell düzenleyicisini açmak için yazın.

![Kod index.js](media/app-service-web-get-started-nodejs-poc/code-indexjs.png)

`response.end` çağrısında metinde küçük bir değişiklik yapın:

```javascript
response.end("Hello Azure!");
```

Değişikliklerinizi kaydedin ve editörden çıkın. Kaydetmek için `^S` ve çıkmak için `^Q` komutunu kullanın.

Zip dosyası oluşturun ve [az webapp dağıtım kaynağı config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) komutunu kullanarak dağıtın.  

```azurecli-interactive
# Bash
zip -r myUpdatedAppFiles.zip *.*

az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src myUpdatedAppFiles.zip
```

**Uygulamaya göz atma** adımında açılan tarayıcı penceresine dönüp sayfayı yenileyin.

![Azure'da çalışan güncelleştirilmiş örnek uygulama](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-app"></a>Yeni Azure uygulamanızı yönetme

Oluşturduğunuz web uygulamasını yönetmek için <a href="https://portal.azure.com" target="_blank">Azure portalına</a> gidin.

Sol menüden **Uygulama Hizmetleri'ni**ve ardından Azure uygulamanızın adını tıklatın.

![Azure uygulamasına portal gezintisi](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Web uygulamanızın Genel Bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

![Azure portalında App Service sayfası](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [MongoDB ile Node.js](app-service-web-tutorial-nodejs-mongodb-app.md)
