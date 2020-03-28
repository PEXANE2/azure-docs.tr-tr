---
title: Azure API Yönetimi'ni kullanarak OpenAPI ile işlevlerinizi ortaya çıkarın
description: Azure’da diğer uygulama ve hizmetlerin işlevinize çağrı yapmasına imkan tanıyan bir OpenAPI tanımı oluşturun.
ms.topic: tutorial
ms.date: 05/08/2019
ms.reviewer: sunayv
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 9465209467c83f7de075d16e724459c307d55bd3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77210217"
---
# <a name="create-an-openapi-definition-for-a-serverless-api-using-azure-api-management"></a>Azure API Yönetimi'ni kullanarak sunucusuz api için OpenAPI tanımı oluşturma

REST API'leri genellikle OpenAPI tanımı kullanılarak tanımlanır. Bu tanım, bir API’de hangi işlemlerin kullanılabildiğinin yanı sıra API için istek ve yanıt verilerinin nasıl yapılandırılması gerektiğiyle ilgili bilgileri içerir.

Bu öğreticide, bir rüzgar türbini için acil onarımın uygun maliyetli olup olmadığını belirleyen bir işlev oluşturursunuz. Daha sonra, işlevin diğer uygulamalardan ve hizmetlerden çağrılayabilmeleri için [Azure API Yönetimi'ni](../api-management/api-management-key-concepts.md) kullanarak işlev uygulaması için bir OpenAPI tanımı oluşturursunuz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure’da işlev oluşturma
> * Azure API Yönetimini kullanarak openapi tanımı oluşturma
> * İşleve çağrı yaparak tanımı test etme
> * OpenAPI tanımını indirin

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlevleri daha kolay yönetim, dağıtım, ölçekleme ve kaynakların paylaşımı için mantıksal bir birim olarak gruplandırmanıza olanak tanır.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-the-function"></a>İşlevi oluşturma

Bu öğretici, iki parametre alan bir HTTP tetiklenen işlevi kullanır:

* Bir türbin tamiri için tahmini süre, saat içinde.
* Türbinin kapasitesi, kilowatt olarak. 

Daha sonra, işlev tarafından onarım maliyetinin ne olacağı ve türbinin 24 saatlik bir dönemde ne kadar gelir kazandırabileceği hesaplanır. [Azure portalında](https://portal.azure.com)HTTP tetiklenen işlevi oluşturmak için:

1. İşlev uygulamanızı genişletin ve **+** **Fonksiyonlar'ın**yanındaki düğmeyi seçin. Portal**Içi'ni**seç **In-portal** > Devam et.

1. **Daha Fazla şablon seçin...** **Finish and view templates**

1. HTTP tetikleyicisini `TurbineRepair` seçin, işlev **Adı**için yazın, **[Kimlik Doğrulama düzeyi](functions-bindings-http-webhook-trigger.md#http-auth)** için seçin `Function` ve sonra **Oluştur'u**seçin.  

    ![OpenAPI için HTTP işlevi oluşturma](media/functions-openapi-definition/select-http-trigger-openapi.png)

1. run.csx C# komut dosyası dosyasının içeriğini aşağıdaki kodla değiştirin ve **kaydet**seçeneğini belirleyin:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    const double revenuePerkW = 0.12;
    const double technicianCost = 250;
    const double turbineCost = 100;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        // Get query strings if they exist
        int tempVal;
        int? hours = Int32.TryParse(req.Query["hours"], out tempVal) ? tempVal : (int?)null;
        int? capacity = Int32.TryParse(req.Query["capacity"], out tempVal) ? tempVal : (int?)null;
    
        // Get request body
        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
    
        // Use request body if a query was not sent
        capacity = capacity ?? data?.capacity;
        hours = hours ?? data?.hours;
    
        // Return bad request if capacity or hours are not passed in
        if (capacity == null || hours == null){
            return new BadRequestObjectResult("Please pass capacity and hours on the query string or in the request body");
        }
        // Formulas to calculate revenue and cost
        double? revenueOpportunity = capacity * revenuePerkW * 24;  
        double? costToFix = (hours * technicianCost) +  turbineCost;
        string repairTurbine;
    
        if (revenueOpportunity > costToFix){
            repairTurbine = "Yes";
        }
        else {
            repairTurbine = "No";
        };
    
        return (ActionResult)new OkObjectResult(new{
            message = repairTurbine,
            revenueOpportunity = "$"+ revenueOpportunity,
            costToFix = "$"+ costToFix
        });
    }
    ```

    Bu işlev kodu, acil onarımın uygun maliyetli olup olmadığının yanı sıra türbinin temsil ettiği gelir fırsatını ve türbin onarımının maliyetini gösteren `Yes` veya `No` şeklinde bir ileti döndürür.

1. İşlevi test etmek için, test sekmesini genişletmek için en sağdaki **Test'i** tıklatın. **İstek gövdesi**için aşağıdaki değeri girin ve ardından **Çalıştır'ı**tıklatın.

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

    ![İşlevi Azure portalında test etme](media/functions-openapi-definition/test-function.png)

    Yanıtın gövdesinde aşağıdaki değer döndürülür.

    ```json
    {"message":"Yes","revenueOpportunity":"$7200","costToFix":"$1600"}
    ```

Acil onarımların maliyet açısından uygunluğunu belirleyen bir işleviniz oldu. Ardından, işlev uygulaması için bir OpenAPI tanımı oluşturursunuz.

## <a name="generate-the-openapi-definition"></a>OpenAPI tanımını oluşturma

Artık OpenAPI tanımını oluşturmaya hazırsınız.

1. İşlev uygulamasını seçin, ardından **Platform özelliklerinde,** **API Yönetimi'ni** seçin ve **API Yönetimi**altında **yeni oluştur'u** seçin.

    ![Platform Özelliklerinde API Yönetimini Seçin](media/functions-openapi-definition/select-all-settings-openapi.png)

1. Görüntünün altındaki tabloda belirtildiği gibi API Yönetimi ayarlarını kullanın.

    ![Yeni API Yönetimi hizmeti oluşturun](media/functions-openapi-definition/new-apim-service-openapi.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Adı** | Genel olarak benzersiz bir ad | İşlev uygulamanızın adına göre bir ad oluşturulur. |
    | **Abonelik** | Aboneliğiniz | Bu yeni kaynağın oluşturulduğu abonelik. |  
    | **[Kaynak Grubu](../azure-resource-manager/management/overview.md)** |  myResourceGroup | Sizin için ayarlanmalıdır işlev uygulaması, aynı kaynak. |
    | **Konum** | Batı ABD | Batı ABD konumunu seçin. |
    | **Kuruluş adı** | Contoso | Geliştirici portalında ve e-posta bildirimleri için kullanılan kuruluşun adı. |
    | **Yönetici e-postası** | e-posta adresiniz | API Yönetimi'nden sistem bildirimleri alan e-posta. |
    | **Fiyatlandırma katmanı** | Tüketim (önizleme) | Tüketim katmanı önizlemededir ve tüm bölgelerde kullanılamaz. Tam fiyatlandırma ayrıntıları için [API Yönetimi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/api-management/) bakın |

1. API Management örneğini oluşturmak için **Oluştur**'u seçin; bu işlem birkaç dakika sürebilir.

1. İşlev uygulamasıyla aynı yere günlük göndermek için **Uygulama Öngörülerini Etkinleştir'i** seçin, ardından kalan varsayılanları kabul edin ve **Bağlantı API'sini**seçin.

1. **Azure İşlerini Alma,** **TurbineRepair** işlevi vurgulanmış olarak açılır. Devam etmek için **Seç**'i seçin.

    ![Azure Fonksiyonlarını API Yönetimine Aktar](media/functions-openapi-definition/import-function-openapi.png)

1. **İşlev Uygulamasından Oluştur** sayfasında varsayılanları kabul edin ve **Oluştur'u** seçin

    ![Fonksiyon Uygulamasından Oluştur](media/functions-openapi-definition/create-function-openapi.png)

API şimdi işlev için oluşturulur.

## <a name="test-the-api"></a>API’yi test etme

OpenAPI tanımını kullanmadan önce API'nin çalıştığını doğrulamanız gerekir.

1. İşlevinizin **Test** sekmesinde **POST** işlemini seçin.

1. **Saat** ve **kapasite** için değerleri girin

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

1. **Gönder'i**tıklatın, ardından HTTP yanıtını görüntüleyin.

    ![Test fonksiyonu API](media/functions-openapi-definition/test-function-api-openapi.png)

## <a name="download-the-openapi-definition"></a>OpenAPI tanımını indirin

API'niz beklendiği gibi çalışıyorsa, OpenAPI tanımını indirebilirsiniz.

1. Sayfanın üst kısmında **OpenAPI tanımını indir'i** seçin.
   
   ![OpenAPI tanımını indirin](media/functions-openapi-definition/download-definition.png)

2. İndirilen JSON dosyasını açın ve tanımı gözden geçirin.

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

İşlevlerinizin OpenAPI tanımını oluşturmak için API Yönetimi tümleştirmesini kullandınız. Artık portaldaki API Yönetimi'ndeki tanımı edebilirsiniz. [Ayrıca API Yönetimi hakkında daha fazla bilgi edinebilirsiniz.](../api-management/api-management-key-concepts.md)

> [!div class="nextstepaction"]
> [API Yönetiminde OpenAPI tanımını edin](../api-management/edit-api.md)
