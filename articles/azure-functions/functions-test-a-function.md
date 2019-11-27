---
title: Azure İşlevlerini test etme
description: Otomatik testler için oluşturma bir C# işlevi Visual Studio ve VS code'da JavaScript işlevi
author: craigshoemaker
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: cshoe
ms.openlocfilehash: c60cd631e703f929eaae56138a2acd3687121924
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74226586"
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a>Kodunuzu Azure işlevleri'nde test stratejileri

Bu makalede, Azure işlevleri için otomatik testler oluşturma işlemini gösterir. 

Tüm kod sınama önerilir, ancak bir işlevin logic kaydırma ve işlev dışındaki testler oluşturma en iyi sonuçlar elde edebilirsiniz. Mantıksal hemen özetleyen bir işlevin satır kod sınırlar ve işlevi diğer sınıfları veya modülleri çağırmak için sorumlu sağlar. Bu makalede, ancak otomatik testler karşı bir HTTP ve Zamanlayıcı ile tetiklenen işlev oluşturma işlemini gösterir.

Aşağıdaki içeriği hedef farklı dilleri ve ortamları yönelik iki farklı bölümlere ayrılır. Testleri oluşturmayı öğrenebilirsiniz:

- [C#xUnit ile Visual Studio 'da](#c-in-visual-studio)
- [Jest ile VS Code JavaScript](#javascript-in-vs-code)

Örnek depo [GitHub](https://github.com/Azure-Samples/azure-functions-tests)' da kullanılabilir.

## <a name="c-in-visual-studio"></a>C#Visual Studio'da
Aşağıdaki örnek, Visual Studio 'da bir C# işlev uygulamasının nasıl oluşturulduğunu ve [xUnit](https://xunit.github.io)ile nasıl çalıştırılacağını ve test edileceğini açıklar.

![İle Azure işlevlerini test etme C# Visual Studio'da](./media/functions-test-a-function/azure-functions-test-visual-studio-xunit.png)

### <a name="setup"></a>Kurulum

Ortamınızı ayarlamak için bir işlev oluşturun ve uygulamayı test etme. Aşağıdaki adımlar, uygulama ve testleri desteklemek için gereken işlevleri oluşturmanıza yardımcı:

1. [Yeni bir işlevler uygulaması oluşturun](./functions-create-first-azure-function.md) ve *işlevleri* adlandırın
2. [Şablondan BIR http Işlevi oluşturun](./functions-create-first-azure-function.md) ve *httptrigger*olarak adlandırın.
3. [Şablondan bir Zamanlayıcı Işlevi oluşturun](./functions-create-scheduled-function.md) ve *timertrigger*olarak adlandırın.
4. Visual Studio 'da [bir xUnit test uygulaması oluşturun](https://xunit.github.io/docs/getting-started-dotnet-core) **> yeni > projesi > Visual C# > .NET Core > xUnit test projesi** ve adı *Functions. test*. 
5. [Microsoft. AspNetCore. Mvc](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc/) test uygulamasından bir başvuru eklemek için NuGet kullanın
6. [ ](https://docs.microsoft.com/visualstudio/ide/managing-references-in-a-project?view=vs-2017) Functions *. test* uygulamasındaki işlevler uygulamasına başvurun.

### <a name="create-test-classes"></a>Test sınıfları oluşturma

Uygulamaları oluşturulan, otomatik testleri çalıştırmak için kullanılan sınıfları oluşturabilirsiniz.

Her işlev, ileti günlüğünü işlemek için bir [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) örneğini alır. Bazı testler olmayan iletileri günlüğe ya da günlük nasıl uygulandığını için herhangi bir sorun vardır. Bir test geçmediği belirlenemiyor oturum iletileri değerlendirmek diğer testlerin gerekir.

`ListLogger` sınıfı, `ILogger` arabirimini uygulamak ve test sırasında değerlendirme için iletilerin iç listesini tutmak içindir.

*İşlevler. test* uygulaması **' na sağ tıklayın** ve **> sınıfı ekle**' yi seçin, **NullScope.cs** olarak adlandırın ve aşağıdaki kodu girin:

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

Ardından, *işlevler. test* uygulaması **' na sağ tıklayın** ve **> sınıfı ekle**' yi seçin, **ListLogger.cs** olarak adlandırın ve aşağıdaki kodu girin:

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

`ListLogger` sınıfı, `ILogger` arabirimi tarafından anlaşmalı olarak aşağıdaki üyeleri uygular:

- **BeginScope**: kapsamlar günlüğe kaydetme uygulamanıza bağlam ekler. Bu durumda, test, testin işlevine izin vermek için `NullScope` sınıfında statik örneği işaret eder.

- **IsEnabled**: `false` varsayılan değeri sağlanır.

- **GNLK**: Bu yöntem, iletiyi biçimlendirmek için sağlanan `formatter` işlevini kullanır ve sonra elde edilen metni `Logs` koleksiyonuna ekler.

`Logs` koleksiyonu bir `List<string>` örneğidir ve oluşturucuda başlatılır.

Ardından, *işlevler. test* uygulaması **' na sağ tıklayın** ve **> sınıfı ekle**' yi seçin, **LoggerTypes.cs** olarak adlandırın ve aşağıdaki kodu girin:

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
Bu numaralandırma testleri tarafından kullanılan Günlükçü türünü belirtir. 

Ardından, *işlevler. test* uygulaması **' na sağ tıklayın** ve **> sınıfı ekle**' yi seçin, **TestFactory.cs** olarak adlandırın ve aşağıdaki kodu girin:

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
`TestFactory` sınıfı aşağıdaki üyeleri uygular:

- **Veri**: Bu özellik, örnek verilerin bir [IEnumerable](https://docs.microsoft.com/dotnet/api/system.collections.ienumerable) koleksiyonunu döndürür. Anahtar-değer çiftlerinin bir sorgu dizesinde geçirilen değerleri temsil eder.

- **CreateDictionary**: Bu yöntem, anahtar/değer çiftini bağımsız değişken olarak kabul eder ve sorgu dizesi değerlerini temsil etmek üzere `QueryCollection` oluşturmak için kullanılan yeni bir `Dictionary` döndürür.

- **Createhttprequest**: Bu yöntem, belirtilen sorgu dizesi parametreleriyle BAŞLATıLAN bir http isteği oluşturur.

- **Creategünlükçü**: günlükçü türü temelinde, bu yöntem test için kullanılan bir günlükçü sınıfı döndürür. `ListLogger`, testlerin değerlendirmesi için kullanılabilen günlüğe kaydedilen iletileri izler.

Ardından, *işlevler. test* uygulaması **' na sağ tıklayın** ve **> sınıfı ekle**' yi seçin, **FunctionsTests.cs** olarak adlandırın ve aşağıdaki kodu girin:

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
Bu sınıfta uygulanır üyeleri şunlardır:

- **Http_trigger_should_return_known_string**: Bu test, bir Http işlevine `name=Bill` sorgu dizesi değerleriyle bir istek oluşturur ve beklenen yanıtın döndürülüp döndürülmediğini denetler.

- **Http_trigger_should_return_string_from_member_data**: Bu test, http işlevine örnek veriler sağlamak Için xUnit özniteliklerini kullanır.

- **Timer_should_log_message**: Bu test bir `ListLogger` örneği oluşturur ve bunu bir Zamanlayıcı işlevlerine geçirir. İşlevi çalıştırıldığında, günlük, beklenen bir ileti mevcut olduğundan emin olmak için denetlenir.

Testlerinizde uygulama ayarlarına erişmek istiyorsanız [System. Environment. GetEnvironmentVariable](./functions-dotnet-class-library.md#environment-variables)kullanabilirsiniz.

### <a name="run-tests"></a>Testleri çalıştırma

Testleri çalıştırmak için **Test Gezgini** ' ne gidin ve **Tümünü Çalıştır**' a tıklayın.

![İle Azure işlevlerini test etme C# Visual Studio'da](./media/functions-test-a-function/azure-functions-test-visual-studio-xunit.png)

### <a name="debug-tests"></a>Testlerde Hata Ayıkla

Testlerde hata ayıklamak için bir test üzerine bir kesme noktası ayarlayın, **Test Gezgini** 'ne gidin ve **son çalıştırma > hata ayıkla Çalıştır**' a tıklayın.

## <a name="javascript-in-vs-code"></a>VS code'da JavaScript

Aşağıdaki örnek, VS Code ' de bir JavaScript Işlev uygulamasının nasıl oluşturulduğunu ve [jest](https://jestjs.io)ile nasıl çalıştırılacağını ve test edileceğini açıklar. Bu yordam, Azure Işlevleri oluşturmak için [vs Code işlevleri uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) kullanır.

![VS Code'da JavaScript ile Azure işlevlerini test etme](./media/functions-test-a-function/azure-functions-test-vs-code-jest.png)

### <a name="setup"></a>Kurulum

Ortamınızı ayarlamak için `npm init`çalıştırarak boş bir klasörde yeni bir Node. js uygulaması başlatın.

```bash
npm init -y
```
Ardından, Jest aşağıdaki komutu çalıştırarak yükleyin:

```bash
npm i jest
```
Şimdi var olan test komutunu aşağıdaki komutla değiştirmek için _Package. JSON_ ' i güncelleştirin:

```bash
"scripts": {
    "test": "jest"
}
```

### <a name="create-test-modules"></a>Test modülleri oluşturma
Başlatılan proje ile otomatik testleri çalıştırmak için kullanılan modüller oluşturabilirsiniz. Destek modüllerini tutmak için *Test* adlı yeni bir klasör oluşturarak başlayın.

*Test* klasöründe yeni bir dosya ekleyin, **DefaultContext. js**olarak adlandırın ve aşağıdaki kodu ekleyin:

```javascript
module.exports = {
    log: jest.fn()
};
```
Bu modül, varsayılan yürütme bağlamını göstermek için *günlük* işlevini gizler.

Sonra, yeni bir dosya ekleyin, **Defaulttimer. js**olarak adlandırın ve aşağıdaki kodu ekleyin:

```javascript
module.exports = {
    IsPastDue: false
};
```

Bu modül, `IsPastDue` özelliğini tek başına sahte bir zamanlayıcı örneği olarak uygular. Test bandı yalnızca sonucu test etmek için işlevi çağırarak, NCRONTAB ifadeleri gibi Zamanlayıcı konfigürasyonları burada gerekli değildir.

Ardından, [Yeni bir JAVASCRIPT http işlevi oluşturmak](/azure/javascript/tutorial-vscode-serverless-node-01) ve *httptrigger*olarak adlandırmak için vs Code işlevleri uzantısını kullanın. İşlev oluşturulduktan sonra **index. test. js**adlı aynı klasöre yeni bir dosya ekleyin ve aşağıdaki kodu ekleyin:

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
Şablondan HTTP işlevi sorgu dizesinde sağlanan ad ile birleştirilmiş "Hello" dizesi döndürür. Bu test, istek sahte bir örneğini oluşturur ve HTTP işleve geçirir. Test, *günlük* yönteminin bir kez çağrıldığını ve döndürülen metnin "Hello Bill" değerine eşit olduğunu denetler.

Ardından, yeni bir JavaScript Zamanlayıcı Işlevi oluşturmak ve bunu *Timertrigger*olarak adlandırmak Için vs Code işlevleri uzantısını kullanın. İşlev oluşturulduktan sonra **index. test. js**adlı aynı klasöre yeni bir dosya ekleyin ve aşağıdaki kodu ekleyin:

```javascript
const timerFunction = require('./index');
const context = require('../testing/defaultContext');
const timer = require('../testing/defaultTimer');

test('Timer trigger should log message', () => {
    timerFunction(context, timer);
    expect(context.log.mock.calls.length).toBe(1);
});
```
Şablondan Zamanlayıcı işlevi, işlev gövdesinin sonuna bir iletiyi günlüğe kaydeder. Bu test, *günlük* işlevinin bir kez çağrıldığından emin olmanızı sağlar.

### <a name="run-tests"></a>Testleri çalıştırma
Testleri çalıştırmak için **CTRL + ~** tuşlarına basarak komut penceresini açın ve `npm test`çalıştırın:

```bash
npm test
```

![VS Code'da JavaScript ile Azure işlevlerini test etme](./media/functions-test-a-function/azure-functions-test-vs-code-jest.png)

### <a name="debug-tests"></a>Testlerde Hata Ayıkla

Testlerinizde hata ayıklamak için, aşağıdaki yapılandırmayı *Launch. JSON* dosyanıza ekleyin:

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

Sonra, testinizde bir kesme noktası ayarlayın ve **F5**tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

İşlevleriniz için otomatik testler yazmak öğrendiniz, bu kaynakları ile devam edin:
- [HTTP ile tetiklenen bir işlevi el ile çalıştırma](./functions-manually-run-non-http.md)
- [Azure Işlevleri hata işleme](./functions-bindings-error-pages.md)
- [Azure Işlevi Event Grid yerel hata ayıklamayı Tetikle](./functions-debug-event-grid-trigger-local.md)
