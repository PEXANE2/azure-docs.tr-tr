---
title: REST kullanarak Azure VMSS'de yönetilen kimlikleri yapılandırma - Azure AD
description: REST API aramaları yapmak için CURL'ü kullanarak bir Azure VMSS'de sistem ve kullanıcı tarafından atanan yönetilen kimlikleri yapılandırmak için adım adım yönergeler.
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
ms.openlocfilehash: dce9894b26d03c351a2209792cc076de91feba54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253344"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-using-rest-api-calls"></a>REST API çağrılarını kullanarak Azure kaynaklarıiçin yönetilen kimlikleri sanal makine ölçeğinde yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir sistem kimliği yle Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure Kaynak Yöneticisi REST bitiş noktasını aramaları yapmak için CURL'u kullanarak, sanal makine ölçeği kümesinde Azure kaynakları işlemleri için aşağıdaki yönetilen kimlikleri nasıl gerçekleştireceklerini öğrenirsiniz:

- Azure sanal makine ölçeğinde sistem tarafından atanan yönetilen kimliği etkinleştirme ve devre dışı bırak
- Azure sanal makine ölçeğinde kullanıcı tarafından atanan yönetilen bir kimlik ekleme ve kaldırma

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için, hesabınızda aşağıdaki Azure rol tabanlı erişim denetimi atamaları gerekir:

    > [!NOTE]
    > Ek Azure AD dizini rol atamaları gerekmez.

    - [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) sanal makine ölçeği kümesi oluşturmak ve sistemi ve/veya kullanıcı tarafından atanan yönetilen kimliği sanal makine ölçeği kümesinden etkinleştirmek ve kaldırmak için.
    - Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için [Yönetilen Kimlik Oluşturici](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü.
    - Kullanıcı tarafından atanan bir kimliği sanal makine ölçeği kümesinden ve sanal makine ölçeği kümesine atamak ve kaldırmak için [Yönetilen Kimlik İşlemi](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü.
- Windows kullanıyorsanız, Linux [için Windows Alt Sistemini](https://msdn.microsoft.com/commandline/wsl/about) yükleyin veya Azure portalında [Azure Bulut Kabuğu'nu](../../cloud-shell/overview.md) kullanın.
- [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya Linux dağıtım işletim [sistemi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest)kullanıyorsanız Azure [CLI yerel konsolu yükleyin.](/cli/azure/install-azure-cli)
- Azure CLI yerel konsolu kullanıyorsanız, sistem `az login` veya kullanıcı tarafından atanan yönetilen kimlikleri yönetmek istediğiniz Azure aboneliğiyle ilişkili bir hesapla Azure'da oturum açın.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

Bu bölümde, Azure Kaynak Yöneticisi REST bitiş noktasına arama yapmak için CURL'u kullanarak sanal makine ölçeğinde sistem tarafından atanan yönetilen kimliği etkinleştirmeyi ve devre dışı bırakabileceğinizi öğrenirsiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesi nin oluşturulması sırasında sistem tarafından atanan yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimlik etkinleştirilmiş sanal bir makine ölçeği kümesi oluşturmak için, sanal bir makine ölçeği kümesi oluşturmanız ve sistem tarafından atanan yönetilen kimlik türü değeriyle Kaynak Yöneticisi uç noktasını aramak için CURL'u kullanmak üzere bir erişim belirteci almanız gerekir.

1. Az [grup](../../azure-resource-manager/management/overview.md#terminology) oluşturma yı kullanarak sanal makine ölçek kümenizin ve ilgili kaynaklarının oluşturulması ve dağıtılması için bir kaynak [grubu oluşturun.](/cli/azure/group/#az-group-create) Bunun yerine kullanmak istediğiniz bir kaynak grubunuz varsa, bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Sanal makine ölçek kümeniz için bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) oluşturun:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. Sistem tarafından atanmış yönetilen bir kimlikle sanal makine ölçeği kümenizi oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Azure Kaynak Yöneticisi REST bitiş noktasını aramak için CURL kullanarak sanal bir makine ölçeği kümesi oluşturun. Aşağıdaki örnek, *myResourceGroup'ta* *myVMSS* adlı sanal bir makine ölçeği kümesi oluşturur ve istek gövdesinde `"identity":{"type":"SystemAssigned"}`değere göre tanımlı olarak sistem tarafından atanmış yönetilen bir kimlik le birlikte. Bir `<ACCESS TOKEN>` Taşıyıcı erişim jetonu ve ortamınız için uygun `<SUBSCRIPTION ID>` değeri istediğinizde önceki adımda aldığınız değeri değiştirin.

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"SystemAssigned"},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın. | 

   **İstek gövdesi**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"SystemAssigned"
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
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
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }  
   ```  

### <a name="enable-system-assigned-managed-identity-on-an-existing-virtual-machine-scale-set"></a>Varolan sanal makine ölçeği kümesinde sistem le atanmış yönetilen kimliği etkinleştirme

Varolan sanal makine ölçeği kümesinde sistem tarafından atanan yönetilen kimliği etkinleştirmek için bir erişim jetonu edinmeniz ve kimlik türünü güncelleştirmek için Kaynak Yöneticisi REST bitiş noktasını aramak için CURL'u kullanmanız gerekir.

1. Sistem tarafından atanmış yönetilen bir kimlikle sanal makine ölçeği kümenizi oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. `{"identity":{"type":"SystemAssigned"}` *MyVMSS*adlı sanal makine ölçeği kümesinin değerine göre istek gövdesinde tanımlanan sanal makine ölçeği kümenizde sistem tarafından atanan yönetilen kimliği etkinleştirmek için Azure Kaynak Yöneticisi REST bitiş noktasını aramak için aşağıdaki CURL komutunu kullanın.  Bir `<ACCESS TOKEN>` Taşıyıcı erişim jetonu ve ortamınız için uygun `<SUBSCRIPTION ID>` değeri istediğinizde önceki adımda aldığınız değeri değiştirin.
   
   > [!IMPORTANT]
   > Sanal makine ölçeği kümesine atanan varolan kullanıcı tarafından atanan yönetilen kimlikleri silmediğinizden emin olmak için, bu CURL komutunu `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`kullanarak kullanıcı tarafından atanan yönetilen kimlikleri listele etmeniz gerekir: . Yanıttaki `identity` değerde tanımlanan sanal makine ölçeğine atanan kullanıcı tarafından atanmış yönetilen kimlikleriniz varsa, sanal makine ölçeği kümenizde sistem tarafından atanan yönetilen kimliği etkinleştirirken kullanıcı tarafından atanan yönetilen kimlikleri nasıl koruyacağınızı gösteren adım 3'e atlayın.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
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

3. Sistem tarafından atanan yönetilen kimliği, varolan kullanıcı tarafından atanan yönetilen kimliklerle sanal `SystemAssigned` makine `type` ölçeğinde etkinleştirmek için değereklemeniz gerekir.  
   
   Örneğin, sanal makine ölçek kümeniz kullanıcı tarafından atanan `ID1` `ID2` yönetilen kimlikleri ve ona atanmışsa ve sanal makine ölçeği kümesine sistem tarafından atanan yönetilen kimlik eklemek istiyorsanız, aşağıdaki CURL çağrısını kullanın. `<ACCESS TOKEN>` Değiştirin `<SUBSCRIPTION ID>` ve ortamınıza uygun değerlerle değiştirin.

   API `2018-06-01` sürümü, kullanıcı tarafından atanan `userAssignedIdentities` yönetilen kimlikleri, API sürümünde `identityIds` `2017-12-01`kullanılan bir dizi biçimindeki değerin aksine sözlük biçiminde ki değerde depolar.
   
   **API SÜRÜM 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
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
          "type":"SystemAssigned,UserAssigned",
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

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
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
          "type":"SystemAssigned,UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```

### <a name="disable-system-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesinden sistem le atanmış yönetilen kimliği devre dışı bırak

Varolan sanal makine ölçeğinde sistem tarafından atanan bir kimliği devre dışı bırakabilmek için, bir erişim jetonu edinmeniz ve `None`ardından kimlik türünü güncelleştirmek için Kaynak Yöneticisi REST bitiş noktasını aramak için CURL'u kullanmanız gerekir.

1. Sistem tarafından atanmış yönetilen bir kimlikle sanal makine ölçeği kümenizi oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sistem tarafından atanan yönetilen kimliği devre dışı bırakabilmek için Azure Kaynak Yöneticisi REST bitiş noktasını aramak için CURL kullanarak sanal makine ölçeği kümesini güncelleştirin.  Aşağıdaki örnek, istek gövdesinde tanımlanan sistem atanan yönetilen kimliği `{"identity":{"type":"None"}}` *myVMSS*adlı sanal makine ölçeği kümesinden alınan değere devre dışı bırakmıştır.  Bir `<ACCESS TOKEN>` Taşıyıcı erişim jetonu ve ortamınız için uygun `<SUBSCRIPTION ID>` değeri istediğinizde önceki adımda aldığınız değeri değiştirin.

   > [!IMPORTANT]
   > Sanal makine ölçeği kümesine atanan varolan kullanıcı tarafından atanan yönetilen kimlikleri silmediğinizden emin olmak için, bu CURL komutunu `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`kullanarak kullanıcı tarafından atanan yönetilen kimlikleri listele etmeniz gerekir: . Sanal makine ölçeği kümesine atanmış kullanıcı tarafından atanmış yönetilen kimliğiniz varsa, sistem tarafından atanan yönetilen kimliği sanal makine ölçeği kümenizden kaldırırken kullanıcı tarafından atanan yönetilen kimlikleri nasıl koruduğunu gösteren adım 3'e atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
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
          "type":"None"
       }
    }
   ```

   Kullanıcı tarafından atanan yönetilen kimlikleri olan sanal makine ölçeği kümesinden sistem `SystemAssigned` le `{"identity":{"type:" "}}` atanmış yönetilen `UserAssigned` kimliği kaldırmak `userAssignedIdentities` için, **2018-06-01 API sürümünü**kullanıyorsanız değeri ve sözlük değerlerini tutarken değerden kaldırın. **API sürümünü 2017-12-01** veya daha erken `identityIds` kullanıyorsanız, diziyi saklayın.

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure Kaynak Yöneticisi REST bitiş noktasına arama yapmak için CURL'u kullanarak sanal makine ölçeğinde kullanıcı tarafından atanan yönetilen kimliği nasıl ekleyeceğinizi ve kaldırabileceğinizi öğrenirsiniz.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesi nin oluşturulması sırasında kullanıcı tarafından atanan yönetilen bir kimlik atama

1. Sistem tarafından atanmış yönetilen bir kimlikle sanal makine ölçeği kümenizi oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sanal makine ölçek kümeniz için bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) oluşturun:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. Sistem tarafından atanmış yönetilen bir kimlikle sanal makine ölçeği kümenizi oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Burada bulunan yönergeleri kullanarak kullanıcı tarafından atanan yönetilen bir kimlik oluşturun: [Kullanıcı tarafından atanan yönetilen bir kimlik oluşturun.](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity)

5. Azure Kaynak Yöneticisi REST bitiş noktasını aramak için CURL kullanarak sanal bir makine ölçeği kümesi oluşturun. Aşağıdaki örnek, kaynak grubu *myResourceGroup'ta* *myVMSS* adlı sanal bir makine ölçeği `ID1`kümesi oluşturur ve istek gövdesinde değere `"identity":{"type":"UserAssigned"}`göre tanımlı olarak kullanıcı tarafından atanan yönetilen bir kimlik le birlikte. Bir `<ACCESS TOKEN>` Taşıyıcı erişim jetonu ve ortamınız için uygun `<SUBSCRIPTION ID>` değeri istediğinizde önceki adımda aldığınız değeri değiştirin.
 
   **API SÜRÜM 2018-06-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"UserAssigned","userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın. | 

   **İstek gövdesi**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
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
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }
   ```   

   **API SÜRÜM 2017-12-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"UserAssigned","identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın. |
 
   **İstek gövdesi**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
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
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen bir kimliği varolan bir Azure sanal makine ölçeği kümesine atama

1. Sistem tarafından atanmış yönetilen bir kimlikle sanal makine ölçeği kümenizi oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Burada bulunan yönergeleri kullanarak kullanıcı tarafından atanan yönetilen bir kimlik oluşturun, [Kullanıcı tarafından atanan yönetilen kimlik oluşturun.](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity)

3. Sanal makine ölçeği kümesine atanan varolan kullanıcı veya sistem atanmış yönetilen kimlikleri silmemeniz için, aşağıdaki CURL komutunu kullanarak sanal makine ölçeğine atanan kimlik türlerini listele etmeniz gerekir. Sanal makine ölçeği kümesine atanan kimlikleri başardıysanız, bunlar `identity` değerde listelenir.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın. |   
 

4. Sanal makine ölçek kümenize atanmış herhangi bir kullanıcı veya sistem atanmış yönetilen kimliğiniz yoksa, sanal makineye ilk kullanıcı tarafından atanan yönetilen kimliği atamak için Azure Kaynak Yöneticisi REST bitiş noktasını aramak için aşağıdaki CURL komutunu kullanın ölçek ayarlanır.  Sanal makine ölçeği kümesine atanmış bir kullanıcı veya sistem atanmış yönetilen kimlik(ler) varsa, sistem tarafından atanan yönetilen yönetilenleri korurken sanal makine ölçeği kümesine birden çok kullanıcı tarafından atanan yönetilen kimlikleri nasıl ekleyeceğinizi gösteren adım 5'e atlayın Kimlik.

   Aşağıdaki örnek, kaynak grubu `ID1` *myResourceGroup'ta* *myVMSS* adlı sanal makine ölçeği kümesine, kullanıcı tarafından atanan yönetilen bir kimlik atar.  Bir `<ACCESS TOKEN>` Taşıyıcı erişim jetonu ve ortamınız için uygun `<SUBSCRIPTION ID>` değeri istediğinizde önceki adımda aldığınız değeri değiştirin.

   **API SÜRÜM 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01 HTTP/1.1
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
          "type":"userAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       }
    }
   ``` 
    
   **API SÜRÜM 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
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
          "type":"userAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```  

5. Sanal makine ölçek kümenize atanmış kullanıcı tarafından atanmış veya sistem tarafından atanmış bir yönetilen kimliğiniz varsa:
   
   **API SÜRÜM 2018-06-01**

   Kullanıcı tarafından atanan yönetilen kimliği `userAssignedIdentities` sözlük değerine ekleyin.

   Örneğin, sistem tarafından atanan yönetilen kimliğiniz ve şu `ID1` anda sanal makine ölçeğinize atanmış kullanıcı tarafından atanan `ID2` yönetilen kimliğiniz varsa ve kullanıcı tarafından atanan yönetilen kimliği eklemek istiyorsanız:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
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

   Örneğin, sistem tarafından atanmış kimliğiniz ve şu anda sanal makine ölçek kümenize atanmış kullanıcı tarafından atanan yönetilen kimliğiniz `ID1` varsa ve kullanıcı tarafından atanan yönetilen kimliği `ID2` eklemek istiyorsanız:

    ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
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
          "type":"SystemAssigned, UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```

### <a name="remove-a-user-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen kimliği sanal makine ölçeği kümesinden kaldırma

1. Sistem tarafından atanmış yönetilen bir kimlikle sanal makine ölçeği kümenizi oluşturmak için Yetkilendirme üstbilgisinde bir sonraki adımda kullanacağınız Bir Taşıyıcı erişim jetonu alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sanal makine ölçeği kümesine atanmış tutmak istediğiniz mevcut kullanıcı tarafından atanmış yönetilen kimlikleri silmemeniz veya sistem tarafından atanan yönetilen kimliği kaldırmamanız için aşağıdaki CURL komutunu kullanarak yönetilen kimlikleri listelamanız gerekir :

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üstbilgi**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın. |
   
   VM'ye atanan kimlikleri yönettiyseniz, bunlar `identity` yanıtta değerde listelenir. 
    
   Örneğin, kullanıcı tarafından atanan yönetilen `ID1` kimlikleriniz `ID2` varsa ve sanal makine ölçek kümenize `ID1` atanmışsanız ve yalnızca atanmış tutmak ve sistem tarafından atanan yönetilen kimliği korumak istiyorsanız:

   **API SÜRÜM 2018-06-01**

   Kaldırmak `null` istediğiniz kullanıcı tarafından atanan yönetilen kimliğe ekleyin:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
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
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```   

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
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
          "type":"SystemAssigned,UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```

Sanal makine ölçek kümenizde hem sistem tarafından atanmış hem de kullanıcı tarafından atanan yönetilen kimlikler varsa, aşağıdaki komutu kullanarak yalnızca sistem tarafından atanan sistemi kullanmak için geçiş yaparak kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırabilirsiniz:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
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
    
Sanal makine ölçek kümenizde yalnızca kullanıcı tarafından atanan yönetilen kimlikler varsa ve bunların hepsini kaldırmak istiyorsanız, aşağıdaki komutu kullanın:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
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
      "type":"None"
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

REST kullanarak kullanıcı tarafından atanan yönetilen kimliklerin nasıl oluşturulacak, listelenebildiğini veya sililenhakkında bilgi için bkz:

- [REST API çağrılarını kullanarak kullanıcı tarafından atanan yönetilen bir kimlik oluşturma, listele veya silme](how-to-manage-ua-identity-rest.md)
