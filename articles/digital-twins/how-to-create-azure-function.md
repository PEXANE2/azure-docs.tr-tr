---
title: Veri işleme için bir Azure işlevi ayarlama
titleSuffix: Azure Digital Twins
description: Bkz. dijital TWINS tarafından erişebilen ve tetiklenebilecek bir Azure işlevi oluşturma.
author: cschormann
ms.author: cschorm
ms.date: 3/17/2020
ms.topic: how-to
ms.service: digital-twins
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: 51e2560a7ad8d62965474082633d4cb66c1d6592
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84725911"
---
# <a name="connect-azure-functions-apps-for-processing-data"></a>Verileri işlemek için Azure Işlevleri uygulamalarına bağlanma

[!INCLUDE [Azure Digital Twins current preview status](../../includes/digital-twins-preview-status.md)]

Önizleme süresince verileri temel alarak dijital TWINS güncelleştirme, [Azure işlevleri](../azure-functions/functions-overview.md)gibi işlem kaynakları aracılığıyla [**olay rotaları**](concepts-route-events.md) kullanılarak işlenir. Bir Azure işlevi, yanıt olarak bir dijital ikizi güncelleştirmek için kullanılabilir:
* IoT Hub gelen cihaz telemetri verileri
* ikizi grafiğinin içindeki başka bir dijital ikizi gelen özellik değişikliği veya diğer veriler

Bu makalede, Azure dijital TWINS ile kullanmak üzere bir Azure işlevi oluşturma işlemi adım adım açıklanmaktadır. 

İçerdiği adımlara genel bakış aşağıda verilmiştir:

1. Visual Studio 'da Azure Işlevleri uygulaması oluşturma
2. [Event Grid](../event-grid/overview.md) tetikleyicisiyle bir Azure işlevi yazma
3. İşleve kimlik doğrulama kodu ekleme (Azure dijital TWINS 'ye erişebilmek için)
4. İşlev uygulamasını Azure 'da yayımlayın
5. [Güvenlik](concepts-security.md) erişimini ayarlayın. Azure işlevinin hizmete erişmek için çalışma zamanında bir erişim belirteci alabilmesi için, işlev uygulaması için Yönetilen Hizmet Kimliği yapılandırmanız gerekir.

Bu makalenin geri kalanında, tek seferde Azure işlevi kurulum adımları gösterilmektedir.

## <a name="create-an-azure-functions-app-in-visual-studio"></a>Visual Studio 'da Azure Işlevleri uygulaması oluşturma

Visual Studio 2019 ' de *dosya > yeni proje*' yi seçin. *Azure işlevleri* şablonunu arayın, seçin ve "ileri" düğmesine basın.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-new-project.png" alt-text="Visual Studio: yeni proje iletişim kutusu":::

İşlev uygulaması için bir ad belirtin ve "Oluştur" düğmesine basın.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-project-config.png" alt-text="Visual Studio: proje yapılandırma iletişim kutusu":::

*Event Grid tetikleyiciyi* seçin ve "Oluştur" düğmesine basın.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-project-trigger.png" alt-text="Visual Studio: Azure işlevi proje tetikleyicisi iletişim kutusu":::

## <a name="write-an-azure-function-with-an-event-grid-trigger"></a>Event Grid tetikleyicisiyle bir Azure işlevi yazma

Aşağıdaki kod, olayları günlüğe kaydetmek için kullanabileceğiniz kısa bir Azure işlevi oluşturur: 

```csharp
// Default URL for triggering Event Grid function in the local environment
// http://localhost:7071/runtime/webhooks/EventGrid?functionName={functionname}
using System;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Extensions.Logging;

namespace FunctionSample
{
    public static class FooFunction    {
        [FunctionName("Foo")]
        public static void Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

Bu, temel Azure işleviniz.

### <a name="run-and-debug-the-azure-function-app"></a>Azure işlev uygulamasını çalıştırma ve hata ayıklama

Artık işlevi derleyebilir ve çalıştırabilirsiniz. Azure işlevlerinin sonunda bulutta çalıştırılması amaçlanıyor olsa da, Azure işlevlerini yerel olarak çalıştırabilir ve hata ayıklayabilirsiniz.

Bunun hakkında daha fazla bilgi için bkz. [Event Grid tetikleyiciden yerel olarak hata ayıklama](../azure-functions/functions-debug-event-grid-trigger-local.md).

### <a name="add-the-azure-digital-twins-sdk-to-your-azure-function-app"></a>Azure Digital TWINS SDK 'sını Azure işlev uygulamanıza ekleyin

İşlev uygulaması, [.net Için Azure IoT Digital ikizi istemci kitaplığı (C#)](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)kullanarak Azure Digital TWINS ile etkileşime girer. SDK 'yı kullanmak için projenize aşağıdaki paketleri eklemeniz gerekir:
* `Azure.DigitalTwins.Core`(sürüm `1.0.0-preview.2` )
* `Azure.Identity`

Azure Işlevleri için doğru şekilde ayarlanmak üzere Azure SDK işlem hattının yapılandırılması için şunlar gerekir:
* `Azure.Net.Http`
* `Azure.Core`

Tercih ettiğiniz araçlara bağlı olarak, Visual Studio Paket Yöneticisi veya `dotnet` komut satırı aracı ile bunu yapabilirsiniz. 

Aşağıdaki using deyimlerini Azure işlevinizde ekleyin.

```csharp
using Azure.Identity;
using Azure.DigitalTwins.Core;
using System.Net.Http;
using Azure.Core.Pipeline;
```

## <a name="add-authentication-code-to-the-azure-function"></a>Azure işlevine kimlik doğrulama kodu ekleme

Sonra, işlevin Azure dijital TWINS 'e erişmesine izin verecek kimlik doğrulama kodu ekleyin.

Bu değerler için işlev sınıfınıza değişkenler ekleyin: 
* Azure dijital TWINS uygulama KIMLIĞI
* Azure dijital TWINS örneğinizin URL 'SI. Hizmet URL 'sini, işlevinde sabit kodlamak yerine, bir ortam değişkeninden okumak iyi bir uygulamadır.
* HttpClient örneğini tutacak statik bir değişken. HttpClient, oluşturmak için nispeten pahalıdır ve her işlev çağrısı için bunu yapmak zorunda kalmamak istiyoruz.

Ayrıca, Azure dijital TWINS istemci örneğinizi işlev projesine tutacak şekilde işlevinizin içine bir yerel değişken ekleyin. Bu değeri, sınıfınızın içinde statik bir *değişken yapmayın.*

Son olarak, işlev imzasını zaman uyumsuz olacak şekilde değiştirin.

Bu değişikliklerden sonra, işlev kodunuz aşağıdakine benzer olmalıdır:

```csharp
namespace FunctionSample
{
    public static class FooFunction
    {
        const string adtAppId = "https://digitaltwins.azure.net";
        private static string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
        private static HttpClient httpClient = new HttpClient();

        [FunctionName("Foo")]
        public static async Task Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            DigitalTwinsClient client = null;
            try
            {
                ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
                DigitalTwinsClientOptions opts = 
                    new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
                client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, opts);
                                                
                log.LogInformation($"ADT service client connection created.");
            }
            catch (Exception e)
            {
                log.LogError($"ADT service client connection failed. " + e.ToString());
                return;
            }
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

Işlevler uygulamanızın Azure dijital TWINS 'e erişebilmesi için sistem tarafından yönetilen bir kimliğe sahip olması ve Azure dijital TWINS örneğinize erişmek için gereken izinlere sahip olması gerekir.

Sistem tarafından yönetilen kimliği oluşturmak için aşağıdaki komutu kullanın. Çıktıda *PrincipalId* alanını bir yere göz atın.

```azurecli
az functionapp identity assign -g <your-resource-group> -n <your-App-Service-(function-app)-name>
```

İşlev uygulamasının kimliğini Azure dijital TWINS örneğinizin *sahip* rolüne atamak için aşağıdaki komutta *PrincipalId* değerini kullanın:

```azurecli
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Owner (Preview)"
```

Yönetilen kimlik hakkında daha fazla bilgi için lütfen bkz. [App Service ve Azure işlevleri için Yönetilen kimlikler kullanma](../app-service/overview-managed-identity.md).

Son olarak, bir ortam değişkeni ayarlayarak Azure dijital TWINS örneğinizin URL 'sini işleviniz için erişilebilir hale getirebilirsiniz. Bunun hakkında daha fazla bilgi için bkz. [ortam değişkenleri](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables).

> [!TIP]
> Azure Digital TWINS örneğinin URL 'SI, Azure Digital TWINS örneğinizin *ana bilgisayar adının*başlangıcına *https://* eklenerek yapılır. Ana bilgisayar adını görmek için, örneğinizin tüm özellikleriyle birlikte çalıştırabilirsiniz `az dt show --dt-name <your-Azure-Digital-Twins-instance>` .

```azurecli
az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-URL>"
```

## <a name="publish-the-azure-function-app"></a>Azure işlevi uygulamasını yayımlama

İşlev uygulamasını Azure 'da yayımlamak için Çözüm Gezgini işlev projesini (çözümü değil) sağ seçin ve *Yayımla ()* öğesini seçin.

Aşağıdaki sekme görünür:

:::image type="content" source="media/how-to-create-azure-function/visual-studio-publish-1.png" alt-text="Visual Studio: Publish işlevi iletişim kutusu, sayfa 1":::

Azure Işlevleri ile kullanmak üzere bir App Service planı seçin veya oluşturun. Emin değilseniz, varsayılan tüketim planını kullanarak başlayın.

> [!IMPORTANT] 
> Azure işlevi yayımlandığında, aboneliğiniz üzerinde Azure dijital TWINS 'ten bağımsız olarak ek ücretler uygulanır.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-publish-2.png" alt-text="Visual Studio: Publish işlevi iletişim kutusu, sayfa 2":::

Aşağıdaki sayfada, yeni işlev uygulaması, bir kaynak grubu ve diğer ayrıntılar için istenen adı girin.

## <a name="set-up-security-access-for-the-azure-function-app"></a>Azure işlevi uygulaması için güvenlik erişimini ayarlama

Önceki örneklerden Azure işlevi iskelet 'i Azure dijital TWINS ile kimlik doğrulaması yapabilmek için bir taşıyıcı belirtecinin geçirilmesi gerekir. Bu taşıyıcı belirtecinin geçirildiğinden emin olmak için, işlev uygulaması için [yönetilen hizmet kimliği (MSI)](../active-directory/managed-identities-azure-resources/overview.md) ayarlamanız gerekir. Bu, her bir işlev uygulaması için yalnızca bir kez yapılmalıdır.

Bunu ayarlamak için [Azure Portal](https://portal.azure.com/) gidin ve işlev uygulamanıza gidin.

*Platform özellikleri* sekmesinde *kimlik*' i seçin:

:::image type="content" source="media/how-to-create-azure-function/visual-studio-msi-1.png" alt-text="Azure portal: bir Azure işlevi için kimlik seçme":::

Kimlik sayfasında, *durumu* *Açık*olarak ayarlayın. 

:::image type="content" source="media/how-to-create-azure-function/visual-studio-msi-2.png" alt-text="Azure portal: kimlik durumunu açma":::

Ayrıca, sonraki bölümde kullanılacak şekilde bu sayfada gösterilen **nesne kimliğini** de aklınızda bulabilirsiniz.

### <a name="assign-access-roles"></a>Erişim rolleri atama

Azure dijital TWINS, erişimi yönetmek için rol tabanlı erişim denetimi kullandığından (Bu konuda daha fazla bilgi için bkz [. kavramlar: Azure dijital TWINS çözümleri Için güvenlik](concepts-security.md) ), Azure dijital TWINS erişimine izin vermek istediğiniz her bir işlev uygulaması için de bir rol eklemeniz gerekir.

Rol atamak için, oluşturduğunuz Azure dijital TWINS örneğinin **kaynak kimliği** gereklidir. Örneğinizi oluşturduğunuz sırada daha önce kaydetmediyseniz, bu komutu kullanarak alabilirsiniz:

```bash
az dt show --name <your-instance-name> -g <your-resource-group-name>
```
Kaynak KIMLIĞI, "/Subscriptions/..." harfleriyle başlayan "ID" adlı uzun bir dize olarak çıktının bir parçası olacaktır.

Aşağıdaki komutta, Azure işlevinin nesne KIMLIĞIYLE birlikte kaynak KIMLIĞINI kullanın:

```azurecli
az role assignment create --role "Azure Digital Twins Owner (Preview)" --assignee <object-ID> --scope <resource-ID>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Digital TWINS ile kullanmak üzere bir Azure işlevi ayarlama adımları izlenir. Sonra, bir uç nokta dinlemek için Azure işlevinizi Event Grid 'e abone olabilirsiniz. Bu uç nokta şu olabilir:
* Azure dijital TWINS 'den gelen iletileri işlemek için Azure dijital TWINS 'e bağlı Event Grid uç noktası (özellik değişiklik iletileri, ikizi grafiğinde [dijital TWINS](concepts-twins-graph.md) tarafından oluşturulan telemetri iletileri veya yaşam döngüsü iletileri gibi)
* Telemetri ve diğer cihaz olaylarını göndermek için IoT Hub tarafından kullanılan IoT sistem konuları
* Diğer hizmetlerden ileti alma Event Grid uç noktası

Daha sonra, Azure dijital TWINS 'e IoT Hub verileri almak için temel Azure işlevinizde nasıl derleme yapılacağını inceleyin:
* [Nasıl yapılır: IoT Hub 'dan alma telemetrisi](how-to-ingest-iot-hub-data.md)