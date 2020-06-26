---
title: Yönetilen hizmet kimliğiyle olay teslimi
description: Bu makalede bir Azure olay Kılavuzu konusu için yönetilen hizmet kimliğinin nasıl etkinleştirileceği açıklanır. Olayları desteklenen hedeflere iletmek için kullanın.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 06/18/2020
ms.author: spelluru
ms.openlocfilehash: 4d81845ab61d8a84b9bad47ede4a027cd772c499
ms.sourcegitcommit: fdaad48994bdb9e35cdd445c31b4bac0dd006294
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85412949"
---
# <a name="event-delivery-with-a-managed-identity"></a>Yönetilen bir kimlikle olay teslimi
Bu makalede, Azure Event Grid konuları veya etki alanları için [yönetilen bir hizmet kimliğinin](../active-directory/managed-identities-azure-resources/overview.md) nasıl etkinleştirileceği açıklanır. Olayları, Service Bus kuyrukları ve konuları, Olay Hub 'ları ve depolama hesapları gibi desteklenen hedeflere iletmek için kullanın.

Bu makalede ayrıntılı olarak ele alınan adımlar aşağıda verilmiştir:
1. Bir sistem tarafından atanan kimliğe sahip bir konu veya etki alanı oluşturun ya da kimliği etkinleştirmek için mevcut bir konuyu veya etki alanını güncelleştirin. 
1. Kimliği, hedef üzerinde (örneğin, bir Service Bus kuyruğu) uygun bir role (örneğin, Service Bus veri gönderici) ekleyin.
1. Olay abonelikleri oluşturduğunuzda, hedefe olayları iletmek için kimliğin kullanımını etkinleştirin. 

## <a name="create-a-topic-or-domain-with-an-identity"></a>Kimlik ile konu veya etki alanı oluşturma
İlk olarak, bir konu veya sistem tarafından yönetilen kimliğe sahip bir etki alanı oluşturma konusuna bakalım.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma
Azure portal oluştururken bir konu veya etki alanı için sistem tarafından atanan kimliği etkinleştirebilirsiniz. Aşağıdaki görüntüde, bir konu için sistem tarafından yönetilen kimliğin nasıl etkinleştirileceği gösterilmektedir. Temel olarak, konu oluşturma sihirbazının **Gelişmiş** sayfasında **sistem tarafından atanan kimliği etkinleştir** seçeneğini belirleyin. Bu seçeneği, etki alanı oluşturma sihirbazının **Gelişmiş** sayfasında de görürsünüz. 

![Konu oluştururken kimliği etkinleştirin](./media/managed-service-identity/create-topic-identity.png)

### <a name="use-the-azure-cli"></a>Azure CLI kullanma
Ayrıca, sistem tarafından atanan bir kimlikle bir konu veya etki alanı oluşturmak için Azure CLı 'yi de kullanabilirsiniz. `az eventgrid topic create`Komutunu `--identity` parametresi olarak ayarlanmış şekilde kullanın `systemassigned` . Bu parametre için bir değer belirtmezseniz, varsayılan değer `noidentity` kullanılır. 

```azurecli-interactive
# create a topic with a system-assigned identity
az eventgrid topic create -g <RESOURCE GROUP NAME> --name <TOPIC NAME> -l <LOCATION>  --identity systemassigned
```

Benzer şekilde, `az eventgrid domain create` sistem tarafından yönetilen kimliğe sahip bir etki alanı oluşturmak için komutunu kullanabilirsiniz.

## <a name="enable-an-identity-for-an-existing-topic-or-domain"></a>Mevcut konu veya etki alanı için bir kimlik etkinleştir
Önceki bölümde, bir konu veya etki alanı oluştururken sistem tarafından yönetilen bir kimliği nasıl etkinleştireceğinizi öğrendiniz. Bu bölümde, var olan konu veya etki alanı için sistem tarafından yönetilen bir kimliği nasıl etkinleştireceğinizi öğreneceksiniz. 

### <a name="use-the-azure-portal"></a>Azure portalı kullanma
Aşağıdaki yordamda, bir konu için sistem tarafından yönetilen kimliğin nasıl etkinleştirileceği gösterilmektedir. Bir etki alanı için kimlik etkinleştirme adımları benzerdir. 

1. [Azure Portal](https://portal.azure.com)gidin.
2. Üstteki arama çubuğunda **olay Kılavuzu konularını** arayın.
3. Yönetilen kimliği etkinleştirmek istediğiniz **konuyu** seçin. 
4. **Kimlik** sekmesine geçin. 
5. Kimliği **etkinleştirmek için anahtarı açın.** 
1. Ayarları kaydetmek için araç çubuğunda **Kaydet** ' i seçin. 

    :::image type="content" source="./media/managed-service-identity/identity-existing-topic.png" alt-text="Konunun kimlik sayfası"::: 

Event Grid etki alanı için bir kimlik etkinleştirmek üzere benzer adımları kullanabilirsiniz.

### <a name="use-the-azure-cli"></a>Azure CLI kullanma
`az eventgrid topic update` `--identity` `systemassigned` Mevcut bir konu için sistem tarafından atanan kimliği etkinleştirmek üzere olarak ayarla komutunu kullanın. Kimliği devre dışı bırakmak istiyorsanız `noidentity` değer olarak belirtin. 

```azurecli-interactive
# Update the topic to assign a system-assigned identity. 
az eventgrid topic update -g $rg --name $topicname --identity systemassigned --sku basic 
```

Var olan bir etki alanını güncelleştirme komutu benzerdir ( `az eventgrid domain update` ).

## <a name="supported-destinations-and-rbac-roles"></a>Desteklenen hedefler ve RBAC rolleri
Olay kılavuzunuzun konusu veya etki alanı için kimliği etkinleştirdikten sonra Azure, Azure Active Directory içinde otomatik olarak bir kimlik oluşturur. Bu kimliği uygun rol tabanlı erişim denetimi (RBAC) rollerine ekleyerek konunun veya etki alanının olayları desteklenen hedeflere iletebilmesini sağlayın. Örneğin, olay Kılavuzu konusunun olayları bu ad alanındaki Olay Hub 'larına iletebilmesi için, Azure Event Hubs ad alanı için **azure Event Hubs Data sender** rolüne kimliği ekleyin. 

Şu anda Azure Event Grid, olayları aşağıdaki hedeflere iletmek için sistem tarafından atanan yönetilen kimlikle yapılandırılmış konuları veya etki alanlarını destekler. Bu tabloda, konunun olayları iletebilmesi için kimliğin içinde olması gereken roller de sunulur.

| Hedef | RBAC rolü | 
| ----------- | --------- | 
| Service Bus kuyrukları ve konuları | [Veri Göndericisini Azure Service Bus](../service-bus-messaging/authenticate-application.md#built-in-rbac-roles-for-azure-service-bus) |
| Azure Event Hubs | [Azure Event Hubs veri gönderici](../event-hubs/authorize-access-azure-active-directory.md#built-in-rbac-roles-for-azure-event-hubs) | 
| Azure Blob depolama | [Depolama Blobu veri Katılımcısı](../storage/common/storage-auth-aad-rbac-portal.md#rbac-roles-for-blobs-and-queues) |
| Azure Kuyruk Depolama |[Depolama kuyruğu veri Iletisi gönderici](../storage/common/storage-auth-aad-rbac-portal.md#rbac-roles-for-blobs-and-queues) | 

## <a name="add-an-identity-to-rbac-roles-on-destinations"></a>Hedeflerdeki RBAC rollerine bir kimlik ekleme
Bu bölümde, konağınız veya etki alanınız için bir RBAC rolüne nasıl kimlik ekleneceği açıklanmaktadır. 

### <a name="use-the-azure-portal"></a>Azure portalı kullanma
Konunun veya etki alanının olayları hedefe iletebilmesi için konuyu veya etki alanı kimliğini uygun bir role atamak üzere Azure portal kullanabilirsiniz. 

Aşağıdaki örnek, bir kuyruk veya konu kaynağı içeren bir Service Bus ad alanı için **Azure Service Bus veri gönderici** rolüne **msitesttopic** adlı bir olay Kılavuzu konusu için yönetilen bir kimlik ekler. Ad alanı düzeyinde role eklediğinizde, konu ad alanı içindeki tüm varlıklara olayları iletebilir. 

1. [Azure portal](https://portal.azure.com) **Service Bus ad alanına** gidin. 
1. Sol bölmedeki **Access Control** seçin. 
1. **Rol ataması Ekle** bölümünde **Ekle** ' yi seçin. 
1. **Rol ataması Ekle** sayfasında, aşağıdaki adımları uygulayın:
    1. Rolü seçin. Bu durumda, **veri gönderici Azure Service Bus**. 
    1. Konağınız veya etki alanınız için **kimlik** seçin. 
    1. Yapılandırmayı kaydetmek için **Kaydet** ' i seçin.

Bu adımlar, tabloda bahsedilen diğer rollere bir kimlik eklemeye benzer. 

### <a name="use-the-azure-cli"></a>Azure CLI kullanma
Bu bölümdeki örnekte, bir RBAC rolüne kimlik eklemek için Azure CLı 'nın nasıl kullanılacağı gösterilmektedir. Örnek komutlar Event Grid konularına yöneliktir. Event Grid etki alanları için komutlar benzerdir. 

#### <a name="get-the-principal-id-for-the-topics-system-identity"></a>Konunun sistem kimliği için sorumlu KIMLIĞI al 
İlk olarak, konunun sistem tarafından yönetilen kimliğin asıl KIMLIĞINI alın ve kimliği uygun rollere atayın.

```azurecli-interactive
topic_pid=$(az ad sp list --display-name "$<TOPIC NAME>" --query [].objectId -o tsv)
```

#### <a name="create-a-role-assignment-for-event-hubs-at-various-scopes"></a>Çeşitli kapsamlardaki Olay Hub 'ları için rol ataması oluşturma 
Aşağıdaki CLı örneği, bir konunun kimliğini ad alanı düzeyinde veya Olay Hub 'ı düzeyinde **Azure Event Hubs veri gönderici** rolüne nasıl ekleneceğini gösterir. Rol atamasını ad alanı düzeyinde oluşturursanız, konu, bu ad alanındaki tüm olay hub 'larına olay iletebilir. Olay Hub 'ı düzeyinde bir rol ataması oluşturursanız, bu konu olayları yalnızca ilgili olay hub 'ına iletebilir. 


```azurecli-interactive
role="Azure Event Hubs Data Sender" 
namespaceresourceid=$(az eventhubs namespace show -n $<EVENT HUBS NAMESPACE NAME> -g <RESOURCE GROUP of EVENT HUB> --query "{I:id}" -o tsv) 
eventhubresourceid=$(az eventhubs eventhub show -n <EVENT HUB NAME> --namespace-name <EVENT HUBS NAMESPACE NAME> -g <RESOURCE GROUP of EVENT HUB> --query "{I:id}" -o tsv) 

# create role assignment for the whole namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$namespaceresourceid" 

# create role assignment scoped to just one event hub inside the namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$eventhubresourceid" 
```

#### <a name="create-a-role-assignment-for-a-service-bus-topic-at-various-scopes"></a>Çeşitli kapsamlar üzerinde Service Bus konu başlığı için rol ataması oluşturma 
Aşağıdaki CLı örneği, bir konunun kimliğini ad alanı düzeyinde veya Service Bus konu düzeyinde **Azure Service Bus Data sender** rolüne nasıl ekleneceğini gösterir. Rol atamasını ad alanı düzeyinde oluşturursanız, olay Kılavuzu konusu ad alanı içinde olayları tüm varlıklara (Service Bus kuyruklar veya konular) iletebilir. Service Bus kuyruğu veya konu düzeyinde bir rol ataması oluşturursanız, olay Kılavuzu konusu olayları yalnızca belirli bir Service Bus kuyruğuna veya konuya iletebilir. 

```azurecli-interactive
role="Azure Service Bus Data Sender" 
namespaceresourceid=$(az servicebus namespace show -n $RG\SB -g "$RG" --query "{I:id}" -o tsv 
sbustopicresourceid=$(az servicebus topic show -n topic1 --namespace-name $RG\SB -g "$RG" --query "{I:id}" -o tsv) 

# create role assignment for the whole namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$namespaceresourceid" 

# create role assignment scoped to just one hub inside the namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$sbustopicresourceid" 
```

## <a name="create-event-subscriptions-that-use-an-identity"></a>Kimlik kullanan olay abonelikleri oluşturma
Sistem tarafından yönetilen kimliğe sahip bir konuya veya etki alanına sahip olduktan sonra kimliği hedefteki uygun role ekledikten sonra, kimliği kullanan abonelikler oluşturmaya hazırsınız demektir. 

### <a name="use-the-azure-portal"></a>Azure portalı kullanma
Bir olay aboneliği oluşturduğunuzda, **uç nokta ayrıntıları** bölümünde bir uç nokta için sistem tarafından atanan kimliğin kullanımını etkinleştirme seçeneğini görürsünüz. 

![Service Bus kuyruğu için olay aboneliği oluştururken kimliği etkinleştirin](./media/managed-service-identity/service-bus-queue-subscription-identity.png)

Ayrıca, **ek özellikler** sekmesinde atılacak kullanım için kullanılacak sistem tarafından atanan bir kimlik kullanmayı da etkinleştirebilirsiniz. 

![Yok sayılma için sistem tarafından atanan kimliği etkinleştirme](./media/managed-service-identity/enable-deadletter-identity.png)

### <a name="use-the-azure-cli---service-bus-queue"></a>Azure CLı-Service Bus kuyruğunu kullanma 
Bu bölümde, bir Service Bus kuyruğuna olayları teslim etmek üzere sistem tarafından atanan kimliğin kullanımını etkinleştirmek için Azure CLı 'nın nasıl kullanılacağını öğrenirsiniz. Kimliğin **Azure Service Bus veri gönderici** rolünün bir üyesi olması gerekir. Ayrıca, yük atılacak için kullanılan depolama hesabındaki **Depolama Blobu veri katılımcısı** rolünün bir üyesi olmalıdır. 

#### <a name="define-variables"></a>Değişkenleri tanımlama
İlk olarak, CLı komutunda kullanılacak aşağıdaki değişkenlerin değerlerini belirtin. 

```azurecli-interactive
subid="<AZURE SUBSCRIPTION ID>"
rg = "<RESOURCE GROUP of EVENT GRID TOPIC>"
topicname = "<EVENT GRID TOPIC NAME>"

# get the service bus queue resource id
queueid=$(az servicebus queue show --namespace-name <SERVICE BUS NAMESPACE NAME> --name <QUEUE NAME> --resource-group <RESOURCE GROUP NAME> --query id --output tsv)
sb_esname = "<Specify a name for the event subscription>" 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery"></a>Dağıtım için yönetilen bir kimlik kullanarak olay aboneliği oluşturma 
Bu örnek komut, **Service Bus kuyruğu**olarak ayarlanmış bir uç nokta türü olan Event Grid konusu için bir olay aboneliği oluşturur. 

```azurecli-interactive
az eventgrid event-subscription create  
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname
    --delivery-identity-endpoint-type servicebusqueue  
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $queueid
    -n $sb_esname 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery-and-dead-lettering"></a>Dağıtım ve atılacak kullanım için yönetilen kimlik kullanarak olay aboneliği oluşturma
Bu örnek komut, **Service Bus kuyruğu**olarak ayarlanmış bir uç nokta türü olan Event Grid konusu için bir olay aboneliği oluşturur. Ayrıca, sistem tarafından yönetilen kimliğin, atılacak kullanım için kullanılacağını belirtir. 

```azurecli-interactive
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)
deadletterendpoint="$storageid/blobServices/default/containers/<BLOB CONTAINER NAME>"

az eventgrid event-subscription create  
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type servicebusqueue
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $queueid
    --deadletter-identity-endpoint $deadletterendpoint 
    --deadletter-identity systemassigned 
    -n $sb_esnameq 
```

### <a name="use-the-azure-cli---event-hubs"></a>Azure CLı-Event Hubs kullanma 
Bu bölümde, bir olay hub 'ına olayları teslim etmek üzere sistem tarafından atanan kimliğin kullanımını etkinleştirmek için Azure CLı 'nın nasıl kullanılacağını öğrenirsiniz. Kimliğin **Azure Event Hubs veri gönderici** rolünün bir üyesi olması gerekir. Ayrıca, yük atılacak için kullanılan depolama hesabındaki **Depolama Blobu veri katılımcısı** rolünün bir üyesi olmalıdır. 

#### <a name="define-variables"></a>Değişkenleri tanımlama
```azurecli-interactive
subid="<AZURE SUBSCRIPTION ID>"
rg = "<RESOURCE GROUP of EVENT GRID TOPIC>"
topicname = "<EVENT GRID TOPIC NAME>"

hubid=$(az eventhubs eventhub show --name <EVENT HUB NAME> --namespace-name <NAMESPACE NAME> --resource-group <RESOURCE GROUP NAME> --query id --output tsv)
eh_esname = "<SPECIFY EVENT SUBSCRIPTION NAME>" 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery"></a>Dağıtım için yönetilen bir kimlik kullanarak olay aboneliği oluşturma 
Bu örnek komut, **Event Hubs**olarak ayarlanmış bir uç nokta türü olan Event Grid konusu için bir olay aboneliği oluşturur. 

```azurecli-interactive
az eventgrid event-subscription create  
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type eventhub 
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $hubid
    -n $sbq_esname 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery--deadletter"></a>Dağıtım için yönetilen bir kimlik ve sahipsiz mektup kullanarak olay aboneliği oluşturma 
Bu örnek komut, **Event Hubs**olarak ayarlanmış bir uç nokta türü olan Event Grid konusu için bir olay aboneliği oluşturur. Ayrıca, sistem tarafından yönetilen kimliğin, atılacak kullanım için kullanılacağını belirtir. 

```azurecli-interactive
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)
deadletterendpoint="$storageid/blobServices/default/containers/<BLOB CONTAINER NAME>"

az eventgrid event-subscription create
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type servicebusqueue  
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $hubid
    --deadletter-identity-endpoint $eh_deadletterendpoint
    --deadletter-identity systemassigned 
    -n $eh_esname 
```

### <a name="use-the-azure-cli---azure-storage-queue"></a>Azure CLı-Azure depolama kuyruğunu kullanma 
Bu bölümde, Azure depolama kuyruğuna olayları teslim etmek üzere sistem tarafından atanan kimliğin kullanımını etkinleştirmek için Azure CLı 'yı nasıl kullanacağınızı öğreneceksiniz. Kimlik, depolama hesabındaki **Depolama Blobu veri katılımcısı** rolünün bir üyesi olmalıdır.

#### <a name="define-variables"></a>Değişkenleri tanımlama  

```azurecli-interactive
subid="<AZURE SUBSCRIPTION ID>"
rg = "<RESOURCE GROUP of EVENT GRID TOPIC>"
topicname = "<EVENT GRID TOPIC NAME>"

# get the storage account resource id
storageid=$(az storage account show --name <STORAGE ACCOUNT NAME> --resource-group <RESOURCE GROUP NAME> --query id --output tsv)

# build the resource id for the queue
queueid="$storageid/queueservices/default/queues/<QUEUE NAME>" 

sa_esname = "<SPECIFY EVENT SUBSCRIPTION NAME>" 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery"></a>Dağıtım için yönetilen bir kimlik kullanarak olay aboneliği oluşturma 

```azurecli-interactive
az eventgrid event-subscription create 
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type storagequeue  
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $queueid
    -n $sa_esname 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery--deadletter"></a>Dağıtım için yönetilen bir kimlik ve sahipsiz mektup kullanarak olay aboneliği oluşturma 

```azurecli-interactive
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)
deadletterendpoint="$storageid/blobServices/default/containers/<BLOB CONTAINER NAME>"

az eventgrid event-subscription create  
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type storagequeue  
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $queueid
    --deadletter-identity-endpoint $deadletterendpoint 
    --deadletter-identity systemassigned 
    -n $sa_esname 
```



## <a name="next-steps"></a>Sonraki adımlar
Yönetilen hizmet kimlikleri hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md). 
