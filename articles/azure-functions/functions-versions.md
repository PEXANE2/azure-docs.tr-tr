---
title: Azure Fonksiyonları çalışma zamanı sürümlerine genel bakış
description: Azure İşlevler çalışma zamanının birden çok sürümlerini destekler. Aralarındaki farkları ve sizin için doğru olanı nasıl seçeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 12/09/2019
ms.openlocfilehash: e90752e89be7e381b06f8a87f76f123f0e4a8e3a
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422490"
---
# <a name="azure-functions-runtime-versions-overview"></a>Azure Fonksiyonları çalışma zamanı sürümlerine genel bakış

Azure İşlevleri çalışma zamanının ana sürümleri, çalışma zamanının temel aldığı .NET sürümüyle ilişkilidir. Aşağıdaki tablo, çalışma zamanının geçerli sürümünü, sürüm düzeyini ve ilgili .NET sürümünü gösterir. 

| Çalışma zamanı sürümü | Sürüm düzeyi<sup>1</sup> | .NET sürümü | 
| --------------- | ------------- | ------------ |
| 3.x | GA | .NET Çekirdek 3.1 | 
| 2.x | GA | .NET Core 2.2 |
| 1.x | GA<sup>2</sup> | .NET Çerçeve 4.7.2<sup>3</sup> |

<sup>1</sup> GA sürümleri üretim senaryoları için desteklenir.   
<sup>2</sup> Sürüm 1.x bakım modundadır. Geliştirmeler yalnızca sonraki sürümlerde sağlanır.   
<sup>3</sup> Yalnızca Azure portalında veya Windows bilgisayarlarında yerel olarak geliştirmeyi destekler.

Bu makalede, çeşitli sürümler arasındaki bazı farklar, her sürümü nasıl oluşturabileceğiniz ve sürümleri nasıl değiştireceğiniz ayrıntılarıyla anlatılabilir.

## <a name="languages"></a>Diller

Sürüm 2.x ile başlayarak, çalışma zamanı bir dil genişletilebilirlik modeli kullanır ve bir işlev uygulamasındaki tüm işlevler aynı dili paylaşmalıdır. Bir işlev uygulamasındaki işlevlerin dili, uygulama oluşturulurken seçilir ve [İşLEVLER\_İşÇİ\_RUNTIME](functions-app-settings.md#functions_worker_runtime) ayarında tutulur. 

Azure İşlevler 1.x deneysel dilleri yeni modeli kullanamaz, bu nedenle 2.x olarak desteklenmez. Aşağıdaki tablo, her çalışma zamanı sürümünde hangi programlama dillerinin şu anda desteklenmediğini gösterir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Daha fazla bilgi için bkz. [Desteklenen diller](supported-languages.md).

## <a name="run-on-a-specific-version"></a><a name="creating-1x-apps"></a>Belirli bir sürümde çalıştırma

Varsayılan olarak, Azure portalında ve Azure CLI tarafından oluşturulan işlev uygulamaları sürüm 3.x olarak ayarlanır. Bu sürümü gerektiği gibi değiştirebilirsiniz. İşlev uygulamanızı oluşturduktan sonra ancak herhangi bir işlev eklemeden önce çalışma zamanı sürümünü yalnızca 1.x olarak değiştirebilirsiniz.  2.x ve 3.x arasında hareket bile işlevleri olan uygulamalar ile izin verilir, ancak yine de ilk yeni bir uygulamada test etmek için tavsiye edilir.

## <a name="migrating-from-1x-to-later-versions"></a>1.x'ten sonraki sürümlere geçiş

Daha yeni bir sürümü kullanmak için sürüm 1.x çalışma zamanını kullanmak üzere yazılmış varolan bir uygulamayı geçirebilirsiniz. Yapmanız gereken değişikliklerin çoğu,.NET Framework 4.7 ve .NET Core arasındaki C# API değişiklikleri gibi dil çalışma zamanındaki değişikliklerle ilgilidir. Ayrıca, kodlarınızın ve kitaplıklarınızın seçtiğiniz dil çalışma süresiyle uyumlu olduğundan emin olmanız gerekir. Son olarak, tetikleyici, bağlama ve aşağıda vurgulanan özelliklerdeki değişiklikleri not aldığınızda dikkat edin. En iyi geçiş sonuçları için, yeni bir sürümde yeni bir işlev uygulaması oluşturmalı ve mevcut sürüm 1.x işlev kodunuzu yeni uygulamaya taşımanız gerekir.  

Uygulama yapılandırmasını el ile güncelleyerek "yerinde" yükseltme yapmak mümkün olsa da, 1.x'ten daha yüksek bir sürüme geçmek bazı kırılma değişiklikleri içerir. Örneğin, C#'da hata ayıklama nesnesi `TraceWriter` `ILogger`. Yeni bir sürüm 3.x projesi oluşturarak, en son sürüm 3.x şablonlarını temel alan güncelleştirilmiş işlevlerle başlarsınız.

### <a name="changes-in-triggers-and-bindings-after-version-1x"></a>Sürüm 1.x'ten sonra tetikleyiciler ve bağlamalarda değişiklikler

Sürüm 2.x ile başlayarak, uygulamanızdaki işlevler tarafından kullanılan belirli tetikleyiciler ve bağlamalar için uzantıları yüklemeniz gerekir. Bu HTTP ve zamanlayıcı tetikleyicileri için tek istisna, bir uzantısı gerektirmez.  Daha fazla bilgi için [bkz.](./functions-bindings-register.md)

Ayrıca *işlev.json* veya sürümler arasında işlevin öznitelikleri birkaç değişiklik vardır. Örneğin, Olay Hub `path` özelliği `eventHubName`şimdi. Her bağlama için belgelere bağlantılar için [varolan bağlama tablosuna](#bindings) bakın.

### <a name="changes-in-features-and-functionality-after-version-1x"></a>Sürüm 1.x'ten sonra özelliklerde ve işlevlerde değişiklikler

Sürüm 1.x'ten sonra birkaç özellik kaldırıldı, güncelleştirildi veya değiştirildi. Bu bölümde, sürüm 1.x kullanıldıktan sonra sonraki sürümlerde gördüğünüz değişiklikler ayrıntılı olarak anlatIlir.

Sürüm 2.x'te aşağıdaki değişiklikler yapılmıştır:

* HTTP uç noktalarını aramak için anahtarlar her zaman Azure Blob depolama alanında şifrelenmiş olarak depolanır. Sürüm 1.x'te, anahtarlar varsayılan olarak Azure Dosyası depolama alanında depolandı. Bir uygulamayı sürüm 1.x'ten sürüm 2.x'e yükseltilirken, dosya depolamada bulunan varolan sırlar sıfırlanır.

* Sürüm 2.x çalışma süresi webhook sağlayıcıları için yerleşik destek içermez. Bu değişiklik performansı artırmak için yapıldı. Yine de webhooks için uç nokta olarak HTTP tetikleyicileri kullanabilirsiniz.

* Ana bilgisayar yapılandırma dosyası (ana bilgisayar.json) `"version": "2.0"`boş olmalı veya dize olmalıdır.

* İzlemeyi iyileştirmek için, [`AzureWebJobsDashboard`](functions-app-settings.md#azurewebjobsdashboard) bu ayarı kullanan portaldaki Web İşler panosu, [`APPINSIGHTS_INSTRUMENTATIONKEY`](functions-app-settings.md#appinsights_instrumentationkey) ayarı kullanan Azure Uygulama Öngörüleri ile değiştirilir. Daha fazla bilgi için [bkz.](functions-monitoring.md)

* Bir işlev uygulamasındaki tüm işlevler aynı dili paylaşmalıdır. Bir işlev uygulaması oluşturduğunuzda, uygulama için bir çalışma zamanı yığını seçmeniz gerekir. Çalışma zamanı yığını, uygulama [`FUNCTIONS_WORKER_RUNTIME`](functions-app-settings.md#functions_worker_runtime) ayarlarındaki değerle belirtilir. Bu gereksinim, ayak izini ve başlangıç süresini iyileştirmek için eklenmiştir. Yerel olarak geliştirirken, bu ayarı [local.settings.json dosyasına](functions-run-local.md#local-settings-file)da eklemeniz gerekir.

* Uygulama Hizmeti planındaki işlevler için varsayılan zaman dilimi 30 dakika olarak değiştirilir. Host.json'daki [timeout işlevini](functions-host-json.md#functiontimeout) kullanarak zaman arasını sınırsıza manuel olarak değiştirebilirsiniz.

* HTTP eşzamanlı lık azaltmaları, tüketim planı işlevleri için varsayılan olarak, her örnek için 100 eşzamanlı istek varsayılan olarak uygulanır. Ana bilgisayar.json [`maxConcurrentRequests`](functions-host-json.md#http) dosyasındaki ayarda bunu değiştirebilirsiniz.

* [.NET Core sınırlamaları](https://github.com/Azure/azure-functions-host/issues/3414)nedeniyle, F# komut dosyası (.fsx) işlevleri için destek kaldırıldı. Derlenmiş F# işlevleri (.fs) hala desteklenir.

* Olay Izgara tetikleyici webhooks'un `https://{app}/runtime/webhooks/{triggerName}`URL biçimi ' olarak değiştirildi.

## <a name="migrating-from-2x-to-3x"></a>2.x'ten 3.x'e geçiş

Azure İşlevler sürüm 3.x, sürüm 2.x ile son derece geriye dönük olarak uyumludur.  Birçok uygulama, herhangi bir kod değişikliği olmadan güvenli bir şekilde 3.x'e yükseltilmelidir.  3.x'e geçmek teşvik edilirken, üretim uygulamalarında ana sürümü değiştirmeden önce kapsamlı testler çalıştırdığından emin olun.

### <a name="breaking-changes-between-2x-and-3x"></a>2.x ve 3.x arasındaki kırılma lar

2.x uygulamasını 3.x'e yükseltmeden önce dikkat edilmesi gereken değişiklikler aşağıda verilmiştir.

#### <a name="javascript"></a>JavaScript

* Üzerinden `context.done` atanan çıktı bağlamaları veya iade değerleri artık `context.bindings`''deki ayarı ile aynı şekilde

* Zamanlayıcı tetikleme nesnesi PascalCase yerine camelCase'dir

* Olay Hub ikili `dataType` ile işlevleri tetiklenen `binary` yerine `string`bir dizi alırsınız.

* HTTP istek yüküne artık .' `context.bindingData.req`  Yine de bir giriş parametresi `context.req`olarak erişilebilir `context.bindings`ve içinde .

* Düğüm.js 8 artık desteklenmez ve 3.x işlevlerinde yürütülmez.

#### <a name="net"></a>.NET

* [Senkron sunucu işlemleri varsayılan olarak devre dışı bırakılır.](https://docs.microsoft.com/dotnet/core/compatibility/2.2-3.0#http-synchronous-io-disabled-in-all-servers)

### <a name="changing-version-of-apps-in-azure"></a>Azure'da uygulamaların sürümünü değiştirme

Azure'da yayınlanan uygulamalar tarafından kullanılan İşlevler çalışma [`FUNCTIONS_EXTENSION_VERSION`](functions-app-settings.md#functions_extension_version) zamanı sürümü, uygulama ayarı tarafından belirlenir. Aşağıdaki ana çalışma zamanı sürüm değerleri desteklenir:

| Değer | Çalışma zamanı hedefi |
| ------ | -------- |
| `~3` | 3.x |
| `~2` | 2.x |
| `~1` | 1.x |

>[!IMPORTANT]
> İşlev kodunuzda değişiklik ve değişiklik gerekebileceğinden, bu ayarı keyfi olarak değiştirmeyin.

### <a name="locally-developed-application-versions"></a>Yerel olarak geliştirilmiş uygulama sürümleri

Hedeflenen sürümleri yerel olarak değiştirmek için işlev uygulamaları için aşağıdaki güncelleştirmeleri yapabilirsiniz.

#### <a name="visual-studio-runtime-versions"></a>Visual Studio çalışma zamanı sürümleri

Visual Studio'da, bir proje oluştururken çalışma zamanı sürümünü seçersiniz. Visual Studio için Azure İşlevler araçları üç ana çalışma zamanı sürümü destekler. Doğru sürüm, proje ayarlarına göre hata ayıklama ve yayımlama kullanılır. Sürüm ayarları `.csproj` aşağıdaki özelliklerde dosyada tanımlanır:

##### <a name="version-1x"></a>Sürüm 1.x

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

##### <a name="version-2x"></a>Sürüm 2.x

```xml
<TargetFramework>netcoreapp2.1</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

##### <a name="version-3x"></a>Sürüm 3.x

```xml
<TargetFramework>netcoreapp3.1</TargetFramework>
<AzureFunctionsVersion>v3</AzureFunctionsVersion>
```

> [!NOTE]
> Azure İşlevler 3.x `Microsoft.NET.Sdk.Functions` ve .NET `3.0.0`uzantısı en az olmasını gerektirir.

###### <a name="updating-2x-apps-to-3x-in-visual-studio"></a>Visual Studio'da 2.x uygulamaları 3.x'e güncelleme

`.csproj` Dosyayı düzenleyerek ve yukarıdaki değerleri güncelleyerek 2.x'i hedefleyen varolan bir işlevi açabilir ve 3.x'e geçebilirsiniz.  Visual Studio, proje meta verilerine dayalı olarak çalışma zamanı sürümlerini sizin için otomatik olarak yönetir.  Ancak, Visual Studio'nun henüz makinenizde 3.x için şablonları ve çalışma süresi ne kadar yoksa, daha önce hiç 3.x uygulaması oluşturmamışsanız mümkündür.  Bu, kendisini "projede belirtilen sürümle eşleşen işlevler çalışma süresi yok" gibi bir hatayla gösterebilir.  En son şablonları ve çalışma süresini almak için, yeni bir işlev projesi oluşturmak için deneyimi gözden geçirin.  Sürüm ve şablon seçin ekranına ulaştığınızda, Visual Studio'nun en son şablonları almayı tamamlamasını bekleyin.  En son .NET Core 3 şablonları kullanılabilir ve görüntülendiğinde, sürüm 3.x için yapılandırılan herhangi bir projeyi çalıştırıp hata ayıklamanız gerekir.

> [!IMPORTANT]
> Sürüm 3.x işlevleri yalnızca Visual Studio sürüm 16.4 veya daha yeni kullanılarak Visual Studio'da geliştirilebilir.

#### <a name="vs-code-and-azure-functions-core-tools"></a>VS Kodu ve Azure Fonksiyonları Temel Araçları

[Azure İşlevler Çekirdek Araçları](functions-run-local.md) komut satırı geliştirme ve Visual Studio Code için [Azure İşlevler uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) için kullanılır. Sürüm 3.x'e karşı geliştirmek için Core Tools'un 3.x sürümünü yükleyin. Sürüm 2.x geliştirme, Çekirdek Araçları sürümü 2.x gerektirir, ve benzeri. Daha fazla bilgi için Azure [İşlevleri Temel Araçlarını Yükle'ye](functions-run-local.md#install-the-azure-functions-core-tools)bakın.

Visual Studio Code geliştirme için, yüklenen araçların sürümüyle `azureFunctions.projectRuntime` eşleşebilmek için kullanıcı ayarını güncelleştirmeniz de gerekebilir.  Bu ayar, işlev uygulaması oluşturma sırasında kullanılan şablonları ve dilleri de güncelleştirir.  `~3` Uygulamaları oluşturmak için `~3`kullanıcı ayarını `azureFunctions.projectRuntime` güncelleştirecek.

![Azure İşlevler uzantısı çalışma zamanı ayarı](./media/functions-versions/vs-code-version-runtime.png)

#### <a name="maven-and-java-apps"></a>Maven ve Java uygulamaları

Java uygulamalarını yerel olarak çalıştırmak için gereken temel [araçların 3.x sürümünü yükleyerek](functions-run-local.md#install-the-azure-functions-core-tools) sürüm 2.x'ten 3.x'e geçirebilirsiniz.  Uygulamanızın sürüm 3.x'te yerel olarak düzgün çalıştığını doğruladıktan sonra, `POM.xml` `FUNCTIONS_EXTENSION_VERSION` aşağıdaki örnekte `~3`olduğu gibi ayarı değiştirmek için uygulamanın dosyasını güncelleyin:

```xml
<configuration>
    <resourceGroup>${functionResourceGroup}</resourceGroup>
    <appName>${functionAppName}</appName>
    <region>${functionAppRegion}</region>
    <appSettings>
        <property>
            <name>WEBSITE_RUN_FROM_PACKAGE</name>
            <value>1</value>
        </property>
        <property>
            <name>FUNCTIONS_EXTENSION_VERSION</name>
            <value>~3</value>
        </property>
    </appSettings>
</configuration>
```

## <a name="bindings"></a>Bağlamalar

Sürüm 2.x ile başlayarak, çalışma zamanı şu avantajları sunan yeni bir [bağlayıcı genişletilebilirlik modeli](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) kullanır:

* Üçüncü taraf bağlama uzantıları için destek.

* Çalışma zamanı ve bağlamaların ayrışma. Bu değişiklik, bağlama uzantılarının bağımsız olarak sürülmesine ve serbest bırakılmasına olanak tanır. Örneğin, altta yatan bir SDK'nın daha yeni bir sürümüne dayanan bir uzantının sürümüne yükseltmeyi tercih edebilirsiniz.

* Yalnızca kullanılan bağlamaların çalışma süresine göre bilindiği ve yüklendiği daha hafif bir yürütme ortamı.

HTTP ve zamanlayıcı tetikleyicileri dışında, tüm bağlamaların işlev uygulaması projesine açıkça eklenmesi veya portala kaydedilmesi gerekir. Daha fazla bilgi için [bkz.](./functions-bindings-expressions-patterns.md)

Aşağıdaki tablo, her çalışma zamanı sürümünde hangi bağlamaların desteklenildiği gösterilmektedir.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-run-local.md)
* [Azure İşlevleri çalışma zamanı sürümlerini hedefleme](set-runtime-version.md)
* [Sürüm notları](https://github.com/Azure/azure-functions-host/releases)
