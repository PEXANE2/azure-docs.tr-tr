---
title: 'Hızlı başlangıç: Linux Python uygulaması oluşturma'
description: İlk Python uygulamanızı App Service bir Linux kapsayıcısına dağıtarak Azure App Service Linux uygulamalarını kullanmaya başlayın.
ms.topic: quickstart
ms.date: 04/03/2020
ms.custom: seo-python-october2019, cli-validate
ms.openlocfilehash: 5b055c3ed93d5f093295b52c7a28a73e242bfe75
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690873"
---
# <a name="quickstart-create-a-python-app-in-azure-app-service-on-linux"></a>Hızlı başlangıç: Linux üzerinde Azure App Service bir Python uygulaması oluşturma

Bu hızlı başlangıçta, Azure 'un yüksek düzeyde ölçeklenebilir ve kendini yayama Web barındırma hizmeti [olan Linux üzerinde App Service](app-service-linux-intro.md)Için bir Python web uygulaması dağıtırsınız. Yerel [Azure komut satırı arabirimi 'ni (CLI)](/cli/azure/install-azure-cli) bir Mac, Linux veya Windows bilgisayarında kullanırsınız. Yapılandırdığınız Web uygulaması ücretsiz bir App Service katmanını kullanır, bu nedenle bu makalenin kursunda hiçbir ücret ödeirsiniz.

Uygulamaları bir IDE aracılığıyla dağıtmayı tercih ediyorsanız, bkz. [Visual Studio Code App Service Için Python uygulamaları dağıtma](/azure/python/tutorial-deploy-app-service-on-linux-01).

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- <a href="https://www.python.org/downloads/" target="_blank">Python 3,7</a> (Python 3,6 de desteklenir)
- <a href="https://git-scm.com/downloads" target="_blank">Git</a>
- <a href="https://docs.microsoft.com/cli/azure/install-azure-cli" target="_blank">Azure CLI</a> 2.0.80 veya üzeri. Sürümünüzü kontrol etmek için `az --version` komutunu çalıştırın.

## <a name="download-the-sample"></a>Örneği indirme

Bir Terminal penceresinde, örnek uygulamayı yerel bilgisayarınıza kopyalamak için aşağıdaki komutu çalıştırın. 

```terminal
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Daha sonra bu klasöre gidin:

```terminal
cd python-docs-hello-world
```

Depo, kodun bir Flask uygulaması içerdiğini App Service söyleyen bir *Application.py* dosyası içerir. Daha fazla bilgi için bkz. [kapsayıcı başlangıç işlemi ve özelleştirmeleri](how-to-configure-python.md).

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

# <a name="powershell"></a>[PowerShell](#tab/powershell)

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

Bir Web tarayıcısı açın ve konumundaki `http://localhost:5000/`örnek uygulamaya gidin. Uygulama **Merhaba Dünya!** iletisini görüntüler.

![Örnek bir Python uygulamasını yerel olarak çalıştırma](./media/quickstart-python/run-hello-world-sample-python-app-in-browser-localhost.png)

Terminal pencerenizde, Web sunucusundan çıkmak için **CTRL**+**C** tuşuna basın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLı, Azure kaynaklarını komut satırından sağlamak ve yönetmek için yerel terminalden kullandığınız birçok kullanışlı komut sağlar. Bir tarayıcıda Azure portal istediğiniz görevleri gerçekleştirmek için komutları kullanabilirsiniz. Yönetim işlemlerini otomatikleştirmek için betiklerdeki CLı komutlarını da kullanabilirsiniz.

Azure CLı 'de Azure komutlarını çalıştırmak için öncelikle `az login` komutunu kullanarak oturum açmalısınız. Bu komut, kimlik bilgilerinizi toplamak için bir tarayıcı açar.

```azurecli
az login
```

## <a name="deploy-the-sample"></a>Örneği dağıtma

[`az webapp up`](/cli/azure/webapp#az-webapp-up) Komut, App Service Web uygulamasını oluşturur ve kodunuzu dağıtır.

Örnek kodu içeren *Python-docs-Hello-World* klasöründe aşağıdaki `az webapp up` komutu çalıştırın. Genel `<app-name>` olarak benzersiz bir uygulama adıyla değiştirin (*geçerli karakterler `a-z`, `0-9`ve `-` *).


```azurecli
az webapp up --sku F1 -n <app-name>
```
> [!CAUTION]
> **Azure-CLI sürüm 2.5.0** kullanıyorsanız, `-l <location-name>` parametresi dahil edilmediğinden belirli senaryoların başarısız `az webapp up` olacağı bir gerileme vardır. Bu sorun [burada izleniyor](https://github.com/Azure/azure-cli/issues/13257).  
> 
>`az --version` Komutuyla HANGI Azure CLI sürümünün kullandığınızı kontrol edebilirsiniz.
>

`--sku F1` Bağımsız değişkeni, ücretsiz fiyatlandırma katmanında Web uygulaması oluşturur. Bunun yerine bir Premium katmanı kullanmak için bu bağımsız değişkeni atlayabilirsiniz, bu da saatlik maliyet olarak kullanılabilir.

İsteğe bağlı **olarak,** **westeurope**, `-l <location-name>` **koreagüney**, **brazilsouth**, **merkezileştirme**vb **. gibi bir**Azure bölgesi `<location_name>` olan bağımsız değişkeni ekleyebilirsiniz. [`az account list-locations`](/cli/azure/appservice?view=azure-cli-latest.md#az-appservice-list-locations) Komutunu çalıştırarak, Azure hesabınız için izin verilen bölgelerin bir listesini alabilirsiniz.

`az webapp up` Komutun tamamen çalışması birkaç dakika sürebilir. Çalışırken, aşağıdaki örneğe benzer bilgiler görüntüler; burada `<app-name>` , daha önce verdiğiniz ad olacaktır:

<pre>
Creating Resource group 'appsvc_rg_Linux_centralus' ...
Resource group creation complete
Creating App service plan 'appsvc_asp_Linux_centralus' ...
App service plan creation complete
Creating app '&lt;app-name&gt;' ....
Configuring default logging for the app, if not already enabled
Creating zip with contents of dir D:\Examples\python-docs-hello-world ...
Getting scm site credentials for zip deployment
Starting zip deployment. This operation can take a while to complete ...
Deployment endpoint responded with status code 202
You can launch the app at http://&lt;app-name&gt;.azurewebsites.net
{
  "URL": "http://&lt;app-name&gt;.net",
  "appserviceplan": "appsvc_asp_Linux_centralus",
  "location": "eastus",
  "name": "&lt;app-name&gt;",
  "os": "Linux",
  "resourcegroup": "appsvc_rg_Linux_centralus",
  "runtime_version": "python|3.7",
  "runtime_version_detected": "-",
  "sku": "FREE",
  "src_path": "D:\\Examples\\python-docs-hello-world"
}
</pre>

[!INCLUDE [AZ Webapp Up Note](../../../includes/app-service-web-az-webapp-up-note.md)]

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

URL `http://<app-name>.azurewebsites.net`'de Web tarayıcınızda dağıtılan uygulamaya gidin.

Python örnek kodu, yerleşik bir görüntü kullanarak App Service bir Linux kapsayıcısı çalıştırıyor.

![Azure 'da örnek bir Python uygulaması çalıştırma](./media/quickstart-python/run-hello-world-sample-python-app-in-browser.png)

**Tebrikler!** Python uygulamanızı Linux üzerinde App Service için dağıttık.

## <a name="redeploy-updates"></a>Güncelleştirmeleri yeniden Dağıt

En sevdiğiniz kod düzenleyicisinde *Application.py* ' ı açın ve `hello` işlevi aşağıdaki gibi güncelleştirin. Bu değişiklik, bir `print` sonraki bölümde birlikte çalıştığınız günlük çıkışını oluşturmak için bir ifade ekler. 

```python
def hello():
    print("Handling request to home page.")
    return "Hello Azure!"
```

Değişikliklerinizi kaydedin ve düzenleyiciden çıkın. 

`az webapp up` Komutu tekrar kullanarak uygulamayı yeniden dağıtın:

```azurecli
az webapp up
```

Bu komut, uygulama adı, kaynak grubu ve App Service planı dahil olmak üzere *. Azure/config* dosyasında önbelleğe alınan değerleri kullanır.

Dağıtım tamamlandıktan sonra tarayıcı penceresine geri dönün `http://<app-name>.azurewebsites.net` ve sayfayı yenileyerek değiştirilen iletiyi görüntülemesi gerekir:

![Azure 'da güncelleştirilmiş örnek bir Python uygulaması çalıştırma](./media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

> [!TIP]
> Visual Studio Code, Python web uygulamalarını App Service dağıtma sürecini kolaylaştıran Python ve Azure App Service için güçlü uzantılar sağlar. Daha fazla bilgi için bkz. [Visual Studio Code App Service Için Python uygulamaları dağıtma](/azure/python/tutorial-deploy-app-service-on-linux-01).

## <a name="stream-logs"></a>Akış günlükleri

Uygulamanın içinden oluşturulan konsol günlüklerine ve çalıştığı kapsayıcıya erişebilirsiniz. Günlükler, using deyimleri kullanılarak `print` oluşturulan herhangi bir çıktıyı içerir.

Günlükleri akışa almak için aşağıdaki komutu çalıştırın:

```azurecli
az webapp log tail
```

Aşağıdaki metne benzer satırları içermesi gereken konsol günlükleri oluşturmak için tarayıcıda uygulamayı yenileyin. Çıktıyı hemen görmüyorsanız, 30 saniye içinde yeniden deneyin.

<pre>
2020-04-03T22:54:04.236405938Z Handling request to home page.
2020-04-03T22:54:04.236497641Z 172.16.0.1 - - [03/Apr/2020:22:54:04 +0000] "GET / HTTP/1.1" 200 12 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.83 Safari/537.36 Edg/81.0.416.41"
</pre>

Ayrıca, konumundaki `https://<app-name>.scm.azurewebsites.net/api/logs/docker`tarayıcıdan günlük dosyalarını inceleyebilirsiniz.

Günlük akışını istediğiniz zaman durdurmak için, yazın `Ctrl` + `C`.

## <a name="manage-the-azure-app"></a>Azure uygulamasını yönetme

Oluşturduğunuz uygulamayı yönetmek için <a href="https://portal.azure.com" target="_blank">Azure Portal</a> gidin. **Uygulama hizmetleri**' ni arayıp seçin.

![Azure portal uygulama hizmetleri 'ne gidin](./media/quickstart-python/navigate-to-app-services-in-the-azure-portal.png)

Azure uygulamanızın adını seçin.

![Azure portal App Services 'ta Python uygulamanıza gidin](./media/quickstart-python/navigate-to-app-in-app-services-in-the-azure-portal.png)

Uygulamanızın genel bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

![Python uygulamanızı Azure portal genel bakış sayfasında yönetin](./media/quickstart-python/manage-an-app-in-app-services-in-the-azure-portal.png)

App Service menüsü, uygulamanızı yapılandırmak için farklı sayfalar sağlar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. Kaynak grubu, konumunuza bağlı olarak "appsvc_rg_Linux_CentralUS" gibi bir ada sahiptir. Ücretsiz F1 katmanından farklı bir App Service SKU 'SU kullanıyorsanız, bu kaynaklar devam eden maliyetlere (bkz. [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/linux/)) sahiptir.

Gelecekte bu kaynaklara ihtiyaç duymazsanız, aşağıdaki komutu çalıştırarak kaynak grubunu silin. Örneğin, "appsvc_rg_Linux_centralus" gibi, `<resource-group-name>` `az webapp up` komutun çıktısında gösterilen kaynak grubuyla değiştirin. Komutun tamamlanması birkaç dakika sürebilir.

```azurecli
az group delete -n <resource-group-name>
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile Python (Docgo) Web uygulaması](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Python web uygulamasına kullanıcı oturumu ekleme](../../active-directory/develop/quickstart-v2-python-webapp.md)

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Öğretici: özel kapsayıcıda Python uygulaması çalıştırma](tutorial-custom-docker-image.md)
