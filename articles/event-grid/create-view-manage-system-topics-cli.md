---
title: CLı kullanarak Azure Event Grid sistem konuları oluşturma, görüntüleme ve yönetme
description: Bu makalede, Azure CLı kullanarak sistem konuları oluşturma, görüntüleme ve silme işlemlerinin nasıl yapılacağı gösterilmektedir.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: spelluru
ms.openlocfilehash: c361e7303f73aee1d2e60bd4dd0b9ed0d0e4746f
ms.sourcegitcommit: ba8df8424d73c8c4ac43602678dae4273af8b336
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84457466"
---
# <a name="create-view-and-manage-event-grid-system-topics-using-azure-cli"></a>Azure CLı kullanarak Event Grid sistem konuları oluşturma, görüntüleme ve yönetme
Bu makalede, Azure CLı kullanarak sistem konuları oluşturma ve yönetme işlemlerinin nasıl yapılacağı gösterilir. Sistem konularına genel bakış için bkz. [sistem konuları](system-topics.md).

## <a name="create-a-system-topic"></a>Bir sistem konusu oluşturun

- Önce bir Azure kaynağı üzerinde bir sistem konusu oluşturmak ve ardından bu konu için bir olay aboneliği oluşturmak için aşağıdaki başvuru konularına bakın:
    - [az eventgrid sistem-konu oluştur](/cli/azure/ext/eventgrid/eventgrid/system-topic?view=azure-cli-latest#ext-eventgrid-az-eventgrid-system-topic-create)

        ```azurecli
        az eventgrid system-topic create \
            -g myResourceGroup \
            --name systemtopic1 \
            --location westus2 \
            --topic-type microsoft.storage.storageaccounts \
            --source /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/storagegaccountname
        ```
    - [az eventgrid System-topic olayı-abonelik oluşturma](/cli/azure/ext/eventgrid/eventgrid/system-topic/event-subscription?view=azure-cli-latest#ext-eventgrid-az-eventgrid-system-topic-event-subscription-create)

        ```azurecli
        az eventgrid system-topic event-subscription create --name es1 \
            -g rg1 --system-topic-name systemtopic1 \
            --endpoint https://contoso.azurewebsites.net/api/f1?code=code         
          ```
- To create a system topic (implicitly) when creating an event subscription for an Azure source, use the [az eventgrid event-subscription create](/cli/azure/ext/eventgrid/eventgrid/event-subscription?view=azure-cli-latest#ext-eventgrid-az-eventgrid-event-subscription-create) method. Here's an example:
    
    ```azurecli
    storageid=$(az storage account show --name <storage_account_name> --resource-group <resource_group_name> --query id --output tsv)
    endpoint=https://$sitename.azurewebsites.net/api/updates

    az eventgrid event-subscription create \
      --source-resource-id $storageid \
      --name <event_subscription_name> \
      --endpoint $endpoint
    ```
    Adım adım yönergeler içeren bir öğretici için bkz. [depolama hesabına abone olma](../storage/blobs/storage-blob-event-quickstart.md?toc=%2Fazure%2Fevent-grid%2Ftoc.json#subscribe-to-your-storage-account).

## <a name="view-all-system-topics"></a>Tüm sistem konularını görüntüleme
Seçilen bir sistem konusunun tüm sistem konularını ve ayrıntılarını görüntülemek için aşağıdaki komutları kullanın:

- [az eventgrid sistem-konu listesi](/cli/azure/ext/eventgrid/eventgrid/system-topic?view=azure-cli-latest#ext-eventgrid-az-eventgrid-system-topic-list)

    ```azurecli
    az eventgrid system-topic list   
     ```
- [az eventgrid System-topic Show](/cli/azure/ext/eventgrid/eventgrid/system-topic?view=azure-cli-latest#ext-eventgrid-az-eventgrid-system-topic-show)

    ```azurecli
    az eventgrid system-topic show -g rg1 -n systemtopic1    
     ```

## <a name="delete-a-system-topic"></a>Bir sistem konusunu silme
Bir sistem konusunu silmek için aşağıdaki komutu kullanın: 

- [az eventgrid sistem-topic Delete](/cli/azure/ext/eventgrid/eventgrid/system-topic?view=azure-cli-latest#ext-eventgrid-az-eventgrid-system-topic-delete)

    ```azurecli
    az eventgrid system-topic delete -g myResourceGroup --name systemtopic1  
     ```

## <a name="next-steps"></a>Sonraki adımlar
Azure Event Grid tarafından desteklenen sistem konuları ve konu türleri hakkında daha fazla bilgi edinmek için [Azure Event Grid bölümündeki sistem konularına](system-topics.md) bakın. 
