---
title: Olayları Azure Event Grid abonelikleri iletmek için yönetilen hizmet kimliğini kullanma
description: Bu makalede bir Azure olay Kılavuzu konusu için yönetilen hizmet kimliğinin nasıl etkinleştirileceği açıklanır. Olayları desteklenen hedeflere iletmek için kullanın.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 04/24/2020
ms.author: spelluru
ms.openlocfilehash: 0e1f46c0bd7ce22cae77db1a524336445fd3f795
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83691029"
---
# <a name="enable-and-use-managed-service-identity-for-an-event-grid-topic-or-domain"></a>Bir olay Kılavuzu konusu veya etki alanı için yönetilen hizmet kimliğini etkinleştirme ve kullanma 
Bu makalede, bir olay Kılavuzu konusu veya etki alanı için [yönetilen hizmet kimliğinin](../active-directory/managed-identities-azure-resources/overview.md) nasıl etkinleştirileceği açıklanır. Olayları, Service Bus kuyrukları ve konuları, Olay Hub 'ları ve depolama hesapları gibi desteklenen hedeflere iletmek için kullanın.

Bu makalede ayrıntılı olarak ele alınan adımlar aşağıda verilmiştir:
1. Kimliği etkinleştirmek için, sistem tarafından atanan kimliğe sahip bir konu veya etki alanı oluşturun (veya) var olan bir konuyu veya etki alanını güncelleştirin. 
2. Kimliği, hedef üzerinde uygun bir role (örnek: Service Bus veri gönderici) ekleyin (örnek: bir Service Bus kuyruğu)
3. Olay abonelikleri oluştururken, kimlik kullanımının hedefe olay sunmaya olanak sağlayın. 

## <a name="create-a-topic-or-domain-with-an-identity"></a>Kimlik ile konu veya etki alanı oluşturma
İlk olarak, bir konu veya sistem tarafından yönetilen kimliğe sahip bir etki alanı oluşturma konusuna bakalım.

### <a name="using-azure-portal"></a>Azure portalını kullanma
Bir konu/etki alanı için sistem tarafından atanan kimliği Azure portal oluştururken etkinleştirebilirsiniz. Aşağıdaki görüntüde, bir konu için sistem tarafından yönetilen kimliğin nasıl etkinleştirileceği gösterilmektedir. Temel olarak, konu oluşturma sihirbazının **Gelişmiş** sayfasında **sistem tarafından atanan kimliği etkinleştir** seçeneğini belirleyin. Bu seçeneği, etki alanı oluşturma sihirbazının **Gelişmiş** sayfasında da görürsünüz. 

![Konu oluştururken kimliği etkinleştirin](./media/managed-service-identity/create-topic-identity.png)

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Ayrıca, sistem tarafından atanan kimliğe sahip bir konu veya etki alanı oluşturmak için Azure CLı 'yi de kullanabilirsiniz. `az eventgrid topic create`Komutunu `--identity` parametresi olarak ayarlanmış şekilde kullanın `systemassigned` . Bu parametre için bir değer belirtmezseniz, varsayılan değer `noidentity` kullanılır. 

```azurecli-interactive
# create a topic with a system-assigned identity
az eventgrid topic create -g <RESOURCE GROUP NAME> --name <TOPIC NAME> -l <LOCATION>  --identity systemassigned
```

Benzer şekilde, `az eventgrid domain create` sistem tarafından yönetilen kimliğe sahip bir etki alanı oluşturmak için komutunu kullanabilirsiniz.

## <a name="enable-identity-for-an-existing-topic-or-domain"></a>Mevcut konu veya etki alanı için kimliği etkinleştir
Son bölümde, bir konu veya etki alanı oluştururken sistem tarafından yönetilen kimliği nasıl etkinleştireceğinizi öğrendiniz. Bu bölümde, mevcut bir konu veya etki alanı için sistem tarafından yönetilen kimliği nasıl etkinleştireceğinizi öğreneceksiniz. 

### <a name="using-azure-portal"></a>Azure portalını kullanma
1. [Azure Portal](https://portal.azure.com) gidin
2. Arama çubuğundaki **olay Kılavuzu konularını** arayın.
3. Yönetilen kimliği etkinleştirmek istediğiniz **konuyu** seçin. 
4. **Kimlik** sekmesine geçin. 
5. Kimliği etkinleştirmek için anahtarı açın. 

    Event Grid etki alanı için kimliği etkinleştirmek üzere benzer adımları kullanabilirsiniz.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
`az eventgrid topic update` `--identity` `systemassigned` Mevcut bir konu için sistem tarafından atanan kimliği etkinleştirmek üzere olarak ayarla komutunu kullanın. Kimliği devre dışı bırakmak istiyorsanız `noidentity` değer olarak belirtin. 

```azurecli-interactive
# Update the topic to assign a system-assigned identity. 
az eventgrid topic update -g $rg --name $topicname --identity systemassigned --sku basic 
```

Var olan bir etki alanını güncelleştirme komutu benzerdir ( `az eventgrid domain update` ).

## <a name="supported-destinations-and-role-based-access-check-rbac-roles"></a>Desteklenen hedefler ve rol tabanlı erişim denetimi (RBAC) rolleri
Olay kılavuzunuzun konusu veya etki alanı için kimliği etkinleştirdikten sonra Azure, Azure Active Directory (Azure AD) içinde otomatik olarak bir kimlik oluşturur. Konunun veya etki alanının olayları desteklenen hedeflere iletebilmesi için bu kimliği uygun RBAC rollerine ekleyin. Örneğin, olay Kılavuzu konusunun olayları bu ad alanındaki Olay Hub 'larına iletebilmesi için, bir Event Hubs ad alanı için **Azure Event Hubs Data sender** rolüne kimliği ekleyin.  

Şu anda Azure Event Grid, olayları aşağıdaki hedeflere iletmek için sistem tarafından atanan yönetilen kimlikle yapılandırılan konuları veya etki alanlarını destekler. Bu tabloda, konunun olayları iletebilmesi için kimliğin içinde olması gereken roller de sunulur.

| Hedef | RBAC rolü | 
| ----------- | --------- | 
| Service Bus kuyrukları ve konuları | [Veri Göndericisini Azure Service Bus](../service-bus-messaging/authenticate-application.md#built-in-rbac-roles-for-azure-service-bus) |
| Olay hub'ı | [Azure Event Hubs veri gönderici](../event-hubs/authorize-access-azure-active-directory.md#built-in-rbac-roles-for-azure-event-hubs) | 
| Blob depolama | [Depolama Blobu veri Katılımcısı](../storage/common/storage-auth-aad-rbac-portal.md#rbac-roles-for-blobs-and-queues) |
| Kuyruk depolama |[Depolama kuyruğu veri Iletisi gönderici](../storage/common/storage-auth-aad-rbac-portal.md#rbac-roles-for-blobs-and-queues) | 

## <a name="add-identity-to-rbac-roles-on-destinations"></a>Hedeflerdeki RBAC rollerine kimlik ekleme
Bu bölümde, konağınız veya etki alanınız için bir RBAC rolüne nasıl kimlik ekleneceği açıklanmaktadır. 

### <a name="using-azure-portal"></a>Azure portalını kullanma
Konunun/etki alanının olayları hedefe iletebilmesi için konuyu/etki alanı kimliğini uygun bir role atamak üzere **Azure Portal** kullanabilirsiniz. 

Aşağıdaki örnek, bir kuyruk veya konu kaynağı içeren bir Service Bus **ad alanı** Için **Azure Service Bus veri gönderici** rolüne **msitesttopic** adlı bir olay Kılavuzu konusu için yönetilen bir kimlik ekler. Ad alanı düzeyinde role eklediğinizde, konu ad alanındaki tüm varlıklara olayları iletebilirler. 

1. [Azure portal](https://portal.azure.com) **Service Bus ad alanına** gidin. 
2. Sol bölmedeki **Access Control** seçin. 
3. **Rol ataması Ekle** bölümünde **Ekle** ' yi seçin. 
4. **Rol ataması Ekle** sayfasında, aşağıdaki adımları uygulayın:
    1. Rolü seçin. Bu durumda, **veri gönderici Azure Service Bus**. 
    2. Konağınız veya etki alanınız için **kimlik** seçin. 
    3. yapılandırmayı kaydetmek için **Kaydet** ' i seçin.

Bu adımlar, tabloda bahsedilen diğer rollere bir kimlik eklemeye benzer. 

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Bu bölümdeki örnekte, bir RBAC rolüne kimlik eklemek için **Azure CLI** 'nin nasıl kullanılacağı gösterilmektedir. Örnek komutlar Event Grid konularına yöneliktir. Event Grid etki alanları için komutlar benzerdir. 

#### <a name="get-principal-id-for-the-topics-system-identity"></a>Konunun sistem kimliği için sorumlu KIMLIĞI al 
İlk olarak, konunun sistem tarafından yönetilen kimliğin asıl KIMLIĞINI alın ve kimliği uygun rollere atayın.

```azurecli-interactive
topic_pid=$(az ad sp list --display-name "$<TOPIC NAME>" --query [].objectId -o tsv)
```

#### <a name="create-a-role-assignment-for-event-hubs-at-various-scopes"></a>Çeşitli kapsamlardaki Olay Hub 'ları için rol ataması oluşturma 
Aşağıdaki CLı örneği, bir konunun kimliğini ad alanı düzeyinde veya Olay Hub 'ı düzeyinde **Azure Event Hubs veri gönderici** rolüne nasıl ekleneceğini gösterir. Rol atamasını ad alanında oluşturursanız, konu, bu ad alanındaki tüm olay hub 'larına olay iletebilir. Olay Hub 'ı düzeyinde oluşturursanız, konusu olayları yalnızca ilgili olay hub 'ına iletebilir. 


```azurecli-interactive
role="Azure Event Hubs Data Sender" 
namespaceresourceid=$(az eventhubs namespace show -n $<EVENT HUBS NAMESPACE NAME> -g <RESOURCE GROUP of EVENT HUB> --query "{I:id}" -o tsv) 
eventhubresourceid=$(az eventhubs eventhub show -n <EVENT HUB NAME> --namespace-name <EVENT HUBS NAMESPACE NAME> -g <RESOURCE GROUP of EVENT HUB> --query "{I:id}" -o tsv) 

# create role assignment for the whole namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$namespaceresourceid" 

# create role assignment scoped to just one event hub inside the namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$eventhubresourceid" 
```

#### <a name="create-a-role-assignment-for-service-bus-topic-at-various-scopes"></a>Çeşitli kapsamlardaki Service Bus konusu için rol ataması oluşturma 
Aşağıdaki CLı örneği, bir konunun kimliğini ad alanı düzeyinde veya Service Bus konu düzeyinde **Azure Service Bus Data sender** rolüne nasıl ekleneceğini gösterir. Rol atamasını ad alanında oluşturursanız, olay Kılavuzu konusu ad alanı içindeki olayları (Service Bus kuyrukları veya konuları) iletebilirler. Service Bus kuyruğu veya konu düzeyinde oluşturursanız, olay Kılavuzu konusu yalnızca belirli bir Service Bus kuyruğu veya konusunun olaylarını iletebilir. 

```azurecli-interactive
role="Azure Service Bus Data Sender" 
namespaceresourceid=$(az servicebus namespace show -n $RG\SB -g "$RG" --query "{I:id}" -o tsv 
sbustopicresourceid=$(az servicebus topic show -n topic1 --namespace-name $RG\SB -g "$RG" --query "{I:id}" -o tsv) 

# create role assignment for the whole namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$namespaceresourceid" 

# create role assignment scoped to just one hub inside the namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$sbustopicresourceid" 
```

## <a name="create-event-subscriptions-that-use-identity"></a>Kimlik kullanan olay abonelikleri oluşturma
Sistem tarafından yönetilen kimliğe sahip bir konuya veya etki alanına sahip olduktan sonra kimliği hedefteki uygun role ekledikten sonra, kimliği kullanan abonelikler oluşturmaya hazırsınız demektir. 

### <a name="using-azure-portal"></a>Azure portalını kullanma
Bir olay aboneliği oluştururken, **uç nokta ayrıntıları** bölümünde bir uç nokta için sistem tarafından atanan kimliğin kullanımını etkinleştirme seçeneği görüntülenir. 

![Service Bus kuyruğu için olay aboneliği oluştururken kimliği etkinleştir](./media/managed-service-identity/service-bus-queue-subscription-identity.png)

Ayrıca, **ek özellikler** sekmesinde atılacak kullanım için kullanılacak sistem tarafından atanan kimliğin kullanılmasını da etkinleştirebilirsiniz. 

![Yok sayılma için sistem tarafından atanan kimliği etkinleştirme](./media/managed-service-identity/enable-deadletter-identity.png)

### <a name="using-azure-cli---service-bus-queue"></a>Azure CLı-Service Bus kuyruğu kullanma 
Bu bölümde, sistem tarafından atanan kimliğin Service Bus kuyruğuna olay teslim etmek üzere kullanımını etkinleştirmek için **Azure CLI** 'yi nasıl kullanacağınızı öğreneceksiniz. Kimliğin **Azure Service Bus veri gönderici** rolünün bir üyesi olması gerekir. Ayrıca, yük atılacak için kullanılan depolama hesabındaki **Depolama Blobu veri katılımcısı** rolünün bir üyesi olmalıdır. 

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

#### <a name="create-an-event-subscription-using-managed-identity-for-delivery"></a>dağıtım için yönetilen kimlik kullanarak bir olay aboneliği oluşturma 
Bu örnek komut, uç nokta türü **Service Bus kuyruğuna**ayarlanmış bir olay Kılavuzu konusu için bir olay aboneliği oluşturur. 

```azurecli-interactive
az eventgrid event-subscription create  
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname
    --delivery-identity-endpoint-type servicebusqueue  
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $queueid
    -n $sb_esname 
```

#### <a name="create-an-event-subscription-using-managed-identity-for-delivery-and-dead-lettering"></a>Dağıtım ve atılacak kullanım için yönetilen kimlik kullanarak bir olay aboneliği oluşturma
Bu örnek komut, uç nokta türü **Service Bus kuyruğuna**ayarlanmış bir olay Kılavuzu konusu için bir olay aboneliği oluşturur. Ayrıca, atılacak kullanım için kullanılacak sistem tarafından yönetilen kimliği belirtir. 

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

### <a name="azure-cli---event-hubs"></a>Azure CLı-Event Hubs 
Bu bölümde, bir olay hub 'ına olayları teslim etmek üzere sistem tarafından atanan kimliğin kullanımını etkinleştirmek için **Azure CLI** 'yi nasıl kullanacağınızı öğreneceksiniz. Kimliğin **Azure Event Hubs veri gönderici** rolünün bir üyesi olması gerekir. Ayrıca, yük atılacak için kullanılan depolama hesabındaki **Depolama Blobu veri katılımcısı** rolünün bir üyesi olmalıdır. 

#### <a name="define-variables"></a>Değişkenleri tanımlama
```azurecli-interactive
subid="<AZURE SUBSCRIPTION ID>"
rg = "<RESOURCE GROUP of EVENT GRID TOPIC>"
topicname = "<EVENT GRID TOPIC NAME>"

hubid=$(az eventhubs eventhub show --name <EVENT HUB NAME> --namespace-name <NAMESPACE NAME> --resource-group <RESOURCE GROUP NAME> --query id --output tsv)
eh_esname = "<SPECIFY EVENT SUBSCRIPTION NAME>" 
```

#### <a name="create-event-subscription-using-managed-identity-for-delivery"></a>dağıtım için yönetilen kimlik kullanarak olay aboneliği oluştur 
Bu örnek komut, uç nokta türü **Event Hubs**olarak ayarlanmış bir olay Kılavuzu konusu için bir olay aboneliği oluşturur. 

```azurecli-interactive
az eventgrid event-subscription create  
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type eventhub 
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $hubid
    -n $sbq_esname 
```

#### <a name="create-event-subscription-using-managed-identity-for-delivery--deadletter"></a>Dağıtım için yönetilen kimlik ve sahipsiz mektup kullanarak olay aboneliği oluştur 
Bu örnek komut, uç nokta türü **Event Hubs**olarak ayarlanmış bir olay Kılavuzu konusu için bir olay aboneliği oluşturur. Ayrıca, atılacak kullanım için kullanılacak sistem tarafından yönetilen kimliği belirtir. 

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

### <a name="azure-cli---azure-storage-queue"></a>Azure CLı-Azure depolama kuyruğu 
Bu bölümde, bir Azure depolama kuyruğuna olayları teslim etmek üzere sistem tarafından atanan kimliğin kullanımını etkinleştirmek için **Azure CLI** 'yi nasıl kullanacağınızı öğreneceksiniz. Kimlik, depolama hesabındaki **Depolama Blobu veri katılımcısı** rolünün bir üyesi olmalıdır.

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

#### <a name="create-event-subscription-using-managed-identity-for-delivery"></a>Dağıtım için yönetilen kimlik kullanarak olay aboneliği oluştur 

```azurecli-interactive
az eventgrid event-subscription create 
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type storagequeue  
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $queueid
    -n $sa_esname 
```

#### <a name="create-event-subscription-using-managed-identity-for-delivery--deadletter"></a>Dağıtım için yönetilen kimlik ve sahipsiz mektup kullanarak olay aboneliği oluştur 

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