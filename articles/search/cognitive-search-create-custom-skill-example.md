---
title: Bing Entity Search API'yi kullanarak özel beceri örneği
titleSuffix: Azure Cognitive Search
description: Bing Varlık Arama hizmetini, Azure Bilişsel Arama'da AI ile zenginleştirilmiş dizin oluşturma boru hattına eşlenmiş özel bir beceride kullanmayı gösterir.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 2994c55b39d30ff16a0ca135e93a116784feb201
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113809"
---
# <a name="example-create-a-custom-skill-using-the-bing-entity-search-api"></a>Örnek: Bing Entity Search API'sini kullanarak özel bir beceri oluşturun

Bu örnekte, web API özel beceri oluşturmak için nasıl öğrenin. Bu beceri konumları, tanınmış kişileri ve kuruluşları kabul eder ve onlar için açıklamaları döndürecektir. Örnek, Bing Varlık Arama [API'sini](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) özel beceri arabirimini uygulayacak şekilde sarmak için bir [Azure İşlevi](https://azure.microsoft.com/services/functions/) kullanır.

## <a name="prerequisites"></a>Ön koşullar

+ Özel [bir becerinin](cognitive-search-custom-skill-interface.md) uygulaması gereken giriş/çıktı arabirimine aşina değilseniz, özel beceri arabirimi makalesi hakkında bilgi edinin.

+ [!INCLUDE [cognitive-services-bing-entity-search-signup-requirements](../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

+ Azure geliştirme iş yükü de dahil olmak üzere [Visual Studio 2019](https://www.visualstudio.com/vs/) veya sonraki lerini yükleyin.

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu örnek, bir web API'sini barındırmak için bir Azure İşlevi kullansa da, gerekli değildir.  Bilişsel bir beceri [için arayüz gereksinimlerini](cognitive-search-custom-skill-interface.md)karşıladığınız sürece, aldığınız yaklaşım önemsizdir. Ancak Azure İşlevler, özel bir beceri oluşturmayı kolaylaştırır.

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

1. Visual Studio'da Dosya menüsünden **Yeni** > **Proje'yi** seçin.

1. Yeni Proje iletişim **kutusunda, Yüklenilen'** i seçin , **Visual C#** > **Cloud'u**genişletin, Azure **İşlevlerini**seçin, projeniz için bir Ad yazın ve **Tamam'ı**seçin. İşlev uygulaması adı C# ad alanı olarak geçerli olmalıdır, bu nedenle alt çizgi, tire veya diğer alfanümerik olmayan karakterleri kullanmayın.

1. **Azure Fonksiyonları v2 (.NET Core) seçeneğini**belirleyin. Ayrıca sürüm 1 ile de yapabilirsiniz, ancak aşağıda yazılan kod v2 şablonuna dayanmaktadır.

1. **HTTP Tetikleyici** olacak türü seçin

1. Depolama Hesabı için, bu işlev için herhangi bir depolama alanına ihtiyacınız olmayacağından **Yok'u**seçebilirsiniz.

1. İşlev projesini ve HTTP tetiklenen işlevi oluşturmak için **Tamam'ı** seçin.

### <a name="modify-the-code-to-call-the-bing-entity-search-service"></a>Bing Varlık Arama Hizmeti'ni aramak için kodu değiştirme

Visual Studio bir proje oluşturur ve bu projenin içinde seçili işlev türü için ortak kod içeren bir sınıf bulunur. Metottaki *FunctionName* özniteliği işlevin adını ayarlar. *HttpTrigger* özniteliği, işlevin bir HTTP isteği tarafından tetiklenip tetiklenmediğini belirtir.

Şimdi, dosyanın tüm içeriğini *Function1.cs* aşağıdaki kodla değiştirin:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

namespace SampleSkills
{
    /// <summary>
    /// Sample custom skill that wraps the Bing entity search API to connect it with a 
    /// AI enrichment pipeline.
    /// </summary>
    public static class BingEntitySearch
    {
        #region Credentials
        // IMPORTANT: Make sure to enter your credential and to verify the API endpoint matches yours.
        static readonly string bingApiEndpoint = "https://api.cognitive.microsoft.com/bing/v7.0/entities/";
        static readonly string key = "<enter your api key here>";  
        #endregion

        #region Class used to deserialize the request
        private class InputRecord
        {
            public class InputRecordData
            {
                public string Name { get; set; }
            }

            public string RecordId { get; set; }
            public InputRecordData Data { get; set; }
        }

        private class WebApiRequest
        {
            public List<InputRecord> Values { get; set; }
        }
        #endregion

        #region Classes used to serialize the response

        private class OutputRecord
        {
            public class OutputRecordData
            {
                public string Name { get; set; } = "";
                public string Description { get; set; } = "";
                public string Source { get; set; } = "";
                public string SourceUrl { get; set; } = "";
                public string LicenseAttribution { get; set; } = "";
                public string LicenseUrl { get; set; } = "";
            }

            public class OutputRecordMessage
            {
                public string Message { get; set; }
            }

            public string RecordId { get; set; }
            public OutputRecordData Data { get; set; }
            public List<OutputRecordMessage> Errors { get; set; }
            public List<OutputRecordMessage> Warnings { get; set; }
        }

        private class WebApiResponse
        {
            public List<OutputRecord> Values { get; set; }
        }
        #endregion

        #region Classes used to interact with the Bing API
        private class BingResponse
        {
            public BingEntities Entities { get; set; }
        }
        private class BingEntities
        {
            public BingEntity[] Value { get; set; }
        }

        private class BingEntity
        {
            public class EntityPresentationinfo
            {
                public string[] EntityTypeHints { get; set; }
            }

            public class License
            {
                public string Url { get; set; }
            }

            public class ContractualRule
            {
                public string _type { get; set; }
                public License License { get; set; }
                public string LicenseNotice { get; set; }
                public string Text { get; set; }
                public string Url { get; set; }
            }

            public ContractualRule[] ContractualRules { get; set; }
            public string Description { get; set; }
            public string Name { get; set; }
            public EntityPresentationinfo EntityPresentationInfo { get; set; }
        }
        #endregion

        #region The Azure Function definition

        [FunctionName("EntitySearch")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("Entity Search function: C# HTTP trigger function processed a request.");

            var response = new WebApiResponse
            {
                Values = new List<OutputRecord>()
            };

            string requestBody = new StreamReader(req.Body).ReadToEnd();
            var data = JsonConvert.DeserializeObject<WebApiRequest>(requestBody);

            // Do some schema validation
            if (data == null)
            {
                return new BadRequestObjectResult("The request schema does not match expected schema.");
            }
            if (data.Values == null)
            {
                return new BadRequestObjectResult("The request schema does not match expected schema. Could not find values array.");
            }

            // Calculate the response for each value.
            foreach (var record in data.Values)
            {
                if (record == null || record.RecordId == null) continue;

                OutputRecord responseRecord = new OutputRecord
                {
                    RecordId = record.RecordId
                };

                try
                {
                    responseRecord.Data = GetEntityMetadata(record.Data.Name).Result;
                }
                catch (Exception e)
                {
                    // Something bad happened, log the issue.
                    var error = new OutputRecord.OutputRecordMessage
                    {
                        Message = e.Message
                    };

                    responseRecord.Errors = new List<OutputRecord.OutputRecordMessage>
                    {
                        error
                    };
                }
                finally
                {
                    response.Values.Add(responseRecord);
                }
            }

            return (ActionResult)new OkObjectResult(response);
        }

        #endregion

        #region Methods to call the Bing API
        /// <summary>
        /// Gets metadata for a particular entity based on its name using Bing Entity Search
        /// </summary>
        /// <param name="entityName">The name of the entity to extract data for.</param>
        /// <returns>Asynchronous task that returns entity data. </returns>
        private async static Task<OutputRecord.OutputRecordData> GetEntityMetadata(string entityName)
        {
            var uri = bingApiEndpoint + "?q=" + entityName + "&mkt=en-us&count=10&offset=0&safesearch=Moderate";
            var result = new OutputRecord.OutputRecordData();

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage {
                Method = HttpMethod.Get,
                RequestUri = new Uri(uri)
            })
            {
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                HttpResponseMessage response = await client.SendAsync(request);
                string responseBody = await response?.Content?.ReadAsStringAsync();

                BingResponse bingResult = JsonConvert.DeserializeObject<BingResponse>(responseBody);
                if (bingResult != null)
                {
                    // In addition to the list of entities that could match the name, for simplicity let's return information
                    // for the top match as additional metadata at the root object.
                    return AddTopEntityMetadata(bingResult.Entities?.Value);
                }
            }

            return result;
        }

        private static OutputRecord.OutputRecordData AddTopEntityMetadata(BingEntity[] entities)
        {
            if (entities != null)
            {
                foreach (BingEntity entity in entities.Where(
                    entity => entity?.EntityPresentationInfo?.EntityTypeHints != null
                        && (entity.EntityPresentationInfo.EntityTypeHints[0] == "Person"
                            || entity.EntityPresentationInfo.EntityTypeHints[0] == "Organization"
                            || entity.EntityPresentationInfo.EntityTypeHints[0] == "Location")
                        && !String.IsNullOrEmpty(entity.Description)))
                {
                    var rootObject = new OutputRecord.OutputRecordData
                    {
                        Description = entity.Description,
                        Name = entity.Name
                    };

                    if (entity.ContractualRules != null)
                    {
                        foreach (var rule in entity.ContractualRules)
                        {
                            switch (rule._type)
                            {
                                case "ContractualRules/LicenseAttribution":
                                    rootObject.LicenseAttribution = rule.LicenseNotice;
                                    rootObject.LicenseUrl = rule.License.Url;
                                    break;
                                case "ContractualRules/LinkAttribution":
                                    rootObject.Source = rule.Text;
                                    rootObject.SourceUrl = rule.Url;
                                    break;
                            }
                        }
                    }

                    return rootObject;
                }
            }

            return new OutputRecord.OutputRecordData();
        }
        #endregion
    }
}
```

Bing varlık arama *key* API'sine `key` kaydolurken aldığınız anahtara göre sabitte kendi anahtar değerinizi girdiğinizden emin olun.

Bu örnek, kolaylık sağlamak için tek bir dosyada gerekli tüm kodu içerir. Güç [becerileri deposunda](https://github.com/Azure-Samples/azure-search-power-skills/tree/master/Text/BingEntitySearch)aynı becerinin biraz daha yapılandırılmış bir sürümünü bulabilirsiniz.

Tabii ki, dosyayı ' `Function1.cs` dan `BingEntitySearch.cs`' a yeniden adlandırabilirsiniz.

## <a name="test-the-function-from-visual-studio"></a>Visual Studio'dan işlevi test edin

Programı çalıştırmak ve işlev davranışlarını test etmek için **F5** tuşuna basın. Bu durumda, iki varlığı aramak için aşağıdaki işlevi kullanacağız. Aşağıda gösterildiği gibi bir çağrı yayınlamak için Postacı veya Fiddler'ı kullanın:

```http
POST https://localhost:7071/api/EntitySearch
```

### <a name="request-body"></a>İstek gövdesi
```json
{
    "values": [
        {
            "recordId": "e1",
            "data":
            {
                "name":  "Pablo Picasso"
            }
        },
        {
            "recordId": "e2",
            "data":
            {
                "name":  "Microsoft"
            }
        }
    ]
}
```

### <a name="response"></a>Yanıt
Aşağıdaki örneğe benzer bir yanıt görmeniz gerekir:

```json
{
    "values": [
        {
            "recordId": "e1",
            "data": {
                "name": "Pablo Picasso",
                "description": "Pablo Ruiz Picasso was a Spanish painter [...]",
                "source": "Wikipedia",
                "sourceUrl": "http://en.wikipedia.org/wiki/Pablo_Picasso",
                "licenseAttribution": "Text under CC-BY-SA license",
                "licenseUrl": "http://creativecommons.org/licenses/by-sa/3.0/"
            },
            "errors": null,
            "warnings": null
        },
        "..."
    ]
}
```

## <a name="publish-the-function-to-azure"></a>İşlevi Azure’a yayımlama

İşlev davranışından memnun olduğunuzda, yayımlayabilirsiniz.

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.  >  **Yeni Yayımla'yı Seçin.****Publish**

1. Visual Studio'yı Azure hesabınıza zaten bağlamadıysanız, **hesap ekle'yi seçin....**

1. Ekrandaki istemleri izleyin. Uygulama hizmetiniz, Azure aboneliğiniz, kaynak grubu, barındırma planı ve kullanmak istediğiniz depolama hesabı için benzersiz bir ad belirtmeniz istenir. Bunlara zaten sahip değilseniz yeni bir kaynak grubu, yeni bir barındırma planı ve depolama hesabı oluşturabilirsiniz. Tamamlandığında, **Oluştur'u** seçin

1. Dağıtım tamamlandıktan sonra Site URL'sini not edin. Azure'daki işlev uygulamanızın adresidir. 

1. Azure [portalında](https://portal.azure.com)Kaynak Grubu'na gidin ve `EntitySearch` yayınladığınız İşlev'i arayın. **Yönet** bölümünün altında Ana Bilgisayar Anahtarlarını görmeniz gerekir. *Varsayılan* ana bilgisayar anahtarı için **Kopyala** simgesini seçin.  

## <a name="test-the-function-in-azure"></a>Azure'da işlevi test edin

Varsayılan ana bilgisayar anahtarına sahip olduğunuza göre, işlevinizi aşağıdaki gibi sınayın:

```http
POST https://[your-entity-search-app-name].azurewebsites.net/api/EntitySearch?code=[enter default host key here]
```

### <a name="request-body"></a>İstek Gövdesi
```json
{
    "values": [
        {
            "recordId": "e1",
            "data":
            {
                "name":  "Pablo Picasso"
            }
        },
        {
            "recordId": "e2",
            "data":
            {
                "name":  "Microsoft"
            }
        }
    ]
}
```

Bu örnek, yerel ortamda işlevi çalıştırırken daha önce gördüğünüz aynı sonucu üretmelidir.

## <a name="connect-to-your-pipeline"></a>Ardınıza bağlanın
Şimdi yeni bir özel beceri var, sizin skillset ekleyebilirsiniz. Aşağıdaki örnekte, belgedeki kuruluşlara açıklamalar eklemek için beceriyi nasıl çağırabileceğinizi gösterir (bu, konumlar ve kişiler üzerinde çalışmak üzere de genişletilebilir). Uygulamanızın adıyla değiştirin. `[your-entity-search-app-name]`

```json
{
    "skills": [
      "[... your existing skills remain here]",  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new Bing entity search custom skill",
        "uri": "https://[your-entity-search-app-name].azurewebsites.net/api/EntitySearch?code=[enter default host key here]",
          "context": "/document/merged_content/organizations/*",
          "inputs": [
            {
              "name": "name",
              "source": "/document/merged_content/organizations/*"
            }
          ],
          "outputs": [
            {
              "name": "description",
              "targetName": "description"
            }
          ]
      }
  ]
}
```

Burada, yerleşik [varlık tanıma becerisinin](cognitive-search-skill-entity-recognition.md) becerilerde mevcut olacağına ve belgeyi organizasyon listesiyle zenginleştirmelerine güveniyoruz. Başvuru için, ihtiyacımız olan verileri oluşturmada yeterli olacak bir varlık çıkarma beceri yapılandırması aşağıda veda edebilirsiniz:

```json
{
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "name": "#1",
    "description": "Organization name extraction",
    "context": "/document/merged_content",
    "categories": [ "Organization" ],
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/merged_content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "organizations",
            "targetName": "organizations"
        }
    ]
},
```

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! İlk özel yeteneğini yarattın. Artık kendi özel işlevselliğinizi eklemek için aynı deseni takip edebilirsiniz. Daha fazla bilgi için aşağıdaki bağlantıları tıklatın.

+ [Güç Becerileri: özel becerilerin deposu](https://github.com/Azure-Samples/azure-search-power-skills)
+ [AI zenginleştirme hattına özel bir beceri ekleme](cognitive-search-custom-skill-interface.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Skillset (REST) oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanların haritası nasıl eşlenir?](cognitive-search-output-field-mapping.md)
