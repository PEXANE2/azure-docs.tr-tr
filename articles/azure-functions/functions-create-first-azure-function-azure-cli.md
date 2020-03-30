---
title: Azure'da HTTP isteklerine yanıt veren bir işlev oluşturma
description: Komut satırından nasıl bir işlev oluşturlayacağınızı ve ardından yerel projeyi Azure İşlevlerinde sunucusuz barındırmaya nasıl yayınlayacağınızı öğrenin.
ms.date: 01/28/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 89b6a9f31414cbaa9cc92c1a0d881a1354180990
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282741"
---
# <a name="quickstart-create-a-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlatma: Azure'da HTTP isteklerine yanıt veren bir işlev oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir işlev oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak sınandıktan sonra, kodu Azure İşlevlerinin sunucusuz ortamına dağıtirsınız. Bu hızlı başlangıcın tamamlanması, Azure hesabınızda birkaç USD sent veya daha az küçük bir maliyete neden olabilir.

Ayrıca bu makalenin [Visual Studio Code tabanlı bir sürümü](functions-create-first-function-vs-code.md) vardır.

## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırın

Başlamadan önce aşağıdakileri almalısınız:

+ Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell"  
+ [Azure İşletme Temel Araçları](./functions-run-local.md#v2) sürümü 2.7.1846 veya daha sonraki bir 2.x sürümü.
::: zone-end  
::: zone pivot="programming-language-python"
+ Python 3.6 ve 3.7, [Azure İşlevler Çekirdek Araçları](./functions-run-local.md#v2) sürümü 2.7.1846 veya daha sonraki bir 2.x sürümü gerektirir. Python 3.8, Çekirdek Araçları'nın [3.x sürümünü](./functions-run-local.md#v2) gerektirir.
::: zone-end

+ [Azure CLI](/cli/azure/install-azure-cli) sürümü 2.0.76 veya sonrası. 
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node.js](https://nodejs.org/), Active LTS ve Maintenance LTS versiyonları (8.11.1 ve 10.14.1 önerilir).
::: zone-end

::: zone pivot="programming-language-python"
+ [Python 3.8](https://www.python.org/downloads/release/python-382/), [Python 3.7](https://www.python.org/downloads/release/python-375/), [Python 3.6](https://www.python.org/downloads/release/python-368/), Azure Fonksiyonları (x64) tarafından desteklenir.
::: zone-end
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Çekirdek SDK 2.2+](https://www.microsoft.com/net/download)
::: zone-end

### <a name="check-your-environment"></a>Çevrenizi kontrol edin

+ Bir terminal veya komut `func --version` penceresinde, Azure İşlevler Temel Araçları'nın sürüm 2.7.1846 veya daha sonraki bir 2.x sürümü olup olmadığını kontrol etmek için çalıştırın.

+ Azure `az --version` CLI sürümünün 2.0.76 veya sonraki sürüm olup olmadığını kontrol etmek için çalıştırın.

+ Azure'da oturum açmak ve etkin bir aboneliği doğrulamak için çalıştırın. `az login`

::: zone pivot="programming-language-javascript,programming-language-typescript"
+ Node.js sürüm raporlarınızı 8.x veya 10.x olarak kontrol etmek için çalıştırın. `node --version`
::: zone-end
::: zone pivot="programming-language-python"
+ Python `python --version` sürüm raporlarınızı 3.8.x, 3.7.x veya 3.6.x olarak denetlemek için (Linux/MacOS) veya `py --version` (Windows) çalıştırın.

## <a name="create-and-activate-a-virtual-environment"></a><a name="create-venv"></a>Sanal ortam oluşturma ve etkinleştirme

Uygun bir klasörde, ', adlı sanal bir ortam `.venv`oluşturmak ve etkinleştirmek için aşağıdaki komutları çalıştırın. Azure İşlevleri tarafından desteklenen Python 3.8, 3.7 veya 3.6'yı kullandığınızdan emin olun.


# <a name="bash"></a>[bash](#tab/bash)

```bash
python -m venv .venv
```

```bash
source .venv/bin/activate
```

Python venv paketini Linux dağıtımınıza yüklemediyse aşağıdaki komutu çalıştırın:

```bash
sudo apt-get install python3-venv
```

# <a name="powershell"></a>[Powershell](#tab/powershell)

```powershell
py -m venv .venv
```

```powershell
.venv\scripts\activate
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -m venv .venv
```

```cmd
.venv\scripts\activate
```

---

Bu etkinleştirilen sanal ortamda sonraki tüm komutları çalıştırın. (Sanal ortamdan çıkmak `deactivate`için çalıştırın.)

::: zone-end

## <a name="create-a-local-function-project"></a>Yerel bir işlev projesi oluşturma

Azure İşlevlerinde işlev projesi, her biri belirli bir tetikleyiciye yanıt veren bir veya daha fazla ayrı işlev için bir kapsayıcıdır. Projedeki tüm işlevler aynı yerel ve barındırma yapılandırmalarını paylaşır. Bu bölümde, tek bir işlev içeren bir işlev projesi oluşturursunuz.

1. Sanal ortamda, belirtilen `func init` çalışma süresi ile *LocalFunctionProj* adlı bir klasörde bir işlev ler projesi oluşturmak için komutu çalıştırın:

    ::: zone pivot="programming-language-python"
    ```
    func init LocalFunctionProj --python
    ```
    ::: zone-end
    ::: zone pivot="programming-language-csharp"
    ```
    func init LocalFunctionProj --dotnet
    ```
    ::: zone-end
    ::: zone pivot="programming-language-javascript"
    ```
    func init LocalFunctionProj --javascript
    ```
    ::: zone-end
    ::: zone pivot="programming-language-typescript"
    ```
    func init LocalFunctionProj --typescript
    ```
    ::: zone-end
    ::: zone pivot="programming-language-powershell"
    ```
    func init LocalFunctionProj --powershell
    ```
    ::: zone-end


    Bu klasör, [local.settings.json](functions-run-local.md#local-settings-file) ve [host.json](functions-host-json.md)adlı yapılandırma dosyaları da dahil olmak üzere proje için çeşitli dosyalar içerir. *local.settings.json* Azure'dan indirilen sırları içerebildiği *için,..gitignore* dosyasında varsayılan olarak dosya kaynak denetiminden çıkarılır.

1. Proje klasörüne gidin:

    ```
    cd LocalFunctionProj
    ```
    
1. `--name` Bağımsız değişkenin işlevinizin benzersiz adı olduğu ve `--template` bağımsız değişkenin işlevin tetikleyicisini belirttiği aşağıdaki komutu kullanarak projenize bir işlev ekleyin. 

    ```
    func new --name HttpExample --template "HTTP trigger"
    ```

    ::: zone pivot="programming-language-csharp"
    `func new`HttpExample.cs kod dosyası oluşturur.
    ::: zone-end
    ::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
    `func new`projenin seçilen diline uygun bir kod dosyası ve *function.json*adlı yapılandırma dosyası içeren işlev adı ile eşleşen bir alt klasör oluşturur.
    ::: zone-end

### <a name="optional-examine-the-file-contents"></a>(İsteğe bağlı) Dosya içeriğini inceleme

İstenirse, [işlevi yerel olarak çalıştırmayı](#run-the-function-locally) atlayabilir ve daha sonra dosya içeriğini inceleyebilirsiniz.

::: zone pivot="programming-language-csharp"
#### <a name="httpexamplecs"></a>HttpExample.cs

`Run` [HttpRequest](/dotnet/api/microsoft.aspnetcore.http.httprequest) *HttpExample.cs* **HttpTriggerAttribute**HttpExample.cs, tetiği davranışını tanımlayan HttpTriggerAtöz ile dekore edilmiş bir HttpRequest'tir. `req` 

:::code language="csharp" source="~/functions-docs-csharp/http-trigger-template/HttpExample.cs":::

İade nesnesi, yanıt iletisini [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200) veya [BadRequestObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestobjectresult) (400) olarak döndüren bir [Eylem](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) Sonucudur. Daha fazla bilgi edinmek için [Azure İşlevleri HTTP tetikleyicileri ve bağlamalarına](/azure/azure-functions/functions-bindings-http-webhook?tabs=csharp)bakın.
::: zone-end

::: zone pivot="programming-language-python"
#### <a name="__init__py"></a>\_\_init\_\_.py

*\_\_init\_\_.py,* *function.json'daki*yapılandırmaya göre tetiklenen bir `main()` Python işlevi içerir.

:::code language="python" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/__init__.py":::

Bir HTTP tetikleyicisi için, `req` *işlev, işlev.json'da*tanımlandığı şekilde değişkendeki istek verilerini alır. `req`[azure.functions'ın bir örneğidir.HttpRequest sınıfı](/python/api/azure-functions/azure.functions.httprequest). *function.json*olarak `$return` tanımlanan return object, [azure.functions.HttpResponse sınıfının](/python/api/azure-functions/azure.functions.httpresponse)bir örneğidir. Daha fazla bilgi edinmek için [Azure İşlevleri HTTP tetikleyicileri ve bağlamalarına](/azure/azure-functions/functions-bindings-http-webhook?tabs=python)bakın.
::: zone-end

::: zone pivot="programming-language-javascript"
#### <a name="indexjs"></a>index.js

*index.js,* *function.json'daki*yapılandırmaya göre tetiklenen bir işlev ihraç eder.

:::code language="javascript" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/index.js":::

Bir HTTP tetikleyicisi için, `req` *işlev, işlev.json'da*tanımlandığı şekilde değişkendeki istek verilerini alır. *function.json*olarak `$return` tanımlanan return object, yanıttır. Daha fazla bilgi edinmek için [Azure İşlevleri HTTP tetikleyicileri ve bağlamalarına](/azure/azure-functions/functions-bindings-http-webhook?tabs=javascript)bakın.
::: zone-end

::: zone pivot="programming-language-typescript"
#### <a name="indexts"></a>index.ts

*index.ts,* *function.json'daki*yapılandırmaya göre tetiklenen bir işlev ihraç eder.

:::code language="typescript" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-TypeScript/index.ts":::

Bir HTTP tetikleyicisi için, `req` *işlev, işlev.json'da*tanımlandığı şekilde **HttpRequest** türünün değişkeninde istek verilerini alır. *function.json*olarak `$return` tanımlanan return object, yanıttır. 
::: zone-end

::: zone pivot="programming-language-powershell"
#### <a name="runps1"></a>run.ps1

*run.ps1,* *function.json'daki*yapılandırmaya göre tetiklenen bir işlev komut dosyası tanımlar.

:::code language="powershell" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/run.ps1":::

Bir HTTP tetikleyicisi için, `$Request` *işlev, function.json'da*tanımlanan param'a geçirilen istek verilerini alır. *function.json*olarak `Response` tanımlanan return object, yanıt `Push-OutputBinding` olarak cmdlet'e geçirilir. 
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
#### <a name="functionjson"></a>function.json

*function.json,* tetikleyici türü de dahil olmak `bindings` üzere işlevin giriş ve çıktısını tanımlayan bir yapılandırma dosyasıdır. 
::: zone-end

::: zone pivot="programming-language-python"
İsterseniz `scriptFile` farklı bir Python dosyasını çağırmak için değiştirebilirsiniz.

:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json":::
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json":::
::: zone-end

::: zone pivot="programming-language-powershell"
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json":::
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"  
Her bağlama bir yön, bir tür ve benzersiz bir ad gerektirir. HTTP tetikleyici türü [`httpTrigger`](functions-bindings-http-webhook-trigger.md) [`http`](functions-bindings-http-webhook-output.md)ve çıkış türü bağlayıcı bir giriş bağlama vardır.
::: zone-end  

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

## <a name="create-supporting-azure-resources-for-your-function"></a>İşleviniz için destekleyici Azure kaynakları oluşturun

İşlev kodunuzu Azure'a dağıtmadan önce üç kaynak oluşturmanız gerekir:

- İlgili kaynaklar için mantıksal bir kapsayıcı olan kaynak grubu.
- Projelerinizle ilgili durumu ve diğer bilgileri koruyan bir Depolama hesabı.
- İşlev kodunuzu yürütmek için ortam sağlayan bir işlev uygulaması. İşlev uygulaması yerel işlev projenizi eşler ve daha kolay yönetim, dağıtım ve kaynakların paylaşımı için işlevleri mantıksal bir birim olarak gruplandırmanızı sağlar.

Bu öğeleri oluşturmak için aşağıdaki Azure CLI komutlarını kullanın. Her komut tamamlandıktan sonra JSON çıkışı sağlar.

1. Bunu daha önce yapmadıysanız, [az giriş](/cli/azure/reference-index#az-login) komutuyla Azure'da oturum açın:

    ```azurecli
    az login
    ```
    
1. [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, `AzureFunctionsQuickstart-rg` `westeurope` bölgede adlı bir kaynak grubu oluşturur. (Genellikle `az account list-locations` kaynak grubunuzu ve kaynaklarınızı komuttan kullanılabilir bir bölgeyi kullanarak yakınınızdaki bir bölgede oluşturursunuz.)

    ```azurecli
    az group create --name AzureFunctionsQuickstart-rg --location westeurope
    ```
    ::: zone pivot="programming-language-python"  
    > [!NOTE]
    > Linux ve Windows uygulamalarını aynı kaynak grubunda barındıramaz. Windows işlev uygulaması uygulaması `AzureFunctionsQuickstart-rg` veya web uygulamasıyla birlikte adında varolan bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.
    ::: zone-end  
    
1. Az depolama hesabı oluşturma komutunu kullanarak kaynak grubunuzda ve bölgenizde genel amaçlı bir [depolama hesabı oluşturun.](/cli/azure/storage/account#az-storage-account-create) Aşağıdaki örnekte, `<STORAGE_NAME>` size uygun genel olarak benzersiz bir ad la değiştirin. Adlar yalnızca üç ila 24 karakter lisi ve küçük harf içermelidir. `Standard_LRS`[fonksiyonlar tarafından desteklenen](storage-considerations.md#storage-account-requirements)genel amaçlı bir hesap belirtir.

    ```azurecli
    az storage account create --name <STORAGE_NAME> --location westeurope --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
    ```
    
    Depolama hesabı bu hızlı başlangıç için yalnızca birkaç sent (USD) alır.
    
1. [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak fonksiyon uygulamasını oluşturun. Aşağıdaki örnekte, `<STORAGE_NAME>` önceki adımda kullandığınız hesabın adını değiştirin ve `<APP_NAME>` size uygun genel olarak benzersiz bir adla değiştirin. `<APP_NAME>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. 

    ::: zone pivot="programming-language-python"  
    Python 3.8 kullanıyorsanız, `--runtime-version` `3.8` değiştirin `--functions_version` `3`ve ' a
    
    Python 3.6 kullanıyorsanız, `--runtime-version` 'ye `3.6`değiştirin.

    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --os-type Linux --consumption-plan-location westeurope --runtime python --runtime-version 3.7 --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  

    ::: zone pivot="programming-language-javascript,programming-language-typescript"  
    Düğüm.js 8 kullanıyorsanız, aynı `--runtime-version` zamanda `8`değiştirin.

    
    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location westeurope --runtime node --runtime-version 10 --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  

    ::: zone pivot="programming-language-csharp"  
    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location westeurope --runtime dotnet --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  
    
    ::: zone pivot="programming-language-powershell"  
    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location westeurope --runtime powershell --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  

    Bu komut, [azure işlevleri tüketim planı](functions-scale.md#consumption-plan)altında belirtilen dilde çalışma zamanında çalışan bir işlev uygulaması oluşturur ve burada maruz kaldığınız kullanım miktarı için ücretsizdir. Komut ayrıca, işlev uygulamanızı izleyip günlüklerinizi görüntüleyebileceğiniz ilişkili bir Azure Uygulama Öngörüleri örneğini de aynı kaynak grubunda karşılar. Daha fazla bilgi için [bkz.](functions-monitoring.md) Örneği etkinleştirene kadar hiçbir ücret etüt etmez.
    
## <a name="deploy-the-function-project-to-azure"></a>İşlev projesini Azure'a dağıtma

::: zone pivot="programming-language-typescript"  
Projenizi Azure'a dağıtmak için Temel Araçlar'ı kullanmadan önce, TypeScript kaynak dosyalarından üretime hazır bir JavaScript dosyaları oluşturursunuz.

Aşağıdaki komut TypeScript projenizi dağıtıma hazırlar:

```
npm run build:production 
```
::: zone-end  

Gerekli kaynaklar yerindeyken, [func azure functionapp publish](functions-run-local.md#project-file-deployment) komutunu kullanarak yerel işlevler projenizi Azure'daki işlev uygulamasına dağıtmaya hazırsınız. Aşağıdaki örnekte, `<APP_NAME>` uygulamanızın adını değiştirin.

```
func azure functionapp publish <APP_NAME>
```

Hatayı görürseniz, "Adı olan uygulamayı bulamıyorum...", birkaç saniye bekleyin ve Azure önceki `az functionapp create` komuttan sonra uygulamayı tam olarak başlatmamış olabileceğinden, yeniden deneyin.

Yayımlama komutu aşağıdaki çıktıya benzer sonuçları gösterir (basitlik için kesilen):

<pre>
...

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
</pre>

## <a name="invoke-the-function-on-azure"></a>Azure'da işlevi çağırma

Işleviniz bir HTTP tetikleyicisi kullandığından, tarayıcıdaki URL'sine veya kıvrılma gibi bir araçla BIR HTTP isteği nde bulunarak bu tetikleyiciyi çağırırsınız. Her iki durumda `code` da, URL parametresi, işlev bitiş noktanızın çağrılması için yetki veren benzersiz [işlev anahtarınızdır.](functions-bindings-http-webhook-trigger.md#authorization-keys)

# <a name="browser"></a>[Tarayıcı](#tab/browser)

Yayımlama komutunun çıktısında gösterilen invoke **URL'sinin** tamamını bir tarayıcı adresi `&name=Functions`çubuğuna kopyalayın ve sorgu parametresini ekler. Tarayıcı, işlevi yerel olarak çalıştırdığınızda olduğu gibi benzer çıktıları görüntülemelidir.

![İşlevin çıktısı bir tarayıcıda Azure'da çalışır](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-browser.png)


# <a name="curl"></a>[Curl](#tab/curl)

Invoke [`curl`](https://curl.haxx.se/) **URL'si**ile çalıştırın, parametreyi `&name=Functions`ekle. Komutun çıktısı "Merhaba İşlevler" metni olmalıdır.

![İşlevin çıktısı curl kullanarak Azure'da çalışır](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-curl.png)

---

> [!TIP]
> Yayınlanan bir işlev uygulaması için neredeyse gerçek zamanlı günlükleri görüntülemek için [Application Insights Live Metrics Stream'i](functions-monitoring.md#streaming-logs)kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki adıma devam ederseniz, [Azure Depolama sıra çıktısı bağlama ekleyin,](functions-add-output-binding-storage-queue-cli.md)daha önce yaptıklarınızı geliştireceğiniz gibi tüm kaynaklarınızı yerinde tutun.

Aksi takdirde, kaynak grubunu ve içerdiği tüm kaynakları silmek için aşağıdaki komutu kullanın.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Depolama kuyruğuna bağlanma](functions-add-output-binding-storage-queue-cli.md)
