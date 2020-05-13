---
title: Azure API Management kullanarak işlevlerinizi Openapı ile kullanıma sunma
description: Azure’da diğer uygulama ve hizmetlerin işlevinize çağrı yapmasına imkan tanıyan bir OpenAPI tanımı oluşturun.
ms.topic: tutorial
ms.date: 04/21/2020
ms.reviewer: sunayv
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 7d63d5ea17184ffa6e456877079da0821a75d59e
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83121494"
---
# <a name="create-an-openapi-definition-for-a-serverless-api-using-azure-api-management"></a>Azure API Management kullanarak sunucusuz bir API için Openapı tanımı oluşturma

REST API 'Ler genellikle bir Openapı tanımı kullanılarak açıklanır. Bu tanım, bir API’de hangi işlemlerin kullanılabildiğinin yanı sıra API için istek ve yanıt verilerinin nasıl yapılandırılması gerektiğiyle ilgili bilgileri içerir.

Bu öğreticide, bir rüzgar türbini için acil onarımın uygun maliyetli olup olmadığını belirleyen bir işlev oluşturursunuz. Daha sonra, işlevin diğer uygulamalardan ve hizmetlerden çağrılabilmesi için [Azure API Management](../api-management/api-management-key-concepts.md) kullanarak işlev uygulaması Için bir openapı tanımı oluşturursunuz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da işlev oluşturma
> * Azure API Management kullanarak bir Openapı tanımı oluşturma
> * İşleve çağrı yaparak tanımı test etme
> * Openapı tanımını indirin

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, kaynakların daha kolay yönetilmesi, dağıtılması, ölçeklendirilmesi ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-the-function"></a>İşlevi oluşturma

Bu öğretici, iki parametre alan HTTP ile tetiklenen bir işlev kullanır:

* Bir türbin onarımı için saat cinsinden tahmini süre.
* Kilowatts cinsinden türbin kapasitesi. 

Bu işlev daha sonra bir onarımın ne kadar maliyetlendirilebileceğini ve turbin 'in 24 saatlik bir dönemde ne kadar gelir yapıp yapabileceğini hesaplar. [Azure Portal](https://portal.azure.com)http ile tetiklenen işlevi oluşturmak için:

1. İşlevler uygulamanızın sol menüsünde **işlevler**' i seçin ve ardından üst menüden **Ekle** ' yi seçin.

1. **Yeni işlev** penceresinde **http tetikleyicisi**' ni seçin.

1. **Yeni işlev**için girin `TurbineRepair` . 

1. **[Yetkilendirme düzeyi](functions-bindings-http-webhook-trigger.md#http-auth)** açılan listesinden **işlev** ' ı seçin ve ardından **işlev oluştur**' u seçin.

    :::image type="content" source="media/functions-openapi-definition/select-http-trigger-openapi.png" alt-text="Openapı için HTTP işlevi oluşturma":::

1. **Kod + test**' i seçin ve ardından açılır listeden **. CSX 'i Çalıştır** ' ı seçin. Run. CSX C# betik dosyasının içeriğini aşağıdaki kodla değiştirin ve ardından **Kaydet**' i seçin:

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

    Bu işlev kodu, `Yes` `No` acil bir onarımın uygun maliyetli olup olmadığını göstermek için veya iletisini döndürür. Ayrıca, Turbin 'in gösterdiği gelir fırsatını ve turbin ' i çözme maliyetini de döndürür.

1. İşlevi test etmek için **Test**' i seçin, **giriş** sekmesini seçin, **gövde**için aşağıdaki girişi girin ve **Çalıştır**' ı seçin:

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

    :::image type="content" source="media/functions-openapi-definition/test-function.png" alt-text="İşlevi Azure portalında test etme":::

    **Çıkış** sekmesinde aşağıdaki çıktı döndürülür:

    ```json
    {"message":"Yes","revenueOpportunity":"$7200","costToFix":"$1600"}
    ```

Acil onarımların maliyet açısından uygunluğunu belirleyen bir işleviniz oldu. Sonra, işlev uygulaması için bir Openapı tanımı oluşturursunuz.

## <a name="generate-the-openapi-definition"></a>OpenAPI tanımını oluşturma

Openapı tanımını oluşturmak için:

1. İşlev uygulamasını seçin, sol menüden **API Management** öğesini seçin ve ardından **API Management** **Yeni oluştur** ' u seçin.

    :::image type="content" source="media/functions-openapi-definition/select-all-settings-openapi.png" alt-text="API Management seçin":::


1. Aşağıdaki tabloda belirtildiği gibi API Management ayarlarını kullanın:

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Adı** | Genel olarak benzersiz bir ad | İşlev uygulamanızın adı temel alınarak bir ad oluşturulur. |
    | **Abonelik** | Aboneliğiniz | Bu yeni kaynağın altında oluşturulduğu abonelik. |  
    | **[Kaynak grubu](../azure-resource-manager/management/overview.md)** |  myResourceGroup | Sizin için ayarlanmış olması gereken işlev uygulamanız ile aynı kaynak. |
    | **Konum** | Batı ABD | Batı ABD konumunu seçin. |
    | **Kuruluş adı** | Contoso | Geliştirici portalında kullanılan kuruluşun adı ve e-posta bildirimleri için. |
    | **Yönetici e-postası** | e-postanız | API Management 'den sistem bildirimleri alan e-posta. |
    | **Fiyatlandırma Katmanı** | Tüketim | Tüketim katmanı tüm bölgelerde kullanılabilir değildir. Tüm fiyatlandırma ayrıntıları için [API Management fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/api-management/) bakın |

    ![Yeni API Management hizmeti oluştur](media/functions-openapi-definition/new-apim-service-openapi.png)

1. API Management örneğini oluşturmak için **Oluştur**'u seçin; bu işlem birkaç dakika sürebilir.

1. Azure örneği oluşturduktan sonra, sayfada **etkinleştir Application Insights** seçeneğini etkinleştirir. Günlükleri işlev uygulamasıyla aynı yere göndermek için bunu seçin ve ardından **API bağlantısı**' nı seçin.

1. **Içeri aktarma Azure işlevleri** , **Turbinerepair** işlevi vurgulanmış şekilde açılır. Devam etmek için **Seç**'i seçin.

    ![Azure Işlevlerini API Management içine aktarın](media/functions-openapi-definition/import-function-openapi.png)

1. **İşlev uygulaması oluştur** sayfasında, varsayılan değerleri kabul edin ve **Oluştur**' u seçin.

    :::image type="content" source="media/functions-openapi-definition/create-function-openapi.png" alt-text="İşlev Uygulaması Oluştur":::

    Azure işlevi için API 'YI oluşturur.

## <a name="test-the-api"></a>API’yi test etme

Openapı tanımını kullanmadan önce API 'nin çalıştığını doğrulamanız gerekir.

1. İşlev uygulaması sayfanızda **API Management**öğesini seçin, **Test** sekmesini seçin ve ardından **turbinerepair gönder**' i seçin. 

1. **İstek gövdesine**aşağıdaki kodu girin:

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

1. **Gönder**' i seçin ve ardından **HTTP yanıtını**görüntüleyin.

    :::image type="content" source="media/functions-openapi-definition/test-function-api-openapi.png" alt-text="Test işlevi API 'SI":::

## <a name="download-the-openapi-definition"></a>Openapı tanımını indirin

API 'niz beklendiği gibi çalışıyorsa, Openapı tanımını indirebilirsiniz.

1. Sayfanın üst kısmında **Openapı tanımını indir** ' i seçin.
   
   ![OpenAPI tanımını indirin](media/functions-openapi-definition/download-definition.png)

2. İndirilen JSON dosyasını kaydedin ve sonra açın. Tanımı gözden geçirin.

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

İşlevlerinizin bir Openapı tanımını oluşturmak için API Management tümleştirme kullandınız. Tanımı artık portalda API Management düzenleyebilirsiniz. Ayrıca [API Management hakkında daha fazla bilgi](../api-management/api-management-key-concepts.md)edinebilirsiniz.

> [!div class="nextstepaction"]
> [API Management 'de Openapı tanımını düzenleme](../api-management/edit-api.md)
