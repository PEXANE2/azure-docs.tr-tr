---
title: Azure İşlevleri için uygulama ayarları başvurusu
description: Azure İşlevleri uygulama ayarları veya ortam değişkenleri için başvuru belgeleri.
ms.topic: conceptual
ms.date: 09/22/2018
ms.openlocfilehash: 3853ccbfd492bfaf4a82d62e6d31ab938285ee2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277836"
---
# <a name="app-settings-reference-for-azure-functions"></a>Azure İşlevleri için uygulama ayarları başvurusu

Bir işlev uygulamasındaki uygulama ayarları, o işlev uygulamasının tüm işlevlerini etkileyen genel yapılandırma seçenekleri içerir. Yerel olarak çalıştırdığınızda, bu ayarlara yerel [ortam değişkenleri](functions-run-local.md#local-settings-file)olarak erişilir. Bu makalede, işlev uygulamalarında kullanılabilen uygulama ayarları listelenir.

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

[Host.json](functions-host-json.md) dosyasında ve [local.settings.json](functions-run-local.md#local-settings-file) dosyasında başka genel yapılandırma seçenekleri de vardır.

## <a name="appinsights_instrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Application Insights kullanıyorsanız, Application Insights enstrümantasyon anahtarı. Bkz. [Azure İşlerini İzle](functions-monitoring.md).

|Anahtar|Örnek değer|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azure_functions_environment"></a>AZURE_FUNCTIONS_ENVIRONMENT

İşlevler çalışma zamanının sürüm 2.x ve sonraki sürümlerinde, uygulama davranışını çalışma zamanı ortamına göre yapılandırır. Bu değer [başlatma sırasında okunur.](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/Program.cs#L43) Herhangi bir `AZURE_FUNCTIONS_ENVIRONMENT` değere ayarlayabilirsiniz, ancak [üç değer](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) desteklenir: [Geliştirme,](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development) [Evreleme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging)ve [Üretim.](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production) `AZURE_FUNCTIONS_ENVIRONMENT` Ayarlı olmadığında, varsayılan olarak `Development` yerel bir ortamda `Production` ve Azure'da olur. Bu ayar çalışma zamanı `ASPNETCORE_ENVIRONMENT` ortamını ayarlamak yerine kullanılmalıdır. 

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Günlükleri depolamak ve portaldaki **Monitör** sekmesinde görüntülemek için isteğe bağlı depolama hesabı bağlantı dizesi. Bu ayar yalnızca Azure İşlevleri çalışma zamanının 1.x sürümünü hedefleyen uygulamalar için geçerlidir. Depolama hesabı, lekeleri, kuyrukları ve tabloları destekleyen genel amaçlı bir hesap olmalıdır. Daha fazla bilgi için [Depolama hesabı gereksinimlerine](storage-considerations.md#storage-account-requirements)bakın.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDashboard|VarsayılanEndpointsProtocol=https; Hesap Adı= ;<name> Hesap Anahtarı=<key>|

> [!NOTE]
> Daha iyi performans ve deneyim için, runtime sürüm 2.x ve sonraki `AzureWebJobsDashboard`sürümler yerine izleme için APPINSIGHTS_INSTRUMENTATIONKEY ve App Insights kullanın.

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableAna Sayfa

`true`bir işlev uygulamasının kök URL'si için gösterilen varsayılan açılış sayfasını devre dışı bırakma anlamına gelir. `false` varsayılan değerdir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDisableAna Sayfa|true|

Bu uygulama ayarı atlandığında veya `false`ayarlandığında, URL'ye `<functionappname>.azurewebsites.net`yanıt olarak aşağıdaki örneğe benzer bir sayfa görüntülenir.

![Fonksiyon uygulaması açılış sayfası](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseDerleme

`true`.NET kodunu derlerken Serbest Bırakma modunu kullanmak anlamına gelir; `false` Hata Ayıklama modunu kullanmak anlamına gelir. `true` varsayılan değerdir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDotNetReleaseDerleme|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Etkinleştirmek için beta özelliklerinin virgülle sınırlı bir listesi. Bu bayraklar tarafından etkinleştirilen Beta özellikleri üretime hazır değildir, ancak yayına girmeden önce deneysel kullanım için etkinleştirilebilir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsFeatureFlags|özellik1,feature2|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Anahtar depolama için kullanılacak depoyu veya sağlayıcıyı belirtir. Şu anda desteklenen depolar blob depolama ("Blob") ve yerel dosya sistemi ("Dosyalar") bulunmaktadır. Varsayılan sürüm 2'deki blob ve sürüm 1'deki dosya sistemidir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsSecretStorageType|Dosyalar|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

Azure İşlevler çalışma süresi, HTTP tetiklenen işlevler dışında tüm işlevler için bu depolama hesabı bağlantı dizesini kullanır. Depolama hesabı, lekeleri, kuyrukları ve tabloları destekleyen genel amaçlı bir hesap olmalıdır. Bkz. [Depolama hesabı](functions-infrastructure-as-code.md#storage-account) ve [Depolama hesabı gereksinimleri.](storage-considerations.md#storage-account-requirements)

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsStorage|VarsayılanEndpointsProtocol=https; Hesap Adı=[ad]; AccountKey=[anahtar]|

## <a name="azurewebjobs_typescriptpath"></a>AzureWebJobs_TypeScriptPath

TypeScript için kullanılan derleyiciye giden yol. Gerekirse varsayılan geçersiz kılmanızı sağlar.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobs_TypeScriptPath|%HOME%\typescript|

## <a name="function_app_edit_mode"></a>\_İŞLEV\_UYGULAMA\_EDIT MODU

Azure portalında düzenlemenin etkin olup olmadığını belirler. Geçerli değerler "readwrite" ve "readonlyonly" dir.

|Anahtar|Örnek değer|
|---|------------|
|\_İŞLEV\_UYGULAMA\_EDIT MODU|readonly|

## <a name="functions_extension_version"></a>İşLEVLER\_UZATMA\_SÜRÜMÜ

Bu işlev uygulamasında kullanılacak Işlevler çalışma zamanı sürümü. Ana sürümü olan bir tilde, bu ana sürümün en son sürümünün (örneğin, "~2") kullanılması anlamına gelir. Aynı ana sürüm için yeni sürümler kullanılabilir olduğunda, işlev uygulamasına otomatik olarak yüklenirler. Uygulamayı belirli bir sürüme sabitlemek için tam sürüm numarasını kullanın (örneğin, "2.0.12345"). Varsayılan değer "~2"dir. Uygulamanızı `~1` çalışma zamanının 1.x sürümüne sabitlemedeğeri.

|Anahtar|Örnek değer|
|---|------------|
|İşLEVLER\_UZATMA\_SÜRÜMÜ|~2|

## <a name="functions_v2_compatibility_mode"></a>\_İşLEVLER\_V2 UYUMLULUK\_MODU

Bu ayar, işlev uygulamanızın sürüm 3.x çalışma zamanında sürüm 2.x uyumlu modda çalışmasını sağlar. Bu ayarı yalnızca işlev [uygulamanızı sürüm 2.x'ten 3.x'e yükselterken](functions-versions.md#migrating-from-2x-to-3x)sorunlarla karşılaşırsanız kullanın. 

>[!IMPORTANT]
> Bu ayar, uygulamanızı sürüm 3.x'te düzgün çalışacak şekilde güncellerken yalnızca kısa vadeli bir geçici çözüm olarak tasarlanmıştır. Bu [ayar, 2.x çalışma süresi desteklendikçe](functions-versions.md)desteklenir. Uygulamanızın bu ayarı kullanmadan sürüm 3.x'te çalışmasını engelleyen sorunlarla karşılaşırsanız, lütfen [sorununuzu bildirin.](https://github.com/Azure/azure-functions-host/issues/new?template=Bug_report.md)

[İşLEVLER\_\_UZATMA SÜRÜMÜ'ne](functions-app-settings.md#functions_extension_version) `~3`ayarlanmalıdır.

|Anahtar|Örnek değer|
|---|------------|
|\_İşLEVLER\_V2 UYUMLULUK\_MODU|true|

## <a name="functions_worker_process_count"></a>İşLEVLER\_İşÇİ\_İŞLEM\_SAYISI

Varsayılan değeri olan en fazla sözcük alt işlem `1`sayısını belirtir. İzin verilen maksimum `10`değer. İşlev çağrıları dil çalışanı süreçleri arasında eşit olarak dağıtılır. İşLEVLER İşÇİ\_\_İŞLEM\_SAYISI tarafından belirlenen sayıma ulaşılıncaya kadar dil işçi süreçleri her 10 saniyede bir ortaya çıkar. Birden çok dil alt işlemleri kullanarak [ölçekleme](functions-scale.md)aynı değildir. İş yükünüzün CPU'ya bağlı ve G/Ç'ye bağlı çağrılarının bir karışımı olduğunda bu ayarı kullanmayı düşünün. Bu ayar tüm non-.NET diller için geçerlidir.

|Anahtar|Örnek değer|
|---|------------|
|İşLEVLER\_İşÇİ\_İŞLEM\_SAYISI|2|


## <a name="functions_worker_runtime"></a>İşLEVLER\_İşÇİ\_Çalışma ZAMANı

İşlev uygulamasında yüklemek için dil çalışanı çalışma süresi.  Bu, uygulamanızda kullanılan dile karşılık gelecektir (örneğin, "dotnet"). Birden çok dildeki işlevler için, her biri karşılık gelen bir alt çalışma zamanı değerine sahip birden çok uygulamada yayımlamanız gerekir.  Geçerli değerler `dotnet` (C#/F#), `node` (JavaScript/TypeScript), `java` `powershell` (Java), (PowerShell) ve `python` (Python) 'dir.

|Anahtar|Örnek değer|
|---|------------|
|İşLEVLER\_İşÇİ\_Çalışma ZAMANı|dotnet|

## <a name="website_contentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

Tüketim & Premium planları için sadece. İşlev uygulama kodu ve yapılandırmasının depolandığı depolama hesabı için bağlantı dizesi. Bkz. [Bir işlev uygulaması oluşturun.](functions-infrastructure-as-code.md#create-a-function-app)

|Anahtar|Örnek değer|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|VarsayılanEndpointsProtocol=https; Hesap Adı=[ad]; AccountKey=[anahtar]|

## <a name="website_contentshare"></a>WEB\_SITESI CONTENTSHARE

Tüketim & Premium planları için sadece. İşlev uygulama koduna ve yapılandırmasına giden dosya yolu. WEBSITE_CONTENTAZUREFILECONNECTIONSTRING ile kullanılır. Varsayılan işlev uygulaması adı ile başlayan benzersiz bir dizedir. Bkz. [Bir işlev uygulaması oluşturun.](functions-infrastructure-as-code.md#create-a-function-app)

|Anahtar|Örnek değer|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="website_max_dynamic_application_scale_out"></a>WEB\_\_SITESI\_\_MAX\_DINAMIK UYGULAMA ÖLÇEĞI

İşlev uygulamasının ölçeklendirebileceği en fazla örnek sayısı. Varsayılan sınır değildir.

> [!NOTE]
> Bu ayar bir önizleme özelliğidir ve yalnızca bir değer <= 5 olarak ayarlanırsa güvenilirdir

|Anahtar|Örnek değer|
|---|------------|
|WEB\_\_SITESI\_\_MAX\_DINAMIK UYGULAMA ÖLÇEĞI|5|

## <a name="website_node_default_version"></a>WEB\_SITESI\_DÜĞÜMÜ DEFAULT_VERSION

_Yalnızca Windows._  
Windows'da işlev uygulamanızı çalıştırırken kullanılacak Node.js sürümünü ayarlar. Hedeflenen ana sürümün kullanılabilir en son sürümünü kullanma zamanı için bir tilde (~) kullanmanız gerekir. Örneğin, Node.js 10'un en son sürümü olarak ayarlandığında `~10`kullanılır. Ana sürüm tilde ile hedeflendiğinde, küçük sürümü el ile güncelleştirmeniz gerekmez. 

|Anahtar|Örnek değer|
|---|------------|
|WEB\_SITESI\_DÜĞÜMÜ DEFAULT_VERSION|~10|

## <a name="website_run_from_package"></a>\_PAKETTEN\_\_WEB SİTESİ ÇALıŞTıR

İşlev uygulamanızın monte edilmiş bir paket dosyasından çalışmasını sağlar.

|Anahtar|Örnek değer|
|---|------------|
|\_PAKETTEN\_\_WEB SİTESİ ÇALıŞTıR|1|

Geçerli değerler, dağıtım paketi dosyasının konumuna göre çözümleyen `1`bir URL veya. `1`Ayarlandığında, paket klasörde `d:\home\data\SitePackages` olmalıdır. Bu ayarla zip dağıtımı kullanırken, paket otomatik olarak bu konuma yüklenir. Önizlemede, bu ayarın adı . `WEBSITE_RUN_FROM_ZIP` Daha fazla bilgi için [bkz.](run-functions-from-deployment-package.md)

## <a name="azure_function_proxy_disable_local_call"></a>AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL

Varsayılan olarak İşlevler yakınlıkları, yeni bir HTTP isteği oluşturmak yerine, yakınlıklardan doğrudan aynı İşlev Uygulamasındaki işlevlere API çağrıları göndermek için bir kısayol kullanır. Bu ayar, bu davranışı devre dışı bilebilir sağlar.

|Anahtar|Değer|Açıklama|
|-|-|-|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|true|Yerel İşlev Uygulamasındaki bir işlevi gösteren arka uç URL'si olan aramalar artık doğrudan işleve gönderilmez ve bunun yerine İşlev Uygulaması için HTTP ön ucuna yönlendirilir|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|yanlış|Varsayılan değer budur. Yerel İşlev Uygulamasındaki bir işlevi gösteren arka uç URL'si olan çağrılar doğrudan bu Fonksiyona iletilir|


## <a name="azure_function_proxy_backend_url_decode_slashes"></a>AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES

Bu ayar, %2F'nin arka uç URL'sine eklendiğinde rota parametrelerinde kesik olarak çözülüp çözülmediğini denetler. 

|Anahtar|Değer|Açıklama|
|-|-|-|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|true|Kodlanmış kesiklerle rota parametreleri deşifre edilir. `example.com/api%2ftest`olacak`example.com/api/test`|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|yanlış|Bu varsayılan davranıştır. Tüm rota parametreleri değişmeden geçirilecektir|

### <a name="example"></a>Örnek

Burada URL myfunction.com bir işlev uygulamasında bir örnek proxies.json olduğunu

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
|URL Kod Çözme|Giriş|Çıktı|
|-|-|-|
|true|myfunction.com/test%2fapi|example.com/test/api
|yanlış|myfunction.com/test%2fapi|example.com/test%2fapi|


## <a name="next-steps"></a>Sonraki adımlar

[Uygulama ayarlarını nasıl güncelleştiridersiniz öğrenin](functions-how-to-use-azure-function-app-settings.md#settings)

[Ana bilgisayar.json dosyasındaki genel ayarlara bakın](functions-host-json.md)

[Uygulama Hizmeti uygulamaları için diğer uygulama ayarlarına bakın](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
