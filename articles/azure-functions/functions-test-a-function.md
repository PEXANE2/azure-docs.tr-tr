---
title: Azure İşlevlerini test etme
description: Visual Studio'da C# Fonksiyonu ve VS Kodu'nda JavaScript Fonksiyonu için otomatik testler oluşturma
author: craigshoemaker
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: cshoe
ms.openlocfilehash: a37fd886e1bc70226b2e54750540dfcb79ee5973
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75768886"
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a>Azure İşlevleri'nde kodunuzu test etmeye yönelik stratejiler

Bu makalede, Azure İşlevler için otomatik testlerin nasıl oluşturulacak gösteriş. 

Tüm kodu niçin sınamanız önerilir, ancak bir İşlev'in mantığını paketleyerek ve İşlev dışında testler oluşturarak en iyi sonuçları elde edebilirsiniz. Mantığın soyutlanması, Bir İşlevin kod satırlarını sınırlar ve Işlevin diğer sınıfları veya modülleri çağırmaktan yalnızca sorumlu olmasını sağlar. Ancak bu makalede, http ve zamanlayıcı tarafından tetiklenen işlevlere karşı otomatik testlerin nasıl oluşturulacak gösteriş olduğu gösterilmiştir.

Aşağıdaki içerik, farklı dilleri ve ortamları hedeflemek için iki farklı bölüme ayrılmıştır. Testler oluşturmayı öğrenebilirsiniz:

- [C# xUnit ile Visual Studio'da](#c-in-visual-studio)
- [JavaScript Jest ile VS Kodu](#javascript-in-vs-code)

Örnek deposu [GitHub'da](https://github.com/Azure-Samples/azure-functions-tests)mevcuttur.

## <a name="c-in-visual-studio"></a>Visual Studio’da C#
Aşağıdaki örnek, Visual Studio'da c# fonksiyonu uygulamasının nasıl oluşturulup [xUnit](https://xunit.github.io)ile nasıl çalıştırılıp test edilebildiğini açıklar.

![Visual Studio'da Azure Fonksiyonlarını C# ile Test Etme](./media/functions-test-a-function/azure-functions-test-visual-studio-xunit.png)

### <a name="setup"></a>Kurulum

Ortamınızı ayarlamak için bir Fonksiyon ve test uygulaması oluşturun. Aşağıdaki adımlar, testleri desteklemek için gereken uygulamaları ve işlevleri oluşturmanıza yardımcı olur:

1. [Yeni bir Fonksiyonlar uygulaması oluşturun](./functions-create-first-azure-function.md) ve *işlevlerini* adlandırın
2. [Şablondan bir HTTP işlevi oluşturun](./functions-create-first-azure-function.md) ve *httptrigger*adını.
3. [Şablondan bir zamanlayıcı işlevi oluşturun](./functions-create-scheduled-function.md) ve *timerTrigger*adını.
4. > Visual Studio'da **Visual Studio'> Visual C# > .NET Core > visual c# >** >'na tıklayarak [bir xUnit Test uygulaması oluşturun](https://xunit.github.io/docs/getting-started-dotnet-core) ve *fonksiyonlar.Test*adını alın. 
5. Test uygulamasından [Microsoft.AspNetCore.Mvc](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc/) adresine referans eklemek için NuGet'i kullanın
6. [Functions.Test uygulamasından *Fonksiyonlar* uygulamasına başvurun.](https://docs.microsoft.com/visualstudio/ide/managing-references-in-a-project?view=vs-2017) *Functions.Test*

### <a name="create-test-classes"></a>Test sınıfları oluşturma

Artık uygulamalar oluşturulduğuna göre, otomatik testleri çalıştırmak için kullanılan sınıfları oluşturabilirsiniz.

Her işlev, ileti günlüğe kaydetmeyi işlemek için [ILogger'ın](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) bir örneğini alır. Bazı testler iletileri günlüğe kaydetmez veya günlüğe kaydetmenin nasıl uygulandığıyla ilgili bir sorunu yoktur. Diğer testlerin, bir testin geçip geçmediğini belirlemek için günlüğe kaydedilen iletileri değerlendirmesi gerekir.

Sınıf `ListLogger` `ILogger` arabirimi uygular ve bir test sırasında değerlendirilmek için iletilerin dahili bir listesini tutar.

*Functions.Test* uygulamasına **sağ tıklayın** ve > **Sınıfı Ekle'yi**seçin, **NullScope.cs** ve aşağıdaki kodu girin:

```csharp
using System;

namespace Functions.Tests
{
    public class NullScope : IDisposable
    {
        public static NullScope Instance { get; } = new NullScope();

        private NullScope() { }

        public void Dispose() { }
    }
}
```

Ardından, *Functions.Test* uygulamasına **sağ tıklayın** ve **> Sınıf Ekle'yi**seçin, **ListLogger.cs** ve aşağıdaki kodu girin:

```csharp
using Microsoft.Extensions.Logging;
using System;
using System.Collections.Generic;
using System.Text;

namespace Functions.Tests
{
    public class ListLogger : ILogger
    {
        public IList<string> Logs;

        public IDisposable BeginScope<TState>(TState state) => NullScope.Instance;

        public bool IsEnabled(LogLevel logLevel) => false;

        public ListLogger()
        {
            this.Logs = new List<string>();
        }

        public void Log<TState>(LogLevel logLevel, 
                                EventId eventId,
                                TState state,
                                Exception exception,
                                Func<TState, Exception, string> formatter)
        {
            string message = formatter(state, exception);
            this.Logs.Add(message);
        }
    }
}
```

Sınıf, `ListLogger` `ILogger` arabirim tarafından sözleşme ile aşağıdaki üyeleri uygular:

- **BeginScope**: Kapsamlar günlüğe bağlam ekler. Bu durumda, test sadece `NullScope` test çalışmasına izin vermek için sınıftastatik örnek işaret eder.

- **IsEnabled**: Varsayılan `false` değeri sağlanır.

- **Günlük**: Bu yöntem, iletiyi biçimlendirmek için sağlanan `formatter` `Logs` işlevi kullanır ve ardından ortaya çıkan metni koleksiyona ekler.

Koleksiyon `Logs` bir örneğidir `List<string>` ve oluşturucuda baş harfe biştir.

Ardından, *Functions.Test* uygulamasına **sağ tıklayın** ve **> Sınıf Ekle'yi**seçin, **LoggerTypes.cs** ve aşağıdaki kodu girin:

```csharp
namespace Functions.Tests
{
    public enum LoggerTypes
    {
        Null,
        List
    }
}
```
Bu numaralandırma, testler tarafından kullanılan logger türünü belirtir. 

Ardından, *Functions.Test* uygulamasına **sağ tıklayın** ve **> Sınıf Ekle'yi**seçin, **TestFactory.cs** ve aşağıdaki kodu girin:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Http.Internal;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Logging.Abstractions;
using Microsoft.Extensions.Primitives;
using System.Collections.Generic;

namespace Functions.Tests
{
    public class TestFactory
    {
        public static IEnumerable<object[]> Data()
        {
            return new List<object[]>
            {
                new object[] { "name", "Bill" },
                new object[] { "name", "Paul" },
                new object[] { "name", "Steve" }

            };
        }

        private static Dictionary<string, StringValues> CreateDictionary(string key, string value)
        {
            var qs = new Dictionary<string, StringValues>
            {
                { key, value }
            };
            return qs;
        }

        public static DefaultHttpRequest CreateHttpRequest(string queryStringKey, string queryStringValue)
        {
            var request = new DefaultHttpRequest(new DefaultHttpContext())
            {
                Query = new QueryCollection(CreateDictionary(queryStringKey, queryStringValue))
            };
            return request;
        }

        public static ILogger CreateLogger(LoggerTypes type = LoggerTypes.Null)
        {
            ILogger logger;

            if (type == LoggerTypes.List)
            {
                logger = new ListLogger();
            }
            else
            {
                logger = NullLoggerFactory.Instance.CreateLogger("Null Logger");
            }

            return logger;
        }
    }
}
```
Sınıf `TestFactory` aşağıdaki üyeleri uygular:

- **Veri**: Bu özellik, örnek verilerin ayrılmaz bir koleksiyonunu [döndürür.](https://docs.microsoft.com/dotnet/api/system.collections.ienumerable) Anahtar değer çiftleri sorgu dizesine geçirilen değerleri temsil eder.

- **CreateDictionary**: Bu yöntem, bir anahtar/değer çiftini `Dictionary` bağımsız `QueryCollection` değişken olarak kabul eder ve sorgu dize değerlerini temsil etmek için kullanılan yeni bir değeri döndürür.

- **CreateHttpRequest**: Bu yöntem, verilen sorgu dize parametreleri ile başlaltımı ile başlaltımı bir HTTP isteği oluşturur.

- **CreateLogger**: Logger türüne bağlı olarak, bu yöntem test için kullanılan bir logger sınıfDöndürür. Günlüğe `ListLogger` kaydedilmiş iletilerin testleri değerlendirmeye hazır olduğunu izler.

Ardından, *Functions.Test* uygulamasına **sağ tıklayın** ve **> Sınıf Ekle'yi**seçin, **FunctionsTests.cs** adlandırın ve aşağıdaki kodu girin:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Logging;
using Xunit;

namespace Functions.Tests
{
    public class FunctionsTests
    {
        private readonly ILogger logger = TestFactory.CreateLogger();

        [Fact]
        public async void Http_trigger_should_return_known_string()
        {
            var request = TestFactory.CreateHttpRequest("name", "Bill");
            var response = (OkObjectResult)await HttpTrigger.Run(request, logger);
            Assert.Equal("Hello, Bill", response.Value);
        }

        [Theory]
        [MemberData(nameof(TestFactory.Data), MemberType = typeof(TestFactory))]
        public async void Http_trigger_should_return_known_string_from_member_data(string queryStringKey, string queryStringValue)
        {
            var request = TestFactory.CreateHttpRequest(queryStringKey, queryStringValue);
            var response = (OkObjectResult)await HttpTrigger.Run(request, logger);
            Assert.Equal($"Hello, {queryStringValue}", response.Value);
        }

        [Fact]
        public void Timer_should_log_message()
        {
            var logger = (ListLogger)TestFactory.CreateLogger(LoggerTypes.List);
            TimerTrigger.Run(null, logger);
            var msg = logger.Logs[0];
            Assert.Contains("C# Timer trigger function executed at", msg);
        }
    }
}
```
Bu sınıfta uygulanan üyeler şunlardır:

- **Http_trigger_should_return_known_string**: Bu test, bir HTTP `name=Bill` işlevine sorgu dize değerleri ile bir istek oluşturur ve beklenen yanıt Döndürülür denetler.

- **Http_trigger_should_return_string_from_member_data**: Bu test, HTTP işlevine örnek veri sağlamak için xUnit özniteliklerini kullanır.

- **Timer_should_log_message**: Bu test bir `ListLogger` örnek oluşturur ve bir zamanlayıcı işlevleri geçer. İşlev çalıştırıldıktan sonra, beklenen iletinin bulunduğundan emin olmak için günlük işaretlenir.

Testlerinizde uygulama ayarlarına erişmek istiyorsanız [System.Environment.GetEnvironmentVariable'i](./functions-dotnet-class-library.md#environment-variables)kullanabilirsiniz.

### <a name="run-tests"></a>Testleri çalıştırma

Testleri çalıştırmak için Test **Gezgini'ne** gidin ve **Tümlerini Çalıştır'ı**tıklatın.

![Visual Studio'da Azure Fonksiyonlarını C# ile Test Etme](./media/functions-test-a-function/azure-functions-test-visual-studio-xunit.png)

### <a name="debug-tests"></a>Hata ayıklama testleri

Testleri hata ayıklamak için, testte bir kesme noktası ayarlayın, **Test Gezgini'ne** gidin ve **Hata Ayıklama Son Çalıştırmayı Çalıştır >'ı**tıklatın.

## <a name="javascript-in-vs-code"></a>VS Kodunda JavaScript

Aşağıdaki örnek, VS Kodu'nda bir JavaScript İşlevi uygulamasının nasıl oluşturulup [Jest](https://jestjs.io)ile nasıl çalıştırılıp test edilebildiğini açıklar. Bu yordam, Azure İşlevleri oluşturmak için [VS Kodu İşlevleri uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) kullanır.

![Azure Fonksiyonlarını VS Kodunda JavaScript ile Test Etme](./media/functions-test-a-function/azure-functions-test-vs-code-jest.png)

### <a name="setup"></a>Kurulum

Ortamınızı ayarlamak için, boş bir klasörde yeni bir Düğüm.js uygulamasını çalıştırarak başlangıç olarak yayın. `npm init`

```bash
npm init -y
```
Ardından, aşağıdaki komutu çalıştırarak Jest yükleyin:

```bash
npm i jest
```
Şimdi aşağıdaki komutile mevcut test komutu değiştirmek için _package.json_ güncelleyin:

```bash
"scripts": {
    "test": "jest"
}
```

### <a name="create-test-modules"></a>Test modülleri oluşturma
Proje başharfle değiştirilirken, otomatik testleri çalıştırmak için kullanılan modülleri oluşturabilirsiniz. Destek modüllerini tutmak için *sınama* adlı yeni bir klasör oluşturarak başlayın.

*Test* klasöründe yeni bir dosya ekleyin, **varsayılan Context.js**adını ve aşağıdaki kodu ekleyin:

```javascript
module.exports = {
    log: jest.fn()
};
```
Bu modül, varsayılan yürütme bağlamını temsil etmek için *günlük* işleviyle alay eder.

Ardından, yeni bir dosya ekleyin, **varsayılan Timer.js**adını ve aşağıdaki kodu ekleyin:

```javascript
module.exports = {
    IsPastDue: false
};
```

Bu modül standı `IsPastDue` özelliği uygular sahte bir zamanlayıcı örneği olarak. Test koşumu yalnızca sonucu test etmek için işlevi doğrudan aradığından, NCRONTAB ifadeleri gibi zamanlayıcı yapılandırmaları burada gerekli değildir.

Ardından, [yeni bir JavaScript HTTP Fonksiyonu oluşturmak](/azure/javascript/tutorial-vscode-serverless-node-01) ve *httptrigger*adını VS Kodu Fonksiyonları uzantısı kullanın. İşlev oluşturulduktan sonra, **index.test.js**adlı klasöre yeni bir dosya ekleyin ve aşağıdaki kodu ekleyin:

```javascript
const httpFunction = require('./index');
const context = require('../testing/defaultContext')

test('Http trigger should return known text', async () => {

    const request = {
        query: { name: 'Bill' }
    };

    await httpFunction(context, request);

    expect(context.log.mock.calls.length).toBe(1);
    expect(context.res.body).toEqual('Hello Bill');
});
```
Şablondaki HTTP işlevi, sorgu dizesinde sağlanan adla birleştirilmiş bir "Merhaba" dizesini döndürür. Bu test, bir isteğin sahte bir örneğini oluşturur ve http işlevine geçirir. Test, *günlük* yönteminin bir kez çağrıldığını ve döndürülen metnin "Merhaba Fatura" ile eşit olduğunu denetler.

Ardından, yeni bir JavaScript Zamanlayıcı Fonksiyonu oluşturmak ve *TimerTrigger*adını almak için VS Kodu İşlevleri uzantısını kullanın. İşlev oluşturulduktan sonra, **index.test.js**adlı klasöre yeni bir dosya ekleyin ve aşağıdaki kodu ekleyin:

```javascript
const timerFunction = require('./index');
const context = require('../testing/defaultContext');
const timer = require('../testing/defaultTimer');

test('Timer trigger should log message', () => {
    timerFunction(context, timer);
    expect(context.log.mock.calls.length).toBe(1);
});
```
Şablondaki zamanlayıcı işlevi, işlevin gövdesinin sonundabir ileti kaydeder. Bu *test, günlük* işlevinin bir kez çağrılmasını sağlar.

### <a name="run-tests"></a>Testleri çalıştırma
Testleri çalıştırmak için komut penceresini açmak için **CTRL** `npm test`+ ~ tuşuna basın ve çalıştırın:

```bash
npm test
```

![Azure Fonksiyonlarını VS Kodunda JavaScript ile Test Etme](./media/functions-test-a-function/azure-functions-test-vs-code-jest.png)

### <a name="debug-tests"></a>Hata ayıklama testleri

Testlerinizi hata ayıklamak için *launch.json* dosyanıza aşağıdaki yapılandırmayı ekleyin:

```json
{
  "type": "node",
  "request": "launch",
  "name": "Jest Tests",
  "disableOptimisticBPs": true,
  "program": "${workspaceRoot}/node_modules/jest/bin/jest.js",
  "args": [
      "-i"
  ],
  "internalConsoleOptions": "openOnSessionStart"
}
```

Ardından, testinizde bir kesme noktası ayarlayın ve **F5 tuşuna**basın.

## <a name="next-steps"></a>Sonraki adımlar

Artık işlevleriniz için otomatik testler yazmayı öğrendiğinize göre, şu kaynaklarla devam edin:
- [HTTP ile tetiklenmeyen bir işlevi el ile çalıştırma](./functions-manually-run-non-http.md)
- [Azure Fonksiyonları hata işleme](./functions-bindings-error-pages.md)
- [Azure İşlevi Olay Izgara yerel hata ayıklama tetikleme](./functions-debug-event-grid-trigger-local.md)
