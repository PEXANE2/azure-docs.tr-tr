---
title: Azure Işlevlerinde HTTP istekleri için sunucusuz Python işlevi oluşturma
description: Azure Işlevleri 'ni kullanarak sunucusuz Python kodunu oluşturun ve buluta dağıtın.
ms.date: 02/11/2020
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: 16d4d5627ea297d825092009511915f5b6e734b6
ms.sourcegitcommit: 0eb0673e7dd9ca21525001a1cab6ad1c54f2e929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77212427"
---
# <a name="quickstart-create-a-python-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlangıç: Azure 'da HTTP isteklerine yanıt veren bir Python işlevi oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir Python işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.

Bu makalenin [Visual Studio Code tabanlı bir sürümü](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-python) de vardır.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure Functions Core Tools](./functions-run-local.md#v2) sürüm 2.7.1846 veya sonraki bir sürümü.
- [Azure CLI](/cli/azure/install-azure-cli) sürüm 2.0.76 veya üzeri. 
- [Python 3.7.4-64 bit](https://www.python.org/downloads/release/python-374/). (Python 3.7.4, Azure Işlevleri ile doğrulanır; Python 3,8 ve sonraki sürümler henüz desteklenmiyor.)

### <a name="prerequisite-check"></a>Önkoşul denetimi

1. Bir Terminal veya komut penceresinde, Azure Functions Core Tools sürüm 2.7.1846 veya üzeri olduğunu denetlemek için `func --version` çalıştırın.
1. Azure CLı sürümünün 2.0.76 veya üzeri olduğunu denetlemek için `az --version` çalıştırın.
1. Azure 'da oturum açmak ve etkin bir aboneliği doğrulamak için `az login` çalıştırın.
1. Python sürüm raporlarınızı 3.7. x olarak denetlemek için `python --version` (Linux/MacOS) veya `py --version` (Windows) çalıştırın.

## <a name="create-and-activate-a-virtual-environment"></a>Oluşturma ve bir sanal ortam etkinleştirin

Uygun bir klasörde, `.venv`adlı bir sanal ortam oluşturmak ve etkinleştirmek için aşağıdaki komutları çalıştırın. Azure Işlevleri tarafından desteklenen Python 3,7 ' i kullandığınızdan emin olun.


# <a name="bashtabbash"></a>[Bash](#tab/bash)

```bash
python -m venv .venv
```

```bash
source .venv/bin/activate
```

Python, venv paketini Linux dağıtımına yüklememediyse aşağıdaki komutu çalıştırın:

```bash
sudo apt-get install python3-venv
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

```powershell
py -m venv .venv
```

```powershell
.venv\scripts\activate
```

# <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)

```cmd
py -m venv .venv
```

```cmd
.venv\scripts\activate
```

---

Bu etkinleştirilmiş sanal ortamda sonraki tüm komutları çalıştırırsınız. (Sanal ortamdan çıkmak için `deactivate`çalıştırın.)

## <a name="create-a-local-function-project"></a>Yerel işlev projesi oluşturma

Azure Işlevlerinde bir işlev projesi, her birinin belirli bir tetikleyiciye yanıt verdiği bir veya daha fazla bağımsız işlev için bir kapsayıcıdır. Projedeki tüm işlevler aynı yerel ve barındırma yapılandırmalarına sahiptir. Bu bölümde, tek bir işlev içeren bir işlev projesi oluşturursunuz.

1. Sanal ortamda, belirtilen çalışma zamanına sahip *Localfunctionproj* adlı bir klasörde bir işlevler projesi oluşturmak için `func init` komutunu çalıştırın:

    ```
    func init LocalFunctionProj --python
    ```
    
    Bu klasör, [yerel. Settings. JSON](functions-run-local.md#local-settings-file) ve [Host. JSON](functions-host-json.md)adlı yapılandırma dosyaları dahil olmak üzere, proje için çeşitli dosyaları içerir. *Local. Settings. JSON* , Azure 'dan indirilen gizli dizileri içerebildiğinden, dosya, *. gitignore* dosyasında varsayılan olarak kaynak denetiminden çıkarılır.

    > [!TIP]
    > Bir işlev projesi belirli bir çalışma zamanına bağlı olduğundan, projedeki tüm işlevlerin aynı dille yazılması gerekir.

1. Proje klasörüne gidin:

    ```
    cd LocalFunctionProj
    ```
    
1. Aşağıdaki komutu kullanarak, `--name` bağımsız değişkeninin işlevinizin benzersiz adını (Bu durumda HttpExample) belirttiği ve `--template` bağımsız değişkeninin işlevin tetikleyicisini (HTTP tetikleyicisi) belirttiği, projenize bir işlev ekleyin. Bu `func new` komut, projenin seçilen diline ve *function. JSON*adlı yapılandırma dosyasına uygun bir kod dosyası içeren işlev adıyla eşleşen bir alt klasör oluşturur.

    ```
    func new --name HttpExample --template "HTTP trigger"
    ```

### <a name="optional-examine-the-file-contents"></a>Seçim Dosya içeriğini inceleyin

İsterseniz, [işlevi yerel olarak çalıştırmayı](#run-the-function-locally) atlayıp dosya içeriğini daha sonra incelemenizi sağlayabilirsiniz.

#### <a name="__init__py"></a>\_\_init\_\_. Kopyala

*\_\_init\_\_. Kopyala* , *function. JSON*içindeki yapılandırmaya göre tetiklenen bir `main()` Python işlevi içeriyor.

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

HTTP tetikleyicisi için işlev, `req` değişkende istek verilerini alır, *function. JSON*içinde tanımlanmıştır. `req`, [Azure. Functions. HttpRequest sınıfının](/python/api/azure-functions/azure.functions.httprequest)bir örneğidir. *Function. JSON*içinde `$return` olarak tanımlanan Return nesnesi, [Azure. Functions. HttpResponse sınıfının](/python/api/azure-functions/azure.functions.httpresponse)bir örneğidir. Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](functions-bindings-http-webhook.md).

#### <a name="functionjson"></a>function. JSON

*function. JSON* , tetikleyici türü de dahil olmak üzere işlevin giriş ve çıkış `bindings` tanımlayan bir yapılandırma dosyasıdır. İsterseniz farklı bir Python dosyası çağırmak için `scriptFile` değiştirebilirsiniz.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

Her bağlama bir yön, tür ve benzersiz bir ad gerektirir. HTTP tetikleyicisinin tür [`httpTrigger`](functions-bindings-http-webhook-trigger.md) ve [`http`](functions-bindings-http-webhook-output.md)türünde çıkış bağlaması olan bir giriş bağlaması vardır.


## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

*Localfunctionproj* klasöründe yerel Azure işlevleri çalışma zamanı konağını başlatarak işlevi başlatın:

```
func start
```

Aşağıdaki çıktı görünmelidir. (HttpExample aşağıda gösterildiği gibi görünmezse, büyük olasılıkla, *http://örnek* klasörü içinden ana bilgisayarı başlatmış olursunuz. Bu durumda, ana bilgisayarı durdurmak için **Ctrl**+**C** kullanın, üst *localfunctionproj* klasörüne gidin ve `func start` yeniden çalıştırın.)

```output
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

        HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
```

`HttpExample` işlevinizin URL 'sini bu çıktıdan tarayıcıya kopyalayın ve sorgu dizesi `?name=<your-name>`ekleyerek `http://localhost:7071/api/HttpExample?name=Functions`gibi tam URL 'yi yapın. Tarayıcı `Hello Functions`gibi bir ileti görüntülemelidir:

![İşlevin sonuçları tarayıcıda yerel olarak çalıştırıldı](./media/functions-create-first-function-python/function-test-local-browser.png)

`func start` çalıştırdığınız terminalde, istek yaptığınız sürece günlük çıktısını gösterir.

Hazırsanız, işlevler ana bilgisayarını durdurmak için **Ctrl**+**C** ' yi kullanabilirsiniz.

## <a name="create-supporting-azure-resources-for-your-function"></a>İşleviniz için destekleyici Azure kaynakları oluşturma

İşlev kodunuzu Azure 'a dağıtabilmeniz için önce üç kaynak oluşturmanız gerekir:

- İlgili kaynaklar için mantıksal kapsayıcı olan bir kaynak grubu.
- Projelerinizle ilgili durumu ve diğer bilgileri tutan bir Azure depolama hesabı.
- İşlev kodunuzun yürütülmesi için ortam sağlayan bir Azure işlevleri uygulaması. Bir işlev uygulaması yerel işlev projenize eşlenir ve kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanızı sağlar.

Bu öğeleri oluşturmak için Azure CLı komutlarını kullanırsınız. Her komut, tamamlandıktan sonra JSON çıktısı sağlar.

1. Şimdiye kadar yapmadıysanız, [az Login](/cli/azure/reference-index#az-login) komutuyla Azure 'da oturum açın:

    ```azurecli
    az login
    ```
    
1. [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, `westeurope` bölgesinde `AzureFunctionsQuickstart-rg` adlı bir kaynak grubu oluşturur. (`az account list-locations` komutundan kullanılabilir bir bölge kullanarak genellikle kaynak grubunuzu ve kaynaklarınızı size yakın bir bölgede oluşturursunuz.)

    ```azurecli
    az group create --name AzureFunctionsQuickstart-rg --location westeurope
    ```
    
    > [!NOTE]
    > Linux ve Windows uygulamalarını aynı kaynak grubunda barındıralamazsınız. Bir Windows işlev uygulaması veya Web uygulamasıyla `AzureFunctionsQuickstart-rg` adlı mevcut bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.
    
1. [Az Storage Account Create](/cli/azure/storage/account#az-storage-account-create) komutunu kullanarak kaynak grubunuzda ve bölgenizde genel amaçlı bir depolama hesabı oluşturun. Aşağıdaki örnekte, `<storage_name>`, sizin için uygun olan bir genel benzersiz adla değiştirin. Adlar yalnızca üç ile 24 karakter arasında ve küçük harflerden oluşmalıdır. `Standard_LRS` tipik bir genel amaçlı hesabı belirtir.

    ```azurecli
    az storage account create --name <storage_name> --location westeurope --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
    ```
    
    Depolama hesabı, bu hızlı başlangıç için yalnızca birkaç ABD Doları olarak yer doğurur.
    
1. [Az functionapp Create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak işlevler uygulamasını oluşturun. Aşağıdaki örnekte, `<storage_name>`, önceki adımda kullandığınız hesabın adıyla değiştirin ve `<app_name>`, sizin için uygun olan bir genel benzersiz adla değiştirin. `<app_name>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır.

    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --os-type Linux --consumption-plan-location westeurope --runtime python --name <app_name> --storage-account <storage_name>
    ```
    
    Bu komut, burada yaptığınız kullanım miktarı için ücretsiz olan [Azure Işlevleri tüketim planı](functions-scale.md#consumption-plan)altında belirtilen dil çalışma zamanını çalıştıran bir işlev uygulaması oluşturur. Bu komut Ayrıca, aynı kaynak grubunda, işlev uygulamanızı izleyebilmeniz ve günlükleri görüntüleyebileceğiniz ilişkili bir Azure Application Insights örneğini sağlar. Daha fazla bilgi için bkz. [Azure Işlevlerini izleme](functions-monitoring.md). Örnek, siz etkinleştirene kadar hiçbir maliyet vermez.
    
## <a name="deploy-the-function-project-to-azure"></a>İşlev projesini Azure 'a dağıtma

Gerekli kaynaklarla, artık yerel işlevler projenizi, [Func Azure functionapp Publish](functions-run-local.md#project-file-deployment) komutunu kullanarak Azure 'daki işlev uygulamasına dağıtmaya hazırsınız demektir. Aşağıdaki örnekte `<app_name>` değerini uygulamanızın adıyla değiştirin.

```
func azure functionapp publish <app_name>
```

"Adı olan uygulama bulunamıyor..." hatasını görürseniz, birkaç saniye bekleyip yeniden deneyin. Bu işlem, Azure, önceki `az functionapp create` komutundan sonra uygulamayı tam olarak başlatılamayabilir.

Yayımla komutu aşağıdaki çıktıya benzer sonuçları gösterir (basitlik için kesildi):

```output
Getting site publishing info...
Creating archive for current directory...
Performing remote build for functions project.

...

Deployment successful.
Remote build succeeded!
Syncing triggers...
Functions in msdocs-azurefunctions-qs:
    HttpExample - [httpTrigger]
        Invoke url: https://msdocs-azurefunctions-qs.azurewebsites.net/api/httpexample?code=KYHrydo4GFe9y0000000qRgRJ8NdLFKpkakGJQfC3izYVidzzDN4gQ==
```

## <a name="invoke-the-function-on-azure"></a>Azure 'da işlevi çağırma

İşleviniz bir HTTP tetikleyicisi kullandığından, tarayıcıda URL 'sine veya kıvrıcı gibi bir araçla HTTP isteği yaparak bu uygulamayı çağırabilirsiniz. Her iki örnekte de `code` URL parametresi, işlev uç noktanızla çağırma yetkisi veren benzersiz işleviniz anahtarıdır.

# <a name="browsertabbrowser"></a>[Tarayıcı](#tab/browser)

Yayımla komutunun çıktısında gösterilen tüm **çağırma URL** 'sini bir tarayıcı adres çubuğuna kopyalayın, `&name=Azure`sorgu parametresini ekleyin. , İşlevi yerel olarak çalıştırdığınızda tarayıcı benzer bir çıktı görüntülemelidir.

![Azure üzerinde çalışan işlevin çıktısı bir tarayıcıda](./media/functions-create-first-function-python/function-test-cloud-browser.png)


# <a name="curltabcurl"></a>[kıvr](#tab/curl)

`&name=Azure`parametresini ekleyerek **Invoke URL 'si**ile [kıvrımlı](https://curl.haxx.se/) çalıştırın. Komutun çıktısı, "Merhaba Azure" metni olmalıdır.

![İşlevin çıktısı, Azure 'da kıvrımlı kullanarak çalıştırıldı](./media/functions-create-first-function-python/function-test-cloud-curl.png)

---

> [!TIP]
> Yayımlanmış bir Python uygulaması için neredeyse gerçek zamanlı günlükleri görüntülemek için [Application Insights canlı ölçüm akışı](functions-monitoring.md#streaming-logs)kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki adıma devam ederseniz, [bir Azure depolama kuyruğu çıkış bağlaması ekleyin](functions-add-output-binding-storage-queue-python.md), zaten yaptığınız şekilde oluşturacağınız sürece tüm kaynaklarınızı yerinde tutun.

Aksi takdirde, daha fazla maliyet ödemeden kaçınmak için kaynak grubunu ve içerdiği tüm kaynakları silmek için aşağıdaki komutu kullanın.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure depolama kuyruğu çıkış bağlaması ekleme](functions-add-output-binding-storage-queue-python.md)
