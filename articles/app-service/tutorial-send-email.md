---
title: 'Öğretici: Logic Apps ile e-posta gönderin'
description: Uygulama Hizmeti uygulamanızdan iş süreçlerini nasıl çağıracaklarınız öğrenin. E-posta, tweet ve Facebook gönderileri gönderin, posta listelerine ekleyin ve çok daha fazlasını ekleyin.
ms.topic: tutorial
ms.date: 04/08/2020
ms.custom: mvc
ms.openlocfilehash: 4073b49a134356943bd7da8d54bf574f2e0d5eea
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604844"
---
# <a name="tutorial-send-email-and-invoke-other-business-processes-from-app-service"></a>Öğretici: Uygulama Hizmeti'nden e-posta gönderin ve diğer iş süreçlerini çağırın

Bu eğitimde, Uygulama Hizmeti uygulamanızı iş süreçlerinizle nasıl entegre acağınızı öğrenirsiniz. Bu, web uygulaması senaryoları için yaygındır:

- Bir işlem için onay e-postası gönderme
- Facebook grubuna kullanıcı ekleme
- SAP, SalesForce, vb. gibi üçüncü taraf sistemlere bağlanın.
- Standart B2B mesajlarını değiştirme

Bu eğitimde, [Azure Logic Apps'ı](../logic-apps/logic-apps-overview.md)kullanarak Uygulama Hizmeti uygulamanızdan Gmail ile e-posta gönderebilirsiniz. Dil çerçeveniz tarafından sağlanan SMTP yapılandırması gibi bir web uygulamasından e-posta göndermenin başka yolları da vardır. Ancak Logic Apps, kodunuza karmaşıklık eklemeden Uygulama Hizmeti uygulamanıza çok daha fazla güç katar. Logic Apps, en popüler iş tümleştirmeleri için basit bir yapılandırma arabirimi sağlar ve uygulamanız bunları her zaman bir HTTP isteğiyle arayabilir.

## <a name="prerequisite"></a>Önkoşul

Seçtiğiniz dil çerçevesine sahip bir uygulamayı Uygulama Hizmeti'ne dağıtın. Örnek bir uygulamayı dağıtmak için bir öğreticiyi takip etmek için aşağıya bakın:

# <a name="aspnet"></a>[ASP.NET](#tab/dotnet)

[Öğretici: Azure’da SQL Veritabanı ile ASP.NET uygulaması derleme](app-service-web-tutorial-dotnet-sqldatabase.md)

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/dotnetcore)

[Öğretici: Azure Uygulama Hizmeti'nde ASP.NET Core ve SQL Veritabanı uygulaması oluşturun](app-service-web-tutorial-dotnetcore-sqldb.md)

# <a name="nodejs"></a>[Node.js](#tab/node)

[Öğretici: Azure'da Bir Düğüm.js ve MongoDB uygulaması oluşturun](app-service-web-tutorial-nodejs-mongodb-app.md)

# <a name="php"></a>[PHP](#tab/php)

[Öğretici: Azure'da PHP ve MySQL uygulaması oluşturun](app-service-web-tutorial-php-mysql.md)

# <a name="python"></a>[Python](#tab/python)

[Öğretici: Azure Uygulama Hizmetinde PostgreSQL ile Python (Django) web uygulamasını çalıştırın](containers/tutorial-python-postgresql-app.md)

# <a name="ruby"></a>[Ruby](#tab/ruby)

[Linux'ta Azure Uygulama Hizmeti'nde Ruby ve Postgres uygulaması oluşturun](containers/tutorial-ruby-postgres-app.md)

---

## <a name="create-the-logic-app"></a>Mantık Uygulaması Oluşturma

1. Azure [portalında,](https://portal.azure.com) [mantık uygulamanızı oluştur'daki](../logic-apps/quickstart-create-first-logic-app-workflow.md#create-your-logic-app)yönergeleri izleyerek boş bir mantık uygulaması oluşturun. Logic Apps **Designer'ı**gördüğünüzde, bu öğreticiye geri dönün.
1. Logic Apps Designer için sıçrama sayfasında, ortak bir **tetikleyiciyle Başlangıç**altında **bir HTTP isteği ne zaman alındığını** seçin.

    ![](./media/tutorial-send-email/receive-http-request.png)
1. **BIR HTTP isteği alındığı zaman**iletişim kutusunda, **şema oluşturmak için örnek yükü kullan'ı**seçin.

    ![](./media/tutorial-send-email/generate-schema-with-payload.png)

1. Aşağıdaki örnek JSON'u textbox'a kopyalayın ve **Bitti'yi**seçin.

    ```json
    {
        "task": "<description>",
        "due": "<date>",
        "email": "<email-address>"
    }
    ```

    Şema artık istediğiniz istek verileri için oluşturulur. Uygulamada, uygulama kodunuzu oluşturan gerçek istek verilerini yakalayabilir ve Azure'un sizin için JSON şemasını oluşturmasına izin verebilirsiniz. 
1. Logic Apps Designer'ın üst kısmında **Kaydet'i**seçin. 

    Artık HTTP istek tetikleyicinizin URL'sini görebilirsiniz. Daha sonra kullanmak üzere kopyalamak için kopya simgesini seçin.

    ![](./media/tutorial-send-email/http-request-url.png)

    Bu HTTP istek tanımı, Gmail veya başka bir şey olsun, bu mantık uygulamasında yapmak istediğiniz her şeyin tetikleyicisidir. Daha sonra bu URL'yi Uygulama Hizmeti uygulamanızda çağırırsınız. İstek tetikleyicisi hakkında daha fazla bilgi için [HTTP isteği/yanıt başvurusuna](../connectors/connectors-native-reqres.md)bakın.

1. Tasarımcının alt kısmında, **Yeni adım'ı**tıklatın, eylemler arama kutusuna **Gmail** yazın ve **e-posta gönder (V2)** bulup seçin.
    
    > [!TIP]
    > SendGrid, MailChimp, Office 365 ve SalesForce gibi diğer tümleştirme türlerini arayabilirsiniz. Daha fazla bilgi için [Logic Apps belgelerine](https://docs.microsoft.com/azure/logic-apps/)bakın.
1. **Gmail** iletişim kutusunda, **Oturum Aç'ı** seçin ve e-postayı göndermek istediğiniz Gmail hesabında oturum açın.

    ![](./media/tutorial-send-email/gmail-sign-in.png)

1. Oturum açtıktan **sonra, To** textbox'ına tıklayın ve dinamik içerik iletişim kutusu otomatik olarak açılır.

1. **BIR HTTP isteği eylem aldığında** **yanında, daha fazla bilgi al'ı**seçin.

    ![](./media/tutorial-send-email/expand-dynamic-content.png)

    Şimdi örnek JSON verilerinden daha önce kullandığınız üç özelliği görmeniz gerekir. Bu adımda, bir e-posta oluşturmak için HTTP isteğibu özellikleri kullanın.
1. **To** alanının değerini seçtiğinizden, **e-postayı**seçin. İsterseniz dinamik **içerik ekle'yi**tıklatarak dinamik içerik iletişim kutusunu kapatın.

    ![](./media/tutorial-send-email/hide-dynamic-content.png)

1. Yeni **parametre** açılır ekle'de **Özne** ve **Gövde'yi**seçin.

1. **Konu** metin kutusuna tıklayın ve aynı şekilde **görevi**seçin. **İmleç** hala Konu kutusunda yken, *oluşturulan*türü yazın. 

1. **Gövde'ye**tıklayın ve aynı **şekilde, nedeniyle**seçin . İmleci **nedeniyle** soluna taşıyın ve *bu çalışma öğesinin vadesi dolduğu*yazın.

    > [!TIP]
    > HTML içeriğini doğrudan e-posta gövdesinde güncellemek istiyorsanız, Logic Apps Designer penceresinin üst kısmındaki **Kod görünümü'nü** seçin. Dinamik içerik kodunu koruduğunuzdan emin olun `@{triggerBody()?['due']}`(örneğin, )
    >
    > ![](./media/tutorial-send-email/edit-rich-html-email.png) 

1. Ardından, HTTP tetikleyicisine eşzamanlı bir HTTP yanıtı ekleyin. HTTP tetikleyicisi ve Gmail eylemi **+** arasında, işareti tıklatın ve **paralel dal ekle'yi**seçin.

    ![](./media/tutorial-send-email/add-http-response.png)

1. Arama kutusunda yanıt **ara,** ardından **Yanıt** eylemini seçin.

    ![](./media/tutorial-send-email/choose-response-action.png)

    Varsayılan olarak, yanıt eylemi bir HTTP 200 gönderir. Bu öğretici için yeterince iyi. Daha fazla bilgi için [HTTP isteği/yanıt başvurusuna](../connectors/connectors-native-reqres.md)bakın.

1. Logic Apps Designer'ın en üstünde, yeniden **Kaydet'i** seçin. 

## <a name="add-http-request-code-to-app"></a>Uygulamaya HTTP istek kodu ekleme

HTTP istek tetikleyicisinin URL'sini daha öncekilerden kopyaladığınızdan emin olun. Hassas bilgiler içerdiğinden, doğrudan koda koymamanız en iyi yöntemdir. Uygulama Hizmeti ile, uygulama ayarlarını kullanarak bunun yerine bir ortam değişkeni olarak başvuruda bulunabilirsiniz. 

Bulut [Kabuğu'nda,](https://shell.azure.com)aşağıdaki komutla uygulama ayarını oluşturun * \<(uygulama adı>, * * \<kaynak grubu adı>* ve * \<mantık-app-url>: *

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings LOGIC_APP_URL="<your-logic-app-url>"
```

Kodunuzda, dil çerçevenizde kullanılabilen herhangi bir HTTP istemci dilini kullanarak URL'ye aşağıdaki yapılandırmayla standart bir HTTP gönderisi yapın:

- İstek gövdesi, mantık uygulamanıza sağladığınız JSON biçimini içerir:

    ```json
    {
        "task": "<description>",
        "due": "<date>",
        "email": "<email-address>"
    }
    ```

- İstek başlığı `Content-Type: application/json`nı içerir. 
- Performansı en iyi duruma getirmek için, isteği mümkünse eşit olarak gönderin.

Bir örneği görmek için aşağıdaki tercih edilen dil/çerçeve sekmesine tıklayın.

# <a name="aspnet"></a>[ASP.NET](#tab/dotnet)

ASP.NET olarak, [System.Net.Http.HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) sınıfı ile HTTP sonrası gönderebilirsiniz. Örneğin:

```csharp
// requires using System.Net.Http;
var client = new HttpClient();
// requires using System.Text.Json;
var jsonData = JsonSerializer.Serialize(new
{
    email = "a-valid@emailaddress.com",
    due = "4/1/2020",
    task = "My new task!"
});

HttpResponseMessage result = await client.PostAsync(
    // requires using System.Configuration;
    ConfigurationManager.AppSettings["LOGIC_APP_URL"],
    new StringContent(jsonData, Encoding.UTF8, "application/json"));
    
var statusCode = result.StatusCode.ToString();
```

Bu kodu Öğretici için örnek uygulamada test ediyorsanız: [SQL Veritabanı ile Azure'da bir ASP.NET uygulaması oluşturun](app-service-web-tutorial-dotnet-sqldatabase.md), `Todo` öğe eklendikten sonra Oluşturma [eyleminde](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/blob/master/DotNetAppSqlDb/Controllers/TodosController.cs#L52-L63)bir e-posta onayı göndermek için kullanabilirsiniz. Yukarıdaki eşzamanlı kodu kullanmak için, Eylem Oluştur'u asynchronous'a dönüştürün.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/dotnetcore)

ASP.NET Core'da, [System.Net.Http.HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) sınıfı ile HTTP gönderisini gönderebilirsiniz. Örneğin:

```csharp
// requires using System.Net.Http;
var client = new HttpClient();
// requires using System.Text.Json;
var jsonData = JsonSerializer.Serialize(new
{
    email = "a-valid@emailaddress.com",
    due = "4/1/2020",
    task = "My new task!"
});

HttpResponseMessage result = await client.PostAsync(
    // Requires DI configuration to access app settings. See https://docs.microsoft.com/azure/app-service/containers/configure-language-dotnetcore#access-environment-variables
    _configuration["LOGIC_APP_URL"],
    new StringContent(jsonData, Encoding.UTF8, "application/json"));
    
var statusCode = result.StatusCode.ToString();
```

> [!NOTE]
> Bu kod gösteri basitliği için yazılmıştır. Uygulamada, her istek için bir `HttpClient` nesneyi anında atmayın. Esnek HTTP [isteklerini uygulamak için IHttpClientFactory'i kullan'daki](https://docs.microsoft.com/dotnet/architecture/microservices/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)kılavuzu izleyin.

Bu kodu Öğretici için örnek uygulamada test ediyorsanız: [Azure Uygulama Hizmeti'nde bir ASP.NET Core ve SQL Veritabanı uygulaması oluşturun](app-service-web-tutorial-dotnetcore-sqldb.md), `Todo` öğe eklendikten sonra Oluşturma [eyleminde](https://github.com/Azure-Samples/dotnetcore-sqldb-tutorial/blob/master/Controllers/TodosController.cs#L56-L65)bir e-posta onayı göndermek için kullanabilirsiniz.

# <a name="nodejs"></a>[Node.js](#tab/node)

Node.js olarak, [axios](https://www.npmjs.com/package/axios)gibi bir npm paketi ile kolayca HTTP sonrası gönderebilirsiniz. Örneğin:

```javascript
// Requires npm install --save axios
const axios = require('axios');

var jsonData = {
        email: "a-valid@emailaddress.com",
        due: "4/1/2020",
        task: "My new task!"
};

(async function(data) {
    try {
        const response = await axios.post(process.env.LOGIC_APP_URL, jsonData);
        console.log(response.status);
    } catch (error) {
        console.log(error);
    }
})(jsonData);

```

Bu kodu Öğretici için örnek uygulamada test [ediyorsanız: Azure'da bir Düğüm.js ve MongoDB uygulaması oluşturun](app-service-web-tutorial-nodejs-mongodb-app.md), makale [başarıyla kaydedildikten](https://github.com/Azure-Samples/meanjs/blob/master/modules/articles/server/controllers/articles.server.controller.js#L24)sonra [oluşturma işlevinde](https://github.com/Azure-Samples/meanjs/blob/master/modules/articles/server/controllers/articles.server.controller.js#L14-L27)bir e-posta onayı göndermek için kullanabilirsiniz.

# <a name="php"></a>[PHP](#tab/php)

PHP olarak, [Guzzle](http://docs.guzzlephp.org/en/stable/index.html)ile kolayca HTTP sonrası gönderebilirsiniz. Örneğin:

```php
// Requires composer require guzzlehttp/guzzle:~6.0
use GuzzleHttp\Client;
...
$client = new Client();
$options = [
    'json' => [ 
        'email' => 'a-valid@emailaddress.com',
        'due' => '4/1/2020',
        'task' => "My new task!"
    ]
];

$promise = $client-> postAsync(getenv($LOGIC_APP_URL), $options)->then( 
    function ($response) {
        return $response->getStatusCode();
    }, function ($exception) {
        return $exception->getResponse();
    }
);

$response = $promise->wait();
// Requires Laravel to run Log::info(). Check the documentation of your preferred framework for logging instructions.
Log::info(print_r($response, TRUE));
```

Bu kodu Öğretici için örnek uygulamada test [ediyorsanız: Azure'da bir PHP ve MySQL uygulaması oluşturun](app-service-web-tutorial-php-mysql.md), return deyiminden hemen önce [Route::post işlevinde](https://github.com/Azure-Samples/laravel-tasks/blob/master/routes/web.php#L30-L48)bir e-posta onayı göndermek için kullanabilirsiniz.

# <a name="python"></a>[Python](#tab/python)

Python'da HTTP gönderisini [isteklerle](https://pypi.org/project/requests/)kolayca gönderebilirsiniz. Örneğin:

```python
# Requires pip install requests && pip freeze > requirements.txt
import requests
...
payload = {
    "email": "a-valid@emailaddress.com",
    "due": "4/1/2020",
    "task": "My new task!"
}
response = requests.post("https://prod-112.westeurope.logic.azure.com:443/workfl$
print(response.status_code)
```
<!-- ```python
# Requires pip install aiohttp && pip freeze > requirements.txt
import aiohttp
...
payload = {
        'email': 'a-valid@emailaddress.com',
        'due': '4/1/2020',
        'task': 'My new task!'
}
async with aiohttp.post('http://httpbin.org/post', data=json.dump(payload)) as resp:
    print(await resp.status())
``` -->

Bu kodu Tutorial: Run a [Python (Django) web uygulamasını Azure App Service'de PostgreSQL ile](containers/tutorial-python-postgresql-app.md)test ediyorsanız, iade bildiriminden hemen önce [Route::post işlevinde](https://github.com/Azure-Samples/laravel-tasks/blob/master/routes/web.php#L30-L48)bir e-posta onayı göndermek için kullanabilirsiniz.

# <a name="ruby"></a>[Ruby](#tab/ruby)

Ruby, [kolayca JSONClient](https://www.rubydoc.info/gems/httpclient/JSONClient)ile HTTP sonrası gönderebilirsiniz. Örneğin:

```ruby
clnt = JSONClient.new
body = { 
    'email' => 'a-valid@emailaddress.com',
    'due' => '4/1/2020',
    'task' => "My new task!"
}

connection = clnt.post_async(ENV['LOGIC_APP_URL'], body)
```

Bu kodu Linux'taki Azure Uygulama [Hizmeti'nde Ruby ve Postgres uygulaması Oluşturma](containers/tutorial-ruby-postgres-app.md)için örnek uygulamada test ediyorsanız, bunu [oluşturma](https://github.com/Azure-Samples/rubyrails-tasks/blob/master/app/controllers/tasks_controller.rb#L26-L38) eyleminde başarılı [olduğunda @task.save ](https://github.com/Azure-Samples/rubyrails-tasks/blob/master/app/controllers/tasks_controller.rb#L30)bir e-posta onayı göndermek için kullanabilirsiniz.

---

# <a name="more-resources"></a>Diğer kaynaklar

[Öğretici: Azure App Service’te CORS ile RESTful API barındırma](app-service-web-tutorial-rest-api.md)  
[Logic Apps için HTTP istek/yanıt başvurusu](../connectors/connectors-native-reqres.md)  
[Hızlı başlangıç: Azure Logic Apps - Azure portalını kullanarak ilk iş akışınızı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md)