---
title: Azure İşlevleri için uygulama ayarları başvurusu
description: Azure Işlevleri uygulama ayarları veya ortam değişkenleri için başvuru belgeleri.
ms.topic: conceptual
ms.date: 09/22/2018
ms.openlocfilehash: 5a0201eeed1678299ec16ff268062463b9c75e5c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84235360"
---
# <a name="app-settings-reference-for-azure-functions"></a>Azure İşlevleri için uygulama ayarları başvurusu

Bir işlev uygulamasındaki uygulama ayarları, bu işlev uygulaması için tüm işlevleri etkileyen genel yapılandırma seçeneklerini içerir. Yerel olarak çalıştırdığınızda, bu ayarlara yerel [ortam değişkenleri](functions-run-local.md#local-settings-file)olarak erişilir. Bu makalede, işlev uygulamalarında kullanılabilen uygulama ayarları listelenir.

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Dosyasında ve [local.settings.json](functions-run-local.md#local-settings-file) dosyasında [host.js](functions-host-json.md) başka genel yapılandırma seçenekleri vardır.

## <a name="appinsights_instrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Application Insights için izleme anahtarı. Ya da yalnızca birini `APPINSIGHTS_INSTRUMENTATIONKEY` kullanın `APPLICATIONINSIGHTS_CONNECTION_STRING` . Daha fazla bilgi için bkz. [Azure Işlevlerini izleme](functions-monitoring.md). 

|Anahtar|Örnek değer|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|55555555-af77-484b-9032-64f83bb83bb|

## <a name="applicationinsights_connection_string"></a>APPLICATIONINSIGHTS_CONNECTION_STRING

Application Insights için bağlantı dizesi. `APPLICATIONINSIGHTS_CONNECTION_STRING` `APPINSIGHTS_INSTRUMENTATIONKEY` İşlev uygulamanız, bağlantı dizesi kullanılarak desteklenen eklenmiş özelleştirmeleri gerektirdiğinde kullanın. Daha fazla bilgi için bkz. [bağlantı dizeleri](../azure-monitor/app/sdk-connection-string.md). 

|Anahtar|Örnek değer|
|---|------------|
|APPLICATIONINSIGHTS_CONNECTION_STRING|Instrumentationkey = [anahtar]; ınestionendpoint = [URL]; LiveEndpoint = [URL]; ProfilerEndpoint = [URL]; Anlık görüntü Tendpoint = [URL];|

## <a name="azure_functions_environment"></a>AZURE_FUNCTIONS_ENVIRONMENT

2. x ve sonraki Işlevler çalışma zamanının sürümlerinde, çalışma zamanı ortamı temelinde uygulama davranışını yapılandırır. Bu değer [başlatma sırasında okundu](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/Program.cs#L43). `AZURE_FUNCTIONS_ENVIRONMENT`Herhangi bir değere ayarlayabilirsiniz, ancak [üç değer](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) desteklenir: [geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [hazırlık](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging)ve [Üretim](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). `AZURE_FUNCTIONS_ENVIRONMENT`Ayarlanmadıysa, varsayılan olarak `Development` yerel bir ortamda ve `Production` Azure 'da olur. Bu ayar, `ASPNETCORE_ENVIRONMENT` çalışma zamanı ortamını ayarlamak için yerine kullanılmalıdır. 

## <a name="azurefunctionsjobhost__"></a>AzureFunctionsJobHost__\*

Işlev çalışma zamanının sürüm 2. x ve sonraki sürümlerinde, uygulama ayarları geçerli ortamdaki ayarlarda [host.js](functions-host-json.md) geçersiz kılabilir. Bu geçersiz kılmalar, adlı uygulama ayarları olarak ifade edilir `AzureFunctionsJobHost__path__to__setting` . Daha fazla bilgi için bkz. [değerler üzerinde host.jsgeçersiz kılma](functions-host-json.md#override-hostjson-values).

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Günlükleri depolamak ve portaldaki **izleyici** sekmesinde görüntülemek için isteğe bağlı depolama hesabı bağlantı dizesi. Bu ayar yalnızca Azure Işlevleri çalışma zamanının 1. x sürümünü hedefleyen uygulamalar için geçerlidir. Depolama hesabı Blobları, kuyrukları ve tabloları destekleyen bir genel amaçlı olmalıdır. Daha fazla bilgi için bkz. [depolama hesabı gereksinimleri](storage-considerations.md#storage-account-requirements).

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol = https; AccountName = <name> ; AccountKey =<key>|

> [!NOTE]
> Daha iyi performans ve deneyim için, çalışma zamanı sürüm 2. x ve sonraki sürümleri, APPINSIGHTS_INSTRUMENTATIONKEY ve izleme için uygulama öngörülerini kullanır `AzureWebJobsDashboard` .

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true`, bir işlev uygulamasının kök URL 'SI için gösterilen varsayılan giriş sayfasını devre dışı bırakır. `false` varsayılan değerdir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDisableHomepage|true|

Bu uygulama ayarı atlandığında veya olarak ayarlandığında `false` , URL 'ye yanıt olarak aşağıdaki örneğe benzer bir sayfa görüntülenir `<functionappname>.azurewebsites.net` .

![İşlev uygulaması giriş sayfası](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true`, .NET kodu derlenirken yayın modunun kullanılacağı anlamına gelir; `false`hata ayıklama modunu kullanma anlamına gelir. `true` varsayılan değerdir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Etkinleştirilecek beta özellikleri için virgülle ayrılmış bir liste. Bu bayraklar tarafından etkinleştirilen beta özellikleri üretime hazırlanmaz, ancak canlı çalışmadan önce deneysel kullanım için etkinleştirilebilir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsFeatureFlags|özellik1, Özellik2|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Anahtar depolaması için kullanılacak depoyu veya sağlayıcıyı belirtir. Şu anda, desteklenen depolar BLOB depolama ("blob") ve yerel dosya sistemi ("dosyalar") ' dir. Varsayılan, sürüm 2 ' deki blob ve sürüm 1 ' de dosya sistemidir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsSecretStorageType|Dosyalar|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

Azure Işlevleri çalışma zamanı, HTTP ile tetiklenen işlevler hariç tüm işlevler için bu depolama hesabı bağlantı dizesini kullanır. Depolama hesabı Blobları, kuyrukları ve tabloları destekleyen bir genel amaçlı olmalıdır. Bkz. [depolama hesabı](functions-infrastructure-as-code.md#storage-account) ve [depolama hesabı gereksinimleri](storage-considerations.md#storage-account-requirements).

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol = https; AccountName = [ad]; AccountKey = [anahtar]|

## <a name="azurewebjobs_typescriptpath"></a>AzureWebJobs_TypeScriptPath

TypeScript için kullanılan derleyicinin yolu. Gerekirse varsayılan ayarı geçersiz kılmanızı sağlar.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobs_TypeScriptPath|%HOME%\typescript|

## <a name="function_app_edit_mode"></a>Işlev \_ uygulaması \_ düzenleme \_ modu

Azure portal düzenlemenin etkinleştirilip etkinleştirilmeyeceğini belirler. Geçerli değerler şunlardır "ReadWrite" ve "ReadOnly".

|Anahtar|Örnek değer|
|---|------------|
|Işlev \_ uygulaması \_ düzenleme \_ modu|readonly|

## <a name="functions_extension_version"></a>Işlev \_ Uzantısı \_ sürümü

Bu işlev uygulamasında kullanılacak Işlevler çalışma zamanının sürümü. Ana sürümü olan bir tilde, bu ana sürümün en son sürümünü (örneğin, "~ 2") kullanır. Aynı ana sürüme yönelik yeni sürümler kullanılabilir olduğunda, bunlar işlev uygulamasına otomatik olarak yüklenir. Uygulamayı belirli bir sürüme sabitlemek için, tam sürüm numarasını (örneğin, "2.0.12345") kullanın. Varsayılan değer "~ 2" dir. Bir değeri `~1` , uygulamanızı çalışma zamanının 1. x sürümüne sabitlemelerini sağlar.

|Anahtar|Örnek değer|
|---|------------|
|Işlev \_ Uzantısı \_ sürümü|~ 2|

## <a name="functions_v2_compatibility_mode"></a>IŞLEVLER \_ v2 \_ Uyumluluk \_ modu

Bu ayar, işlev uygulamanızın sürüm 3. x çalışma zamanı üzerinde sürüm 2. x uyumlu modda çalışmasını sağlar. Bu ayarı yalnızca [, işlev uygulamanızı çalışma zamanının 2. x sürümünden 3. x sürümüne yükseltirken](functions-versions.md#migrating-from-2x-to-3x)sorunlarla karşılaşırsanız kullanın. 

>[!IMPORTANT]
> Bu ayar yalnızca, uygulamanızı 3. x sürümünde doğru şekilde çalışacak şekilde güncelleştirdiğinizde kısa süreli geçici bir çözüm olarak tasarlanmıştır. [2. x çalışma zamanının desteklendiği](functions-versions.md)sürece bu ayar desteklenir. Uygulamanızın bu ayarı kullanmadan sürüm 3. x üzerinde çalışmasını engelleyen sorunlarla karşılaşırsanız, lütfen [sorununuzu bildirin](https://github.com/Azure/azure-functions-host/issues/new?template=Bug_report.md).

[İşlev \_ Uzantısı \_ sürümünün](functions-app-settings.md#functions_extension_version) olarak ayarlanmasını gerektirir `~3` .

|Anahtar|Örnek değer|
|---|------------|
|IŞLEVLER \_ v2 \_ Uyumluluk \_ modu|true|

## <a name="functions_worker_process_count"></a>Işlev \_ çalışan \_ işlem \_ sayısı

Varsayılan değeri olan en fazla dil çalışan işlemi sayısını belirtir `1` . İzin verilen en büyük değer `10` . İşlev etkinleştirmeleri, dil çalışan süreçler arasında eşit olarak dağıtılır. Dil çalışan işlemleri, IŞLEVLERIN \_ çalışan işlem sayısına göre ayarlanan sayıya ulaşılana kadar her 10 saniyede bir oluşturulur \_ \_ . Birden çok dil çalışan işleminin kullanılması, [ölçeklendirmeyle](functions-scale.md)aynı değildir. İş yükünüz, CPU ile bağlantılı ve g/ç bağlantılı çağırma karışımına sahip olduğunda bu ayarı kullanmayı göz önünde bulundurun. Bu ayar tüm non-.NET dilleri için geçerlidir.

|Anahtar|Örnek değer|
|---|------------|
|Işlev \_ çalışan \_ işlem \_ sayısı|2|


## <a name="functions_worker_runtime"></a>IŞLEVLER \_ Worker \_ çalışma zamanı

İşlev uygulamasında yüklenecek dil çalışanı çalışma zamanı.  Bu, uygulamanızda kullanılan dile karşılık gelir (örneğin, "DotNet"). Birden çok dildeki işlevler için, bunları her biri karşılık gelen bir çalışan çalışma zamanı değeri olan birden çok uygulamaya yayımlamanız gerekir.  Geçerli değerler şunlardır `dotnet` (C#/f #), `node` (JavaScript/TypeScript), `java` (Java), `powershell` (PowerShell) ve `python` (Python).

|Anahtar|Örnek değer|
|---|------------|
|IŞLEVLER \_ Worker \_ çalışma zamanı|dotnet|

## <a name="website_contentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

Yalnızca tüketim & Premium planlar için. İşlev uygulaması kodu ve yapılandırmasının depolandığı depolama hesabı için bağlantı dizesi. Bkz. [işlev uygulaması oluşturma](functions-infrastructure-as-code.md#create-a-function-app).

|Anahtar|Örnek değer|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol = https; AccountName = [ad]; AccountKey = [anahtar]|

## <a name="website_contentshare"></a>Web sItesI \_ contentshare

Yalnızca tüketim & Premium planlar için. İşlev uygulaması kodu ve yapılandırmasının dosya yolu. WEBSITE_CONTENTAZUREFILECONNECTIONSTRING ile kullanılır. Varsayılan değer, işlev uygulaması adı ile başlayan benzersiz bir dizedir. Bkz. [işlev uygulaması oluşturma](functions-infrastructure-as-code.md#create-a-function-app).

|Anahtar|Örnek değer|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="website_max_dynamic_application_scale_out"></a>Web sItesI \_ en büyük \_ dinamik \_ uygulama \_ ölçeği \_ genişletme

İşlev uygulamasının ölçeklenebilen en fazla örnek sayısı. Varsayılan sınır yoktur.

> [!NOTE]
> Bu ayar bir önizleme özelliğidir ve yalnızca bir değere ayarlanmışsa güvenilir <= 5

|Anahtar|Örnek değer|
|---|------------|
|Web sItesI \_ en büyük \_ dinamik \_ uygulama \_ ölçeği \_ genişletme|5|

## <a name="website_node_default_version"></a>Web sItesI \_ düğümü \_ DEFAULT_VERSION

_Yalnızca Windows._  
İşlev uygulamanızı Windows 'da çalıştırırken kullanılacak Node.js sürümünü ayarlar. Çalışma zamanının hedeflenen ana sürümün kullanılabilir en son sürümünü kullanmasını sağlamak için bir tilde (~) kullanmanız gerekir. Örneğin, olarak ayarlandığında `~10` , Node.js 10 ' un en son sürümü kullanılır. Ana sürüm bir tilde ile hedeflenirse, ikincil sürümü el ile güncelleştirmeniz gerekmez. 

|Anahtar|Örnek değer|
|---|------------|
|Web sItesI \_ düğümü \_ DEFAULT_VERSION|~ 10|

## <a name="website_run_from_package"></a>paketten Web sItesI \_ çalıştırma \_ \_

İşlev uygulamanızın bağlı bir paket dosyasından çalıştırılmasını sağlar.

|Anahtar|Örnek değer|
|---|------------|
|paketten Web sItesI \_ çalıştırma \_ \_|1|

Geçerli değerler, bir dağıtım paketi dosyasının konumunu çözen bir URL ya da `1` . Olarak ayarlandığında `1` , paketin klasörde olması gerekir `d:\home\data\SitePackages` . Bu ayar ile ZIP dağıtımı kullanılırken, paket otomatik olarak bu konuma yüklenir. Önizlemede, bu ayar adlandırılmıştı `WEBSITE_RUN_FROM_ZIP` . Daha fazla bilgi için bkz. [işlevlerinizi bir paket dosyasından çalıştırma](run-functions-from-deployment-package.md).

## <a name="azure_function_proxy_disable_local_call"></a>AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL

Varsayılan olarak Işlev proxy 'leri, yeni bir HTTP isteği oluşturmak yerine, proxy 'lerden doğrudan aynı İşlev Uygulaması işlevlere API çağrıları göndermek için bir kısayol kullanır. Bu ayar, bu davranışı devre dışı bırakmanızı sağlar.

|Anahtar|Değer|Açıklama|
|-|-|-|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|true|Yerel İşlev Uygulaması bir işleve işaret eden arka uç URL 'SI olan çağrılar artık doğrudan işleve gönderilmez ve bunun yerine İşlev Uygulaması için HTTP ön ucuna geri yönlendirilir|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|yanlış|Varsayılan değer budur. Yerel İşlev Uygulaması bir işleve işaret eden arka uç URL 'SI olan çağrılar, doğrudan bu Işleve iletilir|


## <a name="azure_function_proxy_backend_url_decode_slashes"></a>AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES

Bu ayar,% 2F ' nin, arka uç URL 'sine eklendiklerinde rota parametrelerinde eğik çizgi olarak çözülmüş olup olmadığını denetler. 

|Anahtar|Değer|Açıklama|
|-|-|-|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|true|Kodlanmış eğik çizgi içeren rota parametrelerinin kodu çözülür. `example.com/api%2ftest`olacak`example.com/api/test`|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|yanlış|Bu, varsayılan davranıştır. Tüm rota parametreleri değişmeden kalacak şekilde geçirilecek|

### <a name="example"></a>Örnek

İşte myfunction.com URL 'sindeki bir işlev uygulamasında bir örnek proxies.js

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
|true|myfunction.com/test%2fapi|example.com/test/api
|yanlış|myfunction.com/test%2fapi|example.com/test%2fapi|


## <a name="next-steps"></a>Sonraki adımlar

[Uygulama ayarlarını güncelleştirmeyi öğrenin](functions-how-to-use-azure-function-app-settings.md#settings)

[Dosyadaki host.jsgenel ayarlar ' a bakın](functions-host-json.md)

[App Service uygulamalar için diğer uygulama ayarlarına bakın](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
