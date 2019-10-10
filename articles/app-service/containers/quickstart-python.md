---
title: Linux 'ta Python uygulaması oluşturma-Azure App Service | Microsoft Docs
description: İlk Python Hello World uygulamanızı birkaç dakika içinde Linux üzerinde Azure App Service dağıtın.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 08/23/2019
ms.author: cephalin
experimental: true
experiment_id: 1e304dc9-5add-4b
ms.openlocfilehash: 253e2e5015c2cb7a71386b88b4d86ac6bff6a0fe
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72242454"
---
# <a name="create-a-python-app-in-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service bir Python uygulaması oluşturma

Bu hızlı [Başlangıçta, Linux üzerinde App Service](app-service-linux-intro.md)basit bir Python uygulamasını dağıtarak, yüksek düzeyde ölçeklenebilir ve kendini yayama bir Web barındırma hizmeti sağlar. Azure komut satırı arabirimi ( [Azure CLI](/cli/azure/install-azure-cli)), etkileşimli, tarayıcı tabanlı Azure Cloud Shell aracılığıyla kullanılır. bu sayede, bir Mac, Linux veya Windows bilgisayarı kullanma adımlarını izleyebilirsiniz.

![Azure 'da çalışan örnek uygulama](media/quickstart-python/hello-world-in-browser.png)

## <a name="prerequisites"></a>Prerequisites

Bu hızlı başlangıcı gerçekleştirmek için:

* <a href="https://www.python.org/downloads/" target="_blank">Python 3,7 'yi yükler</a>
* <a href="https://git-scm.com/" target="_blank">Git 'i yükler</a>
* Bir Azure aboneliği. Henüz bir tane yoksa, başlamadan önce ücretsiz bir [Hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="download-the-sample-locally"></a>Örneği yerel olarak indirin

Bir Terminal penceresinde, örnek uygulamayı yerel makinenize kopyalamak ve örnek kodu içeren dizine gitmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
cd python-docs-hello-world
```

Depo, deponun bir Flask uygulaması içerdiğini App Service söyleyen bir *Application.py*içeriyor. Daha fazla bilgi için bkz. [kapsayıcı başlangıç işlemi ve özelleştirmeleri](how-to-configure-python.md).

## <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Uygulamayı Azure 'a dağıtırken nasıl bakacağınızı görmeniz için yerel olarak çalıştırın. Bir Terminal penceresi açın ve gerekli bağımlılıkları yüklemek ve yerleşik geliştirme sunucusunu başlatmak için aşağıdaki komutları kullanın. 

```bash
# In Bash (for Linux or Mac)
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
FLASK_APP=application.py flask run
```
```powershell
# In Powershell (for Windows)
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
Set-Item Env:FLASK_APP ".\application.py"
flask run
```

Bir Web tarayıcısı açın ve `http://localhost:5000/` ' da örnek uygulamaya gidin.

**Merhaba Dünya görürsünüz!** sayfada görünen örnek uygulamadaki ileti.

![Yerel olarak çalışan örnek uygulama](media/quickstart-python/hello-world-in-browser.png)

Terminal pencerenizde, Web sunucusundan çıkmak için **CTRL + C** tuşlarına basın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Örneği indirin

Cloud Shell, bir hızlı başlangıç dizini oluşturun ve ardından bu dizin ile değiştirin.

```bash
mkdir quickstart

cd $HOME/quickstart
```

Ardından, örnek uygulama deposunu hızlı başlangıç dizininize kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Çalışırken, aşağıdaki örneğe benzer bilgiler görüntüler:

```bash
Cloning into 'python-docs-hello-world'...
remote: Enumerating objects: 43, done.
remote: Total 43 (delta 0), reused 0 (delta 0), pack-reused 43
Unpacking objects: 100% (43/43), done.
Checking connectivity... done.
```

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Örnek kodu içeren dizine geçin ve `az webapp up` komutunu çalıştırın.

Aşağıdaki örnekte, `<app-name>` ' ı genel olarak benzersiz bir uygulama adıyla değiştirin (*geçerli karakterler şunlardır. `a-z`, `0-9` ve `-`* ).

```bash
cd python-docs-hello-world

az webapp up -n <app-name>
```

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalışırken, aşağıdaki örneğe benzer bilgiler görüntüler:

```json
The behavior of this command has been altered by the following extension: webapp
Creating Resource group 'appsvc_rg_Linux_CentralUS' ...
Resource group creation complete
Creating App service plan 'appsvc_asp_Linux_CentralUS' ...
App service plan creation complete
Creating app '<app-name>' ....
Webapp creation complete
Creating zip with contents of dir /home/username/quickstart/python-docs-hello-world ...
Preparing to deploy contents to app.
All done.
{
  "app_url": "https:/<app-name>.azurewebsites.net",
  "location": "Central US",
  "name": "<app-name>",
  "os": "Linux",
  "resourcegroup": "appsvc_rg_Linux_CentralUS ",
  "serverfarm": "appsvc_asp_Linux_CentralUS",
  "sku": "BASIC",
  "src_path": "/home/username/quickstart/python-docs-hello-world ",
  "version_detected": "-",
  "version_to_create": "python|3.7"
}
```

[!INCLUDE [AZ Webapp Up Note](../../../includes/app-service-web-az-webapp-up-note.md)]

## <a name="browse-to-the-app"></a>Uygulamaya gidin

Web tarayıcınızı kullanarak dağıtılan uygulamaya gidin.

```bash
http://<app-name>.azurewebsites.net
```

Python örnek kodu, yerleşik bir görüntüyle Linux üzerinde App Service çalışmaktadır.

![Azure 'da çalışan örnek uygulama](media/quickstart-python/hello-world-in-browser.png)

**Mühendisi!** Linux üzerinde App Service için ilk Python uygulamanızı dağıttınız.

## <a name="update-locally-and-redeploy-the-code"></a>Yerel olarak güncelleştir ve kodu yeniden Dağıt

Cloud Shell `code application.py` girerek Cloud Shell düzenleyicisini açın.

![Kod application.py](media/quickstart-python/code-applicationpy.png)

 @No__t çağrısındaki metinde küçük bir değişiklik yapın-0:

```python
return "Hello Azure!"
```

Değişikliklerinizi kaydedin ve düzenleyiciden çıkın. Kaydetmek için `^S` komutunu kullanın ve çıkmak için-1 @no__t.

[@No__t-1](/cli/azure/webapp#az-webapp-up) komutunu kullanarak uygulamayı yeniden dağıtın. @No__t-0 için uygulamanızın adını değiştirin ve `<location-name>` ( [`az account list-locations`](/cli/azure/appservice?view=azure-cli-latest.md#az-appservice-list-locations) komutundan gösterilen değerlerden birini kullanarak) için bir konum belirtin.

```bash
az webapp up -n <app-name> -l <location-name>
```

Dağıtım tamamlandıktan sonra, **uygulamaya gözatma** adımında açılan tarayıcı penceresine dönün ve sayfayı yenileyin.

![Azure 'da çalışan güncelleştirilmiş örnek uygulama](media/quickstart-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-app"></a>Yeni Azure uygulamanızı yönetin

Oluşturduğunuz uygulamayı yönetmek için <a href="https://portal.azure.com" target="_blank">Azure Portal</a> gidin.

Sol menüden **uygulama hizmetleri**' ni seçin ve ardından Azure uygulamanızın adını seçin.

![Azure uygulamasına Portal gezintisi](./media/quickstart-python/app-service-list.png)

Uygulamanızın genel bakış sayfasını görürsünüz. Burada, gezinme, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

![Azure portal App Service sayfası](media/quickstart-python/app-service-detail.png)

Sol menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile Python (Docgo) Web uygulaması](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Öğretici: özel kapsayıcıda Python uygulaması çalıştırma](tutorial-custom-docker-image.md)
