---
title: Azure'da HTTP isteklerine yanıt veren işlev oluşturma
description: Komut satırından bir işlev oluşturmayı ve ardından yerel projeyi Azure Işlevleri 'nde sunucusuz barındırma için yayımlamayı öğrenin.
ms.date: 03/30/2020
ms.topic: quickstart
ms.custom: devx-track-csharp, devx-track-python, devx-track-azurecli, devx-track-azurepowershell
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: b299f0bb13bb25fbc192f3d117be11ca1ce26586
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89145561"
---
# <a name="quickstart-create-a-function-in-azure-that-responds-to-http-requests"></a>Hızlı başlangıç: Azure 'da HTTP isteklerine yanıt veren bir işlev oluşturma

::: zone pivot="programming-language-csharp"  
Bu makalede, HTTP isteklerine yanıt veren C# sınıf kitaplığı tabanlı bir işlev oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end  
::: zone pivot="programming-language-javascript"
Bu makalede, HTTP isteklerine yanıt veren bir JavaScript işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end
::: zone pivot="programming-language-typescript"
Bu makalede, HTTP isteklerine yanıt veren bir TypeScript işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end   
::: zone pivot="programming-language-powershell"
Bu makalede, HTTP isteklerine yanıt veren bir PowerShell işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end  
::: zone pivot="programming-language-python" 
Bu makalede, HTTP isteklerine yanıt veren bir Python işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end  
::: zone pivot="programming-language-java" 
Bu makalede, HTTP isteklerine yanıt veren bir Java işlevi oluşturmak için komut satırı araçlarını kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end

Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
Bu makalenin [Visual Studio Code tabanlı bir sürümü](functions-create-first-function-vs-code.md) de vardır.
::: zone-end  
::: zone pivot="programming-language-java"  
> [!NOTE]
> Maven, tercih edilmemiş geliştirme aracınız değilse, [Gradle](./functions-create-first-java-gradle.md), [IntelliJ fikir](/azure/developer/java/toolkit-for-intellij/quickstart-functions) ve [Visual Studio Code](./functions-create-first-function-vs-code.md?pivots=programming-language-java)kullanarak Java geliştiricileri için benzer öğreticilerimize göz atın.
::: zone-end  

[!INCLUDE [functions-requirements-cli](../../includes/functions-requirements-cli.md)]

[!INCLUDE [functions-cli-verify-prereqs](../../includes/functions-cli-verify-prereqs.md)]

[!INCLUDE [functions-cli-create-venv](../../includes/functions-cli-create-venv.md)]

## <a name="create-a-local-function-project"></a>Yerel işlev projesi oluşturma

Azure Işlevlerinde bir işlev projesi, her birinin belirli bir tetikleyiciye yanıt verdiği bir veya daha fazla bağımsız işlev için bir kapsayıcıdır. Projedeki tüm işlevler aynı yerel ve barındırma yapılandırmalarına sahiptir. Bu bölümde, tek bir işlev içeren bir işlev projesi oluşturursunuz.

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
`func init`Belirtilen çalışma zamanına sahip *Localfunctionproj* adlı klasörde bir işlevler projesi oluşturmak için komutu aşağıdaki gibi çalıştırın:  
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
İşlevler projesini bir [Maven arketipinden](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) oluşturmak için boş bir klasörde aşağıdaki komutu çalıştırın. `-DjavaVersion=11`Işlevlerinizi Java 11 ' de çalıştırmak istiyorsanız kullanın. Daha fazla bilgi için bkz. [Java sürümleri](functions-reference-java.md#java-versions). 

# <a name="bash"></a>[Bash](#tab/bash)
```bash
mvn archetype:generate -DarchetypeGroupId=com.microsoft.azure -DarchetypeArtifactId=azure-functions-archetype -DjavaVersion=8
```
# <a name="powershell"></a>[PowerShell](#tab/powershell)
```powershell
mvn archetype:generate "-DarchetypeGroupId=com.microsoft.azure" "-DarchetypeArtifactId=azure-functions-archetype" "-DjavaVersion=8" 
```
# <a name="cmd"></a>[Cmd](#tab/cmd)
```cmd
mvn archetype:generate "-DarchetypeGroupId=com.microsoft.azure" "-DarchetypeArtifactId=azure-functions-archetype" "-DjavaVersion=8"
```
---

Maven, dağıtımda projenin oluşturulmasını tamamlaması için gereken değerleri ister.   
İstendiğinde aşağıdaki değerleri sağlayın:

| İstem | Değer | Açıklama |
| ------ | ----- | ----------- |
| **groupId** | `com.fabrikam` | Java için [paket adlandırma kurallarını](https://docs.oracle.com/javase/specs/jls/se6/html/packages.html#7.7) takip eden tüm projelerde projenizi benzersiz bir şekilde tanımlayan bir değer. |
| **ArtifactId** | `fabrikam-functions` | Bir sürüm numarası olmadan jar adı olan bir değer. |
| **Sürüm** | `1.0-SNAPSHOT` | Varsayılan değeri seçin. |
| **leyebilir** | `com.fabrikam` | Oluşturulan işlev kodu için Java paketi olan bir değer. Varsayılan değeri kullanın. |

`Y`Onaylamak için yazın veya ENTER tuşuna basın.

Maven, proje dosyalarını, bu örnekte olduğu gibi, _ArtifactId_adında yeni bir klasörde oluşturur `fabrikam-functions` . 

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
Bu klasör, [local.settings.js](functions-run-local.md#local-settings-file) ve [host.js](functions-host-json.md)adlı yapılandırma dosyaları da dahil olmak üzere, proje için çeşitli dosyalar içerir. *local.settings.json* , Azure 'dan indirilen gizli dizileri içerebildiğinden, dosya, *. gitignore* dosyasında varsayılan olarak kaynak denetiminden çıkarılır.

[!INCLUDE [functions-cli-add-function](../../includes/functions-cli-add-function.md)]

### <a name="optional-examine-the-file-contents"></a>Seçim Dosya içeriğini inceleyin

İsterseniz, [işlevi yerel olarak çalıştırmayı](#run-the-function-locally) atlayıp dosya içeriğini daha sonra incelemenizi sağlayabilirsiniz.

::: zone pivot="programming-language-csharp"
#### <a name="httpexamplecs"></a>HttpExample.cs

*HttpExample.cs* , `Run` değişkende istek verilerini alan bir yöntem içerir `req` , tetikleme davranışını tanımlayan **Httptriggerattribute**ile donatılmış bir [HttpRequest](/dotnet/api/microsoft.aspnetcore.http.httprequest) ' dir. 

:::code language="csharp" source="~/functions-docs-csharp/http-trigger-template/HttpExample.cs":::

Dönüş nesnesi, bir [Okobjectresult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200) veya [Badrequestobjectresult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestobjectresult) (400) olarak bir yanıt Iletisi döndüren bir [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) . Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](./functions-bindings-http-webhook.md?tabs=csharp).
::: zone-end

::: zone pivot="programming-language-java"
#### <a name="functionjava"></a>Function. Java
*Function. Java* , `run` değişkende istek verilerini alan bir yöntem içerir `request` . Bu, tetikleme davranışını tanımlayan [httptrigger](/java/api/com.microsoft.azure.functions.annotation.httptrigger) ek açıklaması ile donatılmış bir [HttpRequestMessage](/java/api/com.microsoft.azure.functions.httprequestmessage) 'dir. 

:::code language="java" source="~/azure-functions-samples-java/src/main/java/com/functions/Function.java":::

Yanıt iletisi [HttpResponseMessage. Builder](/java/api/com.microsoft.azure.functions.httpresponsemessage.builder) API 'si tarafından oluşturulur.

#### <a name="pomxml"></a>pom.xml

Uygulamanızı barındırmak üzere oluşturulan Azure kaynakları için ayarlar, eklentinin **yapılandırma** öğesinde oluşturulan pom.xml dosyasında bir **GroupID** ile tanımlanmıştır `com.microsoft.azure` . Örneğin, aşağıdaki yapılandırma öğesi, bir Maven tabanlı dağıtıma `java-functions-group` bölge içindeki kaynak grubunda bir işlev uygulaması oluşturmasını söyler `westus` . İşlev uygulaması, planda barındırılan Windows üzerinde çalışır `java-functions-app-service-plan` ; Bu, varsayılan olarak sunucusuz bir tüketim plandır.    

:::code language="java" source="~/azure-functions-samples-java/pom.xml" range="62-102":::

Bu ayarları, kaynakların Azure 'da nasıl oluşturulduğunu denetlemek için, `runtime.os` `windows` ilk dağıtımdan önce ' dan önce ' ye geçiş yaparak yapabilirsiniz `linux` . Maven eklentisi tarafından desteklenen ayarların tam listesi için [yapılandırma ayrıntılarına](https://github.com/microsoft/azure-maven-plugins/wiki/Azure-Functions:-Configuration-Details)bakın.

İşlev uygulamanızı Java 8 yerine Java 11 ' de çalıştırmak istiyorsanız, pom.xml dosyasını Java 11 değerleriyle el ile güncelleştirmeniz gerekir. Daha fazla bilgi için bkz. [Java sürümleri](functions-reference-java.md#java-versions). Java 11 ' de çalışırken emin olun  

#### <a name="functiontestjava"></a>FunctionTest. Java

Ayrıca, bu, işleviniz için bir birim testi de oluşturur. Bağlama eklemek veya projeye yeni işlevler eklemek için işlevinizi değiştirdiğinizde, *Functiontest. Java* dosyasındaki testleri de değiştirmeniz gerekir.
::: zone-end  
::: zone pivot="programming-language-python"
#### <a name="__init__py"></a>\_\_init \_ \_ . Kopyala

* \_ \_ init \_ \_ . Kopyala* `main()` , *function.jsüzerindeki*yapılandırmaya göre tetiklenen bir Python işlevi içerir.

:::code language="python" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/__init__.py":::

Bir HTTP tetikleyicisi için işlev, `req` *function.json*öğesinde tanımlanan şekilde değişkende istek verilerini alır. `req` , [Azure. Functions. HttpRequest sınıfının](/python/api/azure-functions/azure.functions.httprequest)bir örneğidir. function.jsolarak tanımlanan dönüş nesnesi, `$return` [Azure. Functions. HttpResponse sınıfının](/python/api/azure-functions/azure.functions.httpresponse)bir örneğidir. *function.json* Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](./functions-bindings-http-webhook.md?tabs=python).
::: zone-end

::: zone pivot="programming-language-javascript"
#### <a name="indexjs"></a>index.js

*index.js* , *function.jsüzerindeki*yapılandırmaya göre tetiklenen bir işlevi dışarı aktarır.

:::code language="javascript" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/index.js":::

Bir HTTP tetikleyicisi için işlev, `req` *function.json*öğesinde tanımlanan şekilde değişkende istek verilerini alır. function.jsüzerinde olarak tanımlanan Return nesnesi `$return` Yanıt *function.json*olur. Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](./functions-bindings-http-webhook.md?tabs=javascript).
::: zone-end

::: zone pivot="programming-language-typescript"
#### <a name="indexts"></a>Dizin. TS

*index. TS* , *function.jsüzerindeki*yapılandırmaya göre tetiklenen bir işlevi dışarı aktarır.

:::code language="typescript" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-TypeScript/index.ts":::

Bir HTTP tetikleyicisi için işlev, `req` *function.jsüzerinde*tanımlanan **HttpRequest** türünde değişkende istek verilerini alır. function.jsüzerinde olarak tanımlanan Return nesnesi `$return` Yanıt *function.json*olur. 
::: zone-end

::: zone pivot="programming-language-powershell"
#### <a name="runps1"></a>run.ps1

*run.ps1* , *function.jsüzerindeki*yapılandırmaya göre tetiklenen bir işlev betiği tanımlar.

:::code language="powershell" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/run.ps1":::

Bir HTTP tetikleyicisi için işlev, `$Request` *function.jsüzerinde*tanımlanan param öğesine geçirilen istek verilerini alır. function.jsüzerinde olduğu gibi tanımlanan dönüş nesnesi, `Response` *function.json* `Push-OutputBinding` cmdlet 'e yanıt olarak geçirilir. 
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
#### <a name="functionjson"></a>function.json

*function.js* , `bindings` tetikleyici türü de dahil olmak üzere işlevin giriş ve çıkışını tanımlayan bir yapılandırma dosyasıdır. 
::: zone-end

::: zone pivot="programming-language-python"
İsterseniz `scriptFile` farklı bir Python dosyası çağırmak için ' i değiştirebilirsiniz.

:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json":::
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json":::
::: zone-end

::: zone pivot="programming-language-powershell"
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json":::
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"  
Her bağlama bir yön, tür ve benzersiz bir ad gerektirir. HTTP tetikleyicisinin tür ve çıkış bağlaması türünde bir giriş bağlaması vardır [`httpTrigger`](functions-bindings-http-webhook-trigger.md) [`http`](functions-bindings-http-webhook-output.md) .
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
    
[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, bölgesinde adlı bir kaynak grubu oluşturur `AzureFunctionsQuickstart-rg` `westeurope` . (Bir bölgede kullanılabilir bir bölge kullanarak, genellikle kaynak grubunuzu ve kaynaklarınızı size yakın bir bölgede oluşturursunuz `az account list-locations` .)

```azurecli
az group create --name AzureFunctionsQuickstart-rg --location westeurope
```

> [!NOTE]
> Linux ve Windows uygulamalarını aynı kaynak grubunda barındıralamazsınız. `AzureFunctionsQuickstart-rg`Bir Windows işlev uygulaması veya Web uygulaması ile adlandırılmış bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.
 
    
[Az Storage Account Create](/cli/azure/storage/account#az-storage-account-create) komutunu kullanarak kaynak grubunuzda ve bölgenizde genel amaçlı bir depolama hesabı oluşturun. Aşağıdaki örnekte, öğesini `<STORAGE_NAME>` sizin için uygun olan bir genel benzersiz adla değiştirin. Adlar yalnızca üç ile 24 karakter arasında ve küçük harflerden oluşmalıdır. `Standard_LRS`[işlevleri tarafından desteklenen](storage-considerations.md#storage-account-requirements), genel amaçlı bir hesabı belirtir.

```azurecli
az storage account create --name <STORAGE_NAME> --location westeurope --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
```

Depolama hesabı bu hızlı başlangıç için yalnızca birkaç ilay (USD) doğurur.
    
[Az functionapp Create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak işlev uygulamasını oluşturun. Aşağıdaki örnekte, değerini, `<STORAGE_NAME>` önceki adımda kullandığınız hesabın adıyla değiştirin ve öğesini `<APP_NAME>` sizin için uygun olan bir genel benzersiz adla değiştirin. `<APP_NAME>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. 
::: zone-end  

::: zone pivot="programming-language-python"  
Python 3,8 kullanıyorsanız, ve ile değiştirin `--runtime-version` `3.8` `--functions_version` `3` .

Python 3,6 kullanıyorsanız, `--runtime-version` olarak değiştirin `3.6` .

```azurecli
az functionapp create --resource-group AzureFunctionsQuickstart-rg --os-type Linux --consumption-plan-location westeurope --runtime python --runtime-version 3.7 --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
```
::: zone-end  

::: zone pivot="programming-language-javascript,programming-language-typescript"  
Node.js 8 kullanıyorsanız, olarak da değiştirin `--runtime-version` `8` .


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
Gerekli kaynaklarla, artık yerel işlevler projenizi, [Func Azure functionapp Publish](functions-run-local.md#project-file-deployment) komutunu kullanarak Azure 'daki işlev uygulamasına dağıtmaya hazırsınız demektir. Aşağıdaki örnekte, değerini `<APP_NAME>` uygulamanızın adıyla değiştirin.

```
func azure functionapp publish <APP_NAME>
```

"Adı olan uygulama bulunamıyor..." hatasını görürseniz, birkaç saniye bekleyip yeniden deneyin. Bu işlem, Azure önceki komuttan sonra uygulamayı tam olarak başlatılamayabilir `az functionapp create` .

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

İşlevler projenizi ilk kez dağıttığınızda Azure 'da bir işlev uygulaması ve ilgili kaynaklar oluşturulur. Uygulamanızı barındırmak üzere oluşturulan Azure kaynakları için ayarlar [pom.xml dosyasında](#pomxml)tanımlanmıştır. Bu makalede, Varsayılanları kabul edersiniz.

> [!TIP]
> Windows yerine Linux üzerinde çalışan bir işlev uygulaması oluşturmak için, `runtime.os` pom.xml dosyasındaki öğesini `windows` olarak değiştirin `linux` . [Bu bölgelerde](https://github.com/Azure/azure-functions-host/wiki/Linux-Consumption-Regions)Linux çalıştırmak bir tüketim planında desteklenir. Linux ve uygulamalar üzerinde çalışan ve aynı kaynak grubunda Windows üzerinde çalışan uygulamalar olamaz.

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
+ İşlev uygulaması. İşlev uygulaması, işlevleriniz için dağıtım ve yürütme birimidir. Ad, rastgele oluşturulmuş bir sayı eklenerek, _ArtifactId_temel alınarak rastgele oluşturulur. 

Dağıtım proje dosyalarını paketler ve bunları [ZIP dağıtımı](functions-deployment-technologies.md#zip-deploy)kullanarak yeni işlev uygulamasına dağıtır. Kod, Azure 'daki dağıtım paketinden çalışır.
::: zone-end

## <a name="invoke-the-function-on-azure"></a>Azure 'da işlevi çağırma

İşleviniz bir HTTP tetikleyicisi kullandığından, tarayıcıda URL 'sine veya kıvrıcı gibi bir araçla HTTP isteği yaparak bu uygulamayı çağırabilirsiniz. Her iki örnekte de, `code` URL parametresi, işlev uç noktanızın çağrılmasını yetkilendiren benzersiz [işleviniz anahtarıdır](functions-bindings-http-webhook-trigger.md#authorization-keys) .

# <a name="browser"></a>[Tarayıcı](#tab/browser)

Yayımla komutunun çıktısında gösterilen tüm **ÇAĞıRMA URL** 'sini, sorgu parametresini ekleyerek bir tarayıcı adres çubuğuna kopyalayın `&name=Functions` . , İşlevi yerel olarak çalıştırdığınızda tarayıcı benzer bir çıktı görüntülemelidir.

![Azure üzerinde çalışan işlevin çıktısı bir tarayıcıda](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-browser.png)


# <a name="curl"></a>[kıvr](#tab/curl)

[`curl`](https://curl.haxx.se/)Parametresini ekleyerek **Invoke URL 'si**ile çalıştırın `&name=Functions` . Komutun çıktısı, "Hello Functions" metni olmalıdır.

![İşlevin çıktısı, Azure 'da kıvrımlı kullanarak çalıştırıldı](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-curl.png)

---

> [!TIP]
> Yayınlanan işlev uygulaması için neredeyse gerçek zamanlı günlükleri görüntülemek için [Application Insights canlı ölçüm akışı](functions-monitoring.md#streaming-logs)kullanın.
>
> Canlı ölçümler akışını bir tarayıcıda açmak için aşağıdaki komutu çalıştırın.
>   ```
>   func azure functionapp logstream <APP_NAME> --browser
>   ```

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
::: zone pivot="programming-language-python"
Sanal ortamdan çıkmak için öğesini çalıştırın `deactivate` .
::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure depolama kuyruğuna bağlanma](functions-add-output-binding-storage-queue-cli.md)
 
