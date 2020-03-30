---
title: Azure Fonksiyonları F# geliştirici başvurusu
description: F# komut dosyasını kullanarak Azure İşlevlerini nasıl geliştireceklerini öğrenin.
author: sylvanc
ms.assetid: e60226e5-2630-41d7-9e5b-9f9e5acc8e50
ms.topic: reference
ms.date: 10/09/2018
ms.author: syclebsc
ms.openlocfilehash: 669701f91ab28a4eb734b0346be6515dc44e8685
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276770"
---
# <a name="azure-functions-f-developer-reference"></a>Azure Fonksiyonları F# Geliştirici Başvurusu

Azure İşlevler için F# bulutta küçük kod parçalarını veya "işlevleri" kolayca çalıştırmak için bir çözümdür. Veri, işlev bağımsız değişkenleri aracılığıyla F# işlevinize akar. Bağımsız değişken adları `function.json`belirtilir ve işlev kaydedici ve iptal belirteçleri gibi şeylere erişmek için önceden tanımlanmış adlar vardır. 

>[!IMPORTANT]
>F# komut dosyası (.fsx), yalnızca Azure İşlevleri çalışma zamanının [1.x sürümüyle](functions-versions.md#creating-1x-apps) desteklenir. F# sürüm 2.x ve çalışma zamanının sonraki sürümleriyle birlikte kullanmak istiyorsanız, önceden derlenmiş bir F# sınıfı kitaplık projesi (.fs) kullanmanız gerekir. Visual Studio'da [C# sınıfı kitaplık projesinde](functions-dotnet-class-library.md)olduğu gibi bir F# sınıfı kitaplık projesi oluşturur, yönetir ve yayımlarsınız. İşlevsürümleri hakkında daha fazla bilgi için [Azure İşlevleri çalışma zamanı sürümlerine genel bakış](functions-versions.md)bilgisine bakın.

Bu makalede, [Azure İşlevler geliştirici başvurularını](functions-reference.md)zaten okuduğunuz varsayar.

## <a name="how-fsx-works"></a>.fsx nasıl çalışır?
Dosya `.fsx` F# komut dosyasıdır. Tek bir dosyada bulunan bir F# projesi olarak düşünülebilir. Dosya, hem programınız için kodu (bu durumda Azure İşleviniz) hem de bağımlılıkları yönetme yönergelerini içerir.

Bir Azure `.fsx` İşi için bir işlev kullandığınızda, sık sık gereken derlemeler sizin için otomatik olarak eklenir ve bu da "kazan" kodu yerine işleve odaklanmanıza olanak tanır.

## <a name="folder-structure"></a>Klasör yapısı

F# komut dosyası projesinin klasör yapısı aşağıdaki gibi görünür:

```
FunctionsProject
 | - MyFirstFunction
 | | - run.fsx
 | | - function.json
 | | - function.proj
 | - MySecondFunction
 | | - run.fsx
 | | - function.json
 | | - function.proj
 | - host.json
 | - extensions.csproj
 | - bin
```

İşlev uygulamasını yapılandırmak için kullanılabilecek paylaşılan bir [host.json](functions-host-json.md) dosyası vardır. Her işlevin kendi kod dosyası (.fsx) ve bağlama yapılandırma dosyası (function.json) vardır.

Sürüm [2.x'te](functions-versions.md) ve İşlevler çalışma zamanının sonraki sürümlerinde gerekli olan bağlama `extensions.csproj` uzantıları, `bin` dosyada klasördeki gerçek kitaplık dosyalarıyla birlikte tanımlanır. Yerel olarak geliştirirken, [bağlama uzantılarını kaydetmeniz](./functions-bindings-register.md#extension-bundles)gerekir. Azure portalında işlevler geliştirirken, bu kayıt sizin için yapılır.

## <a name="binding-to-arguments"></a>Bağımsız değişkenlere bağlanma
Her bağlama, Azure İşlevleri'nde ayrıntılı olarak belirtildiği gibi, geliştirici [başvurularını tetikler ve bağlar.](functions-triggers-bindings.md) Örneğin, blob tetikleyicisinin desteklediği bağımsız değişken bağlaçlarından biri, F# kaydı kullanılarak ifade edilebilen bir POCO'dur. Örnek:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

F# Azure Işleviniz bir veya daha fazla bağımsız değişken alır. Azure İşlevler bağımsız değişkenleri hakkında konuştuğumuzda, *giriş* bağımsız değişkenleri ve *çıktı* bağımsız değişkenlerine atıfta bulunuruz. Giriş bağımsız değişkeni tam olarak göründüğü gibidir: F# Azure İşlevinize giriş. *Çıktı* bağımsız değişkeni, değişken `byref<>` veriler veya verileri işlevinizden *geri* geçirmenin bir yolu olarak hizmet veren bir bağımsız değişkendir.

Yukarıdaki örnekte, `blob` bir giriş bağımsız `output` değişkeni ve bir çıktı bağımsız değişkenidir. Kullandığımıza `byref<>` `output` dikkat edin (ek açıklama eklemeye `[<Out>]` gerek yoktur). Bir `byref<>` tür kullanmak, işlevinizin bağımsız değişkenin hangi kayda veya nesneye atıfta bulunduğunu değiştirmesine olanak tanır.

Bir F# kaydı giriş türü olarak kullanıldığında, Azure İşlevler çerçevesinin kaydı işlevinize geçirmeden önce alanları uygun şekilde ayarlamasına izin vermek için kayıt tanımının işaretlemesi `[<CLIMutable>]` gerekir. Kaputun `[<CLIMutable>]` altında, kayıt özellikleri için ayarlayıcılar oluşturur. Örnek:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: ILogger) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

F# sınıfı hem içeri hem de dış bağımsız değişkenler için de kullanılabilir. Bir sınıf için, özellikleri genellikle getters ve ayarlayıcılar gerekir. Örnek:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Günlüğe Kaydetme
F#'daki [akış günlüklerinize](../app-service/troubleshoot-diagnostic-logs.md) çıktınızı günlüğe kaydetmek için, işleviniz [iLogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger)türünden bir bağımsız değişken almalıdır. Tutarlılık için, bu bağımsız değişkenin adını `log`öneririz. Örnek:

```fsharp
let Run(blob: string, output: byref<string>, log: ILogger) =
    log.LogInformation(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Zaman Uyumsuz
İş `async` akışı kullanılabilir, ancak sonuç bir `Task`. Bu, örneğin `Async.StartAsTask`yapılabilir:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>İptal Jetonu
Işlevinizin kapatmaişlemini incelikle işlemesi [`CancellationToken`](/dotnet/api/system.threading.cancellationtoken) gerekiyorsa, bu işleve bir argüman verebilirsiniz. Bu, örneğin, `async`ile birleştirilebilir:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Ad alanlarını alma
Ad boşlukları her zamanki gibi açılabilir:

```fsharp
open System.Net
open System.Threading.Tasks
open Microsoft.Extensions.Logging

let Run(req: HttpRequestMessage, log: ILogger) =
    ...
```

Aşağıdaki ad alanları otomatik olarak açılır:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Dış Meclislere Başvurma
Benzer şekilde, çerçeve derleme başvuruları `#r "AssemblyName"` yönerge ile eklenebilir.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks
open Microsoft.Extensions.Logging

let Run(req: HttpRequestMessage, log: ILogger) =
    ...
```

Aşağıdaki derlemeler Azure İşlevleri barındırma ortamı tarafından otomatik olarak eklenir:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Buna ek olarak, aşağıdaki derlemeler özel örnek ve basit ad (örneğin) `#r "AssemblyName"`ile başvurulabilir:

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Özel bir derlemeye başvurmanız gerekiyorsa, derleme dosyasını `bin` işlevinize göre bir klasöre yükleyebilir ve dosya adını kullanarak dosya adını kullanarak dosyaya başvuruyapabilirsiniz (örn.  `#r "MyAssembly.dll"`). İşlev klasörünüze dosya yükleme hakkında daha fazla bilgi için paket yönetimi yle ilgili aşağıdaki bölüme bakın.

## <a name="editor-prelude"></a>Editör Prelüd
F# Derleyici Hizmetlerini destekleyen bir düzenleyici, Azure İşlevlerinin otomatik olarak içerdiği ad boşluklarından ve derlemelerden haberdar olmaz. Bu nedenle, düzenleyicinin kullanmakta olduğunuz derlemeleri bulmasına yardımcı olan bir prelüd eklemek ve ad alanlarını açıkça açmak yararlı olabilir. Örnek:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open System
open Microsoft.Azure.WebJobs.Host
open Microsoft.Extensions.Logging

let Run(blob: string, output: byref<string>, log: ILogger) =
    ...
```

Azure İşlevler kodunuzu çalıştırdığında, `COMPILED` kaynağı tanımlı olarak işler, böylece düzenleyici prelüd yoksayılır.

<a name="package"></a>

## <a name="package-management"></a>Paket yönetimi
NuGet paketlerini F# işlevinde kullanmak `project.json` için, işlev uygulamasının dosya sisteminde işlev klasörüne bir dosya ekleyin. Sürüm 1.1.0'a `project.json` `Microsoft.ProjectOxford.Face` NuGet paketi başvurusu ekleyen örnek bir dosya aşağıda verilmiştir:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Yalnızca .NET Framework 4.6 desteklenir, bu `project.json` nedenle dosyanızın `net46` burada gösterildiği gibi belirttiğinden emin olun.

Bir `project.json` dosya yüklediğinizde, çalışma zamanı paketleri alır ve otomatik olarak paket derlemelerine referanslar ekler. `#r "AssemblyName"` Direktif eklemenize gerek yoktur. Dosyanıza gerekli `open` ifadeleri `.fsx` eklemeniz gerekir.

Editörünüzün F# Compile Services ile etkileşimini geliştirmek için, editör ünüzün prelüd'üne otomatik olarak referansderlemeleri koymak isteyebilirsiniz.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Azure İşlevinize `project.json` dosya ekleme
1. Azure portalında işlevinizi açarak yapabileceğiniz işlev uygulamanızın çalıştığından emin olarak başlayın. Bu, paket yükleme çıktısının görüntüleneceği akış günlüklerine de erişim sağlar.
2. Bir `project.json` dosyayı yüklemek [için, işlev uygulama dosyalarının nasıl güncelleştirilirse](functions-reference.md#fileupdate)güncelleştirilen yöntemlerden birini kullanın. [Azure İşlevler için Sürekli Dağıtım](functions-continuous-deployment.md)kullanıyorsanız, dağıtım dalınıza eklemeden önce dosyayı denemek için hazırlama dalına bir `project.json` dosya ekleyebilirsiniz.
3. `project.json` Dosya eklendikten sonra, işlevinizin akış günlüğünde aşağıdaki örneğe benzer çıktı görürsünüz:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Ortam değişkenleri
Bir ortam değişkeni veya uygulama ayar `System.Environment.GetEnvironmentVariable`değeri elde etmek için, örneğin:

```fsharp
open System.Environment
open Microsoft.Extensions.Logging

let Run(timer: TimerInfo, log: ILogger) =
    log.LogInformation("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.LogInformation("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>.fsx kodunu yeniden kullanma
Bir `#load` yönerge kullanarak `.fsx` diğer dosyalardaki kodu kullanabilirsiniz. Örnek:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: ILogger) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: ILogger, text: string) =
    log.LogInformation(text);
```

`#load` Yönergeye sağlayan yollar dosyanızın `.fsx` konumuna göredir.

* `#load "logger.fsx"`işlev klasöründe bulunan bir dosyayı yükler.
* `#load "package\logger.fsx"`işlev klasöründeki `package` klasörde bulunan bir dosyayı yükler.
* `#load "..\shared\mylogger.fsx"``shared` işlev klasörüyle aynı düzeyde klasörde bulunan bir dosyayı yükler, `wwwroot`yani doğrudan .

Yönerge `#load` yalnızca `.fsx` (F# komut dosyası) dosyalarıyla çalışır, dosyalarla `.fs` değil.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [F# Rehberi](/dotnet/articles/fsharp/index)
* [Azure İşlevler için En İyi Uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure Fonksiyonları tetikler ve bağlamalar](functions-triggers-bindings.md)
* [Azure İşlevler testi](functions-test-a-function.md)
* [Azure İşlevler ölçekleme](functions-scale.md)

