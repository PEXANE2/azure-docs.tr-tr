---
title: Azure Fonksiyonları için PowerShell geliştirici başvurusu
description: PowerShell'i kullanarak işlevlerinasıl geliştireceklerini anlayın.
author: eamonoreilly
ms.topic: conceptual
ms.date: 04/22/2019
ms.openlocfilehash: 41f977e7e7c23c2f49fd656461b7a3920802997e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276744"
---
# <a name="azure-functions-powershell-developer-guide"></a>Azure İşgücü Shell geliştirici kılavuzu

Bu makalede, PowerShell kullanarak Azure İşlevlerini nasıl yazdığınız hakkında ayrıntılı bilgi verilmektedir.

PowerShell Azure işlevi (işlevi), tetiklendiğinde çalıştıran bir PowerShell komut dosyası olarak temsil edilir. Her işlev komut `function.json` dosyası, işlevin nasıl tetiklendirdiği ve giriş ve çıkış parametreleri gibi nasıl çalıştığını tanımlayan ilgili bir dosyaya sahiptir. Daha fazla bilgi için [Tetikleyiciler ve bağlayıcı makaleye](functions-triggers-bindings.md)bakın. 

Diğer işlev türleri gibi PowerShell komut dosyası işlevleri de `function.json` dosyada tanımlanan tüm giriş bağlamalarının adlarıyla eşleşen parametreleri alır. İşlevi başlatan tetikleyiciüzerinde ek bilgiler içeren bir `TriggerMetadata` parametre de geçirilir.

Bu makalede, [Azure İşlevler geliştirici başvurularını](functions-reference.md)zaten okuduğunuz varsayar. Ayrıca, ilk PowerShell işlevinizi oluşturmak [için PowerShell'in hızlı başlatma fonksiyonlarını](functions-create-first-function-powershell.md) da tamamlamış olmalısınız.

## <a name="folder-structure"></a>Klasör yapısı

PowerShell projesi için gerekli klasör yapısı aşağıdaki gibi görünür. Bu varsayılan değiştirilebilir. Daha fazla bilgi için aşağıdaki [scriptFile](#configure-function-scriptfile) bölümüne bakın.

```
PSFunctionApp
 | - MyFirstFunction
 | | - run.ps1
 | | - function.json
 | - MySecondFunction
 | | - run.ps1
 | | - function.json
 | - Modules
 | | - myFirstHelperModule
 | | | - myFirstHelperModule.psd1
 | | | - myFirstHelperModule.psm1
 | | - mySecondHelperModule
 | | | - mySecondHelperModule.psd1
 | | | - mySecondHelperModule.psm1
 | - local.settings.json
 | - host.json
 | - requirements.psd1
 | - profile.ps1
 | - extensions.csproj
 | - bin
```

Projenin temelinde, işlev uygulamasını yapılandırmak [`host.json`](functions-host-json.md) için kullanılabilecek paylaşılan bir dosya vardır. Her işlevin kendi kod dosyası (.ps1) ve bağlama`function.json`yapılandırma dosyası () olan bir klasörü vardır. function.json dosyasının üst dizininin adı her zaman işlevinizin adıdır.

Belirli bağlamalar bir `extensions.csproj` dosyanın varlığını gerektirir. İşlevler çalışma zamanı [nın sürüm 2.x ve sonraki sürümlerinde](functions-versions.md) `extensions.csproj` gerekli olan bağlama uzantıları, `bin` dosyada klasördeki gerçek kitaplık dosyalarıyla birlikte tanımlanır. Yerel olarak geliştirirken, [bağlama uzantılarını kaydetmeniz](functions-bindings-register.md#extension-bundles)gerekir. Azure portalında işlevler geliştirirken, bu kayıt sizin için yapılır.

PowerShell İşlev Uygulamaları'nda, isteğe bağlı olarak bir işlev uygulaması çalışmaya başladığında çalışan bir uygulamanız `profile.ps1` olabilir (aksi takdirde soğuk bir *[başlangıç](#cold-start)* olarak bilin. Daha fazla bilgi için [PowerShell profiline](#powershell-profile)bakın.

## <a name="defining-a-powershell-script-as-a-function"></a>PowerShell komut dosyasını işlev olarak tanımlama

Varsayılan olarak, İşlevler çalışma zamanı, `run.ps1` ilgili ile aynı ana dizinin `function.json`bulunduğu `run.ps1`işlevinizi görünür.

Komut dosyanız yürütme yle ilgili bir dizi bağımsız değişkenden geçirilir. Bu parametreleri işlemek `param` için, aşağıdaki örnekte olduğu gibi komut dosyanızın üstüne bir blok ekleyin:

```powershell
# $TriggerMetadata is optional here. If you don't need it, you can safely remove it from the param block
param($MyFirstInputBinding, $MySecondInputBinding, $TriggerMetadata)
```

### <a name="triggermetadata-parameter"></a>TriggerMetaveri parametresi

`TriggerMetadata` Parametre tetikleyici hakkında ek bilgi sağlamak için kullanılır. Ek meta veriler bağlamadan bağlamaya kadar değişir, ancak bunların tümü aşağıdaki verileri içeren bir `sys` özellik içerir:

```powershell
$TriggerMetadata.sys
```

| Özellik   | Açıklama                                     | Tür     |
|------------|-------------------------------------------------|----------|
| UtcNow     | UTC'de işlev tetiklendiğinde        | DateTime |
| Methodname | Tetiklenen İşlevin adı     | string   |
| RandGuid   | fonksiyonun bu yürütülmesi için benzersiz bir yol gösterici | string   |

Her tetikleyici türünde farklı bir meta veri kümesi vardır. `$TriggerMetadata` Örneğin, diğer `QueueTrigger` şeylerin `InsertionTime`yanı `Id` `DequeueCount`sıra , , içerir. Sıra tetikleyicisinin meta verileri hakkında daha fazla bilgi [için, sıra tetikleyicileri için resmi belgelere](functions-bindings-storage-queue-trigger.md#message-metadata)gidin. Tetikleyici meta verilerinin içinde ne olduğunu görmek için birlikte çalıştığınız [tetikleyicilerle](functions-triggers-bindings.md) ilgili belgeleri denetleyin.

## <a name="bindings"></a>Bağlamalar

PowerShell'de, [bağlamalar](functions-triggers-bindings.md) bir işlevin işlevi.json'da yapılandırılır ve tanımlanır. İşlevler bağlamalarla çeşitli yollarla etkileşime girmektedir.

### <a name="reading-trigger-and-input-data"></a>Tetikleyici ve giriş verilerini okuma

Tetikleme ve giriş bağlamaları, işlevinize geçirilen parametreler olarak okunur. Giriş bağlamaları function.json `direction` `in` için bir dizi var. Tanımlanan `name` `param` özellik, `function.json` bloktaki parametrenin adıdır. PowerShell bağlama için adlandırılmış parametreleri kullandığından, parametrelerin sırası önemli değildir. Ancak, en iyi uygulama `function.json`dır.

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)
```

### <a name="writing-output-data"></a>Çıktı verilerini yazma

İşlevler'de, bir `direction` çıktı `out` bağlama işlevi.json bir dizi vardır. İşlevler çalışma zamanı için `Push-OutputBinding` kullanılabilir cmdlet kullanarak bir çıkış bağlama yazabilirsiniz. Her durumda, `name` bağlama özelliği olarak tanımlanan `function.json` `Push-OutputBinding` cmdlet `Name` parametre karşılık gelir.

Aşağıdaki işlev komut `Push-OutputBinding` dosyasında nasıl çağrı yapılacağını gösterir:

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Push-OutputBinding -Name myQueue -Value $myValue
```

Ayrıca, belirli bir bağlama için bir değer de boru hattı üzerinden geçirebilirsiniz.

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Produce-MyOutputValue | Push-OutputBinding -Name myQueue
```

`Push-OutputBinding`için belirtilen değere göre farklı `-Name`davranması:

* Belirtilen ad geçerli bir çıktı bağlama sına çözümlenemediğinde, bir hata atılır.

* Çıktı bağlama değerleri bir koleksiyon kabul ettiğinde, `Push-OutputBinding` birden çok değer itmek için tekrar tekrar arayabilirsiniz.

* Çıktı bağlama yalnızca tekton değerini kabul `Push-OutputBinding` ettiğinde, ikinci kez çağrı yapmak hata kaldırır.

#### <a name="push-outputbinding-syntax"></a>`Push-OutputBinding`Sözdizimi

Aşağıdaki arama `Push-OutputBinding`için geçerli parametreler şunlardır:

| Adı | Tür | Konum | Açıklama |
| ---- | ---- |  -------- | ----------- |
| **`-Name`** | Dize | 1 | Ayarlamak istediğiniz çıktı bağlamaadı. |
| **`-Value`** | Nesne | 2 | Ayarlamak istediğiniz çıktı bağlama değeri, byvalue ardışık ardışık değerden kabul edilir. |
| **`-Clobber`** | SwitchParameter | Adlı | (İsteğe bağlı) Belirtildiğinde, belirli bir çıktı bağlama için ayarlanacak değeri zorlar. | 

Aşağıdaki ortak parametreler de desteklenir: 
* `Verbose`
* `Debug`
* `ErrorAction`
* `ErrorVariable`
* `WarningAction`
* `WarningVariable`
* `OutBuffer`
* `PipelineVariable`
* `OutVariable` 

Daha fazla bilgi için [CommonParameters Hakkında'ya](https://go.microsoft.com/fwlink/?LinkID=113216)bakın.

#### <a name="push-outputbinding-example-http-responses"></a>Push-OutputBinding örnek: HTTP yanıtları

Bir HTTP tetikleyicisi adlı `response`bir çıktı bağlama kullanarak bir yanıt döndürür. Aşağıdaki örnekte, çıktı bağlama `response` "çıkış #1" değerine sahiptir:

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #1"
})
```

Çıktı, yalnızca tekton değerini kabul eden HTTP olduğundan, ikinci `Push-OutputBinding` kez çağrıldığında bir hata atılır.

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #2"
})
```

Yalnızca singleton değerlerini kabul eden çıktılar için, koleksiyona eklemeye çalışmak yerine eski değeri geçersiz kılmak için `-Clobber` parametreyi kullanabilirsiniz. Aşağıdaki örnek, zaten bir değer eklediğinizi varsayar. `-Clobber`Kullanarak, aşağıdaki örnekteki yanıt "çıktı #3" değerini döndürmek için varolan değeri geçersiz kılar:

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #3"
}) -Clobber
```

#### <a name="push-outputbinding-example-queue-output-binding"></a>Push-OutputBinding örneği: Sıra çıktısı bağlama

`Push-OutputBinding`[Azure Queue depolama çıktısına bağlama](functions-bindings-storage-queue-output.md)gibi çıktı bağlamalarına veri göndermek için kullanılır. Aşağıdaki örnekte, kuyruğa yazılan iletinin "çıktı #1" değeri vardır:

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #1"
```

Depolama kuyruğu için çıktı bağlama, birden çok çıktı değerini kabul eder. Bu durumda, ilk sıraya iki öğe içeren bir liste yazdıktan sonra aşağıdaki örneği çağırmak: "çıktı #1" ve "çıktı #2".

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #2"
```

Önceki iki den sonra çağrıldığında, çıktı koleksiyonuna iki değer daha ekleyen aşağıdaki örnek:

```powershell
PS >Push-OutputBinding -Name outQueue -Value @("output #3", "output #4")
```

Sıraya yazıldığında, ileti şu dört değeri içerir: "çıktı #1", "çıktı #2", "çıktı #3" ve "çıktı #4".

#### <a name="get-outputbinding-cmdlet"></a>`Get-OutputBinding`Cmdlet

Çıkış bağlamalarınız için ayarlanan değerleri almak için `Get-OutputBinding` cmdlet'i kullanabilirsiniz. Bu cmdlet, çıktı bağlamalarının adlarını kendi değerleriyle içeren bir karma tablo alır. 

Aşağıdaki geçerli bağlama değerlerini `Get-OutputBinding` döndürmek için kullanma nın bir örneği verilmiştir:

```powershell
Get-OutputBinding
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
MyOtherQueue                   myData
```

`Get-OutputBinding`ayrıca, aşağıdaki örnekte olduğu gibi döndürülen bağlamayı filtrelemek için kullanılabilecek `-Name`, adlı bir parametre de içerir:

```powershell
Get-OutputBinding -Name MyQ*
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
```

Joker karakterler (*) olarak `Get-OutputBinding`desteklenir.

## <a name="logging"></a>Günlüğe Kaydetme

PowerShell işlevlerinde oturum açmak normal PowerShell günlüğü gibi çalışır. Her çıkış akışına yazmak için günlük cmdlets kullanabilirsiniz. Her cmdlet, Fonksiyonlar tarafından kullanılan bir günlük düzeyine eşler.

| İşlevler günlük düzeyi | Kütük cmdlet |
| ------------- | -------------- |
| Hata | **`Write-Error`** |
| Uyarı | **`Write-Warning`**  | 
| Bilgi | **`Write-Information`** <br/> **`Write-Host`** <br /> **`Write-Output`**      | Bilgi | _Bilgi_ düzeyi günlüğe yazar. |
| Hata ayıklama | **`Write-Debug`** |
| İzleme | **`Write-Progress`** <br /> **`Write-Verbose`** |

Bu cmdlets ek olarak, ardışık hatlar için `Information` yazılmış bir şey günlük düzeyine yönlendirilir ve varsayılan PowerShell biçimlendirme ile görüntülenir.

> [!IMPORTANT]
> `Write-Verbose` Veya `Write-Debug` cmdlets kullanarak verbose ve hata ayıklama düzeyi günlük görmek için yeterli değildir. Ayrıca, gerçekte hangi günlük düzeyini önemsediğinizi bildiren günlük düzeyi eşiğini de yapılandırmanız gerekir. Daha fazla bilgi için [bkz.](#configure-the-function-app-log-level)

### <a name="configure-the-function-app-log-level"></a>İşlev uygulaması günlük düzeyini yapılandırma

Azure İşlevler, Işlevlerin günlüklere yazma şeklini denetlemeyi kolaylaştırmak için eşik düzeyini tanımlamanıza olanak tanır. Konsola yazılan tüm izlemeler için eşiği `logging.logLevel.default` ayarlamak [ `host.json` için, dosya][host.json başvurusundaki]özelliği kullanın. Bu ayar, işlev uygulamanızdaki tüm işlevler için geçerlidir.

Aşağıdaki örnek, tüm işlevler için ayrıntılı günlüğe kaydetmeyi etkinleştirmek için eşiği ayarlar, ancak aşağıdaki adlı `MyFunction`bir işlev için hata ayıklama günlüğeğini etkinleştirmek için eşiği ayarlar:

```json
{
    "logging": {
        "logLevel": {
            "Function.MyFunction": "Debug",
            "default": "Trace"
        }
    }
}  
```

Daha fazla bilgi için [host.json referansına]bakın.

### <a name="viewing-the-logs"></a>Günlükleri görüntüleme

İşlev Uygulamanız Azure'da çalışıyorsa, izlemek için Uygulama Öngörüleri'ni kullanabilirsiniz. İşlev günlüklerini görüntüleme ve sorgulama hakkında daha fazla bilgi edinmek için [izleme Azure İşlevleri'ni](functions-monitoring.md) okuyun.

İşlev Uygulamanızı geliştirme için yerel olarak çalıştırıyorsanız, dosya sistemine varsayılan olarak günlük ler. Konsoldaki günlükleri görmek için, `AZURE_FUNCTIONS_ENVIRONMENT` İşlev `Development` Uygulamasını başlatmadan önce ortam değişkenini ayarlayın.

## <a name="triggers-and-bindings-types"></a>Tetikleyiciler ve bağlama türleri

İşlev uygulamanızla kullanabileceğiniz bir dizi tetikleyici ve bağlama vardır. Tetikleyiciler ve bağlamaların tam listesini [burada bulabilirsiniz.](functions-triggers-bindings.md#supported-bindings)

Tüm tetikleyiciler ve bağlamalar kodda birkaç gerçek veri türü olarak temsil edilir:

* Hashtable
* string
* bayt[]
* int
* double
* httpRequestContext
* httpresponsecontext

Bu listedeki ilk beş tür standart .NET türleridir. Son iki yalnızca [HttpTrigger tetikleyicisi](#http-triggers-and-bindings)tarafından kullanılır.

İşlevlerinizdeki her bağlama parametresi bu türlerden biri olmalıdır.

### <a name="http-triggers-and-bindings"></a>HTTP tetikleyiciler ve bağlamalar

HTTP ve webhook tetikleyicileri ve HTTP çıkış bağlamaları, HTTP iletisini temsil etmek için istek ve yanıt nesnelerini kullanır.

#### <a name="request-object"></a>Nesne isteği

Komut dosyasına geçirilen istek nesnesi, `HttpRequestContext`aşağıdaki özelliklere sahip türdedir:

| Özellik  | Açıklama                                                    | Tür                      |
|-----------|----------------------------------------------------------------|---------------------------|
| **`Body`**    | İsteğin gövdesini içeren bir nesne. `Body`verilere dayalı olarak en iyi türe seri hale getirilir. Örneğin, veriler JSON ise, karma tablo olarak geçirilir. Veri bir dize ise, bir dize olarak geçirilir. | object |
| **`Headers`** | İstek üstbilgisini içeren bir sözlük.                | Sözlük<dize, string><sup>*</sup> |
| **`Method`** | İsteğin HTTP yöntemi.                                | string                    |
| **`Params`**  | İsteğin yönlendirme parametrelerini içeren bir nesne. | Sözlük<dize, string><sup>*</sup> |
| **`Query`** | Sorgu parametrelerini içeren bir nesne.                  | Sözlük<dize, string><sup>*</sup> |
| **`Url`** | İsteğin URL'si.                                        | string                    |

<sup>*</sup>Tüm `Dictionary<string,string>` anahtarlar büyük/küçük harf duyarsızdır.

#### <a name="response-object"></a>Yanıt nesnesi

Geri göndermeniz gereken yanıt nesnesi, aşağıdaki özelliklere sahip türdedir: `HttpResponseContext`

| Özellik      | Açıklama                                                 | Tür                      |
|---------------|-------------------------------------------------------------|---------------------------|
| **`Body`**  | Yanıtın gövdesini içeren bir nesne.           | object                    |
| **`ContentType`** | Yanıt için içerik türünü ayarlamak için kısa bir el. | string                    |
| **`Headers`** | Yanıt üstbilgisini içeren bir nesne.               | Sözlük veya Hashtable   |
| **`StatusCode`**  | Yanıtın HTTP durum kodu.                       | dize veya int             |

#### <a name="accessing-the-request-and-response"></a>İstek ve yanıta erişim

HTTP tetikleyicileri ile çalışırken, HTTP isteğine diğer giriş bağlamayla aynı şekilde erişebilirsiniz. `param` Blokta.

Aşağıdakilerde `HttpResponseContext` gösterildiği gibi yanıtı döndürmek için bir nesne kullanın:

`function.json`

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "anonymous"
    },
    {
      "type": "http",
      "direction": "out"
    }
  ]
}
```

`run.ps1`

```powershell
param($req, $TriggerMetadata)

$name = $req.Query.Name

Push-OutputBinding -Name res -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "Hello $name!"
})
```

Bu işlevi çağıran sonucu olacaktır:

```
PS > irm http://localhost:5001?Name=Functions
Hello Functions!
```

### <a name="type-casting-for-triggers-and-bindings"></a>Tetikleyiciler ve ciltler için tip döküm

Blob bağlama gibi belirli bağlamalar için parametrenin türünü belirtebilirsiniz.

Örneğin, Blob depolama dan veri bir dize olarak sağlanan olması `param` için, benim blok için aşağıdaki tür döküm ekleyin:

```powershell
param([string] $myBlob)
```

## <a name="powershell-profile"></a>PowerShell profili

PowerShell'de PowerShell profili konsepti vardır. PowerShell profillerine aşina değilseniz, [profiller hakkında](/powershell/module/microsoft.powershell.core/about/about_profiles)bkz.

PowerShell İşlevleri'nde, işlev uygulaması başlatıldığında profil komut dosyası çalıştırılır. İşlev uygulamaları ilk dağıtıldığında ve boşta kaldıktan sonra başlar[(soğuk başlangıç).](#cold-start)

Visual Studio Code ve Azure İşleçleri Temel Araçları gibi araçları `profile.ps1` kullanarak bir işlev uygulaması oluşturduğunuzda, sizin için bir varsayılan oluşturulur. Varsayılan profil Core [Tools GitHub deposunda](https://github.com/Azure/azure-functions-core-tools/blob/dev/src/Azure.Functions.Cli/StaticResources/profile.ps1) korunur ve şunları içerir:

* Azure'a otomatik MSI kimlik doğrulaması.
* İsterseni Azure PowerShell PowerShell `AzureRM` takma adlarını açma özelliği.

## <a name="powershell-version"></a>PowerShell sürümü

Aşağıdaki tabloda, Fonksiyonlar çalışma zamanının her ana sürümü tarafından kullanılan PowerShell sürümü gösterilmektedir:

| Fonksiyonlar sürümü | PowerShell sürümü                             |
|-------------------|------------------------------------------------|
| 1.x               | Windows PowerShell 5.1 (çalışma süresine göre kilitlendi) |
| 2.x               | PowerShell Çekirdek 6                              |

Geçerli sürümü herhangi bir `$PSVersionTable` işlevden yazdırarak görebilirsiniz.

## <a name="dependency-management"></a>Bağımlılık yönetimi

İşlevler, bağımlılıkları yönetmek için [PowerShell galerisinden](https://www.powershellgallery.com) yararlanmanızı sağlar. Bağımlılık yönetimi etkin olduğunda, requirements.psd1 dosyası gerekli modülleri otomatik olarak indirmek için kullanılır. Aşağıdaki `true` örnekte olduğu `managedDependency` gibi, özelliği ana [bilgisayar.json dosyasının](functions-host-json.md)köküne ayarlayarak bu davranışı etkinleştirin:

```json
{
  "managedDependency": {
          "enabled": true
       }
}
```

Yeni bir PowerShell işlevleri projesi oluşturduğunuzda, Azure [ `Az` modülü](/powershell/azure/new-azureps-module-az) dahil olmak üzere varsayılan olarak bağımlılık yönetimi etkinleştirilir. Şu anda desteklenen maksimum modül sayısı 10'dur. Desteklenen _`MajorNumber`_ `.*` sözdizimi, aşağıdaki requirements.psd1 örneğinde gösterildiği gibi veya tam modül sürümüdür:

```powershell
@{
    Az = '1.*'
    SqlServer = '21.1.18147'
}
```

requirements.psd1 dosyasını güncellediğinizde, güncelleştirilmiş modüller yeniden başladıktan sonra yüklenir.

> [!NOTE]
> Yönetilen bağımlılıklar, modülleri indirmek için www.powershellgallery.com erişim gerektirir. Yerel olarak çalışırken, gerekli güvenlik duvarı kuralları nı ekleyerek çalışma zamanının bu URL'ye erişebileceğinden emin olun. 

Yönetilen bağımlılıkların nasıl indirildiğini ve yüklenmesini değiştirmek için aşağıdaki uygulama ayarları kullanılabilir. Uygulama yükseltmeniz `MDMaxBackgroundUpgradePeriod`içinde başlar ve yükseltme işlemi yaklaşık `MDNewSnapshotCheckPeriod`olarak .

| İşlev Uygulaması ayarı              | Varsayılan değer             | Açıklama                                         |
|   -----------------------------   |   -------------------     |  -----------------------------------------------    |
| **`MDMaxBackgroundUpgradePeriod`**      | `7.00:00:00`(7 gün)     | Her PowerShell çalışanı işlemi, powershell galerisinde süreç başlangıcında ve `MDMaxBackgroundUpgradePeriod` bundan sonra modül yükseltmeleri için denetlemebaşlatır. PowerShell Gallery'de yeni bir modül sürümü bulunduğunda, dosya sistemine yüklenir ve PowerShell çalışanlarıtarafından kullanıma sunulmuştur. Bu değeri azaltmak, işlev uygulamanızın daha yeni modül sürümlerini daha erken almasını sağlar, ancak uygulama kaynağı kullanımını da artırır (ağ I/O, CPU, depolama). Bu değeri artırmak uygulamanın kaynak kullanımını azaltır, ancak uygulamanız için yeni modül sürümlerinin sunulmasını da geciktirebilir. | 
| **`MDNewSnapshotCheckPeriod`**         | `01:00:00`(1 saat)       | Dosya sistemine yeni modül sürümleri yüklendikten sonra, her PowerShell alt işlemi yeniden başlatılmalıdır. PowerShell çalışanlarını yeniden başlatmak, geçerli işlev yürütmeyi kesintiye uğratabileceği için uygulamanızın kullanılabilirliğini etkiler. Tüm PowerShell alt işlemleri yeniden başlatılana kadar, işlev çağrıları eski veya yeni modül sürümlerini kullanabilir. Tüm PowerShell işçileri içinde `MDNewSnapshotCheckPeriod`tamamlandı yeniden başlatılması. Bu değerin artırılması kesintilerin sıklığını azaltır, ancak işlev çağrılarının eski veya yeni modül sürümlerini deterministically kullanma süresini de artırabilir. |
| **`MDMinBackgroundUpgradePeriod`**      | `1.00:00:00`(1 gün)     | Sık İşçi yeniden başlatmalarda aşırı modül yükseltmelerini önlemek için, modül yükseltmelerini denetlemek, herhangi bir `MDMinBackgroundUpgradePeriod`çalışan son durumda bu denetimi başlatmışken gerçekleştirilmiyor. |

Kendi özel modüllerinizi kullanmanızdan, normalde yaptığınızdan biraz farklıdır.

Yerel bilgisayarınızda, modül, genel olarak kullanılabilir klasörlerinden birine `$env:PSModulePath`yüklenir. Azure'da çalışırken, makinenizde yüklü olan modüllere erişiminiz yoktur. Bu, bir `$env:PSModulePath` PowerShell işlev uygulaması için `$env:PSModulePath` normal bir PowerShell komut dosyasından farklı olduğu anlamına gelir.

İşlevler'de iki yol `PSModulePath` içerir:

* İşlev uygulamanızın kökünde bulunan bir `Modules` klasör.
* PowerShell dil `Modules` çalışanı tarafından denetlenir bir klasöre giden yol.

### <a name="function-app-level-modules-folder"></a>İşlev uygulama `Modules` düzeyinde klasör

Özel modülleri kullanmak için, işlevlerinizin bağlı olduğu `Modules` modülleri bir klasöre yerleştirebilirsiniz. Bu klasörden, modüller otomatik olarak çalışma zamanı işlevleri için kullanılabilir. Fonksiyon uygulamasındaki herhangi bir işlev bu modülleri kullanabilir. 

> [!NOTE]
> requirements.psd1 dosyasında belirtilen modüller otomatik olarak indirilir ve yola dahil edilir, böylece modüller klasörüne eklemenize gerek yoktur. Bunlar, bulutta çalıştırıldığında `$env:LOCALAPPDATA/AzureFunctions` klasörde `/data/ManagedDependencies` ve klasörde yerel olarak depolanır.

Özel modül özelliğinden yararlanmak için `Modules` işlev uygulamanızın kökünde bir klasör oluşturun. Fonksiyonlarınızda kullanmak istediğiniz modülleri bu konuma kopyalayın.

```powershell
mkdir ./Modules
Copy-Item -Path /mymodules/mycustommodule -Destination ./Modules -Recurse
```

Bir `Modules` klasörle, işlev uygulamanız aşağıdaki klasör yapısına sahip olmalıdır:

```
PSFunctionApp
 | - MyFunction
 | | - run.ps1
 | | - function.json
 | - Modules
 | | - MyCustomModule
 | | - MyOtherCustomModule
 | | - MySpecialModule.psm1
 | - local.settings.json
 | - host.json
 | - requirements.psd1
```

İşlev uygulamanızı başlattığınızda, PowerShell dil `Modules` çalışanı `$env:PSModulePath` bu klasörü normal bir PowerShell komut dosyasında olduğu gibi modül otomatik yüklemesine güvenebilmeniz için ekler.

### <a name="language-worker-level-modules-folder"></a>Dil işçi `Modules` düzeyi klasörü

PowerShell dil çalışanı tarafından çeşitli modüller yaygın olarak kullanılır. Bu modüller son konumda `PSModulePath`tanımlanır. 

Modüllerin geçerli listesi aşağıdaki gibidir:

* [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive): arşivlerle çalışmak için `.zip` `.nupkg`kullanılan modül, gibi , , ve diğerleri.
* **ThreadJob**: PowerShell iş API'lerinin iş parçacığı tabanlı uygulaması.

Varsayılan olarak, Fonksiyonlar bu modüllerin en son sürümünü kullanır. Belirli bir modül sürümünü kullanmak için, `Modules` belirli bir sürümü işlev uygulamanızın klasörüne koyun.

## <a name="environment-variables"></a>Ortam değişkenleri

İşlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md)yürütme sırasında ortam değişkenleri olarak ortaya çıkarır. Aşağıdaki örnekte gösterildiği `$env:NAME_OF_ENV_VAR`gibi, bu ayarlara aşağıdaki leri kullanarak erişebilirsiniz:

```powershell
param($myTimer)

Write-Host "PowerShell timer trigger function ran! $(Get-Date)"
Write-Host $env:AzureWebJobsStorage
Write-Host $env:WEBSITE_SITE_NAME
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Yerel olarak çalışırken, uygulama ayarları [local.settings.json](functions-run-local.md#local-settings-file) project dosyasından okunur.

## <a name="concurrency"></a>Eşzamanlılık

Varsayılan olarak, PowerShell çalışma süresi işlevleri bir seferde yalnızca bir işlevin çağrılması işlemini işleyebilir. Ancak, bu eşzamanlılık düzeyi aşağıdaki durumlarda yeterli olmayabilir:

* Aynı anda çok sayıda çağrıyı halletmeye çalışıyorsanız.
* Aynı işlev uygulaması içinde diğer işlevleri çağıran işlevleri varsa.

Aşağıdaki ortam değişkenini tamsayı değerine ayarlayarak bu davranışı değiştirebilirsiniz:

```
PSWorkerInProcConcurrencyUpperBound
```

Bu ortam değişkenini İşlev Uygulamanızın [uygulama ayarlarında](functions-app-settings.md) ayarlarsınız.

### <a name="considerations-for-using-concurrency"></a>Eşzamanlılık kullanımı için dikkat edilmesi gerekenler

PowerShell varsayılan olarak tek bir _iş parçacığı_ komut dosyası dilidir. Ancak, eşzamanlılık aynı işlemde birden çok PowerShell runspaces kullanılarak eklenebilir. Oluşturulan runspaces miktarı PSWorkerInProcConcurrencyUpperBound uygulama ayarı eşleşir. Elde iş, seçili planda bulunan CPU ve bellek miktarından etkilenir.

Azure PowerShell, aşırı yazmadan tasarruf etmenizi sağlamak için bazı _işlem düzeyi_ bağlamları ve durum kullanır. Ancak, işlev uygulamanızda eşzamanlılığı açar ve durumu değiştiren eylemleri çağırırsanız, yarış koşullarına sahip olabilirsiniz. Bu Yarış koşulları hata ayıklamak zordur, çünkü bir çağrı belirli bir duruma dayanır ve diğeri de devleti değiştirir.

Bazı işlemler önemli ölçüde zaman alabileceğinden, Azure PowerShell ile eşzamanlılıkta çok büyük bir değer vardır. Ancak, dikkatli hareket etmelisiniz. Bir yarış koşulu yaşadığınızı fark ederseniz, PSWorkerInProcConcurrencyUpperBound uygulama ayarını ayarla `1` ve bunun yerine eşzamanlılık için dil alt işlem düzeyi [yalıtımını](functions-app-settings.md#functions_worker_process_count) kullanın.

## <a name="configure-function-scriptfile"></a>İşlevi yapılandırma`scriptFile`

Varsayılan olarak, bir PowerShell `run.ps1`işlevi, karşılık gelen `function.json`aynı üst dizini paylaşan bir dosyadan yürütülür.

Aşağıdaki `scriptFile` örnek `function.json` gibi görünen bir klasör yapısı almak için kullanılabilir:

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.ps1
```

Bu durumda, `function.json` for `myFunction` çalıştırmak `scriptFile` için dışa aktarılan işlevi ile dosya başvuran bir özellik içerir.

```json
{
  "scriptFile": "../lib/PSFunction.ps1",
  "bindings": [
    // ...
  ]
}
```

## <a name="use-powershell-modules-by-configuring-an-entrypoint"></a>Bir entryPoint yapılandırarak PowerShell modüllerini kullanma

Bu makalede, şablonlar tarafından `run.ps1` oluşturulan varsayılan komut dosyası dosyasında PowerShell işlevleri gösterilmiştir.
Ancak, işlevlerinizi PowerShell modüllerine de ekleyebilirsiniz. Fonksiyon.json yapılandırma dosyasındaki `scriptFile` alanları ve `entryPoint` alanları kullanarak modüldeki özel işlev kodunuzu referans alabilirsiniz.

Bu durumda, `entryPoint` PowerShell modülünde başvurulan bir işlev in adı `scriptFile`veya cmdlet.

Aşağıdaki klasör yapısını göz önünde bulundurun:

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.psm1
```

Nerede `PSFunction.psm1` içerir:

```powershell
function Invoke-PSTestFunc {
    param($InputBinding, $TriggerMetadata)

    Push-OutputBinding -Name OutputBinding -Value "output"
}

Export-ModuleMember -Function "Invoke-PSTestFunc"
```

Bu örnekte, yapılandırma, `myFunction` `scriptFile` başka bir `PSFunction.psm1`klasördeki PowerShell modülüne başvuran bir özellik içerir.  Özellik, `entryPoint` modüldeki giriş noktası olan işleve `Invoke-PSTestFunc` başvurur.

```json
{
  "scriptFile": "../lib/PSFunction.psm1",
  "entryPoint": "Invoke-PSTestFunc",
  "bindings": [
    // ...
  ]
}
```

Bu yapılandırma ile, tam olarak `run.ps1` bir şekilde `Invoke-PSTestFunc` yürütülür.

## <a name="considerations-for-powershell-functions"></a>PowerShell işlevleri için dikkat edilecek noktalar

PowerShell işlevleriyle çalışırken, aşağıdaki bölümlerde dikkat edilmesi gereken hususlara dikkat edin.

### <a name="cold-start"></a>Soğuk Başlangıç

[Sunucusuz barındırma modelinde](functions-scale.md#consumption-plan)Azure Fonksiyonları geliştirirken, soğuk başlangıçlar gerçektir. *Soğuk başlangıç,* işlev uygulamanızın bir isteği işlemek için çalışmaya başlaması için gereken süreyi ifade eder. İşlev uygulamanız hareketsizlik dönemlerinde kapatıldığı için Tüketim planında soğuk başlangıç daha sık gerçekleşir.

### <a name="bundle-modules-instead-of-using-install-module"></a>Modülleri kullanmak yerine paketleyin`Install-Module`

Senaryonuz her çağrıda yayınlanır. Komut `Install-Module` dosyanızda kullanmaktan kaçının. Bunun `Save-Module` yerine, işlevinizin modülü indirerek zaman harcamak zorunda kalmaması için yayımlamadan önce kullanın. Soğuk başlangıçlar işlevlerinizi etkiliyorsa, işlev uygulamanızı *her zaman* premium planda veya premium [planda](functions-scale.md#premium-plan)ayarlanmış bir Uygulama [Hizmeti planına](functions-scale.md#app-service-plan) dağıtmayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure Fonksiyonları tetikler ve bağlamalar](functions-triggers-bindings.md)

[host.json başvurusu]: functions-host-json.md
