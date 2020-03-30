---
title: 'Quickstart: Linux Python uygulaması oluşturun'
description: İlk Python uygulamanızı App Service'teki bir Linux konteynerine dağıtarak Azure Uygulama Hizmeti'ndeki Linux uygulamalarıyla başlayın.
ms.topic: quickstart
ms.date: 10/22/2019
ms.custom: seo-python-october2019, cli-validate
experimental: true
experiment_id: 01a9132f-eaab-4c
ms.openlocfilehash: 9cc314edf35d6a327522ed49fcc0c7798c7dcf63
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80045661"
---
# <a name="quickstart-create-a-python-app-in-azure-app-service-on-linux"></a>Quickstart: Linux'ta Azure Uygulama Hizmeti'nde Bir Python uygulaması oluşturun

Bu hızlı başlangıçta, Bir Python web uygulamasını [Linux'taki Uygulama Hizmeti'ne](app-service-linux-intro.md)dağıtirsiniz, Azure'un yüksek ölçeklenebilir, kendi kendini yamalayan web barındırma hizmeti. Mac, Linux veya Windows bilgisayarlarda yerel [Azure komut satırı arabirimini (CLI)](/cli/azure/install-azure-cli) kullanırsınız. Yapılandırdığınız web uygulaması ücretsiz bir Uygulama Hizmeti katmanı kullanır, bu nedenle bu makale nin seyri boyunca hiçbir ücrete tabi olmazsınız.

Uygulamaları bir IDE üzerinden dağıtmayı tercih ediyorsanız, [Visual Studio Code'tan Python uygulamalarını Uygulama Hizmetine dağıt'a](/azure/python/tutorial-deploy-app-service-on-linux-01)bakın.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- <a href="https://www.python.org/downloads/" target="_blank">Python 3.7</a> (Python 3.6 da desteklenir)
- <a href="https://git-scm.com/downloads" target="_blank">Git</a>
- <a href="https://docs.microsoft.com/cli/azure/install-azure-cli" target="_blank">Azure CLI</a>

## <a name="download-the-sample"></a>Örneği indirme

Terminal penceresinde, örnek uygulamayı yerel bilgisayarınıza klonlamak için aşağıdaki komutu çalıştırın. 

```terminal
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Sonra şu klasöre gir:

```terminal
cd python-docs-hello-world
```

Depo, App Service'e kodun bir Flask uygulaması içerdiğini söyleyen bir *application.py* dosyası içerir. Daha fazla bilgi için [Kapsayıcı başlatma işlemi ve özelleştirmelere](how-to-configure-python.md)bakın.

## <a name="run-the-sample"></a>Örneği çalıştırma

Terminal penceresinde, gerekli bağımlılıkları yüklemek ve yerleşik geliştirme sunucusunu başlatmak için aşağıdaki komutları (işletim sisteminize uygun olarak) kullanın. 

# <a name="bash"></a>[Bash](#tab/bash)

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
export FLASK_APP=application.py
flask run
```

# <a name="powershell"></a>[Powershell](#tab/powershell)

```powershell
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
Set-Item Env:FLASK_APP ".\application.py"
flask run
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
SET FLASK_APP=application.py
flask run
```

---

Bir web tarayıcısı açın ve `http://localhost:5000/`örnek uygulamaya gidin. Uygulama Merhaba Dünya mesajını **görüntüler!**.

![Örnek bir Python uygulamasını yerel olarak çalıştırma](./media/quickstart-python/run-hello-world-sample-python-app-in-browser-localhost.png)

Terminal pencerenizde, web sunucusundan çıkmak için **Ctrl**+**C** tuşuna basın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLI, komut satırından Azure kaynaklarını sağlamak ve yönetmek için yerel bir terminalden kullandığınız birçok kullanışlı komut sağlar. Bir tarayıcıdaki Azure portalı üzerinden yaptığınız görevleri tamamlamak için komutları kullanabilirsiniz. Yönetim işlemlerini otomatikleştirmek için komut dosyalarındaki CLI komutlarını da kullanabilirsiniz.

Azure CLI'de Azure komutlarını çalıştırmak için önce `az login` komutu kullanarak oturum açmanız gerekir. Bu komut, kimlik bilgilerinizi toplamak için bir tarayıcı açar.

```azurecli
az login
```

## <a name="deploy-the-sample"></a>Örneği dağıtma

Komut, [`az webapp up`](/cli/azure/webapp#az-webapp-up) App Service'de web uygulamasını oluşturur ve kodunuzu dağıtır.

Örnek kodu içeren *python-docs-hello-world* klasöründe aşağıdaki `az webapp up` komutu çalıştırın. Genel `<app-name>` olarak benzersiz bir uygulama adı ile değiştirin *(geçerli karakterler `a-z`, , `0-9`ve `-` *). Ayrıca `<location-name>` **centralus,** **eastasia**, **westeurope**, koreasouth , **brazilsouth**, **centralindia**, ve benzeri gibi bir Azure bölgesi ile değiştirin. **brazilsouth** [`az account list-locations`](/cli/azure/appservice?view=azure-cli-latest.md#az-appservice-list-locations) (Komutu çalıştırarak Azure hesabınız için izin verilebilen bölgelerin listesini alabilirsiniz.)


```azurecli
az webapp up --sku F1 -n <app-name> -l <location-name>
```

Bu komutun tamamen çalışması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:

```output
The behavior of this command has been altered by the following extension: webapp
Creating Resource group 'appsvc_rg_Linux_centralus' ...
Resource group creation complete
Creating App service plan 'appsvc_asp_Linux_centralus' ...
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
  "resourcegroup": "appsvc_rg_Linux_centralus ",
  "serverfarm": "appsvc_asp_Linux_centralus",
  "sku": "BASIC",
  "src_path": "/home/username/quickstart/python-docs-hello-world ",
  "version_detected": "-",
  "version_to_create": "python|3.7"
}
```

[!INCLUDE [AZ Webapp Up Note](../../../includes/app-service-web-az-webapp-up-note.md)]

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

URL'de `http://<app-name>.azurewebsites.net`web tarayıcınızda dağıtılan uygulamaya göz atın.

Python örnek kodu, yerleşik bir görüntü kullanarak App Service'de bir Linux kapsayıcısı çalıştırıyor.

![Azure'da örnek bir Python uygulaması çalıştırma](./media/quickstart-python/run-hello-world-sample-python-app-in-browser.png)

**Tebrikler!** Python uygulamanızı Linux'taki Uygulama Hizmeti'ne dağıttınız.

## <a name="redeploy-updates"></a>Güncelleştirmeleri yeniden dağıtma

Favori kod düzenleyicinizde, *application.py* açın `return` ve aşağıdaki kodla eşleşecek şekilde son satırdaki deyimi değiştirin. İfade, `print` bir sonraki bölümde birlikte çalıştığınız günlük çıktısını oluşturmak için burada yer almaktadır. 

```python
print("Handling request to home page.")
return "Hello Azure!"
```

Değişikliklerinizi kaydedin ve editörden çıkın. 

Uygulamayı aşağıdaki `az webapp up` komutu kullanarak yeniden dağıtın, uygulamayı ilk kez dağıtmak için `<app-name>` kullandığınız `<location-name>` komutu kullanarak, daha önce kullandığınız adların yerini ve aynı adlarını değiştirin. 

```azurecli
az webapp up --sku F1 -n <app-name> -l <location-name>
```

Dağıtım tamamlandıktan sonra, değiştirilen iletiyi `http://<app-name>.azurewebsites.net` görüntülemesi gereken sayfayı açık olan tarayıcı penceresine geri geçin ve yenileyin:

![Azure'da güncelleştirilmiş bir örnek Python uygulaması çalıştırma](./media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

> [!TIP]
> Visual Studio Code, Python web uygulamalarını Uygulama Hizmetine dağıtma işlemini basitleştiren Python ve Azure Uygulama Hizmeti için güçlü uzantılar sağlar. Daha fazla bilgi için [Bkz. Görsel Stüdyo Kodu'ndan Python uygulamalarını Uygulama Hizmetine Dağıt.](/azure/python/tutorial-deploy-app-service-on-linux-01)

## <a name="stream-logs"></a>Akış günlükleri

Uygulamanın içinden oluşturulan konsol günlüklerine ve çalıştığı kapsayıcıya erişebilirsiniz. Günlükler, deyimler `print` kullanılarak oluşturulan çıktıları içerir.

İlk olarak, aşağıdaki komutu bir terminalde çalıştırarak, `<app-name>` uygulamanızın adını `<resource-group-name>` ve kullandığınız `az webapp up` komutun çıktısında gösterilen kaynak grubunun adını ("appsvc_rg_Linux_centralus" gibi değiştirerek konteyner günlüğe kaydetmeyi açın:

```azurecli
az webapp log config --name <app-name> --resource-group <resource-group-name> --docker-container-logging filesystem
```

Kapsayıcı günlüğe kaydetme açık olduktan sonra, günlük akışını göstermek için aşağıdaki komutu çalıştırın:

```azurecli
az webapp log tail --name <app-name> --resource-group <resource-group-name>
```

Aşağıdaki metne benzer satırlar içermelidir konsol günlükleri oluşturmak için tarayıcıda uygulamayı yenileyin. Çıkışı hemen görmüyorsanız, 30 saniye içinde yeniden deneyin.

```output
2019-10-23T12:40:03.815574424Z Handling request to home page.
2019-10-23T12:40:03.815602424Z 172.16.0.1 - - [23/Oct/2019:12:40:03 +0000] "GET / HTTP/1.1" 200 12 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.63 Safari/537.36 Edg/78.0.276.19"
```

Ayrıca tarayıcıdan günlük dosyalarını da `https://<app-name>.scm.azurewebsites.net/api/logs/docker`inceleyebilirsiniz.

Günlük akışını herhangi bir zamanda `Ctrl` + `C`durdurmak için .

## <a name="manage-the-azure-app"></a>Azure uygulamasını yönetme

Oluşturduğunuz uygulamayı yönetmek için <a href="https://portal.azure.com" target="_blank">Azure portalına</a> gidin. **App Services'ı**arayın ve seçin.

![Azure portalında Uygulama Hizmetleri'ne gidin](./media/quickstart-python/navigate-to-app-services-in-the-azure-portal.png)

Azure uygulamanızın adını seçin.

![Azure portalındaki Uygulama Hizmetleri'nde Python uygulamanıza gidin](./media/quickstart-python/navigate-to-app-in-app-services-in-the-azure-portal.png)

Uygulamanızın Genel Bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

![Python uygulamanızı Azure portalındaki Genel Bakış sayfasında yönetme](./media/quickstart-python/manage-an-app-in-app-services-in-the-azure-portal.png)

Uygulama Hizmeti menüsü, uygulamanızı yapılandırmak için farklı sayfalar sağlar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. Kaynak grubunun konumunuza bağlı olarak "appsvc_rg_Linux_CentralUS" gibi bir adı vardır. Ücretsiz F1 katmanı dışında bir Uygulama Hizmeti SKU kullanıyorsanız, bu kaynaklar sürekli maliyetlere neden olur.

Gelecekte bu kaynaklara ihtiyaç duymayı beklemiyorsanız, "appsvc_rg_Linux_centralus" gibi `<resource-group-name>` `az webapp up` komutun çıktısında gösterilen kaynak grubuyla değiştirerek aşağıdaki komutu çalıştırarak kaynak grubunu silin. Komutun tamamlanması bir dakika sürebilir.

```azurecli
az group delete -n <resource-group-name>
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile Python (Django) web uygulaması](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Öğretici: Özel kapsayıcıda Python uygulamasını çalıştırın](tutorial-custom-docker-image.md)
