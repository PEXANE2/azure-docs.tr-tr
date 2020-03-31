---
title: Şablon işlevleri - kaynaklar
description: Kaynaklarla ilgili değerleri almak için Azure Kaynak Yöneticisi şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: e9e1d700282652304f0bede5e697ba8625f5a5d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156302"
---
# <a name="resource-functions-for-arm-templates"></a>ARM şablonları için kaynak işlevleri

Kaynak Yöneticisi, Azure Kaynak Yöneticisi (ARM) şablonunuzda kaynak değerleri almak için aşağıdaki işlevleri sağlar:

* [uzantısıResourceId](#extensionresourceid)
* [liste*](#list)
* [Sağlayıcı](#providers)
* [Başvuru](#reference)
* [resourceGroup](#resourcegroup)
* [Resourceıd](#resourceid)
* [Abonelik](#subscription)
* [abonelikResourceId](#subscriptionresourceid)
* [tenantResourceId](#tenantresourceid)

Parametrelerden, değişkenlerden veya geçerli dağıtımdan değer elde etmek için [dağıtım değeri işlevlerine](template-functions-deployment.md)bakın.

## <a name="extensionresourceid"></a>uzantısıResourceId

```json
extensionResourceId(resourceId, resourceType, resourceName1, [resourceName2], ...)
```

Yeteneklerine eklemek için başka bir kaynağa uygulanan bir kaynak türü olan [uzantı kaynağının](../management/extension-resource-types.md)kaynak kimliğini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceId |Evet |string |Uzantı kaynağının uygulandığı kaynak için kaynak kimliği. |
| resourceType |Evet |string |Kaynak sağlayıcı ad alanı da dahil olmak üzere kaynak türü. |
| resourceName1 |Evet |string |Kaynağın adı. |
| resourceName2 |Hayır |string |Gerekirse sonraki kaynak adı kesimi. |

Kaynak türü daha fazla kesim içeriyorsa, kaynak adlarını parametre olarak eklemeye devam edin.

### <a name="return-value"></a>Döndürülen değer

Bu işlevle döndürülen kaynak kimliğinin temel biçimi:

```json
{scope}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

Kapsam kesimi, genişletilen kaynağa göre değişir.

Uzantı kaynağı bir **kaynağa**uygulandığında, kaynak kimliği aşağıdaki biçimde döndürülür:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{baseResourceProviderNamespace}/{baseResourceType}/{baseResourceName}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

Uzantı kaynağı bir **kaynak grubuna**uygulandığında, biçim:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

Uzantı kaynağı bir **aboneye**uygulandığında, biçim aşağıdakigibi dir:

```json
/subscriptions/{subscriptionId}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

Uzantı kaynağı bir **yönetim grubuna**uygulandığında, biçim:

```json
/providers/Microsoft.Management/managementGroups/{managementGroupName}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

### <a name="extensionresourceid-example"></a>uzantısıResourceId örneği

Aşağıdaki örnek, kaynak grubu kilidi için kaynak kimliğini döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "lockName":{
            "type": "string"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "lockResourceId": {
            "type": "string",
            "value": "[extensionResourceId(resourceGroup().Id , 'Microsoft.Authorization/locks', parameters('lockName'))]"
        }
    }
}
```

<a id="listkeys" />
<a id="list" />

## <a name="list"></a>liste*

```json
list{Value}(resourceName or resourceIdentifier, apiVersion, functionValues)
```

Bu işlevin sözdizimi liste işlemlerinin adına göre değişir. Her uygulama, bir liste çalışmasını destekleyen kaynak türü için değerleri döndürür. İşlem adı `list`ile başlanmalıdır. Bazı yaygın kullanımlar `listSecrets`ve `listKeys` .

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceName veya resourceIdentifier |Evet |string |Kaynak için benzersiz tanımlayıcı. |
| apiVersion |Evet |string |Kaynak çalışma zamanı durumunun API sürümü. Tipik olarak, biçiminde, **yyyy-mm-dd**. |
| fonksiyonDeğerleri |Hayır |object | İşlev için değerleri olan bir nesne. Bu nesneyi yalnızca depolama **hesabındaki listAccountSas** gibi parametre değerlerine sahip bir nesneyi almayı destekleyen işlevler için sağlayın. Bu makalede, geçen işlev değerlerinin bir örneği gösterilmiştir. |

### <a name="valid-uses"></a>Geçerli kullanımlar

Liste işlevleri yalnızca kaynak tanımının özelliklerinde ve şablon veya dağıtımın çıktılar bölümünde kullanılabilir. [Özellik yinelemesi](copy-properties.md)ile kullanıldığında, ifade kaynak `input` özelliğine atandığı için liste işlevlerini kullanabilirsiniz. Liste işlevi çözülmeden `count` önce sayım belirlenmelidir, çünkü bunları kullanamazsınız.

### <a name="implementations"></a>Uygulamalar

Listenin olası kullanımları* aşağıdaki tabloda gösterilmiştir.

| Kaynak türü | İşlev adı |
| ------------- | ------------- |
| Microsoft.AnalysisServices/sunucular | [listeGatewayDurum](/rest/api/analysisservices/servers/listgatewaystatus) |
| Microsoft.AppConfiguration/configurationStores | ListKeys |
| Microsoft.Automation/automationHesapları | [listeTuşları](/rest/api/automation/keys/listbyautomationaccount) |
| Microsoft.Batch/batchHesapları | [listkeys](/rest/api/batchmanagement/batchaccount/getkeys) |
| Microsoft.BatchAI/çalışma alanları/denemeler/işler | [listoutputfiles](/rest/api/batchai/jobs/listoutputfiles) |
| Microsoft.Blockchain/blockchainÜyeler | [listeApiKeys](/rest/api/blockchain/2019-06-01-preview/blockchainmembers/listapikeys) |
| Microsoft.Blockchain/blockchainÜyeler/transactionNodes | [listeApiKeys](/rest/api/blockchain/2019-06-01-preview/transactionnodes/listapikeys) |
| Microsoft.Önbellek/redis | [listeTuşları](/rest/api/redis/redis/listkeys) |
| Microsoft.CognitiveServices/hesapları | [listeTuşları](/rest/api/cognitiveservices/accountmanagement/accounts/listkeys) |
| Microsoft.ContainerRegistry/registries | [listeBuildSourceUploadUrl](/rest/api/containerregistry/registries%20(tasks)/getbuildsourceuploadurl) |
| Microsoft.ContainerRegistry/registries | [listCredentials](/rest/api/containerregistry/registries/listcredentials) |
| Microsoft.ContainerRegistry/registries | [listeKullanımlar](/rest/api/containerregistry/registries/listusages) |
| Microsoft.ContainerRegistry/registries/webhooks | [listeEtkinlikler](/rest/api/containerregistry/webhooks/listevents) |
| Microsoft.ContainerRegistry/registries/runs | [listLogSasUrl](/rest/api/containerregistry/runs/getlogsasurl) |
| Microsoft.ContainerRegistry/registries/tasks | [listeAyrıntılar](/rest/api/containerregistry/tasks/getdetails) |
| Microsoft.ContainerService/managedClusters | [listClusterAdminCredential](/rest/api/aks/managedclusters/listclusteradmincredentials) |
| Microsoft.ContainerService/managedClusters | [listClusterUserCredential](/rest/api/aks/managedclusters/listclusterusercredentials) |
| Microsoft.ContainerService/managedClusters/accessProfiles | [listeCredential](/rest/api/aks/managedclusters/getaccessprofile) |
| Microsoft.DataBox/jobs | listCredentials |
| Microsoft.DataFactory/datafactorys/ağ geçitleri | listauthkeys |
| Microsoft.DataFactory/fabrikalar/integrationruntimes | [listauthkeys](/rest/api/datafactory/integrationruntimes/listauthkeys) |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers | [listeSasTokens](/rest/api/datalakeanalytics/storageaccounts/listsastokens) |
| Microsoft.DataShare/accounts/shares | [listeSenkronizasyonlar](/rest/api/datashare/shares/listsynchronizations) |
| Microsoft.DataShare/accounts/shareSubscriptions | [listSourceShareSynchronizationSettings](/rest/api/datashare/sharesubscriptions/listsourcesharesynchronizationsettings) |
| Microsoft.DataShare/accounts/shareSubscriptions | [listeSenkronizasyonAyrıntılar](/rest/api/datashare/sharesubscriptions/listsynchronizationdetails) |
| Microsoft.DataShare/accounts/shareSubscriptions | [listeSenkronizasyonlar](/rest/api/datashare/sharesubscriptions/listsynchronizations) |
| Microsoft.Devices/iotHubs | [listkeys](/rest/api/iothub/iothubresource/listkeys) |
| Microsoft.Devices/iotHubs/iotHubKeys | [listkeys](/rest/api/iothub/iothubresource/getkeysforkeyname) |
| Microsoft.Devices/provisioningServices/keys | [listkeys](/rest/api/iot-dps/iotdpsresource/listkeysforkeyname) |
| Microsoft.Devices/provisioningServices | [listkeys](/rest/api/iot-dps/iotdpsresource/listkeys) |
| Microsoft.DevTestLab/laboratuvarları | [ListVhds](/rest/api/dtl/labs/listvhds) |
| Microsoft.DevTestLab/labs/zamanlamaları | [ListeUygulanabilir](/rest/api/dtl/schedules/listapplicable) |
| Microsoft.DevTestLab/labs/users/serviceFabrics | [ListeUygulanabilir Programlar](/rest/api/dtl/servicefabrics/listapplicableschedules) |
| Microsoft.DevTestLab/labs/virtualMachines | [ListeUygulanabilir Programlar](/rest/api/dtl/virtualmachines/listapplicableschedules) |
| Microsoft.DocumentDB/databaseHesapları | [listeConnectionStrings](/rest/api/cosmos-db-resource-provider/databaseaccounts/listconnectionstrings) |
| Microsoft.DocumentDB/databaseHesapları | [listeTuşları](/rest/api/cosmos-db-resource-provider/databaseaccounts/listkeys) |
| Microsoft.DomainRegistration | [listeDomainRecommendations](/rest/api/appservice/domains/listrecommendations) |
| Microsoft.DomainRegistration/topLevelDomains | [listSözleşmeler](/rest/api/appservice/topleveldomains/listagreements) |
| Microsoft.EventGrid/etki alanları | [listeTuşları](/rest/api/eventgrid/domains/listsharedaccesskeys) |
| Microsoft.EventGrid/konular | [listeTuşları](/rest/api/eventgrid/topics/listsharedaccesskeys) |
| Microsoft.EventHub/namespaces/authorizationRules | [listkeys](/rest/api/eventhub/namespaces/listkeys) |
| Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys](/rest/api/eventhub/disasterrecoveryconfigs/listkeys) |
| Microsoft.EventHub/namespaces/eventhubs/authorizationRules | [listkeys](/rest/api/eventhub/eventhubs/listkeys) |
| Microsoft.ImportExport/jobs | [listeBitLockerKeys](/rest/api/storageimportexport/bitlockerkeys/list) |
| Microsoft.Kusto/Kümeler/Veritabanları | [Liste İlkeleri](/rest/api/azurerekusto/databases/listprincipals) |
| Microsoft.LabServices/kullanıcılar | [ListEnvironments](/rest/api/labservices/globalusers/listenvironments) |
| Microsoft.LabServices/kullanıcılar | [ListLabs](/rest/api/labservices/globalusers/listlabs) |
| Microsoft.Logic/integrationAccounts/agreements | [listeContentCallbackUrl](/rest/api/logic/agreements/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/assemblies | [listeContentCallbackUrl](/rest/api/logic/integrationaccountassemblies/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listeCallbackUrl](/rest/api/logic/integrationaccounts/getcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listeKeyVaultKeys](/rest/api/logic/integrationaccounts/listkeyvaultkeys) |
| Microsoft.Logic/integrationAccounts/maps | [listeContentCallbackUrl](/rest/api/logic/maps/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/partners | [listeContentCallbackUrl](/rest/api/logic/partners/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/şema | [listeContentCallbackUrl](/rest/api/logic/schemas/listcontentcallbackurl) |
| Microsoft.Logic/iş akışları | [listeCallbackUrl](/rest/api/logic/workflows/listcallbackurl) |
| Microsoft.Logic/iş akışları | [listeSwagger](/rest/api/logic/workflows/listswagger) |
| Microsoft.Logic/iş akışları/çalıştırmalar/eylemler | [listeExpressionTraces](/rest/api/logic/workflowrunactions/listexpressiontraces) |
| Microsoft.Logic/iş akışları/çalıştırmalar/eylemler/yinelemeler | [listeExpressionTraces](/rest/api/logic/workflowrunactionrepetitions/listexpressiontraces) |
| Microsoft.Logic/iş akışları/tetikleyiciler | [listeCallbackUrl](/rest/api/logic/workflowtriggers/listcallbackurl) |
| Microsoft.Logic/iş akışları/sürümleri/tetikleyicileri | [listeCallbackUrl](/rest/api/logic/workflowversions/listcallbackurl) |
| Microsoft.MachineLearning/webServices | [listkeys](/rest/api/machinelearning/webservices/listkeys) |
| Microsoft.MachineLearning/Çalışma Alanları | listworkspacekeys |
| Microsoft.MachineLearningServices/workspaces/computes | [listeTuşları](/rest/api/azureml/workspacesandcomputes/machinelearningcompute/listkeys) |
| Microsoft.MachineLearningServices/workspaces/computes | [listeDüğümler](/rest/api/azureml/workspacesandcomputes/machinelearningcompute/listnodes) |
| Microsoft.MachineLearningServices/çalışma alanları | [listeTuşları](/rest/api/azureml/workspacesandcomputes/workspaces/listkeys) |
| Microsoft.Haritalar/hesapları | [listeTuşları](/rest/api/maps-management/accounts/listkeys) |
| Microsoft.Media/mediaservices/assets | [listeContainerSas](/rest/api/media/assets/listcontainersas) |
| Microsoft.Media/mediaservices/assets | [listStreamingLocators](/rest/api/media/assets/liststreaminglocators) |
| Microsoft.Media/mediaservices/streamingLocators | [listContentKeys](/rest/api/media/streaminglocators/listcontentkeys) |
| Microsoft.Media/mediaservices/streamingLocators | [listeYolları](/rest/api/media/streaminglocators/listpaths) |
| Microsoft.Network/applicationSecurityGroups | listIpConfigurations |
| Microsoft.NotificationHubs/Namespaces/authorizationRules | [listkeys](/rest/api/notificationhubs/namespaces/listkeys) |
| Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules | [listkeys](/rest/api/notificationhubs/notificationhubs/listkeys) |
| Microsoft.OperationalInsights/çalışma alanları | [listeTuşları](/rest/api/loganalytics/workspaces%202015-03-20/listkeys) |
| Microsoft.PolicyInsights/düzeltmeler | [listeDağıtımlar](/rest/api/policy-insights/remediations/listdeploymentsatresourcegroup) |
| Microsoft.Relay/namespaces/authorizationRules | [listkeys](/rest/api/relay/namespaces/listkeys) |
| Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules | listkeys |
| Microsoft.Relay/namespaces/HybridConnections/authorizationRules | [listkeys](/rest/api/relay/hybridconnections/listkeys) |
| Microsoft.Relay/namespaces/WcfRelays/authorizationRules | [listkeys](/rest/api/relay/wcfrelays/listkeys) |
| Microsoft.Search/searchServices | [listeAdminKeys](/rest/api/searchmanagement/adminkeys/get) |
| Microsoft.Search/searchServices | [listeQueryKeys](/rest/api/searchmanagement/querykeys/listbysearchservice) |
| Microsoft.ServiceBus/namespaces/authorizationRules | [listkeys](/rest/api/servicebus/namespaces/listkeys) |
| Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys](/rest/api/servicebus/disasterrecoveryconfigs/listkeys) |
| Microsoft.ServiceBus/namespaces/queues/authorizationRules | [listkeys](/rest/api/servicebus/queues/listkeys) |
| Microsoft.ServiceBus/namespaces/topics/authorizationRules | [listkeys](/rest/api/servicebus/topics/listkeys) |
| Microsoft.SignalRService/SignalR | [listkeys](/rest/api/signalr/signalr/listkeys) |
| Microsoft.Storage/storageHesapları | [listeAccountSas](/rest/api/storagerp/storageaccounts/listaccountsas) |
| Microsoft.Storage/storageHesapları | [listkeys](/rest/api/storagerp/storageaccounts/listkeys) |
| Microsoft.Storage/storageHesapları | [listeServiceSas](/rest/api/storagerp/storageaccounts/listservicesas) |
| Microsoft.StorSimple/yöneticileri/cihazları | [listeFailoverSets](/rest/api/storsimple/devices/listfailoversets) |
| Microsoft.StorSimple/yöneticileri/cihazları | [listeFailoverHedefler](/rest/api/storsimple/devices/listfailovertargets) |
| Microsoft.StorSimple/yöneticileri | [listeActivationKey](/rest/api/storsimple/managers/getactivationkey) |
| Microsoft.StorSimple/yöneticileri | [listePublicEncryptionKey](/rest/api/storsimple/managers/getpublicencryptionkey) |
| Microsoft.Web/connectionAğ Geçit Leri | ListStatus |
| microsoft.web/bağlantılar | listconsentlinks |
| Microsoft.Web/customApis | listeWsdlInterfaces |
| microsoft.web/locations | listwsdlinterfaces |
| microsoft.web/apimanagementaccounts/apis/connections | listconnection tuşları |
| microsoft.web/apimanagementaccounts/apis/connections | listsecrets |
| microsoft.web/sites/backups | [list](/rest/api/appservice/webapps/listbackups) |
| Microsoft.Web/siteler/config | [list](/rest/api/appservice/webapps/listconfigurations) |
| microsoft.web/siteler/fonksiyonlar | [listkeys](/rest/api/appservice/webapps/listfunctionkeys)
| microsoft.web/siteler/fonksiyonlar | [listsecrets](/rest/api/appservice/webapps/listfunctionsecrets) |
| microsoft.web/sites/hybridconnectionnamespaces/relays | [listkeys](/rest/api/appservice/appserviceplans/listhybridconnectionkeys) |
| microsoft.web/siteler | [listsyncfunctiontriggerstatus](/rest/api/appservice/webapps/listsyncfunctiontriggers) |
| microsoft.web/sites/slots/functions | [listsecrets](/rest/api/appservice/webapps/listfunctionsecretsslot) |
| microsoft.web/sites/slots/backups | [list](/rest/api/appservice/webapps/listbackupsslot) |
| Microsoft.Web/siteler/yuvalar/config | [list](/rest/api/appservice/webapps/listconfigurationsslot) |
| microsoft.web/sites/slots/functions | [listsecrets](/rest/api/appservice/webapps/listfunctionsecretsslot) |

Hangi kaynak türlerinin bir liste çalışması olduğunu belirlemek için aşağıdaki seçeneklere sahipsiniz:

* Bir kaynak sağlayıcısının [REST API işlemlerini](/rest/api/) görüntüleyin ve liste işlemlerine bakın. Örneğin, depolama hesapları [listKeys işlemi](/rest/api/storagerp/storageaccounts)var.
* [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) PowerShell cmdlet'i kullanın. Aşağıdaki örnek, depolama hesapları için tüm liste işlemlerini alır:

  ```powershell
  Get-AzProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Yalnızca liste işlemlerini filtrelemek için aşağıdaki Azure CLI komutunu kullanın:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

### <a name="return-value"></a>Döndürülen değer

Döndürülen nesne kullandığınız liste işlevine göre değişir. Örneğin, bir depolama hesabının listKeys aşağıdaki biçimi döndürür:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Diğer liste işlevlerinin farklı dönüş biçimleri vardır. Bir işlevin biçimini görmek için, örnek şablonda gösterildiği gibi çıktılar bölümüne ekleyin.

### <a name="remarks"></a>Açıklamalar

Kaynak adını veya resourceId işlevini kullanarak kaynağı [belirtin.](#resourceid) Başvurulan kaynağı dağıtan aynı şablonda bir liste işlevi kullanırken, kaynak adını kullanın.

Koşullu olarak dağıtılan bir kaynakta bir **liste** işlevi kullanırsanız, kaynak dağıtılsa bile işlev değerlendirilir. **Liste** işlevi var olmayan bir kaynağa başvuruyorsa bir hata alırsınız. İşlevin yalnızca kaynak dağıtılırken değerlendirildiğinden emin olmak için **if** işlevini kullanın. Koşullu olarak dağıtılan bir kaynağa sahip if ve list kullanan örnek şabloniçin [if işlevini](template-functions-logical.md#if) görün.

### <a name="list-example"></a>Liste örneği

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/listkeys.json) çıktılar bölümündeki bir depolama hesabından birincil ve ikincil anahtarların nasıl döndürüleceklerini gösterir. Ayrıca depolama hesabı için bir SAS belirteci döndürür.

SAS belirteci almak için, son kullanma süresi için bir nesne geçirin. Son kullanma süresi gelecekte olmalıdır. Bu örnek, liste işlevlerini nasıl kullandığınızı göstermek için tasarlanmıştır. Genellikle, SAS belirteci bir kaynak değeri yerine bir çıktı değeri olarak döndürün kullanır. Çıktı değerleri dağıtım geçmişinde depolanır ve güvenli değildir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagename": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus"
        },
        "accountSasProperties": {
            "type": "object",
            "defaultValue": {
                "signedServices": "b",
                "signedPermission": "r",
                "signedExpiry": "2018-08-20T11:00:00Z",
                "signedResourceTypes": "s"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[parameters('storagename')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {
                "supportsHttpsTrafficOnly": false,
                "accessTier": "Hot",
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "keys": {
            "type": "object",
            "value": "[listKeys(parameters('storagename'), '2018-02-01')]"
        },
        "accountSAS": {
            "type": "object",
            "value": "[listAccountSas(parameters('storagename'), '2018-02-01', parameters('accountSasProperties'))]"
        }
    }
}
```

## <a name="providers"></a>Sağlayıcı

```json
providers(providerNamespace, [resourceType])
```

Kaynak sağlayıcısı ve desteklenen kaynak türleri hakkında bilgi verir. Kaynak türü sağlamazsanız, işlev kaynak sağlayıcısı için desteklenen tüm türleri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| sağlayıcıNamealanı |Evet |string |Sağlayıcının ad alanı |
| resourceType |Hayır |string |Belirtilen ad alanı içindeki kaynak türü. |

### <a name="return-value"></a>Döndürülen değer

Desteklenen her tür aşağıdaki biçimde döndürülür:

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Döndürülen değerlerin dizi sırası garanti edilmez.

### <a name="providers-example"></a>Sağlayıcılar örneği

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/providers.json) sağlayıcı işlevinin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "providerNamespace": {
            "type": "string"
        },
        "resourceType": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers(parameters('providerNamespace'), parameters('resourceType'))]",
            "type" : "object"
        }
    }
}
```

**Microsoft.Web** kaynak sağlayıcısı ve **siteler** kaynak türü için, önceki örnek aşağıdaki biçimde bir nesne döndürür:

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

## <a name="reference"></a>reference

```json
reference(resourceName or resourceIdentifier, [apiVersion], ['Full'])
```

Kaynağın çalışma zamanı durumunu temsil eden bir nesne döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceName veya resourceIdentifier |Evet |string |Kaynağın adı veya benzersiz tanımlayıcısı. Geçerli şablonda bir kaynağa başvururken, yalnızca kaynak adını parametre olarak sağlayın. Daha önce dağıtılan bir kaynağa başvururken veya kaynağın adı belirsiz olduğunda kaynak kimliğini sağlayın. |
| apiVersion |Hayır |string |Belirtilen kaynağın API sürümü. Kaynak aynı şablon içinde sağlanmış değilken bu parametreyi ekleyin. Tipik olarak, biçiminde, **yyyy-mm-dd**. Kaynağınız için geçerli API sürümleri için [şablon başvurusuna](/azure/templates/)bakın. |
| 'Tam' |Hayır |string |Tam kaynak nesnesinin döndürülüp döndürülmeyeceğini belirten değer. Belirtmezseniz, `'Full'`yalnızca kaynağın özellikleri nesnesi döndürülür. Tam nesne kaynak kimliği ve konum gibi değerleri içerir. |

### <a name="return-value"></a>Döndürülen değer

Her kaynak türü, başvuru işlevi için farklı özellikler döndürür. İşlev, önceden tanımlanmış tek bir biçimi döndürmez. Ayrıca, döndürülen değer, tam nesneyi belirtip belirtmediğinize bağlı olarak değişir. Kaynak türü özelliklerini görmek için, örnekte gösterildiği gibi çıktılar bölümündeki nesneyi döndürün.

### <a name="remarks"></a>Açıklamalar

Başvuru işlevi, daha önce dağıtılan bir kaynağın veya geçerli şablonda dağıtılan bir kaynağın çalışma zamanı durumunu alır. Bu makalede, her iki senaryo için örnekler gösterir.

Genellikle, blob bitiş noktası URI veya tam nitelikli etki alanı adı gibi bir nesneden belirli bir değer döndürmek için **başvuru** işlevini kullanırsınız.

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))).primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', parameters('ipAddressName'))).dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

Şema özelliklerinin bir parçası olmayan kaynak değerlerine ihtiyacınız olduğunda kullanın. `'Full'` Örneğin, anahtar kasa erişim ilkeleri ayarlamak için sanal bir makinenin kimlik özelliklerini alın.

```json
{
  "type": "Microsoft.KeyVault/vaults",
  "properties": {
    "tenantId": "[subscription().tenantId]",
    "accessPolicies": [
      {
        "tenantId": "[reference(resourceId('Microsoft.Compute/virtualMachines', variables('vmName')), '2019-03-01', 'Full').identity.tenantId]",
        "objectId": "[reference(resourceId('Microsoft.Compute/virtualMachines', variables('vmName')), '2019-03-01', 'Full').identity.principalId]",
        "permissions": {
          "keys": [
            "all"
          ],
          "secrets": [
            "all"
          ]
        }
      }
    ],
    ...
```

### <a name="valid-uses"></a>Geçerli kullanımlar

Başvuru işlevi yalnızca kaynak tanımının özelliklerinde ve şablon veya dağıtımın çıktılar bölümünde kullanılabilir. [Özellik yinelemesi](copy-properties.md)ile kullanıldığında, ifade kaynak `input` özelliğine atandığı için başvuru işlevini kullanabilirsiniz. Başvuru işlevi çözülmeden `count` önce sayım belirlenmelidir, çünkü bunu kullanamazsınız.

İç içe olan şablonda dağıttığınız bir kaynağı döndürmek için [iç içe kullanılan şablonun](linked-templates.md#nested-template) çıktılarında başvuru işlevini kullanamazsınız. Bunun yerine, bağlantılı bir [şablon](linked-templates.md#linked-template)kullanın.

**Başvuru** işlevini koşullu olarak dağıtılan bir kaynakta kullanırsanız, kaynak dağıtılsa bile işlev değerlendirilir.  **Başvuru** işlevi var olmayan bir kaynağa başvuruyorsa bir hata alırsınız. İşlevin yalnızca kaynak dağıtılırken değerlendirildiğinden emin olmak için **if** işlevini kullanın. Koşullu olarak dağıtılan bir kaynakla if ve başvuru kullanan örnek şabloniçin [if işlevini](template-functions-logical.md#if) görün.

### <a name="implicit-dependency"></a>Örtük bağımlılık

Başvuru işlevini kullanarak, başvurulan kaynak aynı şablon içinde sağlanmışsa ve kaynağa adıyla (kaynak kimliğine değil) başvurursanız, bir kaynağın başka bir kaynağa bağlı olduğunu dolaylı olarak beyan emiş olursunuz. Ayrıca bağlı özelliği kullanmanız gerekmez. Başvurulan kaynak dağıtımı tamamlanana kadar işlev değerlendirilmez.

### <a name="resource-name-or-identifier"></a>Kaynak adı veya tanımlayıcı

Aynı şablonda dağıtılan bir kaynağa başvururken, kaynağın adını sağlayın.

```json
"value": "[reference(parameters('storageAccountName'))]"
```

Aynı şablonda dağıtılmadı bir kaynağa başvururken, kaynak kimliğini sağlayın.

```json
"value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]"
```

Hangi kaynağa atıfta bulunduğunuz konusunda belirsizliği önlemek için tam nitelikli bir kaynak tanımlayıcısı sağlayabilirsiniz.

```json
"value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', parameters('ipAddressName')))]"
```

Bir kaynağa tam nitelikli bir başvuru yaparken, tür ve addan segmentleri birleştirme sırası yalnızca ikisinin bir araya getirilmesi değildir. Bunun yerine, ad alanından sonra, en az belirli olandan en spesifik *tür/ad* çiftleri dizisi kullanın:

**{kaynak sağlayıcı-ad alanı}/{üst kaynak-türü}/{üst kaynak-adı}[/{alt kaynak türü}/{alt kaynak-adı}/{alt kaynak adı}]**

Örnek:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`doğru `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` değil doğru

Herhangi bir kaynak kimliğinin oluşturulmasını `resourceId()` kolaylaştırmak için, `concat()` işlev yerine bu belgede açıklanan işlevleri kullanın.

### <a name="get-managed-identity"></a>Yönetilen kimlik elde edin

[Azure kaynakları için yönetilen kimlikler,](../../active-directory/managed-identities-azure-resources/overview.md) bazı kaynaklar için örtülü olarak oluşturulan [uzantı lı kaynak türleridir.](../management/extension-resource-types.md) Yönetilen kimlik şablonda açıkça tanımlanmadığından, kimliğin uygulandığı kaynağa başvurmanız gerekir. Örtülü `Full` olarak oluşturulan kimlik de dahil olmak üzere tüm özellikleri almak için kullanın.

Örneğin, sanal makine ölçeği kümesine uygulanan yönetilen bir kimlik için kiracı kimliğini almak için şunları kullanın:

```json
"tenantId": "[reference(resourceId('Microsoft.Compute/virtualMachineScaleSets',  variables('vmNodeType0Name')), '2019-03-01', 'Full').Identity.tenantId]"
```

### <a name="reference-example"></a>Referans örneği

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) bir kaynak dağıtır ve bu kaynağa başvurur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": {
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      },
      "fullReferenceOutput": {
        "type": "object",
        "value": "[reference(parameters('storageAccountName'), '2016-12-01', 'Full')]"
      }
    }
}
```

Önceki örnek iki nesneyi döndürür. Özellikler nesnesi aşağıdaki biçimdedir:

```json
{
   "creationTime": "2017-10-09T18:55:40.5863736Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

Tam nesne aşağıdaki biçimdedir:

```json
{
  "apiVersion":"2016-12-01",
  "location":"southcentralus",
  "sku": {
    "name":"Standard_LRS",
    "tier":"Standard"
  },
  "tags":{},
  "kind":"Storage",
  "properties": {
    "creationTime":"2017-10-09T18:55:40.5863736Z",
    "primaryEndpoints": {
      "blob":"https://examplestorage.blob.core.windows.net/",
      "file":"https://examplestorage.file.core.windows.net/",
      "queue":"https://examplestorage.queue.core.windows.net/",
      "table":"https://examplestorage.table.core.windows.net/"
    },
    "primaryLocation":"southcentralus",
    "provisioningState":"Succeeded",
    "statusOfPrimary":"available",
    "supportsHttpsTrafficOnly":false
  },
  "subscriptionId":"<subscription-id>",
  "resourceGroupName":"functionexamplegroup",
  "resourceId":"Microsoft.Storage/storageAccounts/examplestorage",
  "referenceApiVersion":"2016-12-01",
  "condition":true,
  "isConditionTrue":true,
  "isTemplateResource":false,
  "isAction":false,
  "provisioningOperation":"Read"
}
```

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) bu şablonda dağıtılmamasa bir depolama hesabına başvurur. Depolama hesabı zaten aynı abonelik içinde bulunmaktadır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageResourceGroup": {
            "type": "string"
        },
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]",
            "type": "object"
        }
    }
}
```

## <a name="resourcegroup"></a>resourceGroup

```json
resourceGroup()
```

Geçerli kaynak grubunu temsil eden bir nesne döndürür.

### <a name="return-value"></a>Döndürülen değer

Döndürülen nesne aşağıdaki biçimdedir:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "type":"Microsoft.Resources/resourceGroups",
  "location": "{resourceGroupLocation}",
  "managedBy": "{identifier-of-managing-resource}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

**Yönetilen By** özelliği yalnızca başka bir hizmet tarafından yönetilen kaynakları içeren kaynak grupları için döndürülür. Yönetilen Uygulamalar, Databricks ve AKS için özelliğin değeri, yönetici kaynağın kaynak kimliğidir.

### <a name="remarks"></a>Açıklamalar

İşlev, `resourceGroup()` abonelik düzeyinde dağıtılan bir [şablonda](deploy-to-subscription.md)kullanılamaz. Yalnızca bir kaynak grubuna dağıtılan şablonlarda kullanılabilir. İşlev, `resourceGroup()` üst şablon aboneye dağıtılsa bile, bir kaynak grubunu hedefleyen [bağlantılı veya iç içe bir şablonda (iç kapsamda)](linked-templates.md) kullanabilirsiniz. Bu senaryoda, bağlı veya iç içe şablon kaynak grubu düzeyinde dağıtılır. Abonelik düzeyi dağıtımında bir kaynak grubunu hedefleme hakkında daha fazla bilgi için azure [kaynaklarını birden fazla abonelik veya kaynak grubuna dağıt'a](cross-resource-group-deployment.md)bakın.

Kaynak Grubu işlevinin yaygın kullanımı, kaynak grubuyla aynı konumda kaynak oluşturmaktır. Aşağıdaki örnekte varsayılan parametre değeri için kaynak grubu konumu kullanır.

```json
"parameters": {
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    }
}
```

Kaynak grubundan bir kaynağa etiketleruygulamak için kaynak Grubu işlevini de kullanabilirsiniz. Daha fazla bilgi için [kaynak grubundan etiketleri uygula'ya](../management/tag-resources.md#apply-tags-from-resource-group)bakın.

Birden çok kaynak grubuna dağıtmak için iç içe yönelik şablonları kullanırken, kaynak Grubu işlevini değerlendirmek için kapsamı belirtebilirsiniz. Daha fazla bilgi için bkz: [Azure kaynaklarını birden fazla abonelik veya kaynak grubuna dağıtın.](cross-resource-group-deployment.md)

### <a name="resource-group-example"></a>Kaynak grubu örneği

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourcegroup.json) kaynak grubunun özelliklerini döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "resourceGroupOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

Önceki örnek aşağıdaki biçimde bir nesne döndürür:

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "type":"Microsoft.Resources/resourceGroups",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

## <a name="resourceid"></a>resourceId

```json
resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2], ...)
```

Kaynağın benzersiz tanımlayıcısını döndürür. Kaynak adı belirsiz olduğunda veya aynı şablon içinde sağlanmadığında bu işlevi kullanırsınız. Döndürülen tanımlayıcının biçimi, dağıtımın kaynak grubu, abonelik, yönetim grubu veya kiracı kapsamında gerçekleşip gerçekleşmediğine bağlı olarak değişir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| subscriptionId |Hayır |string (GUID formatında) |Varsayılan değer geçerli aboneliktir. Başka bir abonelikte kaynak almanız gerektiğinde bu değeri belirtin. Bu değeri yalnızca kaynak grubu veya abonelik kapsamında dağıtırken sağlayın. |
| resourceGroupName |Hayır |string |Varsayılan değer geçerli kaynak grubudur. Başka bir kaynak grubunda kaynak almanız gerektiğinde bu değeri belirtin. Bu değeri yalnızca kaynak grubunun kapsamında dağıtırken sağlayın. |
| resourceType |Evet |string |Kaynak sağlayıcı ad alanı da dahil olmak üzere kaynak türü. |
| resourceName1 |Evet |string |Kaynağın adı. |
| resourceName2 |Hayır |string |Gerekirse sonraki kaynak adı kesimi. |

Kaynak türü daha fazla kesim içeriyorsa, kaynak adlarını parametre olarak eklemeye devam edin.

### <a name="return-value"></a>Döndürülen değer

Şablon bir kaynak grubunun kapsamında dağıtıldığında, kaynak kimliği aşağıdaki biçimde döndürülür:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Abonelik düzeyinde bir [dağıtımda](deploy-to-subscription.md)kullanıldığında, kaynak kimliği aşağıdaki biçimde döndürülür:

```json
/subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Yönetim grubu [düzeyinde dağıtım](deploy-to-management-group.md) veya kiracı düzeyinde dağıtım kullanıldığında, kaynak kimliği aşağıdaki biçimde döndürülür:

```json
/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Kimliği başka biçimlerde almak için bkz:

* [uzantısıResourceId](#extensionresourceid)
* [abonelikResourceId](#subscriptionresourceid)
* [tenantResourceId](#tenantresourceid)

### <a name="remarks"></a>Açıklamalar

Sağladığınız parametrelerin sayısı, kaynağın üst veya alt kaynak olup olmamasına ve kaynağın aynı abonelik veya kaynak grubunda olup olmadığına bağlı olarak değişir.

Aynı abonelik ve kaynak grubundaki bir üst kaynağın kaynak kimliğini almak için kaynağın türünü ve adını sağlayın.

```json
"[resourceId('Microsoft.ServiceBus/namespaces', 'namespace1')]"
```

Alt kaynak için kaynak kimliğini almak için kaynak türündeki segment sayısına dikkat edin. Kaynak türünün her kesimi için bir kaynak adı sağlayın. Kesimin adı, hiyerarşinin o bölümü için var olan kaynağa karşılık gelir.

```json
"[resourceId('Microsoft.ServiceBus/namespaces/queues/authorizationRules', 'namespace1', 'queue1', 'auth1')]"
```

Aynı abonelikteki ancak farklı kaynak grubundaki bir kaynağın kaynak kimliğini almak için kaynak grubu adını sağlayın.

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts', 'examplestorage')]"
```

Farklı bir abonelik ve kaynak grubundaki bir kaynağın kaynak kimliğini almak için abonelik kimliği ve kaynak grubu adı sağlayın.

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Genellikle, alternatif bir kaynak grubunda bir depolama hesabı veya sanal ağ kullanırken bu işlevi kullanmanız gerekir. Aşağıdaki örnek, dış kaynak grubundan bir kaynağın nasıl kolayca kullanılabileceğini gösterir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "subnet1Ref": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworkName'), parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="resource-id-example"></a>Kaynak Kimliği örneği

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) kaynak grubundaki bir depolama hesabının kaynak kimliğini döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('11111111-1111-1111-1111-111111111111', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| sameRGOutput | Dize | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Dize | /subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Dize | /subscriptions/1111111-1111-11111111111111/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| iç içe Kaynak Çıktısı | Dize | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databasename |

## <a name="subscription"></a>aboneliği

```json
subscription()
```

Geçerli dağıtım aboneliği yle ilgili ayrıntıları verir.

### <a name="return-value"></a>Döndürülen değer

İşlev aşağıdaki biçimi döndürür:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="remarks"></a>Açıklamalar

Birden çok aboneye dağıtmak için iç içe yönelik şablonları kullanırken, abonelik işlevini değerlendirmek için kapsamı belirtebilirsiniz. Daha fazla bilgi için bkz: [Azure kaynaklarını birden fazla abonelik veya kaynak grubuna dağıtın.](cross-resource-group-deployment.md)

### <a name="subscription-example"></a>Abonelik örneği

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) çıktılar bölümünde çağrılan abonelik işlevini gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

## <a name="subscriptionresourceid"></a>abonelikResourceId

```json
subscriptionResourceId([subscriptionId], resourceType, resourceName1, [resourceName2], ...)
```

Abonelik düzeyinde dağıtılan bir kaynak için benzersiz tanımlayıcıyı döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| subscriptionId |Hayır |string (GUID formatında) |Varsayılan değer geçerli aboneliktir. Başka bir abonelikte kaynak almanız gerektiğinde bu değeri belirtin. |
| resourceType |Evet |string |Kaynak sağlayıcı ad alanı da dahil olmak üzere kaynak türü. |
| resourceName1 |Evet |string |Kaynağın adı. |
| resourceName2 |Hayır |string |Gerekirse sonraki kaynak adı kesimi. |

Kaynak türü daha fazla kesim içeriyorsa, kaynak adlarını parametre olarak eklemeye devam edin.

### <a name="return-value"></a>Döndürülen değer

Tanımlayıcı aşağıdaki biçimde döndürülür:

```json
/subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Açıklamalar

Bu işlevi, kaynak grubu yerine [aboneliğe dağıtılan](deploy-to-subscription.md) kaynaklar için kaynak kimliğini almak için kullanırsınız. Döndürülen kimlik, kaynak grubu değeri dahil değil, [resourceId](#resourceid) işlevi tarafından döndürülen değerden farklıdır.

### <a name="subscriptionresourceid-example"></a>abonelikResourceID örneği

Aşağıdaki şablon yerleşik bir rol atar. Kaynak grubuna veya aboneye dağıtabilirsiniz. Yerleşik roller için kaynak kimliğini almak için subscriptionResourceId işlevini kullanır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        },
        "builtInRoleType": {
            "type": "string",
            "allowedValues": [
                "Owner",
                "Contributor",
                "Reader"
            ],
            "metadata": {
                "description": "Built-in role to assign"
            }
        },
        "roleNameGuid": {
            "type": "string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "A new GUID used to identify the role assignment"
            }
        }
    },
    "variables": {
        "Owner": "[subscriptionResourceId('Microsoft.Authorization/roleDefinitions', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
        "Contributor": "[subscriptionResourceId('Microsoft.Authorization/roleDefinitions', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Reader": "[subscriptionResourceId('Microsoft.Authorization/roleDefinitions', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[parameters('roleNameGuid')]",
            "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

## <a name="tenantresourceid"></a>tenantResourceId

```json
tenantResourceId(resourceType, resourceName1, [resourceName2], ...)
```

Kiracı düzeyinde dağıtılan bir kaynak için benzersiz tanımlayıcıyı döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceType |Evet |string |Kaynak sağlayıcı ad alanı da dahil olmak üzere kaynak türü. |
| resourceName1 |Evet |string |Kaynağın adı. |
| resourceName2 |Hayır |string |Gerekirse sonraki kaynak adı kesimi. |

Kaynak türü daha fazla kesim içeriyorsa, kaynak adlarını parametre olarak eklemeye devam edin.

### <a name="return-value"></a>Döndürülen değer

Tanımlayıcı aşağıdaki biçimde döndürülür:

```json
/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Açıklamalar

Bu işlevi, kiracıya dağıtılan bir kaynağın kaynak kimliğini almak için kullanırsınız. Döndürülen kimlik, kaynak grubu veya abonelik değerlerini dahil etmeyerek diğer kaynak kimliği işlevleri tarafından döndürülen değerlerden farklıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Kaynak Yöneticisi şablonundaki bölümlerin açıklaması için [bkz.](template-syntax.md)
* Birden çok şablonu birleştirmek için bkz: [Azure Kaynak Yöneticisi ile bağlantılı şablonları kullanma.](linked-templates.md)
* Bir kaynak türü oluştururken belirli sayıda kez yeniden sıralamak için azure [kaynak yöneticisinde birden çok kaynak örneği oluşturma](copy-resources.md)bölümüne bakın.
* Oluşturduğunuz şablonu nasıl dağıtabileceğinizi görmek için Azure [Kaynak Yöneticisi şablonuyla bir uygulama dağıt'a](deploy-powershell.md)bakın.

