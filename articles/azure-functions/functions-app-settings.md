---
title: Azure işlevleri için uygulama ayarları başvurusu
description: Azure işlevleri uygulama ayarları veya ortam değişkenleri için başvuru belgeleri.
ms.topic: conceptual
ms.date: 09/22/2018
ms.openlocfilehash: 3853ccbfd492bfaf4a82d62e6d31ab938285ee2e
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79277836"
---
# <a name="app-settings-reference-for-azure-functions"></a>Azure işlevleri için uygulama ayarları başvurusu

Uygulama ayarlarında, bir işlev uygulaması, işlev uygulaması için tüm işlevleri etkiler genel yapılandırma seçenekleri içerir. Yerel olarak çalıştırdığınızda, bu ayarlara yerel [ortam değişkenleri](functions-run-local.md#local-settings-file)olarak erişilir. Bu makalede, işlev uygulamalarında kullanılabilir uygulama ayarlarını listeler.

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

[Host. JSON](functions-host-json.md) dosyasında ve [Local. Settings. JSON](functions-run-local.md#local-settings-file) dosyasında başka genel yapılandırma seçenekleri vardır.

## <a name="appinsights_instrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Application Insights kullanıyorsanız, Application Insights izleme anahtarı. Bkz. [Azure Işlevlerini izleme](functions-monitoring.md).

|Anahtar|Örnek değer|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azure_functions_environment"></a>AZURE_FUNCTIONS_ENVIRONMENT

2\. x ve sonraki Işlevler çalışma zamanının sürümlerinde, çalışma zamanı ortamı temelinde uygulama davranışını yapılandırır. Bu değer [başlatma sırasında okundu](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/Program.cs#L43). `AZURE_FUNCTIONS_ENVIRONMENT` herhangi bir değere ayarlayabilirsiniz, ancak [üç değer](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) desteklenir: [geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [hazırlık](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging)ve [Üretim](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). `AZURE_FUNCTIONS_ENVIRONMENT` ayarlanmamışsa, varsayılan olarak yerel bir ortamda `Development` ve Azure üzerinde `Production`. Çalışma zamanı ortamını ayarlamak için bu ayar `ASPNETCORE_ENVIRONMENT` yerine kullanılmalıdır. 

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Günlükleri depolamak ve portaldaki **izleyici** sekmesinde görüntülemek için isteğe bağlı depolama hesabı bağlantı dizesi. Bu ayar yalnızca Azure Işlevleri çalışma zamanının 1. x sürümünü hedefleyen uygulamalar için geçerlidir. Bloblar, kuyruklar ve tablolar destekleyen genel amaçlı bir depolama hesabı olmalıdır. Daha fazla bilgi için bkz. [depolama hesabı gereksinimleri](storage-considerations.md#storage-account-requirements).

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol = https; AccountName =<name>; AccountKey =<key>|

> [!NOTE]
> Daha iyi performans ve deneyim için, çalışma zamanı sürüm 2. x ve sonraki sürümleri, `AzureWebJobsDashboard`yerine izleme için APPINSIGHTS_INSTRUMENTATIONKEY ve uygulama öngörülerini kullanır.

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true`, bir işlev uygulamasının kök URL 'SI için gösterilen varsayılan giriş sayfasını devre dışı bırakma anlamına gelir. `false` varsayılan değerdir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDisableHomepage|true|

Bu uygulama ayarı atlandığında veya `false`olarak ayarlandığında, URL `<functionappname>.azurewebsites.net`yanıt olarak aşağıdaki örneğe benzer bir sayfa görüntülenir.

![İşlevi uygulama giriş sayfası](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true`, .NET kodu derlerken yayın modunu kullanmak anlamına gelir; `false`, hata ayıklama modunu kullanma anlamına gelir. `true` varsayılan değerdir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Beta özellikleri etkinleştirmek için virgülle ayrılmış listesi. Bu bayraklar tarafından beta özellikleriyle, üretime hazır değildir, ancak bunlar kullanıma sunulmadan önce Deneysel kullanımı için etkinleştirilebilir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsFeatureFlags|özellik1, Özellik2|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Depo veya için anahtar depolama sağlayıcı belirtir. Şu anda desteklenen depoları blob depolama ("Blob") olan ve yerel dosya sistemi ("Files"). Varsayılan sürüm 2'deki blob ve dosya sistemi sürüm 1'dir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsSecretStorageType|Dosyalar|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

Azure işlevleri çalışma zamanı HTTP tetiklemeli işlevleri hariç tüm işlevler için bu depolama hesabı bağlantı dizesi kullanır. Bloblar, kuyruklar ve tablolar destekleyen genel amaçlı bir depolama hesabı olmalıdır. Bkz. [depolama hesabı](functions-infrastructure-as-code.md#storage-account) ve [depolama hesabı gereksinimleri](storage-considerations.md#storage-account-requirements).

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [anahtar]|

## <a name="azurewebjobs_typescriptpath"></a>AzureWebJobs_TypeScriptPath

TypeScript için kullanılan derleme yolu. Gerekirse varsayılan geçersiz kılmanıza da olanak sağlar.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobs_TypeScriptPath|%Home%\typescript|

## <a name="function_app_edit_mode"></a>Işlev\_APP\_düzenleme\_modu

Azure portal düzenlemenin etkinleştirilip etkinleştirilmeyeceğini belirler. Geçerli değerler şunlardır: "readwrite" ve "salt okunur".

|Anahtar|Örnek değer|
|---|------------|
|Işlev\_APP\_düzenleme\_modu|salt okunur|

## <a name="functions_extension_version"></a>\_UZANTı\_sürümü IŞLEVLERI

Bu işlev uygulamasında kullanmak için İşlevler çalışma zamanı sürümü. Bir tilde ana sürümle (örneğin, "~ 2") bu ana sürüm en son sürümünü kullanmanız anlamına gelir. Aynı ana sürüm için yeni sürümler kullanılabilir olduğunda işlev uygulamasına otomatik olarak yüklenirler. Belirli bir sürüme uygulamayı sabitlemek için tam sürüm numarası (örneğin, "2.0.12345") kullanın. "~ 2" varsayılandır. `~1` değeri, uygulamanızı çalışma zamanının 1. x sürümüne sabitler.

|Anahtar|Örnek değer|
|---|------------|
|\_UZANTı\_sürümü IŞLEVLERI|~ 2|

## <a name="functions_v2_compatibility_mode"></a>\_v2\_uyumluluk\_modu IŞLEVLERI

Bu ayar, işlev uygulamanızın sürüm 3. x çalışma zamanı üzerinde sürüm 2. x uyumlu modda çalışmasını sağlar. Bu ayarı yalnızca [, işlev uygulamanızı çalışma zamanının 2. x sürümünden 3. x sürümüne yükseltirken](functions-versions.md#migrating-from-2x-to-3x)sorunlarla karşılaşırsanız kullanın. 

>[!IMPORTANT]
> Bu ayar yalnızca, uygulamanızı 3. x sürümünde doğru şekilde çalışacak şekilde güncelleştirdiğinizde kısa süreli geçici bir çözüm olarak tasarlanmıştır. [2. x çalışma zamanının desteklendiği](functions-versions.md)sürece bu ayar desteklenir. Uygulamanızın bu ayarı kullanmadan sürüm 3. x üzerinde çalışmasını engelleyen sorunlarla karşılaşırsanız, lütfen [sorununuzu bildirin](https://github.com/Azure/azure-functions-host/issues/new?template=Bug_report.md).

[\_uzantı\_sürümünün](functions-app-settings.md#functions_extension_version) `~3`olarak ayarlanmasını gerektirir.

|Anahtar|Örnek değer|
|---|------------|
|\_v2\_uyumluluk\_modu IŞLEVLERI|true|

## <a name="functions_worker_process_count"></a>Işlemler\_çalışan\_Işlem\_sayısı

Varsayılan değeri `1`olan en fazla dil çalışan işlemi sayısını belirtir. İzin verilen en büyük değer `10`. İşlev etkinleştirmeleri, dil çalışan süreçler arasında eşit olarak dağıtılır. Dil çalışan işlemleri, IŞLEVLER tarafından ayarlanan sayı\_çalışan\_Işlem\_SAYıSıNA ulaşıldığında her 10 saniyede bir oluşturulur. Birden çok dil çalışan işleminin kullanılması, [ölçeklendirmeyle](functions-scale.md)aynı değildir. İş yükünüz, CPU ile bağlantılı ve g/ç bağlantılı çağırma karışımına sahip olduğunda bu ayarı kullanmayı göz önünde bulundurun. Bu ayar tüm non-.NET dilleri için geçerlidir.

|Anahtar|Örnek değer|
|---|------------|
|Işlemler\_çalışan\_Işlem\_sayısı|2|


## <a name="functions_worker_runtime"></a>ÇALıŞAN\_çalışma zamanı\_IŞLEVLERI

İşlev uygulamasına yüklemek için dil alt çalışma zamanı.  Bu, uygulamada (örneğin, "dotnet") kullanılan dil karşılık gelir. Birden çok dilde işlevler için her bir karşılık gelen alt çalışma zamanı değeri ile birden fazla uygulama yayımlamak gerekir.  Geçerli değerler şunlardır `dotnet` (C#/F#), `node` (JavaScript/TypeScript), `java` (Java), `powershell` (PowerShell) ve `python` (Python).

|Anahtar|Örnek değer|
|---|------------|
|ÇALıŞAN\_çalışma zamanı\_IŞLEVLERI|DotNet|

## <a name="website_contentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

Yalnızca tüketim & Premium planlar için. İşlevi uygulama kodu ve yapılandırması depolandığı depolama hesabı için bağlantı dizesi. Bkz. [işlev uygulaması oluşturma](functions-infrastructure-as-code.md#create-a-function-app).

|Anahtar|Örnek değer|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [anahtar]|

## <a name="website_contentshare"></a>Web sItesI\_CONTENTSHARE

Yalnızca tüketim & Premium planlar için. İşlev uygulaması kod ve yapılandırma dosyası yolu. WEBSITE_CONTENTAZUREFILECONNECTIONSTRING ile kullanılır. Varsayılan işlev uygulamasının adı ile başlayan benzersiz bir dizedir. Bkz. [işlev uygulaması oluşturma](functions-infrastructure-as-code.md#create-a-function-app).

|Anahtar|Örnek değer|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="website_max_dynamic_application_scale_out"></a>Web sItesI\_en yüksek\_dınamık\_uygulama\_ölçek\_GENIŞLETME

İşlev uygulaması için ölçeğini genişletebilirsiniz örneklerinin sayısı. Varsayılan olarak sınır yoktur.

> [!NOTE]
> Bu ayar, Önizleme aşamasında yalnızca güvenilir ise bir değere ayarlayın ve özellik - < = 5

|Anahtar|Örnek değer|
|---|------------|
|Web sItesI\_en yüksek\_dınamık\_uygulama\_ölçek\_GENIŞLETME|5|

## <a name="website_node_default_version"></a>Web sItesI\_NODE\_DEFAULT_VERSION

_Yalnızca Windows._  
Windows üzerinde işlev uygulamanızı çalıştırırken kullanılacak Node. js sürümünü ayarlar. Çalışma zamanının hedeflenen ana sürümün kullanılabilir en son sürümünü kullanmasını sağlamak için bir tilde (~) kullanmanız gerekir. Örneğin, `~10`olarak ayarlandığında, Node. js 10 ' un en son sürümü kullanılır. Ana sürüm bir tilde ile hedeflenirse, ikincil sürümü el ile güncelleştirmeniz gerekmez. 

|Anahtar|Örnek değer|
|---|------------|
|Web sItesI\_NODE\_DEFAULT_VERSION|~ 10|

## <a name="website_run_from_package"></a>Web sItesI\_\_PAKETINDEN\_ÇALıŞTıR

Takılı paket dosyasından çalıştırılacak işlev uygulamanızı sağlar.

|Anahtar|Örnek değer|
|---|------------|
|Web sItesI\_\_PAKETINDEN\_ÇALıŞTıR|1|

Geçerli değerler, bir dağıtım paketi dosyasının konumunu çözen bir URL veya `1`. `1`olarak ayarlandığında, paketin `d:\home\data\SitePackages` klasöründe olması gerekir. Zip dağıtımı Bu ayar ile kullanıldığında, paketi bu konuma otomatik olarak yüklenir. Önizlemede, bu ayar `WEBSITE_RUN_FROM_ZIP`olarak adlandırılmıştır. Daha fazla bilgi için bkz. [işlevlerinizi bir paket dosyasından çalıştırma](run-functions-from-deployment-package.md).

## <a name="azure_function_proxy_disable_local_call"></a>AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL

Varsayılan olarak, yeni bir HTTP isteği oluşturmak yerine, işlev uygulamasının işlevleri için doğrudan proxy'leri API çağrıları göndermek için bir kısayol işlev proxy'lerini yararlanacaktır. Bu ayar, bu davranışı devre dışı bırakmanızı sağlar.

|Anahtar|Değer|Açıklama|
|-|-|-|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|true|Yerel İşlev Uygulaması bir işleve işaret eden arka uç URL 'SI olan çağrılar artık doğrudan işleve gönderilmez ve bunun yerine İşlev Uygulaması için HTTP ön ucuna geri yönlendirilir|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|false|Varsayılan değer budur. Yerel İşlev Uygulaması bir işleve işaret eden arka uç URL 'SI olan çağrılar, doğrudan bu Işleve iletilir|


## <a name="azure_function_proxy_backend_url_decode_slashes"></a>AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES

Bu ayar, arka uç URL'si yerleştirildiğinde % 2F rota parametrelerine eğik çizgi olarak olduğu için kodu olup olmadığını denetler. 

|Anahtar|Değer|Açıklama|
|-|-|-|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|true|Rota parametrelerine kodlanmış eğik çizgi ile bunları çözülmüş olacaktır. `example.com/api%2ftest` `example.com/api/test` olacak|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|false|Bu varsayılan davranıştır. Tüm yol boyunca parametreleri geçirilecek değişmedi|

### <a name="example"></a>Örnek

URL myfunction.com bir işlev uygulaması ile bir örnek proxies.json İşte

```JSON
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "root": {
            "matchCondition": {
                "route": "/{*all}"
            },
            "backendUri": "example.com/{all}"
        }
    }
}
```
|URL kod çözme|Girdi|Çıktı|
|-|-|-|
|true|myFunction.com/test%2fapi|example.com/test/api
|false|myFunction.com/test%2fapi|example.com/test%2fapi|


## <a name="next-steps"></a>Sonraki adımlar

[Uygulama ayarlarını güncelleştirmeyi öğrenin](functions-how-to-use-azure-function-app-settings.md#settings)

[Bkz. Host. JSON dosyasındaki genel ayarlar](functions-host-json.md)

[App Service uygulamalar için diğer uygulama ayarlarına bakın](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
