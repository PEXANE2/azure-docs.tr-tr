---
title: Azure Işlevleri çalışma zamanı sürümlerine genel bakış
description: Azure Işlevleri, çalışma zamanının birden çok sürümünü destekler. Aralarındaki farkları ve sizin için doğru olanı seçme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 12/09/2019
ms.openlocfilehash: e90752e89be7e381b06f8a87f76f123f0e4a8e3a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80422490"
---
# <a name="azure-functions-runtime-versions-overview"></a>Azure Işlevleri çalışma zamanı sürümlerine genel bakış

Azure Işlevleri çalışma zamanının ana sürümleri, çalışma zamanının temel aldığı .NET sürümü ile ilgilidir. Aşağıdaki tabloda, çalışma zamanının geçerli sürümü, sürüm düzeyi ve ilgili .NET sürümü gösterilmektedir. 

| Çalışma zamanı sürümü | Yayın düzeyi<sup>1</sup> | .NET sürümü | 
| --------------- | ------------- | ------------ |
| 3.x | GA | .NET Core 3,1 | 
| 2.x | GA | .NET Core 2.2 |
| 'in | GA<sup>2</sup> | .NET Framework 4.7.2<sup>3</sup> |

<sup>1</sup> GA sürüm, üretim senaryolarında desteklenir.   
<sup>2</sup> sürüm 1. x bakım modunda. Geliştirmeler yalnızca sonraki sürümlerde sağlanır.   
<sup>3</sup> yalnızca Azure Portal veya Windows bilgisayarlarda yerel olarak geliştirme desteklenir.

Bu makalede çeşitli sürümler, her sürümü nasıl oluşturabileceğiniz ve sürümlerin nasıl değiştirileceği hakkında bazı farklılıklar açıklanır.

## <a name="languages"></a>Diller

Sürüm 2. x ile başlayarak, çalışma zamanı bir dil genişletilebilirlik modeli kullanır ve bir işlev uygulamasındaki tüm işlevler aynı dili paylaşmalıdır. İşlev uygulamasındaki işlevlerin dili, uygulama oluşturulurken seçilir ve [işlevler\_\_çalışan çalışma zamanı](functions-app-settings.md#functions_worker_runtime) ayarında korunur. 

Azure Işlevleri 1. x deneysel dilleri yeni modeli kullanamaz, bu nedenle 2. x içinde desteklenmez. Aşağıdaki tablo, her çalışma zamanı sürümünde hangi programlama dillerinin desteklendiğini gösterir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Daha fazla bilgi için bkz. [Desteklenen diller](supported-languages.md).

## <a name="run-on-a-specific-version"></a><a name="creating-1x-apps"></a>Belirli bir sürümde Çalıştır

Varsayılan olarak, Azure portal ve Azure CLı tarafından oluşturulan işlev uygulamaları sürüm 3. x olarak ayarlanır. Bu sürümü gerektiği gibi değiştirebilirsiniz. İşlev uygulamanızı oluşturduktan sonra, ancak herhangi bir işlev eklemeden önce çalışma zamanı sürümünü 1. x olarak değiştirebilirsiniz.  2. x ve 3. x arasında çalışmaya, işlevleri olan uygulamalarla bile izin verilir, ancak önce yeni bir uygulamada test etmek önerilir.

## <a name="migrating-from-1x-to-later-versions"></a>1. x 'den sonraki sürümlere geçiş

Sürüm 1. x çalışma zamanını kullanmak üzere yazılmış mevcut bir uygulamayı bunun yerine daha yeni bir sürümü kullanmak üzere geçirmeyi tercih edebilirsiniz. Yapmanız gereken değişikliklerin çoğu, dil çalışma zamanındaki değişikliklerle ilgilidir, örneğin .NET Framework 4,7 ve .NET Core arasında C# API 'SI değişiklikleri. Ayrıca, kodunuzun ve kitaplıklarınızın seçtiğiniz dil çalışma zamanıyla uyumlu olduğundan emin olmanız gerekir. Son olarak, aşağıda vurgulanan tetikleyici, bağlamalar ve özelliklerde değişiklik yaptığınızdan emin olun. En iyi geçiş sonuçları için yeni bir sürümde yeni bir işlev uygulaması oluşturmanız ve var olan sürüm 1. x işlev kodunuzun yeni uygulamaya bağlantı noktası oluşturmanız gerekir.  

Uygulama yapılandırmasını el ile güncelleştirerek bir "yerinde" yükseltme yapmak mümkün olsa da, 1. x sürümünden daha yüksek bir sürüme geçmek bazı önemli değişiklikler içerir. Örneğin, C# ' de, hata ayıklama nesnesi ' dan `TraceWriter` ' a `ILogger`değiştirilir. Yeni bir sürüm 3. x projesi oluşturarak, en son sürüm 3. x şablonlarına göre güncelleştirilmiş işlevlerle başlayabilirsiniz.

### <a name="changes-in-triggers-and-bindings-after-version-1x"></a>Tetikleyiciler ve bağlamalardaki değişiklikler 1. x sürümünden sonra

Sürüm 2. x ile başlayarak, uygulamanızdaki işlevler tarafından kullanılan belirli Tetikleyiciler ve bağlamalar için uzantıları yüklemelisiniz. Bu HTTP ve Zamanlayıcı Tetikleyicileri için uzantı gerektirmeyen tek özel durum.  Daha fazla bilgi için bkz. [bağlama uzantılarını kaydetme ve yüklemeyi bağlama](./functions-bindings-register.md).

Ayrıca, *function. JSON* veya özellikler arasındaki işlevin öznitelikleri için birkaç değişiklik de vardır. Örneğin, Olay Hub `path` 'ı özelliği şu anda. `eventHubName` Her bağlamaya yönelik belgelerin bağlantıları için [mevcut bağlama tablosuna](#bindings) bakın.

### <a name="changes-in-features-and-functionality-after-version-1x"></a>Sürüm 1. x ' den sonraki özelliklerde ve işlevlerde yapılan değişiklikler

Sürüm 1. x 'ten sonra bazı özellikler kaldırıldı, güncelleştirildi veya değiştirildi. Bu bölüm, sürüm 1. x kullandıktan sonra sonraki sürümlerde gördüğünüz değişiklikleri ayrıntılı olarak ayrıntılardır.

2. x sürümünde aşağıdaki değişiklikler yapılmıştır:

* HTTP uç noktalarını çağırma anahtarları, her zaman Azure Blob depolamada şifrelenir. 1. x sürümünde, anahtarlar varsayılan olarak Azure dosya depolama alanında depolanmıştı. Bir uygulamayı 1. x sürümünden sürüm 2. x ' e yükseltirken, dosya depolamada bulunan mevcut gizlilikler sıfırlanır.

* Sürüm 2. x çalışma zamanı, Web kancası sağlayıcıları için yerleşik destek içermez. Bu değişiklik performansı artırmak için yapılmıştır. HTTP tetikleyicilerini Web kancaları için uç nokta olarak kullanmaya devam edebilirsiniz.

* Ana bilgisayar yapılandırma dosyası (Host. JSON) boş olmalıdır veya dize `"version": "2.0"`olmalıdır.

* İzlemeyi geliştirmek için, bu [`AzureWebJobsDashboard`](functions-app-settings.md#azurewebjobsdashboard) ayarı kullanan portaldaki Web İşleri Panosu, [`APPINSIGHTS_INSTRUMENTATIONKEY`](functions-app-settings.md#appinsights_instrumentationkey) ayarı kullanılan Azure Application Insights ile değiştirilmiştir. Daha fazla bilgi için bkz. [Azure Işlevlerini izleme](functions-monitoring.md).

* Bir işlev uygulamasındaki tüm işlevler aynı dili paylaşmalıdır. Bir işlev uygulaması oluşturduğunuzda, uygulama için bir çalışma zamanı yığını seçmeniz gerekir. Çalışma zamanı yığını, uygulama ayarlarındaki [`FUNCTIONS_WORKER_RUNTIME`](functions-app-settings.md#functions_worker_runtime) değerle belirtilir. Bu gereksinim, parmak izini ve başlangıç süresini artırmak için eklenmiştir. Yerel olarak geliştirilirken, bu ayarı [Local. Settings. JSON dosyasına](functions-run-local.md#local-settings-file)da dahil etmeniz gerekir.

* Bir App Service planındaki işlevler için varsayılan zaman aşımı 30 dakikaya dönüştürülür. Host. json ' daki [functiontimeout](functions-host-json.md#functiontimeout) ayarını kullanarak, zaman aşımını tekrar sınırsız olarak değiştirebilirsiniz.

* HTTP eşzamanlılık kısıtlılığı, örnek başına 100 eşzamanlı istek içeren tüketim planı işlevleri için varsayılan olarak uygulanır. Bunu, Host. JSON dosyasındaki [`maxConcurrentRequests`](functions-host-json.md#http) ayarda değiştirebilirsiniz.

* [.NET Core sınırlamaları](https://github.com/Azure/azure-functions-host/issues/3414)nedeniyle F # Script (. FSX) işlevleri için destek kaldırılmıştır. Derlenen F # işlevleri (. FS) hala desteklenmektedir.

* Event Grid tetikleyicisi Web kancalarının URL biçimi olarak `https://{app}/runtime/webhooks/{triggerName}`değiştirildi.

## <a name="migrating-from-2x-to-3x"></a>2. x ile 3. x arasında geçiş

Azure Işlevleri sürüm 3. x, sürüm 2. x ile yüksek oranda geriye dönük olarak uyumludur.  Birçok uygulama herhangi bir kod değişikliği yapmadan 3. x 'e güvenli bir şekilde yükseltebilmelidir.  3. x ' e geçiş yaparken, üretim uygulamalarındaki ana sürümü değiştirmeden önce kapsamlı testler çalıştırdığınızdan emin olun.

### <a name="breaking-changes-between-2x-and-3x"></a>2. x ve 3. x arasındaki son değişiklikler

Bir 2. x uygulamasını 3. x ' e yükseltmeden önce dikkat edilecek değişiklikler aşağıda verilmiştir.

#### <a name="javascript"></a>JavaScript

* Veya dönüş değerleri aracılığıyla `context.done` atanan çıkış bağlamaları artık ' de `context.bindings`ayarıyla aynı şekilde davranır.

* Zamanlayıcı tetikleyici nesnesi PascalCase yerine camelCase

* Olay Hub 'ı ile `dataType` tetiklenen işlevler, `binary` yerine bir dizisi alır. `string`

* HTTP istek yüküne artık aracılığıyla `context.bindingData.req`erişilemez.  Bir giriş parametresi `context.req`olarak, ve ' de `context.bindings`erişilebilir olmaya devam edebilir.

* Node. js 8 artık desteklenmemektedir ve 3. x işlevlerinde yürütülecektir.

#### <a name="net"></a>.NET

* [Zaman uyumlu sunucu işlemleri varsayılan olarak devre dışıdır](https://docs.microsoft.com/dotnet/core/compatibility/2.2-3.0#http-synchronous-io-disabled-in-all-servers).

### <a name="changing-version-of-apps-in-azure"></a>Azure 'da uygulamaların sürümünü değiştirme

Azure 'da yayımlanan uygulamalar tarafından kullanılan Işlevlerin çalışma zamanının sürümü [`FUNCTIONS_EXTENSION_VERSION`](functions-app-settings.md#functions_extension_version) uygulama ayarı tarafından belirlenir. Aşağıdaki büyük çalışma zamanı sürümü değerleri desteklenir:

| Değer | Çalışma zamanı hedefi |
| ------ | -------- |
| `~3` | 3.x |
| `~2` | 2.x |
| `~1` | 'in |

>[!IMPORTANT]
> Bu ayarı rastgele değiştirmeyin, çünkü diğer uygulama ayarı değişiklik ve işlev kodunuzda yapılan değişiklikler gerekebilir.

### <a name="locally-developed-application-versions"></a>Yerel olarak geliştirilen uygulama sürümleri

Hedeflenen sürümleri yerel olarak değiştirmek için uygulama işlevleri işlevine aşağıdaki güncelleştirmeleri yapabilirsiniz.

#### <a name="visual-studio-runtime-versions"></a>Visual Studio çalışma zamanı sürümleri

Visual Studio 'da, bir proje oluştururken çalışma zamanı sürümünü seçersiniz. Visual Studio için Azure Işlevleri araçları, üç ana çalışma zamanı sürümünü destekler. Hata ayıklama sırasında ve proje ayarlarına bağlı olarak yayımlandığında doğru sürüm kullanılır. Sürüm ayarları, `.csproj` dosyasında aşağıdaki özelliklerde tanımlanmıştır:

##### <a name="version-1x"></a>Sürüm 1. x

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

##### <a name="version-2x"></a>Sürüm 2. x

```xml
<TargetFramework>netcoreapp2.1</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

##### <a name="version-3x"></a>Sürüm 3. x

```xml
<TargetFramework>netcoreapp3.1</TargetFramework>
<AzureFunctionsVersion>v3</AzureFunctionsVersion>
```

> [!NOTE]
> Azure Işlevleri 3. x ve .NET, `Microsoft.NET.Sdk.Functions` uzantının en az `3.0.0`olmasını gerektirir.

###### <a name="updating-2x-apps-to-3x-in-visual-studio"></a>Visual Studio 'da 2. x uygulamalarını 3. x olarak güncelleştirme

2. x hedefleme var olan bir işlevi açabilir ve `.csproj` dosyayı düzenleyerek ve yukarıdaki değerleri güncelleştirerek 3. x ' e geçebilirsiniz.  Visual Studio, çalışma zamanı sürümlerini proje meta verilerine göre otomatik olarak yönetir.  Ancak, Visual Studio 'Nun makinenizde 3. x için şablonlar ve çalışma zamanına sahip olmaması durumunda henüz bir 3. x uygulaması oluşturmadıysanız, bu mümkündür.  Bu, "projede belirtilen sürümle eşleşen Işlevler çalışma zamanı yok" gibi bir hatayla kendi kendine sunabilir.  En son şablonları ve çalışma zamanını getirmek için yeni bir işlev projesi oluşturma deneyiminden yararlanın.  Sürüm ve şablon seç ekran ' e geldiğinizde, Visual Studio 'nun en son şablonları getirme işleminin tamamlanmasını bekleyin.  En son .NET Core 3 şablonları kullanılabilir ve görüntülendikten sonra, sürüm 3. x için yapılandırılmış herhangi bir projeyi çalıştırabilmeniz ve hata ayıklaması yapmanız gerekir.

> [!IMPORTANT]
> Sürüm 3. x işlevleri yalnızca Visual Studio sürüm 16,4 veya daha yeni bir sürümde kullanılıyorsa Visual Studio 'da geliştirilebilir.

#### <a name="vs-code-and-azure-functions-core-tools"></a>VS Code ve Azure Functions Core Tools

[Azure Functions Core Tools](functions-run-local.md) , komut satırı geliştirme ve ayrıca Visual Studio Code Için [Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) tarafından kullanılır. Sürüm 3. x ' e karşı geliştirmek için çekirdek araçların 3. x sürümünü yükler. Sürüm 2. x geliştirme temel araçların sürüm 2. x 'i gerektirir ve bu şekilde devam eder. Daha fazla bilgi için bkz. [Azure Functions Core Tools yüklemesi](functions-run-local.md#install-the-azure-functions-core-tools).

Visual Studio Code geliştirmek için, için Kullanıcı ayarını, yüklü araçların sürümüyle eşleşecek `azureFunctions.projectRuntime` şekilde güncelleştirmeniz de gerekebilir.  Bu ayar, işlev uygulaması oluşturma sırasında kullanılan şablonları ve dilleri de güncelleştirir.  ' De `~3` uygulama oluşturmak için `azureFunctions.projectRuntime` Kullanıcı ayarını olarak `~3`güncelleştirebilirsiniz.

![Azure Işlevleri uzantısı çalışma zamanı ayarı](./media/functions-versions/vs-code-version-runtime.png)

#### <a name="maven-and-java-apps"></a>Maven ve Java uygulamaları

Yerel olarak çalıştırmak için gereken [çekirdek araçların 3. x sürümünü yükleyerek](functions-run-local.md#install-the-azure-functions-core-tools) , Java uygulamalarını 2. x sürümünden 3. x ' e geçirebilirsiniz.  Uygulamanızın, sürüm 3. x üzerinde yerel olarak çalışır şekilde çalıştığını doğruladıktan sonra, aşağıdaki örnekte olduğu gibi `POM.xml` , `FUNCTIONS_EXTENSION_VERSION` ayarını `~3`olarak değiştirmek için uygulamanın dosyasını güncelleştirin:

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

Çalışma zamanı, sürüm 2. x ile başlayarak bu avantajları sunan yeni bir [bağlama genişletilebilirlik modeli](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) kullanır:

* Üçüncü taraf bağlama uzantıları için destek.

* Çalışma zamanının ve bağlamaların ayrılması. Bu değişiklik, bağlama uzantılarının bağımsız olarak yayınlanabilmesini ve serbest bırakılacağını sağlar. Örneğin, temel bir SDK 'nın daha yeni bir sürümüne bağımlı olan bir uzantının sürümüne yükseltmeyi tercih edebilirsiniz.

* Yalnızca kullanımdaki bağlamaların bilinen ve çalışma zamanı tarafından yüklendiği, daha hafif bir yürütme ortamıdır.

HTTP ve Zamanlayıcı Tetikleyicileri hariç olmak üzere tüm bağlamalar, işlev uygulaması projesine açıkça eklenmelidir veya portalda kayıtlı olmalıdır. Daha fazla bilgi için bkz. [bağlama uzantılarını kaydetme](./functions-bindings-expressions-patterns.md).

Aşağıdaki tabloda, her çalışma zamanı sürümünde hangi bağlamaların desteklendiği gösterilmektedir.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-run-local.md)
* [Azure Işlevleri çalışma zamanı sürümlerini hedefleme](set-runtime-version.md)
* [Sürüm notları](https://github.com/Azure/azure-functions-host/releases)
