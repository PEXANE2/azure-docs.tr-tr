---
title: Veri işleme için bir Azure işlevi ayarlama
titleSuffix: Azure Digital Twins
description: Bkz. dijital TWINS tarafından erişebilen ve tetiklenebilecek bir Azure işlevi oluşturma.
author: baanders
ms.author: baanders
ms.date: 8/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: a0f5a921ad9eba82f27b3a8945643737b6d76030
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89614074"
---
# <a name="connect-azure-functions-apps-for-processing-data"></a>Verileri işlemek için Azure Işlevleri uygulamalarına bağlanma

Verileri temel alarak dijital TWINS 'nin güncelleştirilmesi, [Azure işlevleri](../azure-functions/functions-overview.md)gibi işlem kaynakları aracılığıyla [**olay rotaları**](concepts-route-events.md) kullanılarak işlenir. Bir Azure işlevi, yanıt olarak bir dijital ikizi güncelleştirmek için kullanılabilir:
* IoT Hub gelen cihaz telemetri verileri
* ikizi grafiğinin içindeki başka bir dijital ikizi gelen özellik değişikliği veya diğer veriler

Bu makalede, Azure dijital TWINS ile kullanmak üzere bir Azure işlevi oluşturma işlemi adım adım açıklanmaktadır. 

İçerdiği adımlara genel bir bakış aşağıda verilmiştir:

1. Visual Studio 'da Azure Işlevleri uygulaması oluşturma
2. [Event Grid](../event-grid/overview.md) tetikleyicisiyle bir Azure işlevi yazma
3. İşleve kimlik doğrulama kodu ekleme (Azure dijital TWINS 'ye erişebilmek için)
4. İşlev uygulamasını Azure 'da yayımlayın
5. Azure işlevi uygulaması için [güvenlik](concepts-security.md) erişimini ayarlama

## <a name="create-an-azure-functions-app-in-visual-studio"></a>Visual Studio 'da Azure Işlevleri uygulaması oluşturma

Visual Studio 2019 ' de _dosya > yeni > proje_ ' yi seçin ve _Azure işlevleri_ şablonunu arayın, _İleri_' yi seçin.

:::image type="content" source="media/how-to-create-azure-function/create-azure-function-project.png" alt-text="Visual Studio: yeni proje iletişim kutusu":::

İşlev uygulaması için bir ad belirtin ve _Oluştur_' u seçin.

:::image type="content" source="media/how-to-create-azure-function/configure-new-project.png" alt-text="Visual Studio: yeni proje yapılandırma":::

*Event Grid tetikleyicisi* işlev türünü seçin ve _Oluştur_' u seçin.

:::image type="content" source="media/how-to-create-azure-function/eventgridtrigger-function.png" alt-text="Visual Studio: Azure işlevi proje tetikleyicisi iletişim kutusu":::

İşlev uygulamanız oluşturulduktan sonra Visual Studio, proje klasörünüzdeki **function.cs** dosyasında otomatik olarak doldurulan kod örneğine sahip olur. Bu kısa Azure işlevi olayları günlüğe kaydetmek için kullanılır.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-sample-code.png" alt-text="Visual Studio: örnek kodlu Proje penceresi":::

## <a name="write-an-azure-function-with-an-event-grid-trigger"></a>Event Grid tetikleyicisiyle bir Azure işlevi yazma

İşlev uygulamanıza SDK ekleyerek bir Azure işlevi yazabilirsiniz. İşlev uygulaması, [.net Için Azure IoT Digital ikizi istemci kitaplığı (C#)](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)kullanarak Azure Digital TWINS ile etkileşime girer. 

SDK 'yı kullanmak için aşağıdaki paketleri projenize eklemeniz gerekir. Paketleri Visual Studio NuGet Paket Yöneticisi 'ni kullanarak yükleyebilir ya da komut satırı aracını kullanarak paketleri ekleyebilirsiniz `dotnet` . Aşağıdaki yöntemlerden birini seçin: 

**1. seçenek. Visual Studio Package Manager 'ı kullanarak paket ekleme:**
    
Bunu projeniz üzerinde sağ seçerek ve listeden _NuGet Paketlerini Yönet_ ' i seçerek yapabilirsiniz. Ardından, açılan pencerede, _gezinme_ sekmesi ' ni seçin ve aşağıdaki paketleri arayın. Paketi yüklemek için, _yüklemeyi_ seçin ve lisans sözleşmesini _kabul edin_ .

* `Azure.DigitalTwins.Core`
* `Azure.Identity` 

Azure Işlevleri için doğru şekilde ayarlanmak üzere Azure SDK işlem hattının yapılandırılması için aşağıdaki paketlere de ihtiyacınız olacaktır. Tüm paketleri yüklemek için yukarıdaki işlemi yineleyin.

* `System.Net.Http`
* `Azure.Core.Pipeline`

**Seçenek 2. `dotnet` Komut satırı aracını kullanarak paket ekleyin:**

```cmd/sh
dotnet add package Azure.DigitalTwins.Core --version 1.0.0-preview.3
dotnet add package Azure.identity --version 1.2.2
dotnet add package System.Net.Http
dotnet add package Azure.Core.Pipeline
```
Ardından, Visual Studio Çözüm Gezgini, örnek kodunuzun bulunduğu _function.cs_ dosyasını açın ve aşağıdaki _using_ deyimlerini Azure işlevinizde ekleyin. 

```csharp
using Azure.DigitalTwins.Core;
using Azure.Identity;
using System.Net.Http;
using Azure.Core.Pipeline;
```
## <a name="add-authentication-code-to-the-azure-function"></a>Azure işlevine kimlik doğrulama kodu ekleme

Artık sınıf düzeyi değişkenleri bildirecektir ve işlevin Azure dijital TWINS 'e erişmesine izin verecek kimlik doğrulama kodu eklersiniz. {Function Name}. cs dosyanızdaki Azure işleviniz için aşağıdakileri ekleyeceksiniz.

* ADT hizmeti URL 'sini bir ortam değişkeni olarak okuyun. Hizmet URL 'sini, işlevinde sabit kodlamak yerine, bir ortam değişkeninden okumak iyi bir uygulamadır.
```csharp     
private static readonly string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
```
* HttpClient örneğini tutacak statik bir değişken. HttpClient, oluşturmak için nispeten pahalıdır ve her işlev çağrısı için bunu yapmak zorunda kalmamak istiyoruz.
```csharp
private static readonly HttpClient httpClient = new HttpClient();
```
* Azure işlevinde yönetilen kimlik kimlik bilgilerini kullanabilirsiniz.
```csharp
ManagedIdentityCredential cred = new ManagedIdentityCredential("https://digitaltwins.azure.net");
```
* Azure dijital TWINS istemci örneğinizi işlev projesine tutmak için işlevinizin içine _DigitalTwinsClient_ yerel bir değişken ekleyin. Bu değişkeni sınıfınız içinde *statik yapmayın.*
```csharp
DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
```
* Özel durumları yakalamak için _adtInstanceUrl_ için null denetimi ekleyin ve işlev mantığınızı bir try catch bloğunda sarın.

Bu değişikliklerden sonra, işlev kodunuz aşağıdakine benzer olacaktır:

```csharp
// Default URL for triggering event grid function in the local environment.
// http://localhost:7071/runtime/webhooks/EventGrid?functionName={functionname}
using System;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Extensions.Logging;
using Azure.DigitalTwins.Core;
using Azure.Identity;
using System.Net.Http;
using Azure.Core.Pipeline;

namespace adtIngestFunctionSample
{
    public class Function1
    {
        //Your Digital Twin URL is stored in an application setting in Azure Functions
        private static readonly string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
        private static readonly HttpClient httpClient = new HttpClient();

        [FunctionName("TwinsFunction")]
        public void Run([EventGridTrigger] EventGridEvent eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.Data.ToString());
            if (adtInstanceUrl == null) log.LogError("Application setting \"ADT_SERVICE_URL\" not set");
            try
            {
                //Authenticate with Digital Twins
                ManagedIdentityCredential cred = new ManagedIdentityCredential("https://digitaltwins.azure.net");
                DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
                log.LogInformation($"ADT service client connection created.");
                /*
                * Add your business logic here
                */
            }
            catch (Exception e)
            {
                log.LogError(e.Message);
            }

        }
    }
}
```

## <a name="publish-the-function-app-to-azure"></a>İşlev uygulamasını Azure 'da yayımlayın

İşlev uygulamasını Azure 'da yayımlamak için Çözüm Gezgini işlev projesini (çözümü değil) sağ seçin ve **Yayımla**' yı seçin.

> [!IMPORTANT] 
> Azure işlevi yayımlandığında, aboneliğiniz üzerinde Azure dijital TWINS 'ten bağımsız olarak ek ücretler uygulanır.

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function.png" alt-text="Visual Studio: yayımlama Azure işlevi ":::

Yayımlama hedefi olarak **Azure** ' u seçin ve **İleri ' yi**seçin.

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-1.png" alt-text="Visual Studio: Azure işlev yayımlama iletişim kutusu, Azure 'u seçin ":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-2.png" alt-text="Visual Studio: Publish işlevi iletişim kutusunda makinenize göre Azure İşlev Uygulaması (Windows) veya (Linux) seçeneğini belirleyin":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-3.png" alt-text="Visual Studio: Publish işlevi iletişim kutusu, yeni bir Azure Işlevi oluşturma":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-4.png" alt-text="Visual Studio: Publish işlevi iletişim kutusu, alanları doldurup oluştur ' u seçin.":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-5.png" alt-text="Visual Studio: Publish işlevi iletişim kutusu, listeden işlev uygulamanızı seçin ve son":::

Aşağıdaki sayfada, yeni işlev uygulaması, bir kaynak grubu ve diğer ayrıntılar için istenen adı girin.
Işlevler uygulamanızın Azure dijital TWINS 'e erişebilmesi için sistem tarafından yönetilen bir kimliğe sahip olması ve Azure dijital TWINS örneğinize erişmek için gereken izinlere sahip olması gerekir.

Daha sonra, CLı veya Azure portal kullanarak işlev için güvenlik erişimi ayarlayabilirsiniz. Aşağıdaki yöntemlerden birini seçin:

## <a name="set-up-security-access-for-the-azure-function-app"></a>Azure işlevi uygulaması için güvenlik erişimini ayarlama
Aşağıdaki seçeneklerden birini kullanarak Azure işlev uygulaması için güvenlik erişimi ayarlayabilirsiniz:

### <a name="option-1-set-up-security-access-for-the-azure-function-app-using-cli"></a>Seçenek 1: CLı kullanarak Azure işlev uygulaması için güvenlik erişimini ayarlama

Önceki örneklerden Azure işlevi iskelet, Azure dijital TWINS ile kimlik doğrulaması yapabilmek için bir taşıyıcı belirtecinin kendisine geçirilmesini gerektirir. Bu taşıyıcı belirtecinin geçirildiğinden emin olmak için, işlev uygulaması için [yönetilen hizmet kimliği (MSI)](../active-directory/managed-identities-azure-resources/overview.md) ayarlamanız gerekir. Bu, her bir işlev uygulaması için yalnızca bir kez yapılmalıdır.

Azure dijital TWINS örneğiniz için sistem tarafından yönetilen kimlik oluşturabilir ve işlev uygulamasının kimliğini _Azure Digital TWINS Owner (Önizleme)_ rolüne atayabilirsiniz. Bu, veri düzlemi etkinliklerini gerçekleştirmek için örnekte işlev uygulamasına izin verir. Daha sonra, bir ortam değişkeni ayarlayarak Azure dijital TWINS örneğinin URL 'sini işlevinizle erişilebilir yapın.

 Komutları çalıştırmak için [Azure Cloud Shell](https://shell.azure.com) kullanın.

Sistem tarafından yönetilen kimliği oluşturmak için aşağıdaki komutu kullanın. Çıktıda _PrincipalId_ alanını bir yere göz atın.

```azurecli 
az functionapp identity assign -g <your-resource-group> -n <your-App-Service-(function-app)-name>   
```
İşlev uygulamasının kimliğini Azure dijital TWINS örneğiniz için _Azure Digital TWINS Owner (Önizleme)_ rolüne atamak için aşağıdaki komutta _PrincipalId_ değerini kullanın.

```azurecli 
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Owner (Preview)"
```
Son olarak, bir ortam değişkeni ayarlayarak Azure dijital TWINS örneğinizin URL 'sini işleviniz için erişilebilir hale getirebilirsiniz. Ortam değişkenlerini ayarlama hakkında daha fazla bilgi için bkz. [*ortam değişkenleri*](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables). 

> [!TIP]
> Azure Digital TWINS örneğinin URL 'SI, Azure Digital TWINS örneğinizin *ana bilgisayar adının*başlangıcına *https://* eklenerek yapılır. Ana bilgisayar adını görmek için, örneğinizin tüm özellikleriyle birlikte çalıştırabilirsiniz `az dt show --dt-name <your-Azure-Digital-Twins-instance>` .

```azurecli 
az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-hostname>"
```
### <a name="option-2-set-up-security-access-for-the-azure-function-app-using-azure-portal"></a>Seçenek 2: Azure portal kullanarak Azure işlev uygulaması için güvenlik erişimini ayarlama

Bir sistem tarafından atanmış yönetilen kimlik, Azure kaynaklarının kimlik bilgilerini kodda depolamadan (örneğin, Azure Key Vault) kimlik doğrulaması yapmasına olanak sağlar. Etkinleştirildikten sonra, tüm gerekli izinler Azure rol tabanlı erişim denetimi aracılığıyla verilebilir. Bu tür yönetilen kimliğin yaşam döngüsü, bu kaynağın yaşam döngüsüne bağlıdır. Ayrıca, her kaynak (örneğin, sanal makine) yalnızca bir sistem tarafından atanmış yönetilen kimliğe sahip olabilir.

[Azure Portal](https://portal.azure.com/), arama çubuğunda _işlev uygulaması_ ' nı daha önce oluşturduğunuz işlev uygulaması adı ile arayın. Listeden *işlev uygulaması* seçin. 

:::image type="content" source="media/how-to-create-azure-function/portal-search-for-functionapp.png" alt-text="Azure portal: işlev uygulamasını ara":::

İşlev uygulaması penceresinde, yönetilen kimliği etkinleştirmek için sol taraftaki Gezinti çubuğundan _kimlik_ ' i seçin.
_Sistem atandı_ sekmesinde _durumu_ açık olarak değiştirin ve _kaydedin_ . _Sistem tarafından atanan yönetilen kimliği etkinleştirmek_için bir açılır pencere görürsünüz.
_Evet_ düğmesini seçin. 

:::image type="content" source="media/how-to-create-azure-function/enable-system-managed-identity.png" alt-text="Azure portal: sistem tarafından yönetilen kimliği etkinleştir":::

İşlevinizin Azure Active Directory başarıyla kaydedildiğini bildirimleri doğrulayabilirsiniz.

:::image type="content" source="media/how-to-create-azure-function/notifications-enable-managed-identity.png" alt-text="Azure portal: bildirimler":::

Ayrıca, _kimlik_ sayfasında GÖSTERILEN **nesne kimliğini** bir sonraki bölümde kullanılacak şekilde aklınızda bulabilirsiniz.

:::image type="content" source="media/how-to-create-azure-function/object-id.png" alt-text="Gelecekte kullanmak üzere nesne KIMLIĞINI kopyalama":::

### <a name="assign-access-roles-using-azure-portal"></a>Azure portal kullanarak erişim rolleri atama

Azure _rol atamaları ' nı seçin,_ Azure rol atamaları sayfasına kadar açılır. Ardından _+ rol ataması Ekle (Önizleme)_ seçeneğini belirleyin.

:::image type="content" source="media/how-to-create-azure-function/add-role-assignments.png" alt-text="Azure portal: rol ataması Ekle":::

Açılan _rol ataması Ekle (Önizleme)_ penceresinde, şunu seçin

* _Kapsam_: Kaynak grubu
* _Abonelik_: Azure aboneliğinizi seçin
* _Kaynak grubu_: açılan listeden kaynak grubunuzu seçin
* _Rol_: açılan listeden _Azure dijital TWINS sahibini (Önizleme)_ seçin

_Kaydet_ düğmesini seçerek ayrıntılarınızı kaydedin.

:::image type="content" source="media/how-to-create-azure-function/add-role-assignment.png" alt-text="Azure portal: rol ataması Ekle (Önizleme) ":::

### <a name="configure-application-settings-using-azure-portal"></a>Azure portal kullanarak uygulama ayarlarını yapılandırma

Bir ortam değişkeni ayarlayarak, Azure dijital TWINS örneğinizin URL 'sini işleviniz için erişilebilir hale getirebilirsiniz. Bunun hakkında daha fazla bilgi için bkz. [*ortam değişkenleri*](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables). Uygulama ayarları, dijital TWINS örneğine erişmek için ortam değişkenleri olarak sunulur. 

Uygulama ayarı oluşturmak için ADT_INSTANCE_URL gerekir.

Örnek ana bilgisayar adına **_https://_** ekleyerek ADT_INSTANCE_URL alabilirsiniz. Azure portal, arama çubuğunda örneğinizi arayarak dijital TWINS örnek ana bilgisayar adınızı bulabilirsiniz. Ardından, _ana bilgisayar adını_görüntülemek için sol gezinti çubuğundaki _genel bakış_ ' ı seçin. Bir uygulama ayarı oluşturmak için bu değeri kopyalayın.

:::image type="content" source="media/how-to-create-azure-function/adt-hostname.png" alt-text="Azure portal: genel bakış-> _Value_ alanında kullanılacak ana bilgisayar adını kopyalayın.":::

Artık aşağıdaki adımları izleyerek bir uygulama ayarı oluşturabilirsiniz:

* Arama çubuğunda işlev adı ' nı kullanarak Azure işlevinizi arayın ve listeden işlevi seçin
* Yeni uygulama ayarı oluşturmak için sol taraftaki Gezinti çubuğunda _yapılandırma_ ' yı seçin.
* _Uygulama ayarları_ sekmesinde _+ Yeni uygulama ayarı_ ' nı seçin.

:::image type="content" source="media/how-to-create-azure-function/search-for-azure-function.png" alt-text="Azure portal: mevcut Azure işlevi için arama":::

:::image type="content" source="media/how-to-create-azure-function/application-setting.png" alt-text="Azure portal: uygulama ayarlarını yapılandırma":::

Açılan pencerede, bir uygulama ayarı oluşturmak için yukarıdan kopyalanmış değeri kullanın. \
_Ad_  : ADT_SERVICE_URL \
_Değer_ : https://{-Azure-dijital-TWINS-hostname}

Bir uygulama ayarı oluşturmak için _Tamam ' ı_ seçin.

:::image type="content" source="media/how-to-create-azure-function/add-application-setting.png" alt-text="Azure portal: uygulama ayarlarını ekleyin.":::

Uygulama ayarlarınızı _ad_ alanı altında uygulama adı ile görüntüleyebilirsiniz. Sonra, _Kaydet_ düğmesini seçerek uygulama ayarlarınızı kaydedin.

:::image type="content" source="media/how-to-create-azure-function/application-setting-save-details.png" alt-text="Azure portal: oluşturulan uygulamayı görüntüleyin ve uygulamayı yeniden başlatın":::

Uygulama ayarlarındaki değişikliklerin uygulamanın yeniden başlatılması gerekir. Uygulamanızı yeniden başlatmak için _devam_ ' ı seçin.

:::image type="content" source="media/how-to-create-azure-function/save-application-setting.png" alt-text="Azure portal: uygulama ayarlarını kaydet":::

_Bildirimler_ simgesini seçerek uygulama ayarlarının güncelleştirildiğini görebilirsiniz. Uygulama ayarınız oluşturulmadıysa, yukarıdaki işlemi izleyerek bir uygulama ayarı eklemeyi yeniden deneyebilirsiniz.

:::image type="content" source="media/how-to-create-azure-function/notifications-update-web-app-settings.png" alt-text="Azure portal: uygulama ayarlarını güncelleştirme bildirimleri":::

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Digital TWINS ile kullanmak üzere bir Azure işlevi ayarlama adımları izlenir. Sonra, bir uç nokta dinlemek için Azure işlevinizi Event Grid 'e abone olabilirsiniz. Bu uç nokta şu olabilir:
* Azure dijital TWINS 'den gelen iletileri işlemek için Azure dijital TWINS 'e bağlı Event Grid uç noktası (özellik değişiklik iletileri, ikizi grafiğinde [dijital TWINS](concepts-twins-graph.md) tarafından oluşturulan telemetri iletileri veya yaşam döngüsü iletileri gibi)
* Telemetri ve diğer cihaz olaylarını göndermek için IoT Hub tarafından kullanılan IoT sistem konuları
* Diğer hizmetlerden ileti alma Event Grid uç noktası

Daha sonra, Azure dijital TWINS 'e IoT Hub verileri almak için temel Azure işlevinizde nasıl derleme yapılacağını inceleyin:
* [*Nasıl yapılır: IoT Hub 'dan alma telemetrisi*](how-to-ingest-iot-hub-data.md)