---
title: Azure'da HTTP isteklerine yanıt veren bir işlev oluşturma
description: Komut satırından nasıl bir işlev oluşturlayacağınızı ve ardından yerel projeyi Azure İşlevlerinde sunucusuz barındırmaya nasıl yayınlayacağınızı öğrenin.
ms.date: 03/30/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 547acb42be6a0c89dbc315031c46192b5c9a4a0d
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732803"
---
# <a name="quickstart-create-a-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlatma: Azure'da HTTP isteklerine yanıt veren bir işlev oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir işlev oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak sınandıktan sonra, kodu Azure İşlevlerinin sunucusuz ortamına dağıtirsınız. Bu hızlı başlangıcın tamamlanması, Azure hesabınızda birkaç USD sent veya daha az küçük bir maliyete neden olabilir.

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
Ayrıca bu makalenin [Visual Studio Code tabanlı bir sürümü](functions-create-first-function-vs-code.md) vardır.
::: zone-end  

::: zone pivot="programming-language-java"  
> [!NOTE]
> Maven tercih geliştirme aracı değilse, [Gradle,](/azure/azure-functions/functions-create-first-java-gradle) [IntelliJ IDEA](/azure/developer/java/toolkit-for-intellij/quickstart-functions) ve [VS Kodu](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-java)kullanarak Java geliştiricileri için benzer öğreticiler göz atın.
::: zone-end  

[!INCLUDE [functions-requirements-cli](../../includes/functions-requirements-cli.md)]

[!INCLUDE [functions-cli-verify-prereqs](../../includes/functions-cli-verify-prereqs.md)]

[!INCLUDE [functions-cli-create-venv](../../includes/functions-cli-create-venv.md)]

## <a name="create-a-local-function-project"></a>Yerel bir işlev projesi oluşturma

Azure İşlevlerinde işlev projesi, her biri belirli bir tetikleyiciye yanıt veren bir veya daha fazla ayrı işlev için bir kapsayıcıdır. Projedeki tüm işlevler aynı yerel ve barındırma yapılandırmalarını paylaşır. Bu bölümde, tek bir işlev içeren bir işlev projesi oluşturursunuz.

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
Belirtilen `func init` çalışma süresiile *LocalFunctionProj* adlı bir klasörde bir işlev projesi oluşturmak için aşağıdaki gibi komutu çalıştırın:  
::: zone-end  
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
::: zone pivot="programming-language-java"  
İşlevler projesini bir [Maven arketipinden](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) oluşturmak için boş bir klasörde aşağıdaki komutu çalıştırın.

# <a name="bash"></a>[bash](#tab/bash)
```bash
mvn archetype:generate -DarchetypeGroupId=com.microsoft.azure -DarchetypeArtifactId=azure-functions-archetype 
```
# <a name="powershell"></a>[PowerShell](#tab/powershell)
```powershell
mvn archetype:generate "-DarchetypeGroupId=com.microsoft.azure" "-DarchetypeArtifactId=azure-functions-archetype" 
```
# <a name="cmd"></a>[Cmd](#tab/cmd)
```cmd
mvn archetype:generate "-DarchetypeGroupId=com.microsoft.azure" "-DarchetypeArtifactId=azure-functions-archetype" 
```
---

Maven, dağıtımı sırasında projeoluşturmayı bitirmek için gereken değerleri sizden sorar.   
İstendiğinde aşağıdaki değerleri sağlayın:

| İstem | Değer | Açıklama |
| ------ | ----- | ----------- |
| **groupId** | `com.fabrikam` | Java için [paket adlandırma kurallarını](https://docs.oracle.com/javase/specs/jls/se6/html/packages.html#7.7) izleyerek, projenizi tüm projelerde benzersiz olarak tanımlayan bir değer. |
| **Artifactıd** | `fabrikam-functions` | Bir sürüm numarası olmayan kavanozun adı olan bir değer. |
| **Sürüm** | `1.0-SNAPSHOT` | Varsayılan değeri seçin. |
| **Paket** | `com.fabrikam.functions` | Oluşturulan işlev kodu için Java paketi olan bir değer. Varsayılan değeri kullanın. |

Onaylamak `Y` için Enter yazın veya enter tuşuna basın.

Maven, bu örnekte yer alan _artifactId_adında yeni bir klasörde proje dosyalarını `fabrikam-functions`oluşturur. 
::: zone-end  
Proje klasörüne gidin:

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
```
cd LocalFunctionProj
```
::: zone-end  
::: zone pivot="programming-language-java"  
```
cd fabrikam-functions
```
::: zone-end  
Bu klasör, [local.settings.json](functions-run-local.md#local-settings-file) ve [host.json](functions-host-json.md)adlı yapılandırma dosyaları da dahil olmak üzere proje için çeşitli dosyalar içerir. *local.settings.json* Azure'dan indirilen sırları içerebildiği *için,..gitignore* dosyasında varsayılan olarak dosya kaynak denetiminden çıkarılır.

[!INCLUDE [functions-cli-add-function](../../includes/functions-cli-add-function.md)]

### <a name="optional-examine-the-file-contents"></a>(İsteğe bağlı) Dosya içeriğini inceleme

İstenirse, [işlevi yerel olarak çalıştırmayı](#run-the-function-locally) atlayabilir ve daha sonra dosya içeriğini inceleyebilirsiniz.

::: zone pivot="programming-language-csharp"
#### <a name="httpexamplecs"></a>HttpExample.cs

`Run` [HttpRequest](/dotnet/api/microsoft.aspnetcore.http.httprequest) *HttpExample.cs* **HttpTriggerAttribute**HttpExample.cs, tetiği davranışını tanımlayan HttpTriggerAtöz ile dekore edilmiş bir HttpRequest'tir. `req` 

:::code language="csharp" source="~/functions-docs-csharp/http-trigger-template/HttpExample.cs":::

İade nesnesi, yanıt iletisini [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200) veya [BadRequestObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestobjectresult) (400) olarak döndüren bir [Eylem](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) Sonucudur. Daha fazla bilgi edinmek için [Azure İşlevleri HTTP tetikleyicileri ve bağlamalarına](/azure/azure-functions/functions-bindings-http-webhook?tabs=csharp)bakın.
::: zone-end

::: zone pivot="programming-language-java"
#### <a name="functionjava"></a>Fonksiyon.java
`request` *Function.java,* `run` istek verilerini değişkenden alan bir yöntem içerir, tetikleyici davranışı tanımlayan [HttpTrigger](/java/api/com.microsoft.azure.functions.annotation.httptrigger) ek açıklamasıyla dekore edilmiş bir [HttpRequestMessage'dır.](/java/api/com.microsoft.azure.functions.httprequestmessage) 

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java":::

Yanıt iletisi [HttpResponseMessage.Builder](/java/api/com.microsoft.azure.functions.httpresponsemessage.builder) API tarafından oluşturulur.

#### <a name="pomxml"></a>pom.xml

Uygulamanızı barındırmak için oluşturulan Azure kaynaklarının ayarları, oluşturulan pom.xml `com.microsoft.azure` dosyasında **groupId** ile eklentinin **yapılandırma** öğesinde tanımlanır. Örneğin, aşağıdaki yapılandırma öğesi, `java-functions-group` `westus` bölgedeki kaynak grubunda bir işlev uygulaması oluşturmak için Maven tabanlı bir dağıtım talimatı verir. İşlev uygulamasının kendisi, varsayılan `java-functions-app-service-plan` olarak sunucusuz bir Tüketim planı olan planda barındırılan Windows'ta çalışır.    

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/pom.xml" range="116-155":::

Bu ayarları, ilk dağıtımdan `runtime.os` `windows` öncekine `linux` geçiş yapmak gibi Kaynakların Azure'da nasıl oluşturulduğunu denetlemek için değiştirebilirsiniz. Maven eklentisi tarafından desteklenen ayarların tam listesi için [yapılandırma ayrıntılarına](https://github.com/microsoft/azure-maven-plugins/wiki/Azure-Functions:-Configuration-Details)bakın.

#### <a name="functiontestjava"></a>FonksiyonTest.java

Arketip ayrıca işleviniz için bir birim testi oluşturur. Projeye bağlama eklemek veya yeni işlevler eklemek için işlevinizi değiştirdiğinizde, *FunctionTest.java* dosyasındaki testleri de değiştirmeniz gerekir.
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

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell,programming-language-csharp"    
## <a name="create-supporting-azure-resources-for-your-function"></a>İşleviniz için destekleyici Azure kaynakları oluşturun

İşlev kodunuzu Azure'a dağıtmadan önce üç kaynak oluşturmanız gerekir:

- İlgili kaynaklar için mantıksal bir kapsayıcı olan kaynak grubu.
- Projelerinizle ilgili durumu ve diğer bilgileri koruyan bir Depolama hesabı.
- İşlev kodunuzu yürütmek için ortam sağlayan bir işlev uygulaması. İşlev uygulaması yerel işlev projenizi eşler ve daha kolay yönetim, dağıtım ve kaynakların paylaşımı için işlevleri mantıksal bir birim olarak gruplandırmanızı sağlar.

Bu öğeleri oluşturmak için aşağıdaki Azure CLI komutlarını kullanın. Her komut tamamlandıktan sonra JSON çıkışı sağlar.

Bunu daha önce yapmadıysanız, [az giriş](/cli/azure/reference-index#az-login) komutuyla Azure'da oturum açın:

```azurecli
az login
```
    
[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, `AzureFunctionsQuickstart-rg` `westeurope` bölgede adlı bir kaynak grubu oluşturur. (Genellikle `az account list-locations` kaynak grubunuzu ve kaynaklarınızı komuttan kullanılabilir bir bölgeyi kullanarak yakınınızdaki bir bölgede oluşturursunuz.)

```azurecli
az group create --name AzureFunctionsQuickstart-rg --location westeurope
```

> [!NOTE]
> Linux ve Windows uygulamalarını aynı kaynak grubunda barındıramaz. Windows işlev uygulaması uygulaması `AzureFunctionsQuickstart-rg` veya web uygulamasıyla birlikte adında varolan bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.
 
    
Az depolama hesabı oluşturma komutunu kullanarak kaynak grubunuzda ve bölgenizde genel amaçlı bir [depolama hesabı oluşturun.](/cli/azure/storage/account#az-storage-account-create) Aşağıdaki örnekte, `<STORAGE_NAME>` size uygun genel olarak benzersiz bir ad la değiştirin. Adlar yalnızca üç ila 24 karakter lisi ve küçük harf içermelidir. `Standard_LRS`[fonksiyonlar tarafından desteklenen](storage-considerations.md#storage-account-requirements)genel amaçlı bir hesap belirtir.

```azurecli
az storage account create --name <STORAGE_NAME> --location westeurope --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
```

Depolama hesabı bu hızlı başlangıç için yalnızca birkaç sent (USD) alır.
    
[az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak fonksiyon uygulamasını oluşturun. Aşağıdaki örnekte, `<STORAGE_NAME>` önceki adımda kullandığınız hesabın adını değiştirin ve `<APP_NAME>` size uygun genel olarak benzersiz bir adla değiştirin. `<APP_NAME>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. 
::: zone-end  

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

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell,programming-language-csharp"  
Bu komut, [azure işlevleri tüketim planı](functions-scale.md#consumption-plan)altında belirtilen dilde çalışma zamanında çalışan bir işlev uygulaması oluşturur ve burada maruz kaldığınız kullanım miktarı için ücretsizdir. Komut ayrıca, işlev uygulamanızı izleyip günlüklerinizi görüntüleyebileceğiniz ilişkili bir Azure Uygulama Öngörüleri örneğini de aynı kaynak grubunda karşılar. Daha fazla bilgi için [bkz.](functions-monitoring.md) Örneği etkinleştirene kadar hiçbir ücret etüt etmez.
    
## <a name="deploy-the-function-project-to-azure"></a>İşlev projesini Azure'a dağıtma
::: zone-end  

::: zone pivot="programming-language-typescript"  
Projenizi Azure'a dağıtmak için Temel Araçlar'ı kullanmadan önce, TypeScript kaynak dosyalarından üretime hazır bir JavaScript dosyaları oluşturursunuz.

Aşağıdaki komut TypeScript projenizi dağıtıma hazırlar:

```
npm run build:production 
```
::: zone-end  

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell,programming-language-csharp"  
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

::: zone-end  
::: zone pivot="programming-language-java"  
## <a name="deploy-the-function-project-to-azure"></a>İşlev projesini Azure'a dağıtma

İşlevprojenizi ilk dağıttığınızda Azure'da bir işlev uygulaması ve ilgili kaynaklar oluşturulur. Uygulamanızı barındırmak için oluşturulan Azure kaynaklarının ayarları [pom.xml dosyasında](#pomxml)tanımlanır. Bu makalede, varsayılanları kabul eeceksiniz.

> [!TIP]
> Windows yerine Linux üzerinde çalışan bir işlev `runtime.os` uygulaması oluşturmak için, pom.xml dosyasındaki öğeyi ' den `windows` ' e `linux`değiştirin. Linux'u bir tüketim planında çalıştırmak [bu bölgelerde](https://github.com/Azure/azure-functions-host/wiki/Linux-Consumption-Regions)desteklenir. Linux'ta çalışan uygulamalara ve aynı kaynak grubunda Windows'da çalışan uygulamalara sahip olamazsınız.

Dağıtmadan önce Azure aboneliğinizde oturum açmak için [az giriş](/cli/azure/authenticate-azure-cli) Azure CLI komutunu kullanın. 

```azurecli
az login
```

Projenizi yeni bir işlev uygulamasına dağıtmak için aşağıdaki komutu kullanın. 

```
mvn azure-functions:deploy
```

Bu, Azure'da aşağıdaki kaynakları oluşturur:

+ Kaynak grubu. _Java-functions-group_olarak adlandırılır.
+ Depolama hesabı. Fonksiyonlar tarafından gereklidir. Ad, Depolama hesabı adı gereksinimlerine göre rasgele oluşturulur.
+ Hosting planı. _Westus_ bölgesindeki işlev uygulamanız için sunucusuz barındırma. Adı _java-functions-app-service-plan._
+ Fonksiyon uygulaması. İşlevler için dağıtım ve yürütme birimidir. Ad rasgele oluşturulan bir sayı ile _eklenen, artifakı_D'niz olduğunuza göre rasgele oluşturulur. 

Dağıtım, proje dosyalarını paketler ve [zip dağıtımını](functions-deployment-technologies.md#zip-deploy)kullanarak bunları yeni işlev uygulamasına dağıtır. Kod, Azure'daki dağıtım paketinden çalışır.
::: zone-end

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
