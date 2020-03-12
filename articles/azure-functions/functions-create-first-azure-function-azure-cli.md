---
title: Azure 'da HTTP isteklerine yanıt veren bir işlev oluşturma
description: Komut satırından bir işlev oluşturmayı ve ardından yerel projeyi Azure Işlevleri 'nde sunucusuz barındırma için yayımlamayı öğrenin.
ms.date: 01/28/2020
ms.topic: quickstart
ms.openlocfilehash: f2ec642a477348923e8f587879d4804c07fff5a0
ms.sourcegitcommit: be53e74cd24bbabfd34597d0dcb5b31d5e7659de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79096250"
---
# <a name="quickstart-create-a-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlangıç: Azure 'da HTTP isteklerine yanıt veren bir işlev oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir işlev oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.

Bu makalenin [Visual Studio Code tabanlı bir sürümü](functions-create-first-function-vs-code.md) de vardır.

## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırma

Başlamadan önce aşağıdakilere sahip olmanız gerekir:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell"  
+ [Azure Functions Core Tools](./functions-run-local.md#v2) sürüm 2.7.1846 veya sonraki bir 2. x sürümü.
::: zone-end  
::: zone pivot="programming-language-python"
+ Python 3,6 ve 3,7 için [Azure Functions Core Tools](./functions-run-local.md#v2) sürüm 2.7.1846 veya sonraki bir 2. x sürümü gerekir. Python 3,8, çekirdek araçların [3. x sürümünü](./functions-run-local.md#v2) gerektirir.
::: zone-end

+ [Azure CLI](/cli/azure/install-azure-cli) sürüm 2.0.76 veya üzeri. 
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node. js](https://nodejs.org/), ETKIN LTS ve bakım LTS sürümleri (8.11.1 ve 10.14.1 önerilir).
::: zone-end

::: zone pivot="programming-language-python"
+ Azure Işlevleri tarafından desteklenen Python [3,8](https://www.python.org/downloads/release/python-382/), [Python 3,7](https://www.python.org/downloads/release/python-375/), [Python 3,6](https://www.python.org/downloads/release/python-368/). 
::: zone-end
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Core SDK 2.2 +](https://www.microsoft.com/net/download)
::: zone-end

### <a name="check-your-environment"></a>Ortamınızı denetleyin

+ Bir Terminal veya komut penceresinde, Azure Functions Core Tools sürüm 2.7.1846 veya daha yeni bir 2. x sürümü olup olmadığını denetlemek için `func --version` çalıştırın.

+ Azure CLı sürümünün 2.0.76 veya üzeri olduğunu denetlemek için `az --version` çalıştırın.

+ Azure 'da oturum açmak ve etkin bir aboneliği doğrulamak için `az login` çalıştırın.

::: zone pivot="programming-language-javascript,programming-language-typescript"
+ Node. js sürüm raporlarınızı 8. x veya 10. x olarak denetlemek için `node --version` çalıştırın.
::: zone-end
::: zone pivot="programming-language-python"
+ Python sürüm raporlarınızı 3.8. x, 3.7. x veya 3.6. x olarak denetlemek için `python --version` (Linux/MacOS) veya `py --version` (Windows) çalıştırın.

## <a name="create-venv"></a>Sanal ortam oluşturma ve etkinleştirme

Uygun bir klasörde, `.venv`adlı bir sanal ortam oluşturmak ve etkinleştirmek için aşağıdaki komutları çalıştırın. Azure Işlevleri tarafından desteklenen Python 3,8, 3,7 veya 3,6 kullandığınızdan emin olun.


# <a name="bash"></a>[Bash](#tab/bash)

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

# <a name="powershell"></a>[PowerShell](#tab/powershell)

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

Bu etkinleştirilmiş sanal ortamda sonraki tüm komutları çalıştırırsınız. (Sanal ortamdan çıkmak için `deactivate`çalıştırın.)

::: zone-end

## <a name="create-a-local-function-project"></a>Yerel işlev projesi oluşturma

Azure Işlevlerinde bir işlev projesi, her birinin belirli bir tetikleyiciye yanıt verdiği bir veya daha fazla bağımsız işlev için bir kapsayıcıdır. Projedeki tüm işlevler aynı yerel ve barındırma yapılandırmalarına sahiptir. Bu bölümde, tek bir işlev içeren bir işlev projesi oluşturursunuz.

1. Sanal ortamda, belirtilen çalışma zamanına sahip *Localfunctionproj* adlı bir klasörde bir işlevler projesi oluşturmak için `func init` komutunu çalıştırın:

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


    Bu klasör, [yerel. Settings. JSON](functions-run-local.md#local-settings-file) ve [Host. JSON](functions-host-json.md)adlı yapılandırma dosyaları dahil olmak üzere, proje için çeşitli dosyaları içerir. *Local. Settings. JSON* , Azure 'dan indirilen gizli dizileri içerebildiğinden, dosya, *. gitignore* dosyasında varsayılan olarak kaynak denetiminden çıkarılır.

1. Proje klasörüne gidin:

    ```
    cd LocalFunctionProj
    ```
    
1. Aşağıdaki komutu kullanarak projenize bir işlev ekleyin; burada `--name` bağımsız değişkeni işlevinizin benzersiz adıdır ve `--template` bağımsız değişkeni işlevin tetikleyicisini belirtir. 

    ```
    func new --name HttpExample --template "HTTP trigger"
    ```

    ::: zone pivot="programming-language-csharp"
    `func new` bir HttpExample.cs kod dosyası oluşturur.
    ::: zone-end
    ::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
    `func new`, projenin seçtiği dile ve *function. JSON*adlı yapılandırma dosyasına uygun bir kod dosyası içeren işlev adıyla eşleşen bir alt klasör oluşturur.
    ::: zone-end

### <a name="optional-examine-the-file-contents"></a>Seçim Dosya içeriğini inceleyin

İsterseniz, [işlevi yerel olarak çalıştırmayı](#run-the-function-locally) atlayıp dosya içeriğini daha sonra incelemenizi sağlayabilirsiniz.

::: zone pivot="programming-language-csharp"
#### <a name="httpexamplecs"></a>HttpExample.cs

*HttpExample.cs* değişkeninde istek `req` verilerini alan bir `Run` yöntemi içerir, tetikleyici davranışını tanımlayan **httptriggerattribute**ile donatılmış bir [HttpRequest](/dotnet/api/microsoft.aspnetcore.http.httprequest) ' dir. 

:::code language="csharp" source="~/functions-docs-csharp/http-trigger-template/HttpExample.cs":::

Dönüş nesnesi, bir [Okobjectresult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200) veya [Badrequestobjectresult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestobjectresult) (400) olarak bir yanıt Iletisi döndüren bir [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) . Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](/azure/azure-functions/functions-bindings-http-webhook?tabs=csharp).
::: zone-end

::: zone pivot="programming-language-python"
#### <a name="__init__py"></a>\_\_init\_\_. Kopyala

*\_\_init\_\_. Kopyala* , *function. JSON*içindeki yapılandırmaya göre tetiklenen bir `main()` Python işlevi içeriyor.

:::code language="python" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/__init__.py":::

Bir HTTP tetikleyicisi için işlev, `req` değişkende istek verilerini alır, *function. JSON*içinde tanımlanmıştır. `req`, [Azure. Functions. HttpRequest sınıfının](/python/api/azure-functions/azure.functions.httprequest)bir örneğidir. *Function. JSON*içinde `$return` olarak tanımlanan Return nesnesi, [Azure. Functions. HttpResponse sınıfının](/python/api/azure-functions/azure.functions.httpresponse)bir örneğidir. Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](/azure/azure-functions/functions-bindings-http-webhook?tabs=python).
::: zone-end

::: zone pivot="programming-language-javascript"
#### <a name="indexjs"></a>index. js

*index. js* , *function. JSON*içindeki yapılandırmaya göre tetiklenen bir işlevi dışarı aktarır.

:::code language="javascript" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/index.js":::

Bir HTTP tetikleyicisi için işlev, `req` değişkende istek verilerini alır, *function. JSON*içinde tanımlanmıştır. *Function. JSON*içinde `$return` olarak tanımlanan Return nesnesi yanıt. Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](/azure/azure-functions/functions-bindings-http-webhook?tabs=javascript).
::: zone-end

::: zone pivot="programming-language-typescript"
#### <a name="indexts"></a>Dizin. TS

*Index. TS* , *function. JSON*içindeki yapılandırmaya göre tetiklenen bir işlevi dışarı aktarır.

:::code language="typescript" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-TypeScript/index.ts":::

Bir HTTP tetikleyicisi için işlev,, *function. JSON*Içinde tanımlanan **HttpRequest** türü `req` değişkende istek verilerini alır. *Function. JSON*içinde `$return` olarak tanımlanan Return nesnesi yanıt. 
::: zone-end

::: zone pivot="programming-language-powershell"
#### <a name="runps1"></a>Run. ps1

*Run. ps1* , *function. JSON*içindeki yapılandırmaya göre tetiklenen bir işlev betiğini tanımlar.

:::code language="powershell" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/run.ps1":::

Bir HTTP tetikleyicisi için işlev, *function. JSON*içinde tanımlanan `$Request` param öğesine geçirilen istek verilerini alır. *Function. JSON*içinde `Response` olarak tanımlanan Return nesnesi, yanıt olarak `Push-OutputBinding` cmdlet 'ine geçirilir. 
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
#### <a name="functionjson"></a>function. JSON

*function. JSON* , tetikleyici türü de dahil olmak üzere işlevin giriş ve çıkış `bindings` tanımlayan bir yapılandırma dosyasıdır. 
::: zone-end

::: zone pivot="programming-language-python"
İsterseniz farklı bir Python dosyası çağırmak için `scriptFile` değiştirebilirsiniz.

:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json":::
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json":::
::: zone-end

::: zone pivot="programming-language-powershell"
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json":::
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"  
Her bağlama bir yön, tür ve benzersiz bir ad gerektirir. HTTP tetikleyicisinin tür [`httpTrigger`](functions-bindings-http-webhook-trigger.md) ve [`http`](functions-bindings-http-webhook-output.md)türünde çıkış bağlaması olan bir giriş bağlaması vardır.
::: zone-end  

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

## <a name="create-supporting-azure-resources-for-your-function"></a>İşleviniz için destekleyici Azure kaynakları oluşturma

İşlev kodunuzu Azure 'a dağıtabilmeniz için önce üç kaynak oluşturmanız gerekir:

- İlgili kaynaklar için mantıksal kapsayıcı olan bir kaynak grubu.
- Projelerinizle ilgili durumu ve diğer bilgileri tutan bir depolama hesabı.
- İşlev kodunuzu yürütmek için ortam sağlayan bir işlev uygulaması. Bir işlev uygulaması yerel işlev projenize eşlenir ve kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanızı sağlar.

Bu öğeleri oluşturmak için aşağıdaki Azure CLı komutlarını kullanın. Her komut, tamamlandıktan sonra JSON çıktısı sağlar.

1. Şimdiye kadar yapmadıysanız, [az Login](/cli/azure/reference-index#az-login) komutuyla Azure 'da oturum açın:

    ```azurecli
    az login
    ```
    
1. [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, `westeurope` bölgesinde `AzureFunctionsQuickstart-rg` adlı bir kaynak grubu oluşturur. (`az account list-locations` komutundan kullanılabilir bir bölge kullanarak genellikle kaynak grubunuzu ve kaynaklarınızı size yakın bir bölgede oluşturursunuz.)

    ```azurecli
    az group create --name AzureFunctionsQuickstart-rg --location westeurope
    ```
    ::: zone pivot="programming-language-python"  
    > [!NOTE]
    > Linux ve Windows uygulamalarını aynı kaynak grubunda barındıralamazsınız. Bir Windows işlev uygulaması veya Web uygulamasıyla `AzureFunctionsQuickstart-rg` adlı mevcut bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.
    ::: zone-end  
    
1. [Az Storage Account Create](/cli/azure/storage/account#az-storage-account-create) komutunu kullanarak kaynak grubunuzda ve bölgenizde genel amaçlı bir depolama hesabı oluşturun. Aşağıdaki örnekte, `<STORAGE_NAME>`, sizin için uygun olan bir genel benzersiz adla değiştirin. Adlar yalnızca üç ile 24 karakter arasında ve küçük harflerden oluşmalıdır. `Standard_LRS`, [işlevler tarafından desteklenen](storage-considerations.md#storage-account-requirements)genel amaçlı bir hesabı belirtir.

    ```azurecli
    az storage account create --name <STORAGE_NAME> --location westeurope --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
    ```
    
    Depolama hesabı bu hızlı başlangıç için yalnızca birkaç ilay (USD) doğurur.
    
1. [Az functionapp Create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak işlev uygulamasını oluşturun. Aşağıdaki örnekte, `<STORAGE_NAME>`, önceki adımda kullandığınız hesabın adıyla değiştirin ve `<APP_NAME>`, sizin için uygun olan bir genel benzersiz adla değiştirin. `<APP_NAME>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. 

    ::: zone pivot="programming-language-python"  
    Python 3,8 kullanıyorsanız, `--runtime-version` `3.8` ve `3``--functions_version` değiştirin.
    
    Python 3,6 kullanıyorsanız, `--runtime-version` `3.6`olarak değiştirin.

    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --os-type Linux --consumption-plan-location westeurope --runtime python --runtime-version 3.7 --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  

    ::: zone pivot="programming-language-javascript,programming-language-typescript"  
    Node. js 8 kullanıyorsanız, `--runtime-version` `8`olarak da değiştirin.

    
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

    Bu komut, burada yaptığınız kullanım miktarı için ücretsiz olan [Azure Işlevleri tüketim planı](functions-scale.md#consumption-plan)altında, belirtilen dil çalışma zamanında çalışan bir işlev uygulaması oluşturur. Bu komut Ayrıca, aynı kaynak grubunda, işlev uygulamanızı izleyebilmeniz ve günlükleri görüntüleyebileceğiniz ilişkili bir Azure Application Insights örneğini sağlar. Daha fazla bilgi için bkz. [Azure Işlevlerini izleme](functions-monitoring.md). Örnek, siz etkinleştirene kadar hiçbir maliyet vermez.
    
## <a name="deploy-the-function-project-to-azure"></a>İşlev projesini Azure 'a dağıtma

::: zone pivot="programming-language-typescript"  
Projenizi Azure 'a dağıtmak için temel araçları kullanmadan önce, TypeScript kaynak dosyalarından bir dizi JavaScript dosyası oluşturun.

Aşağıdaki komut, TypeScript projenizi dağıtım için hazırlar:

```
npm run build:production 
```
::: zone-end  

Gerekli kaynaklarla, artık yerel işlevler projenizi, [Func Azure functionapp Publish](functions-run-local.md#project-file-deployment) komutunu kullanarak Azure 'daki işlev uygulamasına dağıtmaya hazırsınız demektir. Aşağıdaki örnekte `<APP_NAME>` değerini uygulamanızın adıyla değiştirin.

```
func azure functionapp publish <APP_NAME>
```

"Adı olan uygulama bulunamıyor..." hatasını görürseniz, birkaç saniye bekleyip yeniden deneyin. Bu işlem, Azure, önceki `az functionapp create` komutundan sonra uygulamayı tam olarak başlatılamayabilir.

Yayımla komutu aşağıdaki çıktıya benzer sonuçları gösterir (basitlik için kesildi):

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

## <a name="invoke-the-function-on-azure"></a>Azure 'da işlevi çağırma

İşleviniz bir HTTP tetikleyicisi kullandığından, tarayıcıda URL 'sine veya kıvrıcı gibi bir araçla HTTP isteği yaparak bu uygulamayı çağırabilirsiniz. Her iki örnekte de `code` URL parametresi, işlev uç noktanızın çağrılmasını yetkilendiren benzersiz [işleviniz anahtarıdır](functions-bindings-http-webhook-trigger.md#authorization-keys) .

# <a name="browser"></a>[Tarayıcı](#tab/browser)

Yayımla komutunun çıktısında gösterilen tüm **ÇAĞıRMA URL** 'sini bir tarayıcı adres çubuğuna kopyalayın, `&name=Functions`sorgu parametresini ekleyin. , İşlevi yerel olarak çalıştırdığınızda tarayıcı benzer bir çıktı görüntülemelidir.

![Azure üzerinde çalışan işlevin çıktısı bir tarayıcıda](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-browser.png)


# <a name="curl"></a>[kıvr](#tab/curl)

[`curl`](https://curl.haxx.se/) `&name=Functions`parametresini ekleyerek **Invoke URL**'siyle çalıştırın. Komutun çıktısı, "Hello Functions" metni olmalıdır.

![İşlevin çıktısı, Azure 'da kıvrımlı kullanarak çalıştırıldı](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-curl.png)

---

> [!TIP]
> Yayınlanan işlev uygulaması için neredeyse gerçek zamanlı günlükleri görüntülemek için [Application Insights canlı ölçüm akışı](functions-monitoring.md#streaming-logs)kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki adıma devam ederseniz, [bir Azure depolama kuyruğu çıkış bağlaması ekleyin](functions-add-output-binding-storage-queue-cli.md), zaten yaptığınız şekilde oluşturacağınız sürece tüm kaynaklarınızı yerinde tutun.

Aksi takdirde, daha fazla maliyet ödemeden kaçınmak için kaynak grubunu ve içerdiği tüm kaynakları silmek için aşağıdaki komutu kullanın.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure depolama kuyruğuna bağlanma](functions-add-output-binding-storage-queue-cli.md)
