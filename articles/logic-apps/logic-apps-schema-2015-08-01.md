---
title: Ağustos 1-2015 Preview için şema güncelleştirmeleri
description: Güncelleştirilmiş şema sürümü 2015-08-01-Azure Logic Apps içindeki Logic App tanımlarının önizlemesi
services: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 05/31/2016
ms.openlocfilehash: b6746baaede777eb8c2afcae9eb3fe80b669c468
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74792850"
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Azure Logic Apps için şema güncelleştirmeleri-1 Ağustos 2015 Preview

Azure Logic Apps için bu şema ve API sürümü, mantıksal uygulamaları daha güvenilir ve kullanımı kolay hale getirmek için önemli geliştirmeler içerir:

* **Apiapp** eylem türü artık [**Apiconnection**](#api-connections)olarak adlandırılmıştır.
* **Yinele** eylemi şimdi [**foreach**](#foreach)olarak adlandırılmıştır.
* [ **Http dinleyicisi** API 'si uygulamasına](#http-listener) artık gerek yok.
* Alt iş akışlarını çağırmak [Yeni bir şema](#child-workflows)kullanır.

<a name="api-connections"></a>

## <a name="move-to-api-connections"></a>API bağlantılarına taşı

En büyük değişiklik, API 'Leri kullanabilmeniz için artık Azure aboneliğinize API Apps dağıtmanız gerekmez. API 'Leri kullanabileceğiniz yollar şunlardır:

* Yönetilen API 'Ler
* Özel Web API 'leriniz

Yönetim ve barındırma modelleri farklı olduğundan her bir yöntem biraz farklı işlenir. Bu modelin bir avantajı, artık Azure Kaynak grubunuzda dağıtılan kaynaklarla sınırlandırılırsınız. 

### <a name="managed-apis"></a>Yönetilen API 'Ler

Microsoft, sizin adınıza Office 365, Salesforce, Twitter ve FTP gibi bazı API 'Leri yönetir. Bazı yönetilen API 'Leri, örneğin, Bing çeviri gibi, diğer bir deyişle *bağlantı*olarak da adlandırılan yapılandırma için kullanabilirsiniz.

Örneğin, Office 365 kullandığınızda Office 365 oturum açma belirtecinizi içeren bir bağlantı oluşturmanız gerekir. Belirteciniz, mantıksal uygulamanızın Office 365 API 'sini her zaman çağırabilmesi için güvenli bir şekilde depolanır ve yenilenir. SQL veya FTP sunucunuza bağlanmak istiyorsanız bağlantı dizesine sahip bir bağlantı oluşturmanız gerekir. 

Bu tanımda, bu eylemler `APIConnection`olarak adlandırılır. Aşağıda, bir e-posta göndermek için Office 365 ' i çağıran bir bağlantı örneği verilmiştir:

``` json
{
   "actions": {
      "Send_an_email": {
         "type": "ApiConnection",
         "inputs": {
            "host": {
               "api": {
                  "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
               },
               "connection": {
                  "name": "@parameters('$connections')['shared_office365']['connectionId']"
               }
            },
            "method": "POST",
            "body": {
               "Subject": "Reminder",
               "Body": "Don't forget!",
               "To": "me@contoso.com"
            },
            "path": "/Mail"
         }
      }
   }
}
```

`host` nesnesi, API bağlantıları için benzersiz olan girdilerin bir parçasıdır ve şu bölümleri içerir: `api` ve `connection`. `api` nesnesi, yönetilen API 'nin barındırıldığı çalışma zamanı URL 'sini belirtir. Bu yöntemi çağırarak, kullanılabilir tüm yönetilen API 'Leri görebilirsiniz:

```text
GET https://management.azure.com/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/locations/<location>/managedApis?api-version=2015-08-01-preview
```

Bir API kullandığınızda, bu API herhangi bir *bağlantı parametresi*tanımlamış olabilir veya olmayabilir. Bu nedenle, API bu parametreleri tanımlamıyorsa bağlantı gerekmez. API bu parametreleri tanımlamıyorsa, belirtilen ada sahip bir bağlantı oluşturmanız gerekir.  
Daha sonra bu ada `host` nesnesinin içindeki `connection` nesnesinde başvurulamıyor. Bir kaynak grubunda bağlantı oluşturmak için şu yöntemi çağırın:

```text
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/<name>?api-version=2015-08-01-preview
```

Aşağıdaki gövdeden:

``` json
{
   "properties": {
      "api": {
         "id": "/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/managedApis/azureblob"
      },
      "parameterValues": {
         "accountName": "<Azure-storage-account-name-with-different-parameters-for-each-API>"
      }
   },
   "location": "<logic-app-location>"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Yönetilen API 'Leri bir Azure Resource Manager şablonunda dağıtma

Etkileşimli oturum açma gerekli olmadığında, bir Kaynak Yöneticisi şablonu kullanarak tam bir uygulama oluşturabilirsiniz.
Oturum açma gerekliyse, Kaynak Yöneticisi şablonu kullanmaya devam edebilirsiniz, ancak bağlantıları Azure portal aracılığıyla yetkilendirebilirsiniz. 

``` json
"resources": [ {
   "apiVersion": "2015-08-01-preview",
   "name": "azureblob",
   "type": "Microsoft.Web/connections",
   "location": "[resourceGroup().location]",
   "properties": {
      "api": {
         "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
      },
      "parameterValues": {
         "accountName": "[parameters('storageAccountName')]",
         "accessKey": "[parameters('storageAccountKey')]"
      }
    },
},
{
   "type": "Microsoft.Logic/workflows",
   "apiVersion": "2015-08-01-preview",
   "name": "[parameters('logicAppName')]",
   "location": "[resourceGroup().location]",
   "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"],
   "properties": {
      "sku": {
         "name": "[parameters('sku')]",
         "plan": {
            "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('svcPlanName'))]"
         }
      },
      "parameters": {
         "$connections": {
             "value": {
                  "azureblob": {
                     "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                     "connectionName": "azureblob",
                     "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                  }
             }
         }
      },
      "definition": {
         "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "type": "Object",
            "$connections": {
               "defaultValue": {},
 
            }
         },
         "triggers": {
            "Recurrence": {
               "type": "Recurrence",
               "recurrence": {
                  "frequency": "Day",
                  "interval": 1
               }
            }
         },
         "actions": {
            "Create_file": {
               "type": "ApiConnection",
               "inputs": {
                  "host": {
                     "api": {
                        "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                     },
                     "connection": {
                       "name": "@parameters('$connections')['azureblob']['connectionId']"
                     }
                  },
                  "method": "POST",
                  "queries": {
                     "folderPath": "[concat('/', parameters('containerName'))]",
                     "name": "helloworld.txt"
                  },
                  "body": "@decodeDataUri('data:, Hello+world!')",
                  "path": "/datasets/default/files"
               },
               "conditions": []
            }
         },
         "outputs": {}
      }
   }
} ]
```

Bu örnekte, bağlantıların yalnızca kaynak grubunuzda yaşayan kaynaklar olduğunu görebilirsiniz. Bunlar, aboneliğinizde kullanabileceğiniz yönetilen API 'Lere başvurırlar.

### <a name="your-custom-web-apis"></a>Özel Web API 'leriniz

Microsoft tarafından yönetilen bir yerine kendi API 'lerinizi kullanıyorsanız, API 'lerinizi çağırmak için yerleşik **http** eylemini kullanın. İdeal olarak, API 'niz için bir Swagger uç noktası sağlamalısınız. Bu uç nokta Logic App Designer 'ın API 'nizin girişlerini ve çıktılarını göstermesini sağlar. Swagger uç noktası olmadan, tasarımcı yalnızca girdileri ve çıkışları donuk JSON nesneleri olarak gösterebilir.

Yeni `metadata.apiDefinitionUrl` özelliğini gösteren bir örnek aşağıda verilmiştir:

``` json
"actions": {
   "mycustomAPI": {
      "type": "Http",
      "metadata": {
         "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
      },
      "inputs": {
         "uri": "https://mysite.azurewebsites.net/api/getsomedata",
         "method": "GET"
      }
   }
}
```

Web API 'nizi Azure App Service barındırdıysanız, Web API 'niz tasarımcıda bulunan Eylemler listesinde otomatik olarak görüntülenir. Aksi takdirde, URL 'ye doğrudan yapıştırmanız gerekir. Swagger uç noktasının, mantıksal uygulama tasarımcısında kullanılabilmesi için kimliği doğrulanmamış olmalıdır, ancak bu API 'YI Swagger tarafından desteklenen herhangi bir yöntemle güvenli hale getirebilirsiniz.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>2015-08-01-Preview ile dağıtılan API Apps 'i çağırma

Daha önce bir API uygulaması dağıttıysanız, bu uygulamayı **http** eylemiyle çağırabilirsiniz.
Örneğin, dosyaları listelemek için Dropbox kullanırsanız **2014-12-01-Preview** şema sürümü tanımınızda şöyle bir işlem olabilir:

``` json
"definition": {
   "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
         "defaultValue": "eyJ0eX...wCn90",
         "type": "String",
         "metadata": {
            "token": {
               "name": "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
            }
         }
      }
    },
    "actions": {
       "dropboxconnector": {
          "type": "ApiApp",
          "inputs": {
             "apiVersion": "2015-01-14",
             "host": {
                "id": "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                "gateway": "https://avdemo.azurewebsites.net"
             },
             "operation": "ListFiles",
             "parameters": {
                "FolderPath": "/myfolder"
             },
             "authentication": {
                "type": "Raw",
                "scheme": "Zumo",
                "parameter": "@parameters('/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
             }
          }
       }
    }
}
```

Artık benzer bir HTTP eylemi oluşturabilir ve mantıksal uygulama tanımının `parameters` bölümünü değiştirmeden bırakabilirsiniz, örneğin:

``` json
"actions": {
   "dropboxconnector": {
      "type": "Http",
      "metadata": {
         "apiDefinitionUrl": "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
      },
      "inputs": {
         "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
         "method": "POST",
         "body": {
            "FolderPath": "/myfolder"
         },
         "authentication": {
            "type": "Raw",
            "scheme": "Zumo",
            "parameter": "@parameters('/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
         }
      }
   }
}
```

Bu özelliklerden tek tek yürüme:

| Action özelliği | Açıklama |
| --- | --- |
| `type` | `APIapp` yerine `Http` |
| `metadata.apiDefinitionUrl` | Bu eylemi Logic App Designer 'da kullanmak için, öğesinden oluşturulan meta veri uç noktasını ekleyin: `{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Oluşturulan: `{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Her zaman `POST` |
| `inputs.body` | API uygulaması parametreleriyle aynı |
| `inputs.authentication` | API uygulaması kimlik doğrulamasıyla aynı |

Bu yaklaşım tüm API uygulama eylemleri için çalışmalıdır. Bununla birlikte, önceki API Apps artık desteklenmediğini unutmayın. Bu nedenle, yönetilen bir API veya özel Web API 'nizi barındırarak önceki iki seçenekten birine geçmeniz gerekir.

<a name="foreach"></a>

## <a name="renamed-repeat-to-foreach"></a>' Yinele ' olarak yeniden adlandırıldı ' foreach '

Önceki şema sürümü için, **yineleme** eylemi adının kafa karıştırıcı olduğunu ve bu **yinelemeyi** doğru bir For-Each döngüsü olduğunu çok fazla müşteri geri bildirimi aldık. Bu nedenle, `foreach``repeat` yeniden adlandırdık. Daha önce bu eylemi şu örnekte olduğu gibi yazarsınız:

``` json
"actions": {
   "pingBing": {
      "type": "Http",
      "repeat": "@range(0,2)",
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com/search?q=@{repeatItem()}"
      }
   }
}
```

Bunun yerine şu sürümü yazarsınız:

``` json
"actions": {
   "pingBing": {
      "type": "Http",
      "foreach": "@range(0,2)",
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com/search?q=@{item()}"
      }
   }
}
```

Ayrıca, döngünün geçerli yineleme sırasında işlediği öğeye başvuran `repeatItem()` işlevi, artık `item()`olarak yeniden adlandırıldı. 

### <a name="reference-outputs-from-foreach"></a>' Foreach ' öğesinden başvuru çıkışları

Basitme için `foreach` eylemleriyle ilgili çıktılar artık `repeatItems`adlı bir nesne içinde sarmalanır. Ayrıca, bu değişikliklerle `repeatItem()`, `repeatBody()`ve `repeatOutputs()` işlevleri de kaldırılır.

Bu nedenle, önceki `repeat` örneği kullanılarak şu çıktılar alınır:

``` json
"repeatItems": [ {
   "name": "pingBing",
   "inputs": {
      "uri": "https://www.bing.com/search?q=0",
      "method": "GET"
   },
   "outputs": {
      "headers": { },
      "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"https://schemas.live.com/Web/\">...</html>"
   },
   "status": "Succeeded"
} ]
```

Bu çıktıları artık bunun yerine alacaksınız:

``` json
[ {
   "name": "pingBing",
      "inputs": {
         "uri": "https://www.bing.com/search?q=0",
         "method": "GET"
      },
      "outputs": {
         "headers": { },
         "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"https://schemas.live.com/Web/\">...</html>"
      },
      "status": "Succeeded"
} ]
```

Daha önce, bu çıkışlara başvururken eylemden `body` almak için:

``` json
"actions": {
   "secondAction": {
      "type": "Http",
      "repeat": "@outputs('pingBing').repeatItems",
      "inputs": {
         "method": "POST",
         "uri": "https://www.example.com",
         "body": "@repeatItem().outputs.body"
      }
   }
}
```

Artık bunun yerine bu sürümü kullanabilirsiniz:

``` json
"actions": {
   "secondAction": {
      "type": "Http",
      "foreach": "@outputs('pingBing')",
      "inputs": {
         "method": "POST",
         "uri": "https://www.example.com",
         "body": "@item().outputs.body"
      }
   }
}
```

<a name="http-listener"></a>

## <a name="native-http-listener"></a>Yerel HTTP dinleyicisi

HTTP dinleyicisi özellikleri artık yerleşik olarak bulunur, bu nedenle bir HTTP dinleyicisi API uygulaması dağıtmanız gerekmez. Daha fazla bilgi için [mantıksal uygulama uç noktanızı çağrılabilir hale getirme](../logic-apps/logic-apps-http-endpoint.md)hakkında bilgi edinin. 

Bu değişikliklerle Logic Apps, `@accessKeys()` işlevini `@listCallbackURL()` işlevi ile değiştirir ve bu, gerektiğinde uç noktasını alır. Ayrıca, artık mantıksal uygulamanızda en az bir tetikleyici tanımlamanız gerekir. İş akışını `/run` istiyorsanız, şu tetikleyici türlerinden birini kullanmanız gerekir: `Manual`, `ApiConnectionWebhook`veya `HttpWebhook`

<a name="child-workflows"></a>

## <a name="call-child-workflows"></a>Alt iş akışlarını çağır

Daha önce, iş akışına gitmeleri gereken alt iş akışlarını çağırma, erişim belirtecini alma ve bu alt iş akışını çağırmak istediğiniz mantıksal uygulama tanımına belirteci yapıştırma. Bu şemayla, Logic Apps altyapısı alt iş akışı için çalışma zamanında otomatik olarak bir SAS oluşturur, bu sayede herhangi bir gizli dizi yapıştırmak zorunda kalmazsınız. Örnek aşağıda verilmiştir:

``` json
"myNestedWorkflow": {
   "type": "Workflow",
   "inputs": {
      "host": {
         "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/myWorkflow001",
         "triggerName": "myEndpointTrigger"
      },
      "queries": {
         "extrafield": "specialValue"
      },
      "headers": {
         "x-ms-date": "@utcnow()",
         "Content-type": "application/json"
      },
      "body": {
         "contentFieldOne": "value100",
         "anotherField": 10.001
      }
   },
   "conditions": []
}
```

Ayrıca, alt iş akışları gelen isteğe tam erişim alır. Bu nedenle, parametreleri `queries` bölümünde ve `headers` nesnesinde geçirebilirsiniz. Ayrıca tüm `body` bölümünü tam olarak tanımlayabilirsiniz.

Son olarak, alt iş akışlarında bu gerekli değişiklikler vardır. Daha önce ve doğrudan bir alt iş akışını çağırabilir, ancak artık üst öğe çağrısı için iş akışında bir tetikleyici uç noktası tanımlamanız gerekir. Genellikle, `Manual` türüne sahip bir tetikleyici ekler ve sonra bu tetikleyiciyi üst tanımda kullanırsınız. `host` özelliği, çağıran tetikleyiciyi her zaman belirtmeniz gerektiğinden, özellikle bir `triggerName` sahiptir.

## <a name="other-changes"></a>Diğer değişiklikler

### <a name="new-queries-property"></a>Yeni ' Queries ' özelliği

Tüm eylem türleri artık `queries`adlı yeni bir girişi desteklemektedir. Bu giriş, dizeyi el ile birleştirmek zorunda kalmak yerine, yapılandırılmış bir nesne olabilir.

### <a name="renamed-parse-function-to-json"></a>' Parse () ' işlevi ' JSON () ' olarak yeniden adlandırıldı

`parse()` işlevi artık gelecekteki içerik türleri için `json()` işlevini yeniden adlandırmıştır.

## <a name="enterprise-integration-apis"></a>Kurumsal Tümleştirme API 'Leri

Bu şema, AS2 gibi Kurumsal Tümleştirme API 'Leri için yönetilen sürümleri henüz desteklememektedir. Ancak, mevcut dağıtılan BizTalk API 'Lerini HTTP eylemi aracılığıyla kullanabilirsiniz. Daha fazla bilgi için, [tümleştirme yol haritasında](https://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/)"zaten dağıtılmış API uygulamalarınızı kullanma" konusuna bakın. 
