---
title: ML kaynaklarını yönetmek için REST'i kullanın
titleSuffix: Azure Machine Learning
description: Azure ML kaynaklarını oluşturmak, çalıştırmak ve silmek için REST API'leri nasıl kullanılır?
author: lobrien
ms.author: laobri
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 01/31/2020
ms.openlocfilehash: 419dbd998abc5cbd2da64a990e13d46f3fb2efbe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77580637"
---
# <a name="create-run-and-delete-azure-ml-resources-using-rest"></a>REST'i kullanarak Azure ML kaynaklarını oluşturma, çalıştırma ve silme

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure ML kaynaklarınızı yönetmenin birkaç yolu vardır. [Portal,](https://portal.azure.com/) [komut satırı arabirimini](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)veya [Python SDK'yı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)kullanabilirsiniz. Veya REST API'sini seçebilirsiniz. REST API kaynakları oluşturmak, almak, güncelleştirmek ve silmek için http fiillerini standart bir şekilde kullanır. REST API, HTTP isteklerini gerçekleştirebilecek herhangi bir dil veya araçla çalışır. REST'in basit yapısı genellikle komut dosyası ortamlarında ve MLOps otomasyonu için iyi bir seçim dir. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Yetkilendirme belirteci alma
> * Hizmet temel kimlik doğrulamasını kullanarak düzgün biçimlendirilmiş bir REST isteği oluşturma
> * Azure ML'nin hiyerarşik kaynakları hakkında bilgi almak için GET isteklerini kullanma
> * Kaynakları oluşturmak ve değiştirmek için PUT ve POST isteklerini kullanma
> * Kaynakları temizlemek için DELETE isteklerini kullanma 
> * Dağıtılan modelleri puanlamak için anahtar tabanlı yetkilendirmeyi kullanma

## <a name="prerequisites"></a>Ön koşullar

- Yönetim haklarına sahip olduğunuz bir **Azure aboneliği.** Böyle bir aboneliğiniz yoksa, ücretsiz [veya ücretli kişisel aboneliği](https://aka.ms/AMLFree) deneyin
- [Azure Makine Öğrenimi Çalışma Alanı](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace)
- İdari REST istekleri hizmet temel kimlik doğrulamasını kullanır. Çalışma alanınızda bir hizmet ilkesi oluşturmak [için Azure Machine Learning kaynakları ve iş akışları için kimlik doğrulaması ayarlama](https://docs.microsoft.com/azure/machine-learning/how-to-setup-authentication#set-up-service-principal-authentication) adımlarını izleyin
- **Kıvırma** programı. **Curl** programı Linux veya herhangi bir UNIX dağıtımı [için Windows Alt Sistemi](https://aka.ms/wslinstall/) mevcuttur. PowerShell'de, **curl** **Invoke-WebRequest** için `curl -d "key=val" -X POST uri` bir `Invoke-WebRequest -Body "key=val" -Method POST -Uri uri`takma addır ve . 

## <a name="retrieve-a-service-principal-authentication-token"></a>Hizmet temel kimlik doğrulama belirteci alma

İdari REST istekleri OAuth2 örtük akışı ile doğrulanır. Bu kimlik doğrulama akışı, aboneliğinizin hizmet sorumlusu tarafından sağlanan bir belirteç kullanır. Bu belirteci almak için şunları yapmanız gerekir:

- Kiracı kimliğiniz (aboneliğinizin ait olduğu kuruluşu tanımlama)
- İstemci kimliğiniz (oluşturulan belirteçle ilişkilendirilecek)
- Müşteri sırrınız (korumanız gereken)

[Azure Machine Learning kaynakları ve iş akışları için kimlik doğrulamasını ayarla'da](https://docs.microsoft.com/azure/machine-learning/how-to-setup-authentication#set-up-service-principal-authentication)açıklanıldığı gibi hizmet müdürünüzün oluşturulmasına verilen yanıttan bu değerlere sahip olmalısınız. Şirket aboneliğinizi kullanıyorsanız, bir hizmet ilkesi oluşturma izniniz olmayabilir. Bu durumda, ücretsiz veya [ücretli kişisel abonelik](https://aka.ms/AMLFree)kullanmalısınız.

Bir belirteci almak için:

1. Bir terminal penceresi açın
1. Komut satırına aşağıdaki kodu girin
1. Kendi değerlerinizi `{your-tenant-id}` `{your-client-id}`, ve `{your-client-secret}`. Bu makale boyunca, kıvırcık köşeli ayraçlarla çevrili dizeleri kendi uygun değerleri ile değiştirmeniz gerekecek değişkenlerdir.
1. Yeni bir “kurtarma VM’si” oluşturmak ve sorunlu VM’nin işletim sistemi diskini kurtarma VM’sine veri diski olarak takmak için

```bash
curl -X POST https://login.microsoftonline.com/{your-tenant-id}/oauth2/token \
-d "grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.azure.com%2F&client_id={your-client-id}&client_secret={your-client-secret}" \
```

Yanıt, bir saat için iyi bir erişim belirteci sağlamalıdır:

```json
{
    "token_type": "Bearer",
    "expires_in": "3599",
    "ext_expires_in": "3599",
    "expires_on": "1578523094",
    "not_before": "1578519194",
    "resource": "https://management.azure.com/",
    "access_token": "your-access-token"
}
```

Sonraki tüm yönetim isteklerini doğrulamak için kullanacağınız için belirteci not edin. Bunu, tüm isteklerde bir Yetkilendirme üstbilgisi ayarlayarak yaparsınız:

```bash
curl -h "Authentication: Bearer {your-access-token}" ...more args...
```

Belirteci eklemeden önce tek bir boşluk da dahil olmak üzere değerin "Taşıyıcı" dizesiyle başladığını unutmayın.

## <a name="get-a-list-of-resource-groups-associated-with-your-subscription"></a>Aboneliğinizle ilişkili kaynak gruplarının listesini alma

Aboneliğinizle ilişkili kaynak gruplarının listesini almak için aşağıdakileri çalıştırın:

```bash
curl https://management.azure.com/subscriptions/{your-subscription-id}/resourceGroups?api-version=2019-11-01 -H "Authorization:Bearer {your-access-token}"
```

Azure genelinde birçok REST API'si yayımlanır. Her hizmet sağlayıcısı API'lerini kendi programlarına göre güncelleştirir, ancak bunu varolan programları bozmadan yapar. Hizmet sağlayıcısı uyumluluğu sağlamak için bağımsız değişkeni `api-version` kullanır. Bağımsız `api-version` değişken hizmetten hizmete değişir. Örneğin Makine Öğrenimi Hizmeti için geçerli API `2019-11-01`sürümü. Depolama hesapları `2019-06-01`için, bu . Anahtar kasalar `2019-09-01`için, bu. Tüm REST çağrıları `api-version` bağımsız değişkeni beklenen değere ayarlamalıdır. API gelişmeye devam ederken bile belirtilen sürümün sözdizimine ve anlamtına güvenebilirsiniz. `api-version` Bağımsız değişken olmadan bir sağlayıcıya istek gönderirseniz, yanıt, desteklenen değerlerin insan tarafından okunabilir bir listesini içerir. 

Yukarıdaki çağrı, formun sıkıştırılmış bir JSON yanıtıile sonuçlanacaktır: 

```json
{
    "value": [
        {
            "id": "/subscriptions/12345abc-abbc-1b2b-1234-57ab575a5a5a/resourceGroups/RG1",
            "name": "RG1",
            "type": "Microsoft.Resources/resourceGroups",
            "location": "westus2",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/12345abc-abbc-1b2b-1234-57ab575a5a5a/resourceGroups/RG2",
            "name": "RG2",
            "type": "Microsoft.Resources/resourceGroups",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```


## <a name="drill-down-into-workspaces-and-their-resources"></a>Çalışma alanlarını ve kaynaklarını ayrıntılı olarak araştırın

Bir kaynak grubundaki çalışma alanları kümesini almak için aşağıdakileri `{your-subscription-id}` `{your-resource-group}`çalıştırın, yerine , ve: `{your-access-token}` 

```
curl https://management.azure.com/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/providers/Microsoft.MachineLearningServices/workspaces/?api-version=2019-11-01 \
-H "Authorization:Bearer {your-access-token}"
```

Yine bir JSON listesi alırsınız, bu kez bir liste içeren, her öğe ayrıntıları bir çalışma alanı:

```json
{
    "id": "/subscriptions/12345abc-abbc-1b2b-1234-57ab575a5a5a/resourceGroups/DeepLearningResourceGroup/providers/Microsoft.MachineLearningServices/workspaces/my-workspace",
    "name": "my-workspace",
    "type": "Microsoft.MachineLearningServices/workspaces",
    "location": "centralus",
    "tags": {},
    "etag": null,
    "properties": {
        "friendlyName": "",
        "description": "",
        "creationTime": "2020-01-03T19:56:09.7588299+00:00",
        "storageAccount": "/subscriptions/12345abc-abbc-1b2b-1234-57ab575a5a5a/resourcegroups/DeepLearningResourceGroup/providers/microsoft.storage/storageaccounts/myworkspace0275623111",
        "containerRegistry": null,
        "keyVault": "/subscriptions/12345abc-abbc-1b2b-1234-57ab575a5a5a/resourcegroups/DeepLearningResourceGroup/providers/microsoft.keyvault/vaults/myworkspace2525649324",
        "applicationInsights": "/subscriptions/12345abc-abbc-1b2b-1234-57ab575a5a5a/resourcegroups/DeepLearningResourceGroup/providers/microsoft.insights/components/myworkspace2053523719",
        "hbiWorkspace": false,
        "workspaceId": "cba12345-abab-abab-abab-ababab123456",
        "subscriptionState": null,
        "subscriptionStatusChangeTimeStampUtc": null,
        "discoveryUrl": "https://centralus.experiments.azureml.net/discovery"
    },
    "identity": {
        "type": "SystemAssigned",
        "principalId": "abcdef1-abab-1234-1234-abababab123456",
        "tenantId": "1fedcba-abab-1234-1234-abababab123456"
    },
    "sku": {
        "name": "Basic",
        "tier": "Basic"
    }
}
```

Çalışma alanı içindeki kaynaklarla çalışmak için, genel **management.azure.com** sunucusundan çalışma alanının konumuna özgü bir REST API sunucusuna geçersiniz. Yukarıdaki JSON `discoveryUrl` yanıtında anahtarın değerini not edin. Bu URL'yi alırsanız, şu gibi bir yanıt alırsınız:

```json
{
  "api": "https://centralus.api.azureml.ms",
  "catalog": "https://catalog.cortanaanalytics.com",
  "experimentation": "https://centralus.experiments.azureml.net",
  "gallery": "https://gallery.cortanaintelligence.com/project",
  "history": "https://centralus.experiments.azureml.net",
  "hyperdrive": "https://centralus.experiments.azureml.net",
  "labeling": "https://centralus.experiments.azureml.net",
  "modelmanagement": "https://centralus.modelmanagement.azureml.net",
  "pipelines": "https://centralus.aether.ms",
  "studiocoreservices": "https://centralus.studioservice.azureml.com"
}
```

Yanıtın `api` değeri, ek istekler için kullanacağınız sunucunun URL'sidir. Denemeleri listelemek için, örneğin, aşağıdaki komutu gönderin. Yanıtın değeriyle değiştirin `regional-api-server` (örneğin, `centralus.api.azureml.ms`). `api` Ayrıca `your-subscription-id`değiştirin `your-workspace-name`, `your-access-token` `your-resource-group`, , ve her zamanki gibi:

```bash
curl https://{regional-api-server}/history/v1.0/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/\
providers/Microsoft.MachineLearningServices/workspaces/{your-workspace-name}/experiments?api-version=2019-11-01 \
-H "Authorization:Bearer {your-access-token}"
```

Benzer şekilde, çalışma alanınızda kayıtlı modelleri almak için şunları gönderin:

```bash
curl https://{regional-api-server}/modelmanagement/v1.0/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/\
providers/Microsoft.MachineLearningServices/workspaces/{your-workspace-name}/models?api-version=2019-11-01 \
-H "Authorization:Bearer {your-access-token}"
```

Yol denemeleri listelemek için `history/v1.0` modelleri listelemek için sırada, yol ile `modelmanagement/v1.0`başlar. REST API' si, her biri ayrı bir yolu olan çeşitli operasyon gruplarına ayrılmıştır. Aşağıdaki bağlantılardaki API Başvuru dokümanları çeşitli işlemlerin işlemlerini, parametrelerini ve yanıt kodlarını listeler.

|Alan|Yol|Başvuru|
|-|-|-|
|Yapıtlar|artefakt/v2.0/|[REST API Başvurusu](https://docs.microsoft.com/rest/api/azureml/artifacts)|
|Veri depoları|datastore/v1.0/|[REST API Başvurusu](https://docs.microsoft.com/rest/api/azureml/datastores)|
|Hiper parametre ayarı|hipersürücü/v1.0/|[REST API Başvurusu](https://docs.microsoft.com/rest/api/azureml/hyperparametertuning)|
|Modeller|modelmanagement/v1.0/|[REST API Başvurusu](https://docs.microsoft.com/rest/api/azureml/modelsanddeployments/mlmodels)|
|Çalıştırma geçmişi|yürütme/v1.0/ ve tarih/v1.0/|[REST API Başvurusu](https://docs.microsoft.com/rest/api/azureml/runs)|

Rest API'yi genel deseni kullanarak keşfedebilirsiniz:

|URL bileşeni|Örnek|
|-|-|
| https://| |
| bölgesel-api-sunucu/ | centralus.api.azureml.ms/ |
| operasyon-yol/ | tarihçesi/v1.0/ |
| abonelikler/{aboneliğiniz-id}/ | abonelikler/abcde123-abab-abab-1234-0123456789abc/ |
| resourceGroups/{your-resource-group}/ | resourceGroups/MyResourceGroup/ |
| sağlayıcılar/operasyon sağlayıcılar/ | sağlayıcılar/Microsoft.MachineLearningServices/ |
| sağlayıcı-kaynak yolu/ | çalışma alanları/MLWorkspace/MyWorkspace/FirstExperiment/runs/1/ |
| işlemler-bitiş noktası/ | yapılar/meta veriler/ |


## <a name="create-and-modify-resources-using-put-and-post-requests"></a>PUT ve POST isteklerini kullanarak kaynak oluşturma ve değiştirme

GET fiili ile kaynak alma ek olarak, REST API, ML çözümlerini eğitmek, dağıtmak ve izlemek için gerekli tüm kaynakların oluşturulmasını destekler. 

Eğitim ve çalışan ML modelleri hesaplama kaynakları gerektirir. Bir çalışma alanının işlem kaynaklarını aşağıdakilerle listeleyebilirsiniz: 

```bash
curl https://management.azure.com/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/\
providers/Microsoft.MachineLearningServices/workspaces/{your-workspace-name}/computes?api-version=2019-11-01 \
-H "Authorization:Bearer {your-access-token}"
```

Adlandırılmış bir bilgi işlem kaynağı oluşturmak veya üzerine yazmak için PUT isteği kullanırsınız. Aşağıdaki, şimdi tanıdık ikameek , `your-subscription-id`, `your-resource-group` `your-workspace-name`, ve `your-access-token`, yerine `your-compute-name`, ve `location` `vmSize`değerleri `vmPriority` `scaleSettings`, `adminUserName`, `adminUserPassword`, , , ve . [Machine Learning Compute - Create Or Update SDK Reference](https://docs.microsoft.com/rest/api/azureml/workspacesandcomputes/machinelearningcompute/createorupdate)adresindeki başvuruda belirtildiği gibi, aşağıdaki komut 30 dakika sonra küçültülecek özel, tek düğümlü Standard_D1 (temel bir CPU bilgi işlem kaynağı) oluşturur:

```bash
curl -X PUT \
  'https://management.azure.com/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/providers/Microsoft.MachineLearningServices/workspaces/{your-workspace-name}/computes/{your-compute-name}?api-version=2019-11-01' \
  -H 'Authorization:Bearer {your-access-token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "location": "{your-azure-location}",
    "properties": {
        "computeType": "AmlCompute",
        "properties": {
            "vmSize": "Standard_D1",
            "vmPriority": "Dedicated",
            "scaleSettings": {
                "maxNodeCount": 1,
                "minNodeCount": 0,
                "nodeIdleTimeBeforeScaleDown": "PT30M"
            }
        }
    },
    "userAccountCredentials": {
        "adminUserName": "{adminUserName}",
        "adminUserPassword": "{adminUserPassword}"
    }
}'
```

> [!Note]
> Windows terminallerinde JSON verileri gönderirken çift tırnak sembollerinden kaçmanız gerekebilir. Yani, metin gibi `"location"` `\"location\"`olur . 

Başarılı bir istek `201 Created` yanıt alır, ancak bu yanıtın yalnızca sağlama işleminin başladığı anlamına geldiğini unutmayın. Başarılı bir şekilde tamamlanacağını onaylamak için anketi (veya portalı kullanmanız) gerekir.

### <a name="create-an-experimental-run"></a>Deneysel bir çalışma oluşturma

Deneme içinde bir çalıştırma başlatmak için, eğitim komut dosyası ve ilgili dosyaları içeren bir zip klasörü ve bir çalışma tanımı JSON dosyası gerekir. Zip klasöründe Python giriş dosyası kök dizininde olmalıdır. Örnek olarak, aşağıdaki gibi önemsiz bir Python programını **train.zip**adlı bir klasöre sığdırın.

```python
# hello.py
# Entry file for run
print("Hello, REST!")
```

Definition.json olarak bu sonraki snippet **kaydedin.** "Komut Dosyası" değerinin az önce fermuarını kırpdığınız Python dosyasının adıyla eşleştiğini doğrulayın. "Hedef" değerinin kullanılabilir bir bilgi işlem kaynağının adı ile eşleştiğini onaylayın. 

```json
{
    "Configuration":{  
       "Script":"hello.py",
       "Arguments":[  
          "234"
       ],
       "SourceDirectoryDataStore":null,
       "Framework":"Python",
       "Communicator":"None",
       "Target":"cpu-compute",
       "MaxRunDurationSeconds":1200,
       "NodeCount":1,
       "Environment":{  
          "Python":{  
             "InterpreterPath":"python",
             "UserManagedDependencies":false,
             "CondaDependencies":{  
                "name":"project_environment",
                "dependencies":[  
                   "python=3.6.2",
                   {  
                      "pip":[  
                         "azureml-defaults"
                      ]
                   }
                ]
             }
          },
          "Docker":{  
             "BaseImage":"mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04"
          }
      },
       "History":{  
          "OutputCollection":true
       }
    }
}
```

İçeriği kullanarak `multipart/form-data` bu dosyaları sunucuya gönderin:

```bash
curl https://{regional-api-server}/execution/v1.0/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/providers/Microsoft.MachineLearningServices/workspaces/{your-workspace-name}/experiments/{your-experiment-name}/startrun?api-version=2019-11-01 \
  -X POST \
  -H "Content-Type: multipart/form-data" \
  -H "Authorization:Bearer {your-access-token}" \
  -F projectZipFile=@train.zip \
  -F runDefinitionFile=@runDefinition.json
```

Başarılı bir POST isteği, oluşturulan çalıştırmanın tanımlayıcısını içeren bir yanıt gövdesi yle bir `200 OK` durum oluşturur:

```json
{
  "runId": "my-first-experiment_1579642222877"
}
```

Artık tanıdık olması gereken REST-ful deseni kullanarak bir çalıştırmayı izleyebilirsiniz:

```bash
curl 'https://{regional-api-server}/history/v1.0/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/providers/Microsoft.MachineLearningServices/workspaces/{your-workspace-name}/experiments/{your-experiment-names}/runs/{your-run-id}?api-version=2019-11-01' \
  -H 'Authorization:Bearer {your-access-token}'
```

### <a name="delete-resources-you-no-longer-need"></a>Artık ihtiyacınız olmayan kaynakları silme

Tümü olmasa da bazıları DELETE fiilini destekler. Silme kullanım örnekleri için REST API'ye bağlanmadan önce [API Başvurusu'nu](https://docs.microsoft.com/rest/api/azureml/) kontrol edin. Örneğin, bir modeli silmek için şunları kullanabilirsiniz:

```bash
curl
  -X DELETE \
'https://{regional-api-server}/modelmanagement/v1.0/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/providers/Microsoft.MachineLearningServices/workspaces/{your-workspace-name}/models/{your-model-id}?api-version=2019-11-01' \
  -H 'Authorization:Bearer {your-access-token}' 
```

## <a name="use-rest-to-score-a-deployed-model"></a>Dağıtılan bir modeli puanlamak için REST'i kullanın

Bir modeli bir hizmet sorumlusuyla kimlik doğrulaması yapacak şekilde dağıtmak mümkün olsa da, istemciye bakan dağıtımların çoğu anahtar tabanlı kimlik doğrulamasını kullanır. Dağıtımınızın sayfasında, Studio'nun **Uç Noktaları** sekmesinde uygun anahtarı bulabilirsiniz. Aynı konum, bitiş noktanızın puanlama URI'sini gösterir. Modelinizin girişleri json dizisi olarak `data`modellenmelidir:

```bash
curl 'https://{scoring-uri}' \
 -H 'Authorization:Bearer {your-key}' \
 -H 'Content-Type: application/json' \
  -d '{ "data" : [ {model-specific-data-structure} ] }
```

## <a name="create-a-workspace-using-rest"></a>REST'i kullanarak çalışma alanı oluşturma 

Her Azure ML çalışma alanının diğer dört Azure kaynağına bağımlılığı vardır: yönetim etkinleştirilmiş bir kapsayıcı kayıt defteri, önemli bir kasa, Bir Application Insights kaynağı ve bir depolama hesabı. Bu kaynaklar var olana kadar bir çalışma alanı oluşturamazsınız. Bu tür her bir kaynak oluşturma ayrıntıları için REST API başvurusuna başvurun.

Bir çalışma alanı oluşturmak için `management.azure.com`aşağıdakine benzer bir çağrı KOY. Bu çağrı çok sayıda değişken ayarlamanızı gerektirse de, yapısal olarak bu makalenin tartıştığı diğer çağrılarla aynıdır. 

```bash
curl -X PUT \
  'https://management.azure.com/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}\
/providers/Microsoft.MachineLearningServices/workspaces/{your-new-workspace-name}?api-version=2019-11-01' \
  -H 'Authorization: Bearer {your-access-token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "location": "{desired-region}",
    "properties": {
        "friendlyName" : "{your-workspace-friendly-name}",
        "description" : "{your-workspace-description}",
        "containerRegistry" : "/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/\
providers/Microsoft.ContainerRegistry/registries/{your-registry-name}",
        keyVault" : "/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}\
/providers/Microsoft.Keyvault/vaults/{your-keyvault-name}",
        "applicationInsights" : "subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/\
providers/Microsoft.insights/components/{your-application-insights-name}",
        "storageAccount" : "/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/\
providers/Microsoft.Storage/storageAccounts/{your-storage-account-name}"
    },
    "identity" : {
        "type" : "systemAssigned"
    }
}'
```

Bir `202 Accepted` yanıt ve döndürülen üstbilgide bir `Location` URI almalısınız. Bağımlı kaynaklarınızdan biriyle ilgili bir sorun varsa (örneğin, kapsayıcı kayıt defterinizde yönetici erişimini etkinleştirmeyi unuttuysanız) yararlı hata ayıklama bilgileri de dahil olmak üzere dağıtım hakkında bilgi almak için bu URI'yi alabilirsiniz. 

## <a name="troubleshooting"></a>Sorun giderme

### <a name="resource-provider-errors"></a>Kaynak sağlayıcı hataları

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="moving-the-workspace"></a>Çalışma alanını taşıma

> [!WARNING]
> Azure Machine Learning çalışma alanınızı farklı bir aboneye taşımak veya sahip olan aboneliğiyeni bir kiracıya taşımak desteklenmez. Bunu yapmak hatalara neden olabilir.

### <a name="deleting-the-azure-container-registry"></a>Azure Kapsayıcı Kayıt Defterini Silme

Azure Machine Learning çalışma alanı, bazı işlemler için Azure Kapsayıcı Kayıt Defteri'ni (ACR) kullanır. İlk ihtiyacı olduğunda otomatik olarak bir ACR örneği oluşturur.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [AzureML REST API referansını](https://docs.microsoft.com/rest/api/azureml/)keşfedin.
- [Tasarımcı (önizleme) ile otomobil fiyatını tahmin](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-train-score)etmek için Studio & Designer nasıl kullanılacağını öğrenin.
- [Jupyter dizüstü bilgisayarlarla Azure Machine Learning'i](https://docs.microsoft.com/azure//machine-learning/samples-notebooks)keşfedin.
