---
title: Ağustos-1-2015 önizleme için Şema güncellemeleri
description: Azure Logic Apps'ta mantık uygulama tanımları için güncelleştirilmiş şema sürümü 2015-08-01-önizleme
services: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 05/31/2016
ms.openlocfilehash: b6746baaede777eb8c2afcae9eb3fe80b669c468
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792850"
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Azure Logic Apps için Şema güncellemeleri - 1 Ağustos 2015 önizleme

Azure Logic Apps'ın bu şema ve API sürümü, mantık uygulamalarını daha güvenilir ve kullanımı daha kolay hale getiren önemli geliştirmeleri içerir:

* **APIApp** eylem türü artık [**APIConnection**](#api-connections)olarak adlandırılır.
* **Yineleme** eyleminin adı artık [**Foreach'**](#foreach)dir.
* [ **HTTP Dinleyici** API Uygulaması](#http-listener) artık gerekli değildir.
* Alt iş akışlarını arama yeni bir [şema](#child-workflows)kullanır.

<a name="api-connections"></a>

## <a name="move-to-api-connections"></a>API bağlantılarına geçme

En büyük değişiklik, API'leri kullanabilmeniz için Artık API Uygulamalarını Azure aboneliğinize dağıtmak zorunda kalmamaktır. API'leri kullanma yolları şunlardır:

* Yönetilen API'ler
* Özel Web API'leriniz

Yönetim ve barındırma modelleri farklı olduğundan her yol biraz farklı işlenir. Bu modelin bir avantajı, artık Azure kaynak grubunuzda dağıtılan kaynaklarla sınırlı kalmamış olmandır. 

### <a name="managed-apis"></a>Yönetilen API'ler

Microsoft, Sizin adınıza Office 365, Salesforce, Twitter ve FTP gibi bazı API'leri yönetir. Bing Translate gibi bazı yönetilen API'leri kullanabilirsiniz, diğerleri ise *bağlantı*olarak da adlandırılan yapılandırma gerektirir.

Örneğin, Office 365'i kullandığınızda, Office 365 oturum açma belirtecinizi içeren bir bağlantı oluşturmanız gerekir. Belirteciniz güvenli bir şekilde saklanır ve yenilenir, böylece mantık uygulamanız her zaman Office 365 API'yi arayabilir. SQL veya FTP sunucunuza bağlanmak istiyorsanız, bağlantı dizesine sahip bir bağlantı oluşturmanız gerekir. 

Bu tanımda, bu `APIConnection`eylemlere . Aşağıda, Office 365'i e-posta göndermeye çağıran bir bağlantı örneği verilmiştir:

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

Nesne, `host` API bağlantılarına özgü girişlerin bir parçasıdır ve şu bölümleri `api` `connection`içerir: ve. Nesne, `api` yönetilen API'nin barındırıldığı çalışma zamanı URL'sini belirtir. Bu yöntemi arayarak tüm kullanılabilir yönetilen API'leri görebilirsiniz:

```text
GET https://management.azure.com/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/locations/<location>/managedApis?api-version=2015-08-01-preview
```

Bir API kullandığınızda, bu API herhangi bir *bağlantı parametreleri*tanımlanmış olabilir veya olmayabilir. Bu nedenle, API bu parametreleri tanımlamazsa, bağlantı gerekmez. API bu parametreleri tanımlıyorsa, belirtilen bir ada bağlantı oluşturmanız gerekir.  
Daha sonra nesnenin `connection` içindeki `host` nesneye bu adı başvurursunuz. Kaynak grubunda bağlantı oluşturmak için şu yöntemi çağırın:

```text
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/<name>?api-version=2015-08-01-preview
```

Aşağıdaki gövde ile:

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

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Yönetilen API'leri Azure Kaynak Yöneticisi şablonunda dağıtma

Etkileşimli oturum açma gerekli olmadığında, Kaynak Yöneticisi şablonu kullanarak tam bir uygulama oluşturabilirsiniz.
Oturum açma gerekirse, kaynak yöneticisi şablonu kullanmaya devam edebilirsiniz, ancak bağlantıları Azure portalı üzerinden yetkilendirmeniz gerekir. 

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

Bu örnekte bağlantıların kaynak grubunuzda yaşayan kaynaklar dan olduğunu görebilirsiniz. Aboneliğinizde kullanabileceğiniz yönetilen API'lere başvururlar.

### <a name="your-custom-web-apis"></a>Özel Web API'leriniz

Microsoft tarafından yönetilen api'ler yerine kendi API'lerinizi kullanıyorsanız, API'lerinizi aramak için yerleşik **HTTP** eylemini kullanın. İdeal olarak, API'niz için bir Swagger bitiş noktası sağlamalısınız. Bu bitiş noktası, Logic App Designer'ın API giriş ve çıktılarını göstermesine yardımcı olur. Bir Swagger bitiş noktası olmadan, tasarımcı yalnızca giriş ve çıkışları opak JSON nesneleri olarak gösterebilir.

Aşağıda, yeni `metadata.apiDefinitionUrl` özelliği gösteren bir örnek verilmiştir:

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

Web API'nizi Azure Uygulama Hizmeti'nde barındırıyorsanız, Web API'niz tasarımcıda bulunan eylemler listesinde otomatik olarak görünür. Değilse, URL'ye doğrudan yapıştırmanız gerekir. Swagger bitiş noktası, Swagger'ın desteklediği yöntemlerle API'nin kendisini güvence altına alabildiğiniz halde, Mantık Uygulama Tasarımcısı'nda kullanılabilir olması için kimlik doğrulaması yapılmalıdır.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>2015-08-01 önizlemeile dağıtılan API uygulamalarını arayın

Daha önce bir API Uygulaması dağıttıysanız, bu uygulamayı **HTTP** eylemiyle arayabilirsiniz.
Örneğin, dosyaları listelemek için Dropbox kullanıyorsanız, **2014-12-01 önizleme** şema sürüm tanımınızda aşağıdaki gibi bir şey olabilir:

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

Şimdi, benzer bir HTTP eylemi oluşturabilir ve mantık uygulaması `parameters` tanımının bölümünü değiştirmeden bırakabilirsiniz, örneğin:

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

Bu özellikleri tek tek yürümek:

| Eylem özelliği | Açıklama |
| --- | --- |
| `type` | `Http`Onun yerine`APIapp` |
| `metadata.apiDefinitionUrl` | Mantık App Designer bu eylemi kullanmak için, aşağıdakilerden oluşan meta veri bitiş noktası, içerir:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Yapılışı:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Her zaman`POST` |
| `inputs.body` | API Uygulama parametreleri ile aynı |
| `inputs.authentication` | API Uygulaması kimlik doğrulaması ile aynı |

Bu yaklaşım, tüm API App eylemleri için çalışması gerekir. Ancak, bu önceki API Uygulamalarının artık desteklenmediğini unutmayın. Bu nedenle, diğer iki önceki seçenekten birine, yönetilen bir API'ye veya özel Web API'nizi barındırmaya taşımalısınız.

<a name="foreach"></a>

## <a name="renamed-repeat-to-foreach"></a>'Tekrar' olarak 'foreach' olarak yeniden adlandırıldı

Önceki şema sürümü için, **Yinelenen** eylem adının kafa karıştırıcı olduğuna dair çok fazla müşteri geri bildirimi aldık ve **Yineleme'nin** gerçekten her bir döngü için olduğunu düzgün bir şekilde yakalayamadık. Biz de adını `repeat` `foreach`. Daha önce bu eylemi şu örnek gibi yazardın:

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

Şimdi bunun yerine bu sürümü yazmak istiyorum:

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

Ayrıca, `repeatItem()` döngü geçerli yineleme sırasında işlediği öğeye başvuran işlev şimdi yeniden adlandırılır. `item()` 

### <a name="reference-outputs-from-foreach"></a>'Foreach' den referans çıktıları

Basitleştirme için, eylemlerden `foreach` çıkan çıktılar artık adı verilen `repeatItems`bir nesneye sarılmaz. Ayrıca, bu `repeatItem()`değişikliklerle, `repeatBody()`, `repeatOutputs()` , ve işlevler kaldırılır.

Yani, önceki `repeat` örneği kullanarak, bu çıktıları elde elabilirsiniz:

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

Şimdi bunun yerine bu çıktıları almak:

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

Daha önce, bu `body` çıktıları başvururken eylem almak için:

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

Şimdi bunun yerine bu sürümü kullanabilirsiniz:

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

## <a name="native-http-listener"></a>Yerli HTTP dinleyici

HTTP dinleyici özellikleri artık yerleşiktir, böylece bir HTTP Dinleyici API Uygulaması dağıtmak zorunda kalmamanız gerekir. Daha fazla bilgi için, [mantık uygulama bitiş noktası çağrılabilir yapmak](../logic-apps/logic-apps-http-endpoint.md)öğrenin. 

Bu değişikliklerle, Logic Apps `@accessKeys()` `@listCallbackURL()` işlevi gerektiğinde bitiş noktasını alan işlek ile değiştirir. Ayrıca, artık mantık uygulamanızda en az bir tetikleyici tanımlamanız gerekir. İş akışı `/run` istiyorsanız, şu tetikleyici türlerden birini kullanmanız `Manual`gerekir: , `ApiConnectionWebhook`, veya`HttpWebhook`

<a name="child-workflows"></a>

## <a name="call-child-workflows"></a>Alt iş akışlarını arama

Daha önce, alt iş akışlarını arama nız için iş akışına gitmek, erişim belirteci almak ve bu alt iş akışını aramak istediğiniz mantık uygulaması tanımında belirteci yapıştırmak gerekir. Bu şema ile, Logic Apps altyapısı otomatik olarak çocuk iş akışı için çalışma zamanında bir SAS oluşturur, böylece herhangi bir sırrı tanıma yapıştırmak zorunda kalmamanız gerekir. Örnek aşağıda verilmiştir:

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

Ayrıca, alt iş akışları gelen isteğe tam erişim elde. Yani, `queries` bölüm ve nesne parametreleri `headers` geçirebilirsiniz. Bölümün tamamını `body` da tam olarak tanımlayabilirsiniz.

Son olarak, alt iş akışları bu gerekli değişiklikler var. Daha önce ve doğrudan bir alt iş akışı arayabilirsiniz, ancak şimdi üst arama için iş akışında bir tetikleyici bitiş noktası tanımlamanız gerekir. Genellikle, türü olan `Manual` bir tetikleyici ekler ve sonra üst tanımda bu tetikleyici yi kullanırsınız. Özellik `host` özellikle bir `triggerName` çünkü her zaman aradığınız tetikleyici belirtmeniz gerekir.

## <a name="other-changes"></a>Diğer değişiklikler

### <a name="new-queries-property"></a>Yeni 'sorgular' özelliği

Tüm eylem türleri artık yeni `queries`bir girişi destekler. Bu giriş, dizeyi elle birleştirmek yerine yapılandırılmış bir nesne olabilir.

### <a name="renamed-parse-function-to-json"></a>'Ayrıştır()' işlevi 'json()' olarak yeniden adlandırıldı

İşlev `parse()` şimdi gelecekteki `json()` içerik türleri için işlev olarak yeniden adlandırılır.

## <a name="enterprise-integration-apis"></a>Kurumsal Tümleştirme API'leri

Bu şema, AS2 gibi Kurumsal Tümleştirme API'leri için yönetilen sürümleri henüz desteklemez. Ancak, http eylem aracılığıyla varolan dağıtılmış BizTalk API'leri kullanabilirsiniz. Daha fazla bilgi [için, tümleştirme yol haritasında](https://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/)"Zaten dağıtılmış OLAN API uygulamalarınızı kullanma" bilgisine bakın. 
