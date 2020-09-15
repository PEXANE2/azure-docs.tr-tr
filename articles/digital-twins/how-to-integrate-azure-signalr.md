---
title: Azure SignalR Hizmeti ile tümleştirme
titleSuffix: Azure Digital Twins
description: Azure SignalR kullanarak Azure dijital TWINS telemetrisini istemcilere nasıl akışla kullanabileceğinizi öğrenin
author: dejimarquis
ms.author: aymarqui
ms.date: 09/02/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 38e3526627eb4191643f8bc86b9ce5f49e41a71f
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90564415"
---
# <a name="integrate-azure-digital-twins-with-azure-signalr-service"></a>Azure Digital TWINS 'i Azure SignalR hizmeti ile tümleştirme

Bu makalede, Azure Digital TWINS 'i [Azure SignalR hizmeti](../azure-signalr/signalr-overview.md)ile tümleştirmeyi öğreneceksiniz.

Bu makalede açıklanan çözüm, dijital ikizi telemetri verilerini, tek bir Web sayfası veya bir mobil uygulama gibi bağlı istemcilere anında gönderebilmeniz için izin verir. Sonuç olarak, istemciler, gerçek zamanlı ölçümler ve durum, sunucu yoklamaları veya güncelleştirmeler için yeni HTTP istekleri göndermek zorunda kalmadan IoT cihazlarından güncellenir.

## <a name="prerequisites"></a>Önkoşullar

Devam etmeden önce gerçekleştirmeniz gereken önkoşullar şunlardır:

* Çözümünüzü Azure SignalR hizmeti ile tümleştirmadan önce bu makaledeki Azure dijital Twins [_**öğreticisini tamamlamalısınız:**_](tutorial-end-to-end.md)bu, nasıl yapılır, bu da bunun üzerine nasıl oluşturulur. Öğreticide, dijital ikizi güncelleştirmelerini tetiklemek için bir sanal IoT cihazıyla birlikte çalışarak bir Azure dijital TWINS örneği ayarlama işlemi adım adım gösterilmektedir. Bu şekilde, Azure SignalR hizmeti kullanılarak bu güncelleştirmeler örnek bir Web uygulamasına nasıl bağlanır.
    - Öğreticide oluşturduğunuz **olay Kılavuzu konusunun** adı gerekir.
* Makinenizde [**Node.js**](https://nodejs.org/) yüklü olmalıdır.

Ayrıca, Azure hesabınızla [Azure Portal](https://portal.azure.com/) bir sonraki adımda da oturum açabilirsiniz.

## <a name="solution-architecture"></a>Çözüm mimarisi

Azure SignalR hizmetini aşağıdaki yoldan Azure dijital TWINS 'e iliştirirsiniz. Diyagramdaki A, B ve C bölümleri, [uçtan uca öğretici önkoşulun](tutorial-end-to-end.md)mimari diyagramından alınmıştır; Bu nasıl yapılır bölümünde, bu bölümü, D bölümünü ekleyerek oluşturacaksınız.

:::image type="content" source="media/how-to-integrate-azure-signalr/signalr-integration-topology.png" alt-text="Uçtan uca bir senaryoda Azure hizmetlerinin bir görünümü. Bir cihazdan bir Azure işlevi (ok B) aracılığıyla bir Azure dijital TWINS örneğine (Bölüm A) kadar bir cihazdan veri akışını IoT Hub, ardından işleme için başka bir Azure işlevine Event Grid (ok C) ile kullanıma gösterir. Bölüm D, ok C 'deki aynı Event Grid veri akışını, ' Broadcast ' etiketli bir Azure Işlevine gösterir. ' Broadcast ', ' anlaş ' etiketli başka bir Azure işlevi ile iletişim kurar ve bilgisayar cihazlarıyla birlikte ' Broadcast ' ve ' Negotiate ' iletişim kurar." lightbox="media/how-to-integrate-azure-signalr/signalr-integration-topology.png":::

## <a name="download-the-sample-applications"></a>Örnek uygulamaları indirme

İlk olarak, gerekli örnek uygulamaları indirin. Aşağıdakilerin her ikisine de ihtiyacınız olacak:
* [**Azure dijital TWINS örnekleri**](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/): Bu örnek, verileri bir Azure dijital TWINS örneğine taşımak Için iki Azure işlevi tutan bir *AdtSampleApp* içerir (öğreticide daha ayrıntılı bilgi edinmek için bkz. [*uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md)). Ayrıca, bir IoT cihazına benzetiren, her saniye yeni bir sıcaklık değeri oluşturan bir *Devicesimülatör* örnek uygulaması da içerir. 
    - Örnek bağlantısına gidin ve örneğin _**Azure_Digital_Twins_samples.zip**_ bir kopyasını makinenize ındırmek Için *ZIP yükle* düğmesine basın. Klasörün sıkıştırmasını açın.
* [**SignalR tümleştirmesi Web uygulaması örneği**](https://docs.microsoft.com/samples/azure-samples/digitaltwins-signalr-webapp-sample/digital-twins-samples/): Bu, Azure SignalR hizmetinden Azure dijital TWINS telemetri verilerini tüketen örnek bir tepki veren Web uygulamasıdır.
    -  Örnek bağlantısına gidin ve örneğin _**Azure_Digital_Twins_SignalR_integration_web_app_sample.zip**_ bir kopyasını makinenize ındırmek Için *ZIP yükle* düğmesine basın. Klasörün sıkıştırmasını açın.

[!INCLUDE [Create instance](../azure-signalr/includes/signalr-quickstart-create-instance.md)]

Bir sonraki bölümde kullanacağınız gibi tarayıcı penceresini Azure portal açık bırakın.

## <a name="configure-and-run-the-azure-functions-app"></a>Azure Işlevleri uygulamasını yapılandırma ve çalıştırma

Bu bölümde, iki Azure işlevi ayarlayacaksınız:
* **Negotiate** -bir http tetikleyici işlevi. Geçerli bağlantı bilgilerini oluşturmak ve döndürmek için *Signalrconnectionınfo* giriş bağlamasını kullanır.
* **Broadcast** - [Event Grid](../event-grid/overview.md) tetikleyici işlevi. Olay kılavuzuyla Azure dijital TWINS telemetri verilerini alır ve önceki adımda oluşturduğunuz *SignalR* örneğinin çıkış bağlamasını kullanarak iletiyi tüm bağlı istemci uygulamalarına yayınlar.

İlk olarak, Azure portal açılan tarayıcıya gidin ve ayarladığınız SignalR örneğine ilişkin **bağlantı dizesini** almak için aşağıdaki adımları tamamlayın. Bu işlem, işlevleri yapılandırmak için gereklidir.
1. Daha önce dağıttığınız SignalR hizmeti örneğinin başarıyla oluşturulduğunu onaylayın. Bunu, portalın en üstündeki arama kutusunda adını arayarak yapabilirsiniz. Açmak için örneği seçin.

1. SignalR hizmeti örneği için bağlantı dizelerini görüntülemek için örnek menüsünde **anahtarlar** ' ı seçin.

1. Birincil bağlantı dizesini kopyalamak için simgeyi seçin.

    :::image type="content" source="media/how-to-integrate-azure-signalr/signalr-keys.png" alt-text="SignalR örneği için anahtarlar sayfasını gösteren Azure portal ekran görüntüsü. Birincil bağlantı DIZESININ yanındaki ' panoya kopyala ' simgesi vurgulanır." lightbox="media/how-to-integrate-azure-signalr/signalr-keys.png":::

Ardından, Visual Studio 'Yu (veya seçtiğiniz başka bir kod düzenleyicisini) başlatın ve kod çözümünü *Azure_Digital_Twins_samples > ADTSampleApp* klasöründe açın. Ardından, işlevleri oluşturmak için aşağıdaki adımları uygulayın:

1. *Samplefunctionsapp* projesinde **SignalRFunctions.cs** adlı yeni bir C# Sharp sınıfı oluşturun.

1. Sınıf dosyasının içeriğini aşağıdaki kodla değiştirin:

    ```C#
    using System;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Http;
    using Microsoft.Azure.EventGrid.Models;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Extensions.Http;
    using Microsoft.Azure.WebJobs.Extensions.EventGrid;
    using Microsoft.Azure.WebJobs.Extensions.SignalRService;
    using Microsoft.Extensions.Logging;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    using System.Collections.Generic;
    
    namespace SampleFunctionsApp
    {
        public static class SignalRFunctions
        {
            public static double temperature;
    
            [FunctionName("negotiate")]
            public static SignalRConnectionInfo GetSignalRInfo(
                [HttpTrigger(AuthorizationLevel.Anonymous, "post")] HttpRequest req,
                [SignalRConnectionInfo(HubName = "dttelemetry")] SignalRConnectionInfo connectionInfo)
            {
                return connectionInfo;
            }
    
            [FunctionName("broadcast")]
            public static Task SendMessage(
                [EventGridTrigger] EventGridEvent eventGridEvent,
                [SignalR(HubName = "dttelemetry")] IAsyncCollector<SignalRMessage> signalRMessages,
                ILogger log)
            {
                JObject eventGridData = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
    
                log.LogInformation($"Event grid message: {eventGridData}");
    
                var patch = (JObject)eventGridData["data"]["patch"][0];
                if (patch["path"].ToString().Contains("/Temperature"))
                {
                    temperature = Math.Round(patch["value"].ToObject<double>(), 2);
                }
    
                var message = new Dictionary<object, object>
                {
                    { "temperatureInFahrenheit", temperature},
                };
        
                return signalRMessages.AddAsync(
                    new SignalRMessage
                    {
                        Target = "newMessage",
                        Arguments = new[] { message }
                    });
            }
        }
    }
    ```

1. Visual Studio 'nun *Paket Yöneticisi konsol* penceresinde veya makinenizde *Azure_Digital_Twins_samples \adtsampleapp\samplefunctionsapp* klasöründe herhangi bir komut penceresinde, `SignalRService` NuGet paketini projeye yüklemek için aşağıdaki komutu çalıştırın:
    ```cmd
    dotnet add package Microsoft.Azure.WebJobs.Extensions.SignalRService --version 1.2.0
    ```

    Bu, sınıfındaki tüm bağımlılık sorunlarını çözmelidir.

Sonra, *bir uçtan uca çözüm oluşturma* öğreticisinin [ *uygulamayı Yayımla* bölümünde](tutorial-end-to-end.md#publish-the-app) açıklanan adımları kullanarak işlevinizi Azure 'da yayımlayın. Bunu, uçtan uca öğretici ön eki 'nde kullandığınız App Service/Function uygulamasında yayımlayabilir veya yeni bir tane oluşturabilirsiniz, ancak yinelemeyi en aza indirmek için aynı olanı kullanmak isteyebilirsiniz. Ayrıca, uygulamayı aşağıdaki adımlarla Yayımla:
1. *Negotiate* işlevinin **http uç nokta URL 'sini**toplayın. Bunu yapmak için Azure portal [işlev uygulamaları](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2Fsites/kind/functionapp) sayfasına gidin ve listeden işlev uygulamanızı seçin. Uygulama menüsünde *işlevler* ' i seçin ve *Negotiate* işlevini seçin.

    :::image type="content" source="media/how-to-integrate-azure-signalr/functions-negotiate.png" alt-text="Menüde ' Functions ' vurgulanmış olarak işlev uygulamasının Azure portal görünümü. İşlevlerin listesi sayfada gösterilir ve ' anlaş ' işlevi de vurgulanır.":::

    *İşlev URL 'Sini Al ' a* gidin ve değeri ** _/API_ aracılığıyla kopyalayın (son _/Negotiate_'i eklemeyin?)**. Bunu daha sonra kullanacaksınız.

    :::image type="content" source="media/how-to-integrate-azure-signalr/get-function-url.png" alt-text="' Anlaş ' işlevinin Azure portal görünümü. ' İşlev URL 'sini Al ' düğmesi vurgulanır ve URL 'nin kısmı '/API ' ile başlayarak başlar":::

1. Son olarak, aşağıdaki Azure CLı komutunu kullanarak daha önce Azure SignalR **Bağlantı dizenizi** işlevin uygulama ayarlarına ekleyin. Bu komut, [makinenizde yüklü](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)olan Azure CLI 'niz varsa [Azure Cloud Shell](https://shell.azure.com)veya yerel olarak çalıştırılabilir:
 
    ```azurecli
    az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "AzureSignalRConnectionString=<your-Azure-SignalR-ConnectionString>"
    ```

    Bu komutun çıktısı, Azure işleviniz için ayarlanmış tüm uygulama ayarlarını yazdırır. `AzureSignalRConnectionString`Eklendiğinden emin olmak için listenin en altında bulunan bölümüne bakın.

    :::image type="content" source="media/how-to-integrate-azure-signalr/output-app-setting.png" alt-text="Bir komut penceresinde çıkış alıntısı, ' AzureSignalRConnectionString ' adlı bir liste öğesi gösteriliyor":::

#### <a name="connect-the-function-to-event-grid"></a>İşlevi Event Grid bağlayın

Ardından, *yayın* Azure işlevini öğretici sırasında oluşturduğunuz **olay kılavuzu konusuna** abone olun [*: uçtan uca bir çözüm*](tutorial-end-to-end.md) ön eki bağlama. Bu, Telemetri verilerinin *thermostat67* ikizi 'den Event Grid konusunun içindeki işlevine akabilir ve bu da tüm istemcilere yayınlayacak.

Bunu yapmak için, olay kılavuzunuzda, bir uç nokta olarak *yayın* Azure işlevinizden bir **Event Grid aboneliği** oluşturacaksınız.

[Azure Portal](https://portal.azure.com/), en üstteki arama çubuğunda adını arayarak olay kılavuzunuza gidin. *+ Olay Aboneliği*'ni seçin.

:::image type="content" source="media/how-to-integrate-azure-signalr/event-subscription-1b.png" alt-text="Azure portal: Event Grid olay aboneliği":::

*Olay aboneliği oluştur* sayfasında, alanları aşağıdaki gibi girin (varsayılan olarak doldurulmuş alanlar verilmez):
* *olay ABONELIĞI ayrıntıları*  >  **Ad**: olay aboneliğinize bir ad verin.
* *uç nokta ayrıntıları*  >  **Uç nokta türü**: menü seçeneklerinden *Azure işlevi* ' ni seçin.
* *uç nokta ayrıntıları*  >  **Uç nokta**: *uç nokta seçin* bağlantısına tıklayın. Bu işlem bir *Azure Işlevi Seç* penceresi açar:
    - **Aboneliğiniz**, **kaynak grubunuz**, **işlev uygulaması** ve **işlevinizi** (*yayın*) girin. Bunlardan bazıları abonelik seçildikten sonra otomatik olarak doldurulabilir.
    - **Seçimi Onayla**' ya basın.

:::image type="content" source="media/how-to-integrate-azure-signalr/create-event-subscription.png" alt-text="Olay aboneliği oluşturma Azure portal görünümü. Yukarıdaki alanlar doldurulmuştur ve ' seçimi onayla ' ve ' oluştur ' düğmeleri vurgulanır.":::

*Olay aboneliği oluştur* sayfasına dönün ve **Oluştur**' a basın.

## <a name="configure-and-run-the-web-app"></a>Web uygulamasını yapılandırma ve çalıştırma

Bu bölümde, sonucu eylem olarak görürsünüz. İlk olarak, Azure dijital TWINS örneğiniz aracılığıyla telemetri verileri gönderen **sanal cihaz örnek uygulamasını** başlacaksınız. Daha sonra, ayarlamış olduğunuz Azure SignalR akışına bağlanacak **örnek istemci Web uygulamasını** yapılandıracaksınız. Bundan sonra, örnek Web uygulamasını gerçek zamanlı olarak güncelleştiren verileri görebilmelisiniz.

### <a name="run-the-device-simulator"></a>Cihaz simülatörünü Çalıştır

Uçtan uca öğretici ön eki sırasında, [cihaz simülatörünü](tutorial-end-to-end.md#configure-and-run-the-simulation) bir IoT Hub ve Azure dijital TWINS örneğiniz aracılığıyla veri gönderecek şekilde yapılandırdınız.

Şimdi yapmanız gerekir, *Azure_Digital_Twins_samples > devicesimülatör > devicesimülatör. sln*' de bulunan simülatör projesini başlatmıştır. Visual Studio kullanıyorsanız, projeyi açabilir ve araç çubuğunda Bu düğmeyle çalıştırabilirsiniz:

:::image type="content" source="media/how-to-integrate-azure-signalr/start-button-simulator.png" alt-text="Visual Studio Başlangıç düğmesi (Devicesimülatör Projesi)":::

Bir konsol penceresi açılır ve sanal sıcaklık telemetri iletilerini görüntüler. Bunlar Azure Digital Twins örneğiniz aracılığıyla gönderilmekte ve burada Azure işlevleri ve SignalR tarafından alınırlar.

Bu konsolda başka bir şey yapmanız gerekmez, ancak sonraki adımları tamamlayarak çalışır durumda bırakın.

### <a name="configure-the-sample-client-web-app"></a>Örnek istemci Web uygulamasını yapılandırma

Daha sonra, **SignalR Integration Web uygulaması örneğini** şu adımlarla ayarlayın:
1. Visual Studio 'Yu veya tercih ettiğiniz herhangi bir kod düzenleyicisini kullanarak, [*Önkoşullar*](#prerequisites) bölümünde indirdiğiniz sıkıştırılmış _**Azure_Digital_Twins_SignalR_integration_web_app_sample**_ klasörünü açın.

1. *Src/App.js* dosyasını açın ve içindeki URL 'yi, `HubConnectionBuilder` daha önce KAYDETTIĞINIZ **Negotiate** işlevinin HTTP uç noktası URL 'si ile değiştirin:

    ```javascript
        const hubConnection = new HubConnectionBuilder()
            .withUrl('<URL>')
            .build();
    ```
1. Visual Studio 'nun *Geliştirici komut isteminde* veya makinenizde herhangi bir komut penceresinde, *Azure_Digital_Twins_SignalR_integration_web_app_sample \src* klasörüne gidin. Bağımlı düğüm paketlerini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    npm install
    ```

Sonra, Azure portal işlev uygulamanızda izinleri ayarlayın:
1. Azure portal [işlev uygulamaları](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2Fsites/kind/functionapp) sayfasında, işlev uygulaması örneğinizi seçin.
1. Örnek menüsünde aşağı kaydırın ve *CORS*' yi seçin. CORS sayfasında, `http://localhost:3000` boş kutuya girerek izin verilen bir kaynak olarak ekleyin. *Erişim-denetim-izin-kimlik bilgilerini etkinleştir* ve *Kaydet*için kutuyu işaretleyin.

    :::image type="content" source="media/how-to-integrate-azure-signalr/cors-setting-azure-function.png" alt-text="Azure Işlevinde CORS ayarı":::

### <a name="see-the-results"></a>Sonuçları görme

Sonuçları eylem ' e görmek için, **SignalR tümleştirmesi Web uygulaması örneğini**başlatın. Bunu, şu komutu çalıştırarak *Azure_Digital_Twins_SignalR_integration_web_app_sample \src* konumundaki herhangi bir konsol penceresinden yapabilirsiniz:

```cmd
npm start
```

Bu işlem, bir görsel sıcaklık ölçer görüntüleyen örnek uygulamayı çalıştıran bir tarayıcı penceresi açar. Uygulama çalışmaya başladıktan sonra, Web uygulamasına gerçek zamanlı olarak yansıtılmakta olan Azure dijital TWINS aracılığıyla yayılan cihaz simülatörü 'nden sıcaklık telemetri değerlerini görmeye başlamanız gerekir.

:::image type="content" source="media/how-to-integrate-azure-signalr/signalr-webapp-output.png" alt-text="Görsel sıcaklık ölçerin gösterildiği örnek istemci Web uygulamasından alıntı. Yansıtılan sıcaklık 67,52 ' dir":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynaklara artık ihtiyacınız yoksa, bunları silmek için aşağıdaki adımları izleyin. 

Azure Cloud Shell veya yerel Azure CLı kullanarak, [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutuyla bir kaynak grubundaki tüm Azure kaynaklarını silebilirsiniz. Kaynak grubunun kaldırılması da kaldırılır...
* Azure dijital TWINS örneği (uçtan uca öğreticiden)
* IoT Hub ve Hub cihaz kaydı (uçtan uca öğreticiden)
* olay Kılavuzu konusu ve ilişkili abonelikler
* Azure Işlevleri uygulaması, her üç işlev ve depolama gibi ilişkili kaynaklar dahil
* Azure SignalR örneği

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. 

```azurecli
az group delete --name <your-resource-group>
```

Azure dijital TWINS örneğinizi siliyorsanız, bu komutu kullanarak, sizin için oluşturduğunuz Azure AD uygulama kaydını uçtan uca öğreticide da silebilirsiniz:

```azurecli
az ad app delete --id <your-application-ID>
```

Son olarak, indirdiğiniz proje örnek klasörlerini yerel makinenize (*Azure_Digital_Twins_samples.zip* ve *Azure_Digital_Twins_SignalR_integration_web_app_sample.zip*) silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Digital TWINS telemetri olaylarını örnek bir istemci uygulamasına yayımlamak için Azure işlevleri 'ni SignalR ile ayarlarsınız.

Daha sonra, Azure SignalR hizmeti hakkında daha fazla bilgi edinin:
* [*Azure SignalR hizmeti nedir?*](../azure-signalr/signalr-overview.md)

Veya Azure Işlevleri ile Azure SignalR hizmeti kimlik doğrulaması hakkında daha fazla bilgi edinin:
* [*Azure SignalR hizmeti kimlik doğrulaması*](../azure-signalr/signalr-tutorial-authenticate-azure-functions.md)
