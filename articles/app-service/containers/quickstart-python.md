---
title: 'Hızlı başlangıç: Linux Python uygulaması oluşturma'
description: İlk Python uygulamanızı App Service bir Linux kapsayıcısına dağıtarak Azure App Service Linux uygulamalarını kullanmaya başlayın.
ms.topic: quickstart
ms.date: 06/30/2020
ms.custom: seo-python-october2019, cli-validate, devx-track-python
ms.openlocfilehash: 804ed6084454a70fa6fc4edff9a0579e43a3a872
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87853492"
---
# <a name="quickstart-create-a-python-app-in-azure-app-service-on-linux"></a>Hızlı başlangıç: Linux üzerinde Azure App Service bir Python uygulaması oluşturma

Bu hızlı başlangıçta, Azure 'un yüksek düzeyde ölçeklenebilir ve kendini yayama Web barındırma hizmeti [olan Linux üzerinde App Service](app-service-linux-intro.md)Için bir Python web uygulaması dağıtırsınız. Yerel [Azure komut satırı arabirimi 'ni (CLI)](/cli/azure/install-azure-cli) bir Mac, Linux veya Windows bilgisayarında kullanırsınız. Yapılandırdığınız Web uygulaması ücretsiz bir App Service katmanını kullanır, bu nedenle bu makalenin kursunda hiçbir ücret ödeirsiniz.

Uygulamaları bir IDE aracılığıyla dağıtmayı tercih ediyorsanız, bkz. [Visual Studio Code App Service Için Python uygulamaları dağıtma](/azure/developer/python/tutorial-deploy-app-service-on-linux-01).

## <a name="set-up-your-initial-environment"></a>İlk ortamınızı ayarlama

Başlamadan önce aşağıdakilere sahip olmanız gerekir:

1. Etkin aboneliği olan bir Azure hesabına sahip olmanız gerekir. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
1. <a href="https://www.python.org/downloads/" target="_blank">Python 3,6 veya üstünü</a>yükler.
1. Azure <a href="/cli/azure/install-azure-cli" target="_blank">CLI</a> 2.0.80 veya üstünü yükleyerek Azure kaynaklarını temin etmek ve yapılandırmak için herhangi bir kabukta komut çalıştırırsınız.

Bir Terminal penceresi açın ve Python sürümünüzün 3,6 veya üzeri olduğunu denetleyin:

# <a name="bash"></a>[Bash](#tab/bash)

```bash
python3 --version
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```cmd
py -3 --version
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -3 --version
```

---

Azure CLı sürümünüzün 2.0.80 veya üzeri olduğunu kontrol edin:

```azurecli
az --version
```

Daha sonra CLı aracılığıyla Azure 'da oturum açın:

```azurecli
az login
```

Bu komut, kimlik bilgilerinizi toplamak için bir tarayıcı açar. Komut tamamlandığında, abonelikleriniz hakkında bilgi içeren JSON çıkışını gösterir.

Oturum açtıktan sonra, aboneliğinizdeki kaynaklarla çalışmak için Azure CLı ile Azure komutlarını çalıştırabilirsiniz.

## <a name="clone-the-sample"></a>Örneği

Aşağıdaki komutla örnek depoyu kopyalayın. (Zaten git yoksa[Git 'ı yükleyebilirsiniz](https://git-scm.com/downloads) .)

```terminal
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Daha sonra bu klasöre gidin:

```terminal
cd python-docs-hello-world
```

Örnek kod, kodun bir Flask uygulaması içerdiğini App Service söyleyen bir *Application.py* dosyası içerir. Daha fazla bilgi için bkz. [kapsayıcı başlangıç işlemi ve özelleştirmeleri](how-to-configure-python.md).

## <a name="run-the-sample"></a>Örneği çalıştırma

# <a name="bash"></a>[Bash](#tab/bash)

İlk olarak bir sanal ortam oluşturun ve bağımlılıkları yüklemeniz gerekir:

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Ardından, `FLASK_APP` ortam değişkenini uygulamanın giriş modülüne ayarlayın ve Flask geliştirme sunucusunu çalıştırın:

```
export FLASK_APP=application.py
flask run
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

İlk olarak bir sanal ortam oluşturun ve bağımlılıkları yüklemeniz gerekir:

```powershell
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
```

Ardından, `FLASK_APP` ortam değişkenini uygulamanın giriş modülüne ayarlayın ve Flask geliştirme sunucusunu çalıştırın:

```powershell
Set-Item Env:FLASK_APP ".\application.py"
flask run
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

İlk olarak bir sanal ortam oluşturun ve bağımlılıkları yüklemeniz gerekir:

```cmd
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
```

Ardından, `FLASK_APP` ortam değişkenini uygulamanın giriş modülüne ayarlayın ve Flask geliştirme sunucusunu çalıştırın:

```cmd
SET FLASK_APP=application.py
flask run
```

---

Bir Web tarayıcısı açın ve konumundaki örnek uygulamaya gidin `http://localhost:5000/` . Uygulama **Merhaba Dünya!** iletisini görüntüler.

![Örnek bir Python uygulamasını yerel olarak çalıştırma](./media/quickstart-python/run-hello-world-sample-python-app-in-browser-localhost.png)

**Ctrl** + Flask geliştirme sunucusundan çıkmak için Terminal pencerenizde CTRL**C** tuşuna basın.

## <a name="deploy-the-sample"></a>Örneği dağıtma

Şu komutu kullanarak kodu yerel klasörünüzde (*Python-docs-Hello-World*) dağıtın `az webapp up` :

```azurecli
az webapp up --sku F1 -n <app-name>
```

- `az`Komut tanınmazsa, [Ilk ortamınızı ayarlama](#set-up-your-initial-environment)bölümünde AÇıKLANDıĞı gibi Azure CLI 'nin yüklü olduğundan emin olun.
- `<app_name>`Tüm Azure genelinde benzersiz olan bir adla değiştirin (*geçerli karakterler `a-z` , `0-9` ve `-` *). İyi bir model, şirketinizin adının ve uygulama tanımlayıcısının bir birleşimini kullanmaktır.
- `--sku F1`Bağımsız değişkeni, ücretsiz fiyatlandırma katmanında Web uygulaması oluşturur. Saatlik maliyet içeren daha hızlı bir Premium katmanı kullanmak için bu bağımsız değişkeni atlayın.
- İsteğe bağlı olarak, `-l <location-name>` `<location_name>` **westeurope**, **koreagüney**, **brazilsouth**, **merkezileştirme**vb. **eastasia** **gibi**bir Azure bölgesi olan bağımsız değişkeni ekleyebilirsiniz. Komutunu çalıştırarak, Azure hesabınız için izin verilen bölgelerin bir listesini alabilirsiniz [`az account list-locations`](/cli/azure/appservice?view=azure-cli-latest.md#az-appservice-list-locations) .

Komutun tamamlanması birkaç dakika sürebilir. Çalıştırılırken, kaynak grubu oluşturma, App Service planı ve barındırma uygulaması, günlüğü yapılandırma ve ardından ZIP dağıtımı gerçekleştirme hakkında iletiler sağlar. Daha sonra, "uygulamayı &lt; &gt; Azure 'da uygulamanın URL 'si olan http://app-name. azurewebsites.net konumunda başlatabilirsiniz.

![Az WebApp up komutunun örnek çıktısı](./media/quickstart-python/az-webapp-up-output.png)

[!INCLUDE [AZ Webapp Up Note](../../../includes/app-service-web-az-webapp-up-note.md)]

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

URL 'de Web tarayıcınızda dağıtılan uygulamaya gidin `http://<app-name>.azurewebsites.net` .

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

`az webapp up`Komutu tekrar kullanarak uygulamayı yeniden dağıtın:

```azurecli
az webapp up
```

Bu komut, uygulama adı, kaynak grubu ve App Service planı dahil olmak üzere *. Azure/config* dosyasında yerel olarak önbelleğe alınan değerleri kullanır.

Dağıtım tamamlandıktan sonra tarayıcı penceresine geri dönün `http://<app-name>.azurewebsites.net` . Değiştirilen iletiyi görüntülemesi gereken sayfayı yenileyin:

![Azure 'da güncelleştirilmiş örnek bir Python uygulaması çalıştırma](./media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

> [!TIP]
> Visual Studio Code, Python web uygulamalarını App Service dağıtma sürecini kolaylaştıran Python ve Azure App Service için güçlü uzantılar sağlar. Daha fazla bilgi için bkz. [Visual Studio Code App Service Için Python uygulamaları dağıtma](/azure/python/tutorial-deploy-app-service-on-linux-01).

## <a name="stream-logs"></a>Akış günlükleri

Uygulamanın içinden oluşturulan konsol günlüklerine ve çalıştığı kapsayıcıya erişebilirsiniz. Günlükler, using deyimleri kullanılarak oluşturulan herhangi bir çıktıyı içerir `print` .

Günlükleri akışa almak için aşağıdaki komutu çalıştırın:

```azurecli
az webapp log tail
```

Uygulamaya HTTP isteklerini açıklayan iletileri içeren konsol günlükleri oluşturmak için uygulamayı tarayıcıda yenileyin. Hiçbir çıkış hemen yoksa, 30 saniye içinde yeniden deneyin.

Ayrıca, konumundaki tarayıcıdan günlük dosyalarını inceleyebilirsiniz `https://<app-name>.scm.azurewebsites.net/api/logs/docker` .

Günlük akışını istediğiniz zaman durdurmak için **CTRL** + **C**yazın.

## <a name="manage-the-azure-app"></a>Azure uygulamasını yönetme

Oluşturduğunuz uygulamayı yönetmek için <a href="https://portal.azure.com" target="_blank">Azure Portal</a> gidin. **Uygulama hizmetleri**' ni arayıp seçin.

![Azure portal uygulama hizmetleri 'ne gidin](./media/quickstart-python/navigate-to-app-services-in-the-azure-portal.png)

Azure uygulamanızın adını seçin.

![Azure portal App Services 'ta Python uygulamanıza gidin](./media/quickstart-python/navigate-to-app-in-app-services-in-the-azure-portal.png)

Uygulamanın seçilmesi, **genel bakış** sayfasını açar, buradan göz at, durdur, Başlat, yeniden Başlat ve Sil gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

![Python uygulamanızı Azure portal genel bakış sayfasında yönetin](./media/quickstart-python/manage-an-app-in-app-services-in-the-azure-portal.png)

App Service menüsü, uygulamanızı yapılandırmak için farklı sayfalar sağlar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. Kaynak grubu, konumunuza bağlı olarak "appsvc_rg_Linux_CentralUS" gibi bir ada sahiptir. Ücretsiz F1 katmanından farklı bir App Service SKU 'SU kullanıyorsanız, bu kaynaklar devam eden maliyetlere (bkz. [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/linux/)) sahiptir.

Gelecekte bu kaynakların gerekli olmasını istemiyorsanız, aşağıdaki komutu çalıştırarak kaynak grubunu silin:

```azurecli
az group delete
```

Komut *. Azure/config* dosyasında önbelleğe alınmış kaynak grubu adını kullanır.

Komutun tamamlanması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile Python (Docgo) Web uygulaması](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Python web uygulamasına kullanıcı oturumu ekleme](../../active-directory/develop/quickstart-v2-python-webapp.md)

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Öğretici: özel kapsayıcıda Python uygulaması çalıştırma](tutorial-custom-docker-image.md)
