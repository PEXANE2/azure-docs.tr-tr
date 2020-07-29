---
title: Şablon işlevleri-kaynaklar
description: Kaynaklarla ilgili değerleri almak için Azure Resource Manager şablonda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 06/18/2020
ms.openlocfilehash: 89241558164505573e098bdf580af6542c6095c5
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87372391"
---
# <a name="resource-functions-for-arm-templates"></a>ARM şablonları için kaynak işlevleri

Kaynak Yöneticisi, Azure Resource Manager (ARM) şablonunuzda kaynak değerlerini almak için aşağıdaki işlevleri sağlar:

* [Extensionresourceıd](#extensionresourceid)
* [Listele](#list)
* [sağlayıcılarla](#providers)
* [başvurunun](#reference)
* [resourceGroup](#resourcegroup)
* [RESOURCEID](#resourceid)
* [aboneliğiniz](#subscription)
* [Subscriptionresourceıd](#subscriptionresourceid)
* [Tenantresourceıd](#tenantresourceid)

Parametrelerden, değişkenlerden veya geçerli dağıtımdan değer almak için bkz. [dağıtım değeri işlevleri](template-functions-deployment.md).

## <a name="extensionresourceid"></a>Extensionresourceıd

`extensionResourceId(resourceId, resourceType, resourceName1, [resourceName2], ...)`

Özelliklerine eklemek için başka bir kaynağa uygulanan bir kaynak türü olan [uzantı kaynağı](../management/extension-resource-types.md)IÇIN kaynak kimliğini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceId |Yes |dize |Uzantı kaynağının uygulandığı kaynağın kaynak KIMLIĞI. |
| resourceType |Yes |dize |Kaynak sağlayıcısı ad alanı dahil olmak üzere kaynak türü. |
| resourceName1 |Yes |dize |Kaynağın adı. |
| resourceName2 |Hayır |dize |Gerekirse, sonraki kaynak adı segmenti. |

Kaynak türü daha fazla kesim içerdiğinde kaynak adlarını parametre olarak eklemeye devam edin.

### <a name="return-value"></a>Döndürülen değer

Bu işlev tarafından döndürülen kaynak KIMLIĞININ temel biçimi:

```json
{scope}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

Kapsam segmenti, genişletilmekte olan kaynağa göre değişir.

Uzantı kaynağı bir **kaynağa**uygulandığında, kaynak kimliği aşağıdaki biçimde döndürülür:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{baseResourceProviderNamespace}/{baseResourceType}/{baseResourceName}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

Uzantı kaynağı bir **kaynak grubuna**uygulandığında, biçim şu şekilde olur:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

Uzantı kaynağı bir **aboneliğe**uygulandığında, biçim şu şekilde olur:

```json
/subscriptions/{subscriptionId}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

Uzantı kaynağı bir **yönetim grubuna**uygulandığında, biçim şu şekilde olur:

```json
/providers/Microsoft.Management/managementGroups/{managementGroupName}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

### <a name="extensionresourceid-example"></a>Extensionresourceıd örneği

Aşağıdaki örnek, bir kaynak grubu kilidinin kaynak KIMLIĞINI döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

<a id="listkeys"></a>
<a id="list"></a>

## <a name="list"></a>Listele

`list{Value}(resourceName or resourceIdentifier, apiVersion, functionValues)`

Bu işlevin sözdizimi, liste işlemlerinin adına göre değişir. Her uygulama, bir liste işlemini destekleyen kaynak türünün değerlerini döndürür. İşlem adı ile başlamalıdır `list` . Bazı yaygın kullanımlar `listKeys` , `listKeyValue` ve ' dir `listSecrets` .

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceName veya ResourceIdentifier |Yes |dize |Kaynak için benzersiz tanımlayıcı. |
| apiVersion |Yes |dize |Kaynak çalışma zamanı durumunun API sürümü. Genellikle, **yyyy-aa-gg**biçiminde. |
| functionValues |Hayır |object | İşlevi için değerler içeren bir nesne. Bu nesneyi yalnızca bir depolama hesabındaki **Listaccountsas** gibi parametre değerleriyle bir nesne almayı destekleyen işlevler için sağlayın. Bu makalede işlev değerlerini geçirme örneği gösterilmektedir. |

### <a name="valid-uses"></a>Geçerli kullanımlar

Liste işlevleri bir kaynak tanımının özelliklerinde kullanılabilir. Bir şablonun çıktılar bölümünde hassas bilgiler sunan bir liste işlevi kullanmayın. Çıkış değerleri dağıtım geçmişinde depolanır ve kötü niyetli bir kullanıcı tarafından alınabilir.

[Özellik yinelemesi](copy-properties.md)ile kullanıldığında, `input` ifadesi Resource özelliğine atandığı için liste işlevlerini kullanabilirsiniz. `count`Liste işlevi çözümlenmeden önce Count belirlenmesi gerektiğinden, bunları ile kullanamazsınız.

### <a name="implementations"></a>Uygulamalar

Listenin olası kullanımları aşağıdaki tabloda gösterilmiştir.

| Kaynak türü | İşlev adı |
| ------------- | ------------- |
| Microsoft. AnalysisServices/sunucuları | [listGatewayStatus](/rest/api/analysisservices/servers/listgatewaystatus) |
| Microsoft. AppConfiguration | [ListKeyValue](/rest/api/appconfiguration/configurationstores/listkeyvalue) |
| Microsoft. AppConfiguration/Configurationmağazaları | ListKeys 'i al |
| Microsoft. Automation/automationAccounts | [ListKeys 'i al](/rest/api/automation/keys/listbyautomationaccount) |
| Microsoft.Batch/batchAccounts | [ListKeys 'i al](/rest/api/batchmanagement/batchaccount/getkeys) |
| ChAI/Workspaces/denemeleri/Jobs Microsoft.Bat | [lıutputfiles](/rest/api/batchai/jobs/listoutputfiles) |
| Microsoft. Blockzincirine/blockchainMembers | [listApiKeys](/rest/api/blockchain/2019-06-01-preview/blockchainmembers/listapikeys) |
| Microsoft. Blockzincirine/blockchainMembers/transactionNodes | [listApiKeys](/rest/api/blockchain/2019-06-01-preview/transactionnodes/listapikeys) |
| Microsoft. BotService/botServices/Channels | [listChannelWithKeys](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/botservice/resource-manager/Microsoft.BotService/stable/2020-06-02/botservice.json#L553) |
| Microsoft. Cache/redsıs | [ListKeys 'i al](/rest/api/redis/redis/listkeys) |
| Microsoft. Biliveservices/hesapları | [ListKeys 'i al](/rest/api/cognitiveservices/accountmanagement/accounts/listkeys) |
| Microsoft. ContainerRegistry/kayıt defterleri | [listBuildSourceUploadUrl 'Si](/rest/api/containerregistry/registries%20(tasks)/getbuildsourceuploadurl) |
| Microsoft. ContainerRegistry/kayıt defterleri | [listCredentials](/rest/api/containerregistry/registries/listcredentials) |
| Microsoft. ContainerRegistry/kayıt defterleri | [Listkullanımlar](/rest/api/containerregistry/registries/listusages) |
| Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları | [listEvents](/rest/api/containerregistry/webhooks/listevents) |
| Microsoft. ContainerRegistry/kayıt defterleri/çalıştırmaları | [listLogSasUrl 'Si](/rest/api/containerregistry/runs/getlogsasurl) |
| Microsoft. ContainerRegistry/kayıt defterleri/görevler | [listDetails](/rest/api/containerregistry/tasks/getdetails) |
| Microsoft. ContainerService/Managedkümeler | [listClusterAdminCredential](/rest/api/aks/managedclusters/listclusteradmincredentials) |
| Microsoft. ContainerService/Managedkümeler | [listClusterUserCredential](/rest/api/aks/managedclusters/listclusterusercredentials) |
| Microsoft. ContainerService/Managedkümeler/accessProfiles | [listCredential](/rest/api/aks/managedclusters/getaccessprofile) |
| Microsoft. DataBox/Jobs | listCredentials |
| Microsoft. DataFactory/DataFactory/ağ geçitleri | listauthkeys |
| Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları | [listauthkeys](/rest/api/datafactory/integrationruntimes/listauthkeys) |
| Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/kapsayıcılar | [listSasTokens](/rest/api/datalakeanalytics/storageaccounts/listsastokens) |
| Microsoft. DataShare/hesaplar/paylaşımlar | [Listeşitlemeler](/rest/api/datashare/shares/listsynchronizations) |
| Microsoft. DataShare/accounts/shareSubscriptions | [Listsourcesharesynitizationsettings](/rest/api/datashare/sharesubscriptions/listsourcesharesynchronizationsettings) |
| Microsoft. DataShare/accounts/shareSubscriptions | [listSynchronizationDetails](/rest/api/datashare/sharesubscriptions/listsynchronizationdetails) |
| Microsoft. DataShare/accounts/shareSubscriptions | [Listeşitlemeler](/rest/api/datashare/sharesubscriptions/listsynchronizations) |
| Microsoft. Devices/iotHubs | [ListKeys 'i al](/rest/api/iothub/iothubresource/listkeys) |
| Microsoft. Devices/ıothubs/iotHubKeys | [ListKeys 'i al](/rest/api/iothub/iothubresource/getkeysforkeyname) |
| Microsoft. Devices/provisioningServices/anahtarlar | [ListKeys 'i al](/rest/api/iot-dps/iotdpsresource/listkeysforkeyname) |
| Microsoft. Devices/provisioningServices | [ListKeys 'i al](/rest/api/iot-dps/iotdpsresource/listkeys) |
| Microsoft. DevTestLab/Labs | [Listvhd 'ler](/rest/api/dtl/labs/listvhds) |
| Microsoft. DevTestLab/Labs/zamanlamaları | [ListApplicable](/rest/api/dtl/schedules/listapplicable) |
| Microsoft. DevTestLab/Labs/Users/Serviceyapılar | [Listapperepblezamanlamalar](/rest/api/dtl/servicefabrics/listapplicableschedules) |
| Microsoft. DevTestLab/Labs/virtualMachines | [Listapperepblezamanlamalar](/rest/api/dtl/virtualmachines/listapplicableschedules) |
| UmentDB/databaseAccounts Microsoft.Doc | [listConnectionStrings](/rest/api/cosmos-db-resource-provider/databaseaccounts/listconnectionstrings) |
| UmentDB/databaseAccounts Microsoft.Doc | [ListKeys 'i al](/rest/api/cosmos-db-resource-provider/databaseaccounts/listkeys) |
| Microsoft. DomainRegistration | [Listdomainönerilere](/rest/api/appservice/domains/listrecommendations) |
| Microsoft. DomainRegistration/topLevelDomains | [listAgreements](/rest/api/appservice/topleveldomains/listagreements) |
| Microsoft. EventGrid/Domains | [ListKeys 'i al](/rest/api/eventgrid/version2020-06-01/domains/listsharedaccesskeys) |
| Microsoft. EventGrid/konuları | [ListKeys 'i al](/rest/api/eventgrid/version2020-06-01/topics/listsharedaccesskeys) |
| Microsoft. EventHub/ad alanları/authorizationRules | [ListKeys 'i al](/rest/api/eventhub) |
| Microsoft. EventHub/namespaces/Disyıldız Recoveryconfigs/authorizationRules | [ListKeys 'i al](/rest/api/eventhub) |
| Microsoft. EventHub/namespaces/eventhubs/authorizationRules | [ListKeys 'i al](/rest/api/eventhub) |
| Microsoft. ımportexport/işler | [listBitLockerKeys](/rest/api/storageimportexport/bitlockerkeys/list) |
| Microsoft. kusto/kümeler/veritabanları | [Listsorumlularını](/rest/api/azurerekusto/databases/listprincipals) |
| Microsoft. LabServices/kullanıcılar | [Listortamortamları](/rest/api/labservices/globalusers/listenvironments) |
| Microsoft. LabServices/kullanıcılar | [ListLabs](/rest/api/labservices/globalusers/listlabs) |
| Microsoft. Logic/ıntegrationaccounts/sözleşmeleri | [listContentCallbackUrl](/rest/api/logic/agreements/listcontentcallbackurl) |
| Microsoft. Logic/ıntegrationaccounts/derlemeler | [listContentCallbackUrl](/rest/api/logic/integrationaccountassemblies/listcontentcallbackurl) |
| Microsoft. Logic/ıntegrationaccounts | [listCallbackUrl](/rest/api/logic/integrationaccounts/getcallbackurl) |
| Microsoft. Logic/ıntegrationaccounts | [listKeyVaultKeys](/rest/api/logic/integrationaccounts/listkeyvaultkeys) |
| Microsoft. Logic/ıntegrationaccounts/Maps | [listContentCallbackUrl](/rest/api/logic/maps/listcontentcallbackurl) |
| Microsoft. Logic/ıntegrationaccounts/iş ortakları | [listContentCallbackUrl](/rest/api/logic/partners/listcontentcallbackurl) |
| Microsoft. Logic/ıntegrationaccounts/schemas | [listContentCallbackUrl](/rest/api/logic/schemas/listcontentcallbackurl) |
| Microsoft. Logic/iş akışları | [listCallbackUrl](/rest/api/logic/workflows/listcallbackurl) |
| Microsoft. Logic/iş akışları | [listSwagger](/rest/api/logic/workflows/listswagger) |
| Microsoft. Logic/iş akışları/çalıştırmalar/eylemler | [Listexpressionizlemeleri](/rest/api/logic/workflowrunactions/listexpressiontraces) |
| Microsoft. Logic/iş akışları/çalıştırmalar/Actions/tekrarları | [Listexpressionizlemeleri](/rest/api/logic/workflowrunactionrepetitions/listexpressiontraces) |
| Microsoft. Logic/iş akışları/Tetikleyiciler | [listCallbackUrl](/rest/api/logic/workflowtriggers/listcallbackurl) |
| Microsoft. Logic/iş akışları/sürümler/Tetikleyiciler | [listCallbackUrl](/rest/api/logic/workflowversions/listcallbackurl) |
| Microsoft. Machinöğrenim/webServices | [ListKeys 'i al](/rest/api/machinelearning/webservices/listkeys) |
| Microsoft. Machinöğrenim/çalışma alanları | listworkspacekeys |
| Microsoft. MachineLearningServices/çalışma alanları/hesaplar | [ListKeys 'i al](/rest/api/azureml/workspacesandcomputes/machinelearningcompute/listkeys) |
| Microsoft. MachineLearningServices/çalışma alanları/hesaplar | [listNodes](/rest/api/azureml/workspacesandcomputes/machinelearningcompute/listnodes) |
| Microsoft. MachineLearningServices/çalışma alanları | [ListKeys 'i al](/rest/api/azureml/workspacesandcomputes/workspaces/listkeys) |
| Microsoft. Maps/hesaplar | [ListKeys 'i al](/rest/api/maps-management/accounts/listkeys) |
| Microsoft. Media/mediaservices/varlıkları | [listContainerSas](/rest/api/media/assets/listcontainersas) |
| Microsoft. Media/mediaservices/varlıkları | [Liststreamingkonumlandırıcı](/rest/api/media/assets/liststreaminglocators) |
| Microsoft. Media/mediaservices/Streamingkonumlandırıcı | [listContentKeys](/rest/api/media/streaminglocators/listcontentkeys) |
| Microsoft. Media/mediaservices/Streamingkonumlandırıcı | [listPaths](/rest/api/media/streaminglocators/listpaths) |
| Microsoft. Network/applicationSecurityGroups | Liztipi yapılandırması |
| Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules | [ListKeys 'i al](/rest/api/notificationhubs/namespaces/listkeys) |
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules | [ListKeys 'i al](/rest/api/notificationhubs/notificationhubs/listkeys) |
| Microsoft. Operationalınsights/çalışma alanları | [list](/rest/api/loganalytics/workspaces/list) |
| Microsoft. Poliyeleghts/düzeltmelere | [Listdağıtımlar](/rest/api/policy-insights/remediations/listdeploymentsatresourcegroup) |
| Microsoft. Relay/Namespace/authorizationRules | [ListKeys 'i al](/rest/api/relay/namespaces/listkeys) |
| Microsoft. Relay/namespaces/Disyıldız Recoveryconfigs/authorizationRules | ListKeys 'i al |
| Microsoft. Relay/Namespace/HybridConnections/authorizationRules | [ListKeys 'i al](/rest/api/relay/hybridconnections/listkeys) |
| Microsoft. Relay/namespaces/Wcfreyerleştir/authorizationRules | [ListKeys 'i al](/rest/api/relay/wcfrelays/listkeys) |
| Microsoft. Search/searchServices | [listAdminKeys](/rest/api/searchmanagement/adminkeys/get) |
| Microsoft. Search/searchServices | [listQueryKeys](/rest/api/searchmanagement/querykeys/listbysearchservice) |
| Microsoft. ServiceBus/namespaces/authorizationRules | [ListKeys 'i al](/rest/api/servicebus/namespaces/listkeys) |
| Microsoft. ServiceBus/namespaces/Diskalrecoveryconfigs/authorizationRules | [ListKeys 'i al](/rest/api/servicebus/disasterrecoveryconfigs/listkeys) |
| Microsoft. ServiceBus/namespaces/kuyruklar/authorizationRules | [ListKeys 'i al](/rest/api/servicebus/queues/listkeys) |
| Microsoft. ServiceBus/namespaces/konular/authorizationRules | [ListKeys 'i al](/rest/api/servicebus/topics/listkeys) |
| Microsoft. SignalRService/SignalR | [ListKeys 'i al](/rest/api/signalr/signalr/listkeys) |
| Microsoft. Storage/storageAccounts | [listAccountSas](/rest/api/storagerp/storageaccounts/listaccountsas) |
| Microsoft. Storage/storageAccounts | [ListKeys 'i al](/rest/api/storagerp/storageaccounts/listkeys) |
| Microsoft. Storage/storageAccounts | [listServiceSas](/rest/api/storagerp/storageaccounts/listservicesas) |
| Microsoft. StorSimple/yöneticileri/cihazları | [listFailoverSets](/rest/api/storsimple/devices/listfailoversets) |
| Microsoft. StorSimple/yöneticileri/cihazları | [listFailoverTargets](/rest/api/storsimple/devices/listfailovertargets) |
| Microsoft. StorSimple/yöneticileri | [listActivationKey](/rest/api/storsimple/managers/getactivationkey) |
| Microsoft. StorSimple/yöneticileri | [listPublicEncryptionKey](/rest/api/storsimple/managers/getpublicencryptionkey) |
| Microsoft. Web/Connectiongateway 'ler | ListStatus |
| Microsoft. Web/Connections | listconsentlinks |
| Microsoft. Web/Customapsıs | Listwsdlınterfaces |
| Microsoft. Web/konumlar | listwsdlınterfaces |
| Microsoft. Web/apimanagementaccounts/API/bağlantı | listconnectionkeys |
| Microsoft. Web/apimanagementaccounts/API/bağlantı | listgizlilikler |
| Microsoft. Web/siteler/yedeklemeler | [list](/rest/api/appservice/webapps/listbackups) |
| Microsoft. Web/Sites/config | [list](/rest/api/appservice/webapps/listconfigurations) |
| Microsoft. Web/Sites/işlevleri | [ListKeys 'i al](/rest/api/appservice/webapps/listfunctionkeys)
| Microsoft. Web/Sites/işlevleri | [listgizlilikler](/rest/api/appservice/webapps/listfunctionsecrets) |
| Microsoft. Web/Sites/hybridconnectionnamespaces/geçişler | [ListKeys 'i al](/rest/api/appservice/appserviceplans/listhybridconnectionkeys) |
| Microsoft. Web/siteler | [listsyncfunctiontriggerstatus](/rest/api/appservice/webapps/listsyncfunctiontriggers) |
| Microsoft. Web/Sites/yuvaları/işlevleri | [listgizlilikler](/rest/api/appservice/webapps/listfunctionsecretsslot) |
| Microsoft. Web/siteler/yuvalar/yedeklemeler | [list](/rest/api/appservice/webapps/listbackupsslot) |
| Microsoft. Web/siteler/yuvalar/yapılandırma | [list](/rest/api/appservice/webapps/listconfigurationsslot) |
| Microsoft. Web/Sites/yuvaları/işlevleri | [listgizlilikler](/rest/api/appservice/webapps/listfunctionsecretsslot) |

Hangi kaynak türlerinin bir liste işlemine sahip olduğunu öğrenmek için aşağıdaki seçeneklere sahipsiniz:

* Bir kaynak sağlayıcısı için [REST API işlemleri](/rest/api/) görüntüleyin ve liste işlemlerine bakın. Örneğin, depolama hesaplarının [ListKeys işlemi](/rest/api/storagerp/storageaccounts)vardır.
* [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) PowerShell cmdlet 'ini kullanın. Aşağıdaki örnek, depolama hesapları için tüm liste işlemlerini alır:

  ```powershell
  Get-AzProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Yalnızca liste işlemlerini filtrelemek için aşağıdaki Azure CLı komutunu kullanın:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

### <a name="return-value"></a>Döndürülen değer

Döndürülen nesne, kullandığınız liste işlevine göre değişir. Örneğin, bir depolama hesabı için listKeys aşağıdaki biçimi döndürür:

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

Diğer liste işlevlerinde farklı dönüş biçimleri vardır. Bir işlevin biçimini görmek için, örnek şablonda gösterildiği gibi çıktılar bölümüne ekleyin.

### <a name="remarks"></a>Açıklamalar

Kaynak adı ya da [RESOURCEID işlevini](#resourceid)kullanarak kaynağı belirtin. Başvurulan kaynağı dağıtan şablonda bir liste işlevi kullanırken, kaynak adını kullanın.

Koşullu olarak dağıtılan bir kaynakta bir **liste** işlevi kullanıyorsanız, işlev, kaynak dağıtılmasa bile değerlendirilir. **List** işlevi mevcut olmayan bir kaynağa başvuruyorsa bir hata alırsınız. İşlevin yalnızca kaynak dağıtıldığında değerlendirildiğinden emin olmak için **IF** işlevini kullanın. Koşullu olarak dağıtılan bir kaynakla IF ve List kullanan bir örnek şablon için [IF işlevine](template-functions-logical.md#if) bakın.

### <a name="list-example"></a>Liste örneği

Aşağıdaki örnek, [dağıtım betikleri](deployment-script-template.md)için bir değer ayarlarken ListKeys kullanır.

```json
"storageAccountSettings": {
    "storageAccountName": "[variables('storageAccountName')]",
    "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').keys[0].value]"
}
```

Sonraki örnekte bir parametre alan bir liste işlevi gösterilmektedir. Bu durumda, işlev **Listaccountsas**olur. Sona erme saati için bir nesne geçirin. Süre sonu zamanı gelecekte olmalıdır.

```json
"parameters": {
    "accountSasProperties": {
        "type": "object",
        "defaultValue": {
            "signedServices": "b",
            "signedPermission": "r",
            "signedExpiry": "2020-08-20T11:00:00Z",
            "signedResourceTypes": "s"
        }
    }
},
...
"sasToken": "[listAccountSas(parameters('storagename'), '2018-02-01', parameters('accountSasProperties')).accountSasToken]"
```

ListKeyValue örneği için bkz. [hızlı başlangıç: uygulama yapılandırması ve Kaynak Yöneticisi şablonuyla OTOMATIK VM dağıtımı](../../azure-app-configuration/quickstart-resource-manager.md#deploy-vm-using-stored-key-values).

## <a name="providers"></a>sağlayıcılarla

`providers(providerNamespace, [resourceType])`

Kaynak sağlayıcısı ve desteklenen kaynak türleri hakkında bilgi döndürür. Kaynak türü sağlamazsanız, işlev kaynak sağlayıcısı için desteklenen tüm türleri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| providerNamespace |Yes |dize |Sağlayıcının ad alanı |
| resourceType |Hayır |dize |Belirtilen ad alanı içindeki kaynak türü. |

### <a name="return-value"></a>Döndürülen değer

Desteklenen her tür aşağıdaki biçimde döndürülür:

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Döndürülen değerlerin dizi sıralaması garanti edilmez.

### <a name="providers-example"></a>Sağlayıcılar örneği

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/providers.json) , sağlayıcı işlevinin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

**Microsoft. Web** kaynak sağlayıcısı ve **siteleri** kaynak türü için, önceki örnek aşağıdaki biçimde bir nesne döndürür:

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

`reference(resourceName or resourceIdentifier, [apiVersion], ['Full'])`

Kaynağın çalışma zamanı durumunu temsil eden bir nesne döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceName veya ResourceIdentifier |Yes |dize |Kaynağın adı veya benzersiz tanımlayıcısı. Geçerli şablondaki bir kaynağa başvururken, yalnızca kaynak adını parametre olarak belirtin. Daha önce dağıtılan bir kaynağa ya da kaynağın adı belirsiz olduğunda kaynak KIMLIĞI sağlayın. |
| apiVersion |Hayır |dize |Belirtilen kaynağın API sürümü. **Kaynak aynı şablon içinde sağlanmıyorsa Bu parametre gereklidir.** Genellikle, **yyyy-aa-gg**biçiminde. Kaynağınız için geçerli API sürümleri için bkz. [şablon başvurusu](/azure/templates/). |
| Tümünü |Hayır |dize |Tam kaynak nesnesinin döndürülüp döndürülmeyeceğini belirten değer. Belirtmezseniz `'Full'` , yalnızca kaynağın Özellikler nesnesi döndürülür. Tam nesne, kaynak KIMLIĞI ve konum gibi değerleri içerir. |

### <a name="return-value"></a>Döndürülen değer

Her kaynak türü, başvuru işlevi için farklı özellikler döndürür. İşlev tek, önceden tanımlanmış bir biçim döndürmüyor. Ayrıca, döndürülen değer bağımsız değişkenin değerine göre farklılık gösterir `'Full'` . Bir kaynak türünün özelliklerini görmek için, örnekte gösterildiği gibi çıktılar bölümünde nesnesini döndürün.

### <a name="remarks"></a>Açıklamalar

Başvuru işlevi, önceden dağıtılan bir kaynağın veya geçerli şablonda dağıtılan bir kaynağın çalışma zamanı durumunu alır. Bu makalede her iki senaryo için örnekler gösterilmektedir.

Genellikle, blob uç noktası URI 'SI veya tam etki alanı adı gibi bir nesneden belirli bir değer döndürmek için **başvuru** işlevini kullanırsınız.

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

`'Full'`Özellikler şemasının parçası olmayan kaynak değerlerine ihtiyacınız olduğunda kullanın. Örneğin, Anahtar Kasası erişim ilkeleri ayarlamak için, bir sanal makine için kimlik özelliklerini alın.

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

Başvuru işlevi yalnızca bir kaynak tanımı ve bir şablon ya da dağıtımın çıktılar bölümünün özelliklerinde kullanılabilir. [Özellik yinelemesi](copy-properties.md)ile kullanıldığında, `input` ifadesi Resource özelliğine atandığı için başvuru işlevini kullanabilirsiniz.

Bir kopyalama döngüsünde özelliğinin değerini ayarlamak için başvuru işlevini kullanamazsınız `count` . Döngüdeki diğer özellikleri ayarlamak için ' i kullanabilirsiniz. Başvuru işlevi çözümlenmeden önce bu özelliğin belirlenmesi gerektiğinden, Count özelliği için başvuru engellenir.

İç içe geçmiş bir şablonun çıkışlar bölümünde başvuru işlevini veya herhangi bir List * işlevini kullanmak için, öğesini ```expressionEvaluationOptions``` [iç kapsam](linked-templates.md#expression-evaluation-scope-in-nested-templates) değerlendirmesini kullanacak şekilde ayarlamanız veya iç içe geçmiş bir şablon yerine bağlı bir bağlantı kullanmanız gerekir.

Koşullu olarak dağıtılan bir kaynakta **başvuru** işlevini kullanırsanız, işlev, kaynak dağıtılmasa bile değerlendirilir.  **Başvuru** işlevi mevcut olmayan bir kaynağa başvuruyorsa bir hata alırsınız. İşlevin yalnızca kaynak dağıtıldığında değerlendirildiğinden emin olmak için **IF** işlevini kullanın. Koşullu olarak dağıtılan bir kaynakla IF ve Reference kullanan bir örnek şablon için [IF işlevine](template-functions-logical.md#if) bakın.

### <a name="implicit-dependency"></a>Örtük bağımlılık

Başvuru işlevini kullanarak, başvurulan kaynak aynı şablon içinde sağlandıysa ve kaynağa adına (kaynak KIMLIĞI değil) göre başvurduğunuzda, bir kaynağın başka bir kaynağa bağlı olduğunu örtük olarak bildirmezsiniz. Bağımlıdson özelliğini de kullanmanız gerekmez. Başvurulan kaynağın dağıtımı tamamlanana kadar işlev değerlendirilmez.

### <a name="resource-name-or-identifier"></a>Kaynak adı veya tanımlayıcı

Aynı şablonda dağıtılan bir kaynağa başvururken kaynağın adını belirtin.

```json
"value": "[reference(parameters('storageAccountName'))]"
```

Aynı şablonda dağıtılan bir kaynağa başvurulduğunda, kaynak KIMLIĞINI ve ' ı belirtin `apiVersion` .

```json
"value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]"
```

Hangi kaynağı başvurduğunuz hakkında belirsizliğe engel olmak için, tam nitelikli kaynak tanımlayıcısı sağlayabilirsiniz.

```json
"value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', parameters('ipAddressName')))]"
```

Bir kaynağa tam nitelikli bir başvuru oluştururken, kesimleri tür ve ad ile birleştirme sırası yalnızca iki birleştirme değildir. Bunun yerine, ad alanından sonra en az belirli bir *türe/ad* çiftinden en belirgin bir dizi kullanın:

**{Resource-Provider-Namespace}/{Parent-Resource-Type}/{Parent-Resource-Name} [/{Child-Resource-Type}/{Child-Resource-Name}]**

Örnek:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt``Microsoft.Compute/virtualMachines/extensions/myVM/myExt`doğru değil

Herhangi bir kaynak KIMLIĞININ oluşturulmasını basitleştirmek için, `resourceId()` işlevi yerine bu belgede açıklanan işlevleri kullanın `concat()` .

### <a name="get-managed-identity"></a>Yönetilen kimliği al

[Azure kaynakları Için Yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md) , bazı kaynaklar için örtük olarak oluşturulan [uzantı kaynak türleridir](../management/extension-resource-types.md) . Yönetilen kimlik şablonda açıkça tanımlanmadığı için, kimliğin uygulandığı kaynağa başvurmanız gerekir. `Full`Örtük olarak oluşturulan kimlik de dahil olmak üzere tüm özellikleri almak için kullanın.

Bu model:

`"[reference(resourceId(<resource-provider-namespace>, <resource-name>, <API-version>, 'Full').Identity.propertyName]"`

Örneğin, bir sanal makineye uygulanan bir yönetilen kimliğin asıl KIMLIĞINI almak için şunu kullanın:

```json
"[reference(resourceId('Microsoft.Compute/virtualMachines', variables('vmName')),'2019-12-01', 'Full').identity.principalId]",
```

Ya da bir sanal makine ölçek kümesine uygulanan bir yönetilen kimliğin kiracı KIMLIĞINI almak için şunu kullanın:

```json
"[reference(resourceId('Microsoft.Compute/virtualMachineScaleSets',  variables('vmNodeType0Name')), 2019-12-01, 'Full').Identity.tenantId]"
```

### <a name="reference-example"></a>Başvuru örneği

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) bir kaynak dağıtır ve bu kaynağa başvurur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Önceki örnekte iki nesne döndürülür. Properties nesnesi aşağıdaki biçimdedir:

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

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) , bu şablonda dağıtılan bir depolama hesabına başvurur. Depolama hesabı aynı abonelikte zaten var.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

`resourceGroup()`

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

**ManagedBy** özelliği yalnızca başka bir hizmet tarafından yönetilen kaynakları içeren kaynak grupları için döndürülür. Yönetilen uygulamalar, Databricks ve AKS için, özelliğinin değeri, yöneten kaynağın kaynak KIMLIĞIDIR.

### <a name="remarks"></a>Açıklamalar

`resourceGroup()`İşlev, [abonelik düzeyinde dağıtılan](deploy-to-subscription.md)bir şablonda kullanılamaz. Yalnızca bir kaynak grubuna dağıtılan şablonlarda kullanılabilir. `resourceGroup()`İşlevini, ana şablon aboneliğe dağıtıldığında bile, bir kaynak grubunu hedefleyen [bağlantılı veya iç içe bir şablonda (iç kapsamla birlikte)](linked-templates.md) kullanabilirsiniz. Bu senaryoda, bağlantılı veya iç içe yerleştirilmiş şablon kaynak grubu düzeyinde dağıtılır. Abonelik düzeyi dağıtımında bir kaynak grubunu hedefleme hakkında daha fazla bilgi için bkz. [Azure kaynaklarını birden fazla aboneliğe veya kaynak grubuna dağıtma](cross-scope-deployment.md).

ResourceGroup işlevinin ortak kullanımı, kaynak grubuyla aynı konumda kaynak oluşturmaktır. Aşağıdaki örnek, varsayılan bir parametre değeri için kaynak grubu konumunu kullanır.

```json
"parameters": {
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    }
}
```

Kaynak grubundaki etiketleri bir kaynağa uygulamak için resourceGroup işlevini de kullanabilirsiniz. Daha fazla bilgi için bkz. [kaynak grubundan etiket uygulama](../management/tag-resources.md#apply-tags-from-resource-group).

Birden çok kaynak grubuna dağıtmak için iç içe geçmiş şablonlar kullanırken, resourceGroup işlevini değerlendirmek için kapsamı belirtebilirsiniz. Daha fazla bilgi için bkz. [Azure kaynaklarını birden fazla aboneliğe veya kaynak grubuna dağıtma](cross-scope-deployment.md).

### <a name="resource-group-example"></a>Kaynak grubu örneği

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourcegroup.json) , kaynak grubunun özelliklerini döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekte, aşağıdaki biçimdeki bir nesne döndürülür:

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

`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2], ...)`

Bir kaynağın benzersiz tanımlayıcısını döndürür. Bu işlevi, kaynak adı belirsiz olduğunda veya aynı şablon içinde sağlanmamışsa kullanın. Döndürülen Tanımlayıcının biçimi, dağıtımın bir kaynak grubunun, aboneliğin, yönetim grubunun veya kiracının kapsamında gerçekleşmediğine göre farklılık gösterir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| subscriptionId |Hayır |dize (GUID biçiminde) |Varsayılan değer geçerli abonelikte bulunur. Başka bir abonelikteki bir kaynağı almanız gerektiğinde bu değeri belirtin. Bu değeri yalnızca bir kaynak grubunun veya aboneliğin kapsamına dağıtma sırasında belirtin. |
| resourceGroupName |Hayır |dize |Varsayılan değer geçerli kaynak grubudur. Başka bir kaynak grubundaki bir kaynağı almanız gerektiğinde bu değeri belirtin. Yalnızca bir kaynak grubunun kapsamına dağıtım yaparken bu değeri sağlayın. |
| resourceType |Yes |dize |Kaynak sağlayıcısı ad alanı dahil olmak üzere kaynak türü. |
| resourceName1 |Yes |dize |Kaynağın adı. |
| resourceName2 |Hayır |dize |Gerekirse, sonraki kaynak adı segmenti. |

Kaynak türü daha fazla kesim içerdiğinde kaynak adlarını parametre olarak eklemeye devam edin.

### <a name="return-value"></a>Döndürülen değer

Şablon bir kaynak grubunun kapsamına dağıtıldığında, kaynak KIMLIĞI aşağıdaki biçimde döndürülür:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

[Abonelik düzeyinde bir dağıtımda](deploy-to-subscription.md)kullanıldığında, kaynak kimliği aşağıdaki biçimde döndürülür:

```json
/subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

[Yönetim grubu düzeyinde bir dağıtımda](deploy-to-management-group.md) veya kiracı düzeyinde dağıtımda kullanıldığında, kaynak kimliği aşağıdaki biçimde döndürülür:

```json
/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Diğer biçimlerdeki KIMLIĞI almak için bkz.:

* [Extensionresourceıd](#extensionresourceid)
* [Subscriptionresourceıd](#subscriptionresourceid)
* [Tenantresourceıd](#tenantresourceid)

### <a name="remarks"></a>Açıklamalar

Sağladığınız parametrelerin sayısı, kaynağın bir üst veya alt kaynak olduğunu ve kaynağın aynı abonelikte veya kaynak grubunda olup olmamasına göre farklılık gösterir.

Aynı abonelik ve kaynak grubundaki bir üst kaynağın kaynak KIMLIĞINI almak için kaynağın türünü ve adını belirtin.

```json
"[resourceId('Microsoft.ServiceBus/namespaces', 'namespace1')]"
```

Bir alt kaynağın kaynak KIMLIĞINI almak için, kaynak türündeki parçaların sayısına dikkat edin. Kaynak türünün her segmenti için bir kaynak adı belirtin. Segmentin adı, hiyerarşinin o parçası için var olan kaynağa karşılık gelir.

```json
"[resourceId('Microsoft.ServiceBus/namespaces/queues/authorizationRules', 'namespace1', 'queue1', 'auth1')]"
```

Aynı abonelikte ancak farklı kaynak grubunda bulunan bir kaynağın kaynak KIMLIĞINI almak için, kaynak grubu adını sağlayın.

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts', 'examplestorage')]"
```

Farklı bir abonelik ve kaynak grubundaki bir kaynağın kaynak KIMLIĞINI almak için abonelik KIMLIĞI ve kaynak grubu adını sağlayın.

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Genellikle, alternatif bir kaynak grubunda bir depolama hesabı veya sanal ağ kullanırken bu işlevi kullanmanız gerekir. Aşağıdaki örnek, bir dış kaynak grubundaki bir kaynağın nasıl kolayca kullanılabileceğini göstermektedir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

### <a name="resource-id-example"></a>Kaynak KIMLIĞI örneği

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) , kaynak grubundaki bir depolama HESABıNıN kaynak kimliğini döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| sameRGOutput | Dize | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| farklı, Goutput | Dize | /subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| Farklıya yerleştir | Dize | /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Dize | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName |

## <a name="subscription"></a>aboneliği

`subscription()`

Geçerli dağıtım için abonelik hakkındaki ayrıntıları döndürür.

### <a name="return-value"></a>Döndürülen değer

İşlevi aşağıdaki biçimi döndürür:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="remarks"></a>Açıklamalar

Birden çok aboneliğe dağıtmak için iç içe geçmiş şablonlar kullanırken, abonelik işlevini değerlendirmek için kapsamı belirtebilirsiniz. Daha fazla bilgi için bkz. [Azure kaynaklarını birden fazla aboneliğe veya kaynak grubuna dağıtma](cross-scope-deployment.md).

### <a name="subscription-example"></a>Abonelik örneği

Aşağıdaki [örnek şablonda](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) , çıktılar bölümünde çağrılan abonelik işlevi gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

## <a name="subscriptionresourceid"></a>Subscriptionresourceıd

`subscriptionResourceId([subscriptionId], resourceType, resourceName1, [resourceName2], ...)`

Abonelik düzeyinde dağıtılan bir kaynak için benzersiz tanımlayıcıyı döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| subscriptionId |Hayır |dize (GUID biçiminde) |Varsayılan değer geçerli abonelikte bulunur. Başka bir abonelikteki bir kaynağı almanız gerektiğinde bu değeri belirtin. |
| resourceType |Yes |dize |Kaynak sağlayıcısı ad alanı dahil olmak üzere kaynak türü. |
| resourceName1 |Yes |dize |Kaynağın adı. |
| resourceName2 |Hayır |dize |Gerekirse, sonraki kaynak adı segmenti. |

Kaynak türü daha fazla kesim içerdiğinde kaynak adlarını parametre olarak eklemeye devam edin.

### <a name="return-value"></a>Döndürülen değer

Tanımlayıcı aşağıdaki biçimde döndürülür:

```json
/subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Açıklamalar

Bu işlevi, bir kaynak grubu yerine [aboneliğe dağıtılan](deploy-to-subscription.md) KAYNAKLARıN kaynak kimliğini almak için kullanırsınız. Döndürülen KIMLIK, bir kaynak grubu değeri dahil değil [RESOURCEID](#resourceid) işlevi tarafından döndürülen değerden farklı.

### <a name="subscriptionresourceid-example"></a>Subscriptionresourceıd örneği

Aşağıdaki şablon yerleşik bir rol atar. Bunu bir kaynak grubuna veya aboneliğe dağıtabilirsiniz. Yerleşik rollerin kaynak KIMLIĞINI almak için Subscriptionresourceıd işlevini kullanır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

## <a name="tenantresourceid"></a>Tenantresourceıd

`tenantResourceId(resourceType, resourceName1, [resourceName2], ...)`

Kiracı düzeyinde dağıtılan bir kaynak için benzersiz tanımlayıcıyı döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceType |Yes |dize |Kaynak sağlayıcısı ad alanı dahil olmak üzere kaynak türü. |
| resourceName1 |Yes |dize |Kaynağın adı. |
| resourceName2 |Hayır |dize |Gerekirse, sonraki kaynak adı segmenti. |

Kaynak türü daha fazla kesim içerdiğinde kaynak adlarını parametre olarak eklemeye devam edin.

### <a name="return-value"></a>Döndürülen değer

Tanımlayıcı aşağıdaki biçimde döndürülür:

```json
/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Açıklamalar

Kiracıya dağıtılan bir kaynağın kaynak KIMLIĞINI almak için bu işlevi kullanın. Döndürülen KIMLIK, kaynak grubu veya abonelik değerleri dahil değil, diğer kaynak KIMLIĞI işlevleri tarafından döndürülen değerlerden farklıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager şablonundaki bölümlerin açıklaması için bkz. [yazma Azure Resource Manager şablonları](template-syntax.md).
* Birden çok şablonu birleştirmek için bkz. [Azure Resource Manager ile bağlı şablonları kullanma](linked-templates.md).
* Kaynak türünü oluştururken belirtilen sayıda yinelemek için, bkz. [Azure Resource Manager birden fazla kaynak örneği oluşturma](copy-resources.md).
* Oluşturduğunuz şablonun nasıl dağıtılacağını görmek için bkz. [Azure Resource Manager şablonuyla uygulama dağıtma](deploy-powershell.md).

