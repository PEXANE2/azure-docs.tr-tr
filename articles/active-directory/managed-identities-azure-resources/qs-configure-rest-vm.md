---
title: REST kullanarak Azure VM'de yönetilen kimlikleri yapılandırma - Azure AD
description: REST API aramaları yapmak için CURL'ü kullanarak bir Azure VM'de sistem ve kullanıcı tarafından atanan yönetilen kimlikleri yapılandırmak için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/25/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f975595e935a5c0254450168aa295e6e7366a94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244166"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-rest-api-calls"></a>REST API çağrılarını kullanarak Azure VM'de Azure kaynakları için Yönetilen kimlikleri yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir sistem kimliği yle Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure Kaynak Yöneticisi REST bitiş noktasını aramaları yapmak için CURL'ü kullanarak, Azure kaynakları işlemleri için aşağıdaki yönetilen kimlikleri bir Azure VM'de nasıl gerçekleştireceğimiz öğrenebilirsiniz:

- Azure VM'de sistem tarafından atanan yönetilen kimliği etkinleştirme ve devre dışı
- Azure VM'de kullanıcı tarafından atanan yönetilen bir kimlik ekleme ve kaldırma

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Windows kullanıyorsanız, Linux [için Windows Alt Sistemini](https://msdn.microsoft.com/commandline/wsl/about) yükleyin veya Azure portalında [Azure Bulut Kabuğu'nu](../../cloud-shell/overview.md) kullanın.
- [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya Linux dağıtım işletim [sistemi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest)kullanıyorsanız Azure [CLI yerel konsolu yükleyin.](/cli/azure/install-azure-cli)
- Azure CLI yerel konsolu kullanıyorsanız, sistem `az login` veya kullanıcı tarafından atanan yönetilen kimlikleri yönetmek istediğiniz Azure aboneliğiyle ilişkili bir hesapla Azure'da oturum açın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

Bu bölümde, Azure Kaynak Yöneticisi REST bitiş noktasını arama yapmak için CURL'ü kullanarak Bir Azure VM'de sistem tarafından atanan yönetilen kimliği etkinleştirmeyi ve devre dışı kalmayı öğrenirsiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Azure VM oluşturma sırasında sistem tarafından atanmış yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimliği etkinleştirilmiş bir Azure VM oluşturmak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

1. VM'nizin ve onunla ilgili kaynakların kapsaması ve dağıtımı için, [az group create](/cli/azure/group/#az-group-create) komutunu kullanarak bir [kaynak grubu](../../azure-resource-manager/management/overview.md#terminology) oluşturun. Bunun yerine kullanmak istediğiniz bir kaynak grubunuz varsa, bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. VM'iniz için bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) oluşturun:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. VM'inizi sistem tarafından atanmış yönetilen bir kimlikle oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Azure Kaynak Yöneticisi REST bitiş noktasını aramak için CURL kullanarak bir VM oluşturun. Aşağıdaki örnek, istek gövdesinde değere `"identity":{"type":"SystemAssigned"}`göre tanımlı olarak, sistem tarafından atanmış yönetilen bir kimliğe sahip *myVM* adlı bir VM oluşturur. Bir `<ACCESS TOKEN>` Taşıyıcı erişim jetonu ve ortamınız için uygun `<SUBSCRIPTION ID>` değeri istediğinizde önceki adımda aldığınız değeri değiştirin.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"SystemAssigned"},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"<SECURE PASSWORD STRING>"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   
   **İstek üstbilgi**
   
   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 
   
   **İstek gövdesi**

   ```JSON
     {
       "location":"westus",
       "name":"myVM",
       "identity":{
          "type":"SystemAssigned"
       },
       "properties":{
          "hardwareProfile":{
             "vmSize":"Standard_D2_v2"
          },
          "storageProfile":{
             "imageReference":{
                "sku":"2016-Datacenter",
                "publisher":"MicrosoftWindowsServer",
                "version":"latest",
                "offer":"WindowsServer"
             },
             "osDisk":{
                "caching":"ReadWrite",
                "managedDisk":{
                   "storageAccountType":"Standard_LRS"
                },
                "name":"myVM3osdisk",
                "createOption":"FromImage"
             },
             "dataDisks":[
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":0
                },
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":1
                }
             ]
          },
          "osProfile":{
             "adminUsername":"azureuser",
             "computerName":"myVM",
             "adminPassword":"myPassword12"
          },
          "networkProfile":{
             "networkInterfaces":[
                {
                   "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
                   "properties":{
                      "primary":true
                   }
                }
             ]
          }
       }
    }  
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Varolan bir Azure VM'de sistem le atanmış kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimliği, başlangıçta onsuz sağlanan bir VM'de etkinleştirmek için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

1. VM'inizi sistem tarafından atanmış yönetilen bir kimlikle oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. `{"identity":{"type":"SystemAssigned"}` *MyVM*adlı bir VM değerine göre istek gövdesinde tanımlanan VM'nizde sistem tarafından atanan yönetilen kimliği etkinleştirmek için Azure Kaynak Yöneticisi REST bitiş noktasını aramak için aşağıdaki CURL komutunu kullanın.  Bir `<ACCESS TOKEN>` Taşıyıcı erişim jetonu ve ortamınız için uygun `<SUBSCRIPTION ID>` değeri istediğinizde önceki adımda aldığınız değeri değiştirin.
   
   > [!IMPORTANT]
   > VM'ye atanan mevcut kullanıcı tarafından atanan yönetilen kimlikleri silmediğinizden emin olmak için, bu CURL komutunu kullanarak kullanıcı `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`tarafından atanan yönetilen kimlikleri listele etmeniz gerekir: . VM'ye yanıttaki `identity` değerde tanımlanan kullanıcı tarafından atanan yönetilen kimlikleriniz varsa, VM'nizde sistem tarafından atanan yönetilen kimliği etkinleştirirken kullanıcı tarafından atanan yönetilen kimlikleri nasıl koruyacağınızı gösteren adım 3'e atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 
   
   **İstek gövdesi**
    
   ```JSON
    {  
       "identity":{  
          "type":"SystemAssigned"
       }
    }
   ```

3. Varolan kullanıcı tarafından atanan yönetilen kimliklere sahip bir VM'de sistem `SystemAssigned` tarafından `type` atanan yönetilen kimliği etkinleştirmek için değere eklemeniz gerekir.  
   
   Örneğin, VM'niz kullanıcı tarafından atanan yönetilen `ID1` `ID2` kimliklere sahipse ve ona atanmışsa ve VM'ye sistem tarafından atanmış yönetilen kimlik eklemek istiyorsanız, aşağıdaki CURL çağrısını kullanın. `<ACCESS TOKEN>` Değiştirin `<SUBSCRIPTION ID>` ve ortamınıza uygun değerlerle değiştirin.

   API `2018-06-01` sürümü, kullanıcı tarafından atanan `userAssignedIdentities` yönetilen kimlikleri, API sürümünde `identityIds` `2017-12-01`kullanılan bir dizi biçimindeki değerin aksine sözlük biçiminde ki değerde depolar.
   
   **API SÜRÜM 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "identity":{  
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{  
             "/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{  
    
             },
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{  
    
             }
          }
       }
    }
   ```

   **API SÜRÜM 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```
    
   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "identity":{  
          "type":"SystemAssigned, UserAssigned",
          "identityIds":[  
             "/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```   

### <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Azure VM'den sistem tarafından atanan yönetilen kimliği devre dışı

VM'de sistem tarafından atanan yönetilen kimliği devre dışı katmak için hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

1. VM'inizi sistem tarafından atanmış yönetilen bir kimlikle oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sistem tarafından atanan yönetilen kimliği devre dışı etmek için Azure Kaynak Yöneticisi REST bitiş noktasını aramak için CURL kullanarak VM'yi güncelleştirin.  Aşağıdaki örnek, istek gövdesinde tanımlanan sistem atanan yönetilen kimliği `{"identity":{"type":"None"}}` *myVM*adlı bir VM'den alınan değere devre dışı kalmaktadır.  Bir `<ACCESS TOKEN>` Taşıyıcı erişim jetonu ve ortamınız için uygun `<SUBSCRIPTION ID>` değeri istediğinizde önceki adımda aldığınız değeri değiştirin.

   > [!IMPORTANT]
   > VM'ye atanan mevcut kullanıcı tarafından atanan yönetilen kimlikleri silmediğinizden emin olmak için, bu CURL komutunu kullanarak kullanıcı `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`tarafından atanan yönetilen kimlikleri listele etmeniz gerekir: . Yanıttaki `identity` değerde tanımlanan VM'ye atanan kullanıcı tarafından atanmış yönetilen kimlikleriniz varsa, VM'nizde sistem tarafından atanan yönetilen kimliği devre dışı bırakarak kullanıcı tarafından atanan yönetilen kimlikleri nasıl koruyacağınızı gösteren adım 3'e atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "identity":{  
          "type":"None"
       }
    }
   ```

   Kullanıcı tarafından atanan yönetilen kimliklere sahip sanal bir makineden sistem `SystemAssigned` le `{"identity":{"type:" "}}` atanmış yönetilen `UserAssigned` kimliği `userAssignedIdentities` kaldırmak için, **2018-06-01 API sürümünü**kullanıyorsanız değeri ve sözlük değerlerini tutarken değerden kaldırın. **API sürümünü 2017-12-01** veya daha erken `identityIds` kullanıyorsanız, diziyi saklayın.

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure Kaynak Yöneticisi REST bitiş noktasına arama yapmak için CURL'ü kullanarak Azure VM'de kullanıcı tarafından atanan yönetilen kimliği nasıl ekleyeceğinizi ve kaldırabileceğinizi öğrenirsiniz.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-an-azure-vm"></a>Azure VM oluşturma sırasında kullanıcı tarafından atanmış yönetilen bir kimlik atama

Bir VM'ye kullanıcı tarafından atanan bir kimlik atamak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [Yönetilen Kimlik Operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

1. VM'inizi sistem tarafından atanmış yönetilen bir kimlikle oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. VM'iniz için bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) oluşturun:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. VM'inizi sistem tarafından atanmış yönetilen bir kimlikle oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Burada bulunan yönergeleri kullanarak kullanıcı tarafından atanan yönetilen bir kimlik oluşturun: [Kullanıcı tarafından atanan yönetilen bir kimlik oluşturun.](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity)

5. Azure Kaynak Yöneticisi REST bitiş noktasını aramak için CURL kullanarak bir VM oluşturun. Aşağıdaki örnek, kaynak grubu *myResourceGroup'ta* *myVM* adlı bir VM `ID1`oluşturur ve istek gövdesinde değere `"identity":{"type":"UserAssigned"}`göre tanımlı olarak kullanıcı tarafından atanan yönetilen bir kimlik le birlikte. Bir `<ACCESS TOKEN>` Taşıyıcı erişim jetonu ve ortamınız için uygun `<SUBSCRIPTION ID>` değeri istediğinizde önceki adımda aldığınız değeri değiştirin.
 
   **API SÜRÜM 2018-06-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"UserAssigned","identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
    
   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "location":"westus",
       "name":"myVM",
       "identity":{  
          "type":"UserAssigned",
          "identityIds":[  
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       },
       "properties":{  
          "hardwareProfile":{  
             "vmSize":"Standard_D2_v2"
          },
          "storageProfile":{  
             "imageReference":{  
                "sku":"2016-Datacenter",
                "publisher":"MicrosoftWindowsServer",
                "version":"latest",
                "offer":"WindowsServer"
             },
             "osDisk":{  
                "caching":"ReadWrite",
                "managedDisk":{  
                   "storageAccountType":"Standard_LRS"
                },
                "name":"myVM3osdisk",
                "createOption":"FromImage"
             },
             "dataDisks":[  
                {  
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":0
                },
                {  
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":1
                }
             ]
          },
          "osProfile":{  
             "adminUsername":"azureuser",
             "computerName":"myVM",
             "adminPassword":"myPassword12"
          },
          "networkProfile":{  
             "networkInterfaces":[  
                {  
                   "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
                   "properties":{  
                      "primary":true
                   }
                }
             ]
          }
       }
    }

   ```
  
   **API SÜRÜM 2017-12-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"UserAssigned","identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "location":"westus",
       "name":"myVM",
       "identity":{
          "type":"UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       },
       "properties":{
          "hardwareProfile":{
             "vmSize":"Standard_D2_v2"
          },
          "storageProfile":{
             "imageReference":{
                "sku":"2016-Datacenter",
                "publisher":"MicrosoftWindowsServer",
                "version":"latest",
                "offer":"WindowsServer"
             },
             "osDisk":{
                "caching":"ReadWrite",
                "managedDisk":{
                   "storageAccountType":"Standard_LRS"
                },
                "name":"myVM3osdisk",
                "createOption":"FromImage"
             },
             "dataDisks":[
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":0
                },
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":1
                }
             ]
          },
          "osProfile":{
             "adminUsername":"azureuser",
             "computerName":"myVM",
             "adminPassword":"myPassword12"
          },
          "networkProfile":{
             "networkInterfaces":[
                {
                   "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
                   "properties":{
                      "primary":true
                   }
                }
             ]
          }
       }
    }
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Kullanıcı tarafından atanan yönetilen bir kimliği varolan bir Azure VM'sine atama

Bir VM'ye kullanıcı tarafından atanan bir kimlik atamak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [Yönetilen Kimlik Operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

1. VM'inizi sistem tarafından atanmış yönetilen bir kimlikle oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Burada bulunan yönergeleri kullanarak kullanıcı tarafından atanan yönetilen bir kimlik oluşturun, [Kullanıcı tarafından atanan yönetilen kimlik oluşturun.](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity)

3. VM'ye atanan varolan kullanıcı veya sistem atanmış yönetilen kimlikleri silmediğinizden emin olmak için, aşağıdaki CURL komutunu kullanarak VM'ye atanan kimlik türlerini listele etmeniz gerekir. Sanal makine ölçeği kümesine atanan kimlikleri yönettiyseniz, bunlar `identity` değer altında listelenir.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.

    Yanıttaki `identity` değerde tanımlanan VM'ye atanan herhangi bir kullanıcı veya sistem atanmış yönetilen kimliğiniz varsa, VM'nize kullanıcı tarafından atanan yönetilen bir kimlik eklerken sistem tarafından atanan yönetilen kimliği nasıl koruyacağınızı gösteren adım 5'e atlayın.

4. VM'nize atanmış kullanıcı tarafından atanmış yönetilen kimliğiniz yoksa, VM'ye ilk kullanıcı tarafından atanan yönetilen kimliği atamak için Azure Kaynak Yöneticisi REST bitiş noktasını aramak için aşağıdaki CURL komutunu kullanın.

   Aşağıdaki örnekler, `ID1` *myResourceGroup*kaynak grubunda *myVM* adlı bir VM'ye kullanıcı tarafından atanan yönetilen bir kimlik atar.  Bir `<ACCESS TOKEN>` Taşıyıcı erişim jetonu ve ortamınız için uygun `<SUBSCRIPTION ID>` değeri istediğinizde önceki adımda aldığınız değeri değiştirin.

   **API SÜRÜM 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        |
 
   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       }
    }
   ```

   **API SÜRÜM 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```
   
   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"userAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```

5. VM'nize atanmış kullanıcı tarafından atanmış veya sistem tarafından atanmış bir yönetilen kimliğiniz varsa:
   
   **API SÜRÜM 2018-06-01**

   Kullanıcı tarafından atanan yönetilen kimliği `userAssignedIdentities` sözlük değerine ekleyin.
    
   Örneğin, sistem tarafından atanan yönetilen kimliğiniz ve vm'nize atanan kullanıcı tarafından atanan yönetilen kimliğiniz `ID1` varsa `ID2` ve kullanıcı tarafından atanan yönetilen kimliği n için eklemek istiyorsanız:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   
   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             },
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{
    
             }
          }
       }
    }
   ```

   **API SÜRÜM 2017-12-01**

   Yeni kullanıcı tarafından atanan yönetilen kimliği eklerken `identityIds` dizi değerinde tutmak istediğiniz kullanıcı tarafından atanan yönetilen kimlikleri koruyun.

   Örneğin, sistem tarafından atanan yönetilen kimliğiniz ve vm'nize atanan kullanıcı tarafından atanan yönetilen kimliğiniz `ID1` varsa `ID2` ve kullanıcı tarafından atanan yönetilen kimliği n için eklemek istiyorsanız: 

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned,UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```   

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Kullanıcı tarafından atanan yönetilen kimliği Azure VM'den kaldırma

Bir VM'ye kullanıcı tarafından atanan bir kimliği kaldırmak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.

1. VM'inizi sistem tarafından atanmış yönetilen bir kimlikle oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. VM'de atanmış tutmak istediğiniz mevcut kullanıcı tarafından atanmış yönetilen kimlikleri silmemeniz veya sistem tarafından atanan yönetilen kimliği kaldırmamanız için, yönetilen kimlikleri aşağıdaki CURL komutunu kullanarak listelamanız gerekir: 

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
 
   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.
 
   VM'ye atanan kimlikleri yönettiyseniz, bunlar `identity` yanıtta değerde listelenir.

   Örneğin, kullanıcı tarafından atanan yönetilen `ID1` kimlikleriniz `ID2` varsa ve VM'nize atanmışsanız `ID1` ve yalnızca atanmış tutmak ve sistem tarafından atanan kimliği korumak istiyorsanız:
   
   **API SÜRÜM 2018-06-01**

   Kaldırmak `null` istediğiniz kullanıcı tarafından atanan yönetilen kimliğe ekleyin:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null
          }
       }
    }
   ```

   **API SÜRÜM 2017-12-01**

   `identityIds` Yalnızca dizide tutmak istediğiniz kullanıcı tarafından atanan yönetilen kimliği(ler) koruyun:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```

VM'nizde hem sistem tarafından atanmış hem de kullanıcı tarafından atanan yönetilen kimlikler varsa, aşağıdaki komutu kullanarak yalnızca sistem tarafından atanan yönetilen kimliği kullanmak için geçiş yaparak kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırabilirsiniz:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
```

**İstek üstbilgi**

|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın. | 

**İstek gövdesi**

```JSON
{
   "identity":{
      "type":"SystemAssigned"
   }
}
```
    
VM'nizde yalnızca kullanıcı tarafından atanmış yönetilen kimlikler varsa ve bunların hepsini kaldırmak istiyorsanız aşağıdaki komutu kullanın:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
```

**İstek üstbilgi**

|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.| 

**İstek gövdesi**

```JSON
{
   "identity":{
      "type":"None"
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

REST kullanarak kullanıcı tarafından atanan yönetilen kimliklerin nasıl oluşturulacak, listelenebildiğini veya sililenhakkında bilgi için bkz:

- [REST API çağrılarını kullanarak kullanıcı tarafından atanan yönetilen kimlikler oluşturun, listelenin veya silin](how-to-manage-ua-identity-rest.md)
