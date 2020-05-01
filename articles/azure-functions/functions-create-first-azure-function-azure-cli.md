---
title: Azure 'da HTTP isteklerine yanıt veren bir işlev oluşturma
description: Komut satırından bir işlev oluşturmayı ve ardından yerel projeyi Azure Işlevleri 'nde sunucusuz barındırma için yayımlamayı öğrenin.
ms.date: 03/30/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 5022ff7239a040a163d8d6a82864d0dee8e65f1e
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610913"
---
::: zone pivot="programming-language-csharp"  
# <a name="quickstart-create-a-c-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlangıç: Azure 'da HTTP isteklerine yanıt veren bir C# işlevi oluşturma

Bu makalede, HTTP isteklerine yanıt veren C# sınıf kitaplığı tabanlı bir işlev oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.
::: zone-end  
::: zone pivot="programming-language-javascript"
# <a name="quickstart-create-a-javascript-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlangıç: Azure 'da HTTP isteklerine yanıt veren bir JavaScript işlevi oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir JavaScript işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.
::: zone-end
::: zone pivot="programming-language-typescript"
# <a name="quickstart-create-a-typescript-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlangıç: Azure 'da HTTP isteklerine yanıt veren bir TypeScript işlevi oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir TypeScript işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.
::: zone-end   
::: zone pivot="programming-language-powershell"
# <a name="quickstart-create-a-powershell-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlangıç: Azure 'da HTTP isteklerine yanıt veren bir PowerShell işlevi oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir PowerShell işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.
::: zone-end  
::: zone pivot="programming-language-python" 
# <a name="quickstart-create-a-python-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlangıç: Azure 'da HTTP isteklerine yanıt veren bir Python işlevi oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir Python işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.
::: zone-end  
::: zone pivot="programming-language-java" 
# <a name="quickstart-create-a-java-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlangıç: Azure 'da HTTP isteklerine yanıt veren bir Java işlevi oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir Java işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.
::: zone-end

Bu makalede, HTTP isteklerine yanıt veren bir işlev oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
Bu makalenin [Visual Studio Code tabanlı bir sürümü](functions-create-first-function-vs-code.md) de vardır.
::: zone-end  

::: zone pivot="programming-language-java"  
> [!NOTE]
> Maven, tercih edilmemiş geliştirme aracınız değilse, [Gradle](/azure/azure-functions/functions-create-first-java-gradle), [IntelliJ fikir](/azure/developer/java/toolkit-for-intellij/quickstart-functions) ve [vs Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-java)kullanarak Java geliştiricileri için benzer öğreticilerimize göz atın.
::: zone-end  

[!INCLUDE [functions-requirements-cli](../../includes/functions-requirements-cli.md)]

[!INCLUDE [functions-cli-verify-prereqs](../../includes/functions-cli-verify-prereqs.md)]

[!INCLUDE [functions-cli-create-venv](../../includes/functions-cli-create-venv.md)]

## <a name="create-a-local-function-project"></a>Yerel işlev projesi oluşturma

Azure Işlevlerinde bir işlev projesi, her birinin belirli bir tetikleyiciye yanıt verdiği bir veya daha fazla bağımsız işlev için bir kapsayıcıdır. Projedeki tüm işlevler aynı yerel ve barındırma yapılandırmalarına sahiptir. Bu bölümde, tek bir işlev içeren bir işlev projesi oluşturursunuz.

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
Belirtilen çalışma `func init` zamanına sahip *localfunctionproj* adlı klasörde bir işlevler projesi oluşturmak için komutu aşağıdaki gibi çalıştırın:  
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

# <a name="bash"></a>[Bash](#tab/bash)
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

Maven, dağıtımda projenin oluşturulmasını tamamlaması için gereken değerleri ister.   
İstendiğinde aşağıdaki değerleri sağlayın:

| İstem | Değer | Açıklama |
| ------ | ----- | ----------- |
| **groupId** | `com.fabrikam` | Java için [paket adlandırma kurallarını](https://docs.oracle.com/javase/specs/jls/se6/html/packages.html#7.7) takip eden tüm projelerde projenizi benzersiz bir şekilde tanımlayan bir değer. |
| **ArtifactId** | `fabrikam-functions` | Bir sürüm numarası olmadan jar adı olan bir değer. |
| **Sürüm** | `1.0-SNAPSHOT` | Varsayılan değeri seçin. |
| **leyebilir** | `com.fabrikam.functions` | Oluşturulan işlev kodu için Java paketi olan bir değer. Varsayılan değeri kullanın. |

Onaylamak `Y` için yazın veya ENTER tuşuna basın.

Maven, proje dosyalarını, bu örnekte olduğu `fabrikam-functions`gibi, _ArtifactId_adında yeni bir klasörde oluşturur. 
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
Bu klasör, [yerel. Settings. JSON](functions-run-local.md#local-settings-file) ve [Host. JSON](functions-host-json.md)adlı yapılandırma dosyaları dahil olmak üzere, proje için çeşitli dosyaları içerir. *Local. Settings. JSON* , Azure 'dan indirilen gizli dizileri içerebildiğinden, dosya, *. gitignore* dosyasında varsayılan olarak kaynak denetiminden çıkarılır.

[!INCLUDE [functions-cli-add-function](../../includes/functions-cli-add-function.md)]

### <a name="optional-examine-the-file-contents"></a>Seçim Dosya içeriğini inceleyin

İsterseniz, [işlevi yerel olarak çalıştırmayı](#run-the-function-locally) atlayıp dosya içeriğini daha sonra incelemenizi sağlayabilirsiniz.

::: zone pivot="programming-language-csharp"
#### <a name="httpexamplecs"></a>HttpExample.cs

*HttpExample.cs* `req` , değişkende `Run` istek verilerini alan bir yöntem Içerir, tetikleme davranışını tanımlayan **httptriggerattribute**ile donatılmış bir [HttpRequest](/dotnet/api/microsoft.aspnetcore.http.httprequest) ' dir. 

:::code language="csharp" source="~/functions-docs-csharp/http-trigger-template/HttpExample.cs":::

Dönüş nesnesi, bir [Okobjectresult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200) veya [Badrequestobjectresult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestobjectresult) (400) olarak bir yanıt Iletisi döndüren bir [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) . Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](/azure/azure-functions/functions-bindings-http-webhook?tabs=csharp).
::: zone-end

::: zone pivot="programming-language-java"
#### <a name="functionjava"></a>Function. Java
*Function. Java* `request` , değişkende `run` istek verilerini alan bir yöntem içerir. Bu, tetikleme davranışını tanımlayan [Httptrigger](/java/api/com.microsoft.azure.functions.annotation.httptrigger) ek açıklaması ile donatılmış bir [HttpRequestMessage](/java/api/com.microsoft.azure.functions.httprequestmessage) 'dir. 

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java":::

Yanıt iletisi [HttpResponseMessage. Builder](/java/api/com.microsoft.azure.functions.httpresponsemessage.builder) API 'si tarafından oluşturulur.

#### <a name="pomxml"></a>Pod. xml

Uygulamanızı barındırmak için oluşturulan Azure kaynakları ayarları, oluşturulan Pod. xml dosyasında bir **GroupID** `com.microsoft.azure` 'si olan eklentisinin **yapılandırma** öğesinde tanımlanmıştır. Örneğin, aşağıdaki yapılandırma öğesi, bir Maven tabanlı dağıtıma `java-functions-group` `westus` bölge içindeki kaynak grubunda bir işlev uygulaması oluşturmasını söyler. İşlev uygulaması, `java-functions-app-service-plan` planda barındırılan Windows üzerinde çalışır; bu, varsayılan olarak sunucusuz bir tüketim plandır.    

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/pom.xml" range="116-155":::

Bu ayarları, kaynakların Azure 'da nasıl oluşturulduğunu denetlemek için, ilk dağıtımdan `runtime.os` `windows` `linux` önce ' dan önce ' ye geçiş yaparak yapabilirsiniz. Maven eklentisi tarafından desteklenen ayarların tam listesi için [yapılandırma ayrıntılarına](https://github.com/microsoft/azure-maven-plugins/wiki/Azure-Functions:-Configuration-Details)bakın.

#### <a name="functiontestjava"></a>FunctionTest. Java

Ayrıca, bu, işleviniz için bir birim testi de oluşturur. Bağlama eklemek veya projeye yeni işlevler eklemek için işlevinizi değiştirdiğinizde, *Functiontest. Java* dosyasındaki testleri de değiştirmeniz gerekir.
::: zone-end  
::: zone pivot="programming-language-python"
#### <a name="__init__py"></a>\_\_init\_\_. Kopyala

*\_\_\_init\_. Kopyala* , *function. JSON*içindeki yapılandırmaya göre tetiklenen bir `main()` Python işlevi içerir.

:::code language="python" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/__init__.py":::

Bir HTTP tetikleyicisi için işlev, değişkende `req` , *function. JSON*içinde tanımlanan şekilde istek verilerini alır. `req`, [Azure. Functions. HttpRequest sınıfının](/python/api/azure-functions/azure.functions.httprequest)bir örneğidir. `$return` *Function. JSON*içinde olarak tanımlanan Return nesnesi, [Azure. Functions. HttpResponse sınıfının](/python/api/azure-functions/azure.functions.httpresponse)bir örneğidir. Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](/azure/azure-functions/functions-bindings-http-webhook?tabs=python).
::: zone-end

::: zone pivot="programming-language-javascript"
#### <a name="indexjs"></a>index. js

*index. js* , *function. JSON*içindeki yapılandırmaya göre tetiklenen bir işlevi dışarı aktarır.

:::code language="javascript" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/index.js":::

Bir HTTP tetikleyicisi için işlev, değişkende `req` , *function. JSON*içinde tanımlanan şekilde istek verilerini alır. `$return` *Function. JSON*içinde olarak tanımlanan Return nesnesi yanıt olur. Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](/azure/azure-functions/functions-bindings-http-webhook?tabs=javascript).
::: zone-end

::: zone pivot="programming-language-typescript"
#### <a name="indexts"></a>Dizin. TS

*Index. TS* , *function. JSON*içindeki yapılandırmaya göre tetiklenen bir işlevi dışarı aktarır.

:::code language="typescript" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-TypeScript/index.ts":::

Bir HTTP tetikleyicisi için işlev, istek verilerini `req` **HttpRequest** türündeki değişkende, *function. JSON*içinde tanımlandığı şekilde alır. `$return` *Function. JSON*içinde olarak tanımlanan Return nesnesi yanıt olur. 
::: zone-end

::: zone pivot="programming-language-powershell"
#### <a name="runps1"></a>Run. ps1

*Run. ps1* , *function. JSON*içindeki yapılandırmaya göre tetiklenen bir işlev betiğini tanımlar.

:::code language="powershell" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/run.ps1":::

Bir HTTP tetikleyicisi için işlev, `$Request` *function. JSON*içinde tanımlanan param öğesine geçirilen istek verilerini alır. `Response` *Function. JSON*içinde olarak tanımlanan Return nesnesi, yanıt olarak `Push-OutputBinding` cmdlet 'ine geçirilir. 
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
#### <a name="functionjson"></a>function.json

*function. JSON* , tetikleyici türü de dahil olmak üzere işlevin giriş ve `bindings` çıkışını tanımlayan bir yapılandırma dosyasıdır. 
::: zone-end

::: zone pivot="programming-language-python"
İsterseniz farklı bir `scriptFile` Python dosyası çağırmak için ' i değiştirebilirsiniz.

:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json":::
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json":::
::: zone-end

::: zone pivot="programming-language-powershell"
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json":::
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"  
Her bağlama bir yön, tür ve benzersiz bir ad gerektirir. HTTP tetikleyicisinin tür [`httpTrigger`](functions-bindings-http-webhook-trigger.md) ve çıkış bağlaması türünde bir giriş bağlaması vardır [`http`](functions-bindings-http-webhook-output.md).
::: zone-end  

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell,programming-language-csharp"    
## <a name="create-supporting-azure-resources-for-your-function"></a>İşleviniz için destekleyici Azure kaynakları oluşturma

İşlev kodunuzu Azure 'a dağıtabilmeniz için önce üç kaynak oluşturmanız gerekir:

- İlgili kaynaklar için mantıksal kapsayıcı olan bir kaynak grubu.
- Projelerinizle ilgili durumu ve diğer bilgileri tutan bir depolama hesabı.
- İşlev kodunuzu yürütmek için ortam sağlayan bir işlev uygulaması. Bir işlev uygulaması yerel işlev projenize eşlenir ve kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanızı sağlar.

Bu öğeleri oluşturmak için aşağıdaki Azure CLı komutlarını kullanın. Her komut, tamamlandıktan sonra JSON çıktısı sağlar.

Şimdiye kadar yapmadıysanız, [az Login](/cli/azure/reference-index#az-login) komutuyla Azure 'da oturum açın:

```azurecli
az login
```
    
[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, `AzureFunctionsQuickstart-rg` `westeurope` bölgesinde adlı bir kaynak grubu oluşturur. (Bir bölgede kullanılabilir bir bölge `az account list-locations` kullanarak, genellikle kaynak grubunuzu ve kaynaklarınızı size yakın bir bölgede oluşturursunuz.)

```azurecli
az group create --name AzureFunctionsQuickstart-rg --location westeurope
```

> [!NOTE]
> Linux ve Windows uygulamalarını aynı kaynak grubunda barındıralamazsınız. Bir Windows işlev uygulaması veya Web uygulaması ile `AzureFunctionsQuickstart-rg` adlandırılmış bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.
 
    
[Az Storage Account Create](/cli/azure/storage/account#az-storage-account-create) komutunu kullanarak kaynak grubunuzda ve bölgenizde genel amaçlı bir depolama hesabı oluşturun. Aşağıdaki örnekte, öğesini sizin için `<STORAGE_NAME>` uygun olan bir genel benzersiz adla değiştirin. Adlar yalnızca üç ile 24 karakter arasında ve küçük harflerden oluşmalıdır. `Standard_LRS`[işlevleri tarafından desteklenen](storage-considerations.md#storage-account-requirements), genel amaçlı bir hesabı belirtir.

```azurecli
az storage account create --name <STORAGE_NAME> --location westeurope --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
```

Depolama hesabı bu hızlı başlangıç için yalnızca birkaç ilay (USD) doğurur.
    
[Az functionapp Create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak işlev uygulamasını oluşturun. Aşağıdaki örnekte, değerini, önceki `<STORAGE_NAME>` adımda kullandığınız hesabın adıyla değiştirin ve öğesini sizin için uygun olan bir genel benzersiz adla `<APP_NAME>` değiştirin. `<APP_NAME>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. 
::: zone-end  

::: zone pivot="programming-language-python"  
Python `--runtime-version` 3,8 kullanıyorsanız, `3.8` ve `--functions_version` ile değiştirin. `3`

Python 3,6 kullanıyorsanız, olarak `--runtime-version` `3.6`değiştirin.

```azurecli
az functionapp create --resource-group AzureFunctionsQuickstart-rg --os-type Linux --consumption-plan-location westeurope --runtime python --runtime-version 3.7 --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
```
::: zone-end  

::: zone pivot="programming-language-javascript,programming-language-typescript"  
Node. js 8 kullanıyorsanız, olarak `--runtime-version` `8`da değiştirin.


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
Bu komut, burada yaptığınız kullanım miktarı için ücretsiz olan [Azure Işlevleri tüketim planı](functions-scale.md#consumption-plan)altında, belirtilen dil çalışma zamanında çalışan bir işlev uygulaması oluşturur. Bu komut Ayrıca, aynı kaynak grubunda, işlev uygulamanızı izleyebilmeniz ve günlükleri görüntüleyebileceğiniz ilişkili bir Azure Application Insights örneğini sağlar. Daha fazla bilgi için bkz. [Azure Işlevlerini izleme](functions-monitoring.md). Örnek, siz etkinleştirene kadar hiçbir maliyet vermez.
    
## <a name="deploy-the-function-project-to-azure"></a>İşlev projesini Azure 'a dağıtma
::: zone-end  

::: zone pivot="programming-language-typescript"  
Projenizi Azure 'a dağıtmak için temel araçları kullanmadan önce, TypeScript kaynak dosyalarından bir dizi JavaScript dosyası oluşturun.

Aşağıdaki komut, TypeScript projenizi dağıtım için hazırlar:

```
npm run build:production 
```
::: zone-end  

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell,programming-language-csharp"  
Gerekli kaynaklarla, artık yerel işlevler projenizi, [Func Azure functionapp Publish](functions-run-local.md#project-file-deployment) komutunu kullanarak Azure 'daki işlev uygulamasına dağıtmaya hazırsınız demektir. Aşağıdaki örnekte, değerini uygulamanızın adıyla `<APP_NAME>` değiştirin.

```
func azure functionapp publish <APP_NAME>
```

"Adı olan uygulama bulunamıyor..." hatasını görürseniz, birkaç saniye bekleyip yeniden deneyin. Bu işlem, Azure önceki `az functionapp create` komuttan sonra uygulamayı tam olarak başlatılamayabilir.

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

::: zone-end  
::: zone pivot="programming-language-java"  
## <a name="deploy-the-function-project-to-azure"></a>İşlev projesini Azure 'a dağıtma

İşlevler projenizi ilk kez dağıttığınızda Azure 'da bir işlev uygulaması ve ilgili kaynaklar oluşturulur. Uygulamanızı barındırmak üzere oluşturulan Azure kaynakları için ayarlar [Pod. xml dosyasında](#pomxml)tanımlanmıştır. Bu makalede, Varsayılanları kabul edersiniz.

> [!TIP]
> Windows yerine Linux üzerinde çalışan bir işlev uygulaması oluşturmak için, Pok. `runtime.os` XML dosyasındaki öğesini `windows` olarak `linux`değiştirin. [Bu bölgelerde](https://github.com/Azure/azure-functions-host/wiki/Linux-Consumption-Regions)Linux çalıştırmak bir tüketim planında desteklenir. Linux ve uygulamalar üzerinde çalışan ve aynı kaynak grubunda Windows üzerinde çalışan uygulamalar olamaz.

Dağıtmadan önce, Azure aboneliğinizde oturum açmak için [az Login](/cli/azure/authenticate-azure-cli) Azure CLI komutunu kullanın. 

```azurecli
az login
```

Projenizi yeni bir işlev uygulamasına dağıtmak için aşağıdaki komutu kullanın. 

```
mvn azure-functions:deploy
```

Bu, Azure 'da aşağıdaki kaynakları oluşturur:

+ Kaynak grubu. _Java-Functions-Group_olarak adlandırılır.
+ Depolama hesabı. Işlevleri için gereklidir. Ad, depolama hesabı adı gereksinimlerine göre rastgele oluşturulur.
+ Barındırma planı. _Westus_ bölgesindeki işlev uygulamanız için sunucusuz barındırma. Ad, _Java-Functions-App-Service-plan_' dır.
+ İşlev uygulaması. İşlev uygulaması, işlevleriniz için dağıtım ve yürütme birimidir. Ad, rastgele oluşturulmuş bir sayı eklenerek, sizin _ArtifactId_sizin için rastgele oluşturulur. 

Dağıtım proje dosyalarını paketler ve bunları [ZIP dağıtımı](functions-deployment-technologies.md#zip-deploy)kullanarak yeni işlev uygulamasına dağıtır. Kod, Azure 'daki dağıtım paketinden çalışır.
::: zone-end

## <a name="invoke-the-function-on-azure"></a>Azure 'da işlevi çağırma

İşleviniz bir HTTP tetikleyicisi kullandığından, tarayıcıda URL 'sine veya kıvrıcı gibi bir araçla HTTP isteği yaparak bu uygulamayı çağırabilirsiniz. Her iki örnekte de, `code` URL parametresi, işlev uç noktanızın çağrılmasını yetkilendiren benzersiz [işleviniz anahtarıdır](functions-bindings-http-webhook-trigger.md#authorization-keys) .

# <a name="browser"></a>[Tarayıcı](#tab/browser)

Yayımla komutunun çıktısında gösterilen tüm **ÇAĞıRMA URL** 'sini, sorgu parametresini `&name=Functions`ekleyerek bir tarayıcı adres çubuğuna kopyalayın. , İşlevi yerel olarak çalıştırdığınızda tarayıcı benzer bir çıktı görüntülemelidir.

![Azure üzerinde çalışan işlevin çıktısı bir tarayıcıda](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-browser.png)


# <a name="curl"></a>[kıvr](#tab/curl)

Parametresini [`curl`](https://curl.haxx.se/) `&name=Functions`ekleyerek **Invoke URL 'si**ile çalıştırın. Komutun çıktısı, "Hello Functions" metni olmalıdır.

![İşlevin çıktısı, Azure 'da kıvrımlı kullanarak çalıştırıldı](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-curl.png)

---

> [!TIP]
> Yayınlanan işlev uygulaması için neredeyse gerçek zamanlı günlükleri görüntülemek için [Application Insights canlı ölçüm akışı](functions-monitoring.md#streaming-logs)kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki adıma devam ederseniz, [bir Azure depolama kuyruğu çıkış bağlaması ekleyin](functions-add-output-binding-storage-queue-cli.md), zaten yaptığınız şekilde oluşturacağınız sürece tüm kaynaklarınızı yerinde tutun.

Aksi takdirde, daha fazla maliyet ödemeden kaçınmak için kaynak grubunu ve içerdiği tüm kaynakları silmek için aşağıdaki komutu kullanın.

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell,programming-language-csharp" 
```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```
::: zone-end
::: zone pivot="programming-language-java"
```azurecli
az group delete --name java-functions-group
```
::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure depolama kuyruğuna bağlanma](functions-add-output-binding-storage-queue-cli.md)
