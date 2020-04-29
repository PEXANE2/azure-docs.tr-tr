---
title: Azure Işlevleri için PowerShell geliştirici başvurusu
description: PowerShell kullanarak işlevleri geliştirmeyi anlayın.
author: eamonoreilly
ms.topic: conceptual
ms.date: 04/22/2019
ms.openlocfilehash: 41f977e7e7c23c2f49fd656461b7a3920802997e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79276744"
---
# <a name="azure-functions-powershell-developer-guide"></a>Azure Işlevleri PowerShell Geliştirici Kılavuzu

Bu makalede, PowerShell kullanarak Azure Işlevleri yazma hakkında ayrıntılı bilgi sağlanır.

Bir PowerShell Azure işlevi (işlev), tetiklendiğinde yürütülen bir PowerShell betiği olarak temsil edilir. Her işlev betiği, işlevin nasıl `function.json` davranacağını, örneğin nasıl tetikleneceğini ve giriş ve çıkış parametrelerini tanımlayan ilgili bir dosya içerir. Daha fazla bilgi edinmek için [Tetikleyiciler ve bağlama makalesine](functions-triggers-bindings.md)bakın. 

Diğer işlev türleri gibi, PowerShell betik işlevleri de `function.json` dosyada tanımlanan tüm giriş bağlamalarının adlarıyla eşleşen parametreleri alır. İşlevi `TriggerMetadata` Başlatan tetikleyicide ek bilgiler içeren bir parametre de geçirilir.

Bu makalede, [Azure işlevleri geliştirici başvurusunu](functions-reference.md)zaten okuduğunuzu varsaymış olursunuz. İlk PowerShell işlevinizi oluşturmak için [PowerShell için hızlı başlangıç işlevleri](functions-create-first-function-powershell.md) de tamamlanmış olmalıdır.

## <a name="folder-structure"></a>Klasör yapısı

PowerShell projesi için gereken klasör yapısı aşağıdaki gibi görünür. Bu varsayılan değer değiştirilebilir. Daha fazla bilgi için aşağıdaki [scriptfile](#configure-function-scriptfile) bölümüne bakın.

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

Projenin kökünde, işlev uygulamasını yapılandırmak için kullanılabilecek paylaşılan [`host.json`](functions-host-json.md) bir dosya vardır. Her işlevde kendi kod dosyası (. ps1) ve bağlama yapılandırma dosyası (`function.json`) içeren bir klasör vardır. Function. json dosyasının üst dizininin adı her zaman işlevinizin adıdır.

Belirli bağlamalar bir `extensions.csproj` dosya varlığını gerektirir. [Sürüm 2. x ve sonraki](functions-versions.md) sürümlerde işlevler çalışma zamanının gerekli olan bağlama uzantıları, `extensions.csproj` dosyasında, `bin` klasördeki gerçek kitaplık dosyaları ile tanımlanmıştır. Yerel olarak geliştirme yaparken, [bağlama uzantılarını kaydetmeniz](functions-bindings-register.md#extension-bundles)gerekir. Azure portal işlevler geliştirirken, bu kayıt sizin için yapılır.

PowerShell Işlev uygulamalarında, bir işlev uygulaması çalışmaya başladığında isteğe `profile.ps1` bağlı olarak bir çalışır (Aksi takdirde, *[soğuk bir başlangıç](#cold-start)* olarak bilinir. Daha fazla bilgi için bkz. [PowerShell profili](#powershell-profile).

## <a name="defining-a-powershell-script-as-a-function"></a>Bir PowerShell betiğini işlev olarak tanımlama

Varsayılan olarak, Işlevler çalışma zamanı ' de `run.ps1`işlevinize bakar ve buna `run.ps1` karşılık `function.json`olarak aynı üst dizini paylaşır.

Betiğinizin yürütme sırasında bir dizi bağımsız değişkeni geçti. Bu parametreleri işlemek için, betiğinizin en üstüne aşağıdaki örnekte olduğu gibi bir `param` blok ekleyin:

```powershell
# $TriggerMetadata is optional here. If you don't need it, you can safely remove it from the param block
param($MyFirstInputBinding, $MySecondInputBinding, $TriggerMetadata)
```

### <a name="triggermetadata-parameter"></a>TriggerMetadata parametresi

`TriggerMetadata` Parametresi, tetikleyici hakkında ek bilgi sağlamak için kullanılır. Ek meta veriler bağlamadan bağlamaya farklılık gösterir, ancak hepsi aşağıdaki verileri `sys` içeren bir özellik içerir:

```powershell
$TriggerMetadata.sys
```

| Özellik   | Açıklama                                     | Tür     |
|------------|-------------------------------------------------|----------|
| UtcNow     | UTC olarak işlev tetiklendiğinde        | DateTime |
| MethodName | Tetiklenen Işlevin adı     | string   |
| RandGuid   | işlevin bu yürütmeye yönelik benzersiz bir GUID | string   |

Her tetikleyici türünün farklı bir meta veri kümesi vardır. `$TriggerMetadata` Örneğin, için `QueueTrigger` , diğer şeyleri kapsayan `InsertionTime` `Id` `DequeueCount`,,,. Sıra tetikleyicisinin meta verileri hakkında daha fazla bilgi için, [sıra tetikleyicilerinin resmi belgelerine](functions-bindings-storage-queue-trigger.md#message-metadata)gidin. Tetikleyici meta verilerinin içinde neler olduğunu görmek için, üzerinde çalıştığınız [tetikleyicilerle](functions-triggers-bindings.md) ilgili belgelere bakın.

## <a name="bindings"></a>Bağlamalar

PowerShell 'de [bağlamalar](functions-triggers-bindings.md) , bir işlevin function. json dosyasında yapılandırılır ve tanımlanır. İşlevler, bağlamalarla çeşitli yollarla etkileşime geçin.

### <a name="reading-trigger-and-input-data"></a>Tetikleyici ve giriş verilerini okuma

Tetikleyici ve giriş bağlamaları, işleviniz için parametre olarak okunabilir. Giriş bağlamaları function. `direction` JSON içinde `in` bir olarak ayarlanır. İçinde `name` `function.json` tanımlanan özelliği, `param` bloğundaki parametrenin adıdır. PowerShell, bağlama için adlandırılmış parametreler kullandığından, parametrelerin sırası bu şekilde değildir. Ancak, içinde tanımlanan bağlamaların sırasını izlemek en iyi uygulamadır `function.json`.

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)
```

### <a name="writing-output-data"></a>Çıkış verileri yazma

Işlevlerde, bir çıkış bağlamasının function. JSON `direction` içinde öğesine `out` bir kümesi vardır. Işlevler çalışma zamanı için kullanılabilen `Push-OutputBinding` cmdlet 'ini kullanarak bir çıktı bağlamaya yazabilirsiniz. Her `name` durumda, içinde `function.json` tanımlandığı şekilde bağlamanın özelliği `Name` `Push-OutputBinding` cmdlet 'in parametresine karşılık gelir.

Aşağıda, işlev betiğinizde `Push-OutputBinding` nasıl çağrılacağını gösterilmektedir:

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Push-OutputBinding -Name myQueue -Value $myValue
```

Ayrıca işlem hattı aracılığıyla belirli bir bağlama için bir değer geçirebilirsiniz.

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Produce-MyOutputValue | Push-OutputBinding -Name myQueue
```

`Push-OutputBinding`, için `-Name`belirtilen değere göre farklı davranır:

* Belirtilen ad geçerli bir çıkış bağlamasıyla çözülemezse bir hata oluşur.

* Çıkış bağlaması bir değer koleksiyonunu kabul ettiğinde, birden fazla değeri göndermek için `Push-OutputBinding` tekrar tekrar çağırabilirsiniz.

* Çıkış bağlaması yalnızca bir tek değeri kabul ettiğinde, ikinci kez `Push-OutputBinding` çağırma bir hata oluşturur.

#### <a name="push-outputbinding-syntax"></a>`Push-OutputBinding`sözdizimi

Çağırmak `Push-OutputBinding`için geçerli parametreler aşağıda verilmiştir:

| Adı | Tür | Konum | Açıklama |
| ---- | ---- |  -------- | ----------- |
| **`-Name`** | Dize | 1 | Ayarlamak istediğiniz çıkış bağlamasının adı. |
| **`-Value`** | Nesne | 2 | Ayarlamak istediğiniz çıkış bağlamasının değeri, işlem hattı ByValue 'dan kabul edilir. |
| **`-Clobber`** | SwitchParameter | Adlandırılır | Seçim Belirtildiğinde, belirtilen bir çıkış bağlaması için değeri ayarlamaya zorlar. | 

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

Daha fazla bilgi için bkz. [CommonParameters hakkında](https://go.microsoft.com/fwlink/?LinkID=113216).

#### <a name="push-outputbinding-example-http-responses"></a>Gönderme-OutputBinding örneği: HTTP yanıtları

HTTP tetikleyicisi adlı `response`çıkış bağlamasını kullanarak bir yanıt döndürür. Aşağıdaki örnekte, çıkış bağlaması "output #1" `response` değerine sahiptir:

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #1"
})
```

Çıktı yalnızca tek bir değer kabul eden HTTP 'e ait olduğundan, ikinci kez çağrıldığında bir hata oluşur `Push-OutputBinding` .

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #2"
})
```

Yalnızca Singleton değerlerini kabul eden çıkışlar için, bir koleksiyona eklemeye çalışmak `-Clobber` yerine eski değeri geçersiz kılmak için parametresini kullanabilirsiniz. Aşağıdaki örnek, daha önce bir değer eklediğinizi varsayar. Kullanarak `-Clobber`, aşağıdaki örnekteki yanıt, "çıkış #3" değeri döndürecek varolan değeri geçersiz kılar:

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #3"
}) -Clobber
```

#### <a name="push-outputbinding-example-queue-output-binding"></a>Gönderme-OutputBinding örneği: kuyruk çıkış bağlaması

`Push-OutputBinding`, [Azure kuyruk depolama çıkış bağlaması](functions-bindings-storage-queue-output.md)gibi çıkış bağlamalarına veri göndermek için kullanılır. Aşağıdaki örnekte, sıraya yazılan ileti "output #1" değerine sahiptir:

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #1"
```

Depolama kuyruğu için çıkış bağlaması birden çok çıkış değeri kabul eder. Bu durumda, iki öğe içeren bir listeyi kuyruğa yazmaya sonra aşağıdaki örneği çağırmak: "output #1" ve "output #2".

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #2"
```

Aşağıdaki örnek, önceki iki değerin ardından çağrıldığında, çıkış koleksiyonuna iki değer daha ekler:

```powershell
PS >Push-OutputBinding -Name outQueue -Value @("output #3", "output #4")
```

Sıraya yazıldığında ileti şu dört değeri içerir: "çıkış #1", "çıkış #2", "çıkış #3" ve "çıkış #4".

#### <a name="get-outputbinding-cmdlet"></a>`Get-OutputBinding`cmdlet

Çıkış bağlamalarınız için `Get-OutputBinding` Şu anda ayarlanmış olan değerleri almak için cmdlet 'ini kullanabilirsiniz. Bu cmdlet, çıkış bağlamalarının adlarını ilgili değerleriyle içeren bir Hashtable alır. 

Aşağıda geçerli bağlama değerlerini döndürmek için kullanılan `Get-OutputBinding` bir örneği verilmiştir:

```powershell
Get-OutputBinding
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
MyOtherQueue                   myData
```

`Get-OutputBinding`Ayrıca, aşağıdaki örnekte olduğu `-Name`gibi döndürülen bağlamayı filtrelemek için kullanılabilen adlı bir parametresini de içerir:

```powershell
Get-OutputBinding -Name MyQ*
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
```

Joker karakterler (*) ' de `Get-OutputBinding`desteklenir.

## <a name="logging"></a>Günlüğe Kaydetme

PowerShell işlevlerinde günlüğe kaydetme, normal PowerShell günlüğü gibi çalışır. Günlük cmdlet 'lerini her bir çıkış akışına yazmak için kullanabilirsiniz. Her cmdlet, Işlevleri tarafından kullanılan bir günlük düzeyiyle eşlenir.

| İşlev günlüğü düzeyi | Günlük cmdlet 'i |
| ------------- | -------------- |
| Hata | **`Write-Error`** |
| Uyarı | **`Write-Warning`**  | 
| Bilgi | **`Write-Information`** <br/> **`Write-Host`** <br /> **`Write-Output`**      | Bilgi | _Bilgi_ düzeyi günlüğe yazmaya yazar. |
| Hata ayıklama | **`Write-Debug`** |
| İzleme | **`Write-Progress`** <br /> **`Write-Verbose`** |

Bu cmdlet 'lere ek olarak, işlem hattına yazılan her şey `Information` günlük düzeyine yönlendirilir ve varsayılan PowerShell biçimlendirmesi ile görüntülenir.

> [!IMPORTANT]
> `Write-Verbose` Veya `Write-Debug` cmdlet 'lerinin kullanılması, ayrıntılı ve hata ayıklama düzeyinde günlüğe kaydetmeyi görmek için yeterli değildir. Ayrıca, gerçekten önem verdiğiniz günlükleri bildiren günlük düzeyi eşiğini da yapılandırmanız gerekir. Daha fazla bilgi için bkz. [işlev uygulaması günlük düzeyini yapılandırma](#configure-the-function-app-log-level).

### <a name="configure-the-function-app-log-level"></a>İşlev uygulaması günlük düzeyini yapılandırma

Azure Işlevleri, Işlevlerin günlüklere yazma yöntemini denetlemenizi kolaylaştırmak için eşik düzeyini tanımlamanızı sağlar. Konsola yazılan tüm izlemelerin eşiğini ayarlamak için, `logging.logLevel.default` [Host. JSON başvurusu] [ `host.json` dosyasındaki]özelliğini kullanın. Bu ayar, işlev uygulamanızdaki tüm işlevler için geçerlidir.

Aşağıdaki örnek, tüm işlevler için ayrıntılı günlük kaydını etkinleştirmek üzere eşiği ayarlar, ancak şu adlı `MyFunction`bir işlev için hata ayıklama günlüğünü etkinleştirmek üzere eşiği ayarlar:

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

Daha fazla bilgi için bkz. [Host. JSON başvurusu].

### <a name="viewing-the-logs"></a>Günlükleri görüntüleme

İşlev Uygulaması Azure 'da çalışıyorsa, bunu izlemek için Application Insights kullanabilirsiniz. İşlev günlüklerini görüntüleme ve sorgulama hakkında daha fazla bilgi edinmek için [Azure işlevlerini izleme](functions-monitoring.md) makalesini okuyun.

Geliştirme için İşlev Uygulaması yerel olarak çalıştırıyorsanız, varsayılan olarak dosya sistemine kaydedilir. Konsolunda günlükleri görmek için, İşlev Uygulaması başlatmadan önce `AZURE_FUNCTIONS_ENVIRONMENT` ortam değişkenini olarak `Development` ayarlayın.

## <a name="triggers-and-bindings-types"></a>Tetikleyiciler ve bağlamalar türleri

İşlev uygulamanız ile kullanabileceğiniz birkaç tetikleyici ve bağlama vardır. Tetikleyiciler ve bağlamaların tam listesi [burada bulunabilir](functions-triggers-bindings.md#supported-bindings).

Tüm tetikleyiciler ve bağlamalar kodda birkaç gerçek veri türü olarak temsil edilir:

* Hashtable
* string
* Byte []
* int
* double
* HttpRequestContext
* HttpResponseContext

Bu listedeki ilk beş tür standart .NET türlerdir. Son ikisi yalnızca [httptrigger tetikleyicisi](#http-triggers-and-bindings)tarafından kullanılır.

İşlevinizdeki her bağlama parametresi bu türlerden biri olmalıdır.

### <a name="http-triggers-and-bindings"></a>HTTP Tetikleyicileri ve bağlamaları

Http ve Web kancası Tetikleyicileri ve HTTP çıkış bağlamaları, HTTP iletilerini temsil etmek için istek ve yanıt nesnelerini kullanır.

#### <a name="request-object"></a>İstek nesnesi

Betiğe geçirilen istek nesnesi, aşağıdaki özelliklere sahip olan türdür `HttpRequestContext`:

| Özellik  | Açıklama                                                    | Tür                      |
|-----------|----------------------------------------------------------------|---------------------------|
| **`Body`**    | İsteğin gövdesini içeren bir nesne. `Body`, verileri temel alan en iyi türe serileştirilir. Örneğin, veriler JSON ise, bir Hashtable olarak geçirilir. Veriler bir dizeyse, bir dize olarak geçirilir. | object |
| **`Headers`** | İstek üst bilgilerini içeren bir sözlük.                | Sözlük<dize, dize><sup>*</sup> |
| **`Method`** | İsteğin HTTP yöntemi.                                | string                    |
| **`Params`**  | İsteğin yönlendirme parametrelerini içeren nesne. | Sözlük<dize, dize><sup>*</sup> |
| **`Query`** | Sorgu parametrelerini içeren bir nesne.                  | Sözlük<dize, dize><sup>*</sup> |
| **`Url`** | İsteğin URL 'SI.                                        | string                    |

<sup>*</sup>Tüm `Dictionary<string,string>` anahtarlar büyük/küçük harfe duyarlıdır.

#### <a name="response-object"></a>Yanıt nesnesi

Geri göndermeniz gereken yanıt nesnesi, aşağıdaki özelliklere sahip olan türüdür `HttpResponseContext`:

| Özellik      | Açıklama                                                 | Tür                      |
|---------------|-------------------------------------------------------------|---------------------------|
| **`Body`**  | Yanıtın gövdesini içeren bir nesne.           | object                    |
| **`ContentType`** | Yanıt için içerik türünü ayarlamanın kısa bir tarafı. | string                    |
| **`Headers`** | Yanıt üst bilgilerini içeren bir nesne.               | Sözlük veya Hashtable   |
| **`StatusCode`**  | Yanıtın HTTP durum kodu.                       | dize veya tamsayı             |

#### <a name="accessing-the-request-and-response"></a>İstek ve yanıta erişme

HTTP tetikleyicilerle çalışırken, HTTP isteğine diğer giriş bağlamalarla aynı şekilde erişebilirsiniz. Bu, `param` bloğunda.

Aşağıdaki şekilde `HttpResponseContext` gösterildiği gibi bir yanıt döndürmek için bir nesnesi kullanın:

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

Bu işlevi çağırma sonucu şöyle olur:

```
PS > irm http://localhost:5001?Name=Functions
Hello Functions!
```

### <a name="type-casting-for-triggers-and-bindings"></a>Tetikleyiciler ve bağlamalar için tür atama

Blob bağlaması gibi bazı bağlamalar için, parametrenin türünü belirtebilirsiniz.

Örneğin, BLOB depolama alanından bir dize olarak sağlanan verilerin olmasını `param` engellemek için aşağıdaki tür cast 'u bloba ekleyin:

```powershell
param([string] $myBlob)
```

## <a name="powershell-profile"></a>PowerShell profili

PowerShell 'de bir PowerShell profili kavramı vardır. PowerShell profilleri hakkında bilgi sahibi değilseniz, bkz. [profiller hakkında](/powershell/module/microsoft.powershell.core/about/about_profiles).

PowerShell Işlevlerinde, işlev uygulaması başladığında profil betiği yürütülür. İşlev uygulamaları ilk dağıtıldığında ve kullanıldıktan sonra başlar ([soğuk başlatma](#cold-start)).

Visual Studio Code ve Azure Functions Core Tools gibi araçları kullanarak bir işlev uygulaması oluşturduğunuzda, sizin için varsayılan bir değer `profile.ps1` oluşturulur. Varsayılan profil, [temel araçlar GitHub deposunda](https://github.com/Azure/azure-functions-core-tools/blob/dev/src/Azure.Functions.Cli/StaticResources/profile.ps1) tutulur ve şunları içerir:

* Azure 'da otomatik MSI kimlik doğrulaması.
* İsterseniz Azure PowerShell `AzureRM` PowerShell diğer adlarını açma özelliği.

## <a name="powershell-version"></a>PowerShell sürümü

Aşağıdaki tabloda, Işlevler çalışma zamanının her ana sürümü tarafından kullanılan PowerShell sürümü gösterilmektedir:

| İşlevler sürümü | PowerShell sürümü                             |
|-------------------|------------------------------------------------|
| 'in               | Windows PowerShell 5,1 (çalışma zamanı tarafından kilitlendi) |
| 2.x               | PowerShell Core 6                              |

Geçerli sürümü herhangi bir işlevden yazdırarak `$PSVersionTable` görebilirsiniz.

## <a name="dependency-management"></a>Bağımlılık yönetimi

İşlevler, bağımlılıkları yönetmek için [PowerShell galerisinden](https://www.powershellgallery.com) yararlanmanızı sağlar. Bağımlılık yönetimi etkinken, gerekli modülleri otomatik olarak indirmek için requirements. psd1 dosyası kullanılır. Bu davranışı, `managedDependency` özelliği `true` [Host. json dosyasının](functions-host-json.md)kökünde, aşağıdaki örnekte olduğu gibi ayarlayarak etkinleştirirsiniz:

```json
{
  "managedDependency": {
          "enabled": true
       }
}
```

Yeni bir PowerShell işlevleri projesi oluşturduğunuzda, Azure [ `Az` modülü](/powershell/azure/new-azureps-module-az) dahil olmak üzere bağımlılık yönetimi varsayılan olarak etkindir. Şu anda desteklenen en fazla modül sayısı 10 ' dur. Desteklenen sözdizimi, aşağıdaki _`MajorNumber`_ `.*` gereksinimlerde gösterildiği gibi, modül sürümüdür. psd1 örnek:

```powershell
@{
    Az = '1.*'
    SqlServer = '21.1.18147'
}
```

Requirements. psd1 dosyasını güncelleştirdiğinizde, güncelleştirilmiş modüller yeniden başlatmadan sonra yüklenir.

> [!NOTE]
> Yönetilen bağımlılıklar, modülleri indirmek için www.powershellgallery.com 'e erişim gerektirir. Yerel olarak çalışırken, gerekli güvenlik duvarı kurallarını ekleyerek çalışma zamanının bu URL 'ye erişebildiğinizden emin olun. 

Aşağıdaki uygulama ayarları, yönetilen bağımlılıkların nasıl indirileceğini ve yükleneceğini değiştirmek için kullanılabilir. Uygulama yükseltmeniz içinde `MDMaxBackgroundUpgradePeriod`başlar ve yükseltme işlemi yaklaşık olarak içinde tamamlanır `MDNewSnapshotCheckPeriod`.

| İşlev Uygulaması ayarı              | Varsayılan değer             | Açıklama                                         |
|   -----------------------------   |   -------------------     |  -----------------------------------------------    |
| **`MDMaxBackgroundUpgradePeriod`**      | `7.00:00:00`(7 gün)     | Her PowerShell çalışan işlemi, işlem başlatma ve `MDMaxBackgroundUpgradePeriod` sonrasında PowerShell Galerisi modül yükseltmelerini denetlemeyi başlatır. PowerShell Galerisi yeni bir modül sürümü kullanılabilir olduğunda, dosya sistemine yüklenir ve PowerShell çalışanları için kullanılabilir hale getirilir. Bu değeri azaltmak, işlev uygulamanızın daha önce daha yeni modül sürümlerini almasını sağlar, ancak aynı zamanda uygulama kaynak kullanımını (ağ g/ç, CPU, depolama) de artırır. Bu değerin artırılması uygulamanın kaynak kullanımını düşürür, ancak uygulamanıza yeni modül sürümlerinin teslim edilmesini de erteleyebilir. | 
| **`MDNewSnapshotCheckPeriod`**         | `01:00:00`(1 saat)       | Yeni modül sürümleri dosya sistemine yüklendikten sonra, her PowerShell çalışan işleminin yeniden başlatılması gerekir. PowerShell çalışanlarını yeniden başlatmak, geçerli işlev yürütmesini kesintiye uğratmak için uygulamanızın kullanılabilirliğini etkiler. Tüm PowerShell çalışan süreçler yeniden başlatılana kadar, işlev etkinleştirmeleri eski veya yeni modül sürümlerini kullanabilir. Tüm PowerShell çalışanlarının yeniden başlatılması içinde `MDNewSnapshotCheckPeriod`tamamlanır. Bu değerin artırılması kesintiler sıklığını düşürür, ancak işlev etkinleştirmeleri eski veya yeni modül sürümlerini belirleyici olmayan şekilde kullandığınızda zaman dilimini de artırabilir. |
| **`MDMinBackgroundUpgradePeriod`**      | `1.00:00:00`(1 gün)     | Sık gerçekleştirilen çalışan yeniden başlatmalarının aşırı modül yükseltmelerini önlemek için, son `MDMinBackgroundUpgradePeriod`olarak bu denetimi başlatmış bir çalışan varsa modül yükseltmelerini denetlemek gerçekleştirilmez. |

Kendi özel modüllerinizi kullanmak, normal şekilde nasıl yapacağınıza göre biraz farklıdır.

Yerel bilgisayarınızda modül, içindeki genel kullanıma açık klasörlerden birine yüklenir `$env:PSModulePath`. Azure 'da çalışırken, makinenizde yüklü olan modüllere erişiminiz yok. Bu, bir PowerShell `$env:PSModulePath` işlev uygulamasının, normal bir PowerShell betiğinden farklı `$env:PSModulePath` olduğu anlamına gelir.

Işlevlerde iki yol `PSModulePath` içerir:

* İşlev `Modules` uygulamanızın kökünde bulunan bir klasör.
* PowerShell dil çalışanı tarafından `Modules` denetlenen bir klasörün yolu.

### <a name="function-app-level-modules-folder"></a>İşlev uygulama düzeyi `Modules` klasörü

Özel modülleri kullanmak için işlevlerinizin bir `Modules` klasöre bağlı olduğu modülleri yerleştirebilirsiniz. Bu klasörden modüller, işlevler çalışma zamanı tarafından otomatik olarak kullanılabilir. İşlev uygulamasındaki herhangi bir işlev bu modülleri kullanabilir. 

> [!NOTE]
> Requirements. psd1 dosyasında belirtilen modüller otomatik olarak indirilir ve bunları modüller klasörüne dahil etmeniz gerekmez. Bunlar, bulutta çalıştırıldığında `$env:LOCALAPPDATA/AzureFunctions` klasöründe ve `/data/ManagedDependencies` klasöründe yerel olarak depolanır.

Özel modül özelliğinden yararlanmak için, işlev uygulamanızın kökünde bir `Modules` klasör oluşturun. İşlevleriniz içinde kullanmak istediğiniz modülleri bu konuma kopyalayın.

```powershell
mkdir ./Modules
Copy-Item -Path /mymodules/mycustommodule -Destination ./Modules -Recurse
```

Bir `Modules` klasör ile, işlev uygulamanız aşağıdaki klasör yapısına sahip olmalıdır:

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

İşlev uygulamanızı başlattığınızda, PowerShell dil çalışanı bu `Modules` klasörü öğesine ekler. böylece, normal bir `$env:PSModulePath` PowerShell betiğiyle yaptığınız gibi, modül oto yüklemeye güvenebilirsiniz.

### <a name="language-worker-level-modules-folder"></a>Dil çalışanı düzeyi `Modules` klasörü

Birçok modül genellikle PowerShell dil çalışanı tarafından kullanılır. Bu modüller, öğesinin `PSModulePath`son konumunda tanımlanmıştır. 

Geçerli modüller listesi aşağıdaki gibidir:

* [Microsoft. PowerShell. Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive):, `.zip` `.nupkg`ve gibi Arşivlerle çalışmak için kullanılan modül.
* **Threadjob**: PowerShell iş API 'lerinin iş parçacığı tabanlı bir uygulamasıdır.

Varsayılan olarak, Işlevler bu modüllerin en son sürümünü kullanır. Belirli bir modül sürümünü kullanmak için, söz konusu sürümü işlev uygulamanızın `Modules` klasörüne yerleştirin.

## <a name="environment-variables"></a>Ortam değişkenleri

Işlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md), yürütme sırasında ortam değişkenleri olarak sunulur. Aşağıdaki örnekte gösterildiği gibi, kullanarak `$env:NAME_OF_ENV_VAR`bu ayarlara erişebilirsiniz:

```powershell
param($myTimer)

Write-Host "PowerShell timer trigger function ran! $(Get-Date)"
Write-Host $env:AzureWebJobsStorage
Write-Host $env:WEBSITE_SITE_NAME
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Yerel olarak çalışırken, uygulama ayarları [yerel. Settings. JSON](functions-run-local.md#local-settings-file) proje dosyasından okunmalıdır.

## <a name="concurrency"></a>Eşzamanlılık

Varsayılan olarak, Işlevler PowerShell çalışma zamanı tek seferde yalnızca bir işlevin çağrılmasını işleyebilir. Ancak, bu eşzamanlılık düzeyi aşağıdaki durumlarda yeterli olmayabilir:

* Aynı anda çok sayıda çağırma gerçekleştirmeye çalışırken.
* Aynı işlev uygulaması içindeki diğer işlevleri çağıran işlevlere sahip olduğunuzda.

Aşağıdaki ortam değişkenini bir tamsayı değerine ayarlayarak bu davranışı değiştirebilirsiniz:

```
PSWorkerInProcConcurrencyUpperBound
```

Bu ortam değişkenini İşlev Uygulaması [uygulama ayarlarında](functions-app-settings.md) ayarlarsınız.

### <a name="considerations-for-using-concurrency"></a>Eşzamanlılık kullanma konuları

PowerShell, varsayılan olarak _tek bir iş parçacıklı_ betik dilidir. Ancak, aynı işlemde birden fazla PowerShell çalışma alanı kullanılarak eşzamanlılık eklenebilir. Oluşturulan çalışma alanları miktarı PSWorkerInProcConcurrencyUpperBound uygulama ayarıyla eşleşir. Aktarım hızı, seçilen planda kullanılabilir olan CPU ve bellek miktarından etkilenecek.

Azure PowerShell, size fazla yazma işleminden tasarruf etmenize yardımcı olmak için bazı _işlem düzeyi_ bağlamlar ve durumları kullanır. Ancak, işlev uygulamanızda eşzamanlılık özelliğini açıp durumu değiştirme eylemlerini çağırdığınızda, yarış koşullarına sahip olabilirsiniz. Bir çağrı belirli bir duruma bağlı olduğundan ve diğer çağrının durumu değiştiğinden, bu yarış durumlarının hata ayıklaması zordur.

Bazı işlemler oldukça uzun sürebileceğinden, Azure PowerShell eşzamanlılık içinde de bir değer vardır. Ancak, dikkatli ilerlemeniz gerekir. Bir yarış durumu yaşadığınızı düşünüyorsanız, PSWorkerInProcConcurrencyUpperBound uygulama ayarını olarak `1` ayarlayın ve bunun yerine eşzamanlılık için [dil çalışan işlem düzeyi yalıtımı](functions-app-settings.md#functions_worker_process_count) kullanın.

## <a name="configure-function-scriptfile"></a>Configure işlevi`scriptFile`

Varsayılan olarak, karşılık gelen `run.ps1` `function.json`bir üst dizini paylaşan bir dosya olan bir PowerShell işlevi yürütülür.

İçindeki `scriptFile` `function.json` özelliği, aşağıdaki örnekte olduğu gibi görünen bir klasör yapısını almak için kullanılabilir:

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.ps1
```

Bu durumda, `function.json` için `myFunction` öğesine, çalıştırmak için `scriptFile` aktarılmış işlevi ile başvuran bir özelliği içerir.

```json
{
  "scriptFile": "../lib/PSFunction.ps1",
  "bindings": [
    // ...
  ]
}
```

## <a name="use-powershell-modules-by-configuring-an-entrypoint"></a>Giriş noktası yapılandırarak PowerShell modüllerini kullanma

Bu makalede, şablonlar tarafından oluşturulan varsayılan `run.ps1` betik dosyasında PowerShell işlevleri gösterilmektedir.
Ancak, işlevlerinizi PowerShell modüllerine de ekleyebilirsiniz. Function. json ' yapılandırma dosyasındaki `scriptFile` ve `entryPoint` alanlarını kullanarak modüldeki belirli işlevinizin koduna başvurabilirsiniz.

Bu durumda, `entryPoint` Içinde `scriptFile`başvurulan PowerShell modülündeki bir işlevin veya cmdlet 'in adıdır.

Aşağıdaki klasör yapısını göz önünde bulundurun:

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.psm1
```

Şunları `PSFunction.psm1` içerir:

```powershell
function Invoke-PSTestFunc {
    param($InputBinding, $TriggerMetadata)

    Push-OutputBinding -Name OutputBinding -Value "output"
}

Export-ModuleMember -Function "Invoke-PSTestFunc"
```

Bu örnekte, için `myFunction` yapılandırması, başka bir klasördeki `scriptFile` bir PowerShell modülü `PSFunction.psm1`olan başvuran bir özelliği içerir.  `entryPoint` Özelliği, modüldeki giriş `Invoke-PSTestFunc` noktası olan işlevine başvurur.

```json
{
  "scriptFile": "../lib/PSFunction.psm1",
  "entryPoint": "Invoke-PSTestFunc",
  "bindings": [
    // ...
  ]
}
```

Bu yapılandırmayla `Invoke-PSTestFunc` , tam olarak bir `run.ps1` olarak yürütülür.

## <a name="considerations-for-powershell-functions"></a>PowerShell işlevleriyle ilgili hususlar

PowerShell işlevleriyle çalışırken, aşağıdaki bölümlerde yer aldığını göz önünde bulundurun.

### <a name="cold-start"></a>Soğuk başlangıç

[Sunucusuz barındırma modelinde](functions-scale.md#consumption-plan)Azure işlevleri geliştirirken soğuk başlar. *Soğuk başlatma* , bir isteği işlemek için işlev uygulamanızın çalışmaya başlaması için gereken süreyi ifade eder. İşlev uygulamanız işlem yapılmayan dönemler sırasında kapandığı için, tüketim planında soğuk başlatma daha sık gerçekleşir.

### <a name="bundle-modules-instead-of-using-install-module"></a>Kullanmak yerine modülleri paketleyin`Install-Module`

Komut dosyası her çağrıdan çalıştırılır. `Install-Module` Betiğinizdeki kullanmaktan kaçının. Bunun yerine `Save-Module` yayımlamadan önce kullanın, böylece işlevinizin modül indirmede zaman harcanmasına sahip olmaması gerekir. Soğuk başlıyorsa işlevlerinizi etkilese, işlev uygulamanızı *her zaman açık* veya bir [Premium plana](functions-scale.md#premium-plan)ayarlanmış bir [App Service plana](functions-scale.md#app-service-plan) dağıtmaya göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure Işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)

[host.json başvurusu]: functions-host-json.md
