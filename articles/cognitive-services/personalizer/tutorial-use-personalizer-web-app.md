---
title: Web uygulaması kullanma-kişiselleştirici
description: Eylemler (özelliklerle birlikte) ve bağlam özellikleriyle bir kullanıcıya doğru içerik sağlamak için bir kişiselleştirme döngüsüyle bir C# .NET Web uygulamasını özelleştirin.
ms.topic: tutorial
ms.date: 06/10/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: e9ce4c433a038008b1ffd75dc6c4b2f9d0b57fde
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88935628"
---
# <a name="tutorial-add-personalizer-to-a-net-web-app"></a>Öğretici: bir .NET Web uygulamasına kişiselleştirici ekleme

Eylemler (özelliklerle birlikte) ve bağlam özellikleriyle bir kullanıcıya doğru içerik sağlamak için bir kişiselleştirme döngüsüyle bir C# .NET Web uygulamasını özelleştirin.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Kişiselleştirici anahtar ve uç noktası ayarlama
> * Özellikleri topla
> * Arama derecesi ve Reward API 'Leri
> * _Rewarterctionıd_ olarak belirlenmiş olan en iyi eylemi görüntüle



## <a name="select-the-best-content-for-a-web-app"></a>Bir Web uygulaması için en iyi içeriği seçin

Web sayfasında, görüntülenecek tek bir üst öğeye (Rewarterctionıd) kişiselleştirilmesi gereken _eylemlerin_ (bazı içerik türleri) bir listesi olduğunda, kişiselleştirici kullanılmalıdır. Eylem listesi örnekleri arasında haber makaleleri, düğme yerleştirme konumları ve ürün adları için Word seçenekleri bulunur.

Eylem listesini, bağlam özellikleriyle birlikte, kişiselleştirici döngüsüne gönderirsiniz. Kişiselleştirici tek en iyi eylemi seçerse, Web uygulamanız bu eylemi görüntüler.

Bu öğreticide, eylemler yiyecek türleridir:

* pasta
* dondurma
* Juice
* Salad
* popcorn
* ünüzü
* çorba

Kişiselleştiriciye, eylemleriniz hakkında bilgi edinin ve her bir derecelendirme API isteği ile özellik ve _bağlam özellikleriyle_ _eylemleri_ gönderin.

Modelin bir **özelliği** , Web uygulaması Kullanıcı tabanınızın üyeleri genelinde toplanabilecek eylem veya içerikle ilgili bilgiler. Bir özellik ayrı ayrı (Kullanıcı KIMLIĞI gibi) veya yüksek oranda özel (günün tam saati gibi) _değildir_ .

### <a name="actions-with-features"></a>Özelliklerle ilgili eylemler

Her eylem (içerik öğesi), yiyecek öğesini ayırt etmenize yardımcı olan özelliklere sahiptir.

Özellikler, Azure portal döngü yapılandırmasının bir parçası olarak yapılandırılmamıştır. Bunun yerine, her derecelendirme API çağrısıyla bir JSON nesnesi olarak gönderilirler. Bu, eylemlerin ve özelliklerinin zaman içinde büyümesini, değiştirilmesini ve daralmasına olanak tanır. bu sayede, Kişiselleştiriciye eğilimleri takip edebilirsiniz.

```csharp
 /// <summary>
  /// Creates personalizer actions feature list.
  /// </summary>
  /// <returns>List of actions for personalizer.</returns>
  private IList<RankableAction> GetActions()
  {
      IList<RankableAction> actions = new List<RankableAction>
      {
          new RankableAction
          {
              Id = "pasta",
              Features =
              new List<object>() { new { taste = "savory", spiceLevel = "medium" }, new { nutritionLevel = 5, cuisine = "italian" } }
          },

          new RankableAction
          {
              Id = "ice cream",
              Features =
              new List<object>() { new { taste = "sweet", spiceLevel = "none" }, new { nutritionalLevel = 2 } }
          },

          new RankableAction
          {
              Id = "juice",
              Features =
              new List<object>() { new { taste = "sweet", spiceLevel = "none" }, new { nutritionLevel = 5 }, new { drink = true } }
          },

          new RankableAction
          {
              Id = "salad",
              Features =
              new List<object>() { new { taste = "sour", spiceLevel = "low" }, new { nutritionLevel = 8 } }
          },

          new RankableAction
          {
              Id = "popcorn",
              Features =
              new List<object>() { new { taste = "salty", spiceLevel = "none" }, new { nutritionLevel = 3 } }
          },

          new RankableAction
          {
              Id = "coffee",
              Features =
              new List<object>() { new { taste = "bitter", spiceLevel = "none" }, new { nutritionLevel = 3 }, new { drink = true } }
          },

          new RankableAction
          {
              Id = "soup",
              Features =
              new List<object>() { new { taste = "sour", spiceLevel = "high" }, new { nutritionLevel =  7} }
          }
      };

      return actions;
  }
```


## <a name="context-features"></a>Bağlam özellikleri

Bağlam özellikleri, kişiselleştirici eylemlerin bağlamını anlamalarına yardımcı olur. Bu örnek uygulamanın bağlamı şunları içerir:

* Günün saati-sabah, öğleden sonra akşam, gece
* Kullanıcı, tatlı, SWE, Bitter, Sour veya savun tercihi
* tarayıcının bağlamı-Kullanıcı Aracısı, coğrafi konum, başvuran

```csharp
/// <summary>
/// Get users time of the day context.
/// </summary>
/// <returns>Time of day feature selected by the user.</returns>
private string GetUsersTimeOfDay()
{
    Random rnd = new Random();
    string[] timeOfDayFeatures = new string[] { "morning", "noon", "afternoon", "evening", "night", "midnight" };
    int timeIndex = rnd.Next(timeOfDayFeatures.Length);
    return timeOfDayFeatures[timeIndex];
}

/// <summary>
/// Gets user food preference.
/// </summary>
/// <returns>Food taste feature selected by the user.</returns>
private string GetUsersTastePreference()
{
    Random rnd = new Random();
    string[] tasteFeatures = new string[] { "salty", "bitter", "sour", "savory", "sweet" };
    int tasteIndex = rnd.Next(tasteFeatures.Length);
    return tasteFeatures[tasteIndex];
}
```

## <a name="how-does-the-web-app-use-personalizer"></a>Web uygulaması kişiselleştirici nasıl kullanır?

Web uygulaması, yemek seçimleri listesinden en iyi eylemi seçmek için kişiselleştirici kullanır. Bu, her bir derecelendirme API çağrısıyla aşağıdaki bilgileri göndererek bunu yapar:
* ve gibi özelliklerine sahip **Eylemler** `taste``spiceLevel`
* **context** `time` gün, kullanıcının `taste` tercihi ve tarayıcının Kullanıcı Aracısı bilgileri ve bağlam özellikleri gibi bağlam özellikleri
* Bu gibi **tutulacak eylemler**
* Her derecelendirme API çağrısı için farklı olan **EventID**.

## <a name="personalizer-model-features-in-a-web-app"></a>Web uygulamasındaki kişiselleştirici model özellikleri

Kişiselleştirici, eylemler (içerik) ve geçerli bağlam (Kullanıcı ve ortam) için özelliklere ihtiyaç duyuyor. Özellikler, işlemleri modeldeki geçerli içeriğe hizalamak için kullanılır. Model, kişisel kararlar alma kararı veren eylemler, bağlam ve özellikleri hakkında Kişiselleştiriciye ait geçmiş bilgisinin bir gösterimidir.

Özellikler dahil olmak üzere model, Azure portal **model güncelleştirme sıklığı** ayarı temelinde bir zamanlamaya göre güncelleştirilir.

> [!CAUTION]
> Bu uygulamadaki özellikler, özellikleri ve özellik değerlerini göstermek için, ancak bir Web uygulamasında kullanmak için en iyi özelliklerin olması anlamına gelir.

### <a name="plan-for-features-and-their-values"></a>Özellikleri ve değerlerini planlayın

Özellikler, teknik mimarinizdeki herhangi bir şema veya modele uygulayabileceğiniz planlama ve tasarıma sahip olacak şekilde seçilmelidir. Özellik değerleri, iş mantığı veya üçüncü taraf sistemlerle ayarlanabilir. Özellik değerleri, bir grup veya özellik sınıfında uygulanmadığından yüksek oranda özel olmamalıdır.

### <a name="generalize-feature-values"></a>Genelleştirme özellik değerleri

#### <a name="generalize-into-categories"></a>Kategorilere Genelleştirme

Bu uygulama `time` bir özellik olarak kullanır, ancak saat,, ve gibi kategoriler halinde gruplar `morning` `afternoon` `evening` `night` . Bu, zaman bilgilerinin kullanılmasına bir örnektir, ancak gibi yüksek ölçüde belirli bir şekilde değil `10:05:01 UTC+2` .

#### <a name="generalize-into-parts"></a>Parçalar halinde Genelleştirme

Bu uygulama, tarayıcıdan gelen HTTP Isteği özelliklerini kullanır. Bu, tüm verileri içeren çok özel bir dizeyle başlar, örneğin:

```http
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/530.99 (KHTML, like Gecko) Chrome/80.0.3900.140 Safari/537.36
```

**Httprequestfeatures** sınıf kitaplığı, bu dizeyi ayrı değerlerle bir **useragentınfo** nesnesine genelleştirir. Çok özgü olan tüm değerler boş bir dizeye ayarlanır. İstek için içerik özellikleri gönderildiğinde, aşağıdaki JSON biçimine sahiptir:

```JSON
{
  "httpRequestFeatures": {
    "_synthetic": false,
    "OUserAgent": {
      "_ua": "",
      "_DeviceBrand": "",
      "_DeviceFamily": "Other",
      "_DeviceIsSpider": false,
      "_DeviceModel": "",
      "_OSFamily": "Windows",
      "_OSMajor": "10",
      "DeviceType": "Desktop"
    }
  }
}
```


## <a name="using-sample-web-app"></a>Örnek Web uygulaması kullanma

Örnek tarayıcı tabanlı Web uygulaması (tüm kod sağlanır) uygulamayı çalıştırmak için aşağıdaki uygulamaların yüklü olması gerekir.

Aşağıdaki yazılımları yükleyin:

* [.Net core 2,1](https://dotnet.microsoft.com/download/dotnet-core/2.1) -örnek arka uç sunucusu .NET Core kullanır
* [Node.js](https://nodejs.org/) -istemci/ön uç bu uygulamaya bağlıdır
* [Visual studio 2019](https://visualstudio.microsoft.com/vs/)veya [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/) -uygulamayı derlemek ve çalıştırmak için Visual studio 2019 ya da .NET Core CLI geliştirici ortamını kullanın

### <a name="set-up-the-sample"></a>Örneği ayarlama
1. Azure kişiselleştirici örnek deposu ' nu kopyalayın.

    ```bash
    git clone https://github.com/Azure-Samples/cognitive-services-personalizer-samples.git
    ```

1. Çözümü açmak için _Samples/HttpRequestFeatures_ öğesine gidin `HttpRequestFeaturesExample.sln` .

    İstenirse, Visual Studio 'Nun kişiselleştirici için .NET paketini güncelleştirmesine izin verin.

### <a name="set-up-azure-personalizer-service"></a>Azure kişiselleştirici hizmetini ayarlama

1. Azure portal [bir kişiselleştirici kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer) .

1. Azure portal, `Endpoint` `Key1` `Key2` **anahtarlar ve uç noktalar** sekmesinde ve ya da (çalışır) öğesini bulun. Bunlar `PersonalizerServiceEndpoint` ve sizin `PersonalizerApiKey` .
1. `PersonalizerServiceEndpoint` **Üzerindeappsettings.js**' ın üzerine girin.
1. `PersonalizerApiKey`Aşağıdaki yollarla bir [uygulama gizli](https://docs.microsoft.com/aspnet/core/security/app-secrets) dizileri olarak yapılandırın:

    * .NET Core CLI kullanıyorsanız `dotnet user-secrets set "PersonalizerApiKey" "<API Key>"` komutunu kullanabilirsiniz.
    * Visual Studio kullanıyorsanız, projeyi sağ tıklayıp kişiselleştirme anahtarlarını yapılandırmak için **Kullanıcı gizli dizilerini Yönet** menü seçeneğini belirleyebilirsiniz. Bunu yaptığınızda, Visual Studio `secrets.json` anahtarları aşağıdaki gibi ekleyebileceğiniz bir dosya açar:

    ```JSON
    {
      "PersonalizerApiKey": "<your personalizer key here>",
    }
    ```

## <a name="run-the-sample"></a>Örneği çalıştırma

Aşağıdaki yöntemlerden biriyle Httprequestfeaturesexörnek oluşturun ve çalıştırın:

* Visual Studio 2019: **F5** tuşuna basın
* .NET Core CLI: `dotnet build``dotnet run`

Bir Web tarayıcısı aracılığıyla, bir derecelendirme isteği ve bir istek isteği gönderebilir ve bu isteklerin yanı sıra ortamınızdan ayıklanan http istek özelliklerini görebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Httprequestfeaturesexbol projesini derleyin ve çalıştırın. Tek sayfalı uygulamayı göstermek için bir tarayıcı penceresi açılır.](./media/tutorial-web-app/web-app-single-page.png)

## <a name="demonstrate-the-personalizer-loop"></a>Kişiselleştirici döngüsünü gösterir

1. Derece API çağrısı için yeni bir JSON nesnesi oluşturmak üzere **Yeni bir derecelendirme Isteği oluştur** düğmesini seçin. Bu işlem eylemleri (özelliklerle birlikte) ve bağlam özelliklerini oluşturur ve bu sayede JSON 'ın nasıl göründüğünü görebilmeniz için değerleri görüntüler.

    Kendi kendinize ait uygulamanız için, istemci, sunucuda, iki karışımı veya diğer hizmetlere yapılan çağrılarla birlikte eylemler ve özellikler oluşturma işlemi meydana gelebilir.

1. JSON nesnesini sunucuya göndermek için **Derecelendirme Isteği gönder** ' i seçin. Sunucu, kişiselleştirici derecelendirme API 'sini çağırır. Sunucu yanıtı alır ve istemciye görüntülenecek üst dereceli eylemi döndürür.

1. Ödül değerini ayarlayın ve ardından **yeniden istek gönder** düğmesini seçin. Yeniden değer değerini değiştirmezseniz, istemci uygulaması her zaman değerini `1` Kişiselleştiriciye gönderir.

    > [!div class="mx-imgBorder"]
    > ![Httprequestfeaturesexbol projesini derleyin ve çalıştırın. Tek sayfalı uygulamayı göstermek için bir tarayıcı penceresi açılır.](./media/tutorial-web-app/reward-score-api-call.png)

    Gelecekteki uygulamanız için, kullanıcının istemci davranışından, sunucuda iş mantığı ile ilgili bilgiler toplandıktan sonra yeniden Puanlama oluşturma işlemi meydana gelebilir.

## <a name="understand-the-sample-web-app"></a>Örnek Web uygulamasını anlama

Örnek Web uygulaması, özelliklerin toplanmasını ve kişiselleştirici uç noktanıza HTTP çağrıları göndermeyi ve almayı yöneten bir **C# .net** sunucusuna sahiptir.

Örnek Web uygulaması, özellikleri yakalamak ve düğmelere tıklanıp .NET sunucusuna veri göndermek gibi Kullanıcı Arabirimi eylemlerini işlemek için bir **gizleme ön uç istemci uygulaması** kullanır.

Aşağıdaki bölümlerde, bir geliştiricinin kişiselleştirici kullanmak için anlaşılması gereken sunucu ve istemci bölümleri açıklanmaktadır.

## <a name="rank-api-client-application-sends-context-to-server"></a>Derecelendirme API 'SI: Istemci uygulaması sunucuya bağlam gönderir

İstemci uygulaması kullanıcının tarayıcı _Kullanıcı aracısını_toplar.

> [!div class="mx-imgBorder"]
> ![Httprequestfeaturesexbol projesini derleyin ve çalıştırın. Tek sayfalı uygulamayı göstermek için bir tarayıcı penceresi açılır.](./media/tutorial-web-app/user-agent.png)

## <a name="rank-api-server-application-calls-personalizer"></a>Derecelendirme API 'SI: sunucu uygulaması, kişiselleştirici çağırır

Bu, bir istemci uygulaması olan tipik bir .NET Web uygulamasıdır ve sizin için Boiler levha kodunun çoğunu temin etyordu. Kişiselleştiriciye özgü olmayan herhangi bir kod aşağıdaki kod parçacıklarında kaldırılarak, Kişiselleştiriciye özgü koda odaklanabilmenizi sağlar.

### <a name="create-personalizer-client"></a>Kişiselleştirici istemcisi oluştur

Sunucu **Startup.cs**, kişiselleştirme uç noktası ve anahtarı, kişiselleştirici istemcisini oluşturmak için kullanılır. İstemci uygulamasının bu uygulamada, bu SDK çağrısı yapmak için sunucuya güvenmek yerine, bu uygulamadaki Kişiselleştiriciye iletişim kurması gerekmez.

Web sunucusunun .NET başlangıç kodu:

```csharp
using Microsoft.Azure.CognitiveServices.Personalizer;
// ... other using statements removed for brevity

namespace HttpRequestFeaturesExample
{
    public class Startup
    {
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            string personalizerApiKey = Configuration.GetSection("PersonalizerApiKey").Value;
            string personalizerEndpoint = Configuration.GetSection("PersonalizerConfiguration:ServiceEndpoint").Value;
            if (string.IsNullOrEmpty(personalizerEndpoint) || string.IsNullOrEmpty(personalizerApiKey))
            {
                throw new ArgumentException("Missing Azure Personalizer endpoint and/or api key.");
            }
            services.AddSingleton(client =>
            {
                return new PersonalizerClient(new ApiKeyServiceClientCredentials(personalizerApiKey))
                {
                    Endpoint = personalizerEndpoint
                };
            });

            services.AddMvc();
        }

        // ... code removed for brevity
    }
}
```

### <a name="select-best-action"></a>En iyi eylemi seçin

Sunucunun **PersonalizerController.cs**, **GenerateRank** Server API 'si, derecelendirme API 'sini çağırma hazırlığını özetler

* `eventId`Derecelendirme çağrısı için yeni oluştur
* Eylemlerin listesini al
* Kullanıcının özellik listesini al ve bağlam özellikleri oluştur
* İsteğe bağlı olarak, hariç tutulan eylemleri ayarlayın
* Arama derecesi API 'SI, sonuçları istemciye döndür

```csharp
/// <summary>
/// Creates a RankRequest with user time of day, HTTP request features,
/// and taste as the context and several different foods as the actions
/// </summary>
/// <returns>RankRequest with user info</returns>
[HttpGet("GenerateRank")]
public RankRequest GenerateRank()
{
    string eventId = Guid.NewGuid().ToString();

    // Get the actions list to choose from personalizer with their features.
    IList<RankableAction> actions = GetActions();

    // Get context information from the user.
    HttpRequestFeatures httpRequestFeatures = GetHttpRequestFeaturesFromRequest(Request);
    string timeOfDayFeature = GetUsersTimeOfDay();
    string tasteFeature = GetUsersTastePreference();

    // Create current context from user specified data.
    IList<object> currentContext = new List<object>() {
            new { time = timeOfDayFeature },
            new { taste = tasteFeature },
            new { httpRequestFeatures }
    };

    // Exclude an action for personalizer ranking. This action will be held at its current position.
    IList<string> excludeActions = new List<string> { "juice" };

    // Rank the actions
    return new RankRequest(actions, currentContext, excludeActions, eventId);
}
```

Kişiselleştiriciye gönderilen ve hem eylemleri (özelliklerle birlikte) hem de geçerli bağlam özelliklerini içeren JSON, şöyle görünür:

```json
{
    "contextFeatures": [
        {
            "time": "morning"
        },
        {
            "taste": "savory"
        },
        {
            "httpRequestFeatures": {
                "_synthetic": false,
                "MRefer": {
                    "referer": "http://localhost:51840/"
                },
                "OUserAgent": {
                    "_ua": "",
                    "_DeviceBrand": "",
                    "_DeviceFamily": "Other",
                    "_DeviceIsSpider": false,
                    "_DeviceModel": "",
                    "_OSFamily": "Windows",
                    "_OSMajor": "10",
                    "DeviceType": "Desktop"
                }
            }
        }
    ],
    "actions": [
        {
            "id": "pasta",
            "features": [
                {
                    "taste": "savory",
                    "spiceLevel": "medium"
                },
                {
                    "nutritionLevel": 5,
                    "cuisine": "italian"
                }
            ]
        },
        {
            "id": "ice cream",
            "features": [
                {
                    "taste": "sweet",
                    "spiceLevel": "none"
                },
                {
                    "nutritionalLevel": 2
                }
            ]
        },
        {
            "id": "juice",
            "features": [
                {
                    "taste": "sweet",
                    "spiceLevel": "none"
                },
                {
                    "nutritionLevel": 5
                },
                {
                    "drink": true
                }
            ]
        },
        {
            "id": "salad",
            "features": [
                {
                    "taste": "sour",
                    "spiceLevel": "low"
                },
                {
                    "nutritionLevel": 8
                }
            ]
        },
        {
            "id": "popcorn",
            "features": [
                {
                    "taste": "salty",
                    "spiceLevel": "none"
                },
                {
                    "nutritionLevel": 3
                }
            ]
        },
        {
            "id": "coffee",
            "features": [
                {
                    "taste": "bitter",
                    "spiceLevel": "none"
                },
                {
                    "nutritionLevel": 3
                },
                {
                    "drink": true
                }
            ]
        },
        {
            "id": "soup",
            "features": [
                {
                    "taste": "sour",
                    "spiceLevel": "high"
                },
                {
                    "nutritionLevel": 7
                }
            ]
        }
    ],
    "excludedActions": [
        "juice"
    ],
    "eventId": "82ac52da-4077-4c7d-b14e-190530578e75",
    "deferActivation": null
}
```

### <a name="return-personalizer-rewardactionid-to-client"></a>Kişiselleştirici yeniden Warterctionıd değerini istemciye döndür

Rank API 'SI, seçilen en iyi eylemi **yeniden Warterctionıd** 'yi sunucuya döndürür.

**Rewarterctionıd**içinde döndürülen eylemi görüntüleyin.

```json
{
    "ranking": [
        {
            "id": "popcorn",
            "probability": 0.833333254
        },
        {
            "id": "salad",
            "probability": 0.03333333
        },
        {
            "id": "juice",
            "probability": 0
        },
        {
            "id": "soup",
            "probability": 0.03333333
        },
        {
            "id": "coffee",
            "probability": 0.03333333
        },
        {
            "id": "pasta",
            "probability": 0.03333333
        },
        {
            "id": "ice cream",
            "probability": 0.03333333
        }
    ],
    "eventId": "82ac52da-4077-4c7d-b14e-190530578e75",
    "rewardActionId": "popcorn"
}
```

### <a name="client-displays-the-rewardactionid-action"></a>İstemci Rewarterctionıd eylemini görüntüler

Bu öğreticide, `rewardActionId` değer görüntülenir.

Daha sonra gelen uygulamanızda, bir tam metin, bir düğme veya Web sayfasının vurgulandığı bir bölüm olabilir. Liste, içeriğin sıralanmasından değil, puanları analiz sonrası için döndürülür. Yalnızca `rewardActionId` içerik görüntülenmelidir.

## <a name="reward-api-collect-information-for-reward"></a>API: ödül için bilgi toplama

Özellikler planlandığından, [ödül puanı](concept-rewards.md) dikkatle planlanmalıdır. Ödül puanı genellikle 0 ile 1 arasında bir değer olmalıdır. _Değer,_ Kullanıcı davranışlarına ve kısmen sunucuya, iş mantığı ve hedeflerine göre kısmen istemci uygulamasında hesaplanabilir.

Sunucu, kişiselleştirici kaynağınız için Azure Portal yapılandırılan **ödül** API 'sini çağırmazsa, bu olay için **varsayılan ödül** (Azure Portal de yapılandırılır) kullanılır.

Bu örnek uygulamada, seçimleri nasıl etkilediğini görmek için bir değer seçebilirsiniz.

## <a name="additional-ways-to-learn-from-this-sample"></a>Bu örnekten daha fazla bilgi edinmek için ek yollar

Örnek, kişiselleştirici kaynağınız için Azure portal yapılandırılmış çeşitli zamana dayalı olayları kullanır. Bu değerlerle yürütün ve bu örnek Web uygulamasına geri dönüp değişikliklerin derece ve geri aramaları nasıl etkilediğini görün:

* Bekleme süresi
* Model güncelleştirme sıklığı

İle yürütmeye yönelik ek ayarlar şunlardır:
* Varsayılan değer
* Keşif yüzdesi


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyi tamamladığınızda, aşağıdaki kaynakları temizleyin:

* Örnek proje dizininizi silin.
* Kişiselleştirici kaynağınızı silme-bir kişiselleştirici kaynağı, eylemlere ve içeriğe ayrılmış olarak düşünün. Bu durumda, hala bir konu olarak da varsa, bu kaynağı eylemler konu etki alanı olarak kullanıyorsanız kaynak yeniden kullanın.


## <a name="next-steps"></a>Sonraki adımlar
* [Kişiselleştirme nasıl çalışır?](how-personalizer-works.md)
* [Özellikler](concepts-features.md): eylemler ve bağlamla kullanılan özellikler hakkında kavramları öğrenin
* [Re,](concept-rewards.md)ödüller: yeniden temelsiz hesaplamayı öğrenin
